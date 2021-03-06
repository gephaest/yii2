Вхідні скрипти
===============

Вхідні скрипти це перша ланка в процесі початкового завантаження додатку. Додаток (веб додаток або консольний додаток)
включає єдиний вхідний скрипт. Кінцеві користувачі роблять запити до вхідного скрипта, який створює об’єкти додатка і перенаправляє запит до них.

Вхідні скрипти для веб додатків повинні бути збережені в теках доступних з веб, таким чином вони можуть бути доступними кінцевому користувачу. Такі скрипти за звичай називаються `index.php`, але також можут використовуватись і інші імена, які можуть бути розпізнані використовуваними веб-серверами.

Вхідні скрипти для консольних додатків за звичай розміщенні в [кореневій директорії](structure-applications.md) додатку і мають назву
`yii` (з суфіксом `.php`). Вони мають права на виконання, таким чином користувачі зможуть запускати консольні додатки через команду `./yii <маршрут> [аргументи] [опції]`.

Вхідні скрипти в основному виконують наступну роботу:

* Оголошують глобальні константи;
* Реєструють завантажувач класів [Composer](http://getcomposer.org/doc/01-basic-usage.md#autoloading);
* Підключають файл класа [[Yii]];
* Завантажують конфігурацію додатка;
* Створюють і конфігурують об’єкт [додатка](structure-applications.md);
* Викликають метод [[yii\base\Application::run()]] додатка для опрацювання вхідного запиту.


## Веб додатки <a name="web-applications"></a>

Нижче наведений код вхідного скрипта для [базового шаблону додатка](start-installation.md).

```php
<?php

defined('YII_DEBUG') or define('YII_DEBUG', true);
defined('YII_ENV') or define('YII_ENV', 'dev');

// реєстрація завантажувача класів Composer
require(__DIR__ . '/../vendor/autoload.php');

// підключення файла класа Yii
require(__DIR__ . '/../vendor/yiisoft/yii2/Yii.php');

// завантаження конфігурації додатка
$config = require(__DIR__ . '/../config/web.php');

// створення і конфігурація додатка, а також виклик метода для опрацювання вхідного запиту
(new yii\web\Application($config))->run();
```


## Консольні додатки <a name="console-applications"></a>

Нижче наведений аналогічний код вхідного скрипта консольного додатка:

```php
#!/usr/bin/env php
<?php
/**
 * Yii console bootstrap file.
 *
 * @link http://www.yiiframework.com/
 * @copyright Copyright (c) 2008 Yii Software LLC
 * @license http://www.yiiframework.com/license/
 */

defined('YII_DEBUG') or define('YII_DEBUG', true);

// fcgi не має констант STDIN и STDOUT, вони визначаються за замовчуванням
defined('STDIN') or define('STDIN', fopen('php://stdin', 'r'));
defined('STDOUT') or define('STDOUT', fopen('php://stdout', 'w'));

// реєстрація завантажувача класів Composer
require(__DIR__ . '/vendor/autoload.php');

// підключення файла класа Yii
require(__DIR__ . '/vendor/yiisoft/yii2/Yii.php');

// завантаження конфігурації додатка
$config = require(__DIR__ . '/config/console.php');

$application = new yii\console\Application($config);
$exitCode = $application->run();
exit($exitCode);
```


## Оголошення констант <a name="defining-constants"></a>

Вхідні скрипти є найкращим місцем для оголошення глобальних констант. Yii підтримує наступні три константи:

* `YII_DEBUG`: вказує чи працює додаткок у відлагоджувальному режимі. Перебуваючи у відлагоджувальному режимі, додаток буде збирати більше інформації у логи і покаже більш детальний стек викликів, якщо виникне виняток. По цій причині, відлагоджувальний режим повинен бути використаний тільки в процесі розробки. За замовчуванням значення `YII_DEBUG` дорівнює false;
* `YII_ENV`: вказує в якому середовищі працює додаток. Дана тема детально розглянута в розділі [Конфігурації](concept-configurations.md#environment-constants).
  За замовчуванням значення `YII_ENV` дорівнює `'prod'`, що значить, що додаток працює у виробничому режимі;
* `YII_ENABLE_ERROR_HANDLER`: вказує чи потрібно включати наявний у Yii обробник помилок. За замовчуванням значення даної константи
  дорівнює true.

При визначенні константи, ми зазвичай використовуєм наступний код:

```php
defined('YII_DEBUG') or define('YII_DEBUG', true);
```

який рівнозначний коду, наведеному нижче:

```php
if (!defined('YII_DEBUG')) {
    define('YII_DEBUG', true);
}
```

Перший варіант є більш коротким і зрозумілим.

Константи мають бути визначені якомога раніше, в самому початку вхідного скрипта, таким чином вони зможуть вплинути на решту PHP файлів які будуть підключатись.
