# SagaQuests - 高度模块化任务系统

> 🚀 基于 Bukkit API 1.20.1 开发的高度模块化 Minecraft 任务系统插件

**适用场景**：单服务器环境、BungeeCord 跨服网络、大型 RPG 服务器、竞技服务器、生存服务器

## ✨ 核心特性

### 🎯 任务系统
- **任务类型**：击杀、收集、交互、破坏、放置、制作、钓鱼、农业
- **条件系统**：24种条件类型，支持复杂组合逻辑
- **奖励机制**：物品、经验、命令、经济、点数、复合奖励
- **前置任务**：支持复杂的任务依赖关系和逻辑条件
- **任务分类**：主线、支线、日常、特殊任务完整分类
- **重复机制**：支持任务重复完成，可设置冷却时间和最大完成次数
- **完成统计**：自动记录每个玩家的任务完成次数，支持基于完成次数的前置条件

### 🖥️ GUI系统
- **智能分页**：支持大量任务的分页显示
- **实时进度**：任务进度条、状态显示、交互提示
- **多视图模式**：任务列表、进行中、已完成等视图
- **重复性显示**：任务列表和详情页面显示任务是否可重复、完成次数等信息
- **完成统计**：实时显示玩家对每个任务的完成次数和限制信息


## ⏰ 限时任务系统

SagaQuests 支持真实时间限制的任务，为游戏增加紧张感和挑战性。

### 📋 限时任务特性

- **真实时间计算**：使用现实世界时间（秒），不受游戏内时间影响
- **自动超时检测**：每30秒自动检查所有活动任务是否超时
- **超时自动失败**：超时的任务会自动失败并清除进度
- **灵活配置**：可为每个任务单独设置时间限制

### ⚙️ 配置方法

在任务配置中添加 `time_limit` 字段（单位：秒）：

```yaml
quest_example:
  display_name: "&c限时挑战"
  time_limit: 3600  # 1小时限制（3600秒）
  # time_limit: 7200  # 2小时限制
  # time_limit: 0     # 无时间限制（默认）
```

### 📊 时间限制示例

| 时间描述 | 配置值 | 说明 |
|---------|--------|------|
| 5分钟 | 300 | 快速任务 |
| 30分钟 | 1800 | 短期任务 |
| 1小时 | 3600 | 标准限时任务 |
| 2小时 | 7200 | 中等限时任务 |
| 4小时 | 14400 | 长期限时任务 |
| 无限制 | 0 或不设置 | 无时间限制 |

### 🎯 实际应用案例

查看 `quests/example_fishingnew.yml` 中的限时任务示例：
- **任务7 - 极限挑战**：2小时内完成顺序钓鱼任务
- **任务8 - 钓鱼传奇**：4小时内完成终极挑战

### ⚠️ 注意事项

1. **时间从任务开始计算**：玩家接受任务时开始计时
2. **离线时间也计算**：玩家下线期间时间继续流逝
3. **超时立即失败**：超时后任务自动失败，进度清零
4. **失败可重新开始**：限时任务失败后可以重新接受（如果任务允许重复）

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


### 💬 message.yml - 消息配置

消息配置文件，支持完全自定义所有消息文本，支持多语言。

### 📦 placeholder.yml - 占位符配置

定义丰富的占位符支持，集成 PlaceholderAPI。

### 📁 quests/ - 任务配置目录

任务配置文件目录，支持多个 YAML 文件定义任务，灵活组织任务结构。


## 🎯 任务动作类型（ActionType）详解

SagaQuests 使用 UniversalTask 系统，支持9种不同的任务动作类型，每种类型对应不同的游戏行为和事件监听。

### 📋 ActionType 总览表

| ActionType | 图标 | 中文说明 | 用途 | 监听事件 | 主要应用 |
|------------|------|----------|------|----------|----------|
| `BREAK` | ⛏ | 破坏方块 | 挖掘、采集任务 | `BlockBreakEvent` | 采矿、伐木、挖掘 |
| `PLACE` | 🔨 | 放置方块 | 建筑、放置任务 | `BlockPlaceEvent` | 建筑、装饰、基建 |
| `KILL` | ⚔ | 击杀实体 | 战斗、狩猎任务 | `EntityDeathEvent` | PVE战斗、怪物狩猎 |
| `INTERACT` | ✋ | 交互操作 | NPC对话、方块交互 | `PlayerInteractEvent` | NPC任务、机关操作 |
| `CRAFT` | 🔧 | 合成物品 | 制作、工艺任务 | `CraftItemEvent` | 装备制作、道具合成 |
| `COLLECT` | 📦 | 收集物品 | 拾取、收集任务 | 背包检查 | 物品收集、材料获取 |
| `FISHING` | 🎣 | 钓鱼活动 | 钓鱼任务 | `PlayerFishEvent` | 休闲钓鱼、宝物获取 |
| `FARMING` | 🌱 | 农业活动 | 种植、收获任务 | 多种农业事件 | 农作物种植、收获 |
| `CUSTOM` | ✨ | 自定义动作 | 复杂逻辑任务 | 接受所有事件 | 特殊逻辑、组合条件 |

### 🎮 基础配置语法

所有 ActionType 都使用统一的 UniversalTask 语法：

```yaml
task_name:
  type: UNIVERSAL        # 固定值，使用通用任务类型
  action: ACTION_TYPE    # 指定具体的动作类型
  display_name: "任务显示名"
  amount: 数量           # 完成所需的数量
  
  conditions:            # 条件系统配置
    condition_name:
      type: CONDITION_TYPE
      # 条件特定参数...
```

### 🔧 各ActionType详细说明

#### ⛏ BREAK - 破坏方块
```yaml
mine_diamonds:
  type: UNIVERSAL
  action: BREAK
  display_name: "&b挖掘钻石"
  amount: 10
  
  conditions:
    diamond_ore:
      type: BLOCK_TARGET
      blocks:
        - DIAMOND_ORE
        - DEEPSLATE_DIAMOND_ORE
```

#### 🔨 PLACE - 放置方块
```yaml
build_house:
  type: UNIVERSAL
  action: PLACE
  display_name: "&e建筑任务"
  amount: 100
  
  conditions:
    building_blocks:
      type: BLOCK_TARGET
      blocks:
        - COBBLESTONE
        - STONE_BRICKS
        - PLANKS
```

#### ⚔ KILL - 击杀实体
```yaml
hunt_zombies:
  type: UNIVERSAL
  action: KILL
  display_name: "&c僵尸狩猎"
  amount: 20
  
  conditions:
    zombie_target:
      type: ENTITY_TARGET
      entity_type: ZOMBIE
```

#### ✋ INTERACT - 交互操作
```yaml
talk_to_npc:
  type: UNIVERSAL
  action: INTERACT
  display_name: "&a与NPC对话"
  amount: 1
  
  conditions:
    npc_interaction:
      type: COMPOSITE
      logic: AND
      conditions:
        entity_check:
          type: ENTITY_TARGET
          entity_type: VILLAGER
        action_check:
          type: ACTION
          action: RIGHT_CLICK
```

#### 🔧 CRAFT - 合成物品
```yaml
craft_tools:
  type: UNIVERSAL
  action: CRAFT
  display_name: "&6制作工具"
  amount: 5
  
  conditions:
    tool_crafting:
      type: ITEM
      items:
        - material: IRON_PICKAXE
          amount: 1
        - material: IRON_SWORD
          amount: 1
```

#### 📦 COLLECT - 收集物品
```yaml
collect_pearls:
  type: UNIVERSAL
  action: COLLECT
  display_name: "&5收集末影珍珠"
  amount: 16
  
  conditions:
    pearl_collection:
      type: ITEM
      items:
        - material: ENDER_PEARL
          amount: 1  # 检查条件最小量
```
**注意**：COLLECT任务会自动检查背包中的物品总数，进度与实际拥有数量同步。

#### 🎣 FISHING - 钓鱼活动
```yaml
fishing_contest:
  type: UNIVERSAL
  action: FISHING
  display_name: "&3钓鱼比赛"
  amount: 10
  
  conditions:
    fishing_success:
      type: FISHING
      state: CAUGHT_FISH
      catches:
        - SALMON
        - COD
      min_exp: 3
```

#### 🌱 FARMING - 农业活动
```yaml
harvest_wheat:
  type: UNIVERSAL
  action: FARMING
  display_name: "&e收获小麦"
  amount: 20
  
  conditions:
    wheat_harvest:
      type: FARMING
      action: HARVEST
      crops:
        - WHEAT
      require_fully_grown: true
```

#### ✨ CUSTOM - 自定义动作
```yaml
complex_challenge:
  type: UNIVERSAL
  action: CUSTOM
  display_name: "&d复杂挑战"
  amount: 1
  
  conditions:
    sequence_task:
      type: SEQUENTIAL
      steps:
        step_1:
          type: COMPOSITE
          logic: AND
          conditions:
            # 复杂条件组合...
```

### ⚡ ActionType 选择指南

| 需求场景 | 推荐ActionType | 理由 |
|---------|----------------|------|
| 采集资源 | `BREAK` | 直接监听方块破坏事件 |
| 建筑任务 | `PLACE` | 监听方块放置事件 |
| 战斗任务 | `KILL` | 监听实体死亡事件 |
| 物品收集 | `COLLECT` | 背包检查，支持堆叠计算 |
| 休闲活动 | `FISHING` | 专门的钓鱼事件处理 |
| 农业任务 | `FARMING` | 农业相关事件的统一处理 |
| 复杂逻辑 | `CUSTOM` | 自定义条件组合 |
| NPC交互 | `INTERACT` | 玩家交互事件 |
| 制作任务 | `CRAFT` | 物品合成事件 |

## 🔧 统一条件系统详解

SagaQuests 采用模块化的统一条件系统，支持灵活的条件组合和复杂的逻辑判断。

### 📋 条件类型总览

#### 🔄 条件类型别名表
许多条件类型支持多个别名，方便使用：

| 主类型 | 别名 | 说明 |
|---------|------|------|
| `TIME` | `GAME_TIME` | 游戏时间检查 |
| `PLAYER_MOVEMENT` | `MOVEMENT` | 玩家移动检测 |
| `PLAYER_ACTION_EVENT` | `ACTION_EVENT` | 玩家动作事件 |
| `PRE_QUEST` | `PREREQUISITE`, `QUEST_REQUIREMENT` | 前置任务检查 |
| `TASK_REPEATABILITY` | `REPEATABILITY`, `REPEATABLE` | 任务重复性检查 |
| `TIME_RESET` | `RESET` | 时间重置检查 |
| `BIOME` | `BIOMES` | 生物群系检查 |
| `WEATHER_ENVIRONMENT` | `WEATHER`, `ENVIRONMENT` | 天气环境检查 |

#### 🎯 目标条件 (Target Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `BLOCK_TARGET` | 检查目标方块类型 | `block`/`blocks` |
| `ENTITY_TARGET` | 检查目标实体类型 | `entity_type`, `entity_name` |
| `ACTION` | 检查玩家交互动作类型 | `action`/`actions`, `allow_any` (支持: `LEFT_CLICK_BLOCK`, `RIGHT_CLICK_BLOCK`, `LEFT_CLICK_AIR`, `RIGHT_CLICK_AIR`, `PHYSICAL`) |

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
| `PLAYER_MOVEMENT` | 检查玩家移动 | `movement_type`, `min_distance`, `accumulate` |
| `PLAYER_ACTION_EVENT` | 检查玩家动作事件 | `actions`, `event_type`, `state` |
| `VEHICLE` | 检查玩家载具状态 | `require_in_vehicle`, `vehicle_types`, `check_passenger` |

#### 🎒 物品条件 (Item Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `ITEM` | 检查背包物品 | `items`, `consume`, `check_armor` |
| `TOOL` | 检查工具和附魔 | `material`, `enchantments`, `require_silk_touch` |

#### 🎣 活动条件 (Activity Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `FISHING` | 检查钓鱼相关 | `state`, `catches`, `min_exp`, `require_treasure`, `treasures` |
| `FARMING` | 检查农业相关 | `action`, `crops`, `seeds`, `require_fully_grown`, `min_growth` |
| `INTERACT_HARVEST` | 右键采摘检测（新增） | `blocks`, `require_fully_grown`, `min_age`, `max_age` |
| `CRAFT` | 检查合成物品 | `items`, `material`, `amount` |

#### 🔗 组合条件 (Composite Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `COMPOSITE` | 组合多个条件 | `logic`, `conditions` |
| `SEQUENTIAL` | 顺序条件检查 | `steps`, `reset_on_fail` |

#### 📋 任务条件 (Quest Conditions)
| 条件类型 | 用途 | 主要参数 |
|---------|------|----------|
| `PRE_QUEST` | 前置任务检查 | `quest_id`, `completed`, `min_completions` |
| `TASK_REPEATABILITY` | 任务重复性检查 | `quest_id`, `require_repeatable`, `check_cooldown`, `check_max_completions` |
| `TIME_RESET` | 时间重置条件检查 | `quest_id`, `periods`, `timezone` |

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
    catches:            # 指定鱼类
      - SALMON
      - COD
    min_exp: 3         # 最小经验值
    require_treasure: true  # 需要钓到宝物
    treasures:         # 指定宝物类型
      - ENCHANTED_BOOK
      - NAME_TAG
      - SADDLE
```

#### 🌾 FARMING 条件 - 农业检查
```yaml
conditions:
  farming_check:
    type: FARMING
    action: HARVEST  # PLANT/HARVEST/INTERACT_HARVEST/GROW/FERTILIZE
    crops:           # 作物类型（用于收获）
      - WHEAT
      - CARROTS
      - SWEET_BERRY_BUSH  # 支持特殊作物
    seeds:           # 种子类型（用于种植）
      - WHEAT_SEEDS
      - CARROT
    require_fully_grown: true    # 是否要求完全成熟
    require_silk_touch: false    # 是否需要精准采集
    min_growth: 2               # 最小生长阶段（用于min_growth_stage的简写）
    min_growth_stage: 7         # 最小生长阶段
    max_growth_stage: -1        # 最大生长阶段（-1表示不限制）
```

**FARMING动作类型说明**：
- `PLANT`: 种植作物
- `HARVEST`: 左键破坏收获
- `INTERACT_HARVEST`: 右键采摘收获（新增，用于甜浆果等）
- `GROW`: 作物生长
- `FERTILIZE`: 施肥

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

#### 🏃 PLAYER_MOVEMENT 条件 - 玩家移动检测
```yaml
conditions:
  movement_check:
    type: PLAYER_MOVEMENT
    movement_type: HORIZONTAL  # HORIZONTAL/VERTICAL/ANY
    min_distance: 100          # 最小移动距离
    accumulate: true           # 是否累积移动距离
    check_speed: true          # 是否检查速度
    min_speed: 0.2            # 最小速度
    reset_on_teleport: true    # 传送时重置
```

#### ⚡ PLAYER_ACTION_EVENT 条件 - 玩家动作事件
```yaml
conditions:
  action_event:
    type: PLAYER_ACTION_EVENT
    event_type: SNEAK          # SNEAK/SPRINT/FLIGHT/SWAP_HAND
    actions:
      - SNEAK_START            # 开始潜行
      - SNEAK_STOP             # 停止潜行
      - SPRINT_TOGGLE          # 冲刺切换
      - FLIGHT_TOGGLE          # 飞行切换
      - SWAP_HAND              # 交换手部物品
    state: true                # 目标状态（可选）
```

#### 🚗 VEHICLE 条件 - 载具检查
```yaml
conditions:
  vehicle_check:
    type: VEHICLE
    require_in_vehicle: true   # 是否需要在载具中
    vehicle_types:             # 允许的载具类型
      - BOAT                   # 船
      - CHEST_BOAT            # 运输船
      - MINECART              # 矿车
      - HORSE                 # 马
    check_passenger: true      # 检查乘客数量
    min_passengers: 1         # 最小乘客数
    max_passengers: 3         # 最大乘客数
```

### 🎯 ACTION条件详细配置

#### 基础配置语法
```yaml
conditions:
  action_check:
    type: ACTION
    action: RIGHT_CLICK_BLOCK  # 单个动作
    # 或者
    actions:                   # 多个动作
      - RIGHT_CLICK_BLOCK
      - LEFT_CLICK_BLOCK
    allow_any: false          # 是否允许任何动作
```

#### 支持的Action类型
| Action类型 | 说明 | 适用场景 |
|-----------|------|----------|
| `LEFT_CLICK_BLOCK` | 左键点击方块 | 攻击方块、快速挖掘 |
| `RIGHT_CLICK_BLOCK` | 右键点击方块 | 交互方块、使用物品 |
| `LEFT_CLICK_AIR` | 左键点击空气 | 挥舞武器、使用技能 |
| `RIGHT_CLICK_AIR` | 右键点击空气 | 使用物品、抛掷 |
| `PHYSICAL` | 物理接触 | 压力板、绊线、踩踏 |

#### ACTION条件实际应用
```yaml
# NPC对话任务：右键点击村民
talk_to_villager:
  type: UNIVERSAL
  action: INTERACT
  display_name: "&a与村民对话"
  amount: 1
  
  conditions:
    npc_interaction:
      type: COMPOSITE
      logic: AND
      conditions:
        entity_check:
          type: ENTITY_TARGET
          entity_type: VILLAGER
        action_check:
          type: ACTION
          action: RIGHT_CLICK_BLOCK  # 右键交互

# 压力板踩踏任务
step_on_pressure_plate:
  type: UNIVERSAL
  action: INTERACT
  display_name: "&e踩踏机关"
  amount: 5
  
  conditions:
    pressure_interaction:
      type: COMPOSITE
      logic: AND
      conditions:
        block_check:
          type: BLOCK_TARGET
          blocks:
            - STONE_PRESSURE_PLATE
            - WOODEN_PRESSURE_PLATE
        action_check:
          type: ACTION
          action: PHYSICAL  # 物理接触

# 多动作支持：左右键都可以
flexible_interaction:
  type: UNIVERSAL
  action: INTERACT
  display_name: "&6灵活交互"
  amount: 10
  
  conditions:
    multi_action:
      type: ACTION
      actions:
        - LEFT_CLICK_BLOCK
        - RIGHT_CLICK_BLOCK
      allow_any: false  # 只允许配置的动作
```

### 🎯 ActionType与Condition配合使用示例

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

#### 🌾 农业任务配置示例

##### 种植任务（使用复合条件）
```yaml
plant_wheat:
  display_name: "&a种植小麦"
  type: UNIVERSAL
  action: BREAK  # 使用BREAK事件监听
  amount: 10
  
  conditions:
    wheat_planting:
      type: COMPOSITE
      logic: AND
      conditions:
        # 检查手持物品是种子
        hand_item:
          type: ITEM
          check_main_hand: true
          material: WHEAT_SEEDS
        # 检查放置的方块
        placed_block:
          type: BLOCK_TARGET
          blocks:
            - WHEAT

# 另一种方式：使用FARMING条件
plant_carrots:
  display_name: "&a种植胡萝卜"
  type: UNIVERSAL
  action: FARMING
  amount: 10
  
  conditions:
    carrot_planting:
      type: FARMING
      action: PLANT
      seeds:
        - CARROT  # 胡萝卜既是种子也是食物
      crops:
        - CARROTS  # 种植后变成CARROTS方块
```

##### 收获任务
```yaml
harvest_wheat:
  display_name: "&e收获成熟小麦"
  type: UNIVERSAL
  action: FARMING
  amount: 20
  
  conditions:
    wheat_harvest:
      type: FARMING
      action: HARVEST
      crops:
        - WHEAT
      require_fully_grown: true  # 要求完全成熟

harvest_multiple:
  display_name: "&e收获各种作物"
  type: UNIVERSAL
  action: FARMING
  amount: 50
  
  conditions:
    multiple_harvest:
      type: FARMING
      action: HARVEST
      crops:
        - WHEAT
        - CARROTS
        - POTATOES
        - BEETROOTS
      require_fully_grown: true
      min_growth_stage: 7  # 至少7级成熟度
```

##### 复杂农业任务
```yaml
expert_farmer:
  display_name: "&6农业专家"
  type: UNIVERSAL
  action: CUSTOM
  amount: 1
  
  conditions:
    farming_sequence:
      type: SEQUENTIAL
      reset_on_fail: false
      steps:
        # 第一步：种植小麦
        step_1_plant:
          type: COMPOSITE
          logic: AND
          display_name: "&a种植10个小麦"
          conditions:
            hand_check:
              type: ITEM
              check_main_hand: true
              material: WHEAT_SEEDS
            block_check:
              type: BLOCK_TARGET
              blocks:
                - WHEAT
        
        # 第二步：等待生长并施肥
        step_2_fertilize:
          type: COMPOSITE
          logic: AND
          display_name: "&e使用骨粉催熟"
          conditions:
            item_check:
              type: ITEM
              check_main_hand: true
              material: BONE_MEAL
            target_check:
              type: BLOCK_TARGET
              blocks:
                - WHEAT
        
        # 第三步：收获成熟作物
        step_3_harvest:
          type: FARMING
          display_name: "&6收获成熟小麦"
          action: HARVEST
          crops:
            - WHEAT
          require_fully_grown: true
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

## 🔄 任务重复性机制详解

SagaQuests 支持完整的任务重复性机制，包括重复控制、完成统计和GUI显示。

### 📊 重复性配置

#### 基础重复性设置
```yaml
quest_example:
  repeatable: true           # 是否可重复
  max_completions: 10        # 最大完成次数，-1为无限制
  cooldown: 300             # 重复冷却时间(秒)
  time_limit: 3600          # 限时任务时间(秒，真实时间)，0或不设置为无限时
  
  start_conditions:
    # 任务重复性条件检查（可选，系统会自动检查）
    repeatability:
      type: TASK_REPEATABILITY
      require_repeatable: true
      check_cooldown: true
      check_max_completions: true
```

#### 前置任务完成次数要求
```yaml
advanced_quest:
  start_conditions:
    pre_quest:
      type: PRE_QUEST
      quest_id: beginner_quest
      min_completions: 3      # 要求前置任务完成3次以上
```

### 🖥️ GUI显示功能

#### 任务列表显示
玩家在 `/quests gui` 中查看任务时，每个任务会显示：

- **重复性标识**：
  - `▸ 可重复任务` - 绿色显示，任务可重复完成
  - `▸ 单次任务` - 金色显示，任务只能完成一次

- **完成次数统计**：
  - `▸ 完成次数: 5/10次` - 显示当前完成次数/最大次数
  - `▸ 完成次数: 8次` - 无限制任务的完成次数
  - `▸ 尚未完成` - 从未完成的任务

- **冷却信息**：
  - `▸ 冷却中: 2分30秒` - 显示剩余冷却时间
  - `重复冷却: 5分钟` - 显示重复冷却设置

#### 任务详情显示
在任务详情页面中，会显示相同的重复性信息，确保界面一致性。

### 🎯 实际应用示例

#### 可重复日常任务
```yaml
daily_mining:
  display_name: "&e[日常] 采矿任务"
  type: DAILY
  repeatable: true
  max_completions: 5
  cooldown: 86400  # 24小时
  
  start_conditions:
    level:
      type: LEVEL
      min_level: 10
```

#### 基于完成次数的进阶任务
```yaml
expert_challenge:
  display_name: "&c[专家] 挑战任务"
  repeatable: false
  
  start_conditions:
    # 要求日常采矿完成10次以上
    pre_mining:
      type: PRE_QUEST
      quest_id: daily_mining
      min_completions: 10
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
    type: UNIVERSAL
    action: KILL
    amount: 10
    debug: true  # 启用详细日志，推荐在开发测试时使用
    conditions:
      # 条件配置
```

**Debug 模式功能**：
- 输出每次条件检查的详细结果
- 显示任务进度变化
- 记录条件匹配或失败的原因
- 帮助快速定位配置问题

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

### 🏃 移动任务：马拉松挑战
```yaml
marathon_challenge:
  display_name: "&6马拉松挑战"
  description:
    - "&7在不传送的情况下移动1000格"
  
  type: UNIVERSAL
  action: CUSTOM
  amount: 1
  
  tasks:
    run_marathon:
      type: UNIVERSAL
      action: CUSTOM
      display_name: "&e跑步挑战"
      amount: 1
      
      conditions:
        movement:
          type: PLAYER_MOVEMENT
          movement_type: HORIZONTAL
          min_distance: 1000
          accumulate: true
          reset_on_teleport: true
```

### ⚡ 动作事件任务：潜行刺客
```yaml
stealth_assassin:
  display_name: "&8潜行刺客"
  description:
    - "&7在潜行状态下击杀敌人"
  
  type: UNIVERSAL
  action: KILL
  amount: 5
  
  tasks:
    stealth_kill:
      type: UNIVERSAL
      action: KILL
      display_name: "&c潜行击杀"
      amount: 5
      
      conditions:
        kill_condition:
          type: COMPOSITE
          logic: AND
          conditions:
            entity_target:
              type: ENTITY_TARGET
              entity_type: SKELETON
            sneak_event:
              type: PLAYER_ACTION_EVENT
              event_type: SNEAK
              state: true
```

### ⛵ 船上钓鱼任务：海洋探索
```yaml
ocean_explorer:
  display_name: "&3海洋探索者"
  description:
    - "&7在船上钓取珍贵的海洋宝物"
  
  tasks:
    boat_fishing:
      type: UNIVERSAL
      action: FISHING
      display_name: "&3船上钓宝物"
      amount: 10
      
      conditions:
        boat_treasure:
          type: COMPOSITE
          logic: AND
          conditions:
            fishing_check:
              type: FISHING
              state: CAUGHT_FISH
              require_treasure: true
              min_exp: 5
            vehicle_check:
              type: VEHICLE
              require_in_vehicle: true
              vehicle_types:
                - BOAT
                - CHEST_BOAT
            biome_check:
              type: BIOME
              biomes:
                - OCEAN
                - DEEP_OCEAN
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

## 📚 附录

### 🍓 特殊作物收获机制（甜浆果等）

某些作物具有特殊的收获机制，如甜浆果可以右键采摘而不破坏植株。SagaQuests提供多种配置方式来处理这些特殊情况。

#### 甜浆果收获配置示例

甜浆果具有独特的收获机制：
- **成熟阶段**：age 2-3可以采摘
- **右键采摘**：保留植株，掉落1-3个浆果
- **左键破坏**：破坏植株，掉落所有浆果

##### 方法1：使用专门的INTERACT_HARVEST动作（推荐，简洁）
```yaml
harvest_sweet_berries:
  type: UNIVERSAL
  action: FARMING
  amount: 20
  
  conditions:
    berry_harvest:
      type: COMPOSITE
      logic: OR
      conditions:
        # 左键破坏
        break_harvest:
          type: FARMING
          action: HARVEST
          crops:
            - SWEET_BERRY_BUSH
          min_growth: 2
        # 右键采摘（专门动作）
        interact_harvest:
          type: FARMING
          action: INTERACT_HARVEST  # 新增的专门动作
          crops:
            - SWEET_BERRY_BUSH
          min_growth: 2
```

##### 方法2：使用通用组合条件（最灵活）
```yaml
harvest_sweet_berries:
  type: UNIVERSAL
  action: CUSTOM  # 使用CUSTOM接受多种事件
  amount: 20
  
  conditions:
    berry_harvest:
      type: COMPOSITE
      logic: OR
      conditions:
        # 左键破坏
        break_harvest:
          type: FARMING
          action: HARVEST
          crops:
            - SWEET_BERRY_BUSH
          require_fully_grown: false
          min_growth: 2
        # 右键采摘（通用组合）
        interact_harvest:
          type: COMPOSITE
          logic: AND
          conditions:
            # 检测右键交互
            interact_action:
              type: ACTION
              action: RIGHT_CLICK_BLOCK
            # 检测目标方块
            interact_bush:
              type: BLOCK_TARGET
              blocks:
                - SWEET_BERRY_BUSH
            # 可选：检测物品获取
            collect_berries:
              type: ITEM
              items:
                - material: SWEET_BERRIES
                  amount: 1
```

#### 选择建议

| 场景 | 推荐方法 | 原因 |
|------|---------|------|
| 简单的甜浆果收获 | 方法1（INTERACT_HARVEST） | 配置简洁，专门优化 |
| 需要精确控制条件 | 方法2（通用组合） | 可以添加更多条件检查 |
| 混合多种检测方式 | 两种方法结合 | 在OR条件中混用 |

#### 其他支持右键采摘的作物

```yaml
# 发光浆果（洞穴藤蔓）
harvest_glow_berries:
  conditions:
    glow_berry_harvest:
      type: FARMING
      action: INTERACT_HARVEST
      crops:
        - CAVE_VINES
        - CAVE_VINES_PLANT
```

### 🌾 农作物Material映射表

在配置农业任务时，需要注意种子(seeds)和作物(crops)使用不同的Material名称：

| 种子 (Seeds) | 作物方块 (Crop Blocks) | 收获物品 (Harvest Item) | 说明 |
|--------------|------------------------|-------------------------|------|
| `WHEAT_SEEDS` | `WHEAT` | `WHEAT` | 小麦 |
| `BEETROOT_SEEDS` | `BEETROOTS` | `BEETROOT` | 甜菜根（注意作物是复数） |
| `CARROT` | `CARROTS` | `CARROT` | 胡萝卜（既是种子也是食物） |
| `POTATO` | `POTATOES` | `POTATO` | 马铃薯（既是种子也是食物） |
| `MELON_SEEDS` | `MELON_STEM` | `MELON_SLICE` | 西瓜（茎和果实分离） |
| `PUMPKIN_SEEDS` | `PUMPKIN_STEM` | `PUMPKIN` | 南瓜（茎和果实分离） |
| `NETHER_WART` | `NETHER_WART` | `NETHER_WART` | 地狱疣（种子和作物相同） |
| `COCOA_BEANS` | `COCOA` | `COCOA_BEANS` | 可可豆 |
| `SWEET_BERRIES` | `SWEET_BERRY_BUSH` | `SWEET_BERRIES` | 甜浆果 |
| `GLOW_BERRIES` | `CAVE_VINES` | `GLOW_BERRIES` | 发光浆果（1.17+） |
| `TORCHFLOWER_SEEDS` | `TORCHFLOWER_CROP` | `TORCHFLOWER` | 火把花（1.19+） |
| `PITCHER_POD` | `PITCHER_CROP` | `PITCHER_PLANT` | 瓶子草（1.19+） |

#### 特殊种植物

| 种植物 | Material名称 | 说明 |
|--------|-------------|------|
| 竹子 | `BAMBOO` | 可种植和收获 |
| 海带 | `KELP` | 水下种植 |
| 甘蔗 | `SUGAR_CANE` | 需要靠近水源 |
| 仙人掌 | `CACTUS` | 只能在沙子上种植 |
| 棕色蘑菇 | `BROWN_MUSHROOM` | 需要低光照 |
| 红色蘑菇 | `RED_MUSHROOM` | 需要低光照 |
| 绯红菌 | `CRIMSON_FUNGUS` | 下界真菌 |
| 诡异菌 | `WARPED_FUNGUS` | 下界真菌 |

#### 使用提示

1. **种植检测**：使用 `ITEM` 条件检查手持种子 + `BLOCK_TARGET` 检查放置的作物方块
2. **收获检测**：使用 `FARMING` 条件的 `action: HARVEST` 配合 `require_fully_grown: true`
3. **复数形式**：注意某些作物方块使用复数形式（如 `CARROTS`, `POTATOES`, `BEETROOTS`）
4. **双重身份**：胡萝卜和马铃薯既可作为种子也可作为食物

### 🔗 ITEM条件手持物品检测

`ITEM` 条件支持检查玩家手持的物品，特别适用于种植任务：

```yaml
conditions:
  hand_item_check:
    type: ITEM
    check_main_hand: true    # 检查主手物品
    check_off_hand: false    # 检查副手物品（可选）
    material: WHEAT_SEEDS
    amount: 1
```

在 `BlockPlaceEvent`（方块放置事件）中，`ITEM` 条件会自动检查 `getItemInHand()` 返回的物品，确保准确检测玩家使用的种子类型。
