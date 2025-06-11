---
pagination_next: null
pagination_prev: null
---

# Entity matching

<!--What are entity matching?  Generic overview information-->

Use **entity matching** to contextualize your data with machine learning (ML) and rules engines, and then let domain experts validate and fine-tune the results.

Different sources of industrial data can use different naming standards when they refer to the same **entity**. With CDF, you can match all entities like time series, files, and sequences, along with 3d from different source systems to assets.

## About entity matching

[Assets](assets) in CDF connect related data from different sources, such as time series, files, sequences, and events. The data often use different naming conventions, even when they refer to the same **entity**. Entity matching applies artificial intelligence techniques to automatically match the different resources according to name, description, etc.

:::info TIP
See the [entity matching API documentation](/api#tag/Entity-matching) for more information on working with relationships.
:::

## Example data

<!--create dummy data for entity matching  -->

This tutorial uses a small example data set to show how to do entity matching. The data is in Python format, and you can convert it to other formats that fits your programming language.

```python
sources = [
    {"id":0, "name" : "KKL_21AA1019CA.PV", "description": "correct"},
    {"id":1, "name" : "KKL_13FV1234BU.VW", "description": "ok"}
]
targets = [
    {"id":0, "name" : "21AA1019CA", "description": "correct"},
    {"id":1, "name" : "21AA1019CA", "description": "wrong"},
    {"id":2, "name" : "13FV1234BU"},
    {"id":3, "name" : "13FV1234BU", "description": "ok"}
]
true_matches =  [{"sourceId": 0,"targetId": 0}]
```

## Fit a supervised ML model and predict for the same data

<!--Fit a supervised ml-model and predict for the same data  -->

The supervised model calculates one or more similarity measures between match-to and match-from items. Then it uses these calculated similarity measures as features and fits a classification model using the labeled data.

Note that a set of candidate matches is selected before calculating the similarity measures and training a model. To consider a pair of match-to and match-from items a candidate, the items should have at least one token in common. Only the candidate match-from and match-to combinations are used in the training. This is done to reduce computing time - calculating similarity measures for all possible combinations can be extremely heavy (10.000 time series and 30.000 assets -> 300.000.000 combinations).

### When to use a supervised ML model?

<!--When to use a supervised ml-model?  -->

A supervised ML model is applicable when you have a lot of labeled data items. The more labeled data you have, the better results you can achieve. Make sure not to apply a supervised model if you have fewer than 500 labeled data items.

### Create a model

```http
POST /api/v1/projects/publicdata/context/entitymatching
Content-Type: application/json

{
   "sources":sources,
   "targets":targets,
   "trueMatches":true_matches,
}
```

### Predict

When `predict` is called without any data, predictions are on the training data.

`num_matches` determines the number of matches to return for each `matchFrom` item. The default is 1.

```http
POST /api/v1/projects/publicdata/context/entitymatching/predict
Content-Type: application/json

{
  "id": modelId,
  "numMatches": 3,
  "scoreThreshold": 0.7
}
```

### Get matching results

Replace `job_id` in the following GET body with the `jobId` returned by the response above.

```http
GET /api/v1/projects/publicdata/context/entitymatching/jobs/<job_id>
Content-Type: application/json
```

**Example output:**

```json
[
  {
    "matchFrom": {
      "description": "correct",
      "id": 0,
      "name": "KKL_21AA1019CA.PV"
    },
    "matches": [
      {
        "matchTo": { "description": "correct", "id": 0, "name": "21AA1019CA" },
        "score": 0.35,
        "target": { "description": "correct", "id": 0, "name": "21AA1019CA" }
      },
      {
        "matchTo": { "description": "wrong", "id": 1, "name": "21AA1019CA" },
        "score": 0.35,
        "target": { "description": "wrong", "id": 1, "name": "21AA1019CA" }
      }
    ],
    "source": { "description": "correct", "id": 0, "name": "KKL_21AA1019CA.PV" }
  },
  {
    "matchFrom": { "description": "ok", "id": 1, "name": "KKL_13FV1234BU.VW" },
    "matches": [
      {
        "matchTo": { "id": 2, "name": "13FV1234BU" },
        "score": 0.35,
        "target": { "id": 2, "name": "13FV1234BU" }
      },
      {
        "matchTo": { "description": "ok", "id": 3, "name": "13FV1234BU" },
        "score": 0.35,
        "target": { "description": "ok", "id": 3, "name": "13FV1234BU" }
      }
    ],
    "source": { "description": "ok", "id": 1, "name": "KKL_13FV1234BU.VW" }
  }
]
```

:::info note
For both `matchFrom` items, the two matches have an equal score, and the model isn't able to distinguish between the correct and incorrect match. Also, the scores are relatively low. When the data set is small, [unsupervised](#fit-unsupervised-model) learning makes more sense.
:::

### Refit

Refit lets you retrain a model, using the same parameters, with extra labels/true-matches. The new `true_matches` (1,3) are added to the `true_matches` list from the original model.

To fit a model using only the (1,3) label. A new model must be trained using fit.

```python
true_matches=[{"sourceId": 0,"targetId": 3}]
```

```http
POST /api/v1/projects/publicdata/context/entitymatching/refit
Content-Type: application/json

{
   "id": modelId
   "trueMatches":true_matches,
}
```

```http
POST /api/v1/projects/publicdata/context/entitymatching/predict
Content-Type: application/json

{
  "id": modelId,
  "numMatches": 3,
  "scoreThreshold": 0.7
}
```

```http
GET /api/v1/projects/publicdata/context/entitymatching/jobs/<job_id>
Content-Type: application/json
```

:::info note
In this example, the results are the same. The new `true-match` follows the exact same pattern as the original.
:::

## Fit an unsupervised ML model

If there are no `true_matches` included in the `fit` call, an unsupervised model is applied.

As with supervised models, candidates are selected and similarity measures between the candidates are calculated. Instead of training a classification model, the average of the similarity measures is calculated and returned as the score.

### When to use an unsupervised ML model?

Use an unsupervised ML model when there are no or few true matches (labeled data).

### Create a model

```http
POST /api/v1/projects/publicdata/context/entitymatching/
Content-Type: application/json

{
   "sources":sources,
   "targets":targets,
   "name":"simple_model_1",
   "description":"Simple model 1",
   "featureType":"simple",
   "classifier":"randomforest"
}
```

**Example response:**

```json
{
  "classifier": "randomforest",
  "createdTime": 1606151476539,
  "description": "Simple model 1",
  "externalId": "None",
  "featureType": "simple",
  "id": 7895111848480381,
  "ignoreMissingFields": True,
  "matchFields": [
    {
      "source": "name",
      "target": "name"
    },
    {
      "source": "description",
      "target": "description"
    }
  ],
  "name": "simple_model_1",
  "startTime": "None",
  "status": "Queued",
  "statusTime": 1606151476539
}
```

### Create a matching job

In the following POST body, replace `id` with the `id` returned by the response above. You can set maximum matching items and threshold to filter valid matching.

```http
POST /api/v1/projects/publicdata/context/entitymatching/predict
Content-Type: application/json

{
  "id": 7895111848480381,
  "numMatches": 3,
  "scoreThreshold": 0.7
}
```

**Example response:**

```python
{
   "createdTime":1606151691970,
   "jobId":6147120367590349,
   "startTime":"None",
   "status":"Queued",
   "statusTime":1606151691970
}
```

### Get matching results

In the following GET body, replace `job_id` with the `jobId` return by the response above.

```http
GET /api/v1/projects/publicdata/context/entitymatching/jobs/<job_id>
Content-Type: application/json
```

**Example response:**

```python
{'createdTime': 1606216545060,
 'items': [{'matchFrom': {'description': 'correct',
    'id': 0,
    'name': 'KKL_21AA1019CA.PV'},
   'matches': [{'matchTo': {'description': 'correct',
      'id': 0,
      'name': '21AA1019CA'},
     'score': 1.0,
     'target': {'description': 'correct', 'id': 0, 'name': '21AA1019CA'}},
    {'matchTo': {'description': 'wrong', 'id': 1, 'name': '21AA1019CA'},
     'score': 1.0,
     'target': {'description': 'wrong', 'id': 1, 'name': '21AA1019CA'}}],
   'source': {'description': 'correct', 'id': 0, 'name': 'KKL_21AA1019CA.PV'}},
  {'matchFrom': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'},
   'matches': [{'matchTo': {'id': 2, 'name': '13FV1234BU'},
     'score': 1.0,
     'target': {'id': 2, 'name': '13FV1234BU'}},
    {'matchTo': {'description': 'ok', 'id': 3, 'name': '13FV1234BU'},
     'score': 1.0,
     'target': {'description': 'ok', 'id': 3, 'name': '13FV1234BU'}}],
   'source': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'}}],
 'jobId': 6147120367590349,
 'startTime': 1606216546372,
 'status': 'Completed',
 'statusTime': 1606216546552}
}
```

### Add additional key `match_fields`

By default, only `name` in sources and `name` in targets are used to calculate similarity measures. The `match_fields` parameter lets you specify all combinations of fields in sources and targets that should be used to calculate features.

In this example, it looks like comparing the `description` field for both targets and sources could improve the model.

:::info note
Calculating similarity measures can be time-consuming. It's recommended that you don't use `match_fields` combinations that add little or no information to the model.
:::

```http
POST /api/v1/projects/publicdata/context/entitymatching/
Content-Type: application/json

{
   "sources":sources,
   "targets":targets,
   "name":"simple_model_1",
   "description":"Simple model 1",
   "featureType":"simple",
   "matchFields":[
      {
         "source":"name",
         "target":"name"
      },
      {
         "source":"description",
         "target":"description"
      }
   ],
   "classifier":"randomforest",
}
```

The request above results in an error because one of the items in `match_to` is missing `description`. If `complete_missing` is set to `True`, empty strings replace missing values.

### Add `ignore_missing_fields`

To avoid errors if items in sources or targets have missing values, add `ignore_missing_fields=True`.

```http
POST /api/v1/projects/publicdata/context/entitymatching/
Content-Type: application/json

{
   "sources":sources,
   "targets":targets,
   "name":"simple_model_1",
   "description":"Simple model 1",
   "featureType":"simple",
   "matchFields":[
      {
         "source":"name",
         "target":"name"
      },
      {
         "source":"description",
         "target":"description"
      }
   ],
   "classifier":"randomforest",
   "ignoreMissingFields":true
}
```

:::info note
The model gives the correct matches a score of 1 and the incorrect matches a score of 0.5.
:::

### Using feature types

By default, `feature_type` is set to `simple`. The different feature types are created to improve the model's accuracy for different types of input data. Which feature type works best for your model varies based on what your data look like. The options for `feature_type` are: "simple", "bigram", "frequency-weighted-bigram", "bigram-extra-tokenizers", "bigram-combo". This section illustrates the strengths and weaknesses of the different feature types.

#### When to use `feature_type=simple`?

"Simple" is the default feature type and is preferred when one string is a substring of the other and the other characters don't appear in the other string. For example, "BCDEF" is a substring of "ABCDEFG" and the other characters "AG" don't appear in the string "BCDEF". This feature type is also the fastest option.

#### Limitations of the simple feature type

The data below is the same as in the earlier examples, except for two new items in the targets.
_Id 10_ and _13_ are similar to _0_ and _3_, respectively. The first letter combination ("AA" and "FV") is swapped with the prefix for the `match_from` items (KKL).

This leads to difficulties with using the "simple" feature type.

```python
sources = [
    {"id":0, "name" : "KKL_21AA1019CA.PV", "description": "correct"},
    {"id":1, "name" : "KKL_13FV1234BU.VW", "description": "ok"}
]
targets = [
    {"id":0, "name" : "21AA1019CA", "description": "correct"},
    {"id":10, "name" : "21KKL1019CA", "description": "correct"},
    {"id":1, "name" : "21AA1119CA", "description": "wrong"},
    {"id":2, "name" : "13FV1234BU"},
    {"id":3, "name" : "13FV1334BU", "description": "ok"},
    {"id":13, "name" : "13KKL1234BU", "description": "ok"}
]
```

```http
POST /api/v1/projects/publicdata/context/entitymatching/
Content-Type: application/json

{
   "sources":sources,
   "targets":targets,
   "name":"simple_model_1",
   "description":"Simple model 1",
   "featureType":"simple",
   "matchFields":[
      {
         "source":"name",
         "target":"name"
      }
   ],
   "classifier":"randomforest",
}
```

**Example results:**

```python
{'createdTime': 1606153318209,
 'items': [{'matchFrom': {'description': 'correct',
    'id': 0,
    'name': 'KKL_21AA1019CA.PV'},
   'matches': [{'matchTo': {'description': 'correct',
      'id': 0,
      'name': '21AA1019CA'},
     'score': 0.8944271909999159,
     'target': {'description': 'correct', 'id': 0, 'name': '21AA1019CA'}},
    {'matchTo': {'description': 'correct', 'id': 10, 'name': '21KKL1019CA'},
     'score': 0.8944271909999159,
     'target': {'description': 'correct', 'id': 10, 'name': '21KKL1019CA'}}],
   'source': {'description': 'correct', 'id': 0, 'name': 'KKL_21AA1019CA.PV'}},
  {'matchFrom': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'},
   'matches': [{'matchTo': {'id': 2, 'name': '13FV1234BU'},
     'score': 0.8944271909999159,
     'target': {'id': 2, 'name': '13FV1234BU'}},
    {'matchTo': {'description': 'ok', 'id': 13, 'name': '13KKL1234BU'},
     'score': 0.8944271909999159,
     'target': {'description': 'ok', 'id': 13, 'name': '13KKL1234BU'}}],
   'source': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'}}],
 'jobId': 5049964554908808,
 'startTime': 1606153318586,
 'status': 'Completed',
 'statusTime': 1606153318777}
```

:::info note
The new target items have identical scores to the correct matches. This is because with `feature_type="simple"`, only the number of matching tokens are considered.
:::

Target items with `id` _0_ _21_, _AA_, _1019_, and _CA_ match a token in sources items with `id` _0_. Target items with `id` _10_ _21_, _KKL_, _1019_, and _CA_ matches a token in source items with `id` _0_. Thus, the same number of tokens matches.

The model doesn't consider that the target items with `id` _0_ have more and longer contiguous sequences of tokens.

#### When to use `feature_type=bigram`?

The "bigram" feature type does account for sequences of tokens. In addition to counting the number of matching tokens, it looks at the number of matching bigrams. that's the number of matching tokens when two and two adjacent tokens are combined, BCDEF and ABBCDE1F, for example.

```python
sources = [
    {"id":0, "name" : "KKL_21AA1019CA.PV", "description": "correct"},
    {"id":1, "name" : "KKL_13FV1234BU.VW", "description": "ok"}
]
targets = [
    {"id":0, "name" : "21AA1019CA", "description": "correct"},
    {"id":10, "name" : "21KKL1019CA", "description": "correct"},
    {"id":1, "name" : "21AA1119CA", "description": "wrong"},
    {"id":2, "name" : "13FV1234BU"},
    {"id":3, "name" : "13FV1334BU", "description": "ok"},
    {"id":13, "name" : "13KKL1234BU", "description": "ok"}
]
```

```http
POST /api/v1/projects/publicdata/context/entitymatching/
Content-Type: application/json

{
   "sources":sources,
   "targets":targets,
   "name":"bigram_model_1",
   "description":"bigram model 1",
   "featureType":"bigram",
   "matchFields":[
      {
         "source":"name",
         "target":"name"
      }
   ],
   "classifier":"randomforest",
}
```

**Example results:**

```python
{'createdTime': 1606153733720,
 'items': [{'matchFrom': {'description': 'correct',
    'id': 0,
    'name': 'KKL_21AA1019CA.PV'},
   'matches': [{'matchTo': {'description': 'correct',
      'id': 0,
      'name': '21AA1019CA'},
     'score': 0.9149207688467006,
     'target': {'description': 'correct', 'id': 0, 'name': '21AA1019CA'}}],
   'source': {'description': 'correct', 'id': 0, 'name': 'KKL_21AA1019CA.PV'}},
  {'matchFrom': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'},
   'matches': [{'matchTo': {'id': 2, 'name': '13FV1234BU'},
     'score': 0.9149207688467006,
     'target': {'id': 2, 'name': '13FV1234BU'}}],
   'source': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'}}],
 'jobId': 6677803085358639,
 'startTime': 1606153733995,
 'status': 'Completed',
 'statusTime': 1606153734183}
```

#### When to use `feature_type=Frequency-Weighted-Bigram`?

The "Frequency-Weighted-Bigram" feature type calculates a similarity score based on the sequence of the terms. It also gives higher weights to less commonly occurring tokens. This can be helpful when the "simple" feature type doesn't return useful results.

```python
sources = [
    {"id":0, "name" : "KKL_21AA1019CA.PV", "description": "correct"},
    {"id":1, "name" : "KKL_13FV1234BU.VW", "description": "ok"}
]
targets = [
    {"id":0, "name" : "21AA1019CA", "description": "correct"},
    {"id":10, "name" : "21KKL1019CA", "description": "correct"},
    {"id":1, "name" : "21AA1119CA", "description": "wrong"},
    {"id":2, "name" : "13FV1234BU"},
    {"id":3, "name" : "13FV1334BU", "description": "ok"},
    {"id":13, "name" : "13KKL1234BU", "description": "ok"}
]
```

```http
POST /api/v1/projects/publicdata/context/entitymatching/
Content-Type: application/json

{
   "sources":sources,
   "targets":targets,
   "name":"fbw_model_1",
   "description":"fwb model 1",
   "featureType":"Frequency-Weighted-Bigram",
   "matchFields":[
      {
         "source":"name",
         "target":"name"
      }
   ],
   "classifier":"randomforest",
}
```

**Example results:**

```python
{'createdTime': 1606153836538,
 'items': [{'matchFrom': {'description': 'correct',
    'id': 0,
    'name': 'KKL_21AA1019CA.PV'},
   'matches': [{'matchTo': {'description': 'correct',
      'id': 0,
      'name': '21AA1019CA'},
     'score': 0.9149207688467006,
     'target': {'description': 'correct', 'id': 0, 'name': '21AA1019CA'}}],
   'source': {'description': 'correct', 'id': 0, 'name': 'KKL_21AA1019CA.PV'}},
  {'matchFrom': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'},
   'matches': [{'matchTo': {'id': 2, 'name': '13FV1234BU'},
     'score': 0.9149207688467006,
     'target': {'id': 2, 'name': '13FV1234BU'}}],
   'source': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'}}],
 'jobId': 5204435061404568,
 'startTime': 1606153836790,
 'status': 'Completed',
 'statusTime': 1606153837071}
```

#### When to use `feature_type=Bigram-Extra-Tokenizers`?

The "Bigram-Extra-Tokenizers" feature type is similar to bigram but can learn that leading zeros and spaces should be ignored in matching. For example ABCDE and 000ABBCDE1F.

```python
sources = [
    {"id":0, "name" : "KKL_21AA1019CA.PV", "description": "correct"},
    {"id":1, "name" : "KKL_13FV1234BU.VW", "description": "ok"}
]
targets = [
    {"id":0, "name" : "000021AA1019CA", "description": "correct"},
    {"id":10, "name" : "21KKL1019CA", "description": "correct"},
    {"id":1, "name" : "21AA1119CA", "description": "wrong"},
    {"id":2, "name" : "000013FV1234BU"},
    {"id":3, "name" : "13FV1334BU", "description": "ok"},
    {"id":13, "name" : "13KKL1234BU", "description": "ok"}
]
```

```http
POST /api/v1/projects/publicdata/context/entitymatching/
Content-Type: application/json

{
   "sources":sources,
   "targets":targets,
   "name":"BET_model_1",
   "description":"BET model 1",
   "featureType":"Bigram-Extra-Tokenizers",
   "matchFields":[
      {
         "source":"name",
         "target":"name"
      }
   ],
   "classifier":"randomforest",
}
```

**Example results:**

```python
{'createdTime': 1606154023578,
 'items': [{'matchFrom': {'description': 'correct',
    'id': 0,
    'name': 'KKL_21AA1019CA.PV'},
   'matches': [{'matchTo': {'description': 'correct',
      'id': 0,
      'name': '000021AA1019CA'},
     'score': 0.8477338488399959,
     'target': {'description': 'correct', 'id': 0, 'name': '000021AA1019CA'}}],
   'source': {'description': 'correct', 'id': 0, 'name': 'KKL_21AA1019CA.PV'}},
  {'matchFrom': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'},
   'matches': [{'matchTo': {'id': 2, 'name': '000013FV1234BU'},
     'score': 0.8477338488399959,
     'target': {'id': 2, 'name': '000013FV1234BU'}}],
   'source': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'}}],
 'jobId': 6675842531673047,
 'startTime': 1606154023915,
 'status': 'Completed',
 'statusTime': 1606154024190}
```

#### When to use `feature_type=Bigram-Combo`?

The "Bigram-Combo" feature type calculates all the above options relying on the model to find the appropriate features to use. This is the slowest option and is mostly suitable for supervised models.

```http
POST /api/v1/projects/publicdata/context/entitymatching/
Content-Type: application/json

{
   "sources":sources,
   "targets":targets,
   "name":"BC_model_1",
   "description":"BC model 1",
   "featureType":"Bigram-Combo",
   "matchFields":[
      {
         "source":"name",
         "target":"name"
      }
   ],
   "classifier":"randomforest",
}
```

**Example results:**

```python
{'createdTime': 1606154392343,
 'items': [{'matchFrom': {'description': 'correct',
    'id': 0,
    'name': 'KKL_21AA1019CA.PV'},
   'matches': [{'matchTo': {'description': 'correct',
      'id': 0,
      'name': '21AA1019CA'},
     'score': 0.8195704160778803,
     'target': {'description': 'correct', 'id': 0, 'name': '21AA1019CA'}}],
   'source': {'description': 'correct', 'id': 0, 'name': 'KKL_21AA1019CA.PV'}},
  {'matchFrom': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'},
   'matches': [{'matchTo': {'id': 2, 'name': '13FV1234BU'},
     'score': 0.8195704160778803,
     'target': {'id': 2, 'name': '13FV1234BU'}}],
   'source': {'description': 'ok', 'id': 1, 'name': 'KKL_13FV1234BU.VW'}}],
 'jobId': 4397635623636246,
 'startTime': 1606154392670,
 'status': 'Completed',
 'statusTime': 1606154392848}
```

#### About the score

A score above 0.8 indicates that the source and the target are matched with high probability.

Note that a score below 0.8 and above 0.5 doesn't indicate that the source and target are matched with more than 50% probability. The example below illustrates that even if the source and the target don't match at all, they can still receive a score over 0.6:

```python
sources = [{"id":0, "name" : "J04_ONSTREAM_HOUR_AVG", "description": "correct"}]
targets = [{"id":0, "name" : "87-JB-004-J04", "description": "correct"}]
```

```http
POST /api/v1/projects/publicdata/context/entitymatching/
Content-Type: application/json

{
   "sources":sources,
   "targets":targets,
   "name":"bigram_model_1",
   "description":"bigram model 1",
   "featureType":"bigram",
   "matchFields":[
      {
         "source":"name",
         "target":"name"
      }
   ],
   "classifier":"randomforest",
}
```

```http
POST /api/v1/projects/publicdata/context/entitymatching/predict
Content-Type: application/json

{
  "id": 6147120367590349,
  "numMatches": 3,
  "scoreThreshold": 0.5
}
```

```http
GET /api/v1/projects/publicdata/context/entitymatching/jobs/<job_id>
Content-Type: application/json
```

Example matching results

```python
{'createdTime': 1606154525247,
 'items': [{'matchFrom': {'description': 'correct',
    'id': 0,
    'name': 'J04_ONSTREAM_HOUR_AVG'},
   'matches': [{'matchTo': {'description': 'correct',
      'id': 0,
      'name': '87-JB-004-J04'},
     'score': 0.6049029006116509,
     'target': {'description': 'correct', 'id': 0, 'name': '87-JB-004-J04'}}],
   'source': {'description': 'correct',
    'id': 0,
    'name': 'J04_ONSTREAM_HOUR_AVG'}}],
 'jobId': 1019931106940009,
 'startTime': 1606154525580,
 'status': 'Completed',
 'statusTime': 1606154525758}
```

## Get model info

If you have a `model_id` and want to know which parameters you used when training the model, use the retrieve method.

```http
GET /api/v1/projects/publicdata/context/entitymatching/<model_id>
Content-Type: application/json
```
