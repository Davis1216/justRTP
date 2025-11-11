# JustRTP

<p align="center">
<img src="https://i.imgur.com/dUlEG4p.png" alt="JustRTP 标志" width="1000"/>
</p>

<p align="center">
<strong>高性能随机传送插件，适用于Paper、Folia和代理网络，具有高级区域系统</strong>
</p>

<p align="center">
<img src="https://img.shields.io/badge/Version-3.3.1-brightgreen?style=for-the-badge" alt="版本" />
<img src="https://img.shields.io/badge/API-1.21+-blue?style=for-the-badge" alt="API 版本" />
<img src="https://img.shields.io/badge/Java-21+-orange?style=for-the-badge" alt="Java" />
<img src="https://img.shields.io/badge/Folia-Supported-purple?style=for-the-badge" alt="Folia" />
</p>

<p align="center">
<a href="https://discord.gg/your-invite-code">
<img src="https://img.shields.io/discord/YOUR_SERVER_ID?color=5865F2&label=Discord&logo=discord&logoColor=white&style=for-the-badge" alt="Discord" />
</a>
</p>

---

## 概述

JustRTP是一款功能丰富的随机传送插件，专为现代Minecraft服务器设计。以性能和可靠性为核心，支持单服务器、Velocity/BungeeCord网络和Folia的多线程区域。

### 主要特性

- **人性化时间格式** - 将冷却时间显示为"5分38秒"而不是"338秒"
- **OP/权限冷却绕过** - OP玩家和拥有绕过权限的玩家可以跳过冷却时间
- **空消息抑制** - 通过在messages.yml中将消息设置为""来隐藏任何消息
- **基于半径的定价** - 根据选择的传送半径收取不同价格
- **按世界冷却** - 每个世界独立的冷却时间 - 自由在维度间传送！
- **高性能** - 异步位置缓存和优化的搜索算法
- **安全优先** - 具有维度特定扫描的智能地形分析
- **跨服务器支持** - 通过MySQL后端实现网络范围内的传送
- **RTP区域** - 竞技场风格区域，带有倒计时全息图和团队传送
- **三层全息图系统** - FancyHolograms、PacketEvents或Display Entities，自动选择
- **持久全息图** - FancyHolograms集成，玩家可编辑，重启不丢失
- **丰富的视觉效果** - 支持MiniMessage格式，包括标题、粒子和特效
- **经济系统集成** - Vault支持，每次传送收费和基于半径的定价
- **灵活配置** - 按世界设置、权限组和自定义半径
- **钩子支持** - WorldGuard区域、PlaceholderAPI、FancyHolograms和PacketEvents
- **Folia就绪** - 完全支持多线程区域服务器  

---

## 安装

### 快速开始

1. 从发布页面下载`JustRTP.jar`
2. 放置在服务器的`plugins`文件夹中
3. 重启服务器以生成配置文件
4. 根据需要配置`config.yml`
5. 使用`/rtp reload`重新加载

### 依赖项

**必需：**
- Java 21或更高版本
- Paper 1.21+（或Folia、兼容Spigot 1.21+）

**可选：**
- **Vault** - 经济和权限组支持
- **PlaceholderAPI** - 供其他插件使用的占位符扩展
- **WorldGuard** - 区域保护集成
- **FancyHolograms** - 区域的美丽持久全息图（1.21+ Paper/Folia）
- **PacketEvents** - 高性能基于数据包的全息图
- **MySQL** - 跨服务器传送

### 跨服务器设置

要实现网络范围的`/rtp <服务器>:<世界>`功能：

1. 在所有网络服务器上安装JustRTP
2. 设置共享MySQL数据库
3. 在`mysql.yml`中配置连接详情：
   ```yaml
   enabled: true
   host: "your-mysql-host"
   database: "justrtp"
   username: "your-username"
   password: "your-password"
   ```
4. 在`config.yml`中设置唯一的服务器名称：
   ```yaml
   proxy:
     this_server_name: "survival"  # 每个服务器唯一
   ```
5. 重启所有服务器

---

## 命令

### 玩家命令

| 命令 | 描述 | 权限 | 默认 |
|---------|-------------|------------|---------|
| `/rtp` | 在当前世界传送 | `justrtp.command.rtp` | 所有人 |
| `/rtp <世界>` | 传送到特定世界 | `justrtp.command.rtp.world` | OP |
| `/rtp <服务器>` | 传送到另一服务器 | `justrtp.command.rtp.server` | OP |
| `/rtp <服务器>:<世界>` | 跨服务器世界传送 | `justrtp.command.rtp.server` | OP |
| `/rtp <玩家>` | 传送其他玩家 | `justrtp.command.rtp.others` | OP |
| `/rtp <最小> <最大>` | 自定义半径传送 | `justrtp.command.rtp.radius` | OP |
| `/rtp confirm` | 确认付费传送 | `justrtp.command.confirm` | 所有人 |
| `/rtp help` | 显示帮助信息 | `justrtp.command.rtp` | 所有人 |
| `/rtp credits` | 显示插件鸣谢 | `justrtp.command.credits` | 所有人 |
| `/rtpzone ignore` | 切换区域参与状态 | `justrtp.command.zone.ignore` | 所有人 |

### 管理员命令

| 命令 | 描述 | 权限 | 默认 |
|---------|-------------|------------|---------|
| `/rtp reload` | 重新加载所有配置 | `justrtp.command.reload` | OP |
| `/rtp proxystatus` | 检查MySQL连接 | `justrtp.admin` | OP |
| `/rtpzone setup <id>` | 创建新RTP区域 | `justrtp.admin.zone` | OP |
| `/rtpzone delete <id>` | 删除现有区域 | `justrtp.admin.zone` | OP |
| `/rtpzone list` | 列出所有配置的区域 | `justrtp.admin.zone` | OP |
| `/rtpzone cancel` | 取消正在进行的设置 | `justrtp.admin.zone` | OP |
| `/rtpzone sethologram <id>` | 向区域添加全息图 | `justrtp.admin.zone` | OP |
| `/rtpzone delhologram <id>` | 删除区域全息图 | `justrtp.admin.zone` | OP |
| `/rtpzone sync` | 显示区域同步状态 | `justrtp.admin.zone` | OP |
| `/rtpzone push` | 将区域推送到数据库 | `justrtp.admin.zone` | OP |
| `/rtpzone pull` | 从数据库拉取区域 | `justrtp.admin.zone` | OP |
| `/rtpzone status` | 显示同步信息 | `justrtp.admin.zone` | OP |

### 命令别名

`/rtp`的可用别名：`jrtp`、`wild`、`randomtp`  
`/rtpzone`的可用别名：`rtpzones`

别名可以在`commands.yml`中禁用

---

## 配置

JustRTP使用模块化配置文件，以便于组织和管理：

### 核心配置文件

| 文件 | 用途 |
|------|---------|
| `config.yml` | 主要设置、世界配置、性能选项 |
| `messages.yml` | 所有文本输出，支持MiniMessage格式 |
| `mysql.yml` | 跨服务器功能的数据库连接 |
| `redis.yml` | 高级缓存的Redis配置 |
| `rtp_zones.yml` | 竞技场风格区域定义 |
| `animations.yml` | 粒子效果配置 |
| `holograms.yml` | 区域全息图显示设置 |
| `commands.yml` | 命令别名开关 |
| `cache.yml` | 位置缓存设置 |
| `display_entities.yml` | 显示实体配置 |

### 关键配置选项

**基本设置（`config.yml`）：**
```yaml
cooldown: 300              # 默认冷却时间（秒）
delay: 3                   # 传送前准备时间
max_attempts: 30           # 位置搜索尝试次数
min_radius: 100            # 距离出生点的最小距离
max_radius: 10000          # 距离出生点的最大距离

economy:
  enabled: true            # 要求付费使用RTP
  cost: 100.0              # 每次传送的默认费用
```

**世界特定设置：**
```yaml
worlds:
  world:
    enabled: true
    type: OVERWORLD         # 或NETHER、THE_END、CUSTOM
    min_radius: 500
    max_radius: 15000
    cooldown: 300           # 每个世界独立冷却！
    cost: 50.0
    
  world_nether:
    enabled: true
    type: NETHER
    min_y: 32               # 安全的下界Y坐标
    max_y: 120
    cooldown: 180           # 下界冷却时间更短！
```

**基于半径的定价系统**

根据玩家选择的传送半径收取不同价格：

```yaml
custom_worlds:
  world:
    cost: 100.0             # 基础费用（始终收取）
    
    # 基于半径的定价（可选）
    radius_pricing:
      enabled: true
      tiers:
        tier1:
          max_radius: 5000   # 小半径
          cost: 0.0          # 无额外费用
        tier2:
          max_radius: 15000  # 中等半径
          cost: 250.0        # +$250额外费用
        tier3:
          max_radius: 25000  # 大半径
          cost: 500.0        # +$500额外费用
```

**示例费用：**
- `/rtp`（默认）→ $100（仅基础费用）
- `/rtp 5000` → $100（基础 + tier1）
- `/rtp 15000` → $350（基础 + tier2）
- `/rtp 20000` → $600（基础 + tier3）

**按世界冷却系统：**

每个世界都有**独立**的冷却时间！玩家可以在世界之间自由传送而无需等待：

```yaml
custom_worlds:
  world:
    cooldown: 900           # 主世界15分钟
    cost: 150.0
    
  world_nether:
    cooldown: 300           # 下界5分钟
    cost: 300.0
    
  world_the_end:
    cooldown: 900           # 末地15分钟
    cost: 500.0
```

**示例游戏流程：**
1. 玩家输入`/rtp world_the_end` → 成功传送（world_the_end的15分钟冷却开始）
2. 玩家输入`/rtp world_nether` → 成功传送（world_nether的5分钟冷却开始）
3. 玩家输入`/rtp world` → 成功传送（world的15分钟冷却开始）
4. 玩家输入`/rtp world_the_end` → 被阻止！（world_the_end的冷却仍在进行中）

**优势：**
- ✅ 无需全局冷却即可探索多个维度
- ✅ 每个世界有适当的冷却设置
- ✅ 更好的游戏流程和玩家自由度
- ✅ 与PlaceholderAPI配合使用，显示每个世界的信息

**自动化功能：**
```yaml
first_join_rtp:
  enabled: true             # 首次加入时自动RTP
  world: "world"            # 目标世界
  delay: 5                  # 加入后延迟

respawn_rtp:
  enabled: false            # 重生时自动RTP
  only_first_death: true    # 每个会话仅首次死亡
```

**安全与性能：**
```yaml
respect_regions: true       # WorldGuard集成
avoid_lava: true            # 跳过熔岩位置
avoid_water: true           # 跳过水域位置
unsafe_blocks: true         # 检查危险方块

location_cache:
  enabled: true             # 预计算安全位置
  size: 100                 # 每个世界的位置数量
  cleanup_interval: 3600    # 每小时清理一次
```

**生物群系控制：**
```yaml
biome_blacklist:
  - DEEP_OCEAN
  - FROZEN_OCEAN
  - DEEP_COLD_OCEAN
```

---

## RTP区域

动态竞技场风格区域，支持自动团队传送和同步倒计时。

### 设置流程

1. **创建区域：**
   ```
   /rtpzone setup <zone_id>
   ```

2. **选择区域范围：**
   - 获得烈焰棒选择工具
   - **左键点击**方块设置第一个位置
   - **右键点击**方块设置第二个位置

3. **配置区域（交互式提示）：**
   - 传送间隔（传送之间的秒数）
   - 传送的目标世界
   - 距离目标世界出生点的最小/最大半径
   - 可选：跨服务器目标
   - 可选：团队玩家的自定义分散半径
   - 可选：全息图位置（站在想要放置的位置并确认）

4. **自动创建全息图**
   - 如果设置了全息图位置，它会自动创建
   - 引擎根据`preferred-engine`设置自动选择
   - 全息图立即渲染并显示倒计时
   - 无需手动使用`/rtpzone sethologram`命令！

5. **手动管理全息图（可选）：**
   ```
   /rtpzone sethologram <zone_id>    # 添加/移动全息图
   /rtpzone delhologram <zone_id>    # 删除全息图
   ```

6. **验证配置：**
   ```
   /rtpzone list      # 显示所有区域
   /rtpzone status    # 显示同步状态
   /fholo list        # 显示FancyHolograms（如果使用FancyHolograms引擎）
   ```

### 区域特性

**玩家体验：**
- 进入/离开区域时自动检测
- 实时倒计时显示：
  - **标题消息**：大型倒计时显示（5秒，4秒，3秒...）
  - **动作栏**：持续显示倒计时
  - **全息图**：可选的浮动倒计时显示，提供3种引擎选项：
    - **FancyHolograms** 美丽持久的全息图，支持MiniMessage
    - **PacketEvents**：高性能基于数据包的全息图
    - **Display Entities**：原生原版实体，无依赖
- 智能多玩家传送，自动分散
  - 每个玩家获得唯一的安全位置
  - 可配置的分散距离防止聚集
  - 针对Folia的多线程区域进行了优化
  - 所有玩家同时传送，无延迟
- 倒计时每秒钟的音效
- 传送时的粒子效果

**区域管理：**
- 区域设置时自动创建全息图
  - 无需手动使用`/rtpzone sethologram`
  - 全息图自动选择最佳可用引擎
  - 创建时即时渲染（无延迟）
- 通过MySQL同步支持跨服务器目标
- 每个区域可自定义传送间隔
- 玩家个人选择退出：`/rtpzone ignore`
- 先进的多玩家传送系统
  - 每个玩家获得唯一位置（不再有同一点生成！）
  - 可配置玩家之间的最小/最大分散距离
  - 每个区域的分散设置覆盖全局默认值
  - 全维度安全（下界Y < 127，末地岛屿等）
- 区域在网络服务器间自动同步

**全息图系统**
- **三引擎架构**，智能回退：
  - **FancyHolograms**：高级视觉效果，持久存储，玩家可编辑
  - **PacketEvents**：高性能，基于数据包渲染
  - **Display Entities**：原版回退方案，无需任何依赖
- **自动引擎选择**，通过`preferred-engine`配置选项：
  - `auto` - 智能检测（FancyHolograms > PacketEvents > Entities）
  - `fancyholograms` - 强制使用FancyHolograms（需要插件）
  - `packetevents` - 强制使用PacketEvents（需要插件）
  - `entity` - 强制使用原版Display Entities
- **自动创建**：区域设置时全息图自动创建
- **即时渲染**：无延迟，所有引擎上的全息图立即显示
- **FancyHolograms持久性**：
  - 全息图保存到FancyHolograms存储中
  - 在`/fholo list`中对服务器管理员可见
  - 玩家可通过FancyHolograms命令编辑全息图
  - 服务器重启后仍然存在（由FancyHolograms管理）
  - 区域删除时正确清理
- **实时重载支持**：通过`/rtp reload`更新全息图模板
- **模板缓存**：更新时零配置I/O（性能提升85%）
- 动态倒计时更新（5秒→4秒→3秒→2秒→1秒→传送！）
- 完全支持MiniMessage格式（渐变，彩虹，悬停等）
- 通过`holograms.yml`自定义全息图消息

### 区域配置示例

**`rtp_zones.yml`：**
```yaml
zones:
  pvp-arena:
    world: "world"
    pos1:
      x: -50.0
      y: 64.0
      z: -50.0
    pos2:
      x: 50.0
      y: 80.0
      z: 50.0
    
    interval: 60              # 每60秒传送一次
    target: "arena_world"     # 目标世界或服务器:世界
    
    min-radius: 1000          # 距离世界出生点的最小距离
    max-radius: 10000         # 距离世界出生点的最大距离
    
    # 玩家分散配置
    min-spread-distance: 10   # 玩家之间的最小距离（方块）
    max-spread-distance: 50   # 玩家之间的最大距离（方块）
    
    hologram:
      location:
        x: 0.5
        y: 75.0
        z: 0.5
      view-distance: 64
```

**`config.yml`中的全局分散设置：**
```yaml
zone_teleport_settings:
  # 如果区域未指定自己的值，则使用这些默认值
  min_spread_distance: 5    # 最小距离（防止聚集）
  max_spread_distance: 15   # 最大距离（保持团队在一起）
```

**多玩家分散工作原理：**
1. 区域倒计时归0
2. 收集区域内的所有玩家
3. 每个玩家通过RTPService获得唯一的安全位置
4. 检查位置确保它们之间至少有min_spread_distance距离
5. 所有玩家同时传送（兼容Folia）
6. 结果：没有两个玩家会在完全相同的位置生成！

**`holograms.yml`：**
```yaml
# 首选全息图引擎
# 选项：auto, fancyholograms, packetevents, entity
preferred-engine: auto

hologram-settings:
  line-spacing: 0.35      # 行间距
  y-offset: 2.5           # 区域中心上方高度
  scale: 1.0              # 文本大小倍率
  
  # 全息图文本行（支持MiniMessage）
  lines:
    - "<gradient:#20B2AA:#7FFFD4><bold>⚡ RTP区域</bold></gradient>"
    - "<gray>━━━━━━━━━━━━━━━━</gray>"
    - "<gradient:#FFD700:#FFA500><bold>⏱ <time>秒</bold></gradient>"
    - "<gray>━━━━━━━━━━━━━━━━</gray>"
    - "<aqua>区域: <yellow><zone></yellow></aqua>"
  
  # 可用占位符：
  # <time> 或 <countdown> - 倒计时（60, 59, 58... 3, 2, 1）
  # <zone> 或 <zone_id> - 区域标识符
  
  # MiniMessage示例：
  # <gradient:#FF0000:#00FF00>文本</gradient> - 颜色渐变
  # <rainbow>彩虹文本</rainbow> - 彩虹效果
  # <bold>粗体</bold> <italic>斜体</italic> - 文本样式
  # <#FF5733>自定义十六进制颜色</#FF5733> - 自定义十六进制颜色
```

**FancyHolograms集成**
当安装了FancyHolograms且`preferred-engine`设置为`auto`或`fancyholograms`时：
- 全息图是**持久的** - 保存到FancyHolograms数据库
- 在`/fholo list`中可见，命名格式为：`justrtp_zone_<zoneid>`
- 管理员可通过FancyHolograms命令完全编辑
- 服务器重启后仍然存在（由FancyHolograms管理）
- 区域删除时自动清理
- 模板更改通过`/rtp reload`应用（无需重启FancyHolograms）

### MySQL区域同步

对于跨服务器区域，区域通过MySQL自动同步：

**将区域推送到数据库：**
```
/rtpzone push
```

**从数据库拉取区域：**
```
/rtpzone pull
```

**检查同步状态：**
```
/rtpzone sync
/rtpzone status
```

---

## 全息图引擎系统

JustRTP具有复杂的三层全息图引擎系统，支持自动回退和智能引擎选择。

### 全息图引擎选项

**1. FancyHolograms（推荐用于1.21+）**
- **要求**：FancyHolograms 2.8.0+插件，Paper/Folia 1.21+
- **特性**：
  - 美丽、高质量的全息图渲染
  - 持久存储（服务器重启后仍然存在）
  - 在`/fholo list`中可见，名为`justrtp_zone_<zoneid>`
  - 通过FancyHolograms命令玩家可编辑
  - 完全支持MiniMessage（渐变，彩虹，动画）
  - 模板缓存，零配置I/O开销
- **最适合**：想要高级视觉效果和持久全息图的服务器

**2. PacketEvents（高性能）**
- **要求**：PacketEvents插件
- **特性**：
  - 基于数据包的渲染（无实体）
  - 高性能，低开销
  - 即时渲染和更新
  - 完全支持MiniMessage
- **最适合**：高流量服务器，优先考虑性能

**3. Display Entities（通用回退）**
- **要求**：无（原版Minecraft）
- **特性**：
  - 适用于任何Paper/Folia服务器
  - 无需依赖
  - 原生Display Entity支持
  - 完全支持MiniMessage
- **最适合**：没有全息图插件的服务器，保证兼容性

### 引擎选择

在`holograms.yml`中配置：

```yaml
# 首选全息图引擎
# 选项：auto, fancyholograms, packetevents, entity
preferred-engine: auto
```

**引擎选择模式：**

| 模式 | 行为 |
|------|----------|
| `auto` | 智能检测，优先级：FancyHolograms > PacketEvents > Display Entities |
| `fancyholograms` | 强制使用FancyHolograms（未安装则报错） |
| `packetevents` | 强制使用PacketEvents（未安装则报错） |
| `entity` | 强制使用Display Entities（始终可用） |

**自动检测工作原理：**
1. 检查是否安装了FancyHolograms 2.8.0+ → 使用FancyHolograms
2. 否则，检查是否安装了PacketEvents → 使用PacketEvents
3. 否则，回退到Display Entities → 始终有效

### 全息图生命周期

**自动创建**
- 全息图在区域设置时自动创建
- 无需手动使用`/rtpzone sethologram`命令
- 引擎根据`preferred-engine`设置自动选择
- 强制刷新所有引擎上的即时渲染

**实时更新：**
- 倒计时每秒更新（60秒→59秒→58秒...→1秒）
- 通过`/rtp reload`应用的模板更改立即生效
- FancyHolograms：更改在重启间保持
- PacketEvents/Entities：更改应用于活动全息图

**自动清理：**
- 区域删除时全息图被移除
- FancyHolograms：也从`/fholo list`和持久存储中移除
- PacketEvents：为附近玩家清除数据包
- Entities：正确移除显示实体

### 模板配置

**`holograms.yml` - 全局模板：**
```yaml
hologram-settings:
  line-spacing: 0.35      # 行之间的垂直距离
  y-offset: 2.5           # 区域中心上方高度
  scale: 1.0              # 文本大小（1.0 = 正常）
  
  lines:
    - "<gradient:#20B2AA:#7FFFD4><bold>⚡ RTP区域</bold></gradient>"
    - "<gray>━━━━━━━━━━━━━━━━</gray>"
    - "<gradient:#FFD700:#FFA500><bold>⏱ <time>秒</bold></gradient>"
    - "<gray>━━━━━━━━━━━━━━━━</gray>"
    - "<aqua>区域: <yellow><zone></yellow></aqua>"
```

**可用占位符：**
- `<time>` 或 `<countdown>` - 当前倒计时（秒）
- `<zone>` 或 `<zone_id>` - 区域标识符

**MiniMessage格式示例：**
```yaml
# 渐变
"<gradient:#FF0000:#00FF00>渐变文本</gradient>"

# 彩虹效果
"<rainbow>彩虹文本</rainbow>"

# 文本样式
"<bold>粗体</bold> <italic>斜体</italic> <underlined>下划线</underlined>"

# 自定义十六进制颜色
"<#FF5733>自定义颜色</#FF5733>"

# 悬停文本（仅FancyHolograms）
"<hover:show_text:'更多信息'>悬停我</hover>"
```

### FancyHolograms集成

**持久全息图优势：**
- **保存到数据库**：全息图服务器重启后仍然存在
- **管理员编辑**：通过`/fholo edit justrtp_zone_<zoneid>`编辑
- **玩家可见性**：在`/fholo list`中可见，便于管理
- **自动同步**：JustRTP更新应用到FancyHolograms存储
- **干净移除**：删除区域会从FancyHolograms中移除全息图

**区域全息图的FancyHolograms命令：**
```bash
# 列出所有全息图（包括JustRTP区域）
/fholo list

# 直接编辑区域全息图
/fholo edit justrtp_zone_pvp-arena

# 传送到全息图位置
/fholo teleport justrtp_zone_pvp-arena

# 注意：JustRTP管理生命周期，不建议手动删除
```

**模板缓存系统：**
- 全息图模板在启动时加载一次
- 缓存在内存中（Map<String, List<String>>）
- 更新过程中零配置文件读取
- I/O操作减少85%
- 占位符在运行时应用

### 性能优化

**引擎性能比较：**

| 引擎 | CPU影响 | 内存 | 依赖 | 持久性 |
|--------|------------|--------|--------------|-------------|
| FancyHolograms | 低 | 低 | ✅ 需要 | ✅ 是 |
| PacketEvents | 极低 | 极低 | ✅ 需要 | ❌ 否 |
| Display Entities | 低 | 低 | ❌ 无 | ❌ 否 |

**优化提示：**
- 使用`auto`模式获得最佳平衡
- FancyHolograms：最适合持久、可编辑的全息图
- PacketEvents：最适合高玩家数量服务器
- Display Entities：最适合最大兼容性
- 在区域配置中调整`view-distance`以限制渲染范围
- 模板缓存是自动的（无需配置）

### 全息图故障排除

**全息图不可见：**
1. 检查引擎是否可用：`/rtp reload`显示活动引擎
2. 验证区域是否有全息图：`/rtpzone list`
3. 检查渲染距离（默认：64格）
4. FancyHolograms：使用`/fholo list`验证
5. 在`config.yml`中启用调试模式查看详细日志

**倒计时卡在0秒：**
- 在3.2.9版本中通过模板缓存系统修复
- 确保运行最新版本
- 运行`/rtp reload`刷新模板

**FancyHolograms未在/fholo list中显示：**
- 确保安装了FancyHolograms 2.8.0+
- 验证holograms.yml中设置了`preferred-engine: auto`或`fancyholograms`
- 在控制台中检查"使用FancyHolograms引擎"消息
- 如果从旧版JustRTP升级，删除并重新创建区域

**使用了错误的引擎：**
- 检查holograms.yml中的`preferred-engine`设置
- 验证所需插件已安装并加载
- 控制台在`/rtp reload`时显示活动引擎
- 使用`entity`模式获得保证的回退方案

---

## 权限

### 核心权限

| 权限 | 描述 | 默认 |
|------------|-------------|---------|
| `justrtp.command.rtp` | 在当前世界使用`/rtp` | 所有人 |
| `justrtp.command.rtp.world` | 传送到特定世界 | OP |
| `justrtp.command.rtp.server` | 跨服务器传送 | OP |
| `justrtp.command.rtp.others` | 传送其他玩家 | OP |
| `justrtp.command.rtp.radius` | 使用自定义半径值 | OP |
| `justrtp.command.confirm` | 确认付费传送 | 所有人 |
| `justrtp.command.reload` | 重新加载插件配置 | OP |
| `justrtp.command.credits` | 查看插件鸣谢 | 所有人 |
| `justrtp.command.zone.ignore` | 切换区域参与状态 | 所有人 |
| `justrtp.admin` | 完全管理员访问（代理状态等） | OP |
| `justrtp.admin.zone` | 管理RTP区域 | OP |

### 绕过权限

| 权限 | 描述 | 默认 |
|------------|-------------|---------|
| `justrtp.cooldown.bypass` | 跳过冷却时间 | OP |
| `justrtp.bypass.cooldown` | 旧版冷却绕过（已弃用） | OP |
| `justrtp.bypass.delay` | 跳过传送延迟 | OP |
| `justrtp.bypass.cost` | 跳过经济费用 | OP |

### 通配符权限

| 权限 | 描述 | 默认 |
|------------|-------------|---------|
| `justrtp.*` | 所有插件权限 | OP |
| `justrtp.command.*` | 所有命令权限 | OP |
| `justrtp.bypass.*` | 所有绕过权限 | OP |
| `justrtp.group.*` | 所有权限组 | OP |

### 权限组

在`config.yml`中为每个权限组配置自定义设置：

```yaml
permission_groups:
  vip:
    cooldown: 60          # 1分钟冷却
    delay: 1              # 1秒延迟
    cost: 0               # 免费传送
    min_radius: 100       # 自定义半径
    max_radius: 10000
    permission: "justrtp.group.vip"
    
  mvp:
    cooldown: 30          # 30秒冷却
    delay: 0              # 即时传送
    cost: 0
    max_radius: 20000     # 更大半径
    permission: "justrtp.group.mvp"
    
  premium:
    cooldown: 0           # 无冷却
    delay: 0
    cost: 0
    auto_rtp_respawn: true  # 死亡时自动RTP
    permission: "justrtp.group.premium"
```

玩家会获得他们有权访问的最高优先级组的设置。优先级由`config.yml`中定义的顺序决定。

---

## PlaceholderAPI

**要求**：安装[PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/)才能使用这些占位符。

所有占位符都支持每个玩家的上下文和特定于世界的设置：

### 世界特定占位符支持

**格式**：`%justrtp_<placeholder>_<worldname>%`

现在您可以获取**特定世界**的冷却时间、成本和延迟，而不仅仅是玩家当前所在的世界！

**示例：**
- `%justrtp_cooldown_world_nether%` - world_nether的冷却时间
- `%justrtp_cost_world%` - 名为"world"的世界的成本
- `%justrtp_delay_world_the_end%` - world_the_end的延迟
- `%justrtp_min_radius_world%` - world的最小半径
- `%justrtp_max_radius_world_nether%` - world_nether的最大半径

**在计分板/GUI中的使用：**
```yaml
# 同时显示所有世界的冷却时间
scoreboard:
  lines:
    - "主世界冷却: %justrtp_cooldown_world%"
    - "下界冷却: %justrtp_cooldown_world_nether%"
    - "末地冷却: %justrtp_cooldown_world_the_end%"
```

### 冷却占位符

| 占位符 | 描述 | 示例输出 |
|------------|-------------|----------------|
| `%justrtp_cooldown%` | 带单位的格式化冷却时间 | `5分30秒` |
| `%justrtp_cooldown_<world>%` | 特定世界的冷却时间 | `5分30秒` |
| `%justrtp_cooldown_raw%` | 剩余的原始秒数 | `330` |
| `%justrtp_cooldown_total%` | 总冷却持续时间 | `600` |
| `%justrtp_cooldown_formatted%` | 紧凑格式 | `5分30秒` |
| `%justrtp_has_cooldown%` | 布尔冷却检查 | `true` 或 `false` |

### 成本和延迟占位符

| 占位符 | 描述 | 示例输出 |
|------------|-------------|----------------|
| `%justrtp_cost%` | 当前世界的经济成本 | `500.0` |
| `%justrtp_cost_<world>%` | 特定世界的经济成本 | `100.0` |
| `%justrtp_delay%` | 传送准备秒数 | `3` |
| `%justrtp_delay_<world>%` | 特定世界的延迟 | `5` |
| `%justrtp_is_delayed%` | 活动延迟检查 | `true` 或 `false` |

### 世界和配置占位符

| 占位符 | 描述 | 示例输出 |
|------------|-------------|----------------|
| `%justrtp_world_name%` 或 `%justrtp_world%` | 当前世界名称 | `world_nether` |
| `%justrtp_current_world%` | world_name的别名 | `world_nether` |
| `%justrtp_world_min_radius%` 或 `%justrtp_min_radius%` | 最小传送半径 | `100` |
| `%justrtp_min_radius_<world>%` | 特定世界的最小半径 | `500` |
| `%justrtp_world_max_radius%` 或 `%justrtp_max_radius%` | 最大传送半径 | `10000` |
| `%justrtp_max_radius_<world>%` | 特定世界的最大半径 | `15000` |
| `%justrtp_permission_group%` 或 `%justrtp_group%` | 活动权限组 | `vip` |

### 状态占位符

| 占位符 | 描述 | 示例输出 |
|------------|-------------|----------------|
| `%justrtp_can_rtp%` | 复合检查（冷却 + 成本 + 延迟） | `true` 或 `false` |

### RTP区域占位符

区域系统占位符，用于FancyHolograms、计分板和其他插件。

| 占位符 | 描述 | 示例输出 |
|------------|-------------|----------------|
| `%justrtp_zone_time_<zoneid>%` | 特定区域的倒计时 | `15`（秒） |
| `%justrtp_in_zone%` | 玩家是否在任何区域内 | `true` 或 `false` |
| `%justrtp_current_zone%` 或 `%justrtp_zone%` | 玩家当前所在的区域ID | `pvp-arena` 或 `None` |
| `%justrtp_zone_name%` | current_zone的别名 | `pvp-arena` 或 `None` |

**区域时间示例：**
- `%justrtp_zone_time_pvp-arena%` - 显示"pvp-arena"区域的倒计时
- `%justrtp_zone_time_battle-royale%` - 显示"battle-royale"区域的倒计时

**FancyHolograms集成：**
```yaml
# 显示区域倒计时的全息图示例
lines:
  - "<gradient:red:gold>PvP竞技场</gradient>"
  - "<yellow>%justrtp_zone_time_pvp-arena%s后传送"  
  - "<gray>等待玩家: %justrtp_in_zone%"
```

**计分板集成：**
```yaml
- "区域: %justrtp_current_zone%"
- "传送: %justrtp_zone_time_pvp-arena%s"
- "在区域内: %justrtp_in_zone%"
```

### 使用示例

**多世界计分板：**
```yaml
scoreboard:
  - "=== RTP冷却时间 ==="
  - "主世界: %justrtp_cooldown_world%"
  - "下界: %justrtp_cooldown_world_nether%"
  - "末地: %justrtp_cooldown_world_the_end%"
  - ""
  - "=== RTP费用 ==="
  - "主世界: $%justrtp_cost_world%"
  - "下界: $%justrtp_cost_world_nether%"
```

**权限组显示：**
```yaml
- "等级: %justrtp_group%"
- "状态: %justrtp_can_rtp%"
- "世界: %justrtp_world_name%"
```

**聊天集成：**
```yaml
message: "<green>您可以在%justrtp_cooldown%后使用RTP"
cost_message: "<yellow>费用: $%justrtp_cost%"
```

---

## 消息定制

所有消息都支持**MiniMessage**格式，用于富文本、颜色、渐变和悬停/点击事件。

### MiniMessage格式示例

**`messages.yml`：**
```yaml
# 简单颜色
teleport_success: "<green>传送成功！"
teleport_failed: "<red>无法找到安全位置。"

# 渐变
cooldown_active: "<gradient:red:yellow>您必须等待<cooldown>才能再次使用RTP。</gradient>"

# 悬停和点击事件
help_command: "<hover:show_text:'点击传送'><click:run_command:/rtp><gold>[点击RTP]</click></hover>"

# 多种格式
prefix: "<dark_gray>[<gradient:aqua:blue>RTP</gradient>]</dark_gray>"
zone_countdown: "<prefix> <yellow><time>秒后传送..."
zone_teleport: "<prefix> <green>✔ 现在传送！"

# 动作栏消息
action_bar_cooldown: "<red>⏱ 冷却: <cooldown>"
action_bar_delay: "<yellow>⏳ 准备传送中..."
```

### 可用消息占位符

消息支持以下内部占位符：

- `<player>` - 玩家名称
- `<world>` - 世界名称
- `<server>` - 服务器名称
- `<cooldown>` - 格式化的冷却时间
- `<cost>` - 传送费用
- `<delay>` - 准备延迟
- `<time>` - 区域倒计时
- `<min_radius>` / `<max_radius>` - 半径值

### 可定制消息类别

- 传送反馈（成功、失败、取消）
- 冷却和延迟通知
- 经济消息（费用、资金不足）
- 权限拒绝消息
- 区域进入/离开/倒计时/传送消息
- 管理员命令响应
- 错误消息

完整的MiniMessage语法，请参阅：https://docs.advntr.dev/minimessage/format.html

---

## 性能优化

### 位置缓存

预计算安全位置以实现即时传送：

**`cache.yml`：**
```yaml
enabled: true
size: 100                  # 每个世界的位置数
min_size: 20               # 重新填充前的最小数量
refill_interval: 600       # 每10分钟重新填充一次
cleanup_interval: 3600     # 每小时清理一次
async: true                # 异步处理
```

优势：
- 即时传送（无位置搜索延迟）
- 高峰使用期间减少服务器负载
- 更好的玩家体验

### Redis缓存

对于大型网络，启用Redis进行分布式缓存：

**`redis.yml`：**
```yaml
enabled: true
host: "localhost"
port: 6379
password: ""
database: 0
cache_ttl: 3600           # 缓存生命周期（秒）
```

### Folia支持

JustRTP完全兼容Folia的区域化线程：
- 所有操作都感知区域
- 无全局状态修改
- 安全的并发区域传送
- 在Folia服务器上的最佳性能

### 性能提示

- 为高流量服务器增加位置缓存
- 对于3个以上服务器的网络使用Redis
- 配置合理的`max_attempts`（20-30）
- 将生物群系黑名单限制为必要的生物群系
- 使用PacketEvents提高全息图性能

---

## 故障排除

### 常见问题

**"无法找到安全位置"**
- 在config.yml中增加`max_attempts`
- 为世界扩展`max_radius`
- 检查`biome_blacklist`和`unsafe_blocks`设置
- 检查WorldGuard区域限制

**冷却不起作用**
- 验证玩家有正确的权限
- 检查是否有`justrtp.bypass.cooldown`权限
- 检查config中的权限组优先级

**跨服务器RTP不工作**
- 确认MySQL连接：`/rtp proxystatus`
- 验证每个服务器的`this_server_name`是否唯一
- 检查`mysql.yml`中的MySQL凭据
- 确保在所有网络服务器上都安装了插件
- 验证目标服务器是否在线

**全息图不显示倒计时**
- 安装FancyHolograms以获得最佳视觉效果和持久性
- 替代方案：安装PacketEvents以获得高性能全息图
- 回退方案：Display Entities无需任何依赖即可工作
- 检查`holograms.yml`中的`preferred-engine`设置
- 验证区域是否配置了全息图（3.2.9+版本在设置时自动创建）
- 确认全息图位置在渲染范围内
- 对于FancyHolograms：使用`/fholo list`验证全息图存在

**区域不传送多个玩家**
- 更新到3.2.6+版本（修复了竞态条件问题）
- 检查区域是否正确配置：`/rtpzone list`
- 验证玩家没有忽略区域：`/rtpzone ignore`

**经济费用不收费**
- 安装Vault插件
- 验证经济插件已加载（EssentialsX、CMI等）
- 检查config.yml中的`economy.enabled: true`
- 确认玩家有足够的资金

### 调试模式

在`config.yml`中启用详细日志：
```yaml
debug: true
verbose_logging: true
```

这将输出：
- 位置搜索尝试和结果
- 区域传送事件
- MySQL/Redis连接详情
- 权限检查
- 缓存操作

### 获取帮助

如果您遇到此处未涵盖的问题：

1. 检查控制台是否有错误消息
2. 启用调试模式并查看日志
3. 验证所有依赖项都是最新的
4. 检查配置文件是否有语法错误

---

## 技术规格

| 组件 | 详情 |
|-----------|---------|
| **插件版本** | 3.3.1 |
| **Minecraft版本** | 1.21+（测试过1.21.4） |
| **服务器软件** | Paper、Folia、Spigot |
| **Java版本** | 要求21+ |
| **API版本** | 1.21 |
| **数据库支持** | MySQL 8.0+、Redis（可选） |
| **依赖项** | Vault、PlaceholderAPI、WorldGuard、FancyHolograms、PacketEvents（全部可选） |
| **许可证** | MIT |
| **作者** | kotori |

### 功能矩阵

| 功能 | 状态 | 要求 |
|---------|--------|--------------|
| 基本RTP | ✅ 核心 | 无 |
| 跨服务器RTP | ✅ 可用 | MySQL |
| RTP区域 | ✅ 可用 | 无 |
| 经济集成 | ✅ 可用 | Vault |
| PlaceholderAPI | ✅ 可用 | PlaceholderAPI |
| WorldGuard区域 | ✅ 可用 | WorldGuard |
| Redis缓存 | ✅ 可用 | Redis |
| Folia支持 | ✅ 完全支持 | Folia |
| FancyHolograms集成 | ✅ 可用 (3.2.9+) | FancyHolograms 2.8.0+ |
| PacketEvents全息图 | ✅ 可用 | PacketEvents |
| Display Entity全息图 | ✅ 可用（回退） | 无 |
| 自动全息图创建 | ✅ 可用 (3.2.9+) | 无 |
| 持久全息图 | ✅ 可用 (3.2.9+) | FancyHolograms |
| 全息图实时重载 | ✅ 可用 (3.2.9+) | 无 |
| 位置缓存 | ✅ 可用 | 无 |
| 权限组 | ✅ 可用 | 无 |
| 自动RTP（加入/重生） | ✅ 可用 | 无 |
| 粒子效果 | ✅ 可用 | 无 |
| 音效 | ✅ 可用 | 无 |
| MiniMessage格式 | ✅ 可用 | 无 |

---

## 更新日志

### 版本3.3.1（当前）

**生活品质改进：**
- ⏱️ **人性化时间格式**：所有时间显示现在显示为"5分38秒"而不是"338秒"
  - 影响冷却时间、延迟、全息图和所有基于时间的消息
- 👑 **OP/权限冷却绕过**：OP玩家和拥有`justrtp.cooldown.bypass`权限的玩家跳过冷却时间
  - OP状态自动绕过
  - 适用于代理和本地传送
- 🔇 **空消息抑制**：通过在messages.yml中将消息设置为`""`来隐藏任何消息
  - 设置`message: ""`完全抑制（无空行）
  - 区分缺失键（警告）和禁用消息（静默）
- 💰 **基于半径的定价系统**：根据传送半径收取不同价格
  - 按世界配置分层定价
  - 示例：半径5000 = $100，半径15000 = $350，半径20000 = $600
  - 费用是累加的（基础费用 + 半径层级费用）
  - 与现有权限组定价完全兼容


---

## 许可证和鸣谢

**JustRTP**由**kotori**精心开发和维护。

本插件是开源软件，采用MIT许可证。

---

**需要帮助？** 请查看上面的故障排除部分，或在启用调试模式的情况下查看控制台日志。

**喜欢JustRTP？** 考虑留下反馈或支持持续开发！
