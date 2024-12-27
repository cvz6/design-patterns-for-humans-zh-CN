

<p align="center">
🎉 超簡化的設計模式解釋！ 🎉
</p>
<p align="center">
一個容易讓人頭暈的話題。在這裡，我嘗試以<i>最簡單</i>的方式來解釋它們，以便讓它們在你的腦海中（也許還有我的腦海中）留下印象。
</p>

***

<sub>查看我的[其他項目](http://roadmap.sh)，並在[Twitter](https://twitter.com/kamrify)上打個招呼。</sub>

<br>

|[創建型設計模式](#creational-design-patterns)|[結構型設計模式](#structural-design-patterns)|[行為型設計模式](#behavioral-design-patterns)|
|:-|:-|:-|
|[簡單工廠](#-simple-factory)|[適配器](#-adapter)|[責任鏈](#-chain-of-responsibility)|
|[工廠方法](#-factory-method)|[橋接](#-bridge)|[命令](#-command)|
|[抽象工廠](#-abstract-factory)|[組合](#-composite)|[迭代器](#-iterator)|
|[建造者](#-builder)|[裝飾器](#-decorator)|[中介者](#-mediator)|
|[原型](#-prototype)|[外觀](#-facade)|[備忘錄](#-memento)|
|[單例](#-singleton)|[享元](#-flyweight)|[觀察者](#-observer)|
||[代理](#-proxy)|[訪問者](#-visitor)|
|||[策略](#-strategy)|
|||[狀態](#-state)|
|||[模板方法](#-template-method)|

<br>

介紹
=================

設計模式是解決重複問題的方案；**處理特定問題的指導原則**。它們不是可以直接插入到應用程序中的類、包或庫，也不是等待魔法發生的東西。相反，它們是處理特定情況下特定問題的指導原則。

> 設計模式是解決重複問題的方案；處理特定問題的指導原則

維基百科將其描述為

> 在軟體工程中，軟體設計模式是針對軟體設計中常見問題的通用可重用解決方案。它不是可以直接轉換為源代碼或機器代碼的完成設計。它是描述或模板，用於解決可以在許多不同情況下使用的問題。

⚠️ 注意事項
-----------------
- 設計模式並不是解決所有問題的靈丹妙藥。
- 不要強行使用它們；如果這樣做，壞事就會發生。
- 請記住，設計模式是針對**問題**的解決方案，而不是**尋找**問題的解決方案；所以不要過度思考。
- 如果在正確的地方以正確的方式使用，它們可以證明是救星；否則，它們可能導致代碼的可怕混亂。

> 另外請注意，下面的代碼示例是用 PHP-7 編寫的，但這不應該阻止你，因為概念是相同的。

設計模式類型
-----------------

* [創建型](#creational-design-patterns)
* [結構型](#structural-design-patterns)
* [行為型](#behavioral-design-patterns)

創建型設計模式
==========================

簡單來說
> 創建型模式專注於如何實例化一個對象或一組相關對象。

維基百科說
> 在軟體工程中，創建型設計模式是處理對象創建機制的設計模式，試圖以適合情況的方式創建對象。基本的對象創建形式可能導致設計問題或增加設計複雜性。創建型設計模式通過某種方式控制對象的創建來解決這個問題。

 * [簡單工廠](#-simple-factory)
 * [工廠方法](#-factory-method)
 * [抽象工廠](#-abstract-factory)
 * [建造者](#-builder)
 * [原型](#-prototype)
 * [單例](#-singleton)

🏠 簡單工廠
--------------
現實世界示例
> 假設你正在建造一座房子，你需要門。你可以穿上木匠的衣服，帶上木材、膠水、釘子和所有建造門所需的工具，開始在你的房子裡建造門，或者你可以簡單地打電話給工廠，讓他們把建好的門送到你這裡，這樣你就不需要了解門的製作過程或處理製作過程中產生的混亂。

簡單來說
> 簡單工廠為客戶生成一個實例，而不向客戶暴露任何實例化邏輯。

維基百科說
> 在面向對象編程（OOP）中，工廠是創建其他對象的對象——正式來說，工廠是一個函數或方法，它從某個方法調用中返回不同原型或類的對象，假設為“new”。

**編程示例**

首先，我們有一個門接口及其實現
```php:i18n/to/readme.zh-CN.md
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
然後我們有我們的門工廠，它製造門並返回它
```php
class DoorFactory
{
    public static function makeDoor($width, $height): Door
    {
        return new WoodenDoor($width, $height);
    }
}
```
然後可以這樣使用
```php
// 給我做一扇100x200的門
$door = DoorFactory::makeDoor(100, 200);

echo '寬度: ' . $door->getWidth();
echo '高度: ' . $door->getHeight();

// 給我做一扇50x100的門
$door2 = DoorFactory::makeDoor(50, 100);
```

**何時使用？**

當創建一個對象不僅僅是幾個賦值，並且涉及一些邏輯時，將其放入專用工廠中是有意義的，而不是在每個地方重複相同的代碼。

🏭 工廠方法
--------------

現實世界示例
> 考慮一個招聘經理的案例。 一個人不可能面試每個職位。根據職位空缺，她必須決定並將面試步驟委託給不同的人。

簡單來說
> 它提供了一種將實例化邏輯委託給子類的方法。

維基百科說
> 在基於類的編程中，工廠方法模式是一種創建模式，它使用工廠方法來處理創建對象的問題，而不必指定將要創建的對象的確切類。這是通過調用工廠方法來創建對象實現的——該方法要麼在接口中指定並由子類實現，要麼在基類中實現並可選地由派生類重寫——而不是通過調用構造函數。

 **編程示例**

以我們的招聘經理示例為例。首先，我們有一個面試官接口及其實現

```php
interface Interviewer
{
    public function askQuestions();
}

class Developer implements Interviewer
{
    public function askQuestions()
    {
        echo '詢問設計模式相關問題！';
    }
}

class CommunityExecutive implements Interviewer
{
    public function askQuestions()
    {
        echo '詢問社區建設相關問題';
    }
}
```

現在讓我們創建我們的`HiringManager`

```php
abstract class HiringManager
{

    // 工廠方法
    abstract protected function makeInterviewer(): Interviewer;

    public function takeInterview()
    {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}

```
現在任何子類都可以擴展它並提供所需的面試官
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
然後可以這樣使用

```php
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // 輸出: 詢問設計模式相關問題

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // 輸出: 詢問社區建設相關問題。
```

**何時使用？**

當類中有一些通用處理，但所需的子類在運行時動態決定時非常有用。換句話說，當客戶端不知道它可能需要哪個確切的子類時。

🔨 抽象工廠
----------------

現實世界示例
> 擴展我們簡單工廠的門示例。根據你的需求，你可能會從木門商店獲得一扇木門，從鐵門商店獲得一扇鐵門，或者從相關商店獲得一扇PVC門。此外，你可能需要一個具有不同專業技能的人來安裝門，例如木匠安裝木門，焊工安裝鐵門等。正如你所看到的，門之間現在存在依賴關係，木門需要木匠，鐵門需要焊工等。

簡單來說
> 工廠的工廠；一個將單個但相關/依賴工廠組合在一起的工廠，而不指定它們的具體類。

維基百科說
> 抽象工廠模式提供了一種封裝一組具有共同主題的單個工廠的方法，而不指定它們的具體類。

**編程示例**

翻譯上述門的示例。首先，我們有我們的`Door`接口及其實現

```php
interface Door
{
    public function getDescription();
}

class WoodenDoor implements Door
{
    public function getDescription()
    {
        echo '我是一扇木門';
    }
}

class IronDoor implements Door
{
    public function getDescription()
    {
        echo '我是一扇鐵門';
    }
}
```
然後我們有每種門類型的安裝專家

```php
interface DoorFittingExpert
{
    public function getDescription();
}

class Welder implements DoorFittingExpert
{
    public function getDescription()
    {
        echo '我只能安裝鐵門';
    }
}

class Carpenter implements DoorFittingExpert
{
    public function getDescription()
    {
        echo '我只能安裝木門';
    }
}
```

現在我們有我們的抽象工廠，它將讓我們製造一系列相關對象，即木門工廠將創建一扇木門和木門安裝專家，而鐵門工廠將創建一扇鐵門和鐵門安裝專家
```php
interface DoorFactory
{
    public function makeDoor(): Door;
    public function makeFittingExpert(): DoorFittingExpert;
}

// 木門工廠返回木門和木門安裝工
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

// 鐵門工廠獲取鐵門和相關的安裝專家
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
然後可以這樣使用
```php
$woodenFactory = new WoodenDoorFactory();

$door = $woodenFactory->makeDoor();
$expert = $woodenFactory->makeFittingExpert();

$door->getDescription();  // 輸出: 我是一扇木門
$expert->getDescription(); // 輸出: 我只能安裝木門

// 鐵門工廠同樣
$ironFactory = new IronDoorFactory();

$door = $ironFactory->makeDoor();
$expert = $ironFactory->makeFittingExpert();

$door->getDescription();  // 輸出: 我是一扇鐵門
$expert->getDescription(); // 輸出: 我只能安裝鐵門
```

正如你所看到的，木門工廠封裝了`carpenter`和`wooden door`，而鐵門工廠封裝了`iron door`和`welder`。因此，它幫助我們確保為每扇創建的門，我們不會得到錯誤的安裝專家。

**何時使用？**

當存在相互依賴的關係，並且涉及不太簡單的創建邏輯時。

👷 建造者
--------------------------------------------
現實世界示例
> 想象一下你在Hardee's點了一份特定的套餐，比如“Big Hardee”，他們直接把它交給你，而不問任何問題；這就是簡單工廠的例子。但有些情況下，創建邏輯可能涉及更多步驟。例如，你想要一個定制的Subway套餐，你有多個選項來製作你的漢堡，比如你想要什麼麵包？你想要什麼類型的醬？你想要什麼奶酪？等等。在這種情況下，建造者模式就派上用場了。

簡單來說
> 允許你創建對象的不同風味，同時避免構造函數污染。當對象的風味可能有很多種，或者創建對象時涉及很多步驟時非常有用。

維基百科說
> 建造者模式是一種對象創建軟體設計模式，旨在找到解決望遠鏡構造反模式的方案。

在此，我想補充一點關於望遠鏡構造反模式的內容。在某個時刻，我們都見過如下構造函數：

```php
public function __construct($size, $cheese = true, $pepperoni = true, $tomato = false, $lettuce = true)
{
}
```

正如你所看到的，構造函數參數的數量可能迅速增加，並且可能變得難以理解參數的排列。此外，如果你想在將來添加更多選項，這個參數列表可能會不斷增長。這被稱為望遠鏡構造反模式。

**編程示例**

理智的替代方案是使用建造者模式。首先，我們有我們想要製作的漢堡

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

然後我們有建造者

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
然後可以這樣使用：

```php
$burger = (new BurgerBuilder(14))
                    ->addPepperoni()
                    ->addLettuce()
                    ->addTomato()
                    ->build();
```

**何時使用？**

當對象的風味可能有很多種，並且為了避免構造函數的望遠鏡化時。與工廠模式的關鍵區別在於；工廠模式用於創建是一步過程，而建造者模式用於創建是多步過程。

🐑 原型
------------
現實世界示例
> 還記得多莉嗎？那隻被克隆的羊！我們不深入細節，但關鍵點在於它是關於克隆的。

簡單來說
> 通過克隆基於現有對象創建對象。

維基百科說
> 原型模式是一種軟體開發中的創建型設計模式。當要創建的對象類型由原型實例決定時，使用原型實例進行克隆以生成新對象。

簡而言之，它允許你創建現有對象的副本並根據需要修改它，而不是從頭開始創建對象並進行設置。

**編程示例**

在PHP中，可以使用`clone`輕鬆實現

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
然後可以像下面這樣克隆
```php
$original = new Sheep('Jolly');
echo $original->getName(); // Jolly
echo $original->getCategory(); // 山羊

// 克隆並修改所需的內容
$cloned = clone $original;
$cloned->setName('Dolly');
echo $cloned->getName(); // Dolly
echo $cloned->getCategory(); // 山羊
```

你還可以使用魔術方法`__clone`來修改克隆行為。

**何時使用？**

當需要一個與現有對象相似的對象，或者創建對象的成本相對於克隆來說很高時。

💍 單例
------------
現實世界示例
> 一個國家一次只能有一個總統。每當需要時，必須調用同一位總統。總統在這裡是單例。

簡單來說
> 確保某個類只有一個對象被創建。

維基百科說
> 在軟體工程中，單例模式是一種軟體設計模式，它限制了類的實例化為一個對象。當系統中確切需要一個對象來協調操作時，這很有用。

單例模式實際上被認為是一種反模式，應該謹慎使用。它並不一定是壞的，可能有一些有效的用例，但應該謹慎使用，因為它在應用程序中引入了全局狀態，在一個地方的更改可能會影響其他區域，並且可能會變得非常難以調試。另一個壞處是它使你的代碼緊密耦合，並且模擬單例可能很困難。

**編程示例**

要創建單例，請將構造函數設為私有，禁用克隆，禁用擴展，並創建一個靜態變量來存放實例
```php
final class President
{
    private static $instance;

    private function __construct()
    {
        // 隱藏構造函數
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
然後使用
```php
$president1 = President::getInstance();
$president2 = President::getInstance();

var_dump($president1 === $president2); // true
```

結構型設計模式
==========================
簡單來說
> 結構型模式主要關注對象組合，或者換句話說，實體如何相互使用。或者換句話說，它們幫助回答“如何構建軟體組件？”

維基百科說
> 在軟體工程中，結構型設計模式是通過識別實現實體之間關係的簡單方法來簡化設計的設計模式。

 * [適配器](#-adapter)
 * [橋接](#-bridge)
 * [組合](#-composite)
 * [裝飾器](#-decorator)
 * [外觀](#-facade)
 * [享元](#-flyweight)
 * [代理](#-proxy)

🔌 適配器
-------
現實世界示例
> 假設你有一些照片在你的內存卡上，你需要將它們傳輸到你的計算機。為了傳輸它們，你需要某種適配器，使其與計算機端口兼容，以便你可以將內存卡連接到計算機。在這種情況下，讀卡器就是適配器。
> 另一個例子是著名的電源適配器；三腳插頭無法連接到兩個插孔的插座，它需要使用電源適配器使其與兩個插孔的插座兼容。
> 還有一個例子是翻譯者將一個人說的話翻譯成另一個人說的話。

簡單來說
> 適配器模式允許你將一個不兼容的對象包裝在適配器中，使其與另一個類兼容。

維基百科說
> 在軟體工程中，適配器模式是一種軟體設計模式，它允許現有類的接口作為另一種接口使用。它通常用於使現有類與其他類一起工作，而不修改其源代碼。

**編程示例**

考慮一個遊戲，其中有一個獵人，他獵殺獅子。

首先，我們有一個`Lion`接口，所有類型的獅子都必須實現

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
獵人期望任何實現`Lion`接口的對象都可以被獵殺。
```php
class Hunter
{
    public function hunt(Lion $lion)
    {
        $lion->roar();
    }
}
```

現在假設我們必須在遊戲中添加一個`WildDog`，以便獵人也可以獵殺它。但我們不能直接這樣做，因為狗有不同的接口。為了使其與我們的獵人兼容，我們將必須創建一個適配器，使其兼容

```php
// 這需要添加到遊戲中
class WildDog
{
    public function bark()
    {
    }
}

// 針對野狗的適配器，使其與我們的遊戲兼容
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
現在`WildDog`可以通過`WildDogAdapter`在我們的遊戲中使用。

```php
$wildDog = new WildDog();
$wildDogAdapter = new WildDogAdapter($wildDog);

$hunter = new Hunter();
$hunter->hunt($wildDogAdapter);
```

🚡 橋接
------
現實世界示例
> 假設你有一個網站，有不同的頁面，你需要允許用戶更改主題。你會怎麼做？為每個主題創建每個頁面的多個副本，還是僅僅創建單獨的主題並根據用戶的偏好加載它們？橋接模式允許你這樣做。

![有和沒有橋接模式的對比](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

簡單來說
> 橋接模式是關於優先使用組合而不是繼承。實現細節從一個層次結構推送到另一個具有單獨層次結構的對象。

維基百科說
> 橋接模式是一種軟體工程設計模式，旨在“將抽象與其實現解耦，以便兩者可以獨立變化”。

**編程示例**

翻譯我們的網頁示例。這裡我們有`WebPage`層次結構

...(about 1798 lines omitted)...
        return isset($this->stations[$this->counter]);
    }
}
```
然後可以這樣使用

```php
$stationList = new StationList();

$stationList->addStation(new RadioStation(89));
$stationList->addStation(new RadioStation(101));
$stationList->addStation(new RadioStation(102));
$stationList->addStation(new RadioStation(103.2));

foreach($stationList as $station) {
    echo $station->getFrequency() . PHP_EOL;
}

$stationList->removeStation(new RadioStation(89)); // 將移除89頻道
```

👽 中介者
========

現實世界示例
> 一個通用的例子是，當你在手機上與某人交談時，有一個網絡提供商在你和他們之間，您的對話通過它進行，而不是直接發送。在這種情況下，網絡提供商就是中介。

簡單來說
> 中介者模式添加了一個第三方對象（稱為中介）來控制兩個對象（稱為同事）之間的交互。它有助於減少相互通信的類之間的耦合。因為現在它們不需要了解彼此的實現。

維基百科說
> 在軟體工程中，中介者模式定義了一個封裝一組對象如何交互的對象。由於它可以改變程序的運行行為，因此該模式被認為是一種行為模式。

**編程示例**

這裡是一個簡單的聊天房間（即中介）與用戶（即同事）之間發送消息的示例。

首先，我們有中介，即聊天房間

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

然後我們有我們的用戶，即同事
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
然後是使用
```php
$mediator = new ChatRoom();

$john = new User('John Doe', $mediator);
$jane = new User('Jane Doe', $mediator);

$john->send('你好！');
$jane->send('嘿！');

// 輸出將是
// 2月14日，10:58 [John]: 你好！
// 2月14日，10:58 [Jane]: 嘿！
```

💾 備忘錄
-------
現實世界示例
> 以計算器（即發起者）為例，每當你進行某些計算時，最後的計算結果會保存在內存中（即備忘錄），以便你可以返回並通過某些操作按鈕（即看護者）恢復它。

簡單來說
> 備忘錄模式是關於捕獲和存儲對象的當前狀態，以便可以在稍後平滑地恢復。

維基百科說
> 備忘錄模式是一種軟體設計模式，提供將對象恢復到其先前狀態的能力（通過回滾撤消）。

通常在你需要提供某種撤消功能時非常有用。

**編程示例**

讓我們以文本編輯器為例，它會定期保存狀態，以便在需要時可以恢復。

首先，我們有我們的備忘錄對象，它將能夠保存編輯器狀態

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

然後我們有我們的編輯器，即發起者，將使用備忘錄對象

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

然後可以這樣使用

```php
$editor = new Editor();

// 輸入一些內容
$editor->type('這是第一句。');
$editor->type('這是第二句。');

// 保存狀態以恢復：這是第一句。 這是第二句。
$saved = $editor->save();

// 輸入更多內容
$editor->type('這是第三句。');

// 輸出：保存前的內容
echo $editor->getContent(); // 這是第一句。 這是第二句。 這是第三句。

// 恢復到最後保存的狀態
$editor->restore($saved);

$editor->getContent(); // 這是第一句。 這是第二句。
```

😎 觀察者
--------
現實世界示例
> 一個很好的例子是求職者，他們訂閱某個招聘網站，並在有匹配的工作機會時收到通知。

簡單來說
> 定義對象之間的依賴關係，以便每當對象更改其狀態時，所有依賴於它的對象都會收到通知。

維基百科說
> 觀察者模式是一種軟體設計模式，其中一個對象稱為主題，維護其依賴對象（稱為觀察者）的列表，並在狀態更改時自動通知它們，通常通過調用它們的方法。

**編程示例**

翻譯我們上面的例子。首先我們有需要被通知的求職者
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
        // 處理工作發布
        echo '嗨 ' . $this->name . '! 新工作發布: '. $job->getTitle();
    }
}
```
然後我們有我們的工作發布，求職者將訂閱它
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
然後可以這樣使用

```php
// 創建訂閱者
$johnDoe = new JobSeeker('John Doe');
$janeDoe = new JobSeeker('Jane Doe');

// 創建發布者並附加訂閱者
$jobPostings = new EmploymentAgency();
$jobPostings->attach($johnDoe);
$jobPostings->attach($janeDoe);

// 添加新工作，看看訂閱者是否收到通知
$jobPostings->addJob(new JobPost('軟體工程師'));

// 輸出
// 嗨 John Doe! 新工作發布: 軟體工程師
// 嗨 Jane Doe! 新工作發布: 軟體工程師
```

🏃 訪問者
-------
現實世界示例
> 考慮一個人訪問迪拜。他們只需要一種方式（即簽證）進入迪拜。到達後，他們可以自己訪問迪拜的任何地方，而無需詢問許可或進行任何繁瑣的工作；只需告訴他們一個地方，他們就可以訪問。訪問者模式讓你做到這一點，它幫助你添加訪問的地方，以便他們可以盡可能多地訪問，而無需進行任何繁瑣的工作。

簡單來說
> 訪問者模式允許你向對象添加進一步的操作，而無需修改它們。

維基百科說
> 在面向對象編程和軟體工程中，訪問者設計模式是一種將算法與其操作的對象結構分離的方法。該分離的結果是能夠向現有對象結構添加新操作，而無需修改這些結構。這是遵循開放/封閉原則的一種方式。

**編程示例**

讓我們以動物園模擬為例，我們有幾種不同的動物，我們必須讓它們發出聲音。讓我們使用訪問者模式翻譯這個

```php
// 被訪問者
interface Animal
{
    public function accept(AnimalOperation $operation);
}

// 訪問者
interface AnimalOperation
{
    public function visitMonkey(Monkey $monkey);
    public function visitLion(Lion $lion);
    public function visitDolphin(Dolphin $dolphin);
}
```
然後我們有動物的實現
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
讓我們實現我們的訪問者
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

然後可以這樣使用
```php
$monkey = new Monkey();
$lion = new Lion();
$dolphin = new Dolphin();

$speak = new Speak();

$monkey->accept($speak);    // 哦哦啊啊！    
$lion->accept($speak);      // 吼！
$dolphin->accept($speak);   // 嘟嘟嘟！
```
我們本可以通過為動物創建繼承層次結構來簡單地做到這一點，但那樣我們必須在每次添加新操作時修改動物。但現在我們不必這樣做。例如，假設我們被要求為動物添加跳躍行為，我們可以通過創建一個新的訪問者來簡單地添加它，即。

```php
class Jump implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        echo '跳了20英尺高！跳到樹上！';
    }

    public function visitLion(Lion $lion)
    {
        echo '跳了7英尺！回到地面！';
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        echo '在水面上走了一會兒，然後消失了';
    }
}
```
然後使用
```php
$jump = new Jump();

$monkey->accept($speak);   // 哦哦啊啊！
$monkey->accept($jump);    // 跳了20英尺高！跳到樹上！

$lion->accept($speak);     // 吼！
$lion->accept($jump);      // 跳了7英尺！回到地面！

$dolphin->accept($speak);  // 嘟嘟嘟！
$dolphin->accept($jump);   // 在水面上走了一會兒，然後消失了
```

💡 策略
--------

現實世界示例
> 考慮排序的例子，我們實現了冒泡排序，但數據開始增長，冒泡排序變得非常慢。為了應對這一點，我們實現了快速排序。但現在，儘管快速排序算法在大型數據集上表現更好，但在較小的數據集上卻非常慢。為了處理這個問題，我們實現了一種策略，對於小型數據集，將使用冒泡排序，對於大型數據集，將使用快速排序。

簡單來說
> 策略模式允許你根據情況切換算法或策略。

維基百科說
> 在計算機編程中，策略模式（也稱為策略模式）是一種行為軟體設計模式，允許在運行時選擇算法的行為。

**編程示例**

翻譯我們的示例。首先，我們有我們的策略接口和不同的策略實現

```php
interface SortStrategy
{
    public function sort(array $dataset): array;
}

class BubbleSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "使用冒泡排序進行排序";

        // 執行排序
        return $dataset;
    }
}

class QuickSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "使用快速排序進行排序";

        // 執行排序
        return $dataset;
    }
}
```

然後我們有我們的客戶端，將使用任何策略
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
然後可以這樣使用
```php
$smalldataset = [1, 3, 4, 2];
$bigdataset = [1, 4, 3, 2, 8, 10, 5, 6, 9, 7];

$sorter = new Sorter(new BubbleSortStrategy(), new QuickSortStrategy());

$sorter->sort($dataset); // 輸出 : 使用冒泡排序進行排序

$sorter->sort($bigdataset); // 輸出 : 使用快速排序進行排序
```

💢 狀態
-----
現實世界示例
> 想象一下你在使用某個繪圖應用程序，你選擇畫筆進行繪圖。現在畫筆根據所選顏色改變其行為；如果你選擇了紅色，它將用紅色繪製，如果選擇了藍色，則用藍色繪製等。

簡單來說
> 它讓你在狀態變化時改變類的行為。

維基百科說
> 狀態模式是一種行為軟體設計模式，以面向對象的方式實現狀態機。使用狀態模式，通過將每個單獨的狀態實現為狀態模式接口的派生類，並通過調用模式超類中定義的方法來實現狀態轉換。

**編程示例**

讓我們以電話為例。首先，我們有狀態接口和一些狀態實現

```php
interface PhoneState {
    public function pickUp(): PhoneState;
    public function hangUp(): PhoneState;
    public function dial(): PhoneState;
}

// 狀態實現
class PhoneStateIdle implements PhoneState {
    public function pickUp(): PhoneState {
        return new PhoneStatePickedUp();
    }
    public function hangUp(): PhoneState {
        throw new Exception("已經空閒");
    }
    public function dial(): PhoneState {
        throw new Exception("在空閒狀態下無法撥號");
    }
}

class PhoneStatePickedUp implements PhoneState {
    public function pickUp(): PhoneState {
        throw new Exception("已經拿起");
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
        throw new Exception("已經拿起");
    }
    public function hangUp(): PhoneState {
        return new PhoneStateIdle();
    }
    public function dial(): PhoneState {
        throw new Exception("已經撥號");
    }
}
```

然後我們有我們的電話類，根據不同的行為調用不同的方法來改變狀態

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

然後可以這樣使用，它將調用相關的狀態方法：

```php
$phone = new Phone();

$phone->pickUp();
$phone->dial();
```

📒 模板方法
---------------

現實世界示例
> 假設我們正在建造一座房子。建造的步驟可能如下：
> - 準備房子的基礎
> - 建造牆壁
> - 添加屋頂
> - 添加其他樓層

> 這些步驟的順序是不能改變的；例如，你不能在建造牆壁之前建造屋頂，但每個步驟都可以修改，例如牆壁可以用木材、聚酯或石頭建造。

簡單來說
> 模板方法定義了如何執行某個算法的骨架，但將這些步驟的實現推遲到子類。

維基百科說
> 在軟體工程中，模板方法模式定義了操作的程序骨架，將某些步驟推遲到子類。它允許在不改變算法結構的情況下重新定義某些步驟。

**編程示例**

想像一下我們有一個構建工具，幫助我們測試、檢查、構建、生成構建報告（即代碼覆蓋報告、檢查報告等）並在測試伺服器上部署我們的應用程序。

首先，我們有我們的基類，指定構建算法的骨架
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

然後我們可以有我們的實現

```php
class AndroidBuilder extends Builder
{
    public function test()
    {
        echo '運行安卓測試';
    }

    public function lint()
    {
        echo '檢查安卓代碼';
    }

    public function assemble()
    {
        echo '組裝安卓構建';
    }

    public function deploy()
    {
        echo '將安卓構建部署到伺服器';
    }
}

class IosBuilder extends Builder
{
    public function test()
    {
        echo '運行iOS測試';
    }

    public function lint()
    {
        echo '檢查iOS代碼';
    }

    public function assemble()
    {
        echo '組裝iOS構建';
    }

    public function deploy()
    {
        echo '將iOS構建部署到伺服器';
    }
}
```
然後可以這樣使用

```php
$androidBuilder = new AndroidBuilder();
$androidBuilder->build();

// 輸出：
// 運行安卓測試
// 檢查安卓代碼
// 組裝安卓構建
// 將安卓構建部署到伺服器

$iosBuilder = new IosBuilder();
$iosBuilder->build();

// 輸出：
// 運行iOS測試
// 檢查iOS代碼
// 組裝iOS構建
// 將iOS構建部署到伺服器
```

## 🚦 總結

這就是全部。我將繼續改進這個內容，因此你可能想要關注/星標這個存儲庫以便重新訪問。此外，我計劃寫關於架構模式的內容，敬請期待。

## 👬 貢獻

- 報告問題
- 提交改進的拉取請求
- 傳播這個消息
- 隨時與我聯繫，提供反饋 [![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/kamrify.svg?style=social&label=關注%20%40kamrify)](https://twitter.com/kamrify)

## 许可证

[![许可证: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/) 
