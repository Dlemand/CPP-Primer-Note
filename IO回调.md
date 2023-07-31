```c++
class TxtIO {
   public:
    TxtIO(const std::string &file_path) : fin(file_path) {}

    /// 定义回调函数
    using IMUProcessFuncType = std::function<void(const IMU &)>;//IMU为数据结构
    using OdomProcessFuncType = std::function<void(const Odom &)>;
    using GNSSProcessFuncType = std::function<void(const GNSS &)>;

    TxtIO &SetIMUProcessFunc(IMUProcessFuncType imu_proc) {
        imu_proc_ = std::move(imu_proc);
        return *this;
    }

    TxtIO &SetOdomProcessFunc(OdomProcessFuncType odom_proc) {
        odom_proc_ = std::move(odom_proc);
        return *this;
    }

    TxtIO &SetGNSSProcessFunc(GNSSProcessFuncType gnss_proc) {
        gnss_proc_ = std::move(gnss_proc);
        return *this; //返回TxtIO的引用 ，如此可以实现a().b().c()的连续调用
    }

    // 遍历文件内容，调用回调函数
    void Go();

   private:
    std::ifstream fin;
    IMUProcessFuncType imu_proc_;
    OdomProcessFuncType odom_proc_;
    GNSSProcessFuncType gnss_proc_;
};
```
1. std::function函数：
模板函数
主要解决的问题： 原文：不同给的可调用对象共享同一种调用形式（返回类型和调用实参），（可以用map存储函数指针），但是mod和divide不是函数指针因此构建不全。\
```c++
function<T> f;
function<T> f(cullptr);
function<T> f(obj);
```
```std::function<void(const GNSS &)>;```该函数创建function：接受GNSS 返回void

2. std::move();
右值引用：用&&表示 （右值只能绑定在一个要销毁的对象），
常规引用是左值引用。\
对move必须使用std::move 不能够using namespace std
```c++
int i = 42;
int &r = i;
int &&rr = i; //错误，右边是左值
int &&rr = i * 42; //正确，等号右边是右值
int & rr = i * 42; //错误

//可以使用std::move
int &&rr = std::move(r);
//那么从此之后只能对变量 r 赋值或者销毁。
```
