# 服务提供者&Facades

### 使用方法

> 在Laravel中将类注册为Fcade可以使用Ioc容器，每次使用这个类的时候只会初始化一次类，类似单例模式，而且可以像使用静态方法调用类的方法，下面是在Laravel中注册Facades的步骤。

1. 创建一个工具类，此为门面的实体类。

```php
<?php

namespace App\Models;

class Test {
    public function method() {
        
    }
}
```

2. 创建一个门面，可理解为调用工具类的代理层，即门面。

```php
<?php

namespace Lib\Facades;
use Illuminate\Support\Facades\Facade;

class TestFacades {
    protected static function getFacadeAccessor() {

    	return 'testmodel';

    }
}

```

3. 在模块提供者中的register方法新增方法，代码如下：

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
        //门面调用的别名
        $this->app->singleton('testmodel', function ($app) {
			//创建门面的实体类
            return new \App\Models\Test();

        });
		//为门面添加别名
        $this->app->alias('testmodel', 'Lib\\Facades\\TestFacades');
		//第二种添加别名的方式为在项目的config/app.php的aliases字段中添加门面别名，key为别名，value为门面的类名
        //第三种添加别名的方式为在开发composer第三方包时在composer.json的extra中添加，如：
        {
            "extra":{
                "laravel":{
                    "aliases":{
                        "testmodel":"Lib\\Facades\\TestFacades"
                    }
                }
            }
        }
    }

}
```

2. 这里把命名空间是App\Models的Test类注册为单例模式，并且取个别名testmodel

   > 这个类的文件位置Lib\\Facades\\TestFacades.php.



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

        testmodel::show();

        testmodel::show();

    }

}
```

> 经过注册Facade后，调用show方法就是testmodel::show()的形式，并且类似单例模式不会多次实例化，调用也十分简单。