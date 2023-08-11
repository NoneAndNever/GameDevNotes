## 宏

### UPROPERTY()

#### meta

##### BindWidget

> By marking a pointer to a widget as, you can create an **identically-named widget in a Blueprint subclass of your C++ class**, and at run-time **access it from the C++**

通过标记一个Widget组件（按钮，文本等）指针为 `BindWidget`，可以通过C++在运行时访问**同名**组件（不同名会崩溃）

## 方法

###

#### `AActor::DetachFromActor`

切换关卡

### 无缝与非无缝切换

**虚幻引擎（UE）** 中主要有两种转移方式：**无缝** 和 **非无缝方式**。两者的主要区别在于，**无缝** 转移是一种非阻塞（non-blocking）操作，而 **非无缝** 转移则是一种阻塞（blocking）操作。

当客户端执行非无缝转移时，客户端将与服务器断开连接，然后重新连接到同一服务器，而服务器将准备新的地图以供加载。

我们建议虚幻引擎多人模式游戏尽量采用无缝转移。这样做通常可以提供更流畅的体验，同时避免重新连接过程中可能出现的问题。

有三种情形中必然产生非无缝转移：

- 初次加载地图时
- 初次作为客户端连接服务器时
- 想要终止一个多人模式游戏并启动新游戏时

有三个用来驱动转移的主要函数：`UEngine::Browse`、`UWorld::ServerTravel` 和 `APlayerController::ClientTravel`。在确定使用哪个函数时，您可能会感到有些困惑，所以请遵循下面的准则：

#### `UEngine::Browse`

- 就像是加载新地图时的硬重置。
- 将始终导致非无缝切换。
- 将导致服务器在切换到目标地图前与当前客户端断开连接。
- 客户端将与当前服务器断开连接。
- 专用服务器无法切换至其他服务器，因此地图必须存储在本地（不能是 URL）。

#### `UWorld::ServerTravel`

- 仅适用于服务器。
- 会将服务器跳转到新的世界/场景。
- 所有连接的客户端都会跟随。
- 这就是多人游戏在地图之间转移时所用的方法，而服务器将负责调用此函数。
- 服务器将为所有已连接的客户端玩家调用 `APlayerController::ClientTravel`。

#### ` APlayerController::ClientTravel`

- 如果从客户端调用，则转移到新的服务器
- 如果从服务器调用，则要求特定客户端转移到新地图（但仍然连接到当前服务器）

## Enum

### EngineTypes

#### FAttachmentTransformRules

##### 定义

```c++
struct ENGINE_API FAttachmentTransformRules
{
	/** Various preset attachment rules. Note that these default rules do NOT by default weld simulated bodies */
	
	//保持两个物体的相对位置不变
	static FAttachmentTransformRules KeepRelativeTransform;
	//保持两个物体的世界位置不变
	static FAttachmentTransformRules KeepWorldTransform;
	//忽略缩放对齐到目标,保持自己的缩放
	static FAttachmentTransformRules SnapToTargetNotIncludingScale;
	//保持比例对齐到目标，对齐到目标的缩放
	static FAttachmentTransformRules SnapToTargetIncludingScale;
	
	
	FAttachmentTransformRules(EAttachmentRule InRule, bool bInWeldSimulatedBodies)
		: LocationRule(InRule)
		, RotationRule(InRule)
		, ScaleRule(InRule)
		, bWeldSimulatedBodies(bInWeldSimulatedBodies)
	{}

	FAttachmentTransformRules(EAttachmentRule InLocationRule, EAttachmentRule InRotationRule, EAttachmentRule InScaleRule, bool bInWeldSimulatedBodies)
		: LocationRule(InLocationRule)
		, RotationRule(InRotationRule)
		, ScaleRule(InScaleRule)
		, bWeldSimulatedBodies(bInWeldSimulatedBodies)
	{}

	/** The rule to apply to location when attaching */
	EAttachmentRule LocationRule;

	/** The rule to apply to rotation when attaching */
	EAttachmentRule RotationRule;

	/** The rule to apply to scale when attaching */
	EAttachmentRule ScaleRule;

	/** Whether to weld simulated bodies together when attaching */
	bool bWeldSimulatedBodies;
};
```

##### 成员变量

| 访问修饰符 | 变量类型                                                     | 变量名               | 描述                                                     |
| :--------- | :----------------------------------------------------------- | -------------------- | -------------------------------------------------------- |
| 公有变量   | [bool](https://docs.unrealengine.com/4.26/en-US/API/Runtime/LiveLinkInterface/bool/index.html) | bWeldSimulatedBodies | Whether to weld simulated bodies together when attaching |
| 公有变量   | [EAttachmentRule](https://docs.unrealengine.com/4.26/en-US/API/Runtime/Engine/Engine/EAttachmentRule/index.html) | LocationRule         | The rule to apply to location when attaching             |
| 公有变量   | [EAttachmentRule](https://docs.unrealengine.com/4.26/en-US/API/Runtime/Engine/Engine/EAttachmentRule/index.html) | RotationRule         | The rule to apply to rotation when attaching             |
| 公有变量   | [EAttachmentRule](https://docs.unrealengine.com/4.26/en-US/API/Runtime/Engine/Engine/EAttachmentRule/index.html) | ScaleRule            | The rule to apply to scale when attaching                |

