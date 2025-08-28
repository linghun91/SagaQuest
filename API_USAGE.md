# SagaQuests API 使用文档

## 概述
SagaQuests提供了完整的API接口，使其他插件能够轻松与任务系统集成。所有API方法都通过`SagaQuestsAPI`类的静态方法访问。

## API方法详解

### 1. 任务操作

#### 让玩家接受任务
```java
import cn.i7mc.sagaQuests.api.SagaQuestsAPI;
import cn.i7mc.sagaQuests.api.QuestAPIResult;

// 让玩家接受任务
QuestAPIResult result = SagaQuestsAPI.startQuest(player, "main_quest_1");
if (result.isSuccess()) {
    player.sendMessage("成功接受任务！");
} else {
    player.sendMessage("无法接受任务: " + result.getMessage());
}
```

#### 完成任务
```java
// 完成任务（需要所有任务目标已完成）
QuestAPIResult result = SagaQuestsAPI.completeQuest(player, "main_quest_1");
if (result.isSuccess()) {
    // 任务完成，奖励已自动发放
    Map<String, Object> data = result.getData(Map.class);
    player.sendMessage("恭喜完成任务: " + data.get("questName"));
}

// 完成任务（自动完成未完成的子任务）
QuestAPIResult result2 = SagaQuestsAPI.completeQuest(player, "main_quest_1", true);
if (result2.isSuccess()) {
    player.sendMessage("任务已完成（包含自动完成的子任务）");
}
```

#### 强制完成任务
```java
// 直接完成任务（自动完成所有子任务）
QuestAPIResult result = SagaQuestsAPI.directCompleteQuest(player, "main_quest_1");
if (result.isSuccess()) {
    Map<String, Object> data = result.getData(Map.class);
    List<String> completedTasks = (List<String>) data.get("completedTasks");
    player.sendMessage("任务已强制完成，包含 " + completedTasks.size() + " 个子任务");
}

// 强制完成所有子任务
QuestAPIResult result2 = SagaQuestsAPI.forceCompleteAllTasks(player, "main_quest_1");
if (result2.isSuccess()) {
    // 所有子任务已完成，现在可以正常完成总任务
    SagaQuestsAPI.completeQuest(player, "main_quest_1");
}

// 强制完成指定子任务
QuestAPIResult result3 = SagaQuestsAPI.forceCompleteTask(player, "main_quest_1", "collect_wood");
if (result3.isSuccess()) {
    player.sendMessage("子任务已完成: " + result3.getData(Map.class).get("taskName"));
}

// 手动设置子任务进度
QuestAPIResult result4 = SagaQuestsAPI.setTaskProgress(player, "main_quest_1", "collect_wood", 10.0);
if (result4.isSuccess()) {
    Map<String, Object> data = result4.getData(Map.class);
    double percentage = (Double) data.get("percentage");
    player.sendMessage("进度已更新: " + String.format("%.1f%%", percentage));
}
```

#### 放弃任务
```java
// 放弃进行中的任务
QuestAPIResult result = SagaQuestsAPI.abandonQuest(player, "daily_quest");
if (result.isSuccess()) {
    player.sendMessage("已放弃任务");
}
```

### 2. 查询操作

#### 检查任务状态
```java
// 检查是否完成过某任务
if (SagaQuestsAPI.hasCompletedQuest(player, "tutorial_quest")) {
    // 玩家已完成教程任务，可以给予额外奖励
    player.getInventory().addItem(bonusItem);
}

// 检查是否正在进行某任务
if (SagaQuestsAPI.hasActiveQuest(player, "boss_quest")) {
    player.sendMessage("你正在进行BOSS任务！");
}

// 检查任务是否存在
if (!SagaQuestsAPI.questExists("custom_quest")) {
    player.sendMessage("任务不存在！");
}
```

#### 获取任务列表
```java
// 获取玩家所有进行中的任务
List<String> activeQuests = SagaQuestsAPI.getActiveQuests(player);
player.sendMessage("你有 " + activeQuests.size() + " 个进行中的任务");

// 获取玩家所有已完成的任务
List<String> completedQuests = SagaQuestsAPI.getCompletedQuests(player);
for (String questId : completedQuests) {
    player.sendMessage("已完成: " + questId);
}

// 获取所有可用任务
List<String> allQuests = SagaQuestsAPI.getAllQuestIds();
```

#### 获取任务进度
```java
// 获取详细的任务进度
QuestAPIResult progressResult = SagaQuestsAPI.getQuestProgress(player, "mining_quest");
if (progressResult.isSuccess()) {
    Map<String, Object> progress = progressResult.getData(Map.class);
    double overallProgress = (Double) progress.get("overallProgress");
    player.sendMessage("任务进度: " + String.format("%.1f%%", overallProgress));
    
    // 获取各个任务目标的进度
    List<Map<String, Object>> tasks = (List<Map<String, Object>>) progress.get("tasks");
    for (Map<String, Object> task : tasks) {
        String taskName = (String) task.get("taskName");
        double taskProgress = (Double) task.get("progress");
        double required = (Double) task.get("required");
        player.sendMessage(taskName + ": " + (int)taskProgress + "/" + (int)required);
    }
}
```

#### 获取任务详细状态（调试用）
```java
// 获取任务和子任务的详细状态信息
QuestAPIResult statusResult = SagaQuestsAPI.getQuestDetailedStatus(player, "main_quest_1");
if (statusResult.isSuccess()) {
    Map<String, Object> status = statusResult.getData(Map.class);
    
    // 基本状态
    boolean isActive = (Boolean) status.get("isActive");
    boolean isCompleted = (Boolean) status.get("isCompleted");
    boolean canStart = (Boolean) status.get("canStart");
    
    player.sendMessage("任务状态:");
    player.sendMessage("- 进行中: " + isActive);
    player.sendMessage("- 已完成: " + isCompleted);
    player.sendMessage("- 可开始: " + canStart);
    
    // 如果任务在进行中，显示子任务详情
    if (isActive && status.containsKey("tasks")) {
        List<Map<String, Object>> tasks = (List<Map<String, Object>>) status.get("tasks");
        player.sendMessage("子任务详情:");
        for (Map<String, Object> task : tasks) {
            String taskId = (String) task.get("taskId");
            String percentage = (String) task.get("percentage");
            boolean complete = (Boolean) task.get("isComplete");
            player.sendMessage("  - " + taskId + ": " + percentage + (complete ? " ✓" : ""));
        }
    }
    
    // 显示调试提示
    if (status.containsKey("hints")) {
        List<String> hints = (List<String>) status.get("hints");
        player.sendMessage("可用操作:");
        for (String hint : hints) {
            player.sendMessage("  " + hint);
        }
    }
}
```

### 3. 条件检查

#### 检查是否可以开始任务
```java
// 检查玩家是否满足开始任务的条件
QuestAPIResult canStartResult = SagaQuestsAPI.canStartQuest(player, "advanced_quest");
if (canStartResult.isFailure()) {
    QuestAPIResult.FailureReason reason = canStartResult.getFailureReason();
    switch (reason) {
        case ON_COOLDOWN:
            long cooldown = (Long) canStartResult.getMetadata("cooldownRemaining");
            player.sendMessage("任务冷却中，剩余: " + cooldown + "秒");
            break;
        case CONDITIONS_NOT_MET:
            player.sendMessage("你不满足任务条件");
            break;
        case MAX_ACTIVE_QUESTS:
            player.sendMessage("你的活动任务数量已达上限");
            break;
        default:
            player.sendMessage("无法开始任务: " + canStartResult.getMessage());
    }
}
```

### 4. 获取任务信息

```java
// 获取任务的详细信息
QuestAPIResult infoResult = SagaQuestsAPI.getQuestInfo("epic_quest");
if (infoResult.isSuccess()) {
    Map<String, Object> info = infoResult.getData(Map.class);
    String displayName = (String) info.get("displayName");
    List<String> description = (List<String>) info.get("description");
    boolean repeatable = (Boolean) info.get("repeatable");
    long timeLimit = (Long) info.get("timeLimit");
    
    player.sendMessage("任务: " + displayName);
    for (String line : description) {
        player.sendMessage(line);
    }
    if (repeatable) {
        player.sendMessage("此任务可重复完成");
    }
    if (timeLimit > 0) {
        player.sendMessage("限时任务: " + timeLimit + "秒");
    }
}
```

## 实际应用示例

### 示例1：商店系统集成
```java
public class QuestShop {
    
    @EventHandler
    public void onShopPurchase(ShopPurchaseEvent event) {
        Player player = event.getPlayer();
        String itemId = event.getItemId();
        
        // 检查玩家是否完成了解锁此商品的任务
        String requiredQuest = getRequiredQuest(itemId);
        if (requiredQuest != null) {
            if (!SagaQuestsAPI.hasCompletedQuest(player, requiredQuest)) {
                event.setCancelled(true);
                player.sendMessage("需要先完成任务: " + requiredQuest);
                return;
            }
        }
        
        // 购买成功，可能触发购买任务进度
        // 任务系统会自动处理相关事件
    }
}
```

### 示例2：副本系统集成
```java
public class DungeonManager {
    
    public boolean canEnterDungeon(Player player, String dungeonId) {
        // 某些副本需要完成前置任务
        String prerequisiteQuest = getPrerequisiteQuest(dungeonId);
        if (prerequisiteQuest != null) {
            if (!SagaQuestsAPI.hasCompletedQuest(player, prerequisiteQuest)) {
                player.sendMessage("需要完成前置任务才能进入副本");
                return false;
            }
        }
        
        // 进入副本时自动接受相关任务
        String dungeonQuest = getDungeonQuest(dungeonId);
        if (dungeonQuest != null && !SagaQuestsAPI.hasActiveQuest(player, dungeonQuest)) {
            QuestAPIResult result = SagaQuestsAPI.startQuest(player, dungeonQuest);
            if (result.isSuccess()) {
                player.sendMessage("已接受副本任务");
            }
        }
        
        return true;
    }
    
    public void onDungeonComplete(Player player, String dungeonId) {
        // 完成副本时检查并完成相关任务
        String dungeonQuest = getDungeonQuest(dungeonId);
        if (dungeonQuest != null && SagaQuestsAPI.hasActiveQuest(player, dungeonQuest)) {
            // 检查任务进度
            QuestAPIResult progress = SagaQuestsAPI.getQuestProgress(player, dungeonQuest);
            if (progress.isSuccess()) {
                Map<String, Object> data = progress.getData(Map.class);
                if ((Boolean) data.get("isComplete")) {
                    // 任务目标已完成，可以完成任务
                    SagaQuestsAPI.completeQuest(player, dungeonQuest);
                }
            }
        }
    }
}
```

### 示例3：成就系统集成
```java
public class AchievementManager {
    
    @EventHandler
    public void onAchievementUnlock(AchievementUnlockEvent event) {
        Player player = event.getPlayer();
        String achievementId = event.getAchievementId();
        
        // 某些成就需要完成特定任务
        if (achievementId.equals("quest_master")) {
            List<String> completedQuests = SagaQuestsAPI.getCompletedQuests(player);
            if (completedQuests.size() >= 50) {
                // 玩家完成了50个任务，授予成就
                grantAchievement(player, "quest_master");
            }
        }
        
        // 解锁成就可能是某些任务的目标
        // 任务系统会自动处理
    }
}
```

### 示例4：NPC交互系统
```java
public class NPCInteraction {
    
    @EventHandler
    public void onNPCInteract(NPCInteractEvent event) {
        Player player = event.getPlayer();
        String npcId = event.getNPCId();
        
        // 获取NPC相关的任务
        List<String> npcQuests = getNPCQuests(npcId);
        
        for (String questId : npcQuests) {
            // 检查是否可以接受新任务
            if (!SagaQuestsAPI.hasActiveQuest(player, questId) && 
                !SagaQuestsAPI.hasCompletedQuest(player, questId)) {
                
                QuestAPIResult canStart = SagaQuestsAPI.canStartQuest(player, questId);
                if (canStart.isSuccess()) {
                    // 显示任务对话
                    showQuestDialog(player, questId);
                    return;
                }
            }
            
            // 检查是否有进行中的任务需要与此NPC交谈
            if (SagaQuestsAPI.hasActiveQuest(player, questId)) {
                QuestAPIResult progress = SagaQuestsAPI.getQuestProgress(player, questId);
                // 显示任务进度对话
                showProgressDialog(player, progress);
                return;
            }
        }
    }
}
```

## 错误处理

### 使用QuestAPIResult处理错误
```java
QuestAPIResult result = SagaQuestsAPI.startQuest(player, questId);

// 检查操作是否成功
if (result.isSuccess()) {
    // 成功处理
    Object data = result.getData();
    String message = result.getMessage();
} else {
    // 失败处理
    String errorMessage = result.getMessage();
    
    // 获取详细的失败原因
    if (result.getMetadata("cooldownRemaining") != null) {
        long cooldown = (Long) result.getMetadata("cooldownRemaining");
        player.sendMessage("冷却剩余: " + cooldown + "秒");
    }
}
```

### 失败原因枚举
```java
public enum FailureReason {
    QUEST_NOT_FOUND,        // 任务不存在
    PLAYER_OFFLINE,         // 玩家不在线
    ALREADY_ACTIVE,         // 任务已在进行中
    ALREADY_COMPLETED,      // 任务已完成
    CONDITIONS_NOT_MET,     // 不满足条件
    ON_COOLDOWN,           // 冷却中
    MAX_ACTIVE_QUESTS,     // 达到最大任务数
    INTERNAL_ERROR,        // 内部错误
    NOT_ACTIVE,            // 任务未激活
    QUEST_EXPIRED,         // 任务已过期
    INVALID_PARAMETER,     // 参数无效
    PERMISSION_DENIED      // 权限不足
}
```