---
name: mc-mod-workshop
displayName: MC模组工坊
version: 1.2.0
description: "Minecraft Java 版模组（mod）开发助手——只管「写一个 mod」这一件事，不碰整合包。覆盖：套官方模板建工程、注册物品/方块/方块实体/创造模式物品栏、数据生成（配方/战利品表/标签/模型）、网络包、配置、Mixin、客户端渲染与 HUD、自定义生物与实体模型、自定义世界生成器与维度高度、按键系统改造、程序化生成/批量变色的纹理、抄改原版素材、版本合规检查、低版本 mod 迁移升级、专用服务端 + RCON 自动化验证、🔴 进游戏实测门禁（起真实客户端→自动建世界进世界→推进 tick→截图→扫日志，退出码 0 才许交付）。支持复杂任务的多子代理并行编排（任务 DAG / 文件所有权 / 主代理集成验收）。支持 1.20.1 / 1.21.x / 26.x 三代写法与 Forge / NeoForge / Fabric / Architectury 四大平台。触发词：多代理、子代理、任务拆分、并行开发、我的世界mod、MC模组、minecraft mod、写个mod、做个mod、模组开发、mod开发、Forge模组、NeoForge模组、Fabric模组、注册物品、注册方块、方块实体、数据生成、datagen、网络包、Mixin、自定义生物、实体模型、HUD、自定义世界生成器、改地形、维度高度、无限高、按键改造、模组迁移、移植mod、把mod升级到、1.20.1迁移、模组贴图、生成纹理、纹理变色、抄原版素材、模组改名、modid、进游戏实测、进游戏验证、能不能进游戏、起不来、进不去游戏、闪退、启动崩溃。"
agent_created: true
slug: mc-mod-workshop
license: MIT
tags: [minecraft, mod, 模组开发, Forge, NeoForge, Fabric, Architectury, 注册物品, 数据生成, Mixin, 自定义生物, 世界生成, 维度高度, 纹理, 迁移升级, RCON验证]
summary: Minecraft Java 版模组（mod）开发助手。只管「写一个 mod」：建工程、写内容、补资源、检查、构建、真机验证、迁移升级。不覆盖整合包相关（体检/清单/下载/打包/任务书/汉化）——那些走 MC一站式工坊。
---

# Minecraft 模组工坊（MC模组工坊）

**只管一件事：写一个能编译、能跑、能进游戏的 Minecraft Java 版模组。**

支持 1.20.1 / 1.21.x / 26.x 三代写法与 Forge / NeoForge / Fabric / Architectury 四大平台。

> **边界（重要）**：本 skill **不覆盖整合包相关的一切** —— 整合包体检、模组清单与下载、
> 依赖闭包、打包分发（CF zip / mrpack）、FTB 任务书、汉化、整合包实例改造。
> 那些走 **MC一站式工坊**（`mc-one-stop-workshop`）。
> 只有「mod 源码工程」进本 skill；**实例目录（只有 `mods/` `config/` 的那种）不进**。

---

## 🧭 先路由：我要做什么

| 用户要什么 | 走这条 | 用哪个脚本 | 章节 |
|---|---|---|---|
| **写一个新的 mod**（从零建工程） | 定版本/加载器 → 🔴 **出规划书（§1.6，独立阶段）** → 建工程 → 写内容 → 补资源 → 检查 → 构建 → 真机验证 → 🔴 **进游戏实测门禁（§8）** | `新建工程.py` → `检查工程.py` → `游戏内测试.py smoke` | 「工作流」 |
| **任务繁重、能拆成多个独立工作包** | 主代理先锁公共契约 → 任务板查 DAG 与文件冲突 → 并行派 2–4 个子代理 → 主代理集成并跑整体验证；小任务、同文件热区、共享游戏环境不并行 | `多代理任务板.py check <计划.json>` | **工作流 §1.7 + `references/多代理协作.md`** |
| 🔴 **交付前：证明它真能进游戏** | **起真实客户端 → 自动建世界进世界 → 推进 tick → 截图 → 扫日志**，退出码 0 才许谈交付；老版本降级到「窗口 + 主菜单标志行」通道 | `游戏内测试.py smoke --project <工程>` | **「§8 进游戏实测门禁」** |
| **加个物品 / 方块 / 方块实体 / 创造栏** | 查代码片段对应节 → 注册 + 补资源（模型/贴图/lang） | `检查工程.py` | `references/代码片段.md` §1-§4 |
| **加个自定义生物 / 实体模型** | 先读模型铁律（脚底 y=24）→ 写注册 → 离线预览自检 | `实体模型预览.py` | `references/代码片段.md` §15-§16 |
| **画贴图 / 一套只差颜色的物品** | 程序化生成 或 底图变色批量出 | `像素纹理生成.py` / `纹理变色.py` | 「工作流」§4.1 / §4.2 |
| **想抄原版素材**（贴图/生物模型/寻路/配方/语言） | 从原版 client jar 取料 → 改 → 用 | `原版资源提取.py` → `纹理变色.py` | `references/改造原版复用.md` |
| **写数据生成（datagen）** | 配方/战利品表/标签/模型代码化产出 | — | `references/代码片段.md` §5 |
| **写网络包 / 配置 / 事件 / Mixin** | 照片段写，注意版本写法 | — | `references/代码片段.md` §6-§9 |
| **写 HUD / 自定义渲染，怕进游戏才崩** | 静态核对 pipeline×RenderState 顶点元素 → 再写 | `渲染API核对.py` | 「工作流」§7⑤ + `代码片段.md` §17 |
| **按个键开自己的面板**（自定义 Screen、每行一个按钮、点按钮改服务端状态） | `extractRenderState` 覆写 + **必须调 super**（否则按钮不显示且不报错）；数据用 AttachmentType 存档；Mixin 用 export 验证 | 自写 javap 契约断言 | `代码片段.md` **§19** |
| **写一个改地形的 mod**（自定义世界生成 / 世界预设） | 注册 chunk generator → 数据包世界预设 → 挂 preset tag → 起服务端实地取证；**不想写 Java 时走数据包级 density_function**（改完重启就见效，先在这里试形态） | `新建工程.py` → `rcon.py` | `references/26.x-自定义世界生成器.md` + `references/26.x-数据包地形生成.md` |
| **改维度高度 / 放开垂直边界**（无限高、地底无限深） | 先讲清硬上限 4064 格 → 覆盖 `dimension_type` → `noise_settings` 别乱动 | `按深度统计方块.py` | `references/26.x-维度高度与垂直边界.md` |
| **改雾效**（发光雾/雾遮挡视线/地下起雾/雾颜色） | 雾不发光，必须拆「服务端隐藏夜视 + 客户端 `ViewportEvent.RenderFog`」两半；26.1 雾系统重写过但事件没废；`FogData` 拆成五段，别动 `renderDistanceStart/End` | — | `references/26.x-雾效与深层结构改法.md` |
| **让实心方块透天光**（外壳当太阳 / 改了 `getLightBlock` 却不生效 / 内部一直是黑的） | 天光穿透**三件套缺一不可**：属性 `.noOcclusion()`（**真开关**，别删）+ 覆盖 `getLightBlock→0` + `propagatesSkylightDown→true`；⚠️ 删 `noOcclusion()` 会**零报错**地全断；⚠️ 天光**烘焙在区块存档里**，效果必须开新世界/探新区验证；也不必把 `getShape()` 改薄（那会掉穿方块） | 自写逐格剖面命令（打印 天光/块光/方块id）+ 状态断言 `canOcclude==false` | ⭐ **`references/方块光照与透光.md`** |
| **给主世界加自定义群系** | 26.x 只剩 Mixin 一条路（接管宿主群系参数表条目） | — | `references/26.x-自定义群系注入.md` |
| **改按键行为**（组合键 / 键位扩容 / 屏蔽某个键） | 注入 `KeyMapping.forAllKeyMappings` 单点覆盖 → 交付前跑注入契约检查 | 自写 javap 契约断言 | `references/26.x-按键系统改造.md` |
| **做「升级 / 觉醒 / 技能 / 材料」系统**（等级+属性+扣料） | 等级存数据附件当唯一真相 → 属性用 AttributeModifier 运行时挂；枚举 ordinal **就是存档格式**，只能追加在末尾；**等级映射成属性数值前先查属性的硬上限（§10）**——`*_interaction_range` 之类是 RangedAttribute，max 64，翻了 4 次就撞天花板、最后几级会变成"升了没变化"的空壳级别 | 自写材料表断言脚本 | `references/26.x-自定义升级与材料系统.md` **§10** |
| **把平衡数值做成玩家可编辑**（存档内改 / 全局预设 / 主菜单模组设置界面） | 三级解析：存档规则 → 全局预设 → 出厂设置；26.1 用 `SavedDataType` + `SavedDataStorage.computeIfAbsent`；规则整包走 JSON 同步；`IConfigScreenFactory` 注册主菜单设置界面（客户端专用，必须判 dist） | `交叉同步.py --audit` | ⭐ `references/代码片段.md` §22 |
| **自定义容器界面**（嫌画的界面和原版箱子「颜色格格不入」） | 别调色号 —— **切原版 GUI 图集拼装**（blit 签名 / 图集逐像素实测结构 / 分区纯色占比量化「界面烂」） | 离线预览必跑 | `references/26.x-自定义容器界面贴图拼装.md` |
| **做连锁挖矿 / 矿物嗅探 / 方块自动分类**（「只要是矿物就认」） | 三级判据：**标签 → 名字 → 掉落物**，任何一级命中就算；客户端与服务端必须共用同一个分类器 | 运行时探针命令 + RCON 批量验证 | `references/26.x-跨模组方块识别与运行时探针.md` |
| **做附魔**（自定义附魔 / 「附魔台里出不来我的附魔」/「怎么读附魔等级」/「堆叠上限」） | 附魔是 **datapack JSON**（`data/<ns>/enchantment/*.json`）不是代码注册 → 代码里只能持 `ResourceKey`，等级用 `EnchantmentHelper.getItemEnchantmentLevel` **逐槽读**（= 「穿上 / 手持才生效」）→ 属性类效果纯 JSON（`minecraft:attributes` / `damage_protection` / `ammo_use`），`"effects":{}` 合法 → 附魔台入口 = 追加 `#minecraft:non_treasure`（默认合并）| `检查工程.py` + RCON `summon` 验 id | ⭐ **`references/代码片段.md` §21** |
| **把旧 mod 升级/移植到新版本** | 盘点 → 出路线图 → 逐级改 | `检查工程.py` → `迁移检查.py` | 「迁移已有模组」 |
| **改完功能重新打包 / 「我改了，进游戏没变」/ 版本号一直是 1.0.0** | **先分清「包到底换没换」再怀疑代码**：① `gradle.properties` 的 `mod_version` 是不是压根没动过 ② 新旧 jar **同名**、拷贝时静默没覆盖 ③ `mods/` 里同 modid 躺着多个版本。三处版本号（properties ↔ build/libs ↔ jar 内元数据）对一遍，再查实例 | `版本号.py --check --mods <实例/mods>` → `版本号.py --bump patch` | **「6.5 版本号」** |
| **写完 mod 怎么证明它真的能跑** | 起专用服务端 → RCON 驱动 → 离线扫存档取证；**行为挂在物品标签上（`stack.is(Tags.Items.TOOLS)` / `#c:ores`）→ 先验「标签到底认不认」（§8）**，标签没解析上不报错、判据恒 false | `rcon.py` → `扫区域文件.py` | 「真机验证」+ `references/专用服务端自动化验证.md` **§8** |
| ⭐ **客户端侧怎么自动验**（HUD 画出来没有 / 界面点得动没有 / 按键绑上没有 / 模型贴图对不对 / 第一帧崩不崩 / **我的方块被真右键时有没有反应**） | **模组当进程内代理 + 外部 Python 驱动**：注入不碰工程 → 起客户端 → 进世界 → **走路/转向/右键/放置/开背包/点槽位**（玩家输入层）+ 指挥令/按键/开界面/截图 → 读结构化状态断言 | **`游戏内测试.py`**（`doctor` → `run`） | ⭐ **`references/26.x-游戏内自动化测试.md`** |
| **生成类内容怎么验收**（地形/结构/方块摆放/含水洞穴） | 三层判据：离线纯函数（秒级）→ 存档判据（分钟级）→ 出图 | — | ⭐ `references/验证判据设计.md` |
| **改模组的显示名 / 改成中文名** | 改 `gradle.properties` 的 **`mod_name`**（不是 `mod_id`！）→ 中文必须 `\uXXXX` 转义 | 读 jar 内 mods.toml 断言 | `references/工程最佳实践.md` §2.5 |
| **自己方块的贴图在游戏里是「黑底方块」**（不是紫黑格，图案还在但透明处变成黑色） | 方块落在 **solid 渲染层** —— 原版树苗是靠 `ItemBlockRenderTypes` 内部的**硬编码方块名单**才拿到 cutout 的，自定义方块不在名单里。修法两条（都配上最稳）：① model JSON 加 `"render_type": "minecraft:cutout"`（**Forge 官方推荐**；`setRenderLayer` 自 1.19 起已 `@Deprecated`，废弃注释原文就写着这条）；② Java 侧 `@EventBusSubscriber(Dist.CLIENT)` + `ItemBlockRenderTypes.setRenderLayer(block, RenderType.cutout())`。⚠️ **无头服务端永远测不出来**（服务端不碰渲染），必须起真实客户端 | 起客户端进世界截图 → 统计画面里**纯黑像素占比**（正常趋近 0%，只剩准星是黑的） | `references/工程最佳实践.md` §2.5 |
| **压一下 jar 体积 / 发布 mod** | — | — | `references/工程最佳实践.md`（§2.5 反模式清单 / §2.6 标准流程） |

**判断不出来的两个高频歧义**：
- 「写个 mod」≠「做个整合包」。用户说"模组/物品/方块/生物/地形/按键"→ 本 skill；
  说"整合包/包里加个 mod/任务书/打包"→ 转 **MC一站式工坊**。
- 「mod 起不来 / 崩了」先分阶段：**编译期**报错查 `检查工程.py` 反模式；
  **启动期**崩（mixins 兼容、API drift、依赖缺失）查工程声明的版本 vs 实机版本；
  **进世界画第一帧就崩且栈里有 `Missing elements in vertex:`** → 渲染管线顶点元素不匹配，跑 `渲染API核对.py`。

### 🚫 这个 skill 帮不了什么（先看这里，别白跑）

以下情况**如实说做不到 + 给替代方案**，不要硬凑一个看起来像答案的东西：

| 用户要什么 | 为什么帮不了 | 能给什么 |
|---|---|---|
| **整合包相关的一切**（体检实例/挑模组/下模组/打包分发/任务书/汉化） | 本 skill 只管 mod 源码工程 | 转 **MC一站式工坊** |
| **基岩版 / 手机版 / 网易版模组** | 只覆盖 **Java 版**，基岩版是另一套（附加包 JSON + 脚本 API） | 只能指出方向（官方附加包文档），别用 Java 版写法套 |
| **1.7.10 / 1.12.2 等远古版本** | `templates/` 与反模式表从 1.20.1 起，更早的 Forge 写法（`GameRegistry`、`@Mod` 老签名）没覆盖 | 可以帮**读**老代码、判断大致思路，但生成的工程不能保证编译 |
| **运行期随机崩（mod 自身 bug）** | 静态扫描查「装得上」，查不出「特定内容触发才崩」（如 alpha mod 的 NPE） | 帮你**定位到是哪个 mod 的锅**（读崩溃栈的 `TRANSFORMER/<modid>@<版本>` 层）+ 建议移除 |
| **客户端渲染对不对**（HUD/界面/模型/雾效） | 静态核对查得出"要素没写全"，查不出"画出来长什么样" | **起真实客户端跑场景 + 截图给你（或给 AI）看**：`游戏内测试.py`（见 `references/26.x-游戏内自动化测试.md`）；不能替代人对手感/观感的判断 |
| **反编译闭源 mod 改它的代码** | 不涉及版权规避；且反编译产物不可维护 | 可以帮你**读**字节码签名（`JarSig.py`）来判断兼容性、或找官方开源仓库 |
| **保证某个新版本一定不崩** | 新 MC/loader 刚开时生态不稳（mod 按早期 API 编译、loader 还在改） | 做静态验证 + **跑进游戏实测（§8）给本机实证** + 说明验证到哪一层；崩了按崩溃报告定位 |
| **1.20.1 / 1.21.x 的客户端自动化** | 游戏内代理目前只打通 **26.1 / NeoForge**（Fabric 用官方 `fabric-client-gametest-api-v1`） | 移植要点在 `references/26.x-游戏内自动化测试.md` §7；老版本先靠 `runServer` + 离线取证 |

> 判据很简单：**能不能用「静态文件 + 离线命令」验证**。能 → 本 skill 覆盖；
> 需要真跑起来的 → **服务端走 `rcon.py`，客户端走 `游戏内测试.py`**（真实客户端 + 进程内代理）；
> 只有「人对手感/观感的判断」「多人联机环境」这类才交给人。

### 脚本速查（全部可 `--help`；准确数量跑 `python scripts/自检.py` 看）

**建工程 / 写内容 / 补资源**

| 脚本 | 一句话 | 典型命令 |
|---|---|---|
| **`新建工程.py`** | **套官方模板生成工程**（复制模板 + 替换 modid/包名/版本 + 补 gradle wrapper + 探测本机 JDK） | `python 新建工程.py -p neoforge -m 26.1 <名>` |
| **`检查工程.py`** | **源码工程体检**：反模式 / 资源完整性 / 版本匹配 / 出身鉴别（MCreator 等）/ 脚手架缺口 / **版本号三处一致** | `python 检查工程.py <工程>` |
| **`版本号.py`** | **改完功能/交付前 bump 版本 + 一致性门禁**：`gradle.properties` ↔ `build/libs` 产物 ↔ jar 内 `mods.toml`/`fabric.mod.json` 三处对齐；`--mods` 顺带查实例里同 modid **多版本共存**（"改了没生效"的头号原因） | `python 版本号.py <工程> --bump patch` ／ `python 版本号.py <工程> --check --mods <实例/mods>` |
| **`迁移检查.py`** | **出版本迁移路线图**（代码层/构建层/资源层三组规则） | `python 迁移检查.py <工程> --to 26.1` |
| `像素纹理生成.py` | 程序化画 16/32 像素纹理（10 个预设，同 seed 必复现） | `python 像素纹理生成.py --preset mycelium -o x.png` |
| `纹理变色.py` | 形状不动**只换颜色**，一套形状出一整组变体 | `python 纹理变色.py base.png --preset woods -o out/` |
| **`体素模型生成.py`** | **把 16×16 贴图立体化成 3D 体素物品模型**（alpha→距离变换→分层倒角→逐格立方体）。默认输出无缝一体；`--shrink 0.90 --no-cull` 出看得见缝的方块堆 | `python 体素模型生成.py --tex x.png --out-model m.json --out-texture t.png --scale 2 --side 2` |
| **`物品模型预览.py`** | **不开游戏看 3D 物品模型**：正交投影多视角离线渲染（runClient 看不到画面时的视觉验收手段） | `python 物品模型预览.py --model m.json --tex t.png -o p.png --zoom 24` |
| **`物品模型校验.py`** | 按 MC 模型 schema 静态校验 element 模型：坐标范围/from<to/UV 0..16/纹理引用/面名/rotation | `python 物品模型校验.py model.json` |
| `原版资源提取.py` | 从原版 jar 取素材（贴图/模型/数据包/语言/类名） | `python 原版资源提取.py --ver 26.2 --search door` |
| `生成方块配色.py` | 从原版客户端材质提取平均色 → 生成 `block_colors.py` 配色表（`世界预览.py` 渲染色兜底） | `python 生成方块配色.py <原版client.jar>` |
| `读图文字.ps1` | 模型看不了图时的兜底：Windows 自带 OCR（WinRT）把截图转文字，零依赖 | `powershell -File 读图文字.ps1 <图片路径>` |
| `JarSig.py` | 直接解析 .class 常量池，dump 任意 jar 内的**真实方法签名+参数名** | `python JarSig.py <jar> net.minecraft.world.level.chunk.ChunkGenerator` |
| **`渲染API核对.py`** | **26.x 渲染/GUI 专用**：把 pipeline 需要的顶点元素和 `*RenderState` 实际写入的对起来，提前抓 `Missing elements in vertex: UV0` | `python 渲染API核对.py --project <工程>` |
| `实体模型预览.py` | 实体模型**离线等距预览 + 自检**（脚底高度/尺寸/UV 越界重叠） | `python 实体模型预览.py boxes.json --tex entity.png -o preview.png` |
| `还原templates.py` | 还原/校验 `templates/` 归档（部分分发形态下模板会被打包成 `templates.pack.txt`） | `python 还原templates.py --check` |
| `模组兼容扫描.py` | 字节码级 API 断裂扫描（附属 mod 用旧 API 编译 → 一启动 `NoClassDefFoundError`） | `python 模组兼容扫描.py <实例/mods> --era` |
| `scan_null_blockitem.py` | 查 block==null 的 BlockItem（运行期 NPE 元凶） | `python scan_null_blockitem.py <实例/mods>` |
| **`mixin查源.py`** | **从崩溃堆栈的 `handler$xxx$<名>` 反查注入方是哪个 mod**（`--handler` 按 handler 名找 / `--class` dump 某 mixin 常量池 / `--ctor` 找谁注入了某类构造） | `python mixin查源.py --mods <mods目录> --handler onConstructed` |
| **`多代理任务板.py`** | **大任务并行门禁**：生成任务板；检查任务 DAG、依赖悬空/环、文件与目录写冲突；`--delivery` 检查完成状态与证据；`--selftest` 双向验证判据 | `python 多代理任务板.py check 协作计划.json` |
| **`自检.py`** | **脚本自身还能不能跑**：语法 / --help 冒烟 / 脚本间交叉引用 / 退出码约定 / **无副作用** / **双 skill 交叉同步**（`--net` 加测下载通道；`--no-peer` 跳过交叉检查） | `python 自检.py` |
| **`交叉同步.py`** | **双 skill 交叉补全**：`MC一站式工坊` ↔ `MC模组工坊` 的 mod 公共资产比对与补齐（`--sync` 补单边独有 / `--force` 按超集统一 / `--audit` 查「未归类文件」＝清单过期 / `--diff` 看单文件差异）。分叉的不自动覆盖，拒绝执行让你人工合并 | `python 交叉同步.py --sync` |
| **`跨agent同步.py`** | **跨 Agent 副本同步**：本机其他 agent（CodeBuddy 等）`skills/` 目录里的同名副本 ↔ 本源的差异检查与推送（`--sync` 纯加法推送 / `--mirror` 完全镜像 / `--list` 列本机 agent 目录）。副本独有文件（`_skillhub_meta.json`）默认保留 | `python 跨agent同步.py --sync` |
| ⭐ **`经验沉淀.py`** | **自我优化闭环工具**：`--find 关键词` 定位新坑该记到哪+查重（同小节命中全部关键词→只补增量）/ `--doctor` 文档引用完整性体检（挂 `自检.py` H 节：路径引用、§锚点、速查表登记、归档配对）/ `--stale` 生态断言时效清单（超 90 天的「快照/会变」类内容追责） | `python 经验沉淀.py --doctor` |

**真机验证链（起服务端 → 取证）**

| 脚本 | 一句话 | 典型命令 |
|---|---|---|
| `rcon.py` | RCON 客户端：驱动专用服务端跑命令做自动化验证 | `python rcon.py "execute in <维度> run locate biome ..."` |
| `扫区域文件.py` | **非破坏性**统计存档里真生成过哪些方块（直读 .mca） | `python 扫区域文件.py run/world <方块id>` |
| **`按深度统计方块.py`** | **逐段解码调色板**，告诉你每一段（16 格）里到底有哪些方块。改高度/矿脉后**必跑** | `python 按深度统计方块.py run/world` |
| **`世界生成体检.py`** | **「世界像不像一个世界」的整体性体检** —— 按段统计固体/水/岩浆/空气占比，**自动剔除未生成完的空壳区块** | `python 世界生成体检.py <存档目录>` |
| `世界生成体检_自检.py` | 上者的**判据双向验证**（坏样本必报 + 正常样本零误杀）。改判据后必跑 | `python 世界生成体检_自检.py` |
| **`密度函数引用链.py`** | 改 `noise_settings` / `density_function` 前的必跑项：展开**字符串引用**建反向引用图 | `python 密度函数引用链.py ./src/main/resources <原版jar>` |
| `探地表高度.py` | 查**地表高度规律**，判「台阶/平滑」 | `python 探地表高度.py --mca <region目录> --step 6` |
| **`地形剖面.py`** | 把存档切成横/纵截面图（方块=黑·液体=灰·空气=白）。看的是**形状** | `python 地形剖面.py <存档> --axis z --at 0 -o s.png` |
| **`世界预览.py`** | **离线等距预览地形/建筑**（直读 .mca，不用开客户端）—— 改地形的唯一视觉验收手段 | `python 世界预览.py <存档目录> -o preview.png` |

**游戏内测试链（客户端侧：真实客户端 + 进程内代理）**

| 脚本 | 一句话 | 典型命令 |
|---|---|---|
| ⭐ **`游戏内测试.py`** | **让 AI 真的进游戏里跑一遍**：构建代理 → init script 注入 `runClient` 运行时（**不改工程**）→ 进世界 → 走路/右键/放置/开背包/点槽位（玩家输入层）+ 指挥令/按键/开界面/截图 → 读结构化状态 → 出报告。子命令 `doctor` / `agent` / **`smoke`（🔴 进游戏实测门禁）** / `run` / `shot` / `close` | `python 游戏内测试.py run --project <工程> --scenario 场景.py` |
| 🔴 **`游戏内测试.py smoke`** | **交付门禁**：起真实客户端 → 自动建世界进世界 → 推进 N tick（gameTime 必涨）→ 截图（非黑屏）→ 扫客户端日志异常 → 逐条判据出报告。**退出码 0 才许交付** | `python 游戏内测试.py smoke --project <工程>` |
| `游戏内注入.gradle` | 上者的注入件（Gradle init script，把代理 jar 加进 `runtimeOnly`）；也可单独手工用 | `gradlew --init-script 游戏内注入.gradle -Dmctestagent.jars=<jar> runClient` |
| `templates/测试代理-26.1/` | 进程内代理的**源码模板**（NeoForge 26.1 / MDG 工程，产物 `游戏内测试代理-<版本>.jar`） | 由 `游戏内测试.py agent` 自动构建/更新 |

> **退出码约定**：**0 = 干净/成功 ｜ 1 = 发现问题 ｜ 2 = 参数或路径问题**
> 体检类（`检查工程` / `迁移检查`）发现问题返回 **1**、干净 **0**，可用 `&&` / `||` 串成
> 「**体检 → 修 → 构建**」流水线 —— 体检不过自动拦住构建。
> 下载/生成类脚本失败同样非 0。

> ⚠️ **`templates/` 若被归档过，先还原**：部分分发形态下 `.gradle` / `.properties` / `.bat` / `.jar`
> 会被打包成 `templates.pack.txt`（`templates/` 被 `.packignore` 排除）。
> 跑一次 **`python scripts/还原templates.py`** 就能还原出完整模板
> （幂等：已存在则跳过；`--check` 只校验；`--force` 全量覆盖；`--make` 供维护时重新生成归档）。
> 本地开发目录里模板本来就是完整的，跑一下会直接显示「已存在跳过」，无副作用。
> 没还原就建工程 → 模板缺 `gradlew` / wrapper jar，`./gradlew build` 会报
> `Could not find or load class org.gradle.wrapper.GradleWrapperMain`。

<!-- CROSS-SYNC:BEGIN —— 本区由 scripts/交叉同步.py 维护，两个 skill 里必须逐字一致；改这里会被自动推到对方 -->
## 🔁 双 skill 交叉补全（mod 公共资产必须两边一致）

`MC一站式工坊` 和 `MC模组工坊` 覆盖**同一批 mod 开发知识**，各自存了一份 `references/` 与
`scripts/`。两边同时安装时，**mod 公共资产必须逐字一致** —— 复制成两份后各自进化，必然长出
「SKILL.md 引用的文档是旧的 / 另一边根本没有」。2026-10-01 实测就抓到一串：一站的
`代码片段.md` 落后 605 行、`验证判据设计.md` 整篇缺失、`templates/` 里 9 个文件缺了
`neo_version_range` 修复（而那个修复正是「玩家实例启动即拒载」的解药）。

### 规则：改完 mod 相关内容，顺手把对方补齐

**任何一次 mod 相关的新增 / 修改 / 修复之后**（新写 references 文档、改脚本、动 SKILL.md 的
mod 章节、更新 `templates/`），跑一次：

```bash
python scripts/交叉同步.py                     # ① 看差异（默认只检查，不动文件）
python scripts/交叉同步.py --sync              # ② 把「只有一边有」的资产补到另一边（纯加法，安全）
python scripts/交叉同步.py --sync --force      # ③ 「漂移」的按超集方向统一（一边是另一边的新版时）
python scripts/交叉同步.py --diff 代码片段.md   # ④ 分叉的（各有独有内容）看差异后人工合并
```

**判据：`交叉同步.py` 退出码 0，或 `自检.py` 的 F 节全绿** —— 不是「我记得同步过了」。
姊妹 skill 不存在时脚本自动报「单机模式」跳过，单装一个 skill 不受任何影响。

### 什么算「mod 公共资产」（清单写死在 `scripts/交叉同步.py` 里，`--list` 可看）

| 类型 | 例子 | 归属 |
|---|---|---|
| **处理 mod 的东西**（源码工程 / jar / class / 地形 / 纹理 / 模型 / 迁移 / 验证判据） | `代码片段.md`、`改造原版复用.md`、`26.x-*` 系列、`JarSig.py`、`检查工程.py`、`rcon.py`、`templates/` | **两边都要有，逐字一致** |
| **处理实例的东西**（整合包 / 任务书 / 汉化 / 玩家皮肤） | `整合包体检.py`、`任务书*.py`、`皮肤生成.py`、`FTB_Quests任务编辑.md` | **各自专属，绝不往对方搬** |

### 硬规矩（都是踩过才写的）

1. **新写 mod 文档 / 脚本** → 跑 `--sync`；**同时把文件名加进 `交叉同步.py` 的清单** ——
   `--audit` 会报「未归类文件」；清单过期不会自动报警，只会让下次同步静默漏掉它。
2. **改了 `templates/` 或 `还原templates.py`** → 两边都跑 `python scripts/还原templates.py --make`
   重生成归档（归档是确定性生成，两边模板一致时产物 md5 一致）。
3. **`自检.py` / `交叉同步.py` 是两个 skill 共用的同一份**：改完必须 `--sync --force` 推过去，
   否则在另一边跑的还是旧版。
4. **行尾统一 LF**（文档 / 脚本）；**`templates/` 保持原始字节** —— 它是会被复制进用户工程的
   产物源，别对它的行尾做归一化。
5. **分叉不自动覆盖**：两边各有独有内容时脚本拒绝执行、让你人工合并 —— 这是设计不是 bug，
   宁可停下来，不要静默丢东西。
6. **SKILL.md 只有本标记区走自动同步**；其余章节各自演进（本 skill 的整合包 / 皮肤章节对对方无意义）。

## 🌐 跨 Agent 副本同步（同一 skill · 本机多个 agent 目录）

本机同时装着多个 agent（WorkBuddy / CodeBuddy / …），**每个 agent 的 `skills/` 目录里各有一份同名副本**。
只在主目录里改 skill，别的 agent 目录里那份会越放越旧 —— 表现是「同一个 skill，换个 agent 用行为不一样」。
2026-10-09 实测：`.workbuddy/skills/` 与 `.codebuddy/skills/` 的同名副本并存，此前靠手工复制，
漏没漏全凭记性。所以定了这个功能：

```bash
python scripts/跨agent同步.py            # ① 检查：列出各 agent 副本与源的差异（默认只读）
python scripts/跨agent同步.py --sync     # ② 推送：新增/更新 → 所有同名副本（纯加法，安全）
python scripts/跨agent同步.py --list     # ③ 看看本机都扫到哪些 agent 的 skills 目录
```

**改完 skill（任何新增 / 修改 / 修复）后的完整动作链**：
`交叉同步.py --sync`（两个 skill 之间对齐）→ `跨agent同步.py --sync`（推给其他 agent 的副本）。

### 规矩

1. **唯一编辑点在主目录**（`.workbuddy/skills/`），副本侧只读不编辑；脚本从哪运行就以哪为源。
2. **副本独有文件默认保留**：如 `_skillhub_meta.json`（该 agent 的安装元数据），不属于同步内容，不碰。
3. `--mirror` 才删除副本里源没有的文件（谨慎；默认永远不删,只加和覆盖）。
4. 副本发现规则：扫 home 下一层，凡有 `skills/` 子目录的都算一个 agent 根；扫不到的位置用 `--root` 补。
5. **判据：`跨agent同步.py` 退出码 0**（同步后脚本会自复核一遍）——不是「我记得推过了」。

## 🧠 自我优化闭环（踩坑 → 沉淀 → 验证 → 同步，机械可执行）

> 本 skill 的知识 95% 来自实测踩坑。**经验只在对话里说一遍 = 白踩。**
> 2026-10-12 之前沉淀靠自觉（「解决后回头补进 SKILL.md」一句话），漏没漏全凭记性；
> 首次全量体检就抓到：4 处 §锚点悬空、9 个脚本/文档存在但 SKILL.md 从未记载、
> 还有 1 处「旧结论已被实测推翻、姊妹 skill 还在当真理引用」（runClient 旧误判）。
> 现在把沉淀做成四步，每步有工具、有判据：

| 步 | 动作 | 工具 | 判据（不是「我觉得做完了」） |
|---|---|---|---|
| ① **定位** | 新坑解决后，先查「该记到哪 + 是否已记录过」，别凭印象塞 | `python scripts/经验沉淀.py --find <关键词>…` | 从候选落点里挑一个；同小节已命中全部关键词 → 只补增量，别重写 |
| ② **沉淀** | 把「症状 → 根因 → 判据/修法」写进定位到的文档 | 手工编辑 | 三件套齐全 + `YYYY-MM-DD 实测` 戳 |
| ③ **验证** | 写完立即验「引用没写坏、清单没过期」 | `python scripts/经验沉淀.py --doctor` | 退出码 0；`python scripts/自检.py` H 节绿 |
| ④ **同步** | 交叉同步 → 跨 agent 同步 | `交叉同步.py --sync` → `跨agent同步.py --sync` | 两个脚本退出码 0 |

**触发条件（满足其一就走闭环）**：修好一个非平凡 bug / 用户指出一个我漏掉的问题 /
发现一条与文档冲突的实测事实。

**时效体检（每次 MC 大版本发布后跑一次）**：`python scripts/经验沉淀.py --stale`
列出所有自带日期的「生态快照/版本断言」及其年龄，超 90 天（`--days` 可调）的优先
重新核实，核实完把新日期戳盖上去；`--strict` 可当断言串进流水线。

**写沉淀的四条格式规矩**（都是漏沉淀的教训换来的）：
1. 症状写成**可 grep 的原文**（报错消息/日志行），后人才能对号入座；
2. 判据必须**可执行**（命令/断言）——「注意一下」这种话没法验收；
3. 能进工具终检的别只留在文档里（文档会烂，退出码不会）；
4. 带日期戳——过期由 `--stale` 追责，别让后人猜这经验还灵不灵。

<!-- CROSS-SYNC:END -->

## 🔴 第一铁律：先锁版本，再写代码

MC 模组开发最大的坑是**版本写法不通用**。1.20.1 的教程代码照抄到 1.21.x / 26.x 上**直接编译失败**。

**动手前必须确定两个变量**，缺一就问用户（不要猜）：

| 变量 | 取值 | 怎么定 |
|---|---|---|
| **目标 MC 版本** | 1.20.1 / 1.21.1 / 1.21.4 / 26.1 / 26.2 ... | 用户没指定时按下面的推荐表给默认并说明 |
| **加载器** | Forge / NeoForge / Fabric / Architectury | 由版本 + 需求推出 |

确定后，**全程只使用该版本对应的写法**，不同时代的 API 不可混用。

> ⚠️ 本节说的「版本」是**环境版本**（MC / 加载器 / 映射）—— 和**模组自己的版本号** `mod_version` 是两件事：
> 前者定下来就不能乱动，后者每交付一次就该递增。模组版本号的 bump 与三处一致性见 **§6.5**。

### 版本推荐（2026 当下）

| 场景 | 推荐 | 理由 |
|---|---|---|
| 求生态最稳、模组最多 | **1.21.1 + NeoForge** | 主流整合包仍在此版本，第三方库配套全 |
| 要追最新特性、调试友好 | **26.2 + NeoForge 或 Fabric** | 26.1 起彻底移除混淆，代码可读、无 mapping 层 |
| 老生态 / 参考老 mod 源码 | **1.20.1 + Forge** | 历史模组源码最多，但 API 已落后 |
| 轻量 / 性能 / 快速迭代 | **Fabric**（任意支持版本） | 文档最全、更新最快 |
| 一套代码跨 Forge/Fabric | **Architectury** | 有额外抽象成本，仅在确有跨端需求时用 |

**不要**默认选 1.20.1 写新模组——组件系统、物品模型、注册表重构都在 1.21+。

### 加载器与版本对应

| MC 版本 | Forge | NeoForge | Fabric | Java |
|---|---|---|---|---|
| 1.20.1 | ✅ | ✅（与 Forge 互通） | ✅ | 17 |
| 1.20.2–1.21.4 | ✅（1.21+ 基本停更） | ✅ | ✅ | 17 / 21 |
| 1.21.5–1.21.11 | ❌ | ✅ | ✅ | 21 |
| 26.1 / 26.2 | ❌ | ✅ | ✅ | **25** |

---

## 工作流

### 1. 确认需求

提取：目标 MC 版本、加载器、mod 名称/modid/包名、要加什么内容（物品/方块/方块实体/维度/生物/配方/HUD...）。
缺失关键信息就问，不问无关细节。

### 1.5 命名约定 —— **文件名叫中文，代码标识符叫英文**（元的硬性要求）

元要求：**模组文件名一律用中文**。但能中文化的只有「文件名」这一层，
再往里必须是小写英文 —— 那是 MC / Java 自己的规矩，改中文游戏直接拒载：

| 层 | 用什么 | 谁说了算 |
|---|---|---|
| **产物 jar 文件名** | **中文**（= 显示名，如 `绿宝石剑-1.0.0.jar`） | 元的偏好 |
| **工程目录名** | **中文**（如 `绿宝石剑/`） | 元的偏好 |
| `modid` / `mod_id` | 小写英文 `[a-z][a-z0-9_]{1,63}` | 🔴 MC 强制 |
| Java 包名 | 英文（如 `com.yuan.emeraldsword`） | 🔴 Java 强制 |
| 资源目录 / 注册 id | 英文小写（`assets/<modid>/…`、`emerald_sword`） | 🔴 MC 强制 |
| 显示名（mods.toml / lang） | 中文 | 随便 |

**怎么落地**：目录给中文、`--name` 给中文（它同时决定 jar 文件名和游戏内显示名）。

```bash
python scripts/新建工程.py -p neoforge -m 26.1 \
    --modid emeraldsword --name "绿宝石剑" --dir "绿宝石剑"
```

脚手架已经打通这条链：模板 `build.gradle` 里是 **`archivesName = mod_name`**（jar 名取显示名），
并且**自动把 `mod_name` 的中文转成 `\uXXXX`**。

**🔴 手工改工程时最容易漏的一步**：`gradle.properties` 的 `mod_name` 必须写成转义
（`mod_name=\u7EFF\u5B9D\u77F3\u5251`）—— **Gradle 按 ISO-8859-1 读 .properties**。
直写中文 → 取到乱码 → 顺着 `archivesName`（jar 名）和 mods.toml 的 `displayName`（模组名）
**一起烂**：jar 变成 `ç»¿å®ç³å-1.0.0.jar`，游戏里模组名也是乱码。脚手架会自动转义，手改时自己记得转。

**fabric 模板不同**：它的 `build.gradle` 没有 `archivesName`，产物名取**工程目录名**，
所以 fabric 只要目录给中文，jar 名自然就是中文。

**中文文件名安全吗？NeoForge 实测安全；Forge 47.x 必须给 jar 加 `Automatic-Module-Name`，否则启动即崩**（2026-10-13 实测，用户实例 Forge 1.20.1-47.4.23）。把 `绿宝石剑-1.0.0.jar` 丢进 NeoForge `mods/` 起服务端：

```
Considering mod file candidate D:\...\mods\绿宝石剑-1.0.0.jar
Found valid mod file 绿宝石剑-1.0.0.jar with {emeraldsword} mods - versions {1.0.0}
Done (1.898s)!
```

NeoForge 正确扫描、解析了中文文件名，modid / 版本 / 显示名全部读对，无冲突。
中文**工程目录**也让 Gradle 正常工作（只把 `build/` 缓存重建一次，之后照常增量）。

⚠️ **但同一份 jar 丢进 Forge 47.x 的 `mods/` 会直接崩在启动扫描阶段**：

```
Exception in thread "main" java.lang.IllegalArgumentException: : Invalid module name: '' is not a Java identifier
	at java.base/jdk.internal.module.ModuleDescriptor$Builder.<init>(ModuleDescriptor.java:1595)
	at cpw.mods.securejarhandler/cpw.mods.jarhandling.impl.SimpleJarMetadata.descriptor(SimpleJarMetadata.java:13)
	at MC-BOOTSTRAP/fmlloader@1.20.1-47.4.23/net.minecraftforge.fml.loading.ModDirTransformerDiscoverer.visitFile(...)
```

根因：securejarhandler 给每个 jar 算 JPMS 自动模块名，manifest 没有 `Automatic-Module-Name`
时按**文件名**推导（`JarMetadata.fromFileName` → `cleanModuleName`），而清理正则
`[^A-Za-z0-9]` **只认 ASCII**——中文名部分整体变点，再 trim 成**空串** →
`ModuleDescriptor.newAutomaticModule("")` → 崩。
**判据/修法**：`build.gradle` 的 `jar { manifest { attributes 'Automatic-Module-Name': mod_id } }`，
有该属性时 `JarMetadata.fromManifest` 直接拿属性值（字节码 offset 92-121），绕开文件名推导。
验证：`java -cp <securejarhandler.jar> ...` 或 JDK `ModuleFinder.of(jar).findAll()` 应打出
`treehousetree`；空名复现 `ModuleDescriptor.newAutomaticModule("")` 抛一字不差的异常。

### 1.6 模组规划 —— 🔴 独立阶段：不出规划书，不写任何代码

需求确认（§1）与命名约定（§1.5）之后、**建工程 / 生成任何代码之前**，必须先产出一份
**模组规划书**并落盘（写到工程目录或工作区的 `规划.md`）。它不是聊天里的几句话，
而是**后续生成与实现的唯一依据**：写码阶段每实现一个条目都要能对应回规划里的某一条；
要偏离规划（增删内容、换方案、改数值），**先改规划书、说明原因，再动代码**。

**规划书固定六个部分，缺一不可**：

| # | 部分 | 写什么 | 为什么必须有 |
|---|---|---|---|
| 1 | **目标与边界** | 一句话说清 mod 是什么、给谁用；明确**不做什么**——原版已有的内容划掉或写清差异化 | 防"做着做着变成另一个 mod"；§3 开头的选题划界限在这里前置完成 |
| 2 | **技术底盘** | MC 版本 / 加载器 / Java 版本（第一铁律锁定的值，此处写死）；工程名（中文）/ modid / 包名 | 后续所有代码片段的写法选型以此为准，写码中途不允许再换版本 |
| 3 | **内容清单** | 逐条列出全部要注册的内容：**id（英文小写）、显示名（中文）、类型（物品/方块/生物/系统…）、行为一句话、数值初稿**（伤害/耐久/合成成本/属性上限…） | 这是"实现符合设计预期"的对照表——验收时逐条打勾 |
| 4 | **组织方案** | 内容组织决策：哪些走数据驱动（JSON）、哪些直接代码注册、注册类怎么分（参考 `references/实战拆解-真实mod架构.md`）；第三方联动方式 | §3 的"先想清楚内容组织再动手"在这里前置落地，写码时不再临场拍板 |
| 5 | **资源清单** | 每个内容需要什么贴图/模型/lang/配方/战利品表，来源三选一并写明：程序化生成（§4.1）/ 变色批量（§4.2）/ 抄原版改造（`references/改造原版复用.md`） | 防写完代码才发现贴图没着落；来源决定 §4 的工作量与顺序 |
| 6 | **验证计划** | 每块内容怎么算"做完"：静态（`检查工程.py`）→ 构建（`build`）→ 服务端（`runServer` + RCON）→ 客户端（`渲染API核对.py` + `游戏内测试.py`）；生成类内容按 `references/验证判据设计.md` 预先设计判据。**末尾必须挂 🔴 进游戏实测门禁（`游戏内测试.py smoke`，§8）——它是交付的硬闸，不是可选项** | 把验收标准在动手前定死，防"编译过了就算完成" |

**执行规矩（硬性）**：

1. **规划先行，代码后置**。规划书没写出来之前，不生成任何工程文件与代码。
   往现有 mod 加一两个物品这类小改动可出**迷你规划**（第 3、5 两节即可），但不能没有。
2. **规划要给用户过目**。产出后展示规划书全文，请用户确认或改；确认后才进 §2。
   用户明确说"直接做 / 不用确认"→ 视为确认，规划照写但不等待。
3. **实现对照规划**。§3 / §4 阶段逐条按规划实现；发现规划不合理，**先改规划书再改代码**，
   两份东西不允许各说各话。
4. **规划即验收基线**。§5–§7 检查/构建/真机验证按规划书内容清单逐条核对：
   "规划里写了但没实现"和"实现了但规划里没有"都要在交付说明里点名。

### 1.7 多代理协作 —— 可独立才拆，主代理负责集成

任务繁重且能拆成 **至少 2 个独立工作包**时，使用复数子代理并行；先读
**`references/多代理协作.md`**，不要只凭“任务看起来很多”就拆。加一个物品、修一个报错、
多人要写同一文件、方案尚未锁定、共享 build / 客户端 / 服务端 / 存档验证，均保持单线。

硬流程：

1. **主代理先锁契约**：完成 §1.6 规划、版本与加载器、目录命名、公共 API / 注册 id、验证矩阵。
2. **落任务板**：用 `scripts/多代理任务板.py init <计划.json> --project <名>` 生成模板，
   按工作包改好后跑 `check`；退出码 0 才能派代理。每个包写清目标、绝对工程路径、允许写清单、
   依赖、独立验收与回报格式；无依赖的包在同一批并行派出，通常 2–4 个执行代理即可。
3. **文件独占**：不同子代理不得写同一文件；注册聚合、主 mod 类、构建配置、任务书总生成器
   默认归主代理。子代理默认不再派下级代理；只读代理不得改文件。
4. **主代理回收**：对照真实 diff 检查越界、审查证据、处理接口变更，再更新任务状态。
   子代理返回“完成”不等于通过；失败项保持未完成，重派或主代理接手。
5. **统一集成验收**：构建、共享游戏环境与最终门禁由主代理串行跑。模组的
   `build → runServer → smoke`、整合包的静态检查 → 专用服务端 → 打包 → 真客户端进世界证据，
   一项都不能因子代理局部成功而省略。交付前用 `多代理任务板.py check <计划.json> --delivery`
   检查完成状态与证据声明，再由主代理实际复核。

### 2. 建工程 —— **优先套官方模板，不要手写 build.gradle**

本 skill 的 `templates/` 下是**从官方仓库抓的真实模板**，直接复制后替换占位符：

| 目录 | 对应 | 用途 |
|---|---|---|
| `templates/forge-1.20.1/` | Forge 1.20.1（ForgeGradle 6.x，Java 17） | 老版本生态 |
| `templates/neoforge-1.21.1/` | NeoForge 1.21.1（ModDevGradle，Java 21） | 当前主流 |
| `templates/neoforge-26.1-mdg/` | NeoForge 26.1（ModDevGradle 2.0.x，Java 25） | **新工程首选** |
| `templates/neoforge-26.1/` | NeoForge 26.1（NeoGradle 7.1.x，Java 25） | 需 NeoGradle 时 |
| `templates/fabric-26.2/` | Fabric 26.2（Loom 1.17，Java 25） | Fabric 新工程 |

用 `scripts/新建工程.py` 自动套模板并替换 modid/包名/版本，别手工改。

**脚手架已经把这些「生成完还得手动补、不补就翻车」的坑自动化了**：

| 它自动做的事 | 不做会怎样 |
|---|---|
| 复制 **gradle wrapper**（`gradlew` / `gradlew.bat` / `gradle/wrapper/**`，每个模板配对应 Gradle 版本） | 模板里本来没有 wrapper，生成完 `./gradlew build` 直接 `No such file or directory` |
| `minecraft_version_range` 生成成 `[26.1,26.2)` 而不是 `[26.1]` | `[26.1]` 是「精确等于」，而 NeoForge 26.1.2.108 实际带 MC **26.1.2** → 启动报 `Mod xxx requires minecraft 26.1` |
| 另生成 `neo_version_range`（小版本起点，如 `[26.1.0,)`）给 mods.toml 用，**不拿 `neo_version` 当下限** | `neo_version` 是编译构建号（联网取最新正式版）；玩家实例常停在同小版本更老构建（`26.1.0.19-beta`）→ 产物声明 `[26.1.2.109,)` 会让人家**启动即拒载** |
| 探测**官方启动器自带的 JDK**（`%APPDATA%\.minecraft\runtime\java-runtime-*`，挑含 javac 且版本够的那个）写进 `org.gradle.java.installations.paths` | 26.x 要 Java 25 而本机默认 java 是 21，Gradle toolchain 自动下载走 GitHub Releases（国内下不动）→ 构建卡死 |
| 建好 `run/eula.txt` + `run/server.properties` | 第一次 `runServer` 卡在 eula；另外**反复跑 runServer 会残留 java 进程占住 25565**，下次报 `FAILED TO BIND TO PORT`（杀命令行含 `devlaunch` 的 java 进程即可，实测必踩） |
| 生成完自检 wrapper 是否就位，并把 JDK 指向打进输出 | 出问题时不知道该查哪儿 |

> ⚠️ `forge-1.20.1/gradle.properties` 里的 `@MC_VERSION@`、`@FORGE_VERSION@` 等是**占位符**，必须替换成实际值（脚本会自动处理）。
>
> ⚠️⚠️ **`org.gradle.java.installations.paths` 这条路径里如果有中文，同样必须写成 `\uXXXX`** ——
> 它和 `mod_name` 是同一个坑（Gradle 按 ISO-8859-1 读 properties），但**报错完全指不到它**：
> 路径变乱码 ⇒ 找不到 JDK ⇒ 回落去 `api.foojay.io` → `github.com/adoptium` 下工具链（国内下不动）
> ⇒ 最后报的是 `Cannot find a Java installation ... languageVersion=25` + `Read timed out`，
> 看着像"本机没装 JDK"，其实 JDK 就在那儿、只是路径读错了。
> ```
> ❌ org.gradle.java.installations.paths=C:/.../2026-09-06-11-38-22/工具/jdk-25.0.4.1+1
> ✅ org.gradle.java.installations.paths=C:/.../2026-09-06-11-38-22/\u5DE5\u5177/jdk-25.0.4.1+1
> ```
> 再配一条 **`org.gradle.java.installations.auto-download=false`**：关掉联网回落之后
> 路径不对就直接报"找不到 JDK"，一眼能看出是路径写错了，不会被"Read timed out"埋掉。
> ⭐ **同一套配置在多个项目里各写一份就一定会漂**：实测 `Abyss` 转义了、`EndlessDepth`
> 没转义，于是"能编一个不能编另一个"。**改完一个项目的 gradle.properties，
> 要搜一遍同级项目有没有同款**（前置 / 附属 mod / 同一个作者的多份工程）。
>
> ⚠️⚠️ **验证"工具链到底认到没有"不要只看 `BUILD SUCCESSFUL`** ——
> 全 UP-TO-DATE 或 `FROM-CACHE` 都可能压根没触发编译。两条硬证据：
> ```bash
> ./gradlew -q javaToolchains          # 打出 "Detected by: Gradle property
>                                      #   'org.gradle.java.installations.paths'" 才算认到
> ./gradlew build --no-build-cache --rerun-tasks   # 逼它真编
> # 再验产物：class 文件第 6-8 字节 major = 69 ⇒ Java 25（61=17, 65=21）
> ```
>
> ⚠️⚠️ **依赖"前置 mod 的 build/libs 里的 jar"时，别在前置里跑 `clean`** ——
> `clean` 会删掉那个 jar，而引用方（`fileTree("../X/build/libs")`）下次真编译就挂。
> 前置里清完，必须把前置重编回来。

> ⚠️ 官方模板自带 `example_item` / `example_block` / `example_tab` 等**示例内容**，生成后要替换成自己的（检查器会提醒）。

**模板修正记录**：五个模板都补了 gradle wrapper；`neoforge-26.1-mdg` / `neoforge-26.1` / `neoforge-1.21.1` / `forge-1.20.1` 补了官方 MDK **漏抓的 `Config.java`**（缺了它示例代码里 5 处 `Config.xxx` 找不到符号，**开箱编译失败**）。
这三类坑 `检查工程.py` 都会报（`check_scaffold_gaps`）：版本区间写死成精确匹配、缺 wrapper、示例代码缺 `Config.java`。

### 3. 写内容

> 本阶段**按 §1.6 规划书的内容清单（第 3 节）与组织方案（第 4 节）逐条实现**，
> 不临场增删；要改先改规划书（§1.6 执行规矩 3）。

> ⚠️ **选题先划界限**：**原版已有的东西再做一遍就是冗余** —— 玩家分不清、不能与原版堆叠。
> 抄原版的**形状与工艺**，别抄**"身份"**。打算做"十种材质的装备"时先把原版已有的（木/石/铜/铁/金/钻石）划掉或明确差异化，
> 贴图也别直接复制原版（**视觉零差异等于白做**）。详见 `references/改造原版复用.md` 开头的「抄原版的边界」。

按 `references/` 里对应平台/版本的文档写注册与逻辑代码：

| 平台 | 资料 |
|---|---|
| Forge 1.20.1 / NeoForge 1.21+ | **`references/forge-neoforge.md`**（工具链、工程结构、核心系统最小可运行代码、Mixin、调试发布） |
| Fabric / Architectury 跨平台 | **`references/fabric-architectury.md`**（Fabric 工具链、核心系统、Mixin 详解、Architectury 多模块） |

常用系统代码模板见 `references/代码片段.md`（物品 §1 / 方块 §2 / 方块实体 §3 / 创造栏 §4 / datagen §5 / 网络包 §6 / 配置 §7 / 事件 §8 / Mixin §9 / BER §10 / 数据组件 §11 / **26.2 工具盔甲 §13-§14** / **自定义生物 §15** / **实体模型与贴图 §16** / **自定义 HUD §17** / ⭐ **26.1 面板 Screen + 数据附件 + 按键 §19** / ⭐⭐ **世界空间描边线框必须过滤光影 shadow pass §20**）。

> ⚠️ **做实体模型前先读 §16**。两条铁律：脚底必须落在模型坐标 **y = 24**（写成 0 会让整只生物浮空 1.5 格），
> 模型 UV 与贴图分区**硬绑定**（改 `texOffs` 要同步三处，少一处就出错位色块）。
> 动手前先跑 `scripts/实体模型预览.py` 建基线、改完再跑一次自检。

**先想清楚内容组织再动手**——参考 `references/实战拆解-真实mod架构.md`：

- **数值/规则多的系统**（枪械、武器、装备、附魔、转化/进化/蔓延规则）→ **数据驱动**：把内容拆成 JSON，Java 只写加载器。
  - 单件复杂内容（如一把枪）：index/data/display 三 JSON 分离
  - 批量同类内容（矿物、作物、状态转化）：注册循环读 JSON，或独立 data 目录
  - 想要 JSON 可读（带注释）：自定义解析器先剥 `//` 注释再走 Gson（十几行）
  - 转化/蔓延类规则放**非标准 data 目录**，数据包可覆写、`/reload` 热重载
- **给第三方 mod 联动**：优先塞 datapack 配方/标签（往 `data/<目标modid>/recipes/` 写文件，**不要声明强依赖**），只有需调用对方 API 时才写代码集成
- **多内容生物**：按层级分包（BaseXxx/EvolvedXxx…），客户端模型渲染单独 Client/ 子包
- **注册条目集中**：所有物品/方块/实体收进根包的 ModXxx 静态类
- **判断是否值得数据驱动**：一个方块一个物品的工具 mod 直接注册即可，别过度设计

### 4. 补资源

模型、材质、语言文件、配方、战利品表、标签。写法见 `references/资源文件.md`。
**资源路径必须与注册 id 一致**，否则游戏内是紫黑方块。

> 🔴 **动手前先搜原版有没有现成的**：物品贴图 / 生物模型 / 寻路 AI / 配方 / 战利品表 / 标签 /
> 语言键名，全都能从**原版 client jar**里取出来改改用（实例目录里那个 `<实例名>.jar` 就是原版 jar）。
> 取料命令、实测素材清单、生物模型与寻路子类的**真实 API 签名**见 **`references/改造原版复用.md`**；
> 取料工具 `scripts/原版资源提取.py`，贴图改色 `scripts/纹理变色.py`。

#### 4.1 纹理图源（程序化生成）

MC 纹理要 16×16/32×32 像素 + 方块需四边无缝平铺。**直接程序化逐像素画**（原版 MC 纹理就是手绘 + 程序化混合做的，不是大图降采样——降采样会糊、颜色不可控）：

```bash
python scripts/像素纹理生成.py --preset mycelium --seed 7 --colors 16 -o myc.png
python scripts/像素纹理生成.py --preset stone_brick --seed 11 -o brick.png
python scripts/像素纹理生成.py --preset obsidian --seed 42 -o obsid.png
# 自定义三色
python scripts/像素纹理生成.py --base 100,76,52 --spot 130,100,70 --dark 60,40,20 --seed 99 -o dirt.png
```

内置 10 个预设（mycelium/mushroom/dirt/grass_top/stone/stone_brick/obsidian/sand/netherrack/glowstone），每次输出主图 + previewx8 + tile3x3 自检。算法：周期性 value noise（无缝天然成立）+ 随机菌斑 + MC 顶光（左上亮右下暗）+ 可选调色板量化。**同 seed 必复现**。毫秒级生成，仅依赖 Pillow，无 GPU、无模型、不联网。

#### 4.2 一套"只差颜色"的物品：变色批量出（`纹理变色.py`）

**盔甲 / 木头全套（门·活板门·栅栏门·木板）/ 锻造模板 / 染料色系 / 金属宝石**——这类物品形状完全一样，
差的只是颜色。**不要一张张画，画一张底图然后变色**，一次出满一套：

```bash
python scripts/纹理变色.py --list-presets                              # 看内置色板
python scripts/纹理变色.py oak_door.png --preset woods --prefix door -o out/   # 一次出 12 种木门
python scripts/纹理变色.py base_armor.png --colors "#C8C8C8,#FFD700" --names iron,gold -o out/
python scripts/纹理变色.py base.png --preset dyes --mode hue --levels 4 -o out/  # 彩色底图换色系
python scripts/纹理变色.py --demo -o out/                               # 没底图？先生成门形示例底图
```

**原理**：拿源图的**明暗骨架**映射到目标色的暗→亮梯度上——亮暗结构、噪点细节、**透明镂空(alpha) 原样保留**，
只替换色相与饱和度。亮度基准取源图自身的 2%~98% 分位，所以底图整体偏亮/偏暗也能拉满对比。

| 色板组 | 内容 |
|---|---|
| `woods` | 木头 12 种（oak/spruce/birch/jungle/acacia/dark_oak/mangrove/cherry/bamboo/crimson/warped/pale_oak） |
| `copper` | 铜的 4 个氧化阶段（1.21.5+ 铜制物品变体） |
| `dyes` | 染料 16 色 |
| `metals` | 金属与宝石 10 种（iron/gold/copper/netherite/diamond/emerald/lapis/redstone/quartz/amethyst） |
| `stones` | 石料 10 种（stone/granite/diorite/andesite/deepslate/blackstone/sandstone/red_sand/prismarine/end_stone） |

- `--levels 4/6` 量化成像素画色阶（MC 常用 4~6 阶）；默认连续插值不产生 banding。
- `--keep-wood`：**工具类贴图必加** —— 原版所有材质的工具手柄都是同一个木色，只有头部换材质色；
  整体染色会做出"蓝柄剑""红柄镐"。加上它只染剑身/镐头，木柄保留。
- `--contrast` 拉明暗、`--gamma` 提亮暗部；`--mode hue` 只转色相（源图是**彩色**、只想换色系时用）。
- 输出：每个变体一张 + 一张 `_sheet.png` 对比图（第 1 格是源图）。
- ⚠️ 门把手、模板中心图案这类"不该跟着变色"的细节，靠**底图**区分（把它们画成极端亮度，再用 `--contrast` 拉开），别指望脚本自动识别。

> 做完把产物按注册 id 命名放进 `assets/<modid>/textures/item|block/`，文件名必须与注册 id 一致，否则紫黑方块。

### 5. 检查

跑 `scripts/检查工程.py`：校验 modid 合法性、Java/Gradle 版本匹配、资源路径与注册 id 一致性、扫描反模式、**鉴别工程出身（MCreator/lowcodefml）**。

### 6. 构建验证

```
gradlew build          # 打包
gradlew runClient      # 跑客户端（**能到主菜单、能进世界**，见 §7⑥）
gradlew runServer      # 跑服务端（务必测，很多崩溃只在专用服务端出现）
gradlew runData        # 数据生成（NeoForge 用 data / clientData）
```

> 🔴 **`build` 成功 ≠ 能进游戏。** 编译期查不出的致命问题一抓一把：缺依赖 / Mixin 崩 /
> 注册期 NPE / 画第一帧 `Missing elements in vertex`。**交付前必须过进游戏实测门禁**：
> ```bash
> python scripts/游戏内测试.py smoke --project <工程>     # 退出码 0 = 真的进得去游戏
> ```
> 想串成流水线就 `检查工程.py && gradlew build && 游戏内测试.py smoke`，任一环非零即停。
> 细节与判据见 **§8**。

> ⭐ **想在客户端里真的验一遍，别手动跑 `runClient` 空等** —— 用
> `python scripts/游戏内测试.py run --project <工程> --scenario 场景.py`：
> 它自动注入代理、进世界、走路/右键/放置/开背包/点槽位、指挥令/按键/开界面、截图、导出状态、收异常、出报告。
> 见 `references/26.x-游戏内自动化测试.md`。

### 6.5 版本号：什么时候 bump、到底改哪几处（2026-10-22 补）

**一句话**：`mod_version` 是**唯一真源**，jar 名、游戏 mod 列表里显示的版本全是它派生的 —— 只改一处就够；但**一次都不改**，它就会骗人。

本机 8 个实例实测（2026-10-22）：`mod_version` 清一色 `1.0.0`，`build/libs` 里也全是 `<显示名>-1.0.0.jar`。症状不是「不优雅」，是会真的骗人：

| 假象 | 真相 |
|---|---|
| 改了三个版本，jar 名没变过 | 分不清哪个包是新的 |
| 游戏 mod 列表里版本号从头到尾一个数 | 玩家以为没更新 |
| 往 `mods/` 拷新包，跟旧包**同名** | 资源管理器问「是否替换」点否；或文件被游戏占用、替换**静默失败** → 进游戏还是旧内容 → 开始怀疑 Mixin 没生效 / datagen 没跑（其实包就没换） |
| `mods/` 里 `xxx-1.0.0.jar` 和 `xxx-1.0.1.jar` 同时躺着 | 同 modid 双版本共存，加载哪个**不确定**（最恶心的一种，表现成随机「有时新有时旧」） |

**三处必须一致**（`版本号.py --check` 就查这三处，不一致退出码 1）：

| 位置 | 谁产生 | 判据 |
|---|---|---|
| `gradle.properties` 的 `mod_version`（Fabric 是 `version`） | **唯一真源**，人写 | 不是占位符；不是默认 1.0.0（对外发布过就该 bump） |
| `build/libs/<archivesName>-<version>.jar` | Gradle 由上面派生 | 名字里版本段对得上；目录里**不留旧版 jar** |
| jar 内 `META-INF/mods.toml`（或 `neoforge.mods.toml`）/ `fabric.mod.json` 的 `version` | `processResources` 展开 `${mod_version}` | **不能是字面 `${...}`**（说明 processResources 没跑，或手工 zip 的包）；必须等于真源 |

**别混的两件事**：`mod_version`（模组自己的版本，随你改）× `neo_version` / `minecraft_version` / `fabric_api_version`（**环境版本**，跟着玩家装的东西走）。后者**绝不能**因为「版本号要更新」被一起动 —— 把 `neo_version` 写成最新构建号，会把停在同小版本更老构建的玩家直接拒载（见 §2 尾部 `neo_range` 那段）。

```bash
python scripts/版本号.py <工程>                       # 三处并列看一眼
python scripts/版本号.py <工程> --bump patch          # 1.0.0 → 1.0.1（修 bug）
python scripts/版本号.py <工程> --bump minor          # 1.0.1 → 1.1.0（加功能）
python scripts/版本号.py <工程> --bump major          # 1.1.0 → 2.0.0（破坏存档/注册 id/配置格式）
python scripts/版本号.py <工程> --set 1.2.0-beta      # 指定（支持 -pre / +build）
python scripts/版本号.py <工程> --bump patch --clean  # 顺手删 build/libs 里的旧版产物
python scripts/版本号.py <工程> --check --mods <实例/mods>   # 装完核对
```

`--bump` 只改**源头**（`gradle.properties`）；万一 `build.gradle` 或 toml/json 里被人手改成**写死的版本号**，它也会一起改并告警（模板默认是占位符，写死本身就是 bug）。

**装包纪律（比 bump 更重要）**：往 `mods/` 放新 jar 前，**先删掉这个 mod 的旧 jar**，再拷新的。同名覆盖在 Windows 上不可靠 —— 文件被游戏占用时替换会静默失败，这是「改了没生效」出现频率最高的假故障。

**交付门禁一句话**：`版本号.py --bump` → `gradlew build` → 删旧 jar 装新 jar → `版本号.py --check --mods` → `游戏内测试.py smoke`。

### 7. 真机验证（跑起来才算数）

`gradlew build` 成功只说明编译过了。**数据包能不能解析、世界生成有没有生效、地物落不落地**，
都要跑起来才知道。三条纪律 + 一条省时间的技巧：

**① 动手写之前先离线钉死 API（省掉"编译→报错→改"的循环）**

本机 gradle 缓存里就有官方原始 jar，不用等 moddev 反编译：

```bash
# MC 侧：官方 client jar（26.1 起未混淆，类名/方法名/参数名都是真的）
python scripts/JarSig.py ~/.gradle/caches/neoformruntime/artifacts/minecraft_26.1_client.jar \
       net.minecraft.world.level.chunk.ChunkGenerator
# NeoForge 侧：universal jar（版本号必须等于工程的 neo_version）
python scripts/JarSig.py ~/.gradle/caches/modules-2/files-2.1/net.neoforged/neoforge/26.1.0.19-beta/*/neoforge-26.1.0.19-beta-universal.jar \
       net.neoforged.neoforge.registries.DeferredRegister
```
⚠️ 同一个目录下的 `minecraft_<ver>_server.jar` 是 **bundler 壳**（只有 165 条目，真正的类在
`META-INF/versions/<ver>/server-<ver>.jar` 里嵌套着）—— 拿它查签名会一路 `not found`，别浪费时间。

**② runServer 的三条纪律**

| 纪律 | 不遵守的后果 |
|---|---|
| **必须后台跑**（后台任务），别用 `nohup ... &` | 会话一返回进程就被回收，日志文件 0 字节，看起来像"启动失败" |
| 起之前清残留 java（`runServer`/`fml.modFolders` 那几个，**别杀 Gradle daemon**） | 下一次启动直接崩在 `LevelStorageSource.createLock`：另一个程序已锁定文件 |
| `pause-when-empty-seconds=0` | 无玩家 60 秒后**整个服务端暂停**（tick 全停），RCON 做的实体/时间类验证全是"没反应"的假结果 |

起服务端的判定标准：日志出现 `Done (x.xxxs)! For help, type "help"`，此时 RCON 端口才可用。

**③ 改了世界生成相关代码，必须换新世界**

区块一旦生成就写进存档，改生成器不会让旧区块重算。验证时要**停服务端 → 把 `run/<world>` 挪走（别在服务端开着时删）→ 重启**，
否则你看到的是上一版地形的缓存，会得出错误结论。

**④ 拿证据的工具**

- 世界生成类：`探地表高度.py`（地表台阶/平滑）、`按深度统计方块.py`（逐段方块分布）、
  `世界生成体检.py`（整体性体检，改完高度/密度函数第一个跑）、`密度函数引用链.py`（改 noise_settings 前必跑）
- 形状/剖面类：**`地形剖面.py`**（横/纵截面图）—— **判据证明不了"形状对不对"**，
  洞口是不是漏斗、台阶露没露、空腔连通没连通，只有摊成图才看得见；`世界预览.py`（等距预览）
- 方块/结构类：`扫区域文件.py`（非破坏性数方块）、`rcon.py`（跑命令）
- 渲染 / GUI 类：`渲染API核对.py`（见 ⑤）
- 详细套路见 `references/专用服务端自动化验证.md` 与 `references/26.x-自定义世界生成器.md`

**⑤ 渲染 / GUI 代码：签名对了照样崩，写完先跑 `渲染API核对.py`**

26.1 起 GUI 是「`GuiElementRenderState` + `RenderPipeline`」两段式，编译器**只管方法签名，不管顶点元素**。
缺元素要等进游戏画第一帧才炸（`IllegalStateException: Missing elements in vertex: UV0`）。所以**写渲染代码前**先跑：

```bash
python 渲染API核对.py --project <工程>            # 列出 state×pipeline 缺元素的组合（有问题退出码 1）
python 渲染API核对.py --project <工程> --list      # 看各 pipeline 的顶点格式
```
判定规则：`RenderPipeline` 的 `withVertexFormat(DefaultVertexFormat.X)` 里每个元素，
都必须在对应 `*RenderState.buildVertices` 里被写进去（`POSITION`→`addVertex*`、`COLOR`→`setColor`、
`TEX`→`setUv`、`LIGHT`→`setLight/setUv1`、`NORMAL`→`setNormal`）。

```
GUI                 POSITION_COLOR        → 纯色 fill/outline/线
GUI_TEXTURED        POSITION_TEX_COLOR    → 必须走 blit（BlitRenderState 才有 setUv）
❌ ColoredRectangleRenderState + GUI_TEXTURED_SNIPPET：缺 TEX
```
最后那行就是 `gfx.fill(RenderPipelines.GUI_TEXTURED, TextureSetup.singleTexture(...), ...)` 的死因——
**画贴图用 `blit`，别用带 textureSetup 的 `fill`**。细节见 `references/代码片段.md` §17.3。

**⑥ `runClient` 在本机的现实 —— 已更正，而且现在能全自动跑了**

⚠️ **旧结论"本机 runClient 只能跑到资源重载阶段、起不到游戏里"是误判，别再用。**
（2026-10-08 实测更正）客户端日志停在 `Loaded 0 entity animations` 之后**不再输出是正常的**——
主菜单不上报日志，进程不是空转，它就是在标题界面等你操作。实测证据：

```
窗口列表：'Minecraft NeoForge* 26.1'  class=GLFW30  size=584x357
PrintWindow 抓图 → 真实标题界面（Welcome to Minecraft + JAVA EDITION）
```

**教训：日志停住 ≠ 卡住。** 判"起没起来"要看窗口（`游戏内测试.py shot`）或进程状态，别拿日志末尾当结论。

所以客户端 mod 的验证阶梯变成了：
`build` 过 → `检查工程.py` 过 → `runServer` 验"服务端不加载客户端类" → **`渲染API核对.py` 静态核渲染**
→ ⭐ **`游戏内测试.py` 起真实客户端跑场景（走路/右键/放置/开背包 + 截图 + 状态断言）** → 只剩手感/观感交给人。

冷启动约 60~90 秒（热启动更快），**不要用"空等"的方式验证**——要么跑场景，要么就别起。
细节与坑见 `references/26.x-游戏内自动化测试.md`。

---

## 8. 🔴 进游戏实测门禁（delivery gate · 交付前必过）

> **一句话：mod 交付前必须真的进一次游戏，且要有可复现的证据。**
> 2026-10-13 定此规矩的原因很直白 —— 「`gradlew build` 过了」只证明**编译器没意见**，
> 证明不了**游戏愿意加载它**。下面这四类问题编译期一律静默，只有真起客户端才炸：
>
> | 编译期看不出的死法 | 表现 |
> |---|---|
> | 依赖/版本不匹配 | 启动即拒载：`Mod xxx requires neoforge 26.1.2.109 or above` |
> | Mixin 注入断裂 | `Mixin apply failed` / `Critical injection failure`，启动期崩 |
> | 注册期 NPE（如 `block == null` 的 BlockItem） | 进世界/加载注册表时崩，build 完全不知道 |
> | 渲染管线缺顶点元素 | **画第一帧才炸**：`IllegalStateException: Missing elements in vertex: UV0` |
>
> 所以：**没有跑过进游戏实测的 mod，不许说"做完了"。**

### 8.1 一条命令（判据全在里面）

```bash
python scripts/游戏内测试.py doctor --project <工程>      # 第一次先体检（JDK/注入脚本/代理）
python scripts/游戏内测试.py smoke  --project <工程>      # 🔴 进游戏实测，退出码 0 才许交付
```

**退出码**：`0` = 全部判据过（可以谈交付）｜`1` = 有判据没过（报告里点名哪条挂了）｜`2` = 参数/环境问题（路径不对、JDK 不够）。
报告落在 `<工程>/run/测试代理/冒烟报告.json`（逐条判据 + 客户端异常前 20 条），可以直接拿给用户看。

### 8.2 逐条判据（不是"起来了"，是这些全绿）

**通道 A：26.x（有进程内代理，能自动建世界进世界）**

| # | 判据 | 说明 |
|---|---|---|
| 1 | 客户端启动到主菜单 | 代理上报 `boot.ok`；这一步挂 = 启动期问题（依赖/Mixin/配置） |
| 2 | 自动建世界并进入 | `boot.data` 里 `world` + `dimension` 都在；挂在这 = 世界生成或注册期崩 |
| 3 | 推进 N tick 后 `gameTime` 在涨 | **证明世界真的在跑**，不是卡在加载界面、也没被"失焦暂停"冻住 |
| 4 | 玩家状态可读（有 `player.pos`） | 人真的站在世界里，不是空壳存档 |
| 5 | 截图非黑屏 | 亮度均值/极差判据 —— 全黑窗口是「进程活着、GL 没出图」，日志里往往一点错都没有 |
| 6 | 客户端日志无 ERROR / Exception | 兜底：前两条过了但后台在刷异常，也算没过 |

**通道 B：老版本（1.20.1 / 1.21.x 等，没有代理通道）** —— 两条子路：

**B-1 快速冒烟（只到主菜单）**：进程没中途退出 → Minecraft 窗口出现 → 日志出现主菜单标志行
（默认 `Loaded 0 entity animations`，可用 `--ready-line` 改）→ 窗口抓图非黑屏 → 日志无异常。
⚠️ 这条只验到主菜单，交付说明里必须写清"验到主菜单，未自动进世界"。

**B-2 quickPlay 视觉验收（⭐ 2026-10-14 新增，能自动进世界截图）**：
`gradlew runClient -PqpWorld=<世界名>` —— 客户端启动后直接进指定存档，配合「离线摆位 +
抓图」可以验收任何需要**看画面**的需求（天空/遮罩/渲染层级/贴图分层）。
前置与坑（全部实测过）：

| 项 | 做法 |
|---|---|
| 传参 | 必须走 `build.gradle` 的 `runs.client { if (project.hasProperty('qpWorld')) args '--quickPlaySingleplayer', ... }` —— **init script 往 args 尾巴追加会被 ForgeGradle 执行期重建覆盖**（实测：打印了日志但 ModLauncher 收到的 args 里没有） |
| 实验性警告 | `level.dat` 的 `Data.confirmedExperimentalSettings`（TAG_Byte）置 1，否则 quickPlay 卡在 `ConfirmScreen` 进不去 |
| Narrator 弹窗 | `options.txt` 预置 `pauseOnLostFocus:false`（否则弹窗挡住画面） |
| 进世界判据 | 窗口标题带 ` - Singleplayer`（**内置服务器不打印 `For help, type`**，别拿它当判据）；找主窗口要排除 FML 的 `Minecraft: Forge Loading...` splash（320×196 小窗，会先出现且抓不到真画面） |
| 摆机位 | 改 `level.dat` 的 `Data.Player`（**不是 `playerdata/*.dat`**，见反模式清单）+ NBT 类型原样保留（Rotation 必须是 TAG_Float(5)） |
| 判黑屏 | 别信均值 alone —— 全黑窗口均值也稳定；配合多机位对照（同一机位带/不带渲染开关各拍一组，均值应接近） |

工程级范例：位面工程的 `_tmp/视觉验证.py <世界名> <输出前缀> [-PnoDome]`（单轮约 70s：
进世界 → 等 15s → 连抓 3 张 → 关窗），`-PnoDome` 透传 `-Dplanes.noDome=true` 供渲染隔离实验。
⚠️ **判据设计**：客户端渲染类验收**必须有对照组**（带/不带遮罩、近机位/远机位），
单张截图什么都证明不了 —— 本轮「星界全黑」排查的最终结论（机位/渲染距离问题，不是 bug）
就是靠 5 组对照机位才收敛的。

### 8.3 挂了怎么定位（按判据号查）

| 挂在 | 先查 | 工具 |
|---|---|---|
| ① 到主菜单 | 崩溃报告/`latest.log` 的 `TRANSFORMER/<modid>@<版本>` 层；依赖版本区间（§2） | `检查工程.py`、`mixin查源.py`、`模组兼容扫描.py` |
| ② 进世界 | 注册表/方块实体/BER；`block == null` 的 BlockItem | `scan_null_blockitem.py`、§7④ 取证脚本 |
| ③ tick 不涨 | 世界卡加载 / `pauseOnLostFocus` 没关 / 死循环 | 客户端日志尾部 |
| ⑤ 黑屏 | 渲染管线顶点元素不匹配（26.1 GUI 两段式）；老版本先查「自绘遮罩关了深度写入没」+ **机位/渲染距离**（遮罩生效时抬头看天本来就该全黑） | `渲染API核对.py --project <工程>`；quickPlay 多机位对照（§8.2 B-2） |
| ⑥ 日志异常 | 逐条读栈，判是自己 mod 的还是别的 mod 的 | `游戏内测试.py smoke` 报告里的「客户端异常前20条」 |

### 8.4 硬规矩（跟 §1.6 的规划书、§7 的真机验证配套）

1. **门禁在最后、但不是可选项**：`检查工程.py` → `gradlew build` → **smoke** 才算一条完整流水线；
   任何一环非零就停，别带着红灯往下走。
2. **服务端验证替代不了它**：`runServer` 只证明"服务端不加载客户端类"，
   证明不了客户端渲染/HUD/界面/模型。**两个都要跑**。
3. **跑不了就明说**：环境不具备（JDK 装不上、代理编不出、无显卡）时，
   交付说明里必须写「⚠️ 未进游戏实测（原因：…）」，**不许用"应该能进""编译过了"糊过去** —— 这属于反幻觉约束第 3 条。
4. **改一行也要重跑**：门禁跑的是**当前代码**，不是"我上次跑过"。改了注册/渲染/Mixin/依赖后重跑一次。
5. **判据全绿 ≠ 没问题**：smoke 只证明"能进游戏且第一段时间不崩"，
   内容行为（物品效果、地形长相、界面好不好看）仍按 §7 与 `references/验证判据设计.md` 分层验收。

---

## 迁移已有模组（低版本 → 高版本）

```bash
python 迁移检查.py <工程> --to 26.1        # 出路线图
python 迁移检查.py --list                  # 看全部规则
```

三步：**盘点**（现状版本/加载器/用的 API）→ **出路线图**（哪些类/方法/资源格式变了）→ **逐级改**（不要一次跳三个大版本）。
方法论（双工作区对照法）、破坏性变更速查、26.x 专题、迁移反模式，全部在 `references/迁移指南.md`。

> **最容易犯的错**：迁移时**同时**改版本和重构逻辑 —— 出了 bug 分不清是哪边引起的。
> 先只改到能编译、能启动，再单独做重构。

## 🔥 反模式清单（照抄老教程必翻车）

完整表在 `references/版本差异矩阵.md` §4 与 `references/工程最佳实践.md` §2.5，最高频的几条：

| 反模式 | 后果 |
|---|---|
| 🔴 **`gradlew build` 过了就当交付完成**（没跑进游戏实测） | 编译期查不出：依赖不匹配（启动即拒载）、Mixin 断裂、注册期 NPE、画第一帧 `Missing elements in vertex`。**门禁：`游戏内测试.py smoke --project <工程>`（§8）** |
| 用 1.20.1 的 `Item.Properties` / `Registry` 写法套 1.21+ | 编译失败（注册表重构） |
| 🔴 **Forge 1.20.1（47.x）主类写成带参构造 `public MyMod(FMLJavaModLoadingContext ctx)`** | **编译能过、启动即崩**：`java.lang.NoSuchMethodException: com.xxx.MyMod.<init>()`。`FMLModContainer.constructMod` 只 `getDeclaredConstructor()`（无参）找构造 —— 带参注入是更高版本才有的写法。正确：无参构造 + `FMLJavaModLoadingContext.get().getModEventBus()`；注册配置用 `ModLoadingContext.get().registerConfig(...)`（**`FMLJavaModLoadingContext` 上没有 `registerConfig`**，写错编译就挂）。2026-10-22 在 47.3.0 实测，模板已修 |
| 🔴 **`defineListAllowEmpty("items", List.of(...), () -> "", pred)`**（老签名写法） | 1.20.1 的签名是 `(List<String> path, Supplier<List<? extends T>>, Predicate<Object>)`，按老签名写会报「无法推断类型变量 T#1」→ **模板开箱编译失败**。正确：`defineListAllowEmpty(List.of("items"), () -> List.of("minecraft:iron_ingot"), Config::validateItemName)` |
| 资源路径与注册 id 不一致（含大小写、含子目录） | 紫黑方块 / 模型加载失败 |
| `mod_name` 直写中文（未 `\uXXXX` 转义） | 游戏内显示乱码（Gradle 按 ISO-8859-1 读 properties） |
| `org.gradle.java.installations.paths` 里含中文路径却未转义 | 报 `Cannot find a Java installation ... languageVersion=25` + `Read timed out`，**看着像没装 JDK，其实是路径读成乱码后去联网下工具链**（加 `auto-download=false` 能让报错回到正题） |
| 多个工程各写一份 `gradle.properties`，只改了其中一个 | 同款配置漂移：一个能编、一个编不了（实测 `Abyss` vs 前置 `EndlessDepth`） |
| 版本区间写死 `[26.1]`（精确等于） | 启动报 `requires minecraft 26.1` |
| **mods.toml 的 neoforge 依赖下限用 `${neo_version}`**（把编译构建号当成版本下限） | 编译取的是最新正式版（`26.1.2.109`），产物就声明 `[26.1.2.109,)`；而玩家实例常停在同小版本更老构建（`26.1.0.19-beta`）→ **启动直接拒载**：`Mod xxx requires neoforge 26.1.2.109 or above`。下限应写小版本起点（`[26.1.0,)`，用配套的 `neo_version_range` 键），一个产物覆盖全部 26.1.x |
| 忘了 `Heightmap.primeHeightmaps` | 树浮空、结构埋地 |
| 实体模型脚底不落在 y=24 | 整只生物浮空 1.5 格 |
| **中文 jar 文件名却不给 manifest 写 `Automatic-Module-Name`** | Forge 47.x 启动即崩：`Invalid module name: '' is not a Java identifier`（securejarhandler 按文件名推模块名只认 ASCII，中文部分被清成空串）。NeoForge 不崩但照样建议写，见 §1.5 |
| **cross / 含透明像素的自定义方块不声明 cutout 渲染层** | 游戏里显示成**黑底方块**（透明像素被画成不透明黑），一眼看去像"贴图坏了"，实际贴图完全没问题。修法：model JSON 写 `"render_type": "minecraft:cutout"`。⚠️ **无头服务端根本不渲染，这类 bug 静态检查 + runServer 全绿也测不出来**，必须进客户端看（实测 2026-10-13） |
| 🔴 **Forge 专属的方块标签写到 `data/minecraft/tags/blocks/` 下**（`needs_netherite_tool` / `needs_wood_tool` / `needs_gold_tool`） | Forge 的 `Tags.Blocks.*` **全部是 `forge:` 命名空间**（`Tags.java` 里 `new ResourceLocation("forge", name)`）。写到 `minecraft` 下 = 注册成 `minecraft:needs_netherite_tool` → **Forge 根本不读** → 低级工具照样挖、照样掉。⚠️ **全程零报错**，只表现为"这个方块也太好挖了"。正确路径 `data/forge/tags/blocks/needs_netherite_tool.json`。⚠️ 顺带记：1.20.1 Forge 的 `Tier#getLevel()` **已弃用**，挖掘判定改走 `TierSortingRegistry` —— `Tiers.NETHERITE.getTag()` 返回的正是 `forge:needs_netherite_tool`，排序路径会拿它挡下所有更低 tier（实测 2026-10-14） |
| **拿 `loot … mine` 当「挖掘门槛已验证」** | `loot spawn … mine <pos>` 是**直接调战利品表**，完全不走 `isCorrectToolForDrops` —— 它只证明 loot table 写对了，证明不了「木镐挖不动」。门槛要用 FakePlayer + `Player#hasCorrectToolForDrops(BlockState)` 断言，**并且必须带对照组**（同标签组的无门槛方块应放行），否则排除不掉「该判定对方块恒为 false」的假阳性（实测 2026-10-14） |
| 🔴 **自定义世界预设：只写了 `world_preset/*.json`，没挂 `minecraft:normal` 标签** | 创建世界界面的「世界类型」列的是 **`WorldPresetTags.NORMAL` 标签的内容**，不是整个 `WORLD_PRESET` 注册表 —— 反汇编 `WorldCreationUiState#updatePresetLists()` 确认：`normalPresetList.addAll(getNonEmptyList(registry, WorldPresetTags.NORMAL))`。原版 `data/minecraft/tags/worldgen/world_preset/normal.json` 列的那五个，正好就是玩家能选到的五个。少了我们这份，预设**注册得到、注册表查得到，但玩家在界面上永远看不到它**（`extended` 标签引用了 `#minecraft:normal`，只需补 normal 一处）。⚠️ **判据必须连标签成员关系一起断言**，只查"注册表里有这个预设"是假阳性；再加一条对照（原版 `minecraft:normal` 也在该标签内）确保标签读得到（实测 2026-10-14，1.20.1 同样适用） |
| 🔴 **只看 `level.dat` 的 `SpawnX/Y/Z` 就断言"出生点已修好"** | 1.20.1 出生点要过**两道关**：① 建新世界时 `MinecraftServer#setInitialSpawn` 的 11×11 区块螺旋（可取消 `LevelEvent.CreateSpawnPosition`）；② **每次玩家登录**，`ServerPlayer` **构造函数末尾**（偏移 249）无条件调 `fudgeSpawnLocation`（SRG `m_9201_`），拿 `getSharedSpawnPos()` 当起点**再跑一遍同样的螺旋** —— 没事件、没配置、取消不掉。走螺旋的条件是 **`DimensionType.hasSkyLight()`**（主世界与有天空光的自定义维度都走），螺旋半径 = 游戏规则 `spawnRadius`（默认 10 → 21×21=441 列，**起点随机**）。**症状**：`level.dat` 里 `SpawnX/Y/Z` 明明白白是我们设的球内点，可玩家每次登录都落在球壳顶（`服务器日志 logged in with entity id N at (x, 704.0, z)`）。**出路在 `PlayerRespawnLogic` 的第二个出口**：`i = chunk.getHeight(MOTION_BLOCKING, x, z)`、`j = chunk.getHeight(WORLD_SURFACE, x, z)`，**失败(null)条件是 `j <= i && j > ocean`**（实测 i=105 / j=704 / ocean=105）；通过后返回「实心块的 `above()`」= 站立位（别漏这步）。两个高度图对**同一种方块**的取舍可以分开调：`MOTION_BLOCKING` 判据含 `blocksMotion()`，`WORLD_SURFACE` 只判 `!isAir()` —— 于是给晶壁挂 `Properties.forceSolidOff()`：球壳退出 `MOTION_BLOCKING`（i 落到球内地面）但留在 `WORLD_SURFACE`（j 仍是球壳顶）→ `j <= i` 不再成立 → 该列**通过** → 从壳内命中**球内地表站立位**。⚠️ **2026-10-14 二次修正**：早前把这解读成「每列 `j > i` → 螺旋全失败 → 回落 `moveTo`」是**读反了跳转方向**（`j > i` 恰恰是「通过」）；也把 `f_223549_` 当成 hasCeiling（tsrg 查实是 `hasSkyLight`）—— SRG 名必须查 tsrg、跳转方向必须对着字节码逐条核。⚠️ **不能用 `noCollission()` 代替**：它一次关 `hasCollision` + `canOcclude` **两个**字段（玩家穿墙 + 天光全断）。⚠️ 高度图**烘焙进区块存档**，改动只对新生成区块生效。⚠️ **落点 = worldspawn ± spawnRadius 内随机某列，≠ worldspawn 本身** —— 与原版主世界一致，别当 bug。**验证五条**：状态断言 / **反射直测判据函数**（按签名找）/ **端到端 `new ServerPlayer` 看落点**（构造完位置即定，不需登录）/ 对照（j−1 应是壳）/ 老存档信号。详见 `references/26.x-自定义世界生成器.md` §8.3（实测 2026-10-14） |
| 🔴 **模型 JSON 引用完整、静态检查全绿，但贴图文件根本不存在** | 方块模型里写了 `"textures": {"all": "planes:block/crystal_wall"}`，模型 JSON 语法没问题、引用校验（`资源引用校验.py` 若只查 JSON 层级）也过，但 `assets/planes/textures/block/crystal_wall.png` **从来没被生成过** → 游戏里就是**紫黑棋盘格**（Minecraft 的 missing texture 纹样）。⚠️ **编译 / 静态门禁 / 服务端 selftest 全都不报**，客户端只留一行很容易被日志刷走：`Missing textures in model planes:plane_core#:`。**根因常是程序化贴图脚本漏了一节** —— 这次是 `生成贴图.py` 只生成了方块贴图、忘了 4 个盔甲 item 图标与 `plane_core`（模型 JSON 却一直是齐的）。**防线**：`scripts/资源引用校验.py`（工程级 `_tmp/校验资源引用.py`）把「模型 JSON 的 texture 键」与「磁盘上真有这个 .png」交叉核对，**并要反证测试**（临时挪走一张图 → 如期报错）。**排查紫黑方块的三步**：① 看日志有没有 `Missing textures in model` ② 用校验脚本找出缺哪张 ③ 确认是缺图还是缺 `render_type`（后者见本表上面那条 cutout 反模式）。详见 `references/资源文件.md` §13（实测 2026-10-14） |
| 🔴 **smoke 的「Minecraft 窗口出现」判据只按标题含 minecraft 匹配** | 会命中桌面上**别的程序**的窗口：实测命中了 `Minecraft Dungeons II` → 判据假绿、抓的是别人的图；更糟的是收尾 `关窗口(hits[0])` 正好发给它（**等于关掉用户别的游戏**），而真客户端没人关、成了孤儿进程继续跑。**修法**（`游戏内测试.py` 已内置三层）：① 标题含 minecraft；② 窗口进程必须是 `java.exe`/`javaw.exe`（拿不到进程名时跳过该层，不判死）；③ 启动前**快照窗口基线**，只认之后新出现的窗口；④ 多候选时全部打印，别静默取 `hits[0]`（实测 2026-10-14） |
| 🔴 **无代理通道的「到主菜单」判据不验 `latest.log` 新鲜度** | `latest.log` 是上一次运行留下的时，里面早就有标志行（`Sound engine started` / `Loaded 0 entity animations`）→ 等待循环第一圈即命中 → 「到主菜单」假绿。**修法**：启动前记 mtime+size，只把「本次运行新写过的日志」当判据来源（`游戏内测试.py` 已内置）。⚠️ 与上一条是同一次 smoke 的**双重假阳性**（窗口是别人的 + 日志是上一轮的），四个判据全绿却什么都没验到（实测 2026-10-14） |
| 🔴 **自绘天空/遮罩（黑穹、虚空雾、自定义天盒）只写 `disableDepthTest()`，不关深度写入** | OpenGL 语义：`glDisable(GL_DEPTH_TEST)` 只表示「深度测试总是通过」，**深度写入仍由 `glDepthMask` 控制（默认 true = 写）**。在 `RenderLevelStageEvent.AFTER_SKY` 画一个半径 32 的黑盒盖天空 → 它往深度缓冲写下大量「近深度」→ 紧接着的地形渲染（`renderSectionLayer`，反汇编确认在 AFTER_SKY 事件**之后**）深度全部测试失败 ⇒ **地形被整片丢弃**，画面除准星/HUD 外全黑。**正解** = MC 自己画天空的做法：`disableDepthTest()` + `depthMask(false)`，画完两者都还原。⚠️ 排查此类问题**先固定机位再下结论**：球顶外「抬头看天」本来就该是全黑（遮罩生效），被误判成「修复无效」绕了一圈（实测 2026-10-14） |
| 🔴 **用 `Level#getBiome(pos)` 判「我在结构内还是结构外」这类边界** | 群系按 **4×4×4 的 quart 格**存储，`getBiome(pos)` 取的是该格**最小角**坐标算出来的群系 —— 站在球壳外一格时角点可能恰好落在半径上 ⇒ 被判成球内 ⇒ 拿到球内群系 ⇒ 客户端遮罩/效果不触发（症状：「贴着墙外侧还能看见正常的天空」）。**修法**：客户端拿不到 `ChunkGenerator`（`ClientChunkCache` 不持有），用 Forge 网络通道在玩家登录时把几何参数同步过来（`NetworkRegistry.newSimpleChannel`），客户端按相机坐标算精确距离；无几何时才退回群系判定（旧行为，边界最多错 4 格）。⚠️ 另加一道闸：只在 `dimensionType().effectsLocation()` 是自己 mod 的键时生效，去地狱/末地不误触 |
| 🔴 **改 `playerdata/<uuid>.dat` 想给单人存档摆玩家位置/朝向** | 单人游戏加载读的是 **`level.dat` 的 `Data.Player`**（`PlayerList.placeNewPlayer` 反汇编确认：离线玩家数据从 level.dat 带进来），`playerdata/*.dat` 只是**保存**时写的副本 —— 改它加载时完全不读（实测：坐标纹丝不动）。**正解**：改 `level.dat` 的 `Data.Player`（Pos/Rotation/Motion/abilities/playerGameType），`playerdata` 同时双写以防万一。⚠️ **NBT 类型必须原样保留**：`Pos`/`Motion` = TAG_List of TAG_Double(6)、`Rotation` = TAG_List of **TAG_Float(5)**（写成 Double 会让 `Entity.load` 的 `getList("Rotation", 5)` 返回空列表 → **朝向静默丢失**）、`abilities.*` = TAG_Byte(1)。写 NBT 库时让 Compound/NList 携带原始类型，别用 `_infer` 猜（实测 2026-10-14） |
| 🔴 **`gradlew build 2>&1 \| iconv -f GBK -t UTF-8 \| grep ...` 把整条流水线挂死** | 构建「跑了四分钟没动静」，其实 13 秒就编完了 —— iconv 碰到非法字节不再输出、grep 一直等、gradle 往管道写被堵成死锁。**改法**：输出先重定向到文件（`> /tmp/build.log 2>&1`），再对文件 `iconv` / `grep`。**判据**：java 进程 CPU 归零但任务不结束 → 先怀疑管道，别怀疑编译（实测 2026-10-22，同一坑连踩两次） |
| 🔴 **在 Windows 客户端日志上 grep 中文关键词** | 日志文件里中文常以 GBK/乱码落盘（`[位面/调试]` → `[λ��/����]`），grep 中文子串**零命中**，会误判「日志没输出/功能没触发」。**修法**： grep 纯英文子串（类名、`AFTER_SKY`、`ERROR`）或先转码。判据设计同理：**别拿中文日志行当门禁判据**（实测 2026-10-14：一度以为调试日志没打，其实一直在打） |
| 🔴 **在自绘天空/遮罩里往 `POSITION_COLOR` 塞顶点，却不 `setShader(getPositionColorShader)`** | `AFTER_SKY` 阶段当前 shader 是原版天空盒最后用的那个（**带 UV**），顶点格式与 shader 不匹配 ⇒ GL 按错位属性解释 ⇒ 画出来是**散落的深色块状物**，且**日志零报错**。典型踩法：黑穹（纯色，有这行）与天穹（渐变，漏这行）两段几乎一样的代码，复制时漏一行。α 渐隐还要 `enableBlend()/defaultBlendFunc()`（原版天空盒画完时混合是关的）。**同类渲染代码要成对 review**；详见 `references/专用服务端自动化验证.md` §7.2（实测 2026-10-14） |
| 🔴 **自绘天空写死 RGB，不看 `getSkyColor`** | 写死白天色的后果是「**夜里地面全黑、天却是白的**」——一眼就不像天空。正解：颜色取 `ClientLevel#getSkyColor(pos, partialTick)`（群系 × 昼夜 × 天气），夜里按亮度**淡出**（alpha→0）把星空与月亮还给原版。另：天空层**盖不住比它更远的真方块**（不写深度 + 地形后画）—— 看到「天上一块块板子」先确认是不是自己的球壳/穹顶，再动渲染代码（详见 §7.3） |
| 🔴 **用 `options.txt` 做对照实验，却写错了键名** | 1.20.1 关原版云的键是 **`renderClouds`**（`"true"/"fast"/"false"`），**不是** `clouds`。错键会被 MC 保存时**静默丢弃**（下一轮整行消失）⇒「关了却没关」的无效对照。**判据：改完跑一轮，看这行还在不在**。（`renderDistance`/`guiScale`/`fov`/`pauseOnLostFocus` 同在这个文件；摆位改的是 `level.dat`） |
| 🔴 **Windows 显示缩放 ≠ 100% 时用 PrintWindow 抓图** | DPI-unaware 进程的 `GetWindowRect` 返回**逻辑像素**、位图只有真实窗口左上 ~2/3 ⇒ 准星不在图中心、热键栏看不见、HUD 元素缺失，**很容易误判成「HUD 没画出来」**（本轮实测就是这么错的）。修法：抓图前 `ctypes.windll.shcore.SetProcessDpiAwareness(2)`（见 `references/专用服务端自动化验证.md` §7.1） |
| 🔴 **`C:\Users\<用户>\.gradle` 是 junction 到别的盘时，`gradlew` 建不了锁文件** | 症状：`Could not create parent directory for lock file C:\Users\...\.gradle\wrapper\dists\...\gradle-8.8-bin.zip.lck`（而 `wrapper/dists` 明明存在）；PowerShell 枚举直接报 `the path cannot be traversed because it contains an untrusted mount point`（`Get-Item` 看是 `Junction`）。**修法**：`GRADLE_USER_HOME` 指到 junction 的**真实目标**（缓存都在那儿）再跑 `gradlew`（实测 2026-10-14，构建立刻恢复） |

## 关于 26.x 的重大变化（2026）

- **26.1 起彻底移除混淆** —— 类名/方法名/参数名都是真的，`JarSig.py` 读出来即可用，没有 mapping 层。
- Java 要求升到 **25**。
- 三大维度定义（`data/minecraft/dimension/*.json`）**从原版 jar 里消失** → 「加新维度」不能再靠数据包文件；改地形走**世界预设**。
- GUI 变「`GuiElementRenderState` + `RenderPipeline`」两段式 → 必须跑 `渲染API核对.py`。
- FTB Quests 等数据格式转 JSON5（本 skill 不涉及，见 MC一站式工坊）。

## 反幻觉约束（硬性）

1. **不编造 API**。任何类名/方法名/签名，先 `JarSig.py` 从真实 jar 读，读不到就说读不到，别按印象写。
2. **不编造版本号**。NeoForge / Fabric API 版本号要查实（`新建工程.py` 会自动拉最新，或查 maven）。
3. **区分「编译过」与「跑起来对」**。交付时必须说清验证到了哪一层（静态检查 / build / runServer / 实机）。
   🔴 **「实机」这一层必须有 `游戏内测试.py smoke` 的退出码 0 支撑**（§8）——
   说"应该能进""编译过了所以没问题"等同没验证；环境确实跑不了就写「⚠️ 未进游戏实测（原因）」。
4. **不把整合包知识混进来**。本 skill 只谈 mod 源码工程；涉及实例、依赖闭包、打包的，明确转 MC一站式工坊。
5. ⭐ **不把「判据全绿」当成「没问题」**。判据变绿只说明"这个判据没发现问题"。
   生成类内容（地形/结构/方块摆放）的验收**必须分三层**（离线纯函数 → 存档判据 → 出图），
   且判据本身要按"成对样本 + 完整性 + 行为规则 + 边界余量"四条铁律设计。
   **细节见 `references/验证判据设计.md`** —— 里面记了一个真实的教训：
   十五轮迭代里**判据自己出错 5 次**（比地形代码出错还多），每次都是"全绿"→ 交付 → 用户一眼看出不对。
6. 🔴 **不脱离规划写码**（§1.6）。没有规划书不开工；实现与规划不一致时，
   以规划书为准或在改规划之后同步代码——交付时说清"实际产出 vs 规划"的每一条偏差。
