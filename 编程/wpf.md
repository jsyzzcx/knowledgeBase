### 一、概述

WPF 核心可以概括为：一套强大的展示系统，通过数据驱动的方式，将业务逻辑与用户界面清晰分离。

> 1,XAML：如何“写”界面。
> 2,数据绑定：如何让界面“活”起来，与数据联动。
> 3,依赖属性与路由事件：理解 WPF 底层“如何工作”
> 4, MVVM 模式：如何“良好地组织”你的 WPF 代码

### 二、XAML - UI 的骨架

###### 2.1 是什么 - 一种基于 XML 的声明式语言，用于定义用户界面。

###### 2.2 核心价值：将 UI 设计（XAML）与程序逻辑（C# 代码后置）分离，使开发者和设计师可以并行工作。

###### 2.3 关键概念

- 元素树：XAML 元素构成逻辑树和可视化树，这是理解路由事件和资源查找的基础。

- 逻辑树**是“什么”** - 代表了你在 XAML 或 C# 代码中定义的 UI 元素层次结构。它反映了应用程序的“逻辑”结构；
  
  - 用途
    
    - **资源查找** - 当为一个元素设置资源时，WPF 会沿着逻辑树向上查找，直到找到该资源或到达根元素
    - **属性值继承** - 某些依赖项属性（如 DataContext`, `FontSize）会沿着逻辑树向下继承。如果在 Window 上设置了 DataContext，那么其逻辑树下的所有子元素都能访问到它

- 可视化树**是“怎么画”** - 描述了 UI 元素是如何在屏幕上**被渲染**的。包含了控件的内部结构（一个Button在可视化树中可能由一个 Border、一个 ContentPresenter和其他元素组成）
  
  - 用途
    
    - 处理渲染 - WPF 的渲染引擎遍历可视化树来决定如何绘制每个像素
    
    - **路由事件**（如 MouseDown）的传播路径（隧道和冒泡）是基于可视化树

- 标记扩展：{Binding}, {StaticResource}, {x\:Type} 等，为 XAML 提供动态能力和灵活性。是一种特殊的 XAML 语法，允许在属性字符串中嵌入动态的、由代码逻辑决定的值，而不仅仅是简单的文本或数字。通常用于：
  
  - 数据绑定
  
  - 引用 静态|动态资源
  
  - 类型转换
  
  - 其他需要在 XAML 中调用后台逻辑的场景
  
  - 所有扩展标记在 XAML 中都用一对花括号 `{ }` 表示

### 三、布局系统 - UI 的管家，自适应布局，不使用绝对坐标

###### 3.1 核心原则：“测量（Measure）” 和 “排列（Arrange）”。容器会询问子元素所需的大小和位置，然后进行分配。**测量**和**排列**。这是理解 WPF 如何确定控件大小和位置的基础

- 测量决定 **“想要多大”** ，排列决定 **“实际多大和在哪”**。

- 这是一个**递归的、自上而下的过程**，从视觉树的根开始，遍历所有子元素

- WPF 布局是一个递归的、双向协商的过程，主要由两个步骤完成
  
  - Measure - 是布局周期的第一步。它的目的是确定每个子元素在给定约束下的理想大小。
    
    - **核心重写方法**：`protected override Size MeasureOverride(Size availableSize)`
    
    - 自定义容器必须重写此方法，来测量其每个子元素并计算自身所需的大小。
  
  - Arrange - 是布局周期的第二步。在知道了所有子元素的期望大小后，容器决定如何分配实际空间给它们。
    
    - **核心重写方法** `protected override Size ArrangeOverride(Size finalSize)`
    
    - 自定义容器必须重写此方法，来定位其每个子元素并确定自身的最终渲染大小

###### 3.2 布局容器

- Grid 网格布局：最强大、最常用。通过行和列进行精确的网格化布局。可用绝对值、相对比例或者自动调整
- UniformGrid 行列均分布局
- StackPanel 栈布局：沿水平或垂直方向依次排列子元素。
- WrapPanel 流式布局：自动换行布局
- DockPanel 停靠布局：将子元素停靠在上下左右。
- Canvas 绝对坐标布局：通过绝对坐标定位，适用于自由绘图，但不适用于常规业务界面。

###### 3.3 控件库

- 基础控件
  
  - TextBlock/Label
  - Button/ToggleButtone
  - TextBox/PasswordBox
  - CheckBox/RadioButton
  - ComboBox/ListBox/ListView
  - Slider，ProgressBar

- 内容控件
  
  - ContentControl、HeaderedContentControl、ItemsControl   
  - DataGrid、TreeView、TabControl
  - Menu/ContextMenu
  - ToolBar、StatusBar

- 其他：**Border**，Image，MediaElement，WebBrowser 

### 四、数据绑定 - WPF 的灵魂，也是 MVVM 模式的基础。

###### 4.1 是什么：在 UI 元素属性与后台数据对象属性之间建立连接，数据变化自动反映到 UI 上

###### 4.2 核心机制

- DataContext 继承：整个绑定系统的 “数据源上下文”，会沿元素树继承。

- 数据源应实现 `INotifyPropertyChanged` 接口：实现双向绑定的关键。数据模型实现此接口，当其属性改变时通知 UI 更新。

- 集合应该使用  `ObservableCollection<T>`:用 ObservableCollection 替代 List 进行集合绑定，因为其实现了INotifyCollectionChanged和INotifyPropertyChanged，能把集合的变化(添加项、移除项)通知控件

###### 4.3 重要概念

- 绑定源：不指定时，默认用控件的DataContext (DataContext/ElementName/RelativeSource/ObjectDataProvider/XmlDataProvider/普通对象/集合/依赖对象/ DataView/DataTable)

- 绑定目标：必须是 WPF 的依赖项属性。可以是继承自 DependencyProperty的属性或控件目标对象属性，需要绑定的控件属性

- 绑定路径 Path - 把源的属性路径暴露给Binding 表达式

- 绑定模式 Mode
  
  - OneWay（源变→UI变）
  - TwoWay（双向）
  - OneTime（仅一次）
  - OneWayToSource（UI变→源变）

- 值转换器 `IValueConverter` - 在绑定过程中对数据进行转换（如：布尔值转显示/隐藏）

- 绑定验证器 `ValidationRule`

- 绑定更新时机 UpdateSourceTrigger
  
  - LostFous 默认 - 目标控件失去焦点，源就会被更新
  
  - PropertyChanged - 实时更新，绑定的属性值改变，源会立即更新
  
  - Explicit - 源不会更新，除非手动来操作

```xml
<!-- 默认行为 - 失去焦点时更新，TextBox.Text 默认LostFocus -->
<TextBox Text="{Binding UserName, Mode=TwoWay}" />

<!-- 实时更新 - 当绑定目标属性发生改变时，更新数据源 -->
<TextBox Text="{Binding UserName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />

<!-- 显式更新 -->
<TextBox Text="{Binding UserName, Mode=TwoWay, UpdateSourceTrigger=Explicit}" />
```

- 四种 RelativeSource模式 - 描述与绑定目标的位置相对的绑定源位置 
  
  - Self  允许将该元素的一个属性绑定到同一元素的其他属性上
  
  - FindAncestor 绑定元素的上级。 要指定 AncestorType：上级元素类型；AncestorLevel：上级元素级别。
  
  - PreviousData 绑定上一个数据项(在数据项列表中)
  
  - TemplatedParent：表示引用应用了模板的元素，此模板中存在数据绑定元素。

```xml
Width="{Binding RelativeSource={RelativeSource Self}, Path=Height} //绑定自身 Height属性
Text="{Binding RelativeSource={RelativeSource FindAncestor, AncestorType={x:Type Border}, AncestorLevel=2},Path=Name}"  //绑定上级元素
```

### 五、依赖属性 - 支撑系统的基石，掌握它对于开发自定义控件和深入理解 WPF 工作机制至关重要

###### 5.1 是什么：WPF 属性系统的全新实现，不同于普通的 .NET 属性。通过Binding从数据源获取值

###### 5.2 依赖属性 vs CLR 属性对比

| 特性   | CLR 属性   | 依赖属性                       |
| ---- | -------- | -------------------------- |
| 存储   | 在类字段中存储  | 在 WPF 属性系统中集中存储            |
| 默认值  | 在构造函数中设置 | 在注册时指定                     |
| 变更通知 | 需要手动实现   | 内置 PropertyChangedCallback |
| 数据绑定 | 只能作为绑定目标 | 可作为绑定源和目标                  |
| 动画支持 | 不支持      | 原生支持                       |
| 样式设置 | 有限支持     | 完整支持                       |
| 值继承  | 不支持      | 支持属性值继承                    |
| 内存开销 | 较低       | 较高（但值不设置时不占用空间）            |

###### 5.3 为何重要：自定义控件、数据绑定、样式、动画、属性值继承 都依赖于它。

###### 5.4 特点：

- 提供属性变更通知、验证和强制转换机制。

- 支持属性值继承（如字体）。

- 可以从多个提供源（样式、动画等）中自动选择值。

###### 5.5 创建依赖属性

- 使用 `DependencyProperty.Register()` 声明并注册依赖属性
- 提供 CLR 属性包装器（仅调用 GetValue/SetValue）
- 使用 `PropertyChangedCallback` 处理，属性变更逻辑

### 六、命令 - 将用户操作与业务逻辑分离; 是实现 MVVM 模式中分离 UI 逻辑和业务逻辑的关键机制

###### 6.1 是什么 - 将用户操作（如点击菜单、按钮）从事件处理程序中解耦出来。

###### 6.2 核心接口

- 命令必须实现 **ICommand** 接口
  
  - `bool CanExecute(object parameter)`- 命令是否可执行
  
  - `void Execute(object parameter)`命令执行时调用的方法
  
  - `event EventHandler CanExecuteChanged` 当命令可执行状态改变时触发

- CommandBinding 命令绑定 -  将命令连接到具体的处理逻辑

###### 6.3 WPF 命令的核心优势

- 关注点分离：UI 逻辑与业务逻辑分离

- 重用性：同一命令可在多个地方使用

- 状态管理：自动控制 UI 元素的启用状态

- 输入统一：统一处理多种输入方式（点击、快捷键等）

- 测试友好：命令逻辑易于单元测试

###### 6.4 命令系统组成

| 组件   | 描述          | 实现                                       |
| ---- | ----------- | ---------------------------------------- |
| 命令   | 定义要执行的操作    | 实现 ICommand 接口                           |
| 命令源  | 触发命令的 UI 元素 | 实现 ICommandSource 接口（如 Button, MenuItem） |
| 命令目标 | 执行命令的对象     | 通过 CommandTarget 属性指定                    |
| 命令绑定 | 连接命令与处理逻辑   | CommandBinding，在 UI 元素或窗口级别定义            |

### 七、样式与模板 - UI 的美容师，是 WPF 实现 UI 与业务逻辑分离的关键技术

###### 7.1 样式（Style）：是一组属性Setter的集合。统一设置控件现有属性的值（如颜色、字体、边距）

###### 7.2 模板（Template）- 完全自定义，控件外观、行为、视觉树的机制

* 控件模板（ControlTemplate）：重新定义控件的完整视觉外观（如将一个圆形按钮做成按钮的功能）

* 数据模板（DataTemplate）：定义数据对象的显示（如定义一个模板来显示一个“客户”对象）

###### 7.3 触发器(Trigger) 是WPF用于响应属性值变化或事件的机制；基于条件动态改变，控件外观和行为

- 属性触发器 Trigger

- 多条件触发器 MultiTrigger

- 数据触发器 DataTrigger

- 多数据触发器 MultiDataTrigger

- 事件触发器 EventTrigger

###### 7.4 样式 vs 模板对比

| 特性    | 样式           | 模板                                |
| ----- | ------------ | --------------------------------- |
| 目的    | 设置控件**现有属性** | **重新定义**控件的视觉结构                   |
| 作用范围  | 单个属性或一组属性    | 整个控件的视觉树                          |
| 内容    | 属性设置器的集合     | 可视化元素（控件模板）或数据展示逻辑（数据模板）          |
| 类型    | `Style`      | `ControlTemplate`, `DataTemplate` |
| 自定义程度 | 中等           | 高（可以完全改变控件外观）                     |

### 八、路由事件 - 事件传递的管道；它提供了强大而灵活的事件处理机制，是构建复杂交互式应用程序的基础

###### 8.1 是什么：在 WPF 元素树中传递的事件。允许多个元素参与处理同一个事件

###### 8.2 路由事件 vs 普通 CLR 事件

| 特性   | 普通 CLR 事件   | 路由事件                      |
| ---- | ----------- | ------------------------- |
| 传播方式 | 直接调用事件处理程序  | 在元素树中路由传播                 |
| 处理位置 | 只能在源元素处理    | 可以在源元素的祖先或后代元素处理          |
| 事件数据 | `EventArgs` | `RoutedEventArgs`（包含路由信息） |
| 事件处理 | 简单的发布-订阅模式  | 支持事件路由策略                  |

###### 8.3 WPF 定义了三种路由策略

| 路由策略         | 描述           | 示意图                              | 适用场景                                                   |
| ------------ | ------------ | -------------------------------- | ------------------------------------------------------ |
| 冒泡 Bubbling  | 从源元素向上传递到根元素 | `源 → Parent → GrandParent → ...` | 大多数用户输入事件（`Click`, `MouseDown`）                        |
| 隧道 Tunneling | 从根元素向下传递到源元素 | `... → GrandParent → Parent → 源` | 预览事件（`PreviewMouseDown`, `PreviewKeyDown`）以 Preview 开头 |
| 直接 Direct    | 直接在源元素处理，不路由 | `源`                              | 传统事件行为，不路由，如 `Loaded`                                  |

###### 8.4 最佳实践

- 合理使用 Handled 属性：避免不必要的事件传播；使用 `e.Handled = true` 停止事件传播

- 理解事件顺序：**隧道 → 冒泡 → 直接**；同一事件，执行时首先是隧道事件，然后是冒泡事件

- 使用 Preview 事件进行验证：在隧道阶段拦截无效输入

### 九、核心架构模式：MVVM 是一种专门为 WPF/Silverlight 等 XAML-based 技术设计的架构模式

| 组件        | 职责                    | 对应 WPF 概念  |
| --------- | --------------------- | ---------- |
| Model     | 业务逻辑和数据模型             | 数据实体、业务服务  |
| View      | 用户界面展示                | XAML、控件、样式 |
| ViewModel | 视图的抽象，连接 View 和 Model | 数据绑定、命令    |

###### 9.1 MVVM 核心原理 - ViewModel：是连接 View 和 Model 的核心桥梁。

- ViewModel 包含数据属性，供 View 数据绑定。

- ViewModel 包含命令（ICommand），供 View 命令绑定。

- ViewModel 包含通知机制；通知 View 更新（通过 INotifyPropertyChanged）

###### 9.2 MVVM 优势总结

* 分离关注点：UI 逻辑与业务逻辑完全分离

* 可测试性：ViewModel 可以轻松进行单元测试

* 可维护性：代码结构清晰，易于维护和扩展

* 团队协作：设计师和开发者可以并行工作

* 代码复用：ViewModel 可以在不同 View 中复用

###### 9.3 主流框架

- CommunityToolkit.Mvvm
  
  - 微软官方维护（Windows Community Toolkit），轻量、现代、零依赖
  
  - 提供ObservableObject、RelayCommand、源生成器（属性/命令自动样板）
  
  - 适合WPF/UWP/WinUI/.NET MAUI，入门成本最低

- Prism
  
  - 企业级、模块化、成熟生态
  
  - 提供依赖注入、模块化、导航、事件聚合器、对话服务、区域管理
  
  - 适合大型WPF/MAUI项目，团队协作与复杂场景

- ReactiveUI
  
  - 函数响应式（FRP）风格，强大的状态/流式处理
  
  - ReactiveObject、ReactiveCommand、交互（Interactions）、View绑定扩展
  
  - 适合有复杂状态流和实时交互的应用，学习曲线较陡

- Caliburn.Micro
  
  - 简洁、约定优于配置（约定绑定动作/名称）
  
  - 优秀的View-Action绑定（例如按钮自动绑定到方法）
  
  - 适合快速开发、代码量小但很高效

- Stylet
  
  - 现代、轻量、易用，约定式绑定
  
  - 依赖注入、导航、生命周期管理，API简洁
  
  - 适合中小型WPF项目，开箱即用

- Catel
  
  - 功能全面：验证、消息、序列化、行为、测试支持
  
  - 模块较多，学习成本高于轻量框架
  
  - 适合需要强验证/复杂模型的项目

- MvvmCross
  
  - .NET 跨平台（Xamarin/.NET MAUI、Android、iOS、WPF）
  
  - 抽象导航、依赖注入、插件生态（定位、SQLite等）
  
  - 适合移动+桌面统一架构的团队

- MVVM Light（现已不维护）
  
  - 轻量、历史经典，提供ViewModelBase、RelayCommand、Messenger
  
  - 已停止维护，建议新项目改用 CommunityToolkit.Mvvm

###### 9.4 常用特性对比

- 轻量易用：CommunityToolkit.Mvvm、Stylet、Caliburn.Micro

- 企业级/模块化：Prism

- 响应式编程：ReactiveUI

- 跨平台统一：MvvmCross

- 强验证/复杂模型：Catel

###### 9.5 选择建议

- 小型或新项目，追求简单高效：选 CommunityToolkit.Mvvm

- 大型/企业项目，需要模块化和完善服务：选 Prism

- 复杂状态流、实时响应：选 ReactiveUI

- 约定式、快速开发：选 Caliburn.Micro 或 Stylet

- 需要移动与桌面共用一套VM：选 MvvmCross

- 强验证/数据规则繁多：选 Catel

### 十、WPF 常用第三方库

| 库名称                                                                                                                                                                             | 主要特点                                                                                                                                                                                                                                                                          | 适用场景/说明                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **MaterialDesignInXamlToolkit**[](https://kf.zx.zbj.com/kaifa/11326.html)[](https://www.sohu.com/a/854684944_121124363)                                                         | 实现Google Material Design风格，控件丰富，文档完善[](https://www.sohu.com/a/854684944_121124363)。                                                                                                                                                                                           | 追求现代Material Design风格，项目对UI一致性要求高。                                                   |
| **HandyControl**[](https://kf.zx.zbj.com/kaifa/11326.html)[](https://hellogithub.com/repository/1e8c46495b924ce39052d142bbbf3a88)[](https://www.sohu.com/a/854684944_121124363) | 组件丰富（70-80余款控件[](https://hellogithub.com/repository/1e8c46495b924ce39052d142bbbf3a88)[](https://www.sohu.com/a/854684944_121124363)），重写原生样式，支持主题切换，中文友好[](https://hellogithub.com/repository/1e8c46495b924ce39052d142bbbf3a88)[](https://www.sohu.com/a/854684944_121124363)。 | 快速开发，需要丰富组件和个性化UI，尤其适合国内开发者[](https://kf.zx.zbj.com/kaifa/11326.html)。               |
| **WPF UI**[](https://kf.zx.zbj.com/kaifa/11326.html)[](https://www.sohu.com/a/854684944_121124363)                                                                              | 基于Fluent Design System[](https://www.sohu.com/a/854684944_121124363)，现代化设计，原生集成感好[](https://www.sohu.com/a/854684944_121124363)。                                                                                                                                              | 开发类似Windows 11风格的应用，追求新颖流畅的界面[](https://kf.zx.zbj.com/kaifa/11326.html)。             |
| **MahApps.Metro**[](https://kf.zx.zbj.com/kaifa/11326.html)[](https://www.sohu.com/a/854684944_121124363)                                                                       | 经典Metro风格[](https://www.sohu.com/a/854684944_121124363)，成熟稳定，社区资源丰富[](https://kf.zx.zbj.com/kaifa/11326.html)。                                                                                                                                                                | 开发类似Visual Studio或Visual Studio Code的IDE界面，或深色主题应用。                                  |
| **ModernWpf**[](https://kf.zx.zbj.com/kaifa/11326.html)[](https://www.sohu.com/a/854684944_121124363)                                                                           | 提供类似Windows 10/11的现代控件风格，微软技术路线贴近[](https://kf.zx.zbj.com/kaifa/11326.html)。                                                                                                                                                                                                  | 希望应用与当前Windows系统风格保持一致，偏好微软官方风格。                                                     |
| **Panuon.WPF.UI**[](https://kf.zx.zbj.com/kaifa/11326.html)[](https://www.sohu.com/a/854684944_121124363)                                                                       | 注重定制化，提供许多自定义控件，拥有中文Wiki文档[](https://www.sohu.com/a/854684944_121124363)。                                                                                                                                                                                                     | 需要高度定制UI，且希望快速上手，文档阅读无障碍[](https://kf.zx.zbj.com/kaifa/11326.html)。                  |
| **CookPopularUI**[](https://ysgdaydayup.blog.csdn.net/article/details/152281525)                                                                                                | 提供100多款常用控件[](https://ysgdaydayup.blog.csdn.net/article/details/152281525)，覆盖各种UI元素，开源免费（MIT License）[](https://ysgdaydayup.blog.csdn.net/article/details/152281525)。                                                                                                         | 需要大量现成控件快速搭建界面。                                                                      |
| **Fluent.Ribbon**[](https://cloud.tencent.cn/developer/article/1863829?from=15425)                                                                                              | 实现类似Microsoft Office的Ribbon界面[](https://cloud.tencent.cn/developer/article/1863829?from=15425)。                                                                                                                                                                               | 需要为应用开发功能区的办公软件类项目[](https://cloud.tencent.cn/developer/article/1863829?from=15425)。 |
| **Extended WPF Toolkit**[](https://cloud.tencent.cn/developer/article/1863829?from=15425)                                                                                       | WPF标准控件的强大补充集，包含图表、属性网格等高级控件[](https://cloud.tencent.cn/developer/article/1863829?from=15425)。                                                                                                                                                                                | 需要图表、颜色选择器等标准控件库未提供的复杂组件。                                                            |
| **DevExpress WPF**[](https://thebrain.com.cn/product/2346/update?uid=17541)                                                                                                     | 商业控件库，功能全面（如报表、仪表盘[](https://thebrain.com.cn/product/2346/update?uid=17541)），提供大量模板[](https://thebrain.com.cn/product/2346/update?uid=17541)，专业技术支持。                                                                                                                          | 企业级复杂应用，需要网格、报表、图表等高级功能，且有预算。                                                        |

### 十一、图形、动画、多媒体

###### 11.1 矢量图形

- 基于矢量的图形系统
- 支持形状、几何图形、画刷和变换
- 集成2D和3D图形能力
- 内置媒体支持(图像、视频、音频)
- 硬件加速渲染
- Shape 派生类：Rectangle，Ellipse，Path，Line
- Geometry：PathGeometry，StreamGeometry
- 画刷：RotateTransform，ScaleTransform，TranslateTransform

###### 11.2 动画 - 在一段时间内连续改变对象的属性值，从而在视觉上产生运动或变化的效果

- 在WPF中，可以对任何依赖属性（Dependency Property）进行动画处理。为了进行动画处理，属性必须属于实现了*IAnimatable*接口的类
- 定义动画对象 = From 起始值 / To 结束值/By 属性/ Duration 持续时间
- 动画类型
  - 属性动画(*DoubleAnimation*用于生成*double*类型的值，*ColorAnimation*用于颜色值，*PointAnimation*用于点（*Point*）类型的值)、
  - 关键帧动画
  - 路径动画
- 故事板 Storyboard - 是一个容器，用于组织和管理一组动画。通过故事板，可以同时控制多个动画的播放。
- 将动画应用到目标属性 - 使用Storyboard.SetTarget、Storyboard.SetTargetProperty 方法将动画与目标对象的特定属性关联起来。
- 启动动画 - 调用故事板的*Begin*方法来启动动画

### 十二、WPF 性能优化

- 启用 UI 虚拟化，用于大数据列表 - 只在视口内渲染可见的元素，不是全部渲染
  
  ```xml
  <!-- 启用 UI 虚拟化 -->
  <ListBox VirtualizingStackPanel.IsVirtualizing="True"
           VirtualizingStackPanel.VirtualizationMode="Recycling"
           ScrollViewer.IsDeferredScrollingEnabled="True">
      <ListBox.ItemsPanel>
          <ItemsPanelTemplate>
              <VirtualizingStackPanel/>
          </ItemsPanelTemplate>
      </ListBox.ItemsPanel>
  </ListBox>
  ```

- 避免深度布局嵌套

- 及时注销事件处理程序

- 数据分页加载、异步加载数据

- 使用缓存
