

<p align="center">
🎉 超简化的设计模式解释！ 🎉
</p>
<p align="center">
一个容易让人头晕的话题。在这里，我尝试以<i>最简单</i>的方式来解释它们，以便让它们在你的脑海中（也许还有我的脑海中）留下印象。
</p>

***

<sub>查看我的[其他项目](http://roadmap.sh)，并在[Twitter](https://twitter.com/kamrify)上打个招呼。</sub>

<br>

|[创建型设计模式](#creational-design-patterns)|[结构型设计模式](#structural-design-patterns)|[行为型设计模式](#behavioral-design-patterns)|
|:-|:-|:-|
|[简单工厂](#-simple-factory)|[适配器](#-adapter)|[责任链](#-chain-of-responsibility)|
|[工厂方法](#-factory-method)|[桥接](#-bridge)|[命令](#-command)|
|[抽象工厂](#-abstract-factory)|[组合](#-composite)|[迭代器](#-iterator)|
|[建造者](#-builder)|[装饰器](#-decorator)|[中介者](#-mediator)|
|[原型](#-prototype)|[外观](#-facade)|[备忘录](#-memento)|
|[单例](#-singleton)|[享元](#-flyweight)|[观察者](#-observer)|
||[代理](#-proxy)|[访问者](#-visitor)|
|||[策略](#-strategy)|
|||[状态](#-state)|
|||[模板方法](#-template-method)|

<br>

介绍
=================

设计模式是解决重复问题的方案；**处理特定问题的指导原则**。它们不是可以直接插入到应用程序中的类、包或库，也不是等待魔法发生的东西。相反，它们是处理特定情况下特定问题的指导原则。

> 设计模式是解决重复问题的方案；处理特定问题的指导原则

维基百科将其描述为

> 在软件工程中，软件设计模式是针对软件设计中常见问题的通用可重用解决方案。它不是可以直接转换为源代码或机器代码的完成设计。它是描述或模板，用于解决可以在许多不同情况下使用的问题。

⚠️ 注意事项
-----------------
- 设计模式并不是解决所有问题的灵丹妙药。
- 不要强行使用它们；如果这样做，坏事就会发生。
- 请记住，设计模式是针对**问题**的解决方案，而不是**寻找**问题的解决方案；所以不要过度思考。
- 如果在正确的地方以正确的方式使用，它们可以证明是救星；否则，它们可能导致代码的可怕混乱。

> 另外请注意，下面的代码示例是用 PHP-7 编写的，但这不应该阻止你，因为概念是相同的。

设计模式类型
-----------------

* [创建型](#creational-design-patterns)
* [结构型](#structural-design-patterns)
* [行为型](#behavioral-design-patterns)

创建型设计模式
==========================

简单来说
> 创建型模式专注于如何实例化一个对象或一组相关对象。

维基百科说
> 在软件工程中，创建型设计模式是处理对象创建机制的设计模式，试图以适合情况的方式创建对象。基本的对象创建形式可能导致设计问题或增加设计复杂性。创建型设计模式通过某种方式控制对象的创建来解决这个问题。

 * [简单工厂](#-simple-factory)
 * [工厂方法](#-factory-method)
 * [抽象工厂](#-abstract-factory)
 * [建造者](#-builder)
 * [原型](#-prototype)
 * [单例](#-singleton)

🏠 简单工厂
--------------
现实世界示例
> 假设你正在建造一座房子，你需要门。你可以穿上木匠的衣服，带上木材、胶水、钉子和所有建造门所需的工具，开始在你的房子里建造门，或者你可以简单地打电话给工厂，让他们把建好的门送到你这里，这样你就不需要了解门的制作过程或处理制作过程中产生的混乱。

简单来说
> 简单工厂为客户生成一个实例，而不向客户暴露任何实例化逻辑。

维基百科说
> 在面向对象编程（OOP）中，工厂是创建其他对象的对象——正式来说，工厂是一个函数或方法，它从某个方法调用中返回不同原型或类的对象，假设为"new"。

**编程示例**

首先，我们有一个门接口及其实现
```php
interface Door
{
    public function getWidth(): float;
    public function getHeight(): float;
}

class WoodenDoor implements Door
{
    protected $width;
    protected $height;

    public function __construct(float $width, float $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function getWidth(): float
    {
        return $this->width;
    }

    public function getHeight(): float
    {
        return $this->height;
    }
}
```
然后我们有我们的门工厂，它制造门并返回它
```php
class DoorFactory
{
    public static function makeDoor($width, $height): Door
    {
        return new WoodenDoor($width, $height);
    }
}
```
然后可以这样使用
```php
// 给我做一扇100x200的门
$door = DoorFactory::makeDoor(100, 200);

echo '宽度: ' . $door->getWidth();
echo '高度: ' . $door->getHeight();

// 给我做一扇50x100的门
$door2 = DoorFactory::makeDoor(50, 100);
```

**何时使用？**

当创建一个对象不仅仅是几个赋值，并且涉及一些逻辑时，将其放入专用工厂中是有意义的，而不是在每个地方重复相同的代码。

🏭 工厂方法
--------------

现实世界示例
> 考虑一个招聘经理的案例。一个人不可能面试每个职位。根据职位空缺，她必须决定并将面试步骤委托给不同的人。

简单来说
> 它提供了一种将实例化逻辑委托给子类的方法。

维基百科说
> 在基于类的编程中，工厂方法模式是一种创建模式，它使用工厂方法来处理创建对象的问题，而不必指定将要创建的对象的确切类。这是通过调用工厂方法来创建对象实现的——该方法要么在接口中指定并由子类实现，要么在基类中实现并可选地由派生类重写——而不是通过调用构造函数。

 **编程示例**

以我们的招聘经理示例为例。首先，我们有一个面试官接口及其实现

```php
interface Interviewer
{
    public function askQuestions();
}

class Developer implements Interviewer
{
    public function askQuestions()
    {
        echo '询问设计模式相关问题！';
    }
}

class CommunityExecutive implements Interviewer
{
    public function askQuestions()
    {
        echo '询问社区建设相关问题';
    }
}
```

现在让我们创建我们的`HiringManager`

```php
abstract class HiringManager
{

    // 工厂方法
    abstract protected function makeInterviewer(): Interviewer;

    public function takeInterview()
    {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}

```
现在任何子类都可以扩展它并提供所需的面试官
```php
class DevelopmentManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new Developer();
    }
}

class MarketingManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new CommunityExecutive();
    }
}
```
然后可以这样使用

```php
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // 输出: 询问设计模式相关问题

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // 输出: 询问社区建设相关问题。
```

**何时使用？**

当类中有一些通用处理，但所需的子类在运行时动态决定时非常有用。换句话说，当客户端不知道它可能需要哪个确切的子类时。

🔨 抽象工厂
----------------

现实世界示例
> 扩展我们简单工厂的门示例。根据你的需求，你可能会从木门商店获得一扇木门，从铁门商店获得一扇铁门，或者从相关商店获得一扇PVC门。此外，你可能需要一个具有不同专业技能的人来安装门，例如木匠安装木门，焊工安装铁门等。正如你所看到的，门之间��在存在依赖关系，木门需要木匠，铁门需要焊工等。

简单来说
> 工厂的工厂；一个将单个但相关/依赖工厂组合在一起的工厂，而不指定它们的具体类。

维基百科说
> 抽象工厂模式提供了一种封装一组具有共同主题的单个工厂的方法，而不指定它们的具体类。

**编程示例**

翻译上述门的示例。首先，我们有我们的`Door`接口及其实现

```php
interface Door
{
    public function getDescription();
}

class WoodenDoor implements Door
{
    public function getDescription()
    {
        echo '我是一扇木门';
    }
}

class IronDoor implements Door
{
    public function getDescription()
    {
        echo '我是一扇铁门';
    }
}
```
然后我们有每种门类型的安装专家

```php
interface DoorFittingExpert
{
    public function getDescription();
}

class Welder implements DoorFittingExpert
{
    public function getDescription()
    {
        echo '我只能安装铁门';
    }
}

class Carpenter implements DoorFittingExpert
{
    public function getDescription()
    {
        echo '我只能安装木门';
    }
}
```

现在我们有我们的抽象工厂，它将让我们制造一系列相关对象，即木门工厂将创建一扇木门和木门安装专家，而铁门工厂将��建一扇铁门和铁门安装专家
```php
interface DoorFactory
{
    public function makeDoor(): Door;
    public function makeFittingExpert(): DoorFittingExpert;
}

// 木门工厂返回木门和木门安装工
class WoodenDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new WoodenDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Carpenter();
    }
}

// 铁门工厂获取铁门和相关的安装专家
class IronDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new IronDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Welder();
    }
}
```
然后可以这样使用
```php
$woodenFactory = new WoodenDoorFactory();

$door = $woodenFactory->makeDoor();
$expert = $woodenFactory->makeFittingExpert();

$door->getDescription();  // 输出: 我是一扇木门
$expert->getDescription(); // 输出: 我只能安装木门

// 铁门工厂同样
$ironFactory = new IronDoorFactory();

$door = $ironFactory->makeDoor();
$expert = $ironFactory->makeFittingExpert();

$door->getDescription();  // 输出: 我是一扇铁门
$expert->getDescription(); // 输出: 我只能安装铁门
```

正如你所看到的，木门工厂封装��`carpenter`和`wooden door`，而铁门工厂封装了`iron door`和`welder`。因此，它帮助我们确保为每扇创建的门，我们不会得到错误的安装专家。

**何时使用？**

当存在相互依赖的关系，并且涉及不太简单的创建逻辑时。

👷 建造者
--------------------------------------------
现实世界示例
> 想象一下你在Hardee's点了一份特定的套餐，比如"Big Hardee"，他们直接把它交给你，而不问任何问题；这就是简单工厂的例子。但有些情况下，创建逻辑可能涉及更多步骤。例如，你想要一个定制的Subway套餐，你有多个选项来制作你的汉堡，比如你想要什么面包？你想要什么类型的酱？你想要什么奶酪？等等。在这种情况下，建造者模式就派上用场了。

简单来说
> 允许你创建对象的不同风味，同时避免构造函数污染。当对象的风味可能有很多种，或者创建对象时涉及很多步骤时非常有用。

维基百科说
> 建造者模式是一种对象创建软件设计模式，旨在找到解决望远镜构造反模式的方案。

在此，我想补充一点关于望远镜构造反模式的内容。在某个时刻，我们都见过如下构造函数：

```php
public function __construct($size, $cheese = true, $pepperoni = true, $tomato = false, $lettuce = true)
{
}
```

正如你所看到的，构造函数参数的数量可能迅速增加，并且可能变得难以理解参数的排列。此外，如果你想在将来添加更多选项，这个参数列表可能会不断增长。这被称为望远镜构造反模式。

**编程示例**

理智的替代方案是使用建造者模式。首先，我们有我们想要制作的汉堡

```php
class Burger
{
    protected $size;

    protected $cheese = false;
    protected $pepperoni = false;
    protected $lettuce = false;
    protected $tomato = false;

    public function __construct(BurgerBuilder $builder)
    {
        $this->size = $builder->size;
        $this->cheese = $builder->cheese;
        $this->pepperoni = $builder->pepperoni;
        $this->lettuce = $builder->lettuce;
        $this->tomato = $builder->tomato;
    }
}
```

然后我们有建造者

```php
class BurgerBuilder
{
    public $size;

    public $cheese = false;
    public $pepperoni = false;
    public $lettuce = false;
    public $tomato = false;

    public function __construct(int $size)
    {
        $this->size = $size;
    }

    public function addPepperoni()
    {
        $this->pepperoni = true;
        return $this;
    }

    public function addLettuce()
    {
        $this->lettuce = true;
        return $this;
    }

    public function addCheese()
    {
        $this->cheese = true;
        return $this;
    }

    public function addTomato()
    {
        $this->tomato = true;
        return $this;
    }

    public function build(): Burger
    {
        return new Burger($this);
    }
}
```
然后可以这样使用：

```php
$burger = (new BurgerBuilder(14))
                    ->addPepperoni()
                    ->addLettuce()
                    ->addTomato()
                    ->build();
```

**何时使用？**

当对象的风味可能有很多种���并且为了避免构造函数的望远镜化时。与工厂模式的关键区别在于；工厂模式用于创建是一步过程，而建造者模式用于创建是多步骤过程。

🐑 原型
------------
现实世界示例
> 还记得多莉吗？那只被克隆的羊！我们不深入细节，但关键点在于它是关于克隆的。

简单来说
> 通过克隆基于现有对象创建对象。

维基百科说
> 原型模式是一种软件开发中的创建型设计模式。当要创建的对象类型由原型实例决定时，使用原型实例进行克隆以生成新对象。

简而言之，它允许你创建现有对象的副本并根据需要修改它，而不是从头开始创建对象并进行设置。

**编程示例**

在PHP中，可以使用`clone`轻松实现

```php
class Sheep
{
    protected $name;
    protected $category;

    public function __construct(string $name, string $category = '山羊')
    {
        $this->name = $name;
        $this->category = $category;
    }

    public function setName(string $name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        return $this->name;
    }

    public function setCategory(string $category)
    {
        $this->category = $category;
    }

    public function getCategory()
    {
        return $this->category;
    }
}
```
然后可以像下面这样克隆
```php
$original = new Sheep('Jolly');
echo $original->getName(); // Jolly
echo $original->getCategory(); // 山羊

// 克隆并修改所需的内容
$cloned = clone $original;
$cloned->setName('Dolly');
echo $cloned->getName(); // Dolly
echo $cloned->getCategory(); // 山羊
```

你还可以使用魔术方法`__clone`来修改克隆行为。

**何时使用？**

当需要一个与现有对象相似的对象，或者创建对象的成本相对于克隆来说很高时。

💍 单例
------------
现实世界示例
> 一个国家一次只能有一个总统。每当需要时，必须调用同一位总统。总统在这里是单例。

简单来说
> 确保某个类只有一个对象被创建。

维基百科说
> 在软件工程中，单例模式是一种软件设计模式，它限制了类的实例化为一个对象。当系统中确切需要一个对象来协调操作时，这很有用。

单例模式实际上被认为是一种反模式，应该谨慎使用。它并不一定是坏的，可能有一些有效的用例，但应该谨慎使用，因为它在应用程序中引入了全局状态，在一个地方的更改可能会影响其他区域，并且可能会变得非常难以调试。另一个坏处是它使你的代码紧密耦合，并且模拟单例可能��困难。

**编程示例**

要创建单例，请将构造函数设为私有，禁用克隆，禁用扩展，并创建一个静态变量来存放实例
```php
final class President
{
    private static $instance;

    private function __construct()
    {
        // 隐藏构造函数
    }

    public static function getInstance(): President
    {
        if (!self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    private function __clone()
    {
        // 禁用克隆
    }

    private function __wakeup()
    {
        // 禁用反序列化
    }
}
```
然后使用
```php
$president1 = President::getInstance();
$president2 = President::getInstance();

var_dump($president1 === $president2); // true
```

结构型设计模式
==========================
简单来说
> 结构型模式主要关注对象组合，或者换句话说，实体如何相互使用。或者换句话说，它们帮助回答"如何构建软件组件？"

维基百科说
> 在软件工程中，结构型设计模式是通过识别实现实体之间关系的简单方法来简化设计的设计模式。

 * [适配器](#-adapter)
 * [桥接](#-bridge)
 * [组合](#-composite)
 * [装饰器](#-decorator)
 * [外观](#-facade)
 * [享元](#-flyweight)
 * [代理](#-proxy)

🔌 适配器
-------
现实世界示例
> 假设你有一些照片在你的内存卡上，你需要将它们传输到你的计算机。为了传输它们，你需要某种适配器，使其与计算机端口兼容，以便你可以将内存卡连接到计算机。在这种情况下，读卡器就是适配器。
> 另一个例子是著名的电源适配器；三脚插头无法连接到两个插孔的插座，它需要使用电源适配器使其与两个插孔的插座兼容。
> 还有一个例子是翻译者将一个人说的话翻译成另一个人说的话。

简单来说
> 适配器模式允许你将一个不兼容的对象包装在适配器中，使其与另一个类兼容。

维基百科说
> 在软件工程中，适配器模式是一种软件设计模式，它允许现有类的接口作为另一种接口使用。它通常用于使现有类与其他类一起工作，而不修改其源代码。

**编程示例**

考虑一个游戏，其中有一个猎人，他猎杀狮子。

首先，我们有一个`Lion`接口，所有类型的狮子都必须实现

```php
interface Lion
{
    public function roar();
}

class AfricanLion implements Lion
{
    public function roar()
    {
    }
}

class AsianLion implements Lion
{
    public function roar()
    {
    }
}
```
猎人期望任何实现`Lion`接口的对象都可以被猎杀。
```php
class Hunter
{
    public function hunt(Lion $lion)
    {
        $lion->roar();
    }
}
```

现在假设我们必须在游戏中添加一个`WildDog`，以便猎人也可以猎杀它。但我们不能直接这样做，因为狗有不同的接口。为了使其与我们的猎人兼容，我们将必须创建一个适配器，使其兼容

```php
// 这需要添加到游戏中
class WildDog
{
    public function bark()
    {
    }
}

// 针对野狗的适配器，使其与我们的游戏兼容
class WildDogAdapter implements Lion
{
    protected $dog;

    public function __construct(WildDog $dog)
    {
        $this->dog = $dog;
    }

    public function roar()
    {
        $this->dog->bark();
    }
}
```
现在`WildDog`可以通过`WildDogAdapter`在我们的游戏中使用。

```php
$wildDog = new WildDog();
$wildDogAdapter = new WildDogAdapter($wildDog);

$hunter = new Hunter();
$hunter->hunt($wildDogAdapter);
```

🚡 桥接
------
现实世界示例
> 假设你有一个网站，有不同的页面，你需要允许用户更改主题。你会怎么做？为每个主题创建每个页面的多个副本，还是仅仅创建单独的主题并根据用户的偏好加载它们？桥接模式允许你这样做。

![有和没有桥接模式的对比](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

简单来说
> 桥接模式是关于优先使用组合而不是继承。实现细节从一个层次结构推送到另一个具有单独层次结构的对象。

维基百科说
> 桥接模式是一种软件工程设计模式，旨在"将抽象与其实现解耦，以便两者可以独立变化"。

**编程示例**

翻译我们的网页示例。这里我们有`WebPage`层次结构

```php
interface WebPage
{
    public function __construct(Theme $theme);
    public function getContent();
}

class About implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "关于页面的颜色是 " . $this->theme->getColor();
    }
}

class Careers implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "职业页面的颜色是 " . $this->theme->getColor();
    }
}
```
以及单独的主题层次结构
```php

interface Theme
{
    public function getColor();
}

class DarkTheme implements Theme
{
    public function getColor()
    {
        return '深黑色';
    }
}
class LightTheme implements Theme
{
    public function getColor()
    {
        return '淡白色';
    }
}
class AquaTheme implements Theme
{
    public function getColor()
    {
        return '浅蓝色';
    }
}
```
然后是两个层次结构
```php
$darkTheme = new DarkTheme();

$about = new About($darkTheme);
$careers = new Careers($darkTheme);

echo $about->getContent(); // "关于页面的颜色是 深黑色";
echo $careers->getContent(); // "职业页面的颜色是 深黑色";
```

🌿 组合
-----------------

现实世界示例
> 每个组织由员工组成。每个员工都有相同的特征，即有薪水，有一些责任，可能会向某人报告，可能会有一些下属等。

简单来说
> 组合模式允许客户端以统一的方式对待单个对象。

维基百科说
> 在软件工程中，组合模式是一种分区设计模式。组合模式描述了一组对象应以与单个对象实例相同的方式进行处理。实现组合模式使客户端能够以统一的方式对待单个对象和组合。

**编程示例**

以我们的员工示例为例。这里我们有不同类型的员工

```php
interface Employee
{
    public function __construct(string $name, float $salary);
    public function getName(): string;
    public function setSalary(float $salary);
    public function getSalary(): float;
    public function getRoles(): array;
}

class Developer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;
    
    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}

class Designer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;

    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}
```

然后我们有一个组织，其中包含几种不同类型的员工

```php
class Organization
{
    protected $employees;

    public function addEmployee(Employee $employee)
    {
        $this->employees[] = $employee;
    }

    public function getNetSalaries(): float
    {
        $netSalary = 0;

        foreach ($this->employees as $employee) {
            $netSalary += $employee->getSalary();
        }

        return $netSalary;
    }
}
```

然后可以这样使用

```php
// 准备员工
$john = new Developer('John Doe', 12000);
$jane = new Designer('Jane Doe', 15000);

// 将他们添加到组织中
$organization = new Organization();
$organization->addEmployee($john);
$organization->addEmployee($jane);

echo "净薪水: " . $organization->getNetSalaries(); // 净薪水: 27000
```

☕ 装饰器
-------------

现实世界示例

> 想象一下你经营一家汽车服务店，提供多种服务。现在你如何计算要收取的费用？你选择一项服务，并动态地将提供的服务的价格加到一起，直到你得到最终费用。这里每种类型的服务都是一个装饰器。

简单来说
> 装饰器模式允许你在运行时通过将对象包装在装饰器类的对象中动态地改变对象的行为。

维基百科说
> 在面向对象编程中，装饰器模式是一种设计模式，允许向单个对象添加行为，无论是静态的还是动态的，而不影响同一类的其他对象的行为。装饰器模式通常有助于遵循单一职责原则，因为它允许将功能分配给具有独特关注领域的类。

**编程示例**

以咖啡为例。首先，我们有一个简单的咖啡实现咖啡接口

```php
interface Coffee
{
    public function getCost();
    public function getDescription();
}

class SimpleCoffee implements Coffee
{
    public function getCost()
    {
        return 10;
    }

    public function getDescription()
    {
        return '简单咖啡';
    }
}
```
我们希望使代码可扩展，以允许在需要时修改它。让我们制作一些附加选项（装饰器）
```php
class MilkCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 2;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', 牛奶';
    }
}

class WhipCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 5;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', 奶油';
    }
}

class VanillaCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 3;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', 香草';
    }
}
```

现在让我们制作一杯咖啡

```php
$someCoffee = new SimpleCoffee();
echo $someCoffee->getCost(); // 10
echo $someCoffee->getDescription(); // 简单咖啡

$someCoffee = new MilkCoffee($someCoffee);
echo $someCoffee->getCost(); // 12
echo $someCoffee->getDescription(); // 简单咖啡, 牛奶

$someCoffee = new WhipCoffee($someCoffee);
echo $someCoffee->getCost(); // 17
echo $someCoffee->getDescription(); // 简单咖啡, 牛奶, 奶油

$someCoffee = new VanillaCoffee($someCoffee);
echo $someCoffee->getCost(); // 20
echo $someCoffee->getDescription(); // 简单咖啡, 牛奶, 奶油, 香草
```

📦 外观
----------------

现实世界示例
> 你如何打开计算机？"按下电源按钮"你说！这就是你所认为的，因为你使用的是计算机提供的简单接口，内部需要做很多事情才能实现。这种对复杂子系统的简单接口就是外观。

简单来说
> 外观模式为复杂子系统提供了简化的接口。

维基百科说
> 外观是提供简化接口的大型代码库（如类库）的对象。

**编程示例**

以我们的计算机示例为例。这里我们有计算机类

```php
class Computer
{
    public function getElectricShock()
    {
        echo "哎呀！";
    }

    public function makeSound()
    {
        echo "哔哔声！";
    }

    public function showLoadingScreen()
    {
        echo "加载中..";
    }

    public function bam()
    {
        echo "准备好使用了！";
    }

    public function closeEverything()
    {
        echo "嗡嗡嗡！";
    }

    public function sooth()
    {
        echo "呼呼";
    }

    public function pullCurrent()
    {
        echo "哈啊！";
    }
}
```
这里是外观
```php
class ComputerFacade
{
    protected $computer;

    public function __construct(Computer $computer)
    {
        $this->computer = $computer;
    }

    public function turnOn()
    {
        $this->computer->getElectricShock();
        $this->computer->makeSound();
        $this->computer->showLoadingScreen();
        $this->computer->bam();
    }

    public function turnOff()
    {
        $this->computer->closeEverything();
        $this->computer->pullCurrent();
        $this->computer->sooth();
    }
}
```
现在使用外观
```php
$computer = new ComputerFacade(new Computer());
$computer->turnOn(); // 哎呀！ 哔哔声！ 加载中.. 准备好使用了！
$computer->turnOff(); // 嗡嗡嗡！ 哈啊！ 呼呼
```

🍃 享元
---------

现实世界示例
> 你是否曾在某个摊位喝过新鲜茶？他们通常会制作比你要求的更多的茶，并为其他顾客保留剩下的茶，以节省资源，例如燃气等。享元模式就是关于这一点，即共享。

简单来说
> 它用于通过与相似对象共享尽可能多的数据来最小化内存使用或计算开销。

维基百科说
> 在计算机编程中，享元是一种软件设计模式。享元是通过与其他相似对象共享尽可能多的数据来最小化内存使用的对象；它是一种在大量使用简单重复表示时使用对象的方式，当简单重复表示会使用不可接受的内存量时。

**编程示例**

翻译我们的茶示例。首先我们有茶的类型和茶的制造者

```php
// 任何将被缓存的都是享元。
// 茶的类型将是享元。
class KarakTea
{
}

// 作为工厂并保存茶
class TeaMaker
{
    protected $availableTea = [];

    public function make($preference)
    {
        if (empty($this->availableTea[$preference])) {
            $this->availableTea[$preference] = new KarakTea();
        }

        return $this->availableTea[$preference];
    }
}
```

然后我们有`TeaShop`，它接受订单并提供服务

```php
class TeaShop
{
    protected $orders;
    protected $teaMaker;

    public function __construct(TeaMaker $teaMaker)
    {
        $this->teaMaker = $teaMaker;
    }

    public function takeOrder(string $teaType, int $table)
    {
        $this->orders[$table] = $this->teaMaker->make($teaType);
    }

    public function serve()
    {
        foreach ($this->orders as $table => $tea) {
            echo "为桌子# " . $table . " 提供茶";
        }
    }
}
```
然后可以这样使用

```php
$teaMaker = new TeaMaker();
$shop = new TeaShop($teaMaker);

$shop->takeOrder('少糖', 1);
$shop->takeOrder('多牛奶', 2);
$shop->takeOrder('不加糖', 5);

$shop->serve();
// 为桌子# 1 提供茶
// 为桌子# 2 提供茶
// 为桌子# 5 提供茶
```

🎱 代理
-------------------
现实世界示例
> 你是否曾使用门禁卡通过一扇门？有多种打开该门的选项，即可以使用门禁卡打开，也可以按下一个按钮绕过安全措施。门的主要功能是打开，但在其上添加了��个代理以添加一些功能。让我通过下面的代码示例更好地解释一下。

简单来说
> 使用代理模式，一个类代表另一个类的功能。

维基百科说
> 代理，通常是一个类，作为访问其他对象的接口。代理是一个包装器或代理对象，客户端通过它访问幕后真实服务对象。代理的使用可以简单地转发到真实对象，或者可以提供附加逻辑。例如，当对真实对象的操作资源密集时，可以提供缓存，或者在调用真实对象的操作之前检查前提条件。

**编程示例**

以我们的安全门示例为例。首先我们有门接口及其实现

```php
interface Door
{
    public function open();
    public function close();
}

class LabDoor implements Door
{
    public function open()
    {
        echo "打开实验室门";
    }

    public function close()
    {
        echo "关闭实验室门";
    }
}
```
然后我们有一个代理来保护我们想要的任何门
```php
class SecuredDoor implements Door
{
    protected $door;

    public function __construct(Door $door)
    {
        $this->door = $door;
    }

    public function open($password)
    {
        if ($this->authenticate($password)) {
            $this->door->open();
        } else {
            echo "不行！不可能���";
        }
    }

    public function authenticate($password)
    {
        return $password === '$ecr@t';
    }

    public function close()
    {
        $this->door->close();
    }
}
```
然后可以这样使用
```php
$door = new SecuredDoor(new LabDoor());
$door->open('无效'); // 不行！不可能。

$door->open('$ecr@t'); // 打开实验室门
$door->close(); // 关闭实验室门
```
另一个例子可能是某种数据映射器实现。例如，我最近使用此模式为MongoDB制作了一个ODM（对象数据映射器），我在mongo类周围写了一个代理，同时利用魔术方法`__call()`。所有方法调用都被代理到原始mongo类，结果以原样返回，但在`find`或`findOne`的情况下，数据被映射到所需的类对象，并返回对象而不是`Cursor`。

行为型设计模式
==========================

简单来说
> 它关注对象之间的责任分配。与结构型模式不同的是，它们不仅指定结构，还概述了它们之间的消息传递/通信模式。换句话说，它们帮助回答"如何在软件组件中运行行为？"

维基百科说
> 在软件工程中，行为型设计模式是识别对象之间常见通信模式并实现这些模式的设计模式。通过这样做，这些模式增加了在执行此通信时的灵活性。

* [责任链](#-chain-of-responsibility)
* [命令](#-command)
* [迭代器](#-iterator)
* [中介者](#-mediator)
* [备忘录](#-memento)
* [观察者](#-observer)
* [访问者](#-visitor)
* [策略](#-strategy)
* [状态](#-state)
* [模板方法](#-template-method)

🔗 责任链
-----------------------

现实世界示例
> 例如，你在账户中设置了三种支付方式（`A`、`B`和`C`）；每种支付方式的金额不同。`A`有100美元，`B`有300美元，`C`有1000美元，支付优先级选择为`A`，然后是`B`，最后是`C`。你尝试购买价值210美元的东西。使用责任链，首先检查账户`A`是否可以进行购买，如果可以，则进行购买并中断链。如果不行，请继续检查账户`B`的金额，如果可以，则中断链，否则请求将继续向前传递，直到找到合适的处理程序。在这里，`A`、`B`和`C`是链的链接，整个现象就是责任链。

简单来说
> 它帮助构建一系列对象。请求从一端进入，并继续在对象之间传递，直到找到合适的处理程序。

维基百科说
> 在面向对象设计中，责任链模式是一种设计模式，由命令对象的源和一系列处理��象组成。每个处理对象包含定义其可以处理的命令对象类型的逻辑；其余的则传递给链中的下一个处理对象。

**编程示例**

翻译我们的账户示例。首先，我们有一个基本账户，具有将账户链接在一起的逻辑和一些账户

```php
abstract class Account
{
    protected $successor;
    protected $balance;

    public function setNext(Account $account)
    {
        $this->successor = $account;
    }

    public function pay(float $amountToPay)
    {
        if ($this->canPay($amountToPay)) {
            echo sprintf('使用 %s 支付 %s' . PHP_EOL, get_called_class(), $amountToPay);
        } elseif ($this->successor) {
            echo sprintf('无法使用 %s 支付。继续 ..' . PHP_EOL, get_called_class());
            $this->successor->pay($amountToPay);
        } else {
            throw new Exception('没有任何账户有足够的余额');
        }
    }

    public function canPay($amount): bool
    {
        return $this->balance >= $amount;
    }
}

class Bank extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Paypal extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Bitcoin extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}
```

现在让我们准备链条，使用上面定义的链接（即银行、Paypal、比特币）

```php
// 让我们准备一个链条如下
//      $bank->$paypal->$bitcoin
//
// 首先优先银行
//      如果银行无法支付，则使用paypal
//      如果paypal无法支付，则使用比特币

$bank = new Bank(100);          // 银行余额100
$paypal = new Paypal(200);      // Paypal余额200
$bitcoin = new Bitcoin(300);    // 比特币余额300

$bank->setNext($paypal);
$paypal->setNext($bitcoin);

// 让我们尝试使用首选项，即银行支付
$bank->pay(259);

// 输出将是
// ==============
// 无法使用银行支付。继续 ..
// 无法使用paypal支付。继续 ..:
// 使用比特币支付259美元！
```

👮 命令
-------

现实世界示例
> 一个通用的例子是你在餐厅点餐。你（即`客户端`）要求服务员（即`调用者`）带来一些食物（即`命令`），服务员只是将请求转发给厨师（即`接收者`），厨师知道做什么和怎么做。
> 另一个例子是你（即`客户端`）使用遥控器（`调用者`）打开电视（即`接收者`）。

简单来说
> 允许你将操作封装在对象中。该模式的关键思想是提供手段以解耦客户端与接收者。

维基百科说
> 在面向对象编程中，命令模式是一种行为设计模式，其中一个对象用于封装执行某个操作或触发事件所需的所有信息。该信息包括方法名称、拥有该方法的对象和方法参数的值。

**编程示例**

首先，我们有接收者，具有可以执行的每个操作的实现
```php
// 接收者
class Bulb
{
    public function turnOn()
    {
        echo "灯泡已点亮";
    }

    public function turnOff()
    {
        echo "黑暗！";
    }
}
```
然后我们有一个接口，每个命令都将实现，然后我们有一组命令
```php
interface Command
{
    public function execute();
    public function undo();
    public function redo();
}

// 命令
class TurnOn implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOn();
    }

    public function undo()
    {
        $this->bulb->turnOff();
    }

    public function redo()
    {
        $this->execute();
    }
}

class TurnOff implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOff();
    }

    public function undo()
    {
        $this->bulb->turnOn();
    }

    public function redo()
    {
        $this->execute();
    }
}
```
然后我们有一个`调用者`，客户端将与之交互以处理任何命令
```php
// 调用者
class RemoteControl
{
    public function submit(Command $command)
    {
        $command->execute();
    }
}
```
最后让我们看看如何在客户端中使用它
```php
$bulb = new Bulb();

$turnOn = new TurnOn($bulb);
$turnOff = new TurnOff($bulb);

$remote = new RemoteControl();
$remote->submit($turnOn); // 灯泡已点亮！
$remote->submit($turnOff); // 黑暗！
```

命令模式还可以用于实现基于事务的系统。在你执行命令时，保持命令历史记录。如果最终命令成功执行，一切正常，否则只需遍历历史记录并对所有已执行的命令执行`undo`。

➿ 迭代器
--------

现实世界示例
> 一台老式收音机就是一个很好的迭代器示例，用户可以从某个频道开始，然后使用下一步或上一步按钮浏览相应的频道。或者以MP3播放器或电视机为例，你可以按下下一步和上一步按钮浏览连续的频道、歌曲或电台。

简单来说
> 它提��了一种访问对象元素的方法，而不暴露底层表示。

维基百科说
> 在面向对象编程中，迭代器模式是一种设计模式，其中使用迭代器遍历容器并访问容器的元素。迭代器模式将算法与容器解耦；在某些情况下，算法必然是特定于容器的，因此无法解耦。

**编程示例**

在PHP中，使用SPL（标准PHP库）实现非常简单。翻译我们的广播电台示例。首先我们有`RadioStation`

```php
class RadioStation
{
    protected $frequency;

    public function __construct(float $frequency)
    {
        $this->frequency = $frequency;
    }

    public function getFrequency(): float
    {
        return $this->frequency;
    }
}
```
然后我们有我们的迭代器

```php
use Countable;
use Iterator;

class StationList implements Countable, Iterator
{
    /** @var RadioStation[] $stations */
    protected $stations = [];

    /** @var int $counter */
    protected $counter;

    public function addStation(RadioStation $station)
    {
        $this->stations[] = $station;
    }

    public function removeStation(RadioStation $toRemove)
    {
        $toRemoveFrequency = $toRemove->getFrequency();
        $this->stations = array_filter($this->stations, function (RadioStation $station) use ($toRemoveFrequency) {
            return $station->getFrequency() !== $toRemoveFrequency;
        });
    }

    public function count(): int
    {
        return count($this->stations);
    }

    public function current(): RadioStation
    {
        return $this->stations[$this->counter];
    }

    public function key()
    {
        return $this->counter;
    }

    public function next()
    {
        $this->counter++;
    }

    public function rewind()
    {
        $this->counter = 0;
    }

    public function valid(): bool
    {
        return isset($this->stations[$this->counter]);
    }
}
```
然后可以这样使用

```php
$stationList = new StationList();

$stationList->addStation(new RadioStation(89));
$stationList->addStation(new RadioStation(101));
$stationList->addStation(new RadioStation(102));
$stationList->addStation(new RadioStation(103.2));

foreach($stationList as $station) {
    echo $station->getFrequency() . PHP_EOL;
}

$stationList->removeStation(new RadioStation(89)); // 将移除89频道
```

👽 中介者
========

现实世界示例
> 一个通用的例子是，当你在手机上与某人交谈时，有一个网络提供商在你和他们之间，您的对话通过它进行，而不是直接发送。在这种情况下，网络提供商就是中介。

简单来说
> 中介者模式添加了一个第三方对象（称为中介）来控制两个对象（称为同事）之间的交互。它有助于减少相互通信的类之间的耦合。因为现在它们不需要了解彼此的实现。

维基百科说
> 在软件工程中，中介者模式定义了一个封装一组对象如何交互的对象。由于它可以改变程序的运行行为，因此该模式被认为是一种行为模式。

**编程示例**

这里是一个简单的聊天房间（即中介）与用户（即同事）之间发送消息的示例。

首先，我们有中介，即聊天房间

```php
interface ChatRoomMediator 
{
    public function showMessage(User $user, string $message);
}

// 中介
class ChatRoom implements ChatRoomMediator
{
    public function showMessage(User $user, string $message)
    {
        $time = date('M d, y H:i');
        $sender = $user->getName();

        echo $time . '[' . $sender . ']:' . $message;
    }
}
```

然后我们有我们的用户，即同事
```php
class User {
    protected $name;
    protected $chatMediator;

    public function __construct(string $name, ChatRoomMediator $chatMediator) {
        $this->name = $name;
        $this->chatMediator = $chatMediator;
    }

    public function getName() {
        return $this->name;
    }

    public function send($message) {
        $this->chatMediator->showMessage($this, $message);
    }
}
```
然后是使用
```php
$mediator = new ChatRoom();

$john = new User('John Doe', $mediator);
$jane = new User('Jane Doe', $mediator);

$john->send('你好！');
$jane->send('嘿！');

// 输出将是
// 2月14日，10:58 [John]: 你好！
// 2月14日，10:58 [Jane]: 嘿！
```

💾 备忘录
-------
现实世界示例
> 以计算器（即发起者）为例，每当你进行某些计算时，最后的计算结果会保存在内存中（即备忘录），以便你可以返回并通过某些操作按钮（即看护者）恢复它。

简单来说
> 备忘录模式是关于捕获和存储对象的当前状态，以便可以在稍后平滑地恢复。

维基百科说
> 备忘录模式是一种软件设计模式，提供将对象恢复到其先前状态的能力（通过回滚撤消）。

通常在你需要提供某种撤消功能时非常有用。

**编程示例**

让我们以文本编辑器为例，它会定期保存状态，以便在需要时可以恢复。

首先，我们有我们的备忘录对象，它将能够保存编辑器状态

```php
class EditorMemento
{
    protected $content;

    public function __construct(string $content)
    {
        $this->content = $content;
    }

    public function getContent()
    {
        return $this->content;
    }
}
```

然后我们有我们的编辑器，即发起者，将使用备忘录对象

```php
class Editor
{
    protected $content = '';

    public function type(string $words)
    {
        $this->content = $this->content . ' ' . $words;
    }

    public function getContent()
    {
        return $this->content;
    }

    public function save()
    {
        return new EditorMemento($this->content);
    }

    public function restore(EditorMemento $memento)
    {
        $this->content = $memento->getContent();
    }
}
```

然后可以这样使用

```php
$editor = new Editor();

// 输入一些内容
$editor->type('这是第一句。');
$editor->type('这是第二句。');

// 保存状态以恢复：这是第一句。 这是第二句。
$saved = $editor->save();

// 输入更多内容
$editor->type('这是第三句。');

// 输出：保存前的内容
echo $editor->getContent(); // 这是第一句。 这是第二句。 这是第三句。

// 恢复到最后保存的状态
$editor->restore($saved);

$editor->getContent(); // 这是第一句。 这是第二句。
```

😎 观察者
--------
现实世界示例
> 一个很好的例子是求职者，他们订阅某个招聘网站，并在有匹配的工作机会时收到通知。

简单来说
> 定义对象之间的依赖关系，以便每当对象更改其状态时，所有依赖于它的对象都会收到通知。

维基百科说
> 观察者模式是一种软件设计模式，其中一个对象称为主题，维护其依赖对象（称为观察者）的列表，并在状态更改时自动通知它们，通常通过调用它们的方法。

**编程示例**

翻译我们上面的例子。首先我们有需要被通知的求职者
```php
class JobPost
{
    protected $title;

    public function __construct(string $title)
    {
        $this->title = $title;
    }

    public function getTitle()
    {
        return $this->title;
    }
}

class JobSeeker implements Observer
{
    protected $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function onJobPosted(JobPost $job)
    {
        // 处理工作发布
        echo '嗨 ' . $this->name . '! 新工作发布: '. $job->getTitle();
    }
}
```
然后我们有我们的工作发布，求职者将订阅它
```php
class EmploymentAgency implements Observable
{
    protected $observers = [];

    protected function notify(JobPost $jobPosting)
    {
        foreach ($this->observers as $observer) {
            $observer->onJobPosted($jobPosting);
        }
    }

    public function attach(Observer $observer)
    {
        $this->observers[] = $observer;
    }

    public function addJob(JobPost $jobPosting)
    {
        $this->notify($jobPosting);
    }
}
```
然后可以这样使用

```php
// 创建订阅者
$johnDoe = new JobSeeker('John Doe');
$janeDoe = new JobSeeker('Jane Doe');

// 创建发布者并附加订阅者
$jobPostings = new EmploymentAgency();
$jobPostings->attach($johnDoe);
$jobPostings->attach($janeDoe);

// 添加新工作，看看订阅者是否收到通知
$jobPostings->addJob(new JobPost('软��工程师'));

// 输出
// 嗨 John Doe! 新工作发布: 软件工程师
// 嗨 Jane Doe! 新工作发布: 软件工程师
```

🏃 访问者
-------
现实世界示例
> 考虑一个人访问迪拜。他们只需要一种方式（即签证）进入迪拜。到达后，他们可以自己访问迪拜的任何地方，而无需询问许可或进行任何繁琐的工作；只需告诉他们一个地方，他们就可以访问。访问者模式让你做到这一点，它帮助你添加访问的地方，以便他们可以尽可能多地访问，而无需进行任何繁琐的工作。

简单来说
> 访问者模式允许你向对象添加进一步的操作，而无需修改它们。

维基百科说
> 在面向对象编程和软件工程中，访问者设计模式是一种将算法与其操作的对象结构分离的方法。该分离的一个实际结果是能够向现有对象结构添加新操作，而无需修改这些结构。这是遵循开放/封闭原则的一种方式。

**编程示例**

让我们以动物园模拟为例，我们有几种不同的动物，我们必须让它们发出声音。让我们使用访问者模式翻译这个

```php
// 被访问者
interface Animal
{
    public function accept(AnimalOperation $operation);
}

// 访问者
interface AnimalOperation
{
    public function visitMonkey(Monkey $monkey);
    public function visitLion(Lion $lion);
    public function visitDolphin(Dolphin $dolphin);
}
```
然后我们有动物的实现
```php
class Monkey implements Animal
{
    public function shout()
    {
        echo '哦哦啊啊！';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitMonkey($this);
    }
}

class Lion implements Animal
{
    public function roar()
    {
        echo '吼！';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitLion($this);
    }
}

class Dolphin implements Animal
{
    public function speak()
    {
        echo '嘟嘟嘟！';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitDolphin($this);
    }
}
```
让我们实现我们的访问者
```php
class Speak implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        $monkey->shout();
    }

    public function visitLion(Lion $lion)
    {
        $lion->roar();
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        $dolphin->speak();
    }
}
```

然后可以这样使用
```php
$monkey = new Monkey();
$lion = new Lion();
$dolphin = new Dolphin();

$speak = new Speak();

$monkey->accept($speak);    // 哦哦啊啊！    
$lion->accept($speak);      // 吼！
$dolphin->accept($speak);   // 嘟嘟嘟！
```
我们本可以通过为动物创建继承层次结构来简单地做到这一点，但那样我们必须在每次添加新操作时修改动物。但现在我们不必这样做。例如，假设我们被要求为动物添加跳跃行为，我们可以通过创建一个新的访问者来简单地添加它，即。

```php
class Jump implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        echo '跳了20英尺高！跳到树上！';
    }

    public function visitLion(Lion $lion)
    {
        echo '跳了7英尺！回到地面！';
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        echo '在水面上走了一会儿，然后消失了';
    }
}
```
然后使用
```php
$jump = new Jump();

$monkey->accept($speak);   // 哦哦啊啊！
$monkey->accept($jump);    // 跳了20英尺高！跳到树上！

$lion->accept($speak);     // 吼！
$lion->accept($jump);      // 跳了7英尺！回到地面！

$dolphin->accept($speak);  // 嘟嘟嘟！
$dolphin->accept($jump);   // 在水面上走了一会儿，然后消失了
```

💡 策略
--------

现实世界示例
> 考虑排序的例子，我们实现了冒泡排序，但数据开始增长，冒泡排序变得非常慢。为了应对这一点，我们实现了快速排序。但现在，尽管快速排序算法在大型数据集上表现更好，但在较小的数据集上却非常慢。为了处理这个问题，我们实现了一种策略，对于小型数据集，将使用冒泡排序，对于大型数据集，将使用快速排序。

简单来说
> 策略模式允许你根据情况切换算法或策略。

维基百科说
> 在计算机编程中，策略模式（也称为策略模式）是一种行为软件设计模式，允许在运行时选择算法的行为。

**编程示例**

翻译我们的示例。首先，我们有我们的策略接口和不同的策略实现

```php
interface SortStrategy
{
    public function sort(array $dataset): array;
}

class BubbleSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "使用冒泡排序进行排序";

        // 执行排序
        return $dataset;
    }
}

class QuickSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "使用快速排序进行排序";

        // 执行排序
        return $dataset;
    }
}
```

然后我们有我们的客户端，将使用任何策略
```php
class Sorter
{
    protected $sorterSmall;
    protected $sorterBig;

    public function __construct(SortStrategy $sorterSmall, SortStrategy $sorterBig)
    {
        $this->sorterSmall = $sorterSmall;
        $this->sorterBig = $sorterBig;
    }

    public function sort(array $dataset): array
    {
        if (count($dataset) > 5) {
            return $this->sorterBig->sort($dataset);
        } else {
            return $this->sorterSmall->sort($dataset);
        }
    }
}
```
然后可以这样使用
```php
$smalldataset = [1, 3, 4, 2];
$bigdataset = [1, 4, 3, 2, 8, 10, 5, 6, 9, 7];

$sorter = new Sorter(new BubbleSortStrategy(), new QuickSortStrategy());

$sorter->sort($dataset); // 输出 : 使用冒泡排序进行排序

$sorter->sort($bigdataset); // 输出 : 使用快速排序进行排序
```

💢 状态
-----
现实世界示例
> 想象一下你在使用某个绘图应用程序，你选择画笔进行绘图。现在画笔根据所选颜色改变其行为；如果你选择了红色，它将用红色绘制，如果选择了蓝色，则用蓝色绘制等。

简单来说
> 它让你在状态变化时改变类的行为。

维基百科说
> 状态模式是一种行为软件设计模式，以面向对象的方式实现状态机。使用状态模式，通过将每个单独的状态实现为状态模式接口的派生类，并通过调用模式超类中定义的方法来实现状态转换。

**编程示例**

让我们以电话为例。首先，我们有状态接口和一些状态实现

```php
interface PhoneState {
    public function pickUp(): PhoneState;
    public function hangUp(): PhoneState;
    public function dial(): PhoneState;
}

// 状态实现
class PhoneStateIdle implements PhoneState {
    public function pickUp(): PhoneState {
        return new PhoneStatePickedUp();
    }
    public function hangUp(): PhoneState {
        throw new Exception("已经空闲");
    }
    public function dial(): PhoneState {
        throw new Exception("在空闲状态下无法拨号");
    }
}

class PhoneStatePickedUp implements PhoneState {
    public function pickUp(): PhoneState {
        throw new Exception("已经拿起");
    }
    public function hangUp(): PhoneState {
        return new PhoneStateIdle();
    }
    public function dial(): PhoneState {
        return new PhoneStateCalling();
    }
}

class PhoneStateCalling implements PhoneState {
    public function pickUp(): PhoneState {
        throw new Exception("已经拿起");
    }
    public function hangUp(): PhoneState {
        return new PhoneStateIdle();
    }
    public function dial(): PhoneState {
        throw new Exception("已经拨号");
    }
}
```

然后我们有我们的电话类，根据不同的行为调用不同的方法来改变状态

```php
class Phone {
    private $state;

    public function __construct() {
        $this->state = new PhoneStateIdle();
    }
    public function pickUp() {
        $this->state = $this->state->pickUp();
    }
    public function hangUp() {
        $this->state = $this->state->hangUp();
    }
    public function dial() {
        $this->state = $this->state->dial();
    }
}
```

然后可以这样使用，它将调用相关的状态方法：

```php
$phone = new Phone();

$phone->pickUp();
$phone->dial();
```

📒 模板方法
---------------

现实世界示例
> 假设我们正在建造一座房子。建造的步骤可能如下：
> - 准备房子的基础
> - 建造墙壁
> - 添加屋顶
> - 添加其他楼层

> 这些步骤的顺序是不能改变的；例如，你不能在建造墙壁之前建造屋顶，但每个步骤都可以修改，例如墙壁可以用木材、聚酯或石头建造。

简单来说
> 模板方法定义了如何执行某个算法的骨架，但将这些步骤的实现推迟到子类。

维基百科说
> 在软件工程中，模板方法模式定义了操作的程序骨架，将某些步骤推迟到子类。它允许在不改变算法结构的情况下重新定义某些步骤。

**编程示例**

想象一下我们有一个构建工具，帮助我们测��、检查、构建、生成构建报告（即代码覆盖报告、检查报告等）并在测试服务器上部署我们的应用程序。

首先，我们有我们的基类，指定构建算法的骨架
```php
abstract class Builder
{

    // 模板方法
    final public function build()
    {
        $this->test();
        $this->lint();
        $this->assemble();
        $this->deploy();
    }

    abstract public function test();
    abstract public function lint();
    abstract public function assemble();
    abstract public function deploy();
}
```

然后我们可以有我们的实现

```php
class AndroidBuilder extends Builder
{
    public function test()
    {
        echo '运行安卓测试';
    }

    public function lint()
    {
        echo '检查安卓代码';
    }

    public function assemble()
    {
        echo '组装安卓构建';
    }

    public function deploy()
    {
        echo '将安卓构建部署到服务器';
    }
}

class IosBuilder extends Builder
{
    public function test()
    {
        echo '运行iOS测试';
    }

    public function lint()
    {
        echo '检查iOS代码';
    }

    public function assemble()
    {
        echo '组装iOS构建';
    }

    public function deploy()
    {
        echo '将iOS构建部署到服务器';
    }
}
```
然后可以这样使用

```php
$androidBuilder = new AndroidBuilder();
$androidBuilder->build();

// 输出：
// 运行安卓测试
// 检查安卓代码
// 组装安卓构建
// 将安卓构建部署到服务器

$iosBuilder = new IosBuilder();
$iosBuilder->build();

// 输出：
// 运行iOS测试
// 检查iOS代码
// 组装iOS构建
// 将iOS构建部署到服务器
```

## 🚦 总结

这就是全部。我将继续改进这个内容，因此你可能想要关注/星标这个存储库以便重新访问。此外，我计划写关于架构模式的内容，敬请期待。

## 👬 贡献

- 报告问题
- 提交改进的拉取请求
- 传播这个消息
- 随时与我联系，提供反馈 [![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/kamrify.svg?style=social&label=关注%20%40kamrify)](https://twitter.com/kamrify)

## 许可证

[![许可证: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/) 