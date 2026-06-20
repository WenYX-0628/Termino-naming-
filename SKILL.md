---
id: variable-naming
name: Variable Naming
description: 规范代码变量命名 + UI/日志/绘图字符串 + 文档图片标题命名审查——检测不规范命名（拼音、中文、意义不明的缩写），基于内置中英文术语词典（含通用编程 + 雷达/信号处理 + 气象 + LiDAR + 国标PDF 600+ 条术语）给出规范建议，并可自动/手动应用重命名。UI/日志字符串在语法正确前提下容许中英文混写，仅修复格式规范及术语一致性问题。
category: Code
requires: []
examples:
  - /variable-naming review 审查当前工作区的变量命名
  - /variable-naming fix 审查并自动修复命名问题
  - /variable-naming dict 列出内置术语词典
  - /variable-naming add 用户管理  userManager 向词典添加新条目
  - /variable-naming translate 用户管理  翻译中文术语为英文
  - /variable-naming dict 雷达  筛选雷达相关术语
  - /variable-naming translate 恒虚警检测  查询雷达信号处理术语
  - /variable-naming caption review 审查文档中图片/表格标题命名
  - /variable-naming caption fix 审查并修复标题命名
  - /variable-naming caption format 查看标题命名规范格式
  - /variable-naming ui review 审查 UI/日志/绘图字符串
  - /variable-naming ui fix 审查并修复 UI/日志/绘图字符串
---

# Variable Naming Skill

## 功能

### 代码变量命名

审查代码中的变量、函数、类型命名，检测以下问题并给出修复建议：

| 问题类型 | 示例 | 建议 |
|---------|------|------|
| 拼音命名 | `var yonghu string` | `var user string` |
| 中文命名 | `let 名称 = "foo"` | `let name = "foo"` |
| 拼音缩写 | `hs := getList()` | `func getList()` |
| 意义不明的缩写 | `tmp`, `aaa`, `data1`, `val` | 更具描述性的命名 |
| 命名风格不一致 | 混用 camelCase / snake_case | 按语言规范统一 |

### 文档标题（Caption）命名

审查论文/技术文档中图片、表格、公式、算法的标题命名，检测以下问题并给出修复建议：

| 问题类型 | 示例 | 建议 |
|---------|------|------|
| 编号格式不一致 | `图1`/`图 1`/`Fig1`/`Figure1` | 统一编号格式 |
| 无空格断句 | `Fig.1DataFusionFlow` | `Fig.1 Data Fusion Flow` |
| 英文翻译含拼音/中文 | `Fig.1 yonghu guanli liucheng` | `Fig.1 User Management Flow` |
| 大小写不当 | `Fig.1 analysis of radar echo data` | `Fig.1 Analysis of Radar Echo Data` |
| 术语不规范 | 标题中用了不规范的术语 | 对照术语词典修正 |
| 图表术语不统一 | 正文用"用户"，标题用"使用者" | 统一为词典推荐术语 |

### UI / 日志 / 绘图字符串

审查代码中的 `fprintf`、`print`、`console.log`、`title`、`xlabel`、`ylabel`、`legend`、`error`、`warning` 等函数中的字符串内容。**在语法正确的前提下，容许中英文混写**（例如中文叙述 + 英文术语/单位），仅检测格式不规范及术语一致性问题：

| 问题类型 | 示例（❌ 不通过） | 建议 |
|---------|-----------------|------|
| 无空格断句 | `'功率(dB)'` | `'功率 (dB)'`（中文与括号间加空格） |
| 中英文间无空格 | `'噪声参数:%.1f dB'` | `'噪声参数: %.1f dB'`（冒号后加空格） |
| 英文大小写不当 | `title('doppler spectrum')` | `title('Doppler Spectrum')`（英文标题式大写） |
| 中文句尾混英文无空格 | `'正在处理%d个距离库'` | `'正在处理 %d 个距离库'`（格式化占位符前后加空格） |
| **术语不统一** | 同一概念混用不同词（如"杂波"vs"地物"、"信号"vs"回波"） | 统一为上下文约定术语 |
| **术语与词典不符** | 英文术语拼写/缩写与词典不一致 | 对照词典修正（如 `snr`→`SNR`、`doppler`→`Doppler`） |
| 中文标点混用 | 英文句中使用中文逗号、括号 | 统一使用英文标点 |

**术语一致性检查规则：**
- 跨文件扫描同一概念的中英称呼，发现不一致时报警
- 对照内置术语词典，检查英文缩写/大小写是否正确（如 `prf`→`PRF`、`snr`→`SNR`）
- 同义术语在项目内应统一（如"地物杂波"vs"地面杂波"→选择一种）
- 对照 `/variable-naming dict` 查询推荐术语

**以下情况视为规范，不报错：**

**以下情况视为规范，不报错：**
| 写法 | 说明 |
|------|------|
| `'噪声参数: %.1f dB'` | ✅ 中文叙述 + 英文单位，空格规范 |
| `'功率 (dB)'` | ✅ 中文 + 英文单位，括号前有空格 |
| `'原始 I/Q'` | ✅ 中文 + 英文缩写 |
| `title('多普勒谱 / Doppler Spectrum')` | ✅ 中英文对照，语法完整 |
| `'正在处理 N=%d 个脉冲'` | ✅ 中文 + 格式化变量，空格规范 |

## 用法

### `/variable-naming review [路径]`
审查指定路径（默认当前目录）中的命名问题，列出问题的位置、当前名和建议名。

### `/variable-naming fix [路径]`
审查并交互式应用重命名（逐个确认，或 `--yes` 自动应用）。

### `/variable-naming dict [关键词]`
查看内置术语词典。可加关键词筛选，如 `/variable-naming dict 用户`。

### `/variable-naming add <中文> <英文>`
向项目词典添加新的术语条目，如 `/variable-naming add 用户管理 userManagement`。

### `/variable-naming translate <中文>`
查询中文术语对应的推荐英文名。

### `/variable-naming caption review [文件]`
审查文档（`.md .docx .tex`）中图片/表格/公式/算法标题的命名规范。列出文件中所有标题，检测编号格式、英译质量、术语一致性。

### `/variable-naming caption fix [文件]`
审查并交互式修复标题命名问题（逐个确认，或 `--yes` 自动应用）。

### `/variable-naming caption format`
查看标题命名规范说明，包括编号格式、中英文对照写法、大小写规则等。

### `/variable-naming ui review [路径]`
审查指定路径下代码中的 UI/日志/绘图字符串（`fprintf`、`title`、`xlabel`、`ylabel`、`legend`、`error`、`warning`、`console.log`、`print` 等），检测中英文混写、中文残留、术语不规范等问题。

### `/variable-naming ui fix [路径]`
审查并交互式修复 UI/日志/绘图字符串中的中文残留（逐个确认，或 `--yes` 自动应用）。

## 工作流程

### 代码变量命名

1. **扫描**：读取指定路径下的源代码文件（`.py .js .ts .go .java .rs .c .cpp .h .hpp`）
2. **分析**：
   - 用正则匹配标识符（变量名、函数名、类型名、参数名）
   - 检测拼音、中文、缩写等不规范模式
   - 对照内置术语词典查找建议名
3. **输出建议**：表格形式列出文件、行号、当前名 → 建议名
4. **应用**（fix 模式）：逐个或批量执行重命名（通过 IDE/Edit 工具）

### 文档标题（Caption）命名

1. **扫描**：读取文档文件（`.md .docx .tex` 等），用正则匹配标题模式：
   - 中文：`图X` / `表X` / `公式X` / `算法X`
   - 英文：`Fig.X` / `Table X` / `Figure X` / `Algorithm X`
   - LaTeX：`\caption{...}` / `\label{...}`
2. **分析**：
   - 编号格式：检查 `图1` vs `图 1` vs `Fig.1` 是否统一
   - 英译质量：检查英文标题中是否有拼音/中文残留
   - 术语一致性：对照术语词典，检查标题用词是否规范
   - 大小写风格：检查 Sentence case vs Title Case 是否统一
3. **输出建议**：列出文件、标题行、问题类型、建议修改
4. **应用**（fix 模式）：逐个或批量修正标题

### UI / 日志 / 绘图字符串

1. **扫描**：读取源代码文件（`.py .js .ts .go .java .rs .c .cpp .h .hpp .m`），匹配以下模式的字符串内容：
   - 控制台输出：`fprintf`、`printf`、`print`、`console.log`、`println`、`fmt.Printf`、`fmt.Println`
   - 绘图标签：`title`、`xlabel`、`ylabel`、`zlabel`、`label`、`set(gca, ...)`
   - 图例：`legend`、`sgtitle`、`suptitle`、`subtitle`
   - 错误/警告：`error`、`warning`、`throw`、`alert`、`toast`
   - 状态栏/提示：`statusbar`、`tooltip`、`hint`、`notify`
2. **分析**（容许语法正确的混写，仅报格式和术语问题）：
   - 空格规范：中文与英文/数字/格式化占位符之间是否缺空格
   - 括号规范：中文与英文括号是否混用
   - 标点规范：英文文本中是否混入中文逗号、句号、括号
   - 大小写规范：英文部分是否该用 Title Case
   - **术语一致性**：对照术语词典，检查英文单词拼写/大小写/缩写格式
   - **同义术语**：跨文件扫描同一概念是否有不同称呼
3. **输出建议**：列出文件、行号、函数名、问题类型
4. **应用**（ui fix 模式）：逐个或批量修正格式问题

## 内置中英文术语词典

### 通用编程概念

| 中文 | 英文 |
|------|------|
| 用户 | user / account |
| 管理员 | admin / administrator |
| 密码 | password / passwd |
| 登录 | login / signIn |
| 注册 | register / signUp |
| 退出 | logout / signOut |
| 权限 | permission / authority |
| 角色 | role |
| 菜单 | menu |
| 按钮 | button |
| 对话框 | dialog / modal |
| 表单 | form |
| 列表 | list |
| 表格 | table / grid |
| 树 | tree |
| 分页 | pagination / paging |
| 搜索 | search / query |
| 筛选 | filter |
| 排序 | sort / order |
| 导出 | export |
| 导入 | import |
| 上传 | upload |
| 下载 | download |
| 配置 | config / configuration |
| 设置 | settings / preferences |
| 版本 | version |
| 状态 | status / state |
| 类型 | type / kind |
| 标识符 | id / identifier |
| 名称 | name |
| 描述 | description / desc |
| 创建 | create / add |
| 更新 | update / modify |
| 删除 | delete / remove |
| 查询 | query / get / find |
| 保存 | save / persist |
| 批量 | batch |
| 单个 | single |
| 总数 | total / count |
| 摘要 | summary |
| 详情 | detail |
| 日志 | log |
| 错误 | error |
| 异常 | exception |
| 超时 | timeout |
| 重试 | retry |
| 回调 | callback |
| 钩子 | hook |
| 中间件 | middleware |
| 缓存 | cache |
| 队列 | queue |
| 会话 | session |
| 令牌 | token |
| 密钥 | key / secret |
| 加密 | encrypt |
| 解密 | decrypt |
| 校验 | validate / verify |
| 授权 | authorize |
| 认证 | authenticate |
| 通知 | notification / notify |
| 消息 | message / msg |
| 模板 | template |
| 渲染 | render |
| 解析 | parse |
| 序列化 | serialize |
| 反序列化 | deserialize |
| 监听器 | listener |
| 订阅 | subscribe |
| 发布（事件） | publish |
| 连接 | connection / connect |
| 断开 | disconnect |
| 重连 | reconnect |
| 请求 | request / req |
| 响应 | response / res |
| 标头 | header |
| 正文 | body |
| 参数 | params / parameters / args |
| 响应码 | statusCode / code |
| 路由 | route / router |
| 端点 | endpoint |
| 服务 | service |
| 仓库 | repository / repo |
| 控制器 | controller |
| 处理器 | handler |
| 提供者 | provider |
| 工厂 | factory |
| 构建器 | builder |
| 单例 | singleton |
| 代理 | proxy |
| 适配器 | adapter |
| 策略 | strategy |
| 观察者 | observer |
| 迭代器 | iterator |
| 访问者 | visitor |
| 装饰器 | decorator |

### 业务 / 管理

| 中文 | 英文 |
|------|------|
| 订单 | order |
| 支付 | payment / pay |
| 退款 | refund |
| 发票 | invoice |
| 商品 | product / item |
| 库存 | stock / inventory |
| 价格 | price |
| 折扣 | discount |
| 运费 | shipping / freight |
| 购物车 | cart |
| 结算 | checkout |
| 交易 | transaction |
| 账户 | account |
| 余额 | balance |
| 积分 | points / credits |
| 优惠券 | coupon |
| 会员 | member / vip |
| 等级 | level / grade |
| 工单 | ticket / workflow |
| 审批 | approval / review |
| 分配 | assign |
| 流转 | transfer / transition |

### 运维 / 系统

| 中文 | 英文 |
|------|------|
| 部署 | deploy / deployment |
| 发布（版本） | release |
| 回滚 | rollback |
| 监控 | monitor / monitoring |
| 告警 | alert / alarm |
| 指标 | metric |
| 追踪 | trace / tracing |
| 健康检查 | healthCheck / health |
| 均衡器 | balancer / loadBalancer |
| 副本 | replica |
| 节点 | node |
| 集群 | cluster |
| 命名空间 | namespace |
| 环境 | environment / env |
| 调试 | debug |
| 生产 | production / prod |
| 测试 | test / testing |
| 开发 | development / dev |

### 网络 / 通信

| 中文 | 英文 |
|------|------|
| 网络 | network / net |
| 协议 | protocol |
| 地址 | address / addr |
| 端口 | port |
| 域名 | domain |
| 子网 | subnet |
| 网关 | gateway |
| 防火墙 | firewall |
| 代理 | proxy |
| 隧道 | tunnel |
| 握手 | handshake |
| 心跳 | heartbeat |
| 延迟 | latency |
| 带宽 | bandwidth |
| 吞吐量 | throughput |

### 雷达 / 信号处理 — 缩略语

| 缩写 | 英文全称 | 中文 |
|------|---------|------|
| ADC | Analog to Digital Converter | 模数转换器 |
| AESA | Active Electronically Scanned Array | 有源电子扫描阵列 |
| AEWR | Airborne Early Warning Radar | 机载预警雷达 |
| AGC | Automatic Gain Control | 自动增益控制 |
| AoA | Angle of Arrival | 到达角 |
| ASAR | Advanced Synthetic Aperture Radar | 先进合成孔径雷达 |
| ATR | Automatic Target Recognition | 自动目标识别 |
| BPF | Bandpass Filter | 带通滤波器 |
| BPM | Binary Phase Modulation | 二进制相位调制 |
| CFAR | Constant False Alarm Rate | 恒定虚警率（恒虚警） |
| CPI | Coherent Processing Interval | 相干处理间隔 |
| DBF | Digital Beam Forming | 数字波束形成 |
| DDMA | Doppler Division Multiple Access | 多普勒分多址 |
| DDS | Direct Digital Synthesis | 直接数字合成 |
| DOA | Direction of Arrival | 波达方向 |
| DRFM | Digital Radio Frequency Memory | 数字射频存储器 |
| DSP | Digital Signal Processor | 数字信号处理器 |
| EA | Electronic Attack | 电子攻击 |
| ECM | Electronic Counter Measures | 电子对抗 |
| ECCM | Electronic Counter Counter Measures | 电子反对抗 |
| ELINT | Electronic Intelligence | 电子情报 |
| EP | Electronic Protection | 电子防护 |
| ERP/EIRP | Effective (Isotropic) Radiated Power | 有效(全向)辐射功率 |
| ES | Electronic Support | 电子支援 |
| ESM | Electronic Support Measures | 电子支援措施 |
| EW | Electronic Warfare | 电子战 |
| FDOA | Frequency Difference Of Arrival | 到达频差 |
| FFT | Fast Fourier Transform | 快速傅里叶变换 |
| FMCW | Frequency Modulation Continuous Wave | 调频连续波 |
| FPGA | Field Programmable Gate Arrays | 现场可编程门阵列 |
| GMTI | Ground Moving Target Indication | 地面动目标指示 |
| GPS | Global Position System | 全球定位系统 |
| GPU | Graphic Processing Unit | 图形处理单元 |
| HWA | Hardware Accelerator | 硬件加速器 |
| HRR | High Range Resolution | 高距离分辨率 |
| IF | Intermediate Frequency | 中频 |
| IFF | Identification Friend or Foe | 敌我识别 |
| InSAR/IFSAR | Interferometric Synthetic Aperture Radar | 干涉合成孔径雷达 |
| ISAR | Inverse Synthetic Aperture Radar | 逆合成孔径雷达 |
| ISR | Intelligence Surveillance Reconnaissance | 情报监视侦察 |
| LFM | Linear Frequency Modulation | 线性调频信号 |
| LiDAR | Light Detection And Ranging | 激光雷达 |
| LPI | Low Probability of Intercept | 低截获概率 |
| MFR | Multi Function Radar | 多功能雷达 |
| MIMO | Multiple Input Multiple Output | 多输入多输出 |
| MIMO-SAR | — | 多输入多输出合成孔径雷达 |
| ML | Machine Learning | 机器学习 |
| MMW Radar | Millimeter Wave Radar | 毫米波雷达 |
| MTI | Moving Target Indication | 动目标指示 |
| MUSIC | Multiple Signal Classification | 多重信号分类 |
| NGJ | Next Generation Jammer | 下一代干扰机 |
| OTH | Over the Horizon | 天波超视距雷达 |
| PBR | Passive Bistatic Radar | 无源双基地雷达 |
| PRF | Pulse Repetition Frequency | 脉冲重复频率 |
| PRI | Pulse Repetition Interval | 脉冲重复间隔 |
| RCS | Radar Cross Section | 雷达散射截面积 |
| RDA | Range Doppler Algorithm | 距离多普勒算法 |
| RGPO | Range Gate Pull Off | 距离拖引 |
| RWR | Radar Warning Receiver | 雷达告警接收机 |
| SAR | Synthetic Aperture Radar | 合成孔径雷达 |
| SBX | Sea-Based X-band Radar | 海基X波段雷达 |
| SLC | Sidelobe Canceler | 旁瓣对消器 |
| SNR | Signal-to-Noise Ratio | 信噪比 |
| SOJ | Stand Off Jamming | 支援干扰 |
| SPJ | Self Protection Jamming | 自卫干扰 |
| STAP | Space Time Adaptive Processing | 空时自适应处理 |
| STAR | Simultaneous Transmit and Receive | 同时发射和接收 |
| SWT | Search While Track | 边搜索边跟踪 |
| TDM | Time Division Multiplexing | 时分复用 |
| TDOA | Time Difference Of Arrival | 到达时差 |
| TOF | Time of Flight | 飞行时间 |
| TWS | Track While Scan | 边跟踪边扫描 |
| TWT | Traveling Wave Tube | 行波管 |
| UAS/UAV | Unmanned Aircraft System / Unmanned Aerial Vehicle | 无人机系统 |

### 雷达 / 信号处理 — 通用术语

| 中文 | 英文 |
|------|------|
| 雷达 | radar |
| 雷达散射截面积 | radarCrossSection / rcs |
| 目标识别 | targetIdentification / targetRecognition |
| 目标检测 | targetDetection |
| 目标跟踪 | targetTracking / tracking |
| 杂波 | clutter |
| 噪声 | noise |
| 噪声基底 | noiseFloor |
| 信号 | signal |
| 分辨率 | resolution |
| 距离分辨率 | rangeResolution |
| 角度分辨率 | angularResolution |
| 速度分辨率 | velocityResolution |
| 发射机 | transmitter / tx |
| 接收机 | receiver / rx |
| 天线 | antenna |
| 天线阵列 | antennaArray / array |
| 相控阵 | phasedArray |
| 波束 | beam |
| 波束形成 | beamforming / beamForming |
| 数字波束形成 | digitalBeamforming / dbf |
| 旁瓣 | sidelobe |
| 主瓣 | mainlobe |
| 脉冲 | pulse |
| 脉冲压缩 | pulseCompression |
| 脉冲重复频率 | pulseRepetitionFrequency / prf |
| 脉冲重复间隔 | pulseRepetitionInterval / pri |
| 啁啾 | chirp |
| 线性调频 | linearFrequencyModulation / lfm |
| 调频连续波 | fmcw |
| 多普勒效应 | dopplerEffect |
| 多普勒频移 | dopplerShift |
| 距离门 | rangeGate / rangeBin |
| 距离多普勒 | rangeDoppler |
| 距离角 | rangeAngle |
| 距离像 | rangeProfile |
| 合成孔径 | syntheticAperture |
| 逆合成孔径 | inverseSyntheticAperture |
| 干涉 | interferometry / interferometric |
| 调制 | modulation |
| 解调 | demodulation |
| 混频器 | mixer |
| 中频 | intermediateFrequency / if |
| 正交 | quadrature / iq |
| 基带 | baseband |
| 采样 | sampling |
| 量化 | quantization |
| 频谱 | spectrum / frequencySpectrum |
| 功率谱 | powerSpectrum |
| 幅度 | amplitude / magnitude |
| 相位 | phase |
| 频率 | frequency |
| 带宽 | bandwidth |
| 扫频带宽 | sweepBandwidth |
| 频率斜率 | frequencySlope |
| 波长 | wavelength |
| 波数 | wavenumber |
| 极化 | polarization |
| 校准 | calibration / cal |
| 补偿 | compensation |
| 相干 | coherent |
| 非相干 | incoherent / noncoherent |
| 积累 | integration |
| 相参积累 | coherentIntegration |
| 非相参积累 | noncoherentIntegration |
| 检测 | detection |
| 恒虚警检测 | cfar / constantFalseAlarmRate |
| 阈值 | threshold |
| 峰值 | peak |
| 峰值检测 | peakDetection |
| 虚警 | falseAlarm |
| 漏警 | missedDetection |
| 点迹 | plot / detectionPoint |
| 航迹 | track |
| 航迹起始 | trackInitiation |
| 航迹关联 | trackAssociation |
| 滤波 | filter / filtering |
| 卡尔曼滤波 | kalmanFilter |
| 粒子滤波 | particleFilter |
| 数据关联 | dataAssociation |
| 最近邻 | nearestNeighbor |
| 概率数据关联 | probabilisticDataAssociation / pda |
| 跟踪 | tracking |
| 边扫描边跟踪 | trackWhileScan / tws |
| 搜索 | search / query |
| 截获 | acquisition |
| 电子战 | electronicWarfare / ew |
| 电子对抗 | electronicCountermeasures / ecm |
| 电子反对抗 | electronicCounterCountermeasures / eccm |
| 电子支援 | electronicSupport / es |
| 电子攻击 | electronicAttack / ea |
| 电子防护 | electronicProtection / ep |
| 干扰 | jamming |
| 压制干扰 | barrageJamming / blanketJamming |
| 欺骗干扰 | deceptionJamming |
| 自卫干扰 | selfProtectionJamming / spj |
| 支援干扰 | standOffJamming / soj |
| 低截获概率 | lowProbabilityOfIntercept / lpi |
| 信号处理 | signalProcessing |
| 数字信号处理 | digitalSignalProcessing / dsp |
| 空时自适应处理 | spaceTimeAdaptiveProcessing / stap |
| 动目标指示 | movingTargetIndication / mti |
| 地面动目标指示 | groundMovingTargetIndication / gmti |
| 到达角估计 | angleOfArrivalEstimation / aoaEstimation |
| 波达方向 | directionOfArrival / doa |
| 到达时差 | timeDifferenceOfArrival / tdoa |
| 到达频差 | frequencyDifferenceOfArrival / fdoa |
| 测距 | ranging / rangeMeasurement |
| 测速 | velocityMeasurement |
| 测角 | angleMeasurement |
| 链路预算 | linkBudget |
| 信噪比 | signalToNoiseRatio / snr |
| 信杂比 | signalToClutterRatio / scr |
| 信干比 | signalToInterferenceRatio / sir |
| 最大不模糊速度 | maximumUnambiguousVelocity |
| 最大作用距离 | maximumRange |
| 探测范围 | detectionRange / coverage |
| 虚警率 | falseAlarmRate / far |
| 检测概率 | detectionProbability / pd |
| 数据率 | dataRate / updateRate |

### 激光雷达 / LiDAR — 缩略语

| 缩写 | 英文全称 | 中文 |
|------|---------|------|
| APD | Avalanche Photodiode | 雪崩光电二极管 |
| DBSD | Digital Beam Steering Device | 数字波束转向设备 |
| DEM | Digital Elevation Model | 数字高程模型 |
| Flash | Flash Array | Flash 面阵 |
| FLANN | Fast Library for Approximate Nearest Neighbors | 快速近似最近邻搜索库 |
| FMCW | Frequency Modulated Continuous Wave | 调频连续波 |
| FoV | Field of View | 视场角 |
| HDL | High Definition Lidar | 高分辨率激光雷达 |
| HOME | Height of Median Energy | 半波能量高度 |
| LAS | — | 点云标准二进制格式 |
| LiDAR | Light Detection and Ranging | 激光雷达 |
| LII | Laser Interception Index | 激光截获指数 |
| MEMS | Micro-Electro-Mechanical Systems | 微机电系统 |
| OpenCV | Open Source Computer Vision Library | 开源计算机视觉库 |
| OpenGL | Open Graphics Library | 开放图形库 |
| PCD | Point Cloud Data | 点云数据文件格式 |
| PCL | Point Cloud Library | 点云库 |
| PCAP | — | 网络数据包捕获文件格式 |
| RAE | Range-Azimuth-Elevation | 距离-方位角-俯仰角模型 |
| ROI | Region of Interest | 感兴趣区域 |
| ROS | Robot Operating System | 机器人操作系统 |
| SLAM | Simultaneous Localization and Mapping | 同时定位与地图构建 |
| SPAD | Single Photon Avalanche Diode | 单光子雪崩二极管 |
| SiPM | Silicon Photomultiplier | 硅光电倍增管 |
| TOF | Time of Flight | 飞行时间 |
| VTK | Visualization Toolkit | 可视化工具包 |

### 激光雷达 / LiDAR — 通用术语

| 中文 | 英文 |
|------|------|
| 激光雷达 | lidar |
| 激光 | laser |
| 光斑 | spot |
| 激光波长 | laserWavelength |
| 反射率 | reflectivity |
| 反射强度 | intensity |
| 回波强度 | returnIntensity |
| 多回波 | multiReturn |
| 激光回波 | laserReturn / laserEcho |
| 飞行时间 | timeOfFlight / tof |
| 脉冲测量 | pulseMeasurement |
| 激光扫描 | laserScanning |
| 离散回波 | discreteReturn |
| 全波形 | fullWaveform |
| 波形分解 | waveformDecomposition |
| 波形特征提取 | waveformFeatureExtraction |
| 机械式激光雷达 | mechanicalLidar |
| 固态激光雷达 | solidStateLidar |
| 混合固态 | memsLidar |
| 光相控阵 | opticalPhasedArray |
| 单线激光雷达 | singleLineLidar |
| 多线激光雷达 | multiLineLidar |
| 视场角 | fieldOfView / fov |
| 垂直视场角 | verticalFov |
| 水平视场角 | horizontalFov |
| 角度分辨率 | angularResolution |
| 方位角 | azimuth |
| 俯仰角 | elevation |
| 点云 | pointCloud |
| 点云密度 | pointCloudDensity |
| 点云特征 | pointCloudFeature |
| 点云滤波 | pointCloudFiltering |
| 点云分类 | pointCloudClassification |
| 分割 | segmentation |
| 点云分割 | pointCloudSegmentation |
| 点云拟合 | pointCloudFitting |
| 点云简化 | pointCloudSimplification |
| 点云精化 | pointCloudRefinement |
| 点云去噪 | pointCloudDenoising |
| 点云配准 | pointCloudRegistration |
| 深度图 | depthMap |
| 三维重建 | 3dReconstruction |
| 同时定位与地图构建 | slam |
| 局部特征描述 | localFeatureDescription |
| 全局特征描述 | globalFeatureDescription |
| 有序点云 | orderedPointCloud |
| 无序点云 | unorderedPointCloud |
| 监督分类 | supervisedClassification |
| 无监督分类 | unsupervisedClassification |
| 坐标 | coordinate |
| 笛卡尔坐标 | cartesianCoordinate |
| 球坐标 | sphericalCoordinate |
| 测远能力 | rangeCapability |
| 最近测量距离 | minimumMeasurementDistance |
| 盲区 | blindZone |
| 精度 | precision |
| 准度 | accuracy |
| 高反 | highReflectivity |
| 串扰 | crosstalk |
| 拖尾 | tail / signalTail |
| 鬼影 | ghost / ghosting |
| 人眼安全 | eyeSafety |
| 雪崩光电二极管 | apd |
| 单光子雪崩二极管 | spad |
| 硅光电倍增管 | sipm |

### 气象 / 大气科学 — 缩略语

| 缩写 | 英文全称 | 中文 |
|------|---------|------|
| CCC | Central Cold Cover | 中心冷云盖 |
| CDO | Central Dense Overcast | 中心密集云层区 |
| HKO | Hong Kong Observatory | 香港天文台 |
| ITCZ | Intertropical Convergence Zone | 热带辐合区 / 热带辐合带 |
| JMA | Japan Meteorology Agency | 日本气象厅 |
| JTWC | Joint Typhoon Warning Center | 联合台风警报中心 |
| LLCC | Low Level Circulation Centre | 低层环流中心 |
| STS | Severe Tropical Storm | 强烈热带风暴 |
| TC | Tropical Cyclone | 热带气旋 |
| TD | Tropical Depression | 热带低气压 |
| TS | Tropical Storm | 热带风暴 |
| TUTT | Tropical Upper Troposphere Trough | 热带对流层上部槽 |
| TY | Typhoon | 台风 |
| WMO | World Meteorological Organization | 世界气象组织 |
| WWB | Westerly Wind Bursts | 西风爆发 |

### 气象 / 大气科学 — 通用术语

| 中文 | 英文 |
|------|------|
| 气象学 | meteorology |
| 气象卫星 | meteorologicalSatellite / weatherSatellite |
| 气象台 | observatory |
| 大气 | atmosphere |
| 气候 | climate |
| 温度 | temperature |
| 湿度 | moisture / humidity |
| 气压 | airPressure / pressure |
| 降水 | precipitation |
| 雨 | rain |
| 雪 | snow |
| 冰雹 | hail |
| 霜 | frost |
| 露 | dew |
| 酸雨 | acidRain |
| 冻雨 | freezingRain |
| 雨夹雪 | sleet |
| 暴风雨 | rainstorm |
| 暴风雪 | blizzard |
| 云 | cloud |
| 云卷风眼 | bandingEye |
| 漏斗云 | funnelCloud |
| 对流 | convection |
| 辐合 | convergence |
| 辐散 | divergence |
| 风 | wind |
| 风速 | windSpeed |
| 风向 | windDirection |
| 阵风 | gust |
| 阵风锋 | gustFront |
| 疾风 | gale |
| 狂风 | squall |
| 无风 | calm |
| 无风带 | doldrums / horseLatitudes |
| 信风 | tradeWind |
| 东风带 | easterlies |
| 西风带 | westerlies |
| 季风 | monsoon |
| 季风槽 | monsoonTrough |
| 季风涡旋 | monsoonGyre |
| 海风 | seaBreeze |
| 陆风 | landBreeze |
| 热成风 | thermalWind |
| 地转风 | geostrophicWind |
| 梯度风 | gradientWind |
| 引导气流 | steeringFlow |
| 风切变 | windShear |
| 锋面 | front |
| 冷锋 | coldFront |
| 暖锋 | warmFront |
| 静止锋 | stationaryFront |
| 锢囚锋 | occludedFront |
| 气旋 | cyclone |
| 反气旋 | antiCyclone |
| 温带气旋 | temperateCyclone |
| 热带气旋 | tropicalCyclone |
| 台风 | typhoon |
| 飓风 | hurricane |
| 旋风 | cyclone |
| 热带扰动 | tropicalDisturbance |
| 热带低气压 | tropicalDepression |
| 热带风暴 | tropicalStorm |
| 超级台风 | superTyphoon |
| 副热带气旋 | subTropicalCyclone |
| 副热带高压 | subTropicalHigh |
| 低气压 | lowPressure / depression |
| 高气压 | highPressure / antiCyclone |
| 阻塞高压 | blockingHigh |
| 割离低气压 | cutOffLow |
| 槽 | trough |
| 脊 | ridge |
| 切变线 | shearLine |
| 涡度 | vorticity |
| 涡旋 | vortex |
| 风眼 | eye |
| 风眼墙 | eyeWall |
| 眼墙置换 | eyeWallReplacement |
| 德沃夏克分析法 | dvorakAnalysis |
| 藤原效应 | fujiwharaEffect |
| 风暴潮 | stormSurge |
| 寒潮 | coldSpell |
| 梅雨 | meiyu / plumRain |
| 中尺度 | mesoscale |
| 对流层 | troposphere |
| 逆温层 | temperatureInversion |
| 斜压 | baroclinic |
| 正压 | barotropic |
| 静力稳定度 | staticStability |
| 波浪 | wave |
| 东风波 | easterlyWave |
| 西风波动 | westerlyWave |
| 气压梯度力 | pressureGradientForce |
| 科氏力 | coriolisForce |
| 波福风力等级 | beaufortScale |
| 爆发性增强 | explosiveDeepening / rapidDeepening |
| 辐合带 | convergenceZone |
| 赤道无风带 | doldrums |
| 全球变暖 | globalWarming |
| 厄尔尼诺 | elNino |
| 拉尼娜 | laNina |

### 补充术语（来自标准 PDF）

#### 缩略语

| 缩写 | 英文全称 | 中文 |
|------|---------|------|
| AFC | Automatic Frequency Control | 自频控 / 自动频率控制 |
| CAPPI | Constant Altitude Plan Position Indicator | 等高平面位置显示 |
| EMC | Electromagnetic Compatibility | 电磁兼容性 |
| EMI | Electromagnetic Interference | 电磁干扰 |
| ETPPI | Echo Top PPI | 回波顶高显示 |
| HPF | Hail Potential Forecast | 冰雹潜势预测 |
| MDS | Minimum Detectable Signal | 最小可测信号 |
| MTBF | Mean Time Between Failures | 平均故障间隔时间 |
| MTTR | Mean Time To Repair | 平均修复时间 |
| NF | Noise Figure | 噪声系数 |
| OD | Optical Density | 光密度 |
| PA | Precipitation Accumulation | 雨量累积 |
| PMT | Photomultiplier Tube | 光电倍增管 |
| PRF1 / PRF2 | Pulse Repetition Frequency 1/2 | 第一/第二脉冲重复频率 |
| RFI | Radio Frequency Interference | 射频干扰 |
| RZ | Rainfall Rate | 雨强 |
| SQI | Signal Quality Index | 信号质量因子 |
| STC | Sensitivity Time Control | 灵敏度时间控制 |
| SWP | Severe Weather Probability | 强天气概率 |
| TI | Turbulence Intensity | 湍流强度 |
| VAD | Velocity Azimuth Display | 速度方位显示 |
| VCS | Vertical Cross Section | 任意垂直剖面 |
| VIL | Vertically Integrated Liquid | 垂直累积液态含水量 |
| VSWR | Voltage Standing Wave Ratio | 电压驻波比 |

#### 通用术语

| 中文 | 英文 |
|------|------|
| 天气雷达 | weatherRadar |
| 灾害性天气 | severeWeather |
| 天线罩 | radome |
| 伺服驱动 | servoDrive |
| 信号处理器 | signalProcessor |
| 用户终端 | userTerminal |
| 扇区扫描 | sectorScan |
| 传输波导 | waveguide |
| 极坐标 | polarCoordinates |
| 噪声阈值 | noiseThreshold |
| 退模糊 | dealiasing |
| 地物杂波 | groundClutter |
| 标校 | calibration |
| 在线标校 | onlineCalibration |
| 太阳法标校 | solarCalibration |
| 距离订正 | rangeCorrection |
| 雨衰补偿 | rainAttenuationCompensation |
| 二次回波 | secondTripEcho |
| 二次产品 | secondaryProduct |
| 图像产品 | imageProduct |
| 探空气球 | soundingBalloon |
| 探空仪 | radiosonde |
| 回答器 | transponder |
| 风廓线雷达 | windProfiler |
| 风廓线 | windProfile |
| 风切变指数 | windShearIndex |
| 湍流强度 | turbulenceIntensity |
| 相干积累 | coherentIntegration |
| 脉冲压缩 | pulseCompression |
| 改善因子 | improvementFactor |
| 故障检测率 | faultDetectionRate |
| 虚警率 | falseAlarmRate |
| 开机时间 | startupTime |
| 架设撤收时间 | deploymentTeardownTime |
| 连续工作时间 | continuousOperationTime |
| 维修性 | maintainability |
| 人机工程 | ergonomics |
| 互换性 | interchangeability |
| 环境适应性 | environmentalAdaptability |
| 后向散射信号 | backscatterSignal |
| 弹性散射 | elasticScattering |
| 瑞利散射 | rayleighScattering |
| 米散射 | mieScattering |
| 拉曼散射 | ramanScattering |
| 气溶胶消光系数 | aerosolExtinctionCoefficient |
| 气溶胶后向散射系数 | aerosolBackscatterCoefficient |
| 退偏振比 | depolarizationRatio |
| 光子计数 | photonCounting |
| 能见度 | visibility |
| 光学厚度 | opticalDepth |
| 盲区距离 | blindZoneDistance |
| 带外抑制 | outOfBandRejection |
| 视场角 | fieldOfView |
| 发散角 | divergenceAngle |
| 空间分辨率 | spatialResolution |
| 时间分辨率 | temporalResolution |
| 测风塔 | windMeasurementTower |
| 杯式测风仪 | cupAnemometer |
| 相关系数 | correlationCoefficient |
| 拟合优度 | goodnessOfFit |
| 绝对误差 | absoluteError |
| 相对误差 | relativeError |
| 幂指数 | powerLawExponent |
| 平行对比观测 | parallelComparisonObs |
| 有效完整率 | validCompletenessRate |
| 数据可靠性 | dataReliability |
| 地面杂波图 | groundClutterMap |
| 速度退模糊 | velocityDealiasing |
| 微波辐射 | microwaveRadiation |
| 功率容量 | powerCapacity |
| 接收机灵敏度 | receiverSensitivity |
| 动态范围 | dynamicRange |
| 系统动态范围 | systemDynamicRange |
| 对数动态范围 | logarithmicDynamicRange |
| 电压驻波比 | voltageStandingWaveRatio |
| 收发隔离度 | transmitReceiveIsolation |
| 定向灵敏度 | directionalSensitivity |
| 脉冲功率 | pulsePower |
| 射频脉冲频谱 | rfPulseSpectrum |
| 频率稳定度 | frequencyStability |
| 相位稳定度 | phaseStability |
| 光电转换 | photoelectricConversion |
| 望远镜口径 | telescopeAperture |
| 滤光片 | opticalFilter |
| 太阳光抑制比 | sunlightSuppressionRatio |
| 几何重叠因子 | geometricOverlapFactor |
| 偏振校正常数 | polarizationCalibrationConstant |
| 数据采集器 | dataAcquisition |

## 标题命名规范

### 编号格式

| 元素 | 推荐格式 | 不推荐 |
|------|---------|-------|
| 图片 | `图X` / `Fig.X` | `Fig X`(少点)、`figure X`(全小写) |
| 表格 | `表X` / `Table X` | `Tab.X`、`TABLE X` |
| 公式 | `式(X)` / `Eq.(X)` | `公式X`、`equation X` |
| 算法 | `算法X` / `Algorithm X` | `ALGORITHM X` |
| 编号层级 | `图1-a` / `Fig.1(a)` | `图1a`(无分隔)、`Fig1a` |

### 中英文对照格式

**推荐：** 中文标题居上 / 英文翻译居下
```
图1 数据融合处理流程
Fig.1 Data Fusion Processing Flow
```

**或单行中英文并用：**
```
图1 数据融合处理流程 / Fig.1 Data Fusion Processing Flow
```

### 英文大小写规则

**规则：标题式大写（Title Case）**

- 实词（名词、动词、形容词、副词）首字母**大写**
- 虚词（冠词 a/an/the、介词 in/on/at/of/for/to/with/by/from/between/about/through/without/under/over/across/after/before 等、连词 and/or/but/nor/yet/so）首字母**小写**
- 无论词性，**首词**和**末词**必须大写
- 专有名词（雷达型号、缩写等）保留原有大小写

| 示例 | 说明 |
|------|------|
| `Fig.1 Data Fusion Processing Flow` | ✅ 正确：实词大写，无介词 |
| `Fig.1 Analysis of Radar Echo Data` | ✅ 正确：首词大写，of 小写，末词大写 |
| `Fig.1 The Data Processing and Result` | ✅ 正确：The 首词大写，末词 Result 大写 |
| `Fig.1 analysis of radar echo data` | ❌ 不应全小写 |
| `Fig.1 Data Fusion Processing flow` | ❌ 末词 flow 应大写 Flow |

> 英文标题中**词与词之间必须有空格**断句，不要堆叠成连续字符串。
> 英文标题中不要出现拼音或中文（除非是引用中文文献名称）。

### 中英文翻译术语一致性

- 标题中的中文术语 → 英文翻译必须与正文、代码中的翻译一致
- 多义词需根据上下文选择正确的英文（如"相关"在信号处理中是 `correlation`，在管理中可能是 `relevant`）
- 使用 `/variable-naming dict` 查询推荐术语

## 命名规范建议

| 语言 | 变量/函数 | 常量 | 类型/接口 | 文件名 |
|------|-----------|------|-----------|--------|
| Go | `camelCase` | `ALL_CAPS` | `PascalCase` | `snake_case` |
| Python | `snake_case` | `ALL_CAPS` | `PascalCase` | `snake_case` |
| JavaScript/TS | `camelCase` | `ALL_CAPS` | `PascalCase` | `kebab-case` |
| Java | `camelCase` | `ALL_CAPS` | `PascalCase` | `PascalCase` |
| Rust | `snake_case` | `ALL_CAPS` | `PascalCase` | `snake_case` |
| C/C++ | `snake_case` | `ALL_CAPS` | `PascalCase` | `snake_case` |

## 技术说明

- **检测方法**：正则 + 拼音检测（用 `pypinyin` 或内置拼音表反向匹配）
- **重命名方式**：通过 Edit 工具逐个执行，确保不破坏代码
- **词典扩展**：用户通过 `/variable-naming add` 添加的条目保存在项目目录的 `.claude/variable-naming-dict.json` 中
