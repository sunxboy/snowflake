

We have retired the initial release of Snowflake and working on open sourcing
the next version based on
[Twitter-server](https://twitter.github.io/twitter-server/), in a form that can
run anywhere without requiring Twitter's own infrastructure services.

The initial version, released in 2010, was based on Apache Thrift and it
predated [Finagle](https://twitter.github.io/finagle/), our building
block for RPC services at Twitter.  The Snowflake we're using internally is a
full rewrite and heavily relies on existing infrastructure at Twitter to run.
We cannot commit to a date but we're doing our best to add necessary features to
make Snowflake fit for many environments outside of Twitter.

Source code is still in the repository and is reachable from
[snowflake-2010](https://github.com/twitter/snowflake/releases/tag/snowflake-2010)
tag.

We won't be accepting pull requests or responding to issues for the retired
release.
Snowflake

为了满足在分布式系统中可以生成全局唯一且趋势递增的 ID，Twitter 推出了一种算法，该算法由 64 bit 组成。

![avatar](https://note.youdao.com/yws/api/personal/file/WEB494d8493f3a2635c4dda26380c3ee06a?method=download&shareKey=ee740e662e59ad62b907200f6c1aac26)

第一位永远是0，实际上这是为了让生成的 ID 都为正数，以保证趋势递增；
后面 41 位用来记录时间，理论上可以记录 2^41 毫秒，2^41/(24 * 3600 * 365 * 1000) = 69.7 年，所以这里的理论最大使用时间就是 70 年左右；
在后面 10 位用来记录机器 ID ，更准确的应该说是实例 ID，对应的可以是某个 Container 或者某个进程，最多支持 1024 个；
最后12位用来记录序列号，来保证每个实例每毫秒生成的 ID 唯一；


该算法的优点：
不依赖数据库，高性能；
生成的 ID 趋势递增；
64 bit 的数字作为 ID 相比 UUID 短的多，方便建立数据库索引。
该算法的缺点：

依赖系统时钟，如果系统时钟发生回拨，那么有可能造成 ID 冲突或乱序。
