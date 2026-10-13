# K8s 集群监控部署项目
基于 Prometheus + Alertmanager + Grafana + Metrics-Server 搭建 Kubernetes 监控体系，全部资源使用 YAML 原生清单部署，邮件告警，适配 NFS 持久化存储。

## 组件说明
| 组件 | 说明 |
| ---- | ---- |
| Prometheus | 指标采集、时序存储、告警规则评估 |
| Alertmanager | 告警分组、去重、降噪，邮件通知 |
| Grafana | 指标可视化展示，节点、Pod监控大盘 |
| Metrics-server | 提供 `kubectl top`、HPA 所需指标 |
| node-exporter | 采集服务器硬件指标：CPU、内存、磁盘 |
| kube-state-metrics | 采集K8s资源状态指标：Pod、Deployment、StatefulSet等 |

## 项目特性
- Prometheus 与 Alertmanager 同 Pod 部署，Pod 内通过 127.0.0.1 通信
- 告警路由区分节点告警，带有 `team=node` 标签告警走独立邮件路由
- 内置节点内存使用率告警规则，方便扩展CPU、磁盘、Pod重启等告警
- 开启 Prometheus 配置热重载，支持 `/-/reload` 动态加载配置
- 支持 Service 注解自动发现：`prometheus_io_scrape: "true"`
- cadvisor 使用 apiserver 代理抓取，适配网络策略禁止Pod直连 kubelet 10250 的场景
