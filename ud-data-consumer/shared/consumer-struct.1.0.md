
# 需求方项目结构

## 项目结构

```struct
                        核心服务(consumer-core)---------------------------------+
                        /       |       \         \                            |
                       /        |        \          \                          |
                      /         |         \           \                        |
                     /          |          \            \                      |
                    /           |           \             \                    |
                   /            |            \              \                  |
                  /             |             \               \                |
                 /              |              \                \              |
                /               |               \                 \            |
 第三方存证(certification)   黑名单(zebar)   用户影像(id-mapping)  其他场景(...)    |
               |                |                |                             |
               |                |                |                             |
               |                |                |                             |
               +----------------+----------------+-----------需求方服务(consumer-server)

```