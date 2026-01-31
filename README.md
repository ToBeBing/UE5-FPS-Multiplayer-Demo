# UE5 Multiplayer FPS Demo - 开局一课客户端大作业 
**引擎版本：** Unreal Engine 5.7

## 🎮 项目简介

本项目是基于 Unreal Engine 5 First Person Template 开发的多人在线第一人称射击（FPS）游戏 Demo。项目重点实现了 **Listen-Server 架构下的多人网络同步**、**AI 行为树系统** 以及 **角色动画状态机**。

玩家可以进行多人局域网对战，拾取武器，击败具备智能行为的僵尸敌人，并体验完整的游戏胜负循环。

## ✨ 核心功能

### 🛠️ 网络同步 (Multiplayer Networking)
* **架构**：采用 Listen-Server 模式，严格遵循 Server Authoritative（服务器授权）原则。
* **武器同步**：实现了武器生成、拾取、装备的完整 Replicated 流程，解决了客户端“幽灵武器”和物理模拟导致的掉落问题。
* **射击同步**：
    * **逻辑**：Server RPC (`RunOnServer`) 处理射线检测与伤害判定，杜绝客户端作弊。
    * **表现**：Multicast RPC (`NetMulticast`) 处理枪口火光、音效和弹道轨迹，确保所有端可见。
* **状态同步**：血量、弹药数、手持武器状态均通过 `RepNotify` 进行高效同步。

### 🤖 AI 敌人系统
* **行为树 (Behavior Tree)**：构建了完整的 AI 逻辑，包含巡逻、侦测玩家 (`MoveTo`) 和攻击 (`Attack`)。
* **动画同步**：解决了 BTTask 动画仅在服务器播放的问题，通过自定义的多播事件 (`Multi_PlayAttackAnim`) 实现了攻击动作的多端同步。

### 🔫 武器与战斗系统
* **多武器切换**：支持步枪与手枪的切换，实现了“放下旧武器 -> 拿起新武器”的逻辑，解决了模型穿模问题。
* **伤害归属**：修正了 `GetPlayerPawn(0)` 的错误调用，通过传递 Instigator 实现了正确的伤害来源判定（击杀者判定）。

### 🏃 动画系统
* **分层混合 (Layered Blend per Bone)**：实现了下半身移动、上半身独立持枪的混合效果。
* **状态机**：利用枚举 (`E_武器分类`) 驱动动画蓝图，在空手、步枪、手枪姿态间平滑切换。

## 📸 游戏截图

> ![战斗画面](图片链接)
> ![AI敌人](图片链接)

## 🚀 如何运行

1.  克隆本仓库到本地。
2.  双击 `.uproject` 打开项目。
3.  **联机测试**：
    * 点击 Play 按钮旁的三个点，将 **Number of Players** 设置为 2。
    * **Net Mode** 设置为 `Play as Listen Server`。

## 📂 目录结构说明

* `/Content/PlayerBlueprint`: 玩家角色核心逻辑 (Player_BP)。
* `/Content/weaponBluePrints`: 武器类 (Weapon_BP)、子弹及数据资产。
* `/Content/enemyBlueprint`: AI 敌人、行为树及黑板资源。
* `/Content/Characters`: 角色网格体与动画蓝图 (ABP)。

## 📝 致谢

感谢课程老师的指导。本项目在开发过程中深入研究了 UE5 的 Gameplay 框架与网络复制机制，特别是解决了 RPC 调用时机与 Proxy 代理角色的同步问题。

---
*Created for [开局一课] Final Assignment*
