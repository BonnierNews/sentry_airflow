# Sentry Airflow Plugin---deprecated

A plugin for [Airflow](https://airflow.apache.org/) dags and tasks that sets up [Sentry](https://sentry.io) for error logging. 

## Setup

### Install on Google Composer

Install the `sentry-sdk` into Google Composer's [Python dependencies](https://cloud.google.com/composer/docs/how-to/using/installing-python-dependencies#install-package).

Set environment variable SENTRY_DSN to the Sentry project key

Add this folder to the plugin directory:

```shell
$ cd ..
$ gcloud composer environments storage plugins import  \ 
    --location=europe-west1  \ 
    --project=applications-data-bn  \ 
    --environment=bn-composer-prod \ 
    --source=<path to sentry_airflow>
```

### Deploy changes to Google Composer

Same as above. Skip the first 2 steps and only run the gcloud command

### Local Development

Install the `sentry-sdk` with the `flask` extra:

```shell
$ pip install --upgrade 'sentry-sdk[flask]==0.11.2',  'freezegun'
```

Create a plugin folder in your `AIRFLOW_HOME` directory if you do not have one yet:

```shell
$ mkdir -p $AIRFLOW_HOME/plugins
```

Then copy the plugin into your container 

```shell
$ docker cp sentry_airflow <container_id>:/opt/airflow/plugins/sentry_airflow
```

**Make sure you have setup your `SENTRY_DSN` in your environment variables!** 
The DSN can be found in Sentry by navigating to [Project Name] -> Project Settings -> Client Keys (DSN). Its template resembles the following: `'{PROTOCOL}://{PUBLIC_KEY}@{HOST}/{PROJECT_ID}'`

```shell
$ export SENTRY_DSN=<value fount at  https://sentry.io/settings/bonnier-news/projects/data-composer/keys/> 
```


(For more information checkout Google's [Docs](https://cloud.google.com/composer/docs/concepts/plugins#installing_a_plugin))

Either set an environment variable on [Google composer](https://cloud.google.com/composer/docs/how-to/managing/environment-variables) for your `SENTRY_DSN`
or in the Airflow webserver UI, add a connection (Admin->Connections) for `sentry_dsn`. Let the connection type be `HTTP` and the host be the DSN value.
