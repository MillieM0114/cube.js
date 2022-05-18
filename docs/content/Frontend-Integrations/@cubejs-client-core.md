---
title: '@cubejs-client/core'
permalink: /@cubejs-client-core
category: Frontend Integrations
subCategory: Reference
menuOrder: 2
---

Vanilla JavaScript Cube.js client.

## cubejs

```typescript
cubejs(apiToken: string | () => Promise<string>, options: CubeJSApiOptions): CubejsApi
```

Creates an instance of the `CubejsApi`. The API entry point.

```js
import cubejs from '@cubejs-client/core';
const cubejsApi = cubejs(
  'CUBEJS-API-TOKEN',
  { apiUrl: 'http://localhost:4000/cubejs-api/v1' }
);
```

You can also pass an async function or a promise that will resolve to the API token

```js
import cubejs from '@cubejs-client/core';
const cubejsApi = cubejs(
  async () => await Auth.getJwtToken(),
  { apiUrl: 'http://localhost:4000/cubejs-api/v1' }
);
```

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
apiToken | string &#124; () => Promise<string> | [API token](security) is used to authorize requests and determine SQL database you're accessing. In the development mode, Cube.js Backend will print the API token to the console on startup. In case of async function `authorization` is updated for `options.transport` on each request. |
options | CubeJSApiOptions | - |

```typescript
cubejs(options: CubeJSApiOptions): CubejsApi
```

## areQueriesEqual

```typescript
areQueriesEqual(query1: DeeplyReadonly<Query> | null, query2: DeeplyReadonly<Query> | null): boolean
```

## defaultHeuristics

```typescript
defaultHeuristics(newState: TDefaultHeuristicsState, oldQuery: Query, options: TDefaultHeuristicsOptions): TDefaultHeuristicsResponse
```

## defaultOrder

```typescript
defaultOrder(query: DeeplyReadonly<Query>): object
```

## movePivotItem

```typescript
movePivotItem(pivotConfig: PivotConfig, sourceIndex: number, destinationIndex: number, sourceAxis: TSourceAxis, destinationAxis: TSourceAxis): PivotConfig
```

## CubejsApi

Main class for accessing Cube.js API

### <--{"id" : "CubejsApi"}-->  dryRun

```typescript
dryRun(query: DeeplyReadonly<Query | Query[]>, options?: LoadMethodOptions): Promise<DryRunResponse>
```

```typescript
dryRun(query: DeeplyReadonly<Query | Query[]>, options: LoadMethodOptions, callback?: LoadMethodCallback<DryRunResponse>): void
```

Get query related meta without query execution

### <--{"id" : "CubejsApi"}-->  load

```typescript
load‹**QueryType**›(query: QueryType, options?: LoadMethodOptions): Promise<ResultSet<QueryRecordType<QueryType>>>
```

**Type parameters:**

- **QueryType**: *DeeplyReadonly<Query | Query[]>*

```typescript
load‹**QueryType**›(query: QueryType, options?: LoadMethodOptions, callback?: LoadMethodCallback<ResultSet<QueryRecordType<QueryType>>>): void
```

Fetch data for the passed `query`.

```js
import cubejs from '@cubejs-client/core';
import Chart from 'chart.js';
import chartjsConfig from './toChartjsData';

const cubejsApi = cubejs('CUBEJS_TOKEN');

const resultSet = await cubejsApi.load({
 measures: ['Stories.count'],
 timeDimensions: [{
   dimension: 'Stories.time',
   dateRange: ['2015-01-01', '2015-12-31'],
   granularity: 'month'
  }]
});

const context = document.getElementById('myChart');
new Chart(context, chartjsConfig(resultSet));
```

**Type parameters:**

- **QueryType**: *DeeplyReadonly<Query | Query[]>*

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
query | QueryType | [Query object](query-format)  |
options? | LoadMethodOptions | - |
callback? | LoadMethodCallback<ResultSet<QueryRecordType<QueryType>>> | - |

```typescript
load‹**QueryType**›(query: QueryType, options?: LoadMethodOptions, callback?: LoadMethodCallback<ResultSet>, responseFormat?: string): Promise<ResultSet<QueryRecordType<QueryType>>>
```

**Type parameters:**

- **QueryType**: *DeeplyReadonly<Query | Query[]>*

### <--{"id" : "CubejsApi"}-->  meta

```typescript
meta(options?: LoadMethodOptions): Promise<Meta>
```

```typescript
meta(options?: LoadMethodOptions, callback?: LoadMethodCallback<Meta>): void
```

Get meta description of cubes available for querying.

### <--{"id" : "CubejsApi"}-->  sql

```typescript
sql(query: DeeplyReadonly<Query | Query[]>, options?: LoadMethodOptions): Promise<SqlQuery>
```

```typescript
sql(query: DeeplyReadonly<Query | Query[]>, options?: LoadMethodOptions, callback?: LoadMethodCallback<SqlQuery>): void
```

Get generated SQL string for the given `query`.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
query | DeeplyReadonly<Query &#124; Query[]> | [Query object](query-format)  |
options? | LoadMethodOptions | - |
callback? | LoadMethodCallback<SqlQuery> | - |

### <--{"id" : "CubejsApi"}-->  subscribe

```typescript
subscribe‹**QueryType**›(query: QueryType, options: LoadMethodOptions | null, callback: LoadMethodCallback<ResultSet<QueryRecordType<QueryType>>>): void
```

Allows you to fetch data and receive updates over time. See [Real-Time Data Fetch](real-time-data-fetch)

```js
cubejsApi.subscribe(
  {
    measures: ['Logs.count'],
    timeDimensions: [
      {
        dimension: 'Logs.time',
        granularity: 'hour',
        dateRange: 'last 1440 minutes',
      },
    ],
  },
  options,
  (error, resultSet) => {
    if (!error) {
      // handle the update
    }
  }
);
```

**Type parameters:**

- **QueryType**: *DeeplyReadonly<Query | Query[]>*

## HttpTransport

Default transport implementation.

### <--{"id" : "HttpTransport"}-->  constructor

```typescript
new HttpTransport(options: TransportOptions): HttpTransport
```

### <--{"id" : "HttpTransport"}-->  request

```typescript
request(method: string, params: any): ITransportResponse<ResultSet>
```

## Meta

Contains information about available cubes and it's members.

### <--{"id" : "Meta"}-->  cubes

```
 cubes: *Cube[]* 
```

An array of all available cubes with their members

### <--{"id" : "Meta"}-->  cubesMap

```
 cubesMap: *Record<string, Pick<Cube, "dimensions" | "measures" | "segments">>* 
```

A map of all cubes where the key is a cube name

### <--{"id" : "Meta"}-->  meta

```
 meta: *MetaResponse* 
```

Raw meta response

### <--{"id" : "Meta"}-->  defaultTimeDimensionNameFor

```typescript
defaultTimeDimensionNameFor(memberName: string): string
```

### <--{"id" : "Meta"}-->  filterOperatorsForMember

```typescript
filterOperatorsForMember(memberName: string, memberType: MemberType | MemberType[]): FilterOperator[]
```

### <--{"id" : "Meta"}-->  membersForQuery

```typescript
membersForQuery(query: DeeplyReadonly<Query> | null, memberType: MemberType): TCubeMeasure[] | TCubeDimension[] | TCubeMember[]
```

Get all members of a specific type for a given query.
If empty query is provided no filtering is done based on query context and all available members are retrieved.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
query | DeeplyReadonly<Query> &#124; null | context query to provide filtering of members available to add to this query  |
memberType | MemberType | - |

### <--{"id" : "Meta"}-->  membersGroupedByCube

```typescript
membersGroupedByCube(): any
```

### <--{"id" : "Meta"}-->  resolveMember

```typescript
resolveMember‹**T**›(memberName: string, memberType: T | T[]): object | TCubeMemberByType<T>
```

Get meta information for a cube member
Member meta information contains:
```javascript
{
  name,
  title,
  shortTitle,
  type,
  description,
  format
}
```

**Type parameters:**

- **T**: *MemberType*

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
memberName | string | Fully qualified member name in a form `Cube.memberName` |
memberType | T &#124; T[] | - |

## ProgressResult

### <--{"id" : "ProgressResult"}-->  stage

```typescript
stage(): string
```

### <--{"id" : "ProgressResult"}-->  timeElapsed

```typescript
timeElapsed(): string
```

## ResultSet

Provides a convenient interface for data manipulation.

### <--{"id" : "ResultSet"}-->  annotation

```typescript
annotation(): QueryAnnotations
```

### <--{"id" : "ResultSet"}-->  categories

```typescript
categories(pivotConfig?: PivotConfig): ChartPivotRow[]
```

### <--{"id" : "ResultSet"}-->  chartPivot

```typescript
chartPivot(pivotConfig?: PivotConfig): ChartPivotRow[]
```

Returns normalized query result data in the following format.

You can find the examples of using the `pivotConfig` [here](#types-pivot-config)
```js
// For the query
{
  measures: ['Stories.count'],
  timeDimensions: [{
    dimension: 'Stories.time',
    dateRange: ['2015-01-01', '2015-12-31'],
    granularity: 'month'
  }]
}

// ResultSet.chartPivot() will return
[
  { "x":"2015-01-01T00:00:00", "Stories.count": 27120, "xValues": ["2015-01-01T00:00:00"] },
  { "x":"2015-02-01T00:00:00", "Stories.count": 25861, "xValues": ["2015-02-01T00:00:00"]  },
  { "x":"2015-03-01T00:00:00", "Stories.count": 29661, "xValues": ["2015-03-01T00:00:00"]  },
  //...
]

```
When using `chartPivot()` or `seriesNames()`, you can pass `aliasSeries` in the [pivotConfig](#types-pivot-config)
to give each series a unique prefix. This is useful for `blending queries` which use the same measure multiple times.

```js
// For the queries
{
  measures: ['Stories.count'],
  timeDimensions: [
    {
      dimension: 'Stories.time',
      dateRange: ['2015-01-01', '2015-12-31'],
      granularity: 'month',
    },
  ],
},
{
  measures: ['Stories.count'],
  timeDimensions: [
    {
      dimension: 'Stories.time',
      dateRange: ['2015-01-01', '2015-12-31'],
      granularity: 'month',
    },
  ],
  filters: [
    {
      member: 'Stores.read',
      operator: 'equals',
      value: ['true'],
    },
  ],
},

// ResultSet.chartPivot({ aliasSeries: ['one', 'two'] }) will return
[
  {
    x: '2015-01-01T00:00:00',
    'one,Stories.count': 27120,
    'two,Stories.count': 8933,
    xValues: ['2015-01-01T00:00:00'],
  },
  {
    x: '2015-02-01T00:00:00',
    'one,Stories.count': 25861,
    'two,Stories.count': 8344,
    xValues: ['2015-02-01T00:00:00'],
  },
  {
    x: '2015-03-01T00:00:00',
    'one,Stories.count': 29661,
    'two,Stories.count': 9023,
    xValues: ['2015-03-01T00:00:00'],
  },
  //...
];
```

### <--{"id" : "ResultSet"}-->  decompose

```typescript
decompose(): ResultSet[]
```

Can be used when you need access to the methods that can't be used with some query types (eg `compareDateRangeQuery` or `blendingQuery`)
```js
resultSet.decompose().forEach((currentResultSet) => {
  console.log(currentResultSet.rawData());
});
```

### <--{"id" : "ResultSet"}-->  drillDown

```typescript
drillDown(drillDownLocator: DrillDownLocator, pivotConfig?: PivotConfig): Query | null
```

Returns a measure drill down query.

Provided you have a measure with the defined `drillMemebers` on the `Orders` cube
```js
measures: {
  count: {
    type: `count`,
    drillMembers: [Orders.status, Users.city, count],
  },
  // ...
}
```

Then you can use the `drillDown` method to see the rows that contribute to that metric
```js
resultSet.drillDown(
  {
    xValues,
    yValues,
  },
  // you should pass the `pivotConfig` if you have used it for axes manipulation
  pivotConfig
)
```

the result will be a query with the required filters applied and the dimensions/measures filled out
```js
{
  measures: ['Orders.count'],
  dimensions: ['Orders.status', 'Users.city'],
  filters: [
    // dimension and measure filters
  ],
  timeDimensions: [
    //...
  ]
}
```

In case when you want to add `order` or `limit` to the query, you can simply spread it

```js
// An example for React
const drillDownResponse = useCubeQuery(
   {
     ...drillDownQuery,
     limit: 30,
     order: {
       'Orders.ts': 'desc'
     }
   },
   {
     skip: !drillDownQuery
   }
 );
```

### <--{"id" : "ResultSet"}-->  pivot

```typescript
pivot(pivotConfig?: PivotConfig): PivotRow[]
```

Base method for pivoting [ResultSet](#result-set) data.
Most of the times shouldn't be used directly and [chartPivot](#result-set-chart-pivot)
or [tablePivot](#table-pivot) should be used instead.

You can find the examples of using the `pivotConfig` [here](#types-pivot-config)
```js
// For query
{
  measures: ['Stories.count'],
  timeDimensions: [{
    dimension: 'Stories.time',
    dateRange: ['2015-01-01', '2015-03-31'],
    granularity: 'month'
  }]
}

// ResultSet.pivot({ x: ['Stories.time'], y: ['measures'] }) will return
[
  {
    xValues: ["2015-01-01T00:00:00"],
    yValuesArray: [
      [['Stories.count'], 27120]
    ]
  },
  {
    xValues: ["2015-02-01T00:00:00"],
    yValuesArray: [
      [['Stories.count'], 25861]
    ]
  },
  {
    xValues: ["2015-03-01T00:00:00"],
    yValuesArray: [
      [['Stories.count'], 29661]
    ]
  }
]
```

### <--{"id" : "ResultSet"}-->  query

```typescript
query(): Query
```

### <--{"id" : "ResultSet"}-->  rawData

```typescript
rawData(): T[]
```

### <--{"id" : "ResultSet"}-->  serialize

```typescript
serialize(): SerializedResult
```

Can be used to stash the `ResultSet` in a storage and restored later with [deserialize](#result-set-deserialize)

### <--{"id" : "ResultSet"}-->  series

```typescript
series‹**SeriesItem**›(pivotConfig?: PivotConfig): Series<SeriesItem>[]
```

Returns an array of series with key, title and series data.
```js
// For the query
{
  measures: ['Stories.count'],
  timeDimensions: [{
    dimension: 'Stories.time',
    dateRange: ['2015-01-01', '2015-12-31'],
    granularity: 'month'
  }]
}

// ResultSet.series() will return
[
  {
    key: 'Stories.count',
    title: 'Stories Count',
    series: [
      { x: '2015-01-01T00:00:00', value: 27120 },
      { x: '2015-02-01T00:00:00', value: 25861 },
      { x: '2015-03-01T00:00:00', value: 29661 },
      //...
    ],
  },
]
```

**Type parameters:**

- **SeriesItem**

### <--{"id" : "ResultSet"}-->  seriesNames

```typescript
seriesNames(pivotConfig?: PivotConfig): SeriesNamesColumn[]
```

Returns an array of series objects, containing `key` and `title` parameters.
```js
// For query
{
  measures: ['Stories.count'],
  timeDimensions: [{
    dimension: 'Stories.time',
    dateRange: ['2015-01-01', '2015-12-31'],
    granularity: 'month'
  }]
}

// ResultSet.seriesNames() will return
[
  {
    key: 'Stories.count',
    title: 'Stories Count',
    yValues: ['Stories.count'],
  },
]
```

### <--{"id" : "ResultSet"}-->  tableColumns

```typescript
tableColumns(pivotConfig?: PivotConfig): TableColumn[]
```

Returns an array of column definitions for `tablePivot`.

For example:
```js
// For the query
{
  measures: ['Stories.count'],
  timeDimensions: [{
    dimension: 'Stories.time',
    dateRange: ['2015-01-01', '2015-12-31'],
    granularity: 'month'
  }]
}

// ResultSet.tableColumns() will return
[
  {
    key: 'Stories.time',
    dataIndex: 'Stories.time',
    title: 'Stories Time',
    shortTitle: 'Time',
    type: 'time',
    format: undefined,
  },
  {
    key: 'Stories.count',
    dataIndex: 'Stories.count',
    title: 'Stories Count',
    shortTitle: 'Count',
    type: 'count',
    format: undefined,
  },
  //...
]
```

In case we want to pivot the table axes
```js
// Let's take this query as an example
{
  measures: ['Orders.count'],
  dimensions: ['Users.country', 'Users.gender']
}

// and put the dimensions on `y` axis
resultSet.tableColumns({
  x: [],
  y: ['Users.country', 'Users.gender', 'measures']
})
```

then `tableColumns` will group the table head and return
```js
{
  key: 'Germany',
  type: 'string',
  title: 'Users Country Germany',
  shortTitle: 'Germany',
  meta: undefined,
  format: undefined,
  children: [
    {
      key: 'male',
      type: 'string',
      title: 'Users Gender male',
      shortTitle: 'male',
      meta: undefined,
      format: undefined,
      children: [
        {
          // ...
          dataIndex: 'Germany.male.Orders.count',
          shortTitle: 'Count',
        },
      ],
    },
    {
      // ...
      shortTitle: 'female',
      children: [
        {
          // ...
          dataIndex: 'Germany.female.Orders.count',
          shortTitle: 'Count',
        },
      ],
    },
  ],
},
// ...
```

### <--{"id" : "ResultSet"}-->  tablePivot

```typescript
tablePivot(pivotConfig?: PivotConfig): Array<object>
```

Returns normalized query result data prepared for visualization in the table format.

You can find the examples of using the `pivotConfig` [here](#types-pivot-config)

For example:
```js
// For the query
{
  measures: ['Stories.count'],
  timeDimensions: [{
    dimension: 'Stories.time',
    dateRange: ['2015-01-01', '2015-12-31'],
    granularity: 'month'
  }]
}

// ResultSet.tablePivot() will return
[
  { "Stories.time": "2015-01-01T00:00:00", "Stories.count": 27120 },
  { "Stories.time": "2015-02-01T00:00:00", "Stories.count": 25861 },
  { "Stories.time": "2015-03-01T00:00:00", "Stories.count": 29661 },
  //...
]
```

### <--{"id" : "ResultSet"}-->  tableRow

```typescript
tableRow(): ChartPivotRow
```

### <--{"id" : "ResultSet"}-->  totalRow

```typescript
totalRow(pivotConfig?: PivotConfig): ChartPivotRow
```

### <--{"id" : "ResultSet"}-->  deserialize

```typescript
`static`deserialize‹**TData**›(data: Object, options?: Object): ResultSet<TData>
```

```js
import { ResultSet } from '@cubejs-client/core';

const resultSet = await cubejsApi.load(query);
// You can store the result somewhere
const tmp = resultSet.serialize();

// and restore it later
const resultSet = ResultSet.deserialize(tmp);
```

**Type parameters:**

- **TData**

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
data | Object | the result of [serialize](#result-set-serialize)  |
options? | Object | - |

### <--{"id" : "ResultSet"}-->  getNormalizedPivotConfig

```typescript
`static`getNormalizedPivotConfig(query: PivotQuery, pivotConfig?: Partial<PivotConfig>): PivotConfig
```

## SqlQuery

### <--{"id" : "SqlQuery"}-->  rawQuery

```typescript
rawQuery(): SqlData
```

### <--{"id" : "SqlQuery"}-->  sql

```typescript
sql(): string
```

## BinaryFilter

### <--{"id" : "BinaryFilter"}-->  dimension

```
 dimension? : *string* 
```

**`deprecated`** Use `member` instead.

### <--{"id" : "BinaryFilter"}-->  member

```
 member? : *string* 
```

### <--{"id" : "BinaryFilter"}-->  operator

```
 operator: *BinaryOperator* 
```

### <--{"id" : "BinaryFilter"}-->  values

```
 values: *string[]* 
```

## ITransport

### <--{"id" : "ITransport"}-->  request

```typescript
request(method: string, params: Record<string, unknown>): ITransportResponse<R>
```

## ITransportResponse

### <--{"id" : "ITransportResponse"}-->  subscribe

```
 subscribe: *function* 
```

### <--{"id" : "ITransportResponse"}-->  unsubscribe

```
 unsubscribe? : *function* 
```

## Query

### <--{"id" : "Query"}-->  dimensions

```
 dimensions? : *string[]* 
```

### <--{"id" : "Query"}-->  filters

```
 filters? : *Filter[]* 
```

### <--{"id" : "Query"}-->  limit

```
 limit? : *null | number* 
```

### <--{"id" : "Query"}-->  measures

```
 measures? : *string[]* 
```

### <--{"id" : "Query"}-->  offset

```
 offset? : *number* 
```

### <--{"id" : "Query"}-->  order

```
 order? : *TQueryOrderObject | TQueryOrderArray* 
```

### <--{"id" : "Query"}-->  renewQuery

```
 renewQuery? : *boolean* 
```

### <--{"id" : "Query"}-->  responseFormat

```
 responseFormat? : *"compact" | "default"* 
```

### <--{"id" : "Query"}-->  segments

```
 segments? : *string[]* 
```

### <--{"id" : "Query"}-->  timeDimensions

```
 timeDimensions? : *TimeDimension[]* 
```

### <--{"id" : "Query"}-->  timezone

```
 timezone? : *string* 
```

### <--{"id" : "Query"}-->  total

```
 total? : *boolean* 
```

### <--{"id" : "Query"}-->  ungrouped

```
 ungrouped? : *boolean* 
```

## TFlatFilter

### <--{"id" : "TFlatFilter"}-->  dimension

```
 dimension? : *string* 
```

**`deprecated`** Use `member` instead.

### <--{"id" : "TFlatFilter"}-->  member

```
 member? : *string* 
```

### <--{"id" : "TFlatFilter"}-->  operator

```
 operator: *BinaryOperator* 
```

### <--{"id" : "TFlatFilter"}-->  values

```
 values: *string[]* 
```

## TimeDimensionBase

### <--{"id" : "TimeDimensionBase"}-->  dimension

```
 dimension: *string* 
```

### <--{"id" : "TimeDimensionBase"}-->  granularity

```
 granularity? : *TimeDimensionGranularity* 
```

## UnaryFilter

### <--{"id" : "UnaryFilter"}-->  dimension

```
 dimension? : *string* 
```

**`deprecated`** Use `member` instead.

### <--{"id" : "UnaryFilter"}-->  member

```
 member? : *string* 
```

### <--{"id" : "UnaryFilter"}-->  operator

```
 operator: *UnaryOperator* 
```

### <--{"id" : "UnaryFilter"}-->  values

```
 values? : *never* 
```

## Types

### <--{"id" : "Types"}-->  Annotation

Name | Type |
------ | ------ |
format? | "currency" &#124; "percent" &#124; "number" |
shortTitle | string |
title | string |
type | string |

### <--{"id" : "Types"}-->  BaseCubeMember

Name | Type |
------ | ------ |
isVisible? | boolean |
meta? | any |
name | string |
shortTitle | string |
title | string |
type | TCubeMemberType |

### <--{"id" : "Types"}-->  BinaryOperator

```
 BinaryOperator: *"equals" | "notEquals" | "contains" | "notContains" | "startsWith" | "endsWith" | "gt" | "gte" | "lt" | "lte" | "inDateRange" | "notInDateRange" | "beforeDate" | "afterDate"* 
```

### <--{"id" : "Types"}-->  ChartPivotRow

Name | Type |
------ | ------ |
x | string |
xValues | string[] |

### <--{"id" : "Types"}-->  ChartType

```
 ChartType: *"line" | "bar" | "table" | "area" | "number" | "pie"* 
```

### <--{"id" : "Types"}-->  Column

Name | Type |
------ | ------ |
key | string |
series | [] |
title | string |

### <--{"id" : "Types"}-->  Cube

Name | Type |
------ | ------ |
dimensions | TCubeDimension[] |
measures | TCubeMeasure[] |
name | string |
segments | TCubeSegment[] |
title | string |

### <--{"id" : "Types"}-->  CubeJSApiOptions

Name | Type | Description |
------ | ------ | ------ |
apiUrl | string | URL of your Cube.js Backend. By default, in the development environment it is `http://localhost:4000/cubejs-api/v1` |
credentials? | "omit" &#124; "same-origin" &#124; "include" | - |
headers? | Record<string, string> | - |
parseDateMeasures? | boolean | - |
pollInterval? | number | - |
resType? | "default" &#124; "compact" | - |
transport? | ITransport<any> | Transport implementation to use. [HttpTransport](#http-transport) will be used by default. |

### <--{"id" : "Types"}-->  CubeMember

```
 CubeMember: *TCubeMeasure | TCubeDimension | TCubeSegment* 
```

### <--{"id" : "Types"}-->  DateRange

```
 DateRange: *string | [string, string]* 
```

### <--{"id" : "Types"}-->  DeeplyReadonly

### <--{"id" : "Types"}-->  DrillDownLocator

Name | Type |
------ | ------ |
xValues | string[] |
yValues? | string[] |

### <--{"id" : "Types"}-->  DryRunResponse

Name | Type |
------ | ------ |
normalizedQueries | Query[] |
pivotQuery | PivotQuery |
queryOrder | Array<object> |
queryType | QueryType |
transformedQueries | TransformedQuery[] |

### <--{"id" : "Types"}-->  ExtractMembers

```
 ExtractMembers: *T extends object ? Names : never | T extends object ? Names : never | T extends object ? ExtractTimeMembers<U> : never* 
```

### <--{"id" : "Types"}-->  ExtractTimeMember

```
 ExtractTimeMember: *T extends object ? Dimension | `${Dimension & string}.${Granularity & string}` : never* 
```

### <--{"id" : "Types"}-->  ExtractTimeMembers

```
 ExtractTimeMembers: *T extends readonly [infer First] ? ExtractTimeMember<First> | ExtractTimeMembers<Rest> : never* 
```

### <--{"id" : "Types"}-->  Filter

```
 Filter: *BinaryFilter | UnaryFilter | LogicalOrFilter | LogicalAndFilter* 
```

### <--{"id" : "Types"}-->  FilterOperator

Name | Type |
------ | ------ |
name | string |
title | string |

### <--{"id" : "Types"}-->  LeafMeasure

Name | Type |
------ | ------ |
additive | boolean |
measure | string |
type | "count" &#124; "countDistinct" &#124; "sum" &#124; "min" &#124; "max" &#124; "number" &#124; "countDistinctApprox" |

### <--{"id" : "Types"}-->  LoadMethodCallback

```
 LoadMethodCallback: *function* 
```

### <--{"id" : "Types"}-->  LoadMethodOptions

Name | Type | Description |
------ | ------ | ------ |
progressCallback? |  | - |
cubejsApi? | CubejsApi | A Cube.js API instance. If not provided will be taken from `CubeProvider` |
mutexKey? | string | Key to store the current request's MUTEX inside the `mutexObj`. MUTEX object is used to reject orphaned queries results when new queries are sent. For example: if two queries are sent with the same `mutexKey` only the last one will return results. |
mutexObj? | Object | Object to store MUTEX |
subscribe? | boolean | Pass `true` to use continuous fetch behavior. |

### <--{"id" : "Types"}-->  LoadResponse

Name | Type |
------ | ------ |
pivotQuery | PivotQuery |
queryType | QueryType |
results | LoadResponseResult<T>[] |

### <--{"id" : "Types"}-->  LoadResponseResult

Name | Type |
------ | ------ |
annotation | QueryAnnotations |
data | T[] |
dbType | string |
extDbType | string |
external | boolean &#124; null |
lastRefreshTime | string |
query | Query |
requestId? | string |
total? | number |
transformedQuery? | TransformedQuery |
usedPreAggregations? | Record<string, UsedPreAggregation> |

### <--{"id" : "Types"}-->  LogicalAndFilter

Name | Type |
------ | ------ |
and | Filter[] |

### <--{"id" : "Types"}-->  LogicalOrFilter

Name | Type |
------ | ------ |
or | Filter[] |

### <--{"id" : "Types"}-->  MemberType

```
 MemberType: *"measures" | "dimensions" | "segments"* 
```

### <--{"id" : "Types"}-->  MetaResponse

Name | Type |
------ | ------ |
cubes | Cube[] |

### <--{"id" : "Types"}-->  PivotConfig

Configuration object that contains information about pivot axes and other options.

Let's apply `pivotConfig` and see how it affects the axes
```js
// Example query
{
  measures: ['Orders.count'],
  dimensions: ['Users.country', 'Users.gender']
}
```
If we put the `Users.gender` dimension on **y** axis
```js
resultSet.tablePivot({
  x: ['Users.country'],
  y: ['Users.gender', 'measures']
})
```

The resulting table will look the following way

| Users Country | male, Orders.count | female, Orders.count |
| ------------- | ------------------ | -------------------- |
| Australia     | 3                  | 27                   |
| Germany       | 10                 | 12                   |
| US            | 5                  | 7                    |

Now let's put the `Users.country` dimension on **y** axis instead
```js
resultSet.tablePivot({
  x: ['Users.gender'],
  y: ['Users.country', 'measures'],
});
```

in this case the `Users.country` values will be laid out on **y** or **columns** axis

| Users Gender | Australia, Orders.count | Germany, Orders.count | US, Orders.count |
| ------------ | ----------------------- | --------------------- | ---------------- |
| male         | 3                       | 10                    | 5                |
| female       | 27                      | 12                    | 7                |

It's also possible to put the `measures` on **x** axis. But in either case it should always be the last item of the array.
```js
resultSet.tablePivot({
  x: ['Users.gender', 'measures'],
  y: ['Users.country'],
});
```

| Users Gender | measures     | Australia | Germany | US  |
| ------------ | ------------ | --------- | ------- | --- |
| male         | Orders.count | 3         | 10      | 5   |
| female       | Orders.count | 27        | 12      | 7   |

Name | Type | Description |
------ | ------ | ------ |
aliasSeries? | string[] | Give each series a prefix alias. Should have one entry for each query:measure. See [chartPivot](#result-set-chart-pivot) |
fillMissingDates? | boolean &#124; null | If `true` missing dates on the time dimensions will be filled with `0` for all measures.Note: the `fillMissingDates` option set to `true` will override any **order** applied to the query |
x? | string[] | Dimensions to put on **x** or **rows** axis. |
y? | string[] | Dimensions to put on **y** or **columns** axis. |

### <--{"id" : "Types"}-->  PivotQuery

```
 PivotQuery: *Query & object* 
```

### <--{"id" : "Types"}-->  PivotRow

Name | Type |
------ | ------ |
xValues | Array<string &#124; number> |
yValuesArray | Array<[string[], number]> |

### <--{"id" : "Types"}-->  PreAggregationType

```
 PreAggregationType: *"rollup" | "rollupJoin" | "originalSql"* 
```

### <--{"id" : "Types"}-->  ProgressResponse

Name | Type |
------ | ------ |
stage | string |
timeElapsed | number |

### <--{"id" : "Types"}-->  QueryAnnotations

Name | Type |
------ | ------ |
dimensions | Record<string, Annotation> |
measures | Record<string, Annotation> |
timeDimensions | Record<string, Annotation> |

### <--{"id" : "Types"}-->  QueryArrayRecordType

```
 QueryArrayRecordType: *T extends readonly [infer First] ? SingleQueryRecordType<First> | QueryArrayRecordType<Rest> : never* 
```

### <--{"id" : "Types"}-->  QueryOrder

```
 QueryOrder: *"asc" | "desc"* 
```

### <--{"id" : "Types"}-->  QueryRecordType

```
 QueryRecordType: *T extends DeeplyReadonly<Query[]> ? QueryArrayRecordType<T> : T extends DeeplyReadonly<Query> ? SingleQueryRecordType<T> : never* 
```

### <--{"id" : "Types"}-->  QueryType

```
 QueryType: *"regularQuery" | "compareDateRangeQuery" | "blendingQuery"* 
```

### <--{"id" : "Types"}-->  SerializedResult

Name | Type |
------ | ------ |
loadResponse | LoadResponse<T> |

### <--{"id" : "Types"}-->  Series

Name | Type |
------ | ------ |
key | string |
series | T[] |
title | string |

### <--{"id" : "Types"}-->  SeriesNamesColumn

Name | Type |
------ | ------ |
key | string |
title | string |
yValues | string[] |

### <--{"id" : "Types"}-->  SingleQueryRecordType

```
 SingleQueryRecordType: *ExtractMembers<T> extends never ? any : object* 
```

### <--{"id" : "Types"}-->  SqlData

Name | Type |
------ | ------ |
aliasNameToMember | Record<string, string> |
cacheKeyQueries | SqlQueryTuple[] |
dataSource | boolean |
external | boolean |
preAggregations | any[] |
rollupMatchResults | any[] |
sql | SqlQueryTuple |

### <--{"id" : "Types"}-->  SqlQueryTuple

```
 SqlQueryTuple: *[string, any[], any]* 
```

### <--{"id" : "Types"}-->  TCubeDimension

```
 TCubeDimension: *BaseCubeMember & object* 
```

### <--{"id" : "Types"}-->  TCubeMeasure

```
 TCubeMeasure: *BaseCubeMember & object* 
```

### <--{"id" : "Types"}-->  TCubeMember

Name | Type |
------ | ------ |
isVisible? | boolean |
meta? | any |
name | string |
shortTitle | string |
title | string |
type | TCubeMemberType |

### <--{"id" : "Types"}-->  TCubeMemberByType

```
 TCubeMemberByType: *T extends "measures" ? TCubeMeasure : T extends "dimensions" ? TCubeDimension : T extends "segments" ? TCubeSegment : never* 
```

### <--{"id" : "Types"}-->  TCubeMemberType

```
 TCubeMemberType: *"time" | "number" | "string" | "boolean"* 
```

### <--{"id" : "Types"}-->  TCubeSegment

```
 TCubeSegment: *Omit<BaseCubeMember, "type">* 
```

### <--{"id" : "Types"}-->  TDefaultHeuristicsOptions

Name | Type |
------ | ------ |
meta | Meta |
sessionGranularity? | TimeDimensionGranularity |

### <--{"id" : "Types"}-->  TDefaultHeuristicsResponse

Name | Type |
------ | ------ |
chartType? | ChartType |
pivotConfig | PivotConfig &#124; null |
query | Query |
shouldApplyHeuristicOrder | boolean |

### <--{"id" : "Types"}-->  TDefaultHeuristicsState

Name | Type |
------ | ------ |
chartType? | ChartType |
query | Query |

### <--{"id" : "Types"}-->  TDryRunResponse

**`deprecated`** use DryRunResponse

Name | Type |
------ | ------ |
normalizedQueries | Query[] |
pivotQuery | PivotQuery |
queryOrder | Array<object> |
queryType | QueryType |
transformedQueries | TransformedQuery[] |

### <--{"id" : "Types"}-->  TGranularityMap

Name | Type |
------ | ------ |
name | TimeDimensionGranularity &#124; undefined |
title | string |

### <--{"id" : "Types"}-->  TOrderMember

Name | Type |
------ | ------ |
id | string |
order | QueryOrder &#124; "none" |
title | string |

### <--{"id" : "Types"}-->  TQueryOrderArray

```
 TQueryOrderArray: *Array<[string, QueryOrder]>* 
```

### <--{"id" : "Types"}-->  TQueryOrderObject

### <--{"id" : "Types"}-->  TableColumn

Name | Type |
------ | ------ |
children? | TableColumn[] |
dataIndex | string |
format? | any |
key | string |
meta | any |
shortTitle | string |
title | string |
type | string &#124; number |

### <--{"id" : "Types"}-->  TimeDimension

```
 TimeDimension: *TimeDimensionComparison | TimeDimensionRanged* 
```

### <--{"id" : "Types"}-->  TimeDimensionComparison

```
 TimeDimensionComparison: *TimeDimensionBase & TimeDimensionComparisonFields* 
```

### <--{"id" : "Types"}-->  TimeDimensionComparisonFields

Name | Type |
------ | ------ |
compareDateRange | Array<DateRange> |
dateRange? | never |

### <--{"id" : "Types"}-->  TimeDimensionGranularity

```
 TimeDimensionGranularity: *"second" | "minute" | "hour" | "day" | "week" | "month" | "quarter" | "year"* 
```

### <--{"id" : "Types"}-->  TimeDimensionRanged

```
 TimeDimensionRanged: *TimeDimensionBase & TimeDimensionRangedFields* 
```

### <--{"id" : "Types"}-->  TimeDimensionRangedFields

Name | Type |
------ | ------ |
dateRange? | DateRange |

### <--{"id" : "Types"}-->  TransformedQuery

Name | Type |
------ | ------ |
allFiltersWithinSelectedDimensions | boolean |
granularityHierarchies | Record<string, string[]> |
hasMultipliedMeasures | boolean |
hasNoTimeDimensionsWithoutGranularity | boolean |
isAdditive | boolean |
leafMeasureAdditive | boolean |
leafMeasures | string[] |
measureToLeafMeasures? | Record<string, LeafMeasure[]> |
measures | string[] |
sortedDimensions | string[] |
sortedTimeDimensions | [[string, string]] |

### <--{"id" : "Types"}-->  TransportOptions

Name | Type | Description |
------ | ------ | ------ |
apiUrl | string | path to `/cubejs-api/v1` |
authorization | string | [jwt auth token](security) |
credentials? | "omit" &#124; "same-origin" &#124; "include" | - |
headers? | Record<string, string> | custom headers |
method? | "GET" &#124; "PUT" &#124; "POST" &#124; "PATCH" | - |

### <--{"id" : "Types"}-->  UnaryOperator

```
 UnaryOperator: *"set" | "notSet"* 
```

### <--{"id" : "Types"}-->  UsedPreAggregation

Name | Type |
------ | ------ |
targetTableName | string |
type | PreAggregationType |

## GRANULARITIES

```
 GRANULARITIES: *TGranularityMap[]* 
```
