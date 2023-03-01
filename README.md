## SOSD 真实世界数据集

本仓库包含了[SOSD（Search on Sorted Dataset）](https://arxiv.org/pdf/1911.13014.pdf)中的几个重要真实世界数据集。SOSD常作为[Learned Index](https://arxiv.org/pdf/1712.01208.pdf)的测试基准。

由于原[SOSD仓库](https://github.com/learnedsystems/SOSD)real word traces的下载脚本下载过程总是会中断，不得不采用[Harvard Dataverse](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/JGVF9A)的数据源。Harvard数据源下载下来之后并不是直接可用的数据，本仓库的数据集为处理之后可读的数据集。

数据集下载链接：[Download Link]()

数据集名称:
- books_200M_uint32
- fb_200M_uint64
- osm_cellids_200M_uint64
- wiki_ts_200M_uint64

每个数据集有（1+200M）行，第一行是该数据集的总记录数（总行数，即200000000），接下去的200M行，每行为一个数据。

注：
- Harvard原始数据处理流程：原始数据为zst格式压缩文件，使用`zstd -d xxx.zst`解压得到二进制文件xxx，再用如下代码解析得到仓库中的数据集：
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

std::vector<ll> load_data(const std::string &filename) {
    /* Open file. */
    std::ifstream in(filename, std::ios::binary);
    if (!in.is_open())
        exit(EXIT_FAILURE);

    /* Read number of keys. */
    uint64_t n_keys;
    in.read(reinterpret_cast<char*>(&n_keys), sizeof(uint64_t));

    /* Initialize vector. */
    std::vector<ll> data;
    data.resize(n_keys);

    /* Read keys. */
    in.read(reinterpret_cast<char*>(data.data()), n_keys * sizeof(ll));
    in.close();

    return data;
}

int main() {
    // TODO: input the dataset name here
    vector<ll> vec = load_data("./wiki_ts_200M_uint64");
    cout << vec.size() << endl;
    for (ll item : vec) {
        cout << item << "\n";
    }
    return 0;
}
```

后续还会再添加更多的real world trace，供learned index方向的researchers使用，敬请期待！
