# SagaQuests - 高度模块化任务系统

> 🚀 基于 Bukkit API 1.20.1 开发的高度模块化 Minecraft 任务系统插件

**适用场景**：单服务器环境、BungeeCord 跨服网络、大型 RPG 服务器、竞技服务器、生存服务器

## ✨ 核心特性

### 🎯 任务系统
- **任务类型**：击杀、收集、交互、破坏、放置、制作、钓鱼、农业
- **条件系统**：23种条件类型，支持复杂组合逻辑
- **奖励机制**：物品、经验、命令、经济、点数、复合奖励
- **前置任务**：支持复杂的任务依赖关系和逻辑条件
- **任务分类**：主线、支线、日常、特殊任务完整分类

### 🖥️ GUI系统
- **智能分页**：支持大量任务的分页显示
- **实时进度**：任务进度条、状态显示、交互提示
- **多视图模式**：任务列表、进行中、已完成等视图

### 🎮 游戏内编辑器
- **可视化编辑**：游戏内创建、修改、验证任务
- **实时预览**：所见即所得的任务配置体验
- **一键克隆**：快速复制和修改现有任务

### 💾 双模式数据存储
- **SQLite单机模式**：适合单服务器，零配置开箱即用
- **Redis+MySQL分布式模式**：支持 BungeeCord 跨服同步
- **3层缓存架构**：L1本地 → L2Redis → L3MySQL 极致性能优化
- **无缝数据迁移**：SQLite ⇄ MySQL 双向迁移工具

### 🔗 深度集成支持
- **Vault经济系统**：经济条件、经济奖励完整支持
- **PlayerPoints点数**：点数条件、点数奖励集成
- **PlaceholderAPI**：丰富的占位符支持，完美融入其他插件
- **软依赖设计**：所有集成均为可选，保证兼容性



## 🚀 快速入门

### 🎮 玩家命令

| 命令 | 功能描述 | 权限要求 |
|------|----------|----------|
| `/quest gui` | 打开现代化任务GUI界面 | `sagaquests.use` |
| `/quest list` | 查看所有可用任务列表 | `sagaquests.use` |
| `/quest start <任务ID>` | 开始指定任务 | `sagaquests.use` |
| `/quest abandon <任务ID>` | 放弃正在进行的任务 | `sagaquests.use` |
| `/quest progress` | 查看当前任务进度 | `sagaquests.use` |

### 🛠️ 管理员命令

#### 基础管理
| 命令 | 功能描述 |
|------|----------|
| `/quest reload` | 重新加载所有配置文件 |
| `/quest cancel` | 取消当前的设置会话 |

#### 交互管理
| 命令 | 功能描述 |
|------|----------|
| `/quest interact set <任务ID>` | 设置实体/方块交互目标 |
| `/quest interact remove` | 移除交互设置 |
| `/quest interact list` | 查看交互统计信息 |

#### 任务编辑器
| 命令 | 功能描述 |
|------|----------|
| `/quest editor create <任务ID>` | 创建新任务 |
| `/quest editor modify <任务ID> <路径> <值>` | 修改任务属性 |
| `/quest editor validate <任务ID>` | 验证任务配置 |
| `/quest editor clone <源ID> <新ID>` | 克隆任务 |
| `/quest editor delete <任务ID>` | 删除任务 |

#### 任务快速编辑
📌 **物品编辑**
```bash
# 奖励物品管理
/quest editquest <任务ID> rewards additems <物品ID>    # 添加手中物品为奖励
/quest editquest <任务ID> rewards removeitems <物品ID>  # 移除奖励物品
/quest editquest <任务ID> rewards listitems            # 列出所有奖励物品

# 条件物品管理
/quest editquest <任务ID> conditions additems <物品ID>   # 添加手中物品为条件
/quest editquest <任务ID> conditions removeitems <物品ID> # 移除条件物品
/quest editquest <任务ID> conditions listitems           # 列出所有条件物品
```

📌 **交互目标编辑**
```bash
# 实体交互管理
/quest editquest <任务ID> interact addentity <名称>     # 添加实体交互目标
/quest editquest <任务ID> interact removeentity <名称>  # 移除实体交互目标
/quest editquest <任务ID> interact listentities         # 列出所有实体交互目标

# 方块交互管理
/quest editquest <任务ID> interact addblock <名称>       # 添加方块交互目标
/quest editquest <任务ID> interact removeblock <名称>    # 移除方块交互目标
/quest editquest <任务ID> interact listblocks           # 列出所有方块交互目标
```

#### 数据迁移工具
| 命令 | 功能描述 |
|------|----------|
| `/quest migrate export-yaml` | 导出数据到YAML文件 |
| `/quest migrate import-yaml` | 从 YAML文件导入数据 |
| `/quest migrate sqlite-to-mysql <主机> <端口> <数据库> <用户> <密码>` | SQLite迁移到MySQL |
| `/quest migrate mysql-to-sqlite <主机> <端口> <数据库> <用户> <密码>` | MySQL迁移到SQLite |


## 📂 配置文件详解

### 🌐 config.yml - 主配置

主配置文件，包含所有核心设置：

#### 🔧 基础设置
```yaml
settings:
  language: zh_CN                    # 语言设置
  auto-save-interval: 300            # 自动保存间隔(秒)
  max-active-quests: 5               # 玩家最大活动任务数
  abandon-cooldown: 3600             # 放弃任务冷却时间(秒)
  track-quest-limit: 3               # 任务追踪显示限制
  auto-start-on-interact: false      # 与实体/方块交互时是否自动开始任务
```

#### 💾 数据存储配置
```yaml
storage:
  type: sqlite                       # 存储类型: sqlite 或 redis
  
  # SQLite 配置
  sqlite:
    file: database.db
  
  # Redis 配置
  redis:
    host: localhost
    port: 6379
    password: ""
    database: 0
    max_connections: 20
    connection_timeout: 2000
  
  # MySQL 配置  
  mysql:
    host: localhost
    port: 3306
    database: sagaquests
    username: root
    password: ""
    max_pool_size: 20
    connection_timeout: 30000
  
  # 缓存配置
  cache:
    local_ttl: 5                     # L1本地缓存TTL(秒)
    redis_ttl: 300                   # L2Redis缓存TTL(秒)
    max_local_size: 1000             # 本地缓存最大大小
```

### 💬 message.yml - 消息配置

消息配置文件，支持完全自定义所有消息文本，支持多语言。

### 📦 placeholder.yml - 占位符配置

定义丰富的占位符支持，集成 PlaceholderAPI。

### 📁 quests/ - 任务配置目录

任务配置文件目录，支持多个 YAML 文件定义任务，灵活组织任务结构。


## 🔧 统一条件系统详解

SagaQuests 采用模块化的统一条件系统，支持灵活的条件组合和复杂的逻辑判断。

### 📋 条件类型总览

#### 🎯 目标条件 (Target Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `BLOCK_TARGET` | 检查目标方块类型 | `block`/`blocks` |
| `ENTITY_TARGET` | 检查目标实体类型 | `entity_type`, `entity_name` |
| `ACTION` | 检查玩家动作类型 | `action` |

#### 🌍 环境条件 (Environment Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `LOCATION` | 检查位置相关条件 | `world`, `min_x`, `max_y`, `radius` |
| `BIOME` | 检查生物群系 | `biomes` |
| `TIME` | 检查游戏时间 | `min_time`, `max_time` |
| `WEATHER` | 检查天气条件 | `weather`, `allow_rain` |

#### 👤 玩家条件 (Player Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `LEVEL` | 检查玩家等级 | `min_level`, `max_level` |
| `PERMISSION` | 检查权限 | `permissions`, `require_all` |
| `PLAYER_ACTION` | 检查玩家动作状态 | `actions`, `logic`, `inverted` |
| `PLAYER_POSE` | 检查玩家姿态 | `pose`, `allow_multiple` |
| `PLAYER_MOVEMENT` | 检查玩家移动 | `movement_type`, `min_distance` |

#### 🎒 物品条件 (Item Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `ITEM` | 检查背包物品 | `items`, `consume`, `check_armor` |
| `TOOL` | 检查工具和附魔 | `material`, `enchantments`, `require_silk_touch` |

#### 🎣 活动条件 (Activity Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `FISHING` | 检查钓鱼相关 | `state`, `catches`, `min_exp` |
| `FARMING` | 检查农业相关 | `action`, `crops`, `require_fully_grown` |

#### 🔗 组合条件 (Composite Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `COMPOSITE` | 组合多个条件 | `logic`, `conditions` |
| `SEQUENTIAL` | 顺序条件检查 | `steps`, `reset_on_fail` |

#### 📋 任务条件 (Quest Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `PRE_QUEST` | 前置任务检查 | `quest_id`, `completed` |

### 🎯 条件语法详解

#### 基础条件语法
```yaml
conditions:
  condition_name:
    type: CONDITION_TYPE
    # 条件特定参数
```

#### 🔧 COMPOSITE 条件 - 逻辑组合
```yaml
conditions:
  complex_condition:
    type: COMPOSITE
    logic: AND  # AND/OR/NOT/XOR
    conditions:
      level_check:
        type: LEVEL
        min_level: 10
      item_check:
        type: ITEM
        items:
          - material: DIAMOND_SWORD
            amount: 1
```

#### 📊 SEQUENTIAL 条件 - 顺序执行
```yaml
conditions:
  sequence:
    type: SEQUENTIAL
    reset_on_fail: true
    allow_skip: false
    display_name: "&6顺序挑战"
    fail_message: "&c必须按顺序完成！"
    
    steps:
      step_1:
        type: COMPOSITE
        logic: AND
        display_name: "步骤1: 击杀僵尸"
        conditions:
          entity_check:
            type: ENTITY_TARGET
            entity_type: ZOMBIE
          action_check:
            type: ACTION
            action: KILL
      
      step_2:
        type: COMPOSITE
        logic: AND
        display_name: "步骤2: 收集战利品"
        conditions:
          item_check:
            type: ITEM
            items:
              - material: ROTTEN_FLESH
                amount: 5
```

#### 🎒 ITEM 条件 - 物品检查
```yaml
conditions:
  item_requirement:
    type: ITEM
    consume: false  # 是否消耗物品
    check_main_hand: true
    check_off_hand: false
    check_armor: false
    items:
      diamond_pickaxe:
        material: DIAMOND_PICKAXE
        amount: 1
        enchantments:
          - "EFFICIENCY:3"
          - "UNBREAKING:2"
```

#### 🔧 TOOL 条件 - 工具和附魔
```yaml
conditions:
  tool_check:
    type: TOOL
    check_main_hand: true
    material: DIAMOND_PICKAXE  # 可选：指定工具类型
    require_silk_touch: true   # 必须有精准采集
    enchantments:
      - "SILK_TOUCH:1"
      - "UNBREAKING:3"
    min_durability: 50         # 最小耐久度
```

#### 🌍 LOCATION 条件 - 位置检查
```yaml
conditions:
  location_check:
    type: LOCATION
    world: world_nether
    min_x: -100
    max_x: 100
    min_y: 10
    max_y: 120
    min_z: -100
    max_z: 100
    radius: 50
```

#### 🎣 FISHING 条件 - 钓鱼检查
```yaml
conditions:
  fishing_check:
    type: FISHING
    state: CAUGHT_FISH  # FISHING/CAUGHT_FISH/CAUGHT_ENTITY/FAILED
    catches:
      - SALMON
      - COD
    min_exp: 3
    specific_items:
      - ENCHANTED_BOOK
```

#### 🌾 FARMING 条件 - 农业检查
```yaml
conditions:
  farming_check:
    type: FARMING
    action: HARVEST  # PLANT/HARVEST/GROW
    crops:
      - WHEAT
      - CARROTS
    require_fully_grown: true
    require_silk_touch: false
    min_growth_stage: 7
```

#### 👤 PLAYER_ACTION 条件 - 玩家状态
```yaml
conditions:
  action_check:
    type: PLAYER_ACTION
    logic: OR  # AND/OR/XOR
    inverted: false
    actions:
      - SNEAKING
      - SWIMMING
```

### 🎯 实际应用示例

#### 🌟 基础任务条件
```yaml
# 简单击杀任务
kill_zombies:
  type: UNIVERSAL
  action: KILL
  amount: 10
  
  conditions:
    zombie_kill:
      type: ENTITY_TARGET
      entity_type: ZOMBIE
```

#### 🎯 中级任务条件
```yaml
# 特定环境挖矿任务
mine_diamonds:
  type: UNIVERSAL
  action: BREAK
  amount: 5
  
  conditions:
    diamond_mining:
      type: COMPOSITE
      logic: AND
      conditions:
        block_check:
          type: BLOCK_TARGET
          blocks:
            - DIAMOND_ORE
            - DEEPSLATE_DIAMOND_ORE
        location_check:
          type: LOCATION
          max_y: 16
        tool_check:
          type: TOOL
          material: DIAMOND_PICKAXE
          enchantments:
            - "FORTUNE:3"
```

#### 🎆 高级任务条件
```yaml
# 复杂的顺序任务
master_challenge:
  type: UNIVERSAL
  action: CUSTOM
  amount: 1
  
  conditions:
    sequence:
      type: SEQUENTIAL
      reset_on_fail: true
      fail_message: "&c必须按正确顺序完成挑战！"
      
      steps:
        step_1_preparation:
          type: COMPOSITE
          logic: AND
          display_name: "&a准备阶段"
          conditions:
            level_check:
              type: LEVEL
              min_level: 30
            item_check:
              type: ITEM
              items:
                - material: DIAMOND_SWORD
                  enchantments:
                    - "SHARPNESS:4"
                - material: DIAMOND_CHESTPLATE
                  enchantments:
                    - "PROTECTION:3"
        
        step_2_location:
          type: COMPOSITE
          logic: AND
          display_name: "&e前往指定区域"
          conditions:
            location_check:
              type: LOCATION
              world: world_nether
              radius: 100
            biome_check:
              type: BIOME
              biomes:
                - NETHER_WASTES
        
        step_3_combat:
          type: COMPOSITE
          logic: AND
          display_name: "&c战斗测试"
          conditions:
            entity_check:
              type: ENTITY_TARGET
              entity_type: WITHER_SKELETON
            action_check:
              type: ACTION
              action: KILL
            time_check:
              type: TIME
              min_time: 18000  # 夜晚
```

### ⚠️ 常见语法错误与修复

#### ❌ 错误的 SEQUENTIAL 格式
```yaml
# 错误：使用数组格式
steps:
  step_1:
    conditions:
      - type: ENTITY_TARGET
        entity_type: ZOMBIE
      - type: ACTION
        action: KILL
```

#### ✅ 正确的 SEQUENTIAL 格式
```yaml
# 正确：使用嵌套对象格式
steps:
  step_1:
    type: COMPOSITE
    logic: AND
    conditions:
      entity_check:
        type: ENTITY_TARGET
        entity_type: ZOMBIE
      action_check:
        type: ACTION
        action: KILL
```

#### ❌ 错误的条件参数
```yaml
# 错误：PLAYER_ACTION 不支持经验检查
player_check:
  type: PLAYER_ACTION
  min_exp_level: 30
```

#### ✅ 正确的条件参数
```yaml
# 正确：使用 LEVEL 条件检查经验
player_check:
  type: LEVEL
  min_level: 30
```

### 🔧 条件调试技巧

#### 启用调试模式
```yaml
tasks:
  debug_task:
    debug: true  # 启用详细日志
    conditions:
      # 条件配置
```

#### 使用简化条件测试
```yaml
# 先用简单条件测试基本功能
simple_test:
  type: COMPOSITE
  logic: AND
  conditions:
    basic_check:
      type: LEVEL
      min_level: 1
```

#### 逐步增加复杂度
```yaml
# 逐步添加更多条件
complex_test:
  type: COMPOSITE
  logic: AND
  conditions:
    level_check:
      type: LEVEL
      min_level: 10
    item_check:
      type: ITEM
      items:
        - material: STONE_SWORD
          amount: 1
    # 继续添加更多条件...
```

## 📦 任务配置示例

基于统一条件系统的详细配置示例：

### 🎆 新手任务 - 使用统一条件系统
```yaml
starter_quest:
  display_name: "&6新手任务"
  description:
    - "&7欢迎来到服务器！"
    - "&7完成这个任务来熟悉基本操作"
    - ""
    - "&e任务目标:"
    - "&7• 击杀10只僵尸"
    - "&7• 收集64个圆石"
    - "&7• 破坏10个原木"
  
  type: MAIN                         # 任务类型
  repeatable: false                  # 是否可重复
  max_completions: 1                 # 最大完成次数
  cooldown: 0                        # 冷却时间(秒)
  time_limit: 0                      # 时间限制(秒, 0=无限制)
  
  # 开始条件 - 使用统一条件系统
  start_conditions:
    level_check:
      type: LEVEL
      min_level: 0
      max_level: 100
  
  # 任务列表 - 使用 UniversalTask
  tasks:
    kill_zombies:
      type: UNIVERSAL
      action: KILL
      display_name: "&c击杀僵尸"
      amount: 10
      
      conditions:
        zombie_target:
          type: ENTITY_TARGET
          entity_type: ZOMBIE
    
    collect_cobblestone:
      type: UNIVERSAL
      action: COLLECT
      display_name: "&7收集圆石"
      amount: 1
      
      conditions:
        cobblestone_item:
          type: ITEM
          items:
            - material: COBBLESTONE
              amount: 64
    
    break_logs:
      type: UNIVERSAL
      action: BREAK
      display_name: "&a破坏原木"
      amount: 10
      
      conditions:
        log_blocks:
          type: BLOCK_TARGET
          blocks:
            - OAK_LOG
            - BIRCH_LOG
            - SPRUCE_LOG
            - JUNGLE_LOG
            - ACACIA_LOG
            - DARK_OAK_LOG
  
  # 奖励系统
  rewards:
    items:
      type: ITEM
      items:
        sword:
          material: IRON_SWORD
          amount: 1
          name: "&6新手之剑"
          lore:
            - "&7完成新手任务的奖励"
            - "&7祝你在冒险中好运！"
        food:
          material: COOKED_BEEF
          amount: 32
    
    experience:
      type: EXP
      levels: 5
      exp_points: 100
    
    commands:
      type: COMMAND
      executor: CONSOLE
      commands:
        - "give %player% diamond 3"
        - "say &a恭喜 %player% 完成了新手任务！"
```

### 🌅 日常任务：精准采集矿工
```yaml
daily_miner:
  display_name: "&e日常：精准采集矿工"
  type: DAILY
  repeatable: true
  cooldown: 86400                    # 24小时冷却
  
  start_conditions:
    permission:
      type: PERMISSION
      permissions:
        - "sagaquests.daily"
      require_all: true
  
  tasks:
    mine_iron_with_silk:
      type: UNIVERSAL
      action: BREAK
      display_name: "&7精准采集铁矿"
      amount: 30
      
      conditions:
        iron_mining:
          type: COMPOSITE
          logic: AND
          conditions:
            block_check:
              type: BLOCK_TARGET
              blocks:
                - IRON_ORE
                - DEEPSLATE_IRON_ORE
            tool_check:
              type: TOOL
              check_main_hand: true
              require_silk_touch: true
```

### 🎆 复杂的顺序任务示例
```yaml
sequential_challenge:
  display_name: "&d&l维度征服挑战"
  description:
    - "&7按顺序在三个维度完成史诗任务"
    - "&c必须按正确顺序完成！"
  
  type: SPECIAL
  time_limit: 7200  # 2小时限制
  
  start_conditions:
    level_check:
      type: LEVEL
      min_level: 50
    
    pre_quest:
      type: PRE_QUEST
      quest_id: starter_quest
      completed: true
  
  tasks:
    dimension_conquest:
      type: UNIVERSAL
      action: CUSTOM
      display_name: "&d&l维度征服"
      amount: 1
      
      conditions:
        sequence:
          type: SEQUENTIAL
          reset_on_fail: true
          allow_skip: false
          display_name: "&d&l顺序维度挑战"
          fail_message: "&c必须按照顺序完成各个维度的挑战！"
          
          steps:
            step_1_overworld:
              type: COMPOSITE
              logic: AND
              display_name: "&2步骤1: 主世界守护者"
              conditions:
                entity_check:
                  type: ENTITY_TARGET
                  entity_type: ELDER_GUARDIAN
                location_check:
                  type: LOCATION
                  world: world
                action_check:
                  type: ACTION
                  action: KILL
            
            step_2_nether:
              type: COMPOSITE
              logic: AND
              display_name: "&c步骤2: 下界征服"
              conditions:
                entity_check:
                  type: ENTITY_TARGET
                  entity_type: WITHER_SKELETON
                location_check:
                  type: LOCATION
                  world: world_nether
                action_check:
                  type: ACTION
                  action: KILL
            
            step_3_end:
              type: COMPOSITE
              logic: AND
              display_name: "&5步骤3: 末地龙战"
              conditions:
                entity_check:
                  type: ENTITY_TARGET
                  entity_type: ENDER_DRAGON
                location_check:
                  type: LOCATION
                  world: world_the_end
                action_check:
                  type: ACTION
                  action: KILL
```

## 🚀 安装与部署

### 📋 系统要求
- **Java**: 17+ 版本
- **服务端**: Bukkit 1.20.1+ 

### 💾 单端模式部署
1. 下载 `SagaQuests.jar` 到 `plugins/` 目录
2. 重启服务器，插件将自动初始化
3. 编辑 `config.yml` （可选）
4. 在 `quests/` 目录中添加任务配置
5. 执行 `/quest reload` 重新加载

### 🌐 群组模式部署
1. **准备环境**
   - 搭建 Redis 服务器
   - 搭建 MySQL 数据库
   - 创建 `sagaquests` 数据库

2. **配置插件**
   ```yaml
   storage:
     type: redis
     redis:
       host: "redis.example.com"
       port: 6379
       password: "your_redis_password"
     mysql:
       host: "mysql.example.com"
       database: "sagaquests"
       username: "sagaquests_user"
       password: "your_mysql_password"
   ```
