# ООП в PHP

# Зміст

${toc}

# ООП в PHP

## Опис класу і ініціалізація об'єкту

### Конструктор

PHP 5 дозволяє оголошувати методи-конструктори. Класи, в яких оголошено метод-конструктор, будуть викликати цей метод при кожному створенні нового об'єкта, так що це може виявитися корисним, наприклад, для ініціалізації будь-якого стану об'єкта перед його використанням.

```php
<?php
class BaseClass {
   function __construct() {
       print "Конструктор класса BaseClass\n";
   }
}
```

### Деструктор

PHP 5 надає концепцію **деструкції**, аналогічну до тієї, яка застосовується в інших ООП-мовах, таких як C ++. Деструкція буде викликаний при звільненні всіх посилань на певний об'єкт або при завершенні скрипта (**порядок виконання деструкторів не гарантовано**).

```php
<?php
class MyDestructableClass
{
   function __construct() {
       print "Конструктор\n";
   }

   function __destruct() {
       print "Уничтожается " . __CLASS__  . "\n";
   }
}

$obj = new MyDestructableClass();
```

### Статичні поля та методи

## Інкапсуляція

## Наслідування

### Виклик контруктора батьківського класа

Конструктори, які визначені в батьківських класах не викликаються автоматично, якщо дочірній клас визначає власний конструктор. Щоб викликати конструктор, оголошений в батьківському класі, потрібно здійснити виклик parent::__construct () всередині конструктора дочірнього класу. Якщо в дочірньому класі не визначений конструктор, то він може бути успадкований від батьківського класу як звичайний метод (якщо він не був визначений як приватний).

```php
<? Php
class BaseClass {
   function __construct () {
       print "Конструктор класу BaseClass \ n";
   }
}

class SubClass extends BaseClass {
   function __construct () {
       parent :: __ construct ();
       print "Конструктор класу SubClass \ n";
   }
}

class OtherSubClass extends BaseClass {
    // успадковує конструктор BaseClass
}
```

## Абстрактні класи та інтерфейси

## Трейти

З версії 5.4 в PHP з'явився такий цікавий механізм як домішки (trait), який за задумом розробників повинен допомогти розрулювати ситуації коли вже дуже хочеться застосувати множинне наслідування, але не можна.

Приклад використання трейтів: є такі класи - абстрактний клас Мебль, який лише знає що у меблів є розміри, Стіл з площею стільниці та Стілець з якимось максимально-можливим навантаженням, на додачу є Диван, поки просто диван:

```php
abstract class Furniture {
    protected $width;
    protected $height;
    protected $length;
    public function getDimension() {
        return [$this->width, $this->height, $this->length];
    }
}
class Table extends Furniture {
    protected $square;
    public function getSquare() {
        return $this->square;
    }
}
class Chair extends Furniture {
    protected $maxWeight;
    public function getMaxWeight() {
        return $this->maxWeight;
    }
}
class Couch extends Furniture {
}
```

Ось така у нас прості меблі виходить, тепер давайте міркувати як ми можемо розширити дані класи, і яким чином ми б це робили:
- Якщо нам потрібно дізнатися обсяг займаний меблями - створимо ще один метод в класі предка Furniture, тому що даний функціонал буде загальним для всіх меблів.
- Якщо нам потрібно дізнатися колір і матеріал меблів - то нам теж підійде клас Furniture, лише з одним застереженням, що колір і матеріал однорідні.
- Якщо ж нам треба вказати матеріал оббивки меблів, то нам вже треба або вносити даний функціонал в обидва класу Chair і Couch, але це копі-паст і зовсім не ООП, або повинен з'явиться новий клас Upholstered, від якого і будуть успадковані ці класи.
- Тепер згадаємо, що деякі меблі у нас може розкладатися, і нам треба додати цю інформацію для класів Table і Couch, можна було б створити ще один клас Folding і розширювати його, але ця зміна буде конфліктувати з попереднім рішенням, і виходить, що є єдиний вихід - копіпаст методів між класами.

```php
abstract class Furniture {
    protected $width;
    protected $height;
    protected $length;
    public function getDimension() {
        return [$this->width, $this->height, $this->length];
    }
 
    // requirement 01
    public function getVolume() {
        return $this->width * $this->height * $this->length;
    }
 
    // requirement 02
    protected $color;
    protected $material;
    public function getColor() {
        return $this->color;
    }
    public function getMaterial() {
        return $this->material;
    }
}
 
class Table extends Furniture {
    protected $square;
    public function getSquare() {
        return $this->square;
    }
 
    // requirement 04
    protected $maxSquare;
    public function getMaxSquare() {
        return $this->maxSquare;
    }
}
 
class Chair extends Furniture {
    protected $maxWeight;
    public function getMaxWeight() {
        return $this->maxWeight;
    }
    // requirement 03
    protected $upholstery;
    public function getUpholstery() {
        return $this->upholstery;
    }
}
 
class Couch extends Furniture {
    // requirement 03
    protected $upholstery;
    public function getUpholstery() {
        return $this->upholstery;
    }
    // requirement 04
    protected $maxSquare;
    public function getMaxSquare() {
        return $this->maxSquare;
    }
}
```

Так тут неозброєним оком видно копіпаст, і дуже хотілося б позбавиться від нього, хотілося б реалізацію вимог 3 і 4 закинути в окремий клас, і успадковувати його, але в PHP немає множинного наслідування, може бути тільки один клас предок. І ось в PHP 5.4 на сцену виходять домішки (trait), чимось вони схожі на класи, але лише здалеку, домішки лише групують якийсь набір функціоналу під однією вивіскою, але не більше. Давайте таки опишемо необхідний функціонал в домішках:

```php
// requirement 03
trait Upholstery {
    protected $upholstery;
    public function getUpholstery() {
        return $this->upholstery;
    }
}
 
// requirement 04
trait MaxSquare {
    protected $maxSquare;
    public function getMaxSquare() {
        return $this->maxSquare;
    }
}
```

Тепер дані домішки легко можна підключити в наших класах:

```php
class Table extends Furniture {
    // requirement 04
    use MaxSquare;
 
    protected $square;
    public function getSquare() {
        return $this->square;
    }
}
class Chair extends Furniture {
    // requirement 03
    use Upholstery;
 
    protected $maxWeight;
    public function getMaxWeight() {
        return $this->maxWeight;
    }
}
class Couch extends Furniture {
    // requirement 03
    use Upholstery;
    // requirement 04
    use MaxSquare;
}
```

## Поліморфізм

# Простір імен та імпортування класів

# PSR

## PSR-0

## PSR-1

## PSR-2

## PSR-3

## PSR-4

# Composer

> Composer (getcomposer.org) - це досить популярний менеджер залежностей для PHP. Ви можете описати від яких бібліотек залежить ваш проект і Composer встановить потрібні бібліотеки за вас! Причому Composer - це не менеджер пакетів в класичному розумінні. Так, він оперує з сутностями, які ми будемо називати «пакетами» або бібліотеками, але встановлюються вони всередину кожного проекту окремо, а не глобально (це одне з основних відмінностей від старого-доброго PEAR).

![](../resources/img/oop/img-1.png)

Коротко, як це працює:
1. У вас є проект, який залежить від декількох бібліотек.
2. Деякі з цих бібліотек залежать від інших бібліотек.
3. Ви описуєте в своєму проекті ті бібліотеки, від яких безпосередньо залежить ваш код.
4. Composer знаходить потрібні версії необхідних бібліотек для всього проекту, завантажує їх і встановлює в папку вашого проекту.

При створенні Composer автори черпали ідеї і натхнення з аналогічних проектів: npm для Node.js і Bundler для Ruby.

## Ініціалізація нового проекту

## Включення завантаження класів

# Логування за допомогою Monolog

# Домашнє завдання

## Варіанти

# Контрольні запитання

1. Поясніть принципи інкапсуляції, поліморфізму і наслідування.
2. Що таке трейти в PHP?
3. Що таке Composer?
4. Навіщо потрібен PSR?
