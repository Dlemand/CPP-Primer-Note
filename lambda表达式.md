C++11的lambda表达式是一种允许内联函数的特性，它可以用于不需要重用和命名的代码片段。lambda表达式的一般形式是：\
```c++
[capture clause] (parameters) mutable -> return-type { function body }
```
[captureclause]：捕捉列表。该列表总是出现在lambda函数的**开始位置[必须]**，编译器根据[]来判断接下来的代码是否为lambda函数，捕捉列表能够**捕捉上下文中的变量供lambda函数使用**。\
(parameters)：参数列表。与普通函数的参数列表一致，如果不需要参数传递，则**可以连同()一起省略**。\
mutable：默认情况下，lambda函数总是一个const函数，mutable可以取消其常量性。使用该修饰符时，参数列表不可省略（即使参数为空）。\
->return-type：**返回值类型**。用追踪返回类型形式声明函数的返回值类型，没有返回值时此部分可以省略。返回值类型明确情况下，也可省略，由编译器对返回类型进行推导。
{statement}：函数体。在该函数体内，除了可以使用其参数外，还可以使用所有捕获到的变量。\
举例：
```c++
bool StaticIMUInit::TryInit() {

    // 计算均值和方差
    Vec3d mean_gyro, mean_acce;
    math::ComputeMeanAndCovDiag(init_imu_deque_, mean_gyro, cov_gyro_, [](const IMU& imu) { return imu.gyro_; });
    math::ComputeMeanAndCovDiag(init_imu_deque_, mean_acce, cov_acce_, [this](const IMU& imu) { return imu.acce_; });

//其中
template <typename C, typename D, typename Getter>
void ComputeMeanAndCovDiag(const C& data, D& mean, D& cov_diag, Getter&& getter) {//getter就是 [](const IMU& imu) { return imu.gyro_; }这个lamba函数
    size_t len = data.size();
    assert(len > 1);
    mean = std::accumulate(data.begin(), data.end(), D::Zero().eval(),
                           [&getter](const D& sum, const auto& data) -> D { return sum + getter(data); }) / len;
}
```
其中涉及到对标准库中```std::accumulate```的重写，相当于在accumulate的第四个位置重写了一个类，本来labma就可以看做一个匿名类。

这样子写代码确实简洁，要是我来写那么就不会传递lamba，直接对deque中数据遍历了，<u>但是这样是否能够兼容不同容器呢？</u>。唯一可以确定的是写起来会更复杂。这也是stl的好用所在吧、
##### 下边是一些注意事项
```c++
template<typename InputIterator, typename T>
T accumulate(InputIterator first, InputIterator last, T initialValue);
```
求和的数据类型为模板T，就是accumlate传入的第三个参数的类型，所以如果不加注意的话，可能会让编译器产生错误的类型转换。
例如：
注意sum1和sum2的区别
```c++
std::vector<double> doubles = { 1.5, 2, 3.5 };
double sum1 = std::accumulate(begin(doubles), end(doubles), 0.);
double sum2 = std::accumulate(begin(doubles), end(doubles), 0);



```
