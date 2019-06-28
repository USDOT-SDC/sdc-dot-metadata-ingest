[![Build Status](https://travis-ci.com/usdot-jpo-sdc/sdc-dot-metadata-ingest.svg?branch=master)](https://travis-ci.com/usdot-jpo-sdc/sdc-dot-metadata-ingest)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=usdot-jpo-sdc_sdc-dot-metadata-ingest&metric=alert_status)](https://sonarcloud.io/dashboard?id=usdot-jpo-sdc_sdc-dot-metadata-ingest)
[![Coverage](https://sonarcloud.io/api/project_badges/measure?project=usdot-jpo-sdc_sdc-dot-metadata-ingest&metric=coverage)](https://sonarcloud.io/dashboard?id=usdot-jpo-sdc_sdc-dot-metadata-ingest)
# sdc-dot-metadata-ingest
This is a lambda function developed by SDC Team for generating the metadata from an s3 key and indexing into Elasticsearch Service.

There are two primary functions serves the need for two different lambda functions:
* **bucket-handler-lambda** - generates the metadata and indexes into Elasticsearch
* **register-kibana-dashboards** - generates the default datalake visualization dashboards


<a name="toc"/>

## Table of Contents

[I. Release Notes](#release-notes)

[II. Overview](#overview)

[III. Design Diagram](#design-diagram)

[IV. Getting Started](#getting-started)

[V. Unit Tests](#unit-tests)

[VI. Support](#support)

---

<a name="release-notes"/>


## [I. Release Notes](ReleaseNotes.md)
TO BE UPDATED

<a name="overview"/>

## II. Overview
This lamda function is triggered by aws-s3-notification whenever an object is put into raw submission bucket or curated bucket.The primary function of this lambda is given below.

**1.** It creates metadata of the new object and push metadata to elastic search.

**2.** It also push custom metrics for raw submission count,zero byte and curated count to cloud watch metrics.

**3.** It also creates visualization metrics in kibana.

**4.** In case of any failures/errors it push messages in DLQ so that this can be processed later.

<a name="design-diagram"/>

## III. Design Diagram

![sdc-dot-metadata-ingest](images/sdc-dot-metadata-ingest.png)

<a name="getting-started"/>

## IV. Getting Started

The following instructions describe the procedure to build and deploy the lambda.

### Prerequisites
* NA 

---
### ThirdParty library

*NA

### Licensed softwares

*NA

### Programming tool versions

*Python 3.6


---
### Build and Deploy the Lambda

#### Environment Variables
Below are the environment variable needed :- 

CURATED_BUCKET_NAME - {name_of_the_curate_bucket}

CV_SUBMISSIONS_COUNTS_METRIC  - {metrics_name_of_cv_submission}

ELASTICSEARCH_ENDPOINT  - {url_of_elatic_search}

ENVIRONMENT_NAME        -{dev/preprod/prod}

PUBLISHED_BUCKET_NAME   -{name_of_the_published_bucket}

SUBMISSIONS_BUCKET_NAME - {name_of_the_raw_submission_bucket}

WAZE_CURATED_COUNTS_METRIC - {metrics_name_of_curated}

WAZE_SUBMISSIONS_COUNT_METRIC -{metrics_name_of_raw_submission}

WAZE_ZERO_BYTE_SUBMISSIONS_COUNT_METRIC - {metrics_name_of_zero_byte}

#### Build Process

**Step 1**: Setup virtual environment on your system by foloowing below link
https://docs.aws.amazon.com/lambda/latest/dg/with-s3-example-deployment-pkg.html#with-s3-example-deployment-pkg-python

**Step 2**: Crete a script file with below contents for e.g(sdc-dot-waze-data-ingest.sh)
```#!/bin/sh

cd {path_to_your_repository}/sdc-dot-metadata-ingest
zipFileName="{path_to_your_repository}/sdc-dot-metadata-ingest.zip"

echo "Zip file name is = ${zipFileName}"

zip -9 $zipFileName lambdas/*
zip -r9 $zipFileName common/*
zip -r9 $zipFileName dashboard_registry_handler_main.py.py
zip -r9 $zipFileName bucket_event_handler_main.py

cd {path_to_your_virtual_env}/python3.6/site-packages/
zip -r9 $zipFileName chardet certifi idna
```

**Step 3**: Change the permission of the script file

```
chmod u+x sdc-dot-waze-data-ingest.sh
```

**Step 4** Run the script file
./sdc-dot-metadata-ingest.sh

**Step 5**: Upload the sdc-dot-metadata-ingest.zip generated from Step 4 to a lambda function via aws console.

[Back to top](#toc)

---
<a name="unit-tests"/>

## V. Unit Tests

TO BE UPDATED

---
<a name="support"/>

## VI. Support

For any queries you can reach to support@securedatacommons.com
---
[Back to top](#toc)
