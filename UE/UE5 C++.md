

#### check()

ue的check函数是一种断言函数，用来检测和诊断运行时的异常或无效条件。¹ check函数的作用是，如果传入的表达式为假，就会终止程序的运行，并输出错误信息到日志中。¹²

check函数可以用来检查指针是否为空，除数是否为零，函数是否递归调用，或者其他代码需要但不方便每次都检查的假设。¹ check函数有时候还可以发现导致延迟崩溃的bug，比如删除了一个将来会用到的对象，从而帮助开发者找到崩溃的根源。¹

check函数只在Debug, Development, Test, 和 Shipping Editor构建中有效，而在Shipping构建中默认无效。¹ 如果想让check函数在所有构建中有效，可以定义USE_CHECKS_IN_SHIPPING为真值（通常是1）。¹

#### 初始化变量

在头文件中使用{}初始化和在构造函数中使用初始化列表有一些不同，主要有以下几点：¹²

- 在头文件中使用{}初始化可以简化代码，避免重复写相同的初始化值，特别是当有多个构造函数时。¹²
- 在头文件中使用{}初始化可以避免一些隐式转换的问题，比如narrowing conversion。¹²
- 在头文件中使用{}初始化可能会导致不必要的依赖，因为初始化值暴露在头文件中，如果需要修改，可能会引起很多代码的重新编译。²
- 在构造函数中使用初始化列表可以根据构造函数的参数来动态地初始化成员变量，而在头文件中使用{}初始化只能用编译时确定的常量。¹²
- 在构造函数中使用初始化列表可以对const或引用类型的成员变量进行初始化，而在头文件中使用{}初始化不能。¹³

源: 与必应的对话， 2023/5/11
(1) c++ - Constructor initializer list vs initializing in the header file .... https://stackoverflow.com/questions/52091600/constructor-initializer-list-vs-initializing-in-the-header-file.
(2) C++ - initializing variables in header vs with constructor. https://stackoverflow.com/questions/28413154/c-initializing-variables-in-header-vs-with-constructor.
(3) Standard library header <initializer_list> (C++11). https://en.cppreference.com/w/cpp/header/initializer_list.
(4) Brace initialization for classes, structs, and unions. https://learn.microsoft.com/en-us/cpp/cpp/initializing-classes-and-structs-without-constructors-cpp?view=msvc-170.





#### TSubClass

TSubclassOf是UE4中提供的一个模板类，它用于表示一个UClass的子类。它可以保证类型安全，只允许指定或选择继承自某个基类的类。例如，如果你想定义一个投射物类的属性，你可以使用TSubclassOf<UDamageType>来限制只能选择继承自UDamageType的类。这样可以避免错误地指定一个不相关的类，也可以在编辑器中过滤出合适的类列表。¹²

TSubclassOf的实现原理是利用了模板特化和函数重载，在编译期就可以判断两个类是否有继承关系，从而决定使用UClass还是FFieldClass作为反射类。³

希望这能帮到你！如果你还有其他问题，请告诉我。

源: 与必应的对话， 2023/5/17
(1) TSubclassOf | Unreal Engine 4.27 Documentation. https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/ProgrammingWithCPP/UnrealArchitecture/TSubclassOf/.
(2) TSubclassOf | 虚幻引擎文档 - Unreal Engine. https://docs.unrealengine.com/4.26/zh-CN/ProgrammingAndScripting/ProgrammingWithCPP/UnrealArchitecture/TSubclassOf/.
(3) UE4：TSubclassOf相关 - 知乎 - 知乎专栏. https://zhuanlan.zhihu.com/p/401008837.



#### FActorSpawnParameters.Instigator

Instigator是FActorSpawnParameters中的一个参数，它用于表示生成的Actor造成伤害的责任者。它是一个APawn类型的指针，可以在生成Actor时指定，也可以留空。¹

Instigator的作用是可以在生成的Actor中获取到它，从而进行一些逻辑判断或操作。例如，如果你生成了一个投射物类的Actor，你可以在投射物类中调用GetInstigator()方法，来获取Instigator，并让投射物忽略与Instigator的碰撞。³

希望这能帮到你！如果你还有其他问题，请告诉我。

源: 与必应的对话， 2023/5/17
(1) Instigator | Unreal Engine Documentation. https://api.unrealengine.com/INT/API/Runtime/Engine/Engine/FActorSpawnParameters/Instigator/index.html.
(2) FActorSpawnParameters - how exactly does it work?. https://forums.unrealengine.com/t/factorspawnparameters-how-exactly-does-it-work/326493.
(3) FActorSpawnParameters | Unreal Engine Documentation. https://api.unrealengine.com/INT/API/Runtime/Engine/Engine/FActorSpawnParameters/index.html.