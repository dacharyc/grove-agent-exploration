# Mdb-Vs-Index - Other

**Pages:** 1

---

## How to Index Fields for Vector Search

**URL:** https://www.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type.md/

**Contents:**
- Considerations
  - API Keys
  - Billing
  - Querying
- Syntax
- MongoDB Vector Search Index Fields
  - About the `vector` Type
    - About the Similarity Functions
  - About the `vector` Type
    - About the Similarity Functions

You can use the `vectorSearch` type to index fields for running [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch) queries. You can define the index for the vector embeddings in your data that you want to query and any additional fields in your collection that you want to use to [pre-filter](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-filter) your data. Filtering your data is useful to narrow the scope of your semantic search and ensure that certain vector embeddings are not considered for comparison, such as in a multi-tenant environment.

You can't use the [`$search`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/aggregation-stages/search/#mongodb-pipeline-pipe.-search) [vectorSearch](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/operators-collectors/vectorSearch/#std-label-fts-vectorSearch-ref) or the deprecated [knnBeta](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/operators-collectors/knn-beta/#std-label-knn-beta-ref) operator to query fields indexed using the `vectorSearch` type index definition.

In a `vectorSearch` type index definition, you can index arrays with only a single element. You can't index embedding fields inside arrays of documents or embedding fields inside arrays of objects. You can index embedding fields inside documents using dot notation. The same embedding field can't be indexed multiple times in the same index defintion.

Before indexing your embeddings, we recommend converting your embeddings to BSON (Binary Javascript Object Notation) [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/) vectors with subtype `float32`, `int1`, or `int8` for efficient storage in your cluster.  To learn more, see [how to convert your embeddings to BSON vectors](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-quantization/#std-label-avs-bindata-vector-subtype).

MongoDB Vector SearchWhen you use MongoDB Vector Search indexes, you might experience elevated resource consumption on an idle node for your Atlas cluster. This is due to the underlying [mongot](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/query-ref/#std-label-about-mongot) process, which performs various essential operations for MongoDB Vector Search. The CPU utilization on an idle node can vary depending on the number, complexity, and size of the indexes.

To learn more about sizing considerations for your indexes, see [Memory Requirements for Indexing Vectors](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/deployment-options/#std-label-avs-index-memory-requirements).

If you make changes to the collection for which you defined a MongoDB Vector Search index, the latest data might not be available immediately for queries. However, `mongot` monitors the change streams and updates stored copies of data, making MongoDB Vector Search indexes eventually consistent. You can view the number of indexed Documents in the Atlas UI to verify that changes to the collection are reflected in the index.

Alternatively, you can create a new index after adding new documents to your collection and wait for the index to become queryable. You can also implement a polling logic similar to the following to ensure that the index is ready for querying before attempting to use it.

You can use the `vectorSearch` type to index fields for running [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch) queries. You can define the index for the vector embeddings in your data that you want to query and any additional fields in your collection that you want to use to [pre-filter](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-filter) your data. Filtering your data is useful to narrow the scope of your semantic search and ensure that certain vector embeddings are not considered for comparison, such as in a multi-tenant environment.

You can't use the [`$search`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/aggregation-stages/search/#mongodb-pipeline-pipe.-search) [vectorSearch](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/operators-collectors/vectorSearch/#std-label-fts-vectorSearch-ref) or the deprecated [knnBeta](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/operators-collectors/knn-beta/#std-label-knn-beta-ref) operator to query fields indexed using the `vectorSearch` type index definition.

Automated Embeddings is available as a Preview feature. The feature and the corresponding documentation might change at any time during the Preview period. Do not use this feature in your production environment. To learn more, see [Preview Features](https://www.mongodb.com/docs/preview-features/).

You can configure MongoDB Vector Search to automatically generate and manage vector embeddings for the text data in your collection and run [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch) queries against the generated embeddings. You can create a MongoDB Vector Search index with type `autoEmbed` and choose from available Voyage AI embedding models to generate embeddings, simplifying indexing, updating, and querying with vectors.

When you configure Automated Embedding, MongoDB Vector Search automatically generates embeddings using the specified embedding model at index-time for the specified text field in your collection, during updates, and at query-time for your query text against the field indexed for automated embeddings. MongoDB Vector Search uses the Voyage AI API (Application Programming Interface) key that you specified during deployment to generate the embeddings.

You can also index additional fields in your collection for [pre-filtering](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-filter) your data. Filtering your data is useful to narrow the scope of your semantic search and ensure that certain vector embeddings are not considered for comparison.

In a `vectorSearch` type index definition, you can index arrays with only a single element. You can't index embedding fields inside arrays of documents or embedding fields inside arrays of objects. You can index embedding fields inside documents using dot notation. The same embedding field can't be indexed multiple times in the same index defintion.

Before indexing your embeddings, we recommend converting your embeddings to BSON (Binary Javascript Object Notation) [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/) vectors with subtype `float32`, `int1`, or `int8` for efficient storage in your cluster.  To learn more, see [how to convert your embeddings to BSON vectors](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-quantization/#std-label-avs-bindata-vector-subtype).

MongoDB Vector SearchWhen you use MongoDB Vector Search indexes, you might experience elevated resource consumption on an idle node for your Atlas cluster. This is due to the underlying [mongot](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/query-ref/#std-label-about-mongot) process, which performs various essential operations for MongoDB Vector Search. The CPU utilization on an idle node can vary depending on the number, complexity, and size of the indexes.

To learn more about sizing considerations for your indexes, see [Memory Requirements for Indexing Vectors](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/deployment-options/#std-label-avs-index-memory-requirements).

If you make changes to the collection for which you defined a MongoDB Vector Search index, the latest data might not be available immediately for queries. However, `mongot` monitors the change streams and updates stored copies of data, making MongoDB Vector Search indexes eventually consistent. You can view the number of indexed Documents in the Atlas UI to verify that changes to the collection are reflected in the index.

Alternatively, you can create a new index after adding new documents to your collection and wait for the index to become queryable. You can also implement a polling logic similar to the following to ensure that the index is ready for querying before attempting to use it.

If your collection already has embeddings, you must use the `vector` type fields to index the embeddings. In a `vectorSearch` type index definition, you can index arrays with only a single element. You can't index embedding fields inside arrays of documents or embedding fields inside arrays of objects. You can index embedding fields inside documents using dot notation. The same embedding field can't be indexed multiple times in the same index definition.

Before indexing your embeddings, we recommend converting your embeddings to BSON (Binary Javascript Object Notation) [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/) vectors with subtype `float32`, `int1`, or `int8` for efficient storage in your cluster.  To learn more, see [how to convert your embeddings to BSON vectors](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-quantization/#std-label-avs-bindata-vector-subtype).

If you make changes to the collection for which you defined a MongoDB Vector Search index, the latest data might not be available immediately for queries. However, `mongot` monitors the change streams and updates stored copies of data, making MongoDB Vector Search indexes eventually consistent. You can implement a polling logic similar to the following to ensure that the index is ready for querying before attempting to use it.

If you want to generate embeddings for text data in your collection, you can use the `autoEmbed` type to index a field with text data. You must have a Voyage AI API (Application Programming Interface) key to generate the embeddings.

To automatically generate embeddings for your data, MongoDB Vector Search uses the Voyage AI API (Application Programming Interface) key that you provided during deployment of `mongot`.

You can generate API (Application Programming Interface) keys [using](https://www.mongodb.com/docs/voyageai/management/api-keys/#std-label-voyage-api-keys) your Atlas account, which allows you to manage your API (Application Programming Interface) key from the Atlas UI. To learn more about generating and managing API (Application Programming Interface) keys including [configuring the rate limits](https://www.mongodb.com/docs/voyageai/management/rate-limits/#std-label-voyage-rate-limits) (which is a combination of TPM (Tokens Per Minute) and RPM (Requests Per Minute)) and [monitoring API key usage](https://www.mongodb.com/docs/voyageai/management/monitor-usage/#std-label-voyage-monitor-usage), see [Model API Keys](https://www.mongodb.com/docs/voyageai/management/api-keys/#std-label-voyage-manage-api-keys).

Alternatively, you can generate the API (Application Programming Interface) key directly from [Voyage AI](https://www.voyageai.com/). If you generate Voyage AI API (Application Programming Interface) key directly from Voyage AI, to learn more about managing the API (Application Programming Interface) keys, see [API Keys](https://docs.voyageai.com/docs/api-key-and-installation/).

Voyage AI model pricing is usage-based, with charges billed to the account linked to the API (Application Programming Interface) key used for access. Pricing is based on the number of tokens in your text field and queries.

If you generated the API (Application Programming Interface) key using your Atlas account, you can monitor API (Application Programming Interface) key usage from the Atlas UI. To learn more, see [Billing](https://www.mongodb.com/docs/voyageai/management/billing/#std-label-voyage-billing).

If you generated Voyage AI API (Application Programming Interface) key directly from Voyage AI, see [Pricing](https://docs.voyageai.com/docs/pricing/) to learn more about the charge for requests to the Voyage AI embedding endpoint.

You must use the [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch) stage to query fields indexed as the `autoEmbed` type.

You can't use the [`$search`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/aggregation-stages/search/#mongodb-pipeline-pipe.-search) [vectorSearch](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/operators-collectors/vectorSearch/#std-label-fts-vectorSearch-ref) or the deprecated [knnBeta](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/operators-collectors/knn-beta/#std-label-knn-beta-ref) operator to query fields indexed using the `vectorSearch` type index definition.

The following syntax defines the `vector` type:

The following syntax defines the `vector` type:

The following syntax defines the `autoEmbed` type:

The MongoDB Vector Search index definition takes the following fields:

[About the Similarity Functions](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-similarity-functions)[About the `vector` Type](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-vector)<table>
<tr>
<th id="Option">
Option

</th>
<th id="Type">
Type

</th>
<th id="Necessity">
Necessity

</th>
<th id="Purpose">
Purpose

</th>
</tr>
<tr>
<td headers="Option">
`fields`

</td>
<td headers="Type">
Array of field definition documents

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Definitions for the vector and filter fields to index, one definition per document. Each field definition document specifies the `type`, `path`, and other configuration options for the field to index.

The `fields` array must contain at least one `vector`-type field definition. You can add additional `filter`-type field definitions to your array to pre-filter your data.

</td>
</tr>
<tr>
<td headers="Option">
`fields.``type`

</td>
<td headers="Type">
String

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Field type to use to index fields for [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch). You can specify one of the following values:

- `vector` - for fields that contain vector embeddings.

- `filter` - for additional fields to filter on. You can filter on boolean, date, objectId, numeric, string, and UUID values, including arrays of these types.

To learn more, see [About the `vector` Type](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-vector) and [About the `filter` Type](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-filter).

</td>
</tr>
<tr>
<td headers="Option">
`fields.``path`

</td>
<td headers="Type">
String

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Name of the field to index. For nested fields, use dot notation to specify path to embedded fields.

</td>
</tr>
<tr>
<td headers="Option">
`fields.``numDimensions`

</td>
<td headers="Type">
Int

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time. You can set this field only for `vector`-type fields. You must specify a value less than or equal to `8192`.

For indexing quantized vectors or [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/), you can specify one of the following values:

- `1` to `8192` for `int8` vectors for ingestion.

- Multiple of `8` for `int1` vectors for ingestion.

- `1` to `8192` for `binData(float32)` and `array(float32)` vectors for automatic scalar quantization.

- Multiple of `8` for `binData(float32)` and `array(float32)` vectors for automatic binary quantization.

The embedding model you choose determines the number of dimensions in your vector embeddings, with some models having multiple options for how many dimensions are output. To learn more, see [Choosing a Method to Create Embeddings](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/crud-embeddings/create-embeddings-manual/#std-label-choose-embedding-method).

</td>
</tr>
<tr>
<td headers="Option">
`fields.``similarity`

</td>
<td headers="Type">
String

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Vector similarity function to use to search for top K-nearest neighbors. You can set this field only for `vector`-type fields.

You can specify one of the following values:

- `euclidean` - measures the distance between ends of vectors.

- `cosine` - measures similarity based on the angle between vectors.

- `dotProduct` - measures similarity like `cosine`, but takes into account the magnitude of the vector.

To learn more, see [About the Similarity Functions](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-similarity-functions).

</td>
</tr>
<tr>
<td headers="Option">
`fields.``quantization`

</td>
<td headers="Type">
String

</td>
<td headers="Necessity">
Optional

</td>
<td headers="Purpose">
Type of automatic vector quantization for your vectors. Use this setting only if your embeddings are `float` or `double` vectors.

You can specify one of the following values:

- `none` - Indicates no automatic quantization for the vector embeddings. Use this setting if you have pre-quantized vectors for ingestion. If omitted, this is the default value.

- `scalar` - Indicates scalar quantization, which transforms values to 1 byte integers.

- `binary` - Indicates binary quantization, which transforms values to a single bit. To use this value, `numDimensions` must be a multiple of 8.

If precision is critical, select `none` or `scalar` instead of `binary`.

To learn more, see [Vector Quantization](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-quantization/#std-label-avs-quantization).

</td>
</tr>
<tr>
<td headers="Option">
`fields.``hnswOptions`

</td>
<td headers="Type">
Object

</td>
<td headers="Necessity">
Optional

</td>
<td headers="Purpose">
Parameters to use for [Hierarchical Navigable Small Worlds](https://arxiv.org/abs/1603.09320) graph construction. If omitted, uses the default values for the `maxEdges` and `numEdgeCandidates` parameters.

IMPORTANT: This is available as a Preview feature. Modifying the default values might negatively impact your MongoDB Vector Search index and queries.

</td>
</tr>
<tr>
<td headers="Option">
`fields.``hnswOptions.``maxEdges`

</td>
<td headers="Type">
Int

</td>
<td headers="Necessity">
Optional

</td>
<td headers="Purpose">
Maximum number of edges (or connections) that a node can have in the [Hierarchical Navigable Small Worlds](https://arxiv.org/abs/1603.09320) graph. Value can be between `16` and `64`, both inclusive. If omitted, defaults to `16`. For example, for a value of `16`, each node can have a maximum of sixteen outgoing edges at each layer of the [Hierarchical Navigable Small Worlds](https://arxiv.org/abs/1603.09320) graph.

A higher number improves [recall](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-recall) (accuracy of search results) because the graph is better connected. However, this slows down query speed because of the number of neighbors to evaluate per graph node, increases the memory for the [Hierarchical Navigable Small Worlds](https://arxiv.org/abs/1603.09320) graph because each node stores more connections, and slows down indexing because MongoDB Vector Search evaluates more neighbors and adjusts for every new node added to the graph.

</td>
</tr>
<tr>
<td headers="Option">
`fields.``hnswOptions.``numEdgeCandidates`

</td>
<td headers="Type">
Int

</td>
<td headers="Necessity">
Optional

</td>
<td headers="Purpose">
Analogous to `numCandidates` at query-time, this parameter controls the maximum number of nodes to evaluate to find the closest neighbors to connect to a new node. Value can be between `100` and `3200`, both inclusive. If omitted, defaults to `100`.

A higher number provides a graph with high-quality connections, which can improve search quality (recall), but it can also negatively affect query latency.

</td>
</tr>
</table>The MongoDB Vector Search index definition takes the following fields:

[About the Similarity Functions](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-mdb-vs-similarity-functions)[About the `vector` Type](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-mdb-vs-types-vector)<table>
<tr>
<th id="Option">
Option

</th>
<th id="Type">
Type

</th>
<th id="Necessity">
Necessity

</th>
<th id="Purpose">
Purpose

</th>
</tr>
<tr>
<td headers="Option">
`fields`

</td>
<td headers="Type">
Array of field definition documents

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Definitions for the vector and filter fields to index, one definition per document. Each field definition document specifies the `type`, `path`, and other configuration options for the field to index.

The `fields` array must contain at least one `vector`-type field definition. You can add additional `filter`-type field definitions to your array to pre-filter your data.

</td>
</tr>
<tr>
<td headers="Option">
`fields.``type`

</td>
<td headers="Type">
String

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Field type to use to index fields for [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch). You can specify one of the following values:

- `vector` - for fields that contain vector embeddings.

- `filter` - for additional fields to filter on. You can filter on boolean, date, objectId, numeric, string, and UUID values, including arrays of these types.

To learn more, see [About the `vector` Type](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-mdb-vs-types-vector) and [About the `filter` Type](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-filter).

</td>
</tr>
<tr>
<td headers="Option">
`fields.``path`

</td>
<td headers="Type">
String

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Name of the field to index. For nested fields, use dot notation to specify path to embedded fields.

</td>
</tr>
<tr>
<td headers="Option">
`fields.``numDimensions`

</td>
<td headers="Type">
Int

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time. You can set this field only for `vector`-type fields. You must specify a value less than or equal to `8192`.

For indexing quantized vectors or [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/), you can specify one of the following values:

- `1` to `8192` for `int8` vectors for ingestion.

- Multiple of `8` for `int1` vectors for ingestion.

- `1` to `8192` for `binData(float32)` and `array(float32)` vectors for automatic scalar quantization.

- Multiple of `8` for `binData(float32)` and `array(float32)` vectors for automatic binary quantization.

The embedding model you choose determines the number of dimensions in your vector embeddings, with some models having multiple options for how many dimensions are output. To learn more, see [Choosing a Method to Create Embeddings](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/crud-embeddings/create-embeddings-manual/#std-label-choose-embedding-method).

</td>
</tr>
<tr>
<td headers="Option">
`fields.``similarity`

</td>
<td headers="Type">
String

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Vector similarity function to use to search for top K-nearest neighbors. You can set this field only for `vector`-type fields.

You can specify one of the following values:

- `euclidean` - measures the distance between ends of vectors.

- `cosine` - measures similarity based on the angle between vectors.

- `dotProduct` - measures similarity like `cosine`, but takes into account the magnitude of the vector.

To learn more, see [About the Similarity Functions](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-mdb-vs-similarity-functions).

</td>
</tr>
<tr>
<td headers="Option">
`fields.``quantization`

</td>
<td headers="Type">
String

</td>
<td headers="Necessity">
Optional

</td>
<td headers="Purpose">
Type of automatic vector quantization for your vectors. Use this setting only if your embeddings are `float` or `double` vectors.

You can specify one of the following values:

- `none` - Indicates no automatic quantization for the vector embeddings. Use this setting if you have pre-quantized vectors for ingestion. If omitted, this is the default value.

- `scalar` - Indicates scalar quantization, which transforms values to 1 byte integers.

- `binary` - Indicates binary quantization, which transforms values to a single bit. To use this value, `numDimensions` must be a multiple of 8.

If precision is critical, select `none` or `scalar` instead of `binary`.

To learn more, see [Vector Quantization](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-quantization/#std-label-avs-quantization).

</td>
</tr>
<tr>
<td headers="Option">
`fields.``hnswOptions`

</td>
<td headers="Type">
Object

</td>
<td headers="Necessity">
Optional

</td>
<td headers="Purpose">
Parameters to use for [Hierarchical Navigable Small Worlds](https://arxiv.org/abs/1603.09320) graph construction. If omitted, uses the default values for the `maxEdges` and `numEdgeCandidates` parameters.

IMPORTANT: This is available as a Preview feature. Modifying the default values might negatively impact your MongoDB Vector Search index and queries.

</td>
</tr>
<tr>
<td headers="Option">
`fields.``hnswOptions.``maxEdges`

</td>
<td headers="Type">
Int

</td>
<td headers="Necessity">
Optional

</td>
<td headers="Purpose">
Maximum number of edges (or connections) that a node can have in the [Hierarchical Navigable Small Worlds](https://arxiv.org/abs/1603.09320) graph. Value can be between `16` and `64`, both inclusive. If omitted, defaults to `16`. For example, for a value of `16`, each node can have a maximum of sixteen outgoing edges at each layer of the [Hierarchical Navigable Small Worlds](https://arxiv.org/abs/1603.09320) graph.

A higher number improves [recall](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-recall) (accuracy of search results) because the graph is better connected. However, this slows down query speed because of the number of neighbors to evaluate per graph node, increases the memory for the [Hierarchical Navigable Small Worlds](https://arxiv.org/abs/1603.09320) graph because each node stores more connections, and slows down indexing because MongoDB Vector Search evaluates more neighbors and adjusts for every new node added to the graph.

</td>
</tr>
<tr>
<td headers="Option">
`fields.``hnswOptions.``numEdgeCandidates`

</td>
<td headers="Type">
Int

</td>
<td headers="Necessity">
Optional

</td>
<td headers="Purpose">
Analogous to `numCandidates` at query-time, this parameter controls the maximum number of nodes to evaluate to find the closest neighbors to connect to a new node. Value can be between `100` and `3200`, both inclusive. If omitted, defaults to `100`.

A higher number provides a graph with high-quality connections, which can improve search quality (recall), but it can also negatively affect query latency.

</td>
</tr>
</table>The MongoDB Vector Search index definition takes the following fields:

<table>
<tr>
<th id="Option">
Option

</th>
<th id="Type">
Type

</th>
<th id="Necessity">
Necessity

</th>
<th id="Purpose">
Purpose

</th>
</tr>
<tr>
<td headers="Option">
`fields`

</td>
<td headers="Type">
Array of field definition documents

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Definitions for the vector and filter fields to index, one definition per document. Each field definition document specifies the `type`, `path`, and other configuration options for the field to index.

The `fields` array must contain one `autoEmbed` type field definition. You can add additional `filter`-type field definitions to your array to pre-filter your data.

</td>
</tr>
<tr>
<td headers="Option">
`fields.``type`

</td>
<td headers="Type">
String

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Field type to use to index fields for [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch). You can specify one of the following values:

- `autoEmbed` - for automatically generating vector embeddings.

- `filter` - for pre-filtering documents by non-vector fields. You can filter on boolean, date, objectId, numeric, string, and UUID values, including arrays of these types.

To learn more, see [About the `autoEmbed` Type](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-auto-embed) and [About the `filter` Type](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-filter).

</td>
</tr>
<tr>
<td headers="Option">
`fields.``path`

</td>
<td headers="Type">
String

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Name of the text field to index. For nested fields, use dot notation to specify the path to embedded fields. If the text value in the specified field exceeds 32,000 tokens, MongoDB Vector Search automatically truncates during indexing to fit the context window of the embedding model.

</td>
</tr>
<tr>
<td headers="Option">
`fields.``modality`

</td>
<td headers="Type">
string

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Type of data in the field that you specified in the `path`. Value must be `text`.

</td>
</tr>
<tr>
<td headers="Option">
`fields.``model`

</td>
<td headers="Type">
string

</td>
<td headers="Necessity">
Required

</td>
<td headers="Purpose">
Voyage AI embedding model to use for generating the embeddings. You can specify one of the following models:

- `voyage-4-lite` - Optimized for high-volume, cost-sensitive applications.

- `voyage-4` - (**Recommended**) Balanced performance for general text search.

- `voyage-4-large` - Maximum accuracy for complex semantic relationships.

- `voyage-code-3` - Specialized for code search and technical documentation.

</td>
</tr>
</table>
### About the `vector` Type

Your index definition's `vector` field must contain an array of numbers of *one* of the following types:

- BSON (Binary Javascript Object Notation) `double`

- BSON (Binary Javascript Object Notation) [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/) `vector` subtype `float32`

- BSON (Binary Javascript Object Notation) [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/) `vector` subtype `int1`

- BSON (Binary Javascript Object Notation) [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/) `vector` subtype `int8`

To learn more about generating BSON (Binary Javascript Object Notation) [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/) vectors with subtype `float32`
`int1` or `int8` for your data, see [How to Ingest Pre-Quantized Vectors](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-quantization/#std-label-avs-bindata-vector-subtype).

You must index the vector field as the `vector` type inside the `fields` array.

The following syntax defines the `vector` field type:

MongoDB Vector Search supports the following similarity functions:

- `euclidean` - measures the distance between ends of vectors. This value allows you to measure similarity based on varying dimensions. To learn more, see [Euclidean](https://en.wikipedia.org/wiki/Euclidean_distance).

- `cosine` - measures similarity based on the angle between  vectors. This value allows you to measure similarity that isn't scaled by magnitude. You can't use zero magnitude vectors with `cosine`. To measure cosine similarity, we recommend that you normalize your vectors and use `dotProduct` instead.

- `dotProduct` - measures similarity like `cosine`, but takes into account the magnitude of the vector. If you normalize the magnitude, `cosine` and `dotProduct` are almost identical in measuring similarity.

To use `dotProduct`, you must normalize the vector to unit length at index-time and query-time.

The following table shows the similarity functions for the various types:

<table>
<tr>
<th id="Vector%20Embeddings%20Type">
Vector Embeddings Type

</th>
<th id="euclidean">
`euclidean`

</th>
<th id="cosine">
`cosine`

</th>
<th id="dotProduct">
`dotProduct`

</th>
</tr>
<tr>
<td headers="Vector%20Embeddings%20Type">
`binData(int1)`

</td>
<td headers="euclidean">
√

</td>
<td headers="cosine">

</td>
<td headers="dotProduct">

</td>
</tr>
<tr>
<td headers="Vector%20Embeddings%20Type">
`binData(int8)`

</td>
<td headers="euclidean">
√

</td>
<td headers="cosine">
√

</td>
<td headers="dotProduct">
√

</td>
</tr>
<tr>
<td headers="Vector%20Embeddings%20Type">
`binData(float32)`

</td>
<td headers="euclidean">
√

</td>
<td headers="cosine">
√

</td>
<td headers="dotProduct">
√

</td>
</tr>
<tr>
<td headers="Vector%20Embeddings%20Type">
`array(float32)`

</td>
<td headers="euclidean">
√

</td>
<td headers="cosine">
√

</td>
<td headers="dotProduct">
√

</td>
</tr>
</table> For vector ingestion.

For automatic scalar or binary quantization.

The formula for each similarity function is as follows:

For `cosine`, MongoDB Vector Search uses the following algorithm to normalize the score:

- This algorithm normalizes the score by considering the similarity score of the document vector (`v1`) and the query vector (`v2`), which has the range [`-1`, `1`]. MongoDB Vector Search adds `1` to the similarity score to normalize the score to a range [`0`, `2`] and then divides by `2` to ensure a value between `0` and `1`.

<Tab name="Dot Product">

For `dotProduct`, MongoDB Vector Search uses the following algorithm to normalize the score:

- This algorithm normalizes the score by considering the similarity score of the document vector (`v1`) and the query vector (`v2`), which has the range [`-1`, `1`]. MongoDB Vector Search adds `1` to the similarity score to normalize the score to a range [`0`, `2`] and then divides by `2` to ensure a value between `0` and `1`.

<Tab name="Euclidean">

For `euclidean` similarity, MongoDB Vector Search uses the following algorithm to normalize the score to ensure a value between `0` and `1`:

- This algorithm normalizes the score by calculating the euclidean distance, which is the distance between the document vector (`v1`) and the query vector (`v2`), which has the range [`0`, `∞`]. MongoDB Vector Search then transforms the distance to a similarity score by adding `1` to the distance and then divides `1` by the result to ensure a value between `0` and `1`.

For best performance, check your embedding model to determine which similarity function aligns with your embedding model's training process. If you don't have any guidance, start with `dotProduct`. Setting `fields.similarity` to the `dotProduct` value allows you to efficiently measure similarity based on both angle and magnitude. `dotProduct` consumes less computational resources than `cosine` and is efficient when vectors are of unit length. However, if your vectors aren't normalized, evaluate the similarity scores in the results of a sample query for `euclidean` distance and `cosine` similarity to determine which corresponds to reasonable results.

Your index definition's `vector` field must contain an array of numbers of *one* of the following types:

- BSON (Binary Javascript Object Notation) `double`

- BSON (Binary Javascript Object Notation) [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/) `vector` subtype `float32`

- BSON (Binary Javascript Object Notation) [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/) `vector` subtype `int1`

- BSON (Binary Javascript Object Notation) [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/) `vector` subtype `int8`

To learn more about generating BSON (Binary Javascript Object Notation) [BinData](https://www.mongodb.com/docs/manual/reference/method/BinData/) vectors with subtype `float32`
`int1` or `int8` for your data, see [How to Ingest Pre-Quantized Vectors](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-quantization/#std-label-avs-bindata-vector-subtype).

You must index the vector field as the `vector` type inside the `fields` array.

The following syntax defines the `vector` field type:

MongoDB Vector Search supports the following similarity functions:

- `euclidean` - measures the distance between ends of vectors. This value allows you to measure similarity based on varying dimensions. To learn more, see [Euclidean](https://en.wikipedia.org/wiki/Euclidean_distance).

- `cosine` - measures similarity based on the angle between  vectors. This value allows you to measure similarity that isn't scaled by magnitude. You can't use zero magnitude vectors with `cosine`. To measure cosine similarity, we recommend that you normalize your vectors and use `dotProduct` instead.

- `dotProduct` - measures similarity like `cosine`, but takes into account the magnitude of the vector. If you normalize the magnitude, `cosine` and `dotProduct` are almost identical in measuring similarity.

To use `dotProduct`, you must normalize the vector to unit length at index-time and query-time.

The following table shows the similarity functions for the various types:

<table>
<tr>
<th id="Vector%20Embeddings%20Type">
Vector Embeddings Type

</th>
<th id="euclidean">
`euclidean`

</th>
<th id="cosine">
`cosine`

</th>
<th id="dotProduct">
`dotProduct`

</th>
</tr>
<tr>
<td headers="Vector%20Embeddings%20Type">
`binData(int1)`

</td>
<td headers="euclidean">
√

</td>
<td headers="cosine">

</td>
<td headers="dotProduct">

</td>
</tr>
<tr>
<td headers="Vector%20Embeddings%20Type">
`binData(int8)`

</td>
<td headers="euclidean">
√

</td>
<td headers="cosine">
√

</td>
<td headers="dotProduct">
√

</td>
</tr>
<tr>
<td headers="Vector%20Embeddings%20Type">
`binData(float32)`

</td>
<td headers="euclidean">
√

</td>
<td headers="cosine">
√

</td>
<td headers="dotProduct">
√

</td>
</tr>
<tr>
<td headers="Vector%20Embeddings%20Type">
`array(float32)`

</td>
<td headers="euclidean">
√

</td>
<td headers="cosine">
√

</td>
<td headers="dotProduct">
√

</td>
</tr>
</table> For vector ingestion.

For automatic scalar or binary quantization.

The formula for each similarity function is as follows:

For `cosine`, MongoDB Vector Search uses the following algorithm to normalize the score:

- This algorithm normalizes the score by considering the similarity score of the document vector (`v1`) and the query vector (`v2`), which has the range [`-1`, `1`]. MongoDB Vector Search adds `1` to the similarity score to normalize the score to a range [`0`, `2`] and then divides by `2` to ensure a value between `0` and `1`.

<Tab name="Dot Product">

For `dotProduct`, MongoDB Vector Search uses the following algorithm to normalize the score:

- This algorithm normalizes the score by considering the similarity score of the document vector (`v1`) and the query vector (`v2`), which has the range [`-1`, `1`]. MongoDB Vector Search adds `1` to the similarity score to normalize the score to a range [`0`, `2`] and then divides by `2` to ensure a value between `0` and `1`.

<Tab name="Euclidean">

For `euclidean` similarity, MongoDB Vector Search uses the following algorithm to normalize the score to ensure a value between `0` and `1`:

- This algorithm normalizes the score by calculating the euclidean distance, which is the distance between the document vector (`v1`) and the query vector (`v2`), which has the range [`0`, `∞`]. MongoDB Vector Search then transforms the distance to a similarity score by adding `1` to the distance and then divides `1` by the result to ensure a value between `0` and `1`.

For best performance, check your embedding model to determine which similarity function aligns with your embedding model's training process. If you don't have any guidance, start with `dotProduct`. Setting `fields.similarity` to the `dotProduct` value allows you to efficiently measure similarity based on both angle and magnitude. `dotProduct` consumes less computational resources than `cosine` and is efficient when vectors are of unit length. However, if your vectors aren't normalized, evaluate the similarity scores in the results of a sample query for `euclidean` distance and `cosine` similarity to determine which corresponds to reasonable results.

Your index definition's `autoEmbed` field must contain only text. When configuring fields for Automated Embedding, the following limitations apply:

- You can't configure both `vector` and `autoEmbed` type fields in the same index definition. MongoDB Vector Search throws an exception if you define fields of both types in the same index.

- You must use the same embedding model for all the `autoEmbed` type fields in the same index. MongoDB Vector Search throws an exception if you specify multiple models in the same index.

- After MongoDB Vector Search creates the index, you can't edit or delete `autoEmbed` type fields in the index.

You can optionally index additional fields to pre-filter your data. You can filter on boolean, date, objectId, numeric, string, and UUID values, including arrays of these types. Filtering your data is useful to narrow the scope of your semantic search and ensure that not all vectors are considered for comparison. It reduces the number of documents against which to run similarity comparisons, which can decrease query latency and increase the accuracy of search results.

You must index the fields that you want to filter by using the `filter` type inside the `fields` array.

The following syntax defines the `filter` field type:

Pre-filtering your data doesn't affect the score that MongoDB Vector Search returns using `vectorSearchScore` for [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch) queries.

You can optionally index additional fields to pre-filter your data. You can filter on boolean, date, objectId, numeric, string, and UUID values, including arrays of these types. Filtering your data is useful to narrow the scope of your semantic search and ensure that not all vectors are considered for comparison. It reduces the number of documents against which to run similarity comparisons, which can decrease query latency and increase the accuracy of search results.

You must index the fields that you want to filter by using the `filter` type inside the `fields` array.

The following syntax defines the `filter` field type:

Pre-filtering your data doesn't affect the score that MongoDB Vector Search returns using `vectorSearchScore` for [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch) queries.

You can optionally index additional fields to pre-filter your data. You can filter on boolean, date, objectId, numeric, string, and UUID values, including arrays of these types. Filtering your data is useful to narrow the scope of your semantic search and ensure that not all vectors are considered for comparison. It reduces the number of documents against which to run similarity comparisons, which can decrease query latency and increase the accuracy of search results.

You must index the fields that you want to filter by using the `filter` type inside the `fields` array.

The following syntax defines the `filter` field type:

Pre-filtering your data doesn't affect the score that MongoDB Vector Search returns using `vectorSearchScore` for [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch) queries.

You can create and manage MongoDB Vector Search indexes through the Atlas UI, [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), Atlas CLI, Atlas Administration API, and the following [MongoDB Drivers](https://www.mongodb.com/docs/drivers/):

<table>
<tr>
<th id="MongoDB%20Driver">
MongoDB Driver

</th>
<th id="Version">
Version

</th>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[C](https://www.mongodb.com/docs/drivers/c/)

</td>
<td headers="Version">
1.28.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[C++](https://www.mongodb.com/docs/drivers/cxx/)

</td>
<td headers="Version">
3.11.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[C#](https://www.mongodb.com/docs/drivers/csharp/)

</td>
<td headers="Version">
3.1.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Go](https://www.mongodb.com/docs/drivers/go/current/)

</td>
<td headers="Version">
1.16.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Java](https://www.mongodb.com/docs/drivers/java-drivers/)

</td>
<td headers="Version">
5.2.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Kotlin](https://www.mongodb.com/docs/drivers/kotlin/)

</td>
<td headers="Version">
5.2.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Node](https://www.mongodb.com/docs/drivers/node/current/)

</td>
<td headers="Version">
6.6.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[PHP](https://www.mongodb.com/docs/drivers/php-drivers/)

</td>
<td headers="Version">
1.20.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Python](https://www.mongodb.com/docs/drivers/python-drivers/)

</td>
<td headers="Version">
4.7 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Rust](https://www.mongodb.com/docs/drivers/rust/current/)

</td>
<td headers="Version">
3.1.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Scala](https://www.mongodb.com/docs/drivers/scala/)

</td>
<td headers="Version">
5.2.0 or higher

</td>
</tr>
</table>You can create and manage MongoDB Vector Search indexes through the [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), MongoDB Compass, and the following [MongoDB Drivers](https://www.mongodb.com/docs/drivers/):

<table>
<tr>
<th id="MongoDB%20Driver">
MongoDB Driver

</th>
<th id="Version">
Version

</th>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[C](https://www.mongodb.com/docs/drivers/c/)

</td>
<td headers="Version">
1.28.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[C++](https://www.mongodb.com/docs/drivers/cxx/)

</td>
<td headers="Version">
3.11.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[C#](https://www.mongodb.com/docs/drivers/csharp/)

</td>
<td headers="Version">
3.1.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Go](https://www.mongodb.com/docs/drivers/go/current/)

</td>
<td headers="Version">
1.16.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Java](https://www.mongodb.com/docs/drivers/java-drivers/)

</td>
<td headers="Version">
5.2.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Kotlin](https://www.mongodb.com/docs/drivers/kotlin/)

</td>
<td headers="Version">
5.2.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Node](https://www.mongodb.com/docs/drivers/node/current/)

</td>
<td headers="Version">
6.6.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[PHP](https://www.mongodb.com/docs/drivers/php-drivers/)

</td>
<td headers="Version">
1.20.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Python](https://www.mongodb.com/docs/drivers/python-drivers/)

</td>
<td headers="Version">
4.7 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Rust](https://www.mongodb.com/docs/drivers/rust/current/)

</td>
<td headers="Version">
3.1.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Scala](https://www.mongodb.com/docs/drivers/scala/)

</td>
<td headers="Version">
5.2.0 or higher

</td>
</tr>
</table>You can create and manage MongoDB Vector Search indexes through the [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), MongoDB Compass, and the following [MongoDB Drivers](https://www.mongodb.com/docs/drivers/):

<table>
<tr>
<th id="MongoDB%20Driver">
MongoDB Driver

</th>
<th id="Version">
Version

</th>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Node](https://www.mongodb.com/docs/drivers/node/current/)

</td>
<td headers="Version">
6.6.0 or higher

</td>
</tr>
<tr>
<td headers="MongoDB%20Driver">
[Python](https://www.mongodb.com/docs/drivers/python-drivers/)

</td>
<td headers="Version">
4.7 or higher

You can create a MongoDB Vector Search index for all collections that contain vector embeddings less than or equal to 8192 dimensions in length for any kind of data along with other data on your cluster through the Atlas UI, Atlas Administration API, Atlas CLI, [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), or a supported [MongoDB Driver](https://www.mongodb.com/docs/drivers/).

To create a MongoDB Vector Search index, you must have a cluster with the following prerequisites:

- MongoDB version `6.0.11`, `7.0.2`, or higher

- A collection for which to create the MongoDB Vector Search index

create[Supported Clients](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-index-supported-drivers)MongoDB Vector SearchYou can use the [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh) command or driver helper methods to create
MongoDB Vector Search indexes on all Atlas cluster tiers. For a list of supported driver versions, see [Supported Clients](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-index-supported-drivers).

You can create a MongoDB Vector Search index for all collections that contain vector embeddings less than or equal to 8192 dimensions in length for any kind of data along with other data on your cluster through [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), MongoDB Compass, or a supported [MongoDB Driver](https://www.mongodb.com/docs/drivers/).

The following procedure walks through the steps for enabling Automated Embedding in your MongoDB Vector Search index. If you loaded the `sample_mflix` dataset, the example in the procedure demonstrates how to enable Automated Embedding for the `fullplot` field in the `movies` collection.

You need the [`Project Data Access Admin`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/reference/user-roles/#mongodb-authrole-Project-Data-Access-Admin) or higher role to create and manage MongoDB Vector Search indexes.

You cannot create more than:

- 3 indexes (regardless of the type, `search` or `vector`) on `M0` clusters.

- 10 indexes on Flex clusters.

We recommend that you create no more than 2500 search indexes on a single `M10+` cluster.

You need [`readWrite`](https://www.mongodb.com/docs/manual/reference/built-in-roles/#mongodb-authrole-readWrite) or higher role to create and manage MongoDB Vector Search indexes.

The procedure includes index definition examples for the `embedded_movies` collection in the `sample_mflix` database. If you load the [sample data](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/sample-data/sample-mflix/#std-label-mflix-embedded_movies) on your cluster and create the example MongoDB Vector Search indexes for this collection, you can run the sample [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch) queries against this collection. To learn more about the sample queries that you can run, see [$vectorSearch Examples](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#std-label-vectorSearch-agg-pipeline-egs).

Vector Search`vector_index` is the default index name. Index names must be unique within the namespace, regardless of the index type. If you already have an index named `vector_index` on this collection, enter a different name.Select the database for which to create the index. For example, `sample_mflix`.Select the collection for which to create the index. For example, `embedded_movies`.`sample_mflix.embedded_movies``plot_embedding_voyage_3_large`Dot ProductScalar`genres` and  `year` fields#### In Atlas, go to the Search & Vector Search page for your cluster.

You can go the MongoDB Search page from the Search & Vector Search option, or the Data Explorer.

<Tab name="Search & Vector Search">

- If it's not already displayed, select the organization that contains your project from the  Organizations menu in the navigation bar.

- If it's not already displayed, select your project from the Projects menu in the navigation bar.

- In the sidebar, click Search & Vector Search under the Database heading.

If you have no clusters, click Create cluster to create one. To learn more, see [Create a Cluster](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/tutorial/create-new-cluster/#std-label-create-new-cluster).

- If your project has multiple clusters, select the cluster you want to use from the Select cluster dropdown, then click Go to Atlas Search.

The [Search & Vector Search](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fclusters%2FatlasSearch%2F%3Ccluster%3E) page displays.

<Tab name="Data Explorer">

- If it's not already displayed, select the organization that contains your project from the  Organizations menu in the navigation bar.

- If it's not already displayed, select your project from the Projects menu in the navigation bar.

- In the sidebar, click Data Explorer under the Database heading.

- Expand the database and select the collection.

- Click the Indexes tab for the collection.

The [Atlas Search](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fclusters%2FatlasSearch%2F%3Ccluster%3E%3Fdatabase%3Dsample_mflix%26collectionName%3Dusers) page displays.

Make the following selections on the page and then click Next.

<tr>
<td>
Search Type

</td>
<td>
Select the Vector Search index type.

</td>
</tr>
<tr>
<td>
Index Name and Data Source

</td>
<td>
Specify the following information:

- Index Name: `vector_index` is the default index name. Index names must be unique within the namespace, regardless of the index type. If you already have an index named `vector_index` on this collection, enter a different name.

- Database and Collection:

- Select the database for which to create the index. For example, `sample_mflix`.

- Select the collection for which to create the index. For example, `embedded_movies`.

</td>
</tr>
<tr>
<td>
Configuration Method

</td>
<td>
For a guided experience, select Visual Editor.To edit the raw index definition, select JSON Editor.

</td>
</tr>
</table>IMPORTANT:

Your MongoDB Search index is named `default` by default. If you keep this name, then your index will be the default Search index for any MongoDB Search query that does not specify a different `index` option in its [operators](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/operators-and-collectors/#std-label-fts-operators). If you are creating multiple indexes, we recommend that you maintain a consistent, descriptive naming convention across your indexes.

<Tab name="Visual Editor">

Atlas automatically detects fields that contain vector embeddings, as well as their corresponding dimensions, and pre-populates up to three vector fields. To configure the index, do the following:

- If necessary, select the vector field to index from the Path dropdown.

Select Add Another Field to index any additional fields.

- Specify the similarity method for each indexed field in the Similarity Method dropdown menu.

- *(Optional)* Click Advanced and select either Scalar or Binary quantization from the dropdown menu to [automatically quantize](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-quantization/#std-label-avs-automatic-quantization) the embeddings in the field.

- *(Optional)* Specify other fields in your collection to filter the data by in the Filter Field section.

To learn more about the MongoDB Vector Search index settings, see [How to Index Fields for Vector Search](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-vector-search).

<Tab name="Basic Example">

For the `embedded_movies` collection, the `plot_embedding_voyage_3_large` field displays.

To configure the index, select Dot Product from the Similarity Method dropdown.

This index definition indexes only the vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search.

<Tab name="Filter Example">

For the `embedded_movies` collection, the `plot_embedding_voyage_3_large` field displays.

To configure the index, do the following:

- Select Dot Product from the Similarity Method dropdown.

- Click Advanced, then select Scalar quantization from the dropdown menu.

- In the Filter Field section, specify the `genres` and  `year` fields to filter the data by.

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

It also enables automatic quantization (`scalar`) for efficient processing of the embeddings.

<Tab name="JSON Editor">

a MongoDB Vector Search index resembles the following example:

To learn more about the fields in the index, see [How to Index Fields for Vector Search](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-vector-search).

The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type. The `plot_embedding_voyage_3_large` field contains `2048` vector dimension embeddings created using Voyage AI's `voyage-3-large` embedding model. The index measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field for performing vector search.

<Tab name="Advanced Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

It also enables automatic quantization (`scalar`) for efficient processing of the embeddings.

Atlas displays a modal window to let you know your index is building.

The newly created index displays on the Atlas Search tab. While the index is building, the Status field reads Build in Progress. When the index is finished building, the Status field reads Active.

Larger collections take longer to index. You will receive an email notification when your index is finished building.

To create a MongoDB Vector Search index for a collection using the Atlas Administration API, send a `POST` request to the MongoDB Search
`indexes` endpoint with the required parameters.

To learn more about the syntax and parameters for the endpoint, see [Create One MongoDB Search Index](https://www.mongodb.com/docs/api/doc/atlas-admin-api-v2/operation/operation-creategroupclustersearchindex).

The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type. The `plot_embedding_voyage_3_large` field contains `2048` vector dimension embeddings created using Voyage AI's `voyage-3-large` embedding model. The index measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field for performing vector search.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

To create a MongoDB Vector Search index for a collection using the Atlas CLI v1.14.3 or later, perform the following steps:

Your index definition should resemble the following format:

Create a file named `vector-index.json`.

<tr>
<td>
`<name-of-database>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<name-of-collection>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<index-name>`

</td>
<td>
Name of your index. If you omit the index name, MongoDB Vector Search names the index `vector_index`.

</td>
</tr>
<tr>
<td>
`<number-of-dimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<field-to-index>`

</td>
<td>
Vector and filter fields to index.

</td>
</tr>
</table>Copy and paste the following index definition into the `vector-index.json` file. The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type and the `genres` and `year` fields as the `filter` type in a MongoDB Vector Search index. The `plot_embedding_voyage_3_large` field contains `2048` vector dimension embeddings created using Voyage AI's `voyage-3-large` embedding model. The index specifies similarity measurement using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field for performing vector search.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

In the command, replace the following placeholder values:

- `cluster_name` is the name of the cluster that contains the collection for which you want to create the index.

- `vector_index` is the name of the JSON (Javascript Object Notation) file that contains the index definition for the MongoDB Vector Search index.

To learn more about the command syntax and parameters, see the Atlas CLI documentation for the [atlas clusters search indexes create](https://www.mongodb.com/docs/atlas/cli/current/command/atlas-clusters-search-indexes-create/) command.

To create a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh) v2.1.2 or later, perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.createSearchIndex()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.createSearchIndex/#mongodb-method-db.collection.createSearchIndex) method has the following syntax:

The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type. The `plot_embedding_voyage_3_large` field contains `2048` vector dimension embeddings created using Voyage AI's `voyage-3-large` embedding model. The index measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field for performing vector search.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

To create a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh) v2.1.2 or later, perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.createSearchIndex()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.createSearchIndex/#mongodb-method-db.collection.createSearchIndex) method has the following syntax:

The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type. The `plot_embedding_voyage_3_large` field contains `2048` vector dimension embeddings created using Voyage AI's `voyage-3-large` embedding model. The index measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field for performing vector search.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

To create a MongoDB Vector Search index for a collection using the [C#](https://www.mongodb.com/docs/drivers/csharp/current/fundamentals/indexes/#atlas-search-indexes) driver v3.6.0 or later, perform the following steps:

Create a file named `IndexService.cs`.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<documentType>`

</td>
<td>
Class that represents a document in the collection. To learn more, see [POCOs](https://www.mongodb.com/docs/drivers/csharp/current/serialization/poco/) in the C# driver documentation.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index. For this parameter, you can pass either a `FieldDefinition<TDocument>` object or a lambda expression.

</td>
</tr>
<tr>
<td>
`<vectorSimilarity>`

</td>
<td>
Vector similarity function, defined in the `VectorSimilarity` enum.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
</table>Copy and paste the following into the `IndexService.cs` and replace the `<connectionString>` placeholder value. The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type and the `genres` and `year` fields as the `filter` type in a MongoDB Vector Search index. The `plot_embedding_voyage_3_large` field contains embeddings created using Voyage AI's `voyage-3-large` embedding model. The index definition specifies `2048` vector dimensions and measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

To create a MongoDB Vector Search index for a collection using the [MongoDB Go driver](https://www.mongodb.com/docs/drivers/go/current/fundamentals/indexes/) v2.0 or later, perform the following steps:

The MongoDB Go driver supports programmatic MongoDB Vector Search index management starting in v1.16.0, but the preceding code shows the syntax for the v2.x driver.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

</td>
</tr>
</table>Copy and paste the following into the `create-index.go` file and replace the `<connectionString>` placeholder value. The following index definition indexes the `plot_embedding_voyage_3_large field contains embeddings created using Voyage AI's `voyage-3-large` embedding model. The index definition specifies `2048` vector dimensions and measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field (`plot_embedding_voyage_3_largego
package main

import (
	"context"
	"fmt"
	"log"
	"time"

"go.mongodb.org/mongo-driver/v2/bson"
	"go.mongodb.org/mongo-driver/v2/mongo"
	"go.mongodb.org/mongo-driver/v2/mongo/options"
)

func main() {
	ctx := context.Background()

// Replace the placeholder with your connection string
	const uri = "<connectionString>"

// Connect to your cluster
	clientOptions := options.Client().ApplyURI(uri)
	client, err := mongo.Connect(clientOptions)
	if err != nil {
		log.Fatalf("failed to connect to the server: %v", err)
	}
	defer func() { _ = client.Disconnect(ctx) }()

// Set the namespace
	coll := client.Database("sample_mflix").Collection("embedded_movies")

// Define the index details
	type vectorDefinitionField struct {
		Type          string `bson:"type"`
		Path          string `bson:"path"`
		NumDimensions int    `bson:"numDimensions"`
		Similarity    string `bson:"similarity"`
		Quantization  string `bson:"quantization"`
	}

type vectorDefinition struct {
		Fields []vectorDefinitionField `bson:"fields"`
	}

indexName := "vector_index"
	opts := options.SearchIndexes().SetName(indexName).SetType("vectorSearch")

indexModel := mongo.SearchIndexModel{
		Definition: vectorDefinition{
			Fields: []vectorDefinitionField{{
				Type:          "vector",
				Path:          "plot_embedding_voyage_3_large",
				NumDimensions: 2048,
				Similarity:    "dotProduct",
				Quantization:  "scalar"}},
		},
		Options: opts,
	}

// Create the index
	searchIndexName, err := coll.SearchIndexes().CreateOne(ctx, indexModel)
	if err != nil {
		log.Fatalf("failed to create the search index: %v", err)
	}
	log.Println("New search index named " + searchIndexName + " is building.")

// Await the creation of the index.
	log.Println("Polling to check if the index is ready. This may take up to a minute.")
	searchIndexes := coll.SearchIndexes()
	var doc bson.Raw
	for doc == nil {
		cursor, err := searchIndexes.List(ctx, options.SearchIndexes().SetName(searchIndexName))
		if err != nil {
			fmt.Errorf("failed to list search indexes: %w", err)
		}

if !cursor.Next(ctx) {
			break
		}

name := cursor.Current.Lookup("name").StringValue()
		queryable := cursor.Current.Lookup("queryable").Boolean()
		if name == searchIndexName && queryable {
			doc = cursor.Current
		} else {
			time.Sleep(5 * time.Second)
		}
	}

log.Println(searchIndexName + " is ready for querying.")
}

) for performing vector search against pre-filtered data.

To create a MongoDB Vector Search index for a collection using the [MongoDB Java driver](https://www.mongodb.com/docs/drivers/java/sync/current/fundamentals/indexes/) v5.2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

</td>
</tr>
</table>The following example index definitions:

- Index the `plot_embedding_voyage_3_large` field as the `vector` type and the `genres` and `year` fields as the `filter` type in a MongoDB Vector Search index.

- Specifies the `plot_embedding_voyage_3_large` field as the vector embeddings field, which contains embeddings created using Voyage AI's `voyage-3-large` embedding model.

- Specifies `2048` vector dimensions and measures similarity using `dotProduct` function.

<Tab name="Basic Example">

This index definition indexes only the vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search.

Copy and paste the following into the file you created, and replace the `<connectionString>` placeholder value.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

Copy and paste the following into the file you created, and replace the `<connectionString>` placeholder value.

From your IDE, run the file to create the index.

To create a MongoDB Vector Search index for a collection using the [MongoDB Node driver](https://www.mongodb.com/docs/drivers/node/current/fundamentals/indexes/) v6.6.0 or later, perform the following steps:

Create a file named `vector-index.js`.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

</td>
</tr>
</table>Copy and paste the following into the `vector-index.js` file and replace the `<connectionString>` placeholder value. The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type and the `genres` and `year` fields as the `filter` type in a MongoDB Vector Search index. The `plot_embedding_voyage_3_large` field contains embeddings created using Voyage AI's `voyage-3-large` embedding model. The index definition specifies `2048` vector dimensions and measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

To create MongoDB Vector Search indexes for a collection using [PyMongo](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/indexes/atlas-search-index/) driver v4.7 or later, perform the following steps:

To learn more, see the [create_search_index()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.create_search_index) method.

To learn more, see the [create_search_indexes()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.create_search_indexes) method.

Create a file named `vector-index.py`.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

</td>
</tr>
</table>Copy and paste the following into the `vector-index.py` and replace the `<connectionString>` placeholder value. The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type and the `genres` and `year` fields as the `filter` type in a MongoDB Vector Search index. The `plot_embedding_voyage_3_large` field contains embeddings created using Voyage AI's `voyage-3-large` embedding model. The index definition specifies `2048` vector dimensions and measures similarity using `dotProduct` function.

<Tab name="Basic Example">

Work with a runnable version of this example as a [Python notebook](https://github.com/mongodb/docs-notebooks/blob/main/manage-indexes/create-indexes-basic.ipynb).

The following index definition indexes only the vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search.

<Tab name="Filter Example">

Work with a runnable version of this example as a [Python notebook](https://github.com/mongodb/docs-notebooks/blob/main/manage-indexes/create-indexes-filter.ipynb).

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

To create a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh) v2.1.2 or later, perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.createSearchIndex()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.createSearchIndex/#mongodb-method-db.collection.createSearchIndex) method has the following syntax:

The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type. The `plot_embedding_voyage_3_large` field contains `2048` vector dimension embeddings created using Voyage AI's `voyage-3-large` embedding model. The index measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field for performing vector search.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

To create a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh) v2.1.2 or later, perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.createSearchIndex()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.createSearchIndex/#mongodb-method-db.collection.createSearchIndex) method has the following syntax:

The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type. The `plot_embedding_voyage_3_large` field contains `2048` vector dimension embeddings created using Voyage AI's `voyage-3-large` embedding model. The index measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field for performing vector search.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

To create a MongoDB Vector Search index for a collection using the [C#](https://www.mongodb.com/docs/drivers/csharp/current/fundamentals/indexes/#atlas-search-indexes) driver v3.1.0 or later, perform the following steps:

Create a file named `IndexService.cs`.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<documentType>`

</td>
<td>
Class that represents a document in the collection. To learn more, see [POCOs](https://www.mongodb.com/docs/drivers/csharp/current/serialization/poco/) in the C# driver documentation.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index. For this parameter, you can pass either a `FieldDefinition<TDocument>` object or a lambda expression.

</td>
</tr>
<tr>
<td>
`<vectorSimilarity>`

</td>
<td>
Vector similarity function, defined in the `VectorSimilarity` enum.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
</table>Copy and paste the following into the `IndexService.cs` and replace the `<connectionString>` placeholder value. The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type and the `genres` and `year` fields as the `filter` type in a MongoDB Vector Search index. The `plot_embedding_voyage_3_large` field contains embeddings created using Voyage AI's `voyage-3-large` embedding model. The index definition specifies `2048` vector dimensions and measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

To create a MongoDB Vector Search index for a collection using the [MongoDB Go driver](https://www.mongodb.com/docs/drivers/go/current/fundamentals/indexes/) v2.0 or later, perform the following steps:

The MongoDB Go driver supports programmatic MongoDB Vector Search index management starting in v1.16.0, but the preceding code shows the syntax for the v2.x driver.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

</td>
</tr>
</table>Copy and paste the following into the `create-index.go` file and replace the `<connectionString>` placeholder value. The following index definition indexes the `plot_embedding_voyage_3_large field contains embeddings created using Voyage AI's `voyage-3-large` embedding model. The index definition specifies `2048` vector dimensions and measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field (`plot_embedding_voyage_3_largego
package main

import (
	"context"
	"fmt"
	"log"
	"time"

"go.mongodb.org/mongo-driver/v2/bson"
	"go.mongodb.org/mongo-driver/v2/mongo"
	"go.mongodb.org/mongo-driver/v2/mongo/options"
)

func main() {
	ctx := context.Background()

// Replace the placeholder with your connection string
	const uri = "<connectionString>"

// Connect to your cluster
	clientOptions := options.Client().ApplyURI(uri)
	client, err := mongo.Connect(clientOptions)
	if err != nil {
		log.Fatalf("failed to connect to the server: %v", err)
	}
	defer func() { _ = client.Disconnect(ctx) }()

// Set the namespace
	coll := client.Database("sample_mflix").Collection("embedded_movies")

// Define the index details
	type vectorDefinitionField struct {
		Type          string `bson:"type"`
		Path          string `bson:"path"`
		NumDimensions int    `bson:"numDimensions"`
		Similarity    string `bson:"similarity"`
		Quantization  string `bson:"quantization"`
	}

type vectorDefinition struct {
		Fields []vectorDefinitionField `bson:"fields"`
	}

indexName := "vector_index"
	opts := options.SearchIndexes().SetName(indexName).SetType("vectorSearch")

indexModel := mongo.SearchIndexModel{
		Definition: vectorDefinition{
			Fields: []vectorDefinitionField{{
				Type:          "vector",
				Path:          "plot_embedding_voyage_3_large",
				NumDimensions: 2048,
				Similarity:    "dotProduct",
				Quantization:  "scalar"}},
		},
		Options: opts,
	}

// Create the index
	searchIndexName, err := coll.SearchIndexes().CreateOne(ctx, indexModel)
	if err != nil {
		log.Fatalf("failed to create the search index: %v", err)
	}
	log.Println("New search index named " + searchIndexName + " is building.")

// Await the creation of the index.
	log.Println("Polling to check if the index is ready. This may take up to a minute.")
	searchIndexes := coll.SearchIndexes()
	var doc bson.Raw
	for doc == nil {
		cursor, err := searchIndexes.List(ctx, options.SearchIndexes().SetName(searchIndexName))
		if err != nil {
			fmt.Errorf("failed to list search indexes: %w", err)
		}

if !cursor.Next(ctx) {
			break
		}

name := cursor.Current.Lookup("name").StringValue()
		queryable := cursor.Current.Lookup("queryable").Boolean()
		if name == searchIndexName && queryable {
			doc = cursor.Current
		} else {
			time.Sleep(5 * time.Second)
		}
	}

log.Println(searchIndexName + " is ready for querying.")
}

) for performing vector search against pre-filtered data.

To create a MongoDB Vector Search index for a collection using the [MongoDB Java driver](https://www.mongodb.com/docs/drivers/java/sync/current/fundamentals/indexes/) v5.2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

</td>
</tr>
</table>The following example index definitions:

- Index the `plot_embedding_voyage_3_large` field as the `vector` type and the `genres` and `year` fields as the `filter` type in a MongoDB Vector Search index.

- Specifies the `plot_embedding_voyage_3_large` field as the vector embeddings field, which contains embeddings created using Voyage AI's `voyage-3-large` embedding model.

- Specifies `2048` vector dimensions and measures similarity using `dotProduct` function.

<Tab name="Basic Example">

This index definition indexes only the vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search.

Copy and paste the following into the file you created, and replace the `<connectionString>` placeholder value.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

Copy and paste the following into the file you created, and replace the `<connectionString>` placeholder value.

From your IDE, run the file to create the index.

To create a MongoDB Vector Search index for a collection using the [MongoDB Node driver](https://www.mongodb.com/docs/drivers/node/current/fundamentals/indexes/) v6.6.0 or later, perform the following steps:

Create a file named `vector-index.js`.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

</td>
</tr>
</table>Copy and paste the following into the `vector-index.js` file and replace the `<connectionString>` placeholder value. The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type and the `genres` and `year` fields as the `filter` type in a MongoDB Vector Search index. The `plot_embedding_voyage_3_large` field contains embeddings created using Voyage AI's `voyage-3-large` embedding model. The index definition specifies `2048` vector dimensions and measures similarity using `dotProduct` function.

<Tab name="Basic Example">

The following index definition indexes only the vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

To create MongoDB Vector Search indexes for a collection using [PyMongo](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/indexes/atlas-search-index/) driver v4.7 or later, perform the following steps:

To learn more, see the [create_search_index()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.create_search_index) method.

To learn more, see the [create_search_indexes()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.create_search_indexes) method.

Create a file named `vector-index.py`.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

</td>
</tr>
</table>Copy and paste the following into the `vector-index.py` and replace the `<connectionString>` placeholder value. The following index definition indexes the `plot_embedding_voyage_3_large` field as the `vector` type and the `genres` and `year` fields as the `filter` type in a MongoDB Vector Search index. The `plot_embedding_voyage_3_large` field contains embeddings created using Voyage AI's `voyage-3-large` embedding model. The index definition specifies `2048` vector dimensions and measures similarity using `dotProduct` function.

<Tab name="Basic Example">

Work with a runnable version of this example as a [Python notebook](https://github.com/mongodb/docs-notebooks/blob/main/manage-indexes/create-indexes-basic.ipynb).

The following index definition indexes only the vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search.

<Tab name="Filter Example">

Work with a runnable version of this example as a [Python notebook](https://github.com/mongodb/docs-notebooks/blob/main/manage-indexes/create-indexes-filter.ipynb).

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The vector embeddings field (`plot_embedding_voyage_3_large`) for performing vector search against pre-filtered data.

To create a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

- Define the index in the `db.collection.createSearchIndex()` method

The [`db.collection.createSearchIndex()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.createSearchIndex/#mongodb-method-db.collection.createSearchIndex) method has the following syntax:

- Replace the following placeholder values in your index definition:

<tr>
  <td>
  `<collectionName>`

</td>
  <td>
  Name of the collection.

</td>
  </tr>
  <tr>
  <td>
  `<index-name>`

</td>
  <td>
  Name of the index.

</td>
  </tr>
  <tr>
  <td>
  `<field-to-index>`

</td>
  <td>
  Name of the field to index.

</td>
  </tr>
  <tr>
  <td>
  `<embedding-model>`

</td>
  <td>
  Voyage AI embedding model to use for generating embeddings.

</td>
  </tr>
  </table>

- Run the `db.collection.createSearchIndex()` method.

<Tab name="Basic Example">

The following index definition indexes the `fullplot` field as the `autoEmbed` type to enable Automated Embedding for that field. It specifies the `voyage-4` embedding model as the embedding model to use for generating embeddings for the `fullplot` field.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The `fullplot` field as the `autoEmbed` type to enable Automated Embedding for that field using the `voyage-4` embedding model.

To create a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

Create a file named `vector-index.js`.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

</td>
</tr>
<tr>
<td>
`<embeddingModel>`

</td>
<td>
Name of Voyage AI embedding model to use.

</td>
</tr>
</table>Copy and paste the following into the `vector-index.js` file and replace the `<connectionString>` placeholder value. The following index definition uses the `sample_mflix.movies` collection.

<Tab name="Basic Example">

The following index definition indexes the `fullplot` field as the `autoEmbed` type to enable Automated Embedding for that field. It specifies the `voyage-4` embedding model as the embedding model to use for generating embeddings for the `fullplot` field.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The `fullplot` field as the `autoEmbed` type to enable Automated Embedding for that field using the `voyage-4` embedding model.

To create a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see the [create_search_index()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.create_search_index) method.

To learn more, see the [create_search_indexes()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.create_search_indexes) method.

Create a file named `vector-index.py`.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name for your index.

</td>
</tr>
<tr>
<td>
`<embeddingModel>`

</td>
<td>
Voyage AI embedding model you want MongoDB Vector Search to use for automatically generating embeddings.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Name of field to index.

</td>
</tr>
</table>Copy and paste the following into the `vector-index.py` and replace the `<connectionString>` placeholder value.

<Tab name="Basic Example">

The following index definition indexes the `fullplot` field as the `autoEmbed` type to enable Automated Embedding for that field. It specifies the `voyage-4` embedding model as the embedding model to use for generating embeddings for the `fullplot` field.

<Tab name="Filter Example">

This index definition indexes the following fields:

- A string field (`genres`) and a numeric field (`year`) for pre-filtering the data.

- The `fullplot` field as the `autoEmbed` type to enable Automated Embedding for that field using the `voyage-4` embedding model.

You can view MongoDB Vector Search indexes for all collections from the Atlas UI, Atlas Administration API, Atlas CLI, [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), or a supported [MongoDB Driver](https://www.mongodb.com/docs/drivers/).

You need the [`Project Search Index Editor`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/reference/user-roles/#mongodb-authrole-Project-Search-Index-Editor) or higher role to view MongoDB Vector Search indexes.

retrieveMongoDB Vector SearchYou can use the [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh) command or driver helper methods to retrieve
MongoDB Vector Search indexes on all Atlas cluster tiers. For a list of supported driver versions, see [Supported Clients](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-index-supported-drivers).

You need [`readWrite`](https://www.mongodb.com/docs/manual/reference/built-in-roles/#mongodb-authrole-readWrite) or higher role to view MongoDB Vector Search indexes.

You can go the MongoDB Search page from the Search & Vector Search option, or the Data Explorer.

<Tab name="Search & Vector Search">

- If it's not already displayed, select the organization that contains your project from the  Organizations menu in the navigation bar.

- If it's not already displayed, select your project from the Projects menu in the navigation bar.

- In the sidebar, click Search & Vector Search under the Database heading.

- If your project has multiple clusters, select the cluster you want to use from the Select cluster dropdown, then click Go to Atlas Search.

The [Search & Vector Search](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fclusters%2FatlasSearch%2F%3Ccluster%3E) page displays.

<Tab name="Data Explorer">

- If it's not already displayed, select the organization that contains your project from the  Organizations menu in the navigation bar.

- If it's not already displayed, select your project from the Projects menu in the navigation bar.

- In the sidebar, click Data Explorer under the Database heading.

- Expand the database and select the collection.

- Click the Indexes tab for the collection.

The [Atlas Search](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fclusters%2FatlasSearch%2F%3Ccluster%3E%3Fdatabase%3Dsample_mflix%26collectionName%3Dusers) page displays.

The page displays the following details for the indexes on the page:

</td>
<td>
Label that identifies the index.

</td>
</tr>
<tr>
<td>
Index Type

</td>
<td>
Label that indicates a MongoDB Search or MongoDB Vector Search index. Values include:

- `search` for MongoDB Search indexes.

- `vectorSearch` for MongoDB Vector Search indexes.

</td>
</tr>
<tr>
<td>
Index Fields

</td>
<td>
List that contains the fields that this index indexes.

</td>
</tr>
<tr>
<td>
Status

</td>
<td>
Current state of the index on the primary node of the cluster. For valid values, see [Index Status](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-node-status-ref).

</td>
</tr>
<tr>
<td>
Size

</td>
<td>
Size of the index on the primary node.

</td>
</tr>
<tr>
<td>
Documents

</td>
<td>
Number of indexed documents out of the total number of documents in the collection.

</td>
</tr>
<tr>
<td>
Required Memory

</td>
<td>
Approximate amount of memory required to run vector search queries.

</td>
</tr>
<tr>
<td>
Actions

</td>
<td>
Actions that you can take on the index. You can:

- [Edit a MongoDB Vector Search Index](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-edit-index)

- [Delete a MongoDB Vector Search Index](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-delete-index)

You can't run queries in the Search Tester UI against indexes of the `vectorSearch` type. If you click the Query button, MongoDB Vector Search displays a sample [`$vectorSearch`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#mongodb-pipeline-pipe.-vectorSearch) that you can copy, modify, and run in Atlas UI and using other [supported clients](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-stage/#std-label-vectorSearch-agg-pipeline-clients).

</td>
</tr>
</table>To retrieve all the MongoDB Vector Search indexes for a collection using the Atlas Administration API, send a `GET` request to the MongoDB Search
`indexes` endpoint with the name of the database and collection.

To learn more about the syntax and parameters for the endpoint, see [Return All MongoDB Search Indexes for One Collection](https://www.mongodb.com/docs/api/doc/atlas-admin-api-v2/operation/operation-listgroupclustersearchindexes).

To retrieve one MongoDB Vector Search index for a collection using the Atlas Administration API, send a `GET` request to the MongoDB Search
`indexes` endpoint with either the unique ID or name of the index (line 4) to retrieve.

To learn more about the syntax and parameters for the endpoint, [Get One By Name](https://www.mongodb.com/docs/api/doc/atlas-admin-api-v2/operation/operation-getgroupclustersearchindexbyname) and [Get One By ID](https://www.mongodb.com/docs/api/doc/atlas-admin-api-v2/operation/operation-getgroupclustersearchindex).

To return MongoDB Vector Search indexes for a collection using Atlas CLI, perform the following steps:

<tr>
<td>
`clusterName`

</td>
<td>
The name of the cluster.

</td>
</tr>
<tr>
<td>
`db`

</td>
<td>
The name of the database on the cluster that contains your indexed collection.

</td>
</tr>
<tr>
<td>
`collection`

</td>
<td>
The name of the indexed collection in the database.

</td>
</tr>
<tr>
<td>
`projectId`

</td>
<td>
The unique identifier of the project.

In the command, replace the following placeholder values:

- `cluster-name` - the name of the cluster that contains the indexed collection.

- `db-name` - the name of the database that contains the collection for which you want to retrieve the indexes.

- `collection-name` - the name of the collection for which you want to retrieve the indexes.

To learn more about the command syntax and parameters, see the Atlas CLI documentation for the [atlas clusters search indexes list](https://www.mongodb.com/docs/atlas/cli/current/command/atlas-clusters-search-indexes-list/) command.

To view a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.getSearchIndexes()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.getSearchIndexes/#mongodb-method-db.collection.getSearchIndexes) method has the following syntax:

To view MongoDB Vector Search indexes for a collection using [C#](https://www.mongodb.com/docs/drivers/csharp/current/fundamentals/indexes/#list-search-indexes) driver 3.1.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The name of the database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The name of the collection.

To view a MongoDB Vector Search index for a collection using [MongoDB Go driver](https://www.mongodb.com/docs/drivers/go/current/fundamentals/indexes/) v2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to retrieve the indexes.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index if you want to retrieve a specific index. To return all indexes on the collection, omit this value and remove the call to the `SetName()` method when creating the Search index options.

To view a MongoDB Vector Search index for a collection using the [MongoDB Java driver](https://www.mongodb.com/docs/drivers/java/sync/current/fundamentals/indexes/) v5.2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to retrieve the indexes.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index if you want to retrieve a specific index. To return all indexes on the collection, omit this value.

From your IDE, run the file to retrieve the specified index.

To view a MongoDB Vector Search index for a collection using [MongoDB Node driver](https://www.mongodb.com/docs/drivers/node/current/fundamentals/indexes/) v6.6.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to retrieve the indexes.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index if you want to retrieve a specific index. To return all indexes on the collection, omit this value.

Work with a runnable version of this example as a [Python notebook](https://github.com/mongodb/docs-notebooks/blob/main/manage-indexes/view-indexes.ipynb).

To view MongoDB Vector Search indexes for a collection using [PyMongo](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/indexes/atlas-search-index/) driver v4.7 or later, perform the following steps:

To learn more, see the [list_search_indexes()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.list_search_indexes) method.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The name of the database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The name of the collection.

To view a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.getSearchIndexes()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.getSearchIndexes/#mongodb-method-db.collection.getSearchIndexes) method has the following syntax:

To view MongoDB Vector Search indexes for a collection using [C#](https://www.mongodb.com/docs/drivers/csharp/current/fundamentals/indexes/#list-search-indexes) driver 3.1.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The name of the database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The name of the collection.

To view a MongoDB Vector Search index for a collection using [MongoDB Go driver](https://www.mongodb.com/docs/drivers/go/current/fundamentals/indexes/) v2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to retrieve the indexes.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index if you want to retrieve a specific index. To return all indexes on the collection, omit this value and remove the call to the `SetName()` method when creating the Search index options.

To view a MongoDB Vector Search index for a collection using the [MongoDB Java driver](https://www.mongodb.com/docs/drivers/java/sync/current/fundamentals/indexes/) v5.2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to retrieve the indexes.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index if you want to retrieve a specific index. To return all indexes on the collection, omit this value.

From your IDE, run the file to retrieve the specified index.

To view a MongoDB Vector Search index for a collection using [MongoDB Node driver](https://www.mongodb.com/docs/drivers/node/current/fundamentals/indexes/) v6.6.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to retrieve the indexes.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index if you want to retrieve a specific index. To return all indexes on the collection, omit this value.

Work with a runnable version of this example as a [Python notebook](https://github.com/mongodb/docs-notebooks/blob/main/manage-indexes/view-indexes.ipynb).

To view MongoDB Vector Search indexes for a collection using [PyMongo](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/indexes/atlas-search-index/) driver v4.7 or later, perform the following steps:

To learn more, see the [list_search_indexes()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.list_search_indexes) method.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The name of the database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The name of the collection.

To view a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.getSearchIndexes()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.getSearchIndexes/#mongodb-method-db.collection.getSearchIndexes) method has the following syntax:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to retrieve the indexes.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index if you want to retrieve a specific index. To return all indexes on the collection, omit this value.

<tr>
<td>
`<connectionString>`

</td>
<td>
Your cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The name of the database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The name of the collection.

You can change the [index definition](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-vector-search) of an existing MongoDB Vector Search index from the Atlas UI, Atlas Administration API, Atlas CLI, [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), or a supported [MongoDB Driver](https://www.mongodb.com/docs/drivers/). You can't rename an index or change the index type. If you need to change an index name or type, you must create a new index and delete the old one.

After you edit an index, MongoDB Vector Search rebuilds it. While the index rebuilds, you can continue to run vector search queries by using the old index definition. When the index finishes rebuilding, the old index is automatically replaced. This process is similar to MongoDB Search indexes. To learn more, see [Creating and Updating a MongoDB Search Index](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-search/performance/index-performance/#std-label-index-create-and-update).

You need the [`Project Data Access Admin`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/reference/user-roles/#mongodb-authrole-Project-Data-Access-Admin) or higher role to edit MongoDB Vector Search indexes.

You need [`readWrite`](https://www.mongodb.com/docs/manual/reference/built-in-roles/#mongodb-authrole-readWrite) or higher role to edit MongoDB Vector Search indexes.

You can go the MongoDB Search page from the Search & Vector Search option, or the Data Explorer.

<Tab name="Search & Vector Search">

- If it's not already displayed, select the organization that contains your project from the  Organizations menu in the navigation bar.

- If it's not already displayed, select your project from the Projects menu in the navigation bar.

- In the sidebar, click Search & Vector Search under the Database heading.

- If your project has multiple clusters, select the cluster you want to use from the Select cluster dropdown, then click Go to Atlas Search.

The [Search & Vector Search](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fclusters%2FatlasSearch%2F%3Ccluster%3E) page displays.

<Tab name="Data Explorer">

- If it's not already displayed, select the organization that contains your project from the  Organizations menu in the navigation bar.

- If it's not already displayed, select your project from the Projects menu in the navigation bar.

- In the sidebar, click Data Explorer under the Database heading.

- Expand the database and select the collection.

- Click the Indexes tab for the collection.

The [Atlas Search](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fclusters%2FatlasSearch%2F%3Ccluster%3E%3Fdatabase%3Dsample_mflix%26collectionName%3Dusers) page displays.

- Locate the `vectorSearch` type index to edit.

- Click  from the Actions column for that index.

- Select either Edit With Visual Editor for a guided experience or Edit With JSON Editor to edit the raw index definition.

- Review the current configuration settings and edit them as needed.

To learn more about the fields in a MongoDB Vector Search index, see [How to Index Fields for Vector Search](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-types-vector-search).

- Click Save to apply the changes.

The index's status changes from Active to Building. In this state, you can continue to use the old index because MongoDB Vector Search does not delete the old index until the updated index is ready for use. Once the status returns to Active, the modified index is ready to use.

To edit a MongoDB Vector Search index for a collection using the Atlas Administration API, send a `PATCH` request to the MongoDB Search
`indexes` endpoint with either the unique ID or name of the index (line 4) to edit.

To learn more about the syntax and parameters for the endpoints, see [Update One By Name](https://www.mongodb.com/docs/api/doc/atlas-admin-api-v2/operation/operation-updategroupclustersearchindexbyname) and [Update One By ID](https://www.mongodb.com/docs/api/doc/atlas-admin-api-v2/operation/operation-updategroupclustersearchindex).

To edit a MongoDB Vector Search index for a collection using Atlas CLI, perform the following steps:

Your index definition should resemble the following format:

<tr>
<td>
`<name-of-database>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<name-of-collection>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<index-name>`

</td>
<td>
Name of your index. If you omit the index name, MongoDB Vector Search names the index `vector_index`.

</td>
</tr>
<tr>
<td>
`<number-of-dimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<field-to-index>`

</td>
<td>
Vector and filter fields to index.

In the command, replace the following placeholder values:

- `cluster_name` - the name of the cluster that contains the collection for which you want to update the index.

- `vector_index` - the name of the JSON (Javascript Object Notation) file that contains the modified index definition for the MongoDB Vector Search index.

To learn more about the command syntax and parameters, see the Atlas CLI documentation for the [atlas clusters search indexes update](https://www.mongodb.com/docs/atlas/cli/current/command/atlas-clusters-search-indexes-update/) command.

To edit a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.updateSearchIndex()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.updateSearchIndex/#mongodb-method-db.collection.updateSearchIndex) method has the following syntax:

To update a MongoDB Vector Search index for a collection using the [C#](https://www.mongodb.com/docs/drivers/csharp/current/fundamentals/indexes/#update-a-search-index) driver 3.1.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Bame of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

To update a MongoDB Vector Search index for a collection using the [MongoDB Go driver](https://www.mongodb.com/docs/drivers/go/current/fundamentals/indexes/) v2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

To edit a MongoDB Vector Search index for a collection using the [MongoDB Java driver](https://www.mongodb.com/docs/drivers/java/sync/current/fundamentals/indexes/) v5.2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

From your IDE, run the file to update the index with your changes.

To update a MongoDB Vector Search index for a collection using the [MongoDB Node driver](https://www.mongodb.com/docs/drivers/node/current/fundamentals/indexes/) v6.6.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

Work with a runnable version of this example as a [Python notebook](https://github.com/mongodb/docs-notebooks/blob/main/manage-indexes/edit-indexes.ipynb).

To update a MongoDB Vector Search index for a collection using the [PyMongo](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/indexes/atlas-search-index/) driver v4.7 or later, perform the following steps:

To learn more, see the [update_search_index()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.update_search_index) method.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Bame of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

To edit a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.updateSearchIndex()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.updateSearchIndex/#mongodb-method-db.collection.updateSearchIndex) method has the following syntax:

To update a MongoDB Vector Search index for a collection using the [C#](https://www.mongodb.com/docs/drivers/csharp/current/fundamentals/indexes/#update-a-search-index) driver 3.1.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Bame of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

To update a MongoDB Vector Search index for a collection using the [MongoDB Go driver](https://www.mongodb.com/docs/drivers/go/current/fundamentals/indexes/) v2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

To edit a MongoDB Vector Search index for a collection using the [MongoDB Java driver](https://www.mongodb.com/docs/drivers/java/sync/current/fundamentals/indexes/) v5.2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

From your IDE, run the file to update the index with your changes.

To update a MongoDB Vector Search index for a collection using the [MongoDB Node driver](https://www.mongodb.com/docs/drivers/node/current/fundamentals/indexes/) v6.6.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

Work with a runnable version of this example as a [Python notebook](https://github.com/mongodb/docs-notebooks/blob/main/manage-indexes/edit-indexes.ipynb).

To update a MongoDB Vector Search index for a collection using the [PyMongo](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/indexes/atlas-search-index/) driver v4.7 or later, perform the following steps:

To learn more, see the [update_search_index()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.update_search_index) method.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Bame of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<numberOfDimensions>`

</td>
<td>
Number of vector dimensions that MongoDB Vector Search enforces at index-time and query-time.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Vector and filter fields to index.

To view a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.updateSearchIndex()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.updateSearchIndex/#mongodb-method-db.collection.updateSearchIndex) method has the following syntax:

You can add text fields to index as the `autoEmbed` type, but you can't replace or delete existing `autoEmbed` type fields in the index definition.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index. If you omit the index name, defaults to `vector_index`.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Text and filter fields to index.

You can add text fields to index as the `autoEmbed` type, but you can't replace or delete existing `autoEmbed` type fields in the index definition.

</td>
</tr>
<tr>
<td>
`<modelName>`

</td>
<td>
Name of the embedding model to use to generate the embeddings.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
Database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
Name of the collection for which you want to update the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
Name of your index that you want to update.

</td>
</tr>
<tr>
<td>
`<modelName>`

</td>
<td>
Name of the embedding model to use to generate embeddings.

</td>
</tr>
<tr>
<td>
`<fieldToIndex>`

</td>
<td>
Name of field to index.

You can add text fields to index as the `autoEmbed` type, but you can't replace or delete existing `autoEmbed` type fields in the index definition.

You can delete a MongoDB Vector Search index at any time from the Atlas UI, Atlas Administration API, Atlas CLI, [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), or a supported [MongoDB Driver](https://www.mongodb.com/docs/drivers/).

You must have the [`Project Search Index Editor`](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/reference/user-roles/#mongodb-authrole-Project-Search-Index-Editor) or higher role to delete a MongoDB Vector Search index.

deleteMongoDB Vector SearchYou can use the [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh) command or driver helper methods to delete
MongoDB Vector Search indexes on all Atlas cluster tiers. For a list of supported driver versions, see [Supported Clients](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/atlas-vector-search/vector-search-type/#std-label-avs-index-supported-drivers).

You need [`readWrite`](https://www.mongodb.com/docs/manual/reference/built-in-roles/#mongodb-authrole-readWrite) or higher role to delete MongoDB Vector Search indexes.

You can go the MongoDB Search page from the Search & Vector Search option, or the Data Explorer.

<Tab name="Search & Vector Search">

- If it's not already displayed, select the organization that contains your project from the  Organizations menu in the navigation bar.

- If it's not already displayed, select your project from the Projects menu in the navigation bar.

- In the sidebar, click Search & Vector Search under the Database heading.

- If your project has multiple clusters, select the cluster you want to use from the Select cluster dropdown, then click Go to Atlas Search.

The [Search & Vector Search](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fclusters%2FatlasSearch%2F%3Ccluster%3E) page displays.

<Tab name="Data Explorer">

- If it's not already displayed, select the organization that contains your project from the  Organizations menu in the navigation bar.

- If it's not already displayed, select your project from the Projects menu in the navigation bar.

- In the sidebar, click Data Explorer under the Database heading.

- Expand the database and select the collection.

- Click the Indexes tab for the collection.

The [Atlas Search](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fclusters%2FatlasSearch%2F%3Ccluster%3E%3Fdatabase%3Dsample_mflix%26collectionName%3Dusers) page displays.

- Locate the `vectorSearch` type index to delete.

- Click Delete Index from the Actions dropdown for that index.

- Click Drop Index in the confirmation window.

To delete a MongoDB Vector Search index for a collection using the Atlas Administration API, send a `DELETE` request to the MongoDB Search
`indexes` endpoint with either the unique ID or the name of the index to delete.

To learn more about the syntax and parameters for the endpoint, see [Remove One Search Index By Name](https://www.mongodb.com/docs/api/doc/atlas-admin-api-v2/operation/operation-deletegroupclustersearchindexbyname) and [Remove One Search Index By ID](https://www.mongodb.com/docs/api/doc/atlas-admin-api-v2/operation/operation-deletegroupclustersearchindex).

To delete a MongoDB Vector Search index for a collection using Atlas CLI, perform the following steps:

<tr>
<td>
`<indexId>`

</td>
<td>
The unique identifier of the index to delete.

</td>
</tr>
<tr>
<td>
`<clusterName>`

</td>
<td>
The name of the cluster.

</td>
</tr>
<tr>
<td>
`<projectId>`

</td>
<td>
The unique identifier of the project.

In the command, replace the `indexId` placeholder value with the unique identifier of the index to delete.

To learn more about the command syntax and parameters, see the Atlas CLI documentation for the [atlas clusters search indexes delete](https://www.mongodb.com/docs/atlas/cli/current/command/atlas-clusters-search-indexes-delete/) command.

To delete a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.dropSearchIndex()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.dropSearchIndex/#mongodb-method-db.collection.dropSearchIndex) method has the following syntax:

To delete a MongoDB Vector Search index for a collection using [C#](https://www.mongodb.com/docs/drivers/csharp/current/fundamentals/indexes/#drop-a-search-index) driver 3.1.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The name of the database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The name of the collection.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of the index to delete.

To delete a MongoDB Vector Search index for a collection using [MongoDB Go driver](https://www.mongodb.com/docs/drivers/go/current/fundamentals/indexes/) v2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index. If you omit the index name, defaults to `vector_index`.

To delete a MongoDB Vector Search index for a collection using the [MongoDB Java driver](https://www.mongodb.com/docs/drivers/java/sync/current/fundamentals/indexes/) v5.2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index. If you omit the index name, defaults to `vector_index`.

From your IDE, run the file to delete the specified index.

To delete a MongoDB Vector Search index for a collection using [MongoDB Node driver](https://www.mongodb.com/docs/drivers/node/current/fundamentals/indexes/) v6.6.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index. If you omit the index name, defaults to `vector_index`.

Work with a runnable version of this example as a [Python notebook](https://github.com/mongodb/docs-notebooks/blob/main/manage-indexes/delete-indexes.ipynb).

To delete a MongoDB Vector Search index for a collection using [PyMongo](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/indexes/atlas-search-index/) driver v4.7 or later, perform the following steps:

To learn more, see the [drop_search_index()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.drop_search_index) method.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The name of the database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The name of the collection.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of the index to delete.

To delete a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.dropSearchIndex()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.dropSearchIndex/#mongodb-method-db.collection.dropSearchIndex) method has the following syntax:

To delete a MongoDB Vector Search index for a collection using [C#](https://www.mongodb.com/docs/drivers/csharp/current/fundamentals/indexes/#drop-a-search-index) driver 3.1.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The name of the database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The name of the collection.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of the index to delete.

To delete a MongoDB Vector Search index for a collection using [MongoDB Go driver](https://www.mongodb.com/docs/drivers/go/current/fundamentals/indexes/) v2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index. If you omit the index name, defaults to `vector_index`.

To delete a MongoDB Vector Search index for a collection using the [MongoDB Java driver](https://www.mongodb.com/docs/drivers/java/sync/current/fundamentals/indexes/) v5.2.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index. If you omit the index name, defaults to `vector_index`.

From your IDE, run the file to delete the specified index.

To delete a MongoDB Vector Search index for a collection using [MongoDB Node driver](https://www.mongodb.com/docs/drivers/node/current/fundamentals/indexes/) v6.6.0 or later, perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index. If you omit the index name, defaults to `vector_index`.

Work with a runnable version of this example as a [Python notebook](https://github.com/mongodb/docs-notebooks/blob/main/manage-indexes/delete-indexes.ipynb).

To delete a MongoDB Vector Search index for a collection using [PyMongo](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/indexes/atlas-search-index/) driver v4.7 or later, perform the following steps:

To learn more, see the [drop_search_index()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.drop_search_index) method.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The name of the database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The name of the collection.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of the index to delete.

To delete a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see [Connect to a Cluster via mongosh](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/mongo-shell-connection/#std-label-connect-mongo-shell).

The [`db.collection.dropSearchIndex()`](https://www.mongodb.com/docs/manual/reference/method/db.collection.dropSearchIndex/#mongodb-method-db.collection.dropSearchIndex) method has the following syntax:

To delete a MongoDB Vector Search index for a collection using [MongoDB Node driver](https://www.mongodb.com/docs/drivers/node/current/fundamentals/indexes/), perform the following steps:

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The database that contains the collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The collection for which you want to create the index.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of your index. If you omit the index name, defaults to `vector_index`.

To delete a MongoDB Vector Search index for a collection using [PyMongo](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/indexes/atlas-search-index/) driver, perform the following steps:

To delete a MongoDB Vector Search index for a collection using [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), perform the following steps:

To learn more, see the [drop_search_index()](https://pymongo.readthedocs.io/en/4.7.1/api/pymongo/collection.html#pymongo.collection.Collection.drop_search_index) method.

<tr>
<td>
`<connectionString>`

</td>
<td>
Cluster connection string. To learn more, see [Connect to a Cluster via Drivers](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/driver-connection/#std-label-connect-via-driver).

</td>
</tr>
<tr>
<td>
`<databaseName>`

</td>
<td>
The name of the database that contains the collection.

</td>
</tr>
<tr>
<td>
`<collectionName>`

</td>
<td>
The name of the collection.

</td>
</tr>
<tr>
<td>
`<indexName>`

</td>
<td>
The name of the index to delete.

When you create the MongoDB Vector Search index, the Status column shows the current state of the index on the primary node of the cluster. Click the View status details link below the status to view the state of the index on all the nodes of the cluster.

When the Status column reads Active, the index is ready to use. In other states, queries against the index may return incomplete results.

<table>
<tr>
<th id="Status">
Status

</th>
<th id="Description">
Description

</th>
</tr>
<tr>
<td headers="Status">
Not Started

</td>
<td headers="Description">
Atlas has not yet started building the index.

</td>
</tr>
<tr>
<td headers="Status">
Initial Sync

</td>
<td headers="Description">
Atlas is building the index or re-building the index after an edit. When the index is in this state:

- For a new index, MongoDB Vector Search doesn't serve queries until the index build is complete.

- For an existing index, you can continue to use the old index for existing and new queries until the index rebuild is complete.

</td>
</tr>
<tr>
<td headers="Status">
Active

</td>
<td headers="Description">
Index is ready to use.

</td>
</tr>
<tr>
<td headers="Status">
Recovering

</td>
<td headers="Description">
Replication encountered an error. This state commonly occurs when the current replication point is no longer available on the [`mongod`](https://www.mongodb.com/docs/manual/reference/program/mongod/#mongodb-binary-bin.mongod) oplog. You can still query the existing index until it updates and its status changes to Active. Use the error in the View status details modal window to troubleshoot the issue. To learn more, see [Fix Issues](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/reference/alert-resolutions/atlas-search-alerts/#std-label-atlas-search-alerts).

</td>
</tr>
<tr>
<td headers="Status">
Failed

</td>
<td headers="Description">
Atlas could not build the index. Use the error in the View status details modal window to troubleshoot the issue. To learn more, see [Fix Issues](https://mongodbcom-cdn.staging.corp.mongodb.com/docs/atlas/reference/alert-resolutions/atlas-search-alerts/#std-label-atlas-search-alerts).

</td>
</tr>
<tr>
<td headers="Status">
Delete in Progress

</td>
<td headers="Description">
Atlas is deleting the index from the cluster nodes.

</td>
</tr>
</table>While Atlas builds the index and after the build completes, the Documents column shows the percentage and number of documents indexed. The column also shows the total number of documents in the collection.

**Examples:**

Example 1 (unknown):
```unknown
console.log("Polling to check if the index is ready. This may take up to a minute.")
let isQueryable = false;
while (!isQueryable) {
  const cursor = collection.listSearchIndexes();
  for await (const index of cursor) {
    if (index.name === result) {
      if (index.queryable) {
        console.log(`${result} is ready for querying.`);
        isQueryable = true;
      } else {
        await new Promise(resolve => setTimeout(resolve, 5000));
      }
    }
  }
}
```

Example 2 (unknown):
```unknown
console.log("Polling to check if the index is ready. This may take up to a minute.")
let isQueryable = false;
while (!isQueryable) {
  const cursor = collection.listSearchIndexes();
  for await (const index of cursor) {
    if (index.name === result) {
      if (index.queryable) {
        console.log(`${result} is ready for querying.`);
        isQueryable = true;
      } else {
        await new Promise(resolve => setTimeout(resolve, 5000));
      }
    }
  }
}
```

Example 3 (unknown):
```unknown
console.log("Polling to check if the index is ready. This may take up to a minute.")
let isQueryable = false;
while (!isQueryable) {
  const cursor = collection.listSearchIndexes();
  for await (const index of cursor) {
    if (index.name === result) {
      if (index.queryable) {
        console.log(`${result} is ready for querying.`);
        isQueryable = true;
      } else {
        await new Promise(resolve => setTimeout(resolve, 5000));
      }
    }
  }
}
```

Example 4 (javascript):
```javascript
{
  "fields":[
    {
      "type": "vector",
      "path": "<field-to-index>",
      "numDimensions": <number-of-dimensions>,
      "similarity": "euclidean | cosine | dotProduct",
      "quantization": "none | scalar | binary",
      "hnswOptions": {
        "maxEdges": <number-of-connected-neighbors>,
        "numEdgeCandidates": <number-of-nearest-neighbors>
      }
    },
    {
      "type": "filter",
      "path": "<field-to-index>"
    },
    ...
  ]
}
```

---
