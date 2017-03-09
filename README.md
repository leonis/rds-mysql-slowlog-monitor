# rds-mysql-slowlog-monitor

[![dockerhub](https://img.shields.io/docker/automated/leonisandco/rds-mysql-slowlog-monitor.svg)](https://hub.docker.com/r/leonisandco/rds-mysql-slowlog-monitor/)

This repository provides a docker image which monitors the slow query logs in the RDS(mysql engine) and notifies the event to the slack channel.

## Architecture

```
[RDS(mysql)] --(slow query log)--> {[fluentd on docker]} --(incoming webhook)--> [slack channel]
```

# HowToUse

## Configuration

### RDS(mysql)

slow query logが出力されるように設定を調整してください。

### docker image

環境変数によって、対象にするRDS/slackの通知先を変更します。

see `sample.env`.

# HowToRun

環境変数は`sample.env`を参考に、dotenvのフォーマットでファイルにまとめておくと便利です。

```
# pull the docker image
docker pull leonisandco/rds-mysql-slowlog-monitor:latest

# just run , not as a daemon
docker run -it --rm --env-file dot.env leonisandco/rds-mysql-slowlog-monitor:latest

# run as a daemon
docker run -d --env-file dot.env leonisandco/rds-mysql-slowlog-monitor:latest
```

# WARN

このdocker imageを使うと、RDS(mysql)上のslow_logテーブルの中身が定期的にクリアされて無くなります。

**slow_logのデータを保存したい場合は、どこかのデータストアに保存する/fluent.confを調整してbackupするようにするなどが必要**です。
