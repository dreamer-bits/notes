# C指针注意项

1. 当定义指针时，如：

   ```c
   int *p;
   ```

   应看作(int *)p;

   因此当定义指针变量同时又赋值时应赋值地址，如：

   ```c
   int *p = &a;
   ```

   当定义过指针变量时，如果没赋值过地址时，应该先赋值地址

   ```c
   int *p;

   p = &a;
   ```

   如此则造成*p赋值和p赋值都是等价的错觉

2. 当有（类型 *）指针的形式时，说明是强制转换而不是指针操作，如：

   ```c
   *((int *)p)	//等价于p只是*p转换成整型指针

   *(*p)	//则是二级指针操作
   ```

3. 结构体指针操作

   1. 结构体指针有独立的指针操作符->，使用->操作符时只需要指针的地址，如：

      ```c
      //指针初始化参考第一点，亦可写写成：

      //student *p;

      //p =  (student *)malloc(sizeof(student));

      student *p = (student *)malloc(sizeof(student));

      p->name;
      ```

   2. 结构体操作符(圆点操作符) . 是对结构体变量的操作，如

      ```c
      student *p = (student *)malloc(sizeof(student));

      student s;

      //s.name等价于p->name

      //而且p.name写法是错误的，因为圆点操作符只针对结构体变量，则：

      (*p).name = p->name

      //而*(p->name)是正确的，说明地址等于p->name变量的值进行操作
      ```

   3. 指针函数和函数指针

      1. 指针函数是值返回值为指针的函数，如：

         ```c
         void *function_a();
         ```

      2. 函数指针指的是函数名为指针的函数，如：

         ```c
         void (*function_b)(int x, int y);
         ```

         > 此类型函数可作注册函数使用，如：

         ```c
         void function_c(int a, int b);

         function_b = &function_c;

         //则可以这样调用function_c

         (*function_b)(x, y);

         //如果函数指针已使用typedef声明类型，如：

         typedef void (*function_b)(int x, int y);

         //则可以这样调用function_c

         function_b func

         func = &function_c

         (*func)(x, y);
         ```

         ​