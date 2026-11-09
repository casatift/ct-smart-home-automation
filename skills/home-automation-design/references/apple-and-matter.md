# 苹果家庭、HomeKit Bridge 与 Matter Hub

## 用户体验与选路

将“接入 HomeKit/苹果家庭”理解为用户希望在苹果家庭 App 和 Siri 使用设备，再比较设备原生接入、HomeKit Bridge 和 Matter。保留已稳定使用的配对，按能力、稳定性、依赖和维护成本选择。用户偏好集中管理时可建议一个 HomeKit 主桥；不是每家必选，也不能承诺所有类型都能进入同一桥。

发现扫地机有有效分区清扫能力时，主动说明苹果家庭原生分区选择与基础启停的差别。用户需要原生分区时，评估合适的 Matter Hub 或设备原生 Matter 路线；不强制所有家庭安装某个 Hub。检查当前维护仓库、正式版本、源集成、房间数据、真实命令、Apple 系统与家庭中枢。只有机器在厂商 App 支持但 HA 没有数据时，先补齐源端，不宣称已具备端到端能力。

HomeKit Bridge 的扫地机通常是开关映射，分房间脚本也只是开关式操作，不是原生扫地机的房间选择界面。Matter Hub 不应一概替代 HomeKit：按设备类型、能力、稳定性及目标控制器支持比较，部分映射只是模拟类型。

## 一个 HomeKit 主桥的边界

- 按 unique accessories/AID 计数，官方限制每桥 150，不是按 HA 设备或实体总数简单计数。超限先报告实际数量并解释不可兼得，再安排必要扩展，不能静默漏导。
- 当前 HA 官方文档要求 camera、lock、TV/receiver 类型 media_player、activity remote 采用单实体 accessory 模式；作为明确例外，不强塞进主桥。
- 为每个物理设备选择主要实体及有意义功能，去掉重复接入的副本；诊断实体、调试开关、内部模式 helper 不自动当真实设备。用户要求“所有设备”时提供导出/例外/不支持清单。
- 原生灯、风扇、温控、窗帘等与 switch 模拟不同；传感器单位和 device_class 确认后再导出。select、script、scene 等映射不能假装是设备完整协议能力。
- 保留 bridge 身份、配对存储、稳定 unique_id；重新建桥或更换身份可能丢苹果家庭房间/场景。已有配对不无故删除。
- 确认局域网可达、mDNS、网络隔离情况；用户远程控制苹果家庭需适用的家庭中枢。HomeKit 暴露与 AI/Assist 暴露分别配置。

## Matter 扫地机准备与验收

以下以 RiDDiX Matter Hub 为一种可选实现参考，采用其他方案时按其实际接口验证。以当前维护项目的正式发行和对应 tag 源码为依据，核验 RVC、Service Area 与房间选择，不把 main 分支功能当已发布。

1. **源实体**：确认同一物理机的各个集成。优先有 `CLEAN_AREA` 和有效 HA 区域映射的实体；否则核对厂商地图、`rooms`、`segments`、`room_list`、MIoT `room_mapping` 或自定义适配。标准服务在系统中存在，不等于每台机支持。
2. **数据**：地图 ID、真实 segment ID、名称、楼层和当前地图稳定。API 回空房间不能从别台机或云端全屋房间列表猜编号；坐标划区与已命名房间分区分开。
   如果房间对应表为空但 App 确有分区，可只读比较设备原始对应表与 HA 缓存，再用与当前集成匹配的地图解析器检查原始地图 segment。地图可能提供编号却没有名称，需用户对照编号图确认名称。核对当前地图、编号有效性并按授权测试命令后，可用显式分区适配；不要因此直接更换集成、重建地图或修改 HA 源码。
3. **命令**：核对目标实体的 `vacuum.clean_area` 或厂商命令确实支持。MIoT 等可能通过 `vacuum.send_command` 的 `app_segment_clean`；参数必须与实际固件/服务一致。文档举例的 `xiaomi_home.vacuum_clean_room_ids` 不证明官方 Xiaomi Home 实际存在此服务。
4. **节点**：核对选定项目和发行版本对 Apple Home 扫地机的节点要求；要求独立 **Server Mode** 时通常建立单独节点，作为集中主桥方案的例外。普通 bridged vacuum 不直接认定兼容。只导出选定主实体，避免相同机器重复出现。
5. **苹果环境**：核对当前官方 Apple 系统/家庭中枢对 Matter 扫地机与分区功能的要求，并检查设备实际版本。局域网 IPv6 与 mDNS、必要跨网段发现应通畅；不为配对随意关闭整网安全。已有 Thread 边界路由器不是所有 Wi-Fi/以太网 Matter 桥都必需，按实际链路判断。
6. **选择行为**：Apple-only 使用可按项目建议关闭“将各房间暴露为独立 cleaning modes”，让 Service Area 负责房间选择；与 Alexa/Google 共用时先核对取舍，不能一刀切。
7. **批量**：多房选择应一次传递完整集合或使用已证明可靠的厂商调度。自定义 `batchDispatch` 会组合数据并注入 selected_area_* 元数据；目标服务不接受额外字段时用真实脚本吸收和转换。不要逐房调用覆盖前一房而最后只扫一个。
8. **验收**：先只读核对，再经任务授权选一个可安全清扫区域测试；验证单房、多房、暂停、回充及状态同步。不把节点上线或二维码出现当成功。配对需用户手机扫码时，交付清晰操作和待验收项。

接入前备份 Hub 配置和配对信息，不触碰现有地图。Matter 只做能力导出，不补齐源集成缺失能力，也不改变后端的云依赖。勿把水浸模拟接触、燃气模拟 CO 等不同报警语义当安全等价。

参考：

- [HA HomeKit Bridge 类型与限制](https://www.home-assistant.io/integrations/homekit/)
- [RiDDiX Matter Hub](https://github.com/RiDDiX/home-assistant-matter-hub)
- [设备类型](https://riddix.github.io/home-assistant-matter-hub/supported-device-types)
- [扫地机与 Server Mode](https://riddix.github.io/home-assistant-matter-hub/devices/robot-vacuum)
- [项目正式发行](https://github.com/RiDDiX/home-assistant-matter-hub/releases)
- [Apple 家庭配件支持](https://support.apple.com/102135)
