# 服务提供者&Contracts

---

### 使用方法

1. 写一个Contract接口：

```php
<?php
namespace App\Contracts;

interface Hello {
	public function hello();
}
```

2. 写上面Contract的实现类：

```php
<?php
namespace App\Services;
use App\Contracts\Hello;

class HelloWorld implements Hello {
    function hello() {
    	return "Hello!~~";
    }
}
```

3. 写一个自定义的ServiceProvider：

```php
<?php
namespace App\Providers;
use Illuminate\Support\ServiceProvider;

class HelloServiceProvider extends ServiceProvider {

    public function boot() {
        //
    }

    public function register() {
        //给这个接口一个别名
        $this->app->bind('Hello','App\Contracts\Hello');
        //将Contract接口和它的实现类绑定
        $this->app->bind('App\Contracts\Hello','App\Services\HelloWorld');
    }
}
```

4. 在config\app.php中注册这个服务提供者：

```php
//在providers中加入这行代码即可：
App\Providers\HelloServiceProvider::class;
```

5. 使用：

```php
<?php

namespace App\Http\Controllers;

use App\Contracts\Hello;

class DiaryController extends Controller
{
    protected $hello;

    public function __construct(Hello $hello){
        $this->hello=$hello;
    }

    public function index(){
        return view('diaries.index',[
            'hello'=>$this->hello->hello(),
        ]);
    }
}
```

