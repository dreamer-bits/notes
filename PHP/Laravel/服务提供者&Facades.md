# 服务提供者&Facades

### 使用方法

> 在Laravel中将类注册为Fcade可以使用Ioc容器，每次使用这个类的时候只会初始化一次类，类似单例模式，而且可以像使用静态方法调用类的方法，下面是在Laravel中注册Facades的步骤。

1. 在项目app目录的Providers/AppServiceProvider.php中的register方法新增方法，代码如下：

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider {
    
    public function boot() {	
    	//
    }

    public function register() {

        $this->registerTestModel();

    }

    private function registerTestModel() {

        this->app->singleton('testmodel', function (app) {

            $model = 'App\Models\Test';

            return new $model();

        });

        $this->app->alias('testmodel', 'App\Models\Test');

    }

}
```

2. 这里把命名空间是App\Models的Test类注册为单例模式，并且取个别名testmodel

   > 这个Test类的文件位置app/Models/Test.php.

3. 建立一个Facade类

   > 在项目根目录app\Facades目录新增文件，如Test.php，代码如下，目录不存在可以新建一个

```php
<?php

namespace App\Facades;

use Illuminate\Support\Facades\Facade;

class Test extends Facade {

    protected static function getFacadeAccessor() {

    	return 'testmodel';

    }

}
```

4. 通过继承Facade，重载getFacadeAccessor方法，返回之前绑定的单例模式的类的别名。
5. 使用Facade
6. 经过前面的步骤后，可以使用Test这个Facade了，如下示例是在控制器中使用Facade的方式。

```c
<?php

namespace App\Http\Controllers;

use App\Facades\Test;

use Illuminate\Routing\Controller;

class TestController extends Controller {

    public function __construct() {

        Test::show();

        Test::show();

    }

}
```

> 经过注册Facade后，调用show方法就是Test::show()的形式，并且类似单例模式不会多次实例化，调用也十分简单。