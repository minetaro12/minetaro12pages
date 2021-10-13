+++
date = 2021-10-14T10:51:00Z
draft = true
tags = ["linux", "ubuntu", "wal-g", "postgresql"]
title = "PostgresqlをWAL-Gでバックアップ&リストア"

+++
こちらのサイトを参考にしました。

[https://blog.noellabo.jp/entry/2019/03/05/yMjQeU9JXHxcyHTL](https://blog.noellabo.jp/entry/2019/03/05/yMjQeU9JXHxcyHTL)

[https://qiita.com/atsu1125/items/676d24c0473ad94b3f2b](https://qiita.com/atsu1125/items/676d24c0473ad94b3f2b)

***

環境

OracleCloudのA1インスタンス

Ubuntu20.04LTS

PostgreSQL12

***

## 1\.WAL-Gをインストールする