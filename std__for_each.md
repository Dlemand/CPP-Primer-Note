```c++
template <int dim>
bool GridNN<dim>::SetPointCloud(CloudPtr cloud) {
    std::vector<size_t> index(cloud->size());//点云大小，空
    std::for_each(index.begin(), index.end(), [idx = 0](size_t& i) mutable { i = idx++; });//功能为给index填上数字

    std::for_each(index.begin(), index.end(), [&cloud, this](const size_t& idx) {
        auto pt = cloud->points[idx];
        auto key = Pos2Grid(ToEigen<float, dim>(pt));
        if (grids_.find(key) == grids_.end()) {
            grids_.insert({key, {idx}});
        } else {
            grids_[key].emplace_back(idx);
        }
    });

    cloud_ = cloud;
    LOG(INFO) << "grids: " << grids_.size();
    return true;
}
```
分析
```c++
std::for_each(index.begin(), index.end(), [idx = 0](size_t& i) mutable { i = idx++; });
```
第三个元素中接受for_each所遍历的数据类型（begin（）指针所指数据类型），在代码中vector初始为0,因此每个接受的每个i都为0, \
[idx=0]为lamba表达式声明的可用数据，**capture的数据类型可以被编译器自动推导** \
mutable 是因为默认情况下，**lambda函数总是一个const函数**，mutable可以取消其常量性。使用该修饰符时，参数列表不可省略（即使参数为空）。\