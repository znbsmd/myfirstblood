# myfirstblood
php代码规范


1.mvc 啊  ，不能讲数据操作写在controller ，避免冗余 ，影响性能 

2.多使用try...catch...

3.多使用构造函数,减少控制器里重复调用

4.不符合设计模式的最少知道原则,类One内部直接依赖了类Two

class One
{
    private $closure;
    public function __construct(Closure $closure)
    {  
        $this->closure = $closure;
    }
    public function doSomething()
    {
        if (...) {
            // 用的时候再实例化
            // 实现懒加载
            $instance = $this->closure();
            $instance->do(...)
        }
        ...
    }
}
...
$instance = new One(function () {
    // 类One内部外部依赖了类Two
    return new Two();
});
$instance->doSomething();
...
5.减少if...else... 异常放最前面不要夹杂
function doSomething() {
    if (...) {
        // 异常情况
        return ...;
    }
    if (...) {
        // 异常情况
        return ...;
    }
    //　正常逻辑
    ...
}
//　同样，如果是在一个类里面我会先处理异常的情况，然后先抛出异常
class One
{
    public function doSomething()
    {
        if (...) {
            // 异常情况
            throw new Exception(...);
        }
        if (...) {
            // 异常情况
            throw new Exception(...);
        }
        //　正常逻辑
        ...
    }
}
6.flase =>

class One
{
    public function doSomething()
    {
        if (...) {
            $instance = new A();
        }　elseif (...) {
            $instance = new A();
        } else {
            $instance = new C();
        }
        $instance->doSomething(...);
        ...
    }
}
true=>
class One
{
    private $map = [
        'a' => 'namespace\A', // 带上命名空间，因为变量是动态的
        'b' => 'namespace\B',
        'c' => 'namespace\C'
    ];
    public function doSomething()
    {
        ...
        $instance = new $this->map[$strategy];// $strategy是'a'或'b'或'c'
        $instance->doSomething(...);
        ...
    }
}
7.使用 接口

Interface Promotion
{
    public function promote(...);
}
class OnePromotion implement Promotion
{
    public function doSomething(...)
    {
        ...
    }
}
class TwoPromotion implement Promotion
{
    public function doSomething(...)
    {
        ...
    }
}