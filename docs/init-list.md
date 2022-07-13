# 数组的初始化列表的实现

数组的列表初始化是类似如下的语法：

``` c
int a[10][10] = {{1, 5}, {2, 3}, 4};
```

具体实现工具如下：

``` cpp
struct init_tree_node {
    std::shared_ptr<std::vector<std::size_t>>init_vals;
    std::shared_ptr<init_tree_node> prev;
    std::vector<std::shared_ptr<init_tree_node>> next;

    init_tree_node() {
        init_vals = std::make_shared<std::vector<std::size_t>>();
    }
};

class gen_quads {

// ...

    std::shared_ptr<init_tree_node> root{nullptr};
    std::shared_ptr<init_tree_node> walker{nullptr};

// ...

    void init_init_node();
    void enter_init_node();
    void exit_init_node();
    void add_init_node_v();
};

void gen_quads::init_init_node() {
    walker = root = std::make_shared<init_tree_node>();
    root->prev = nullptr;
}

void gen_quads::enter_init_node() {
    walker->next.push_back(std::make_shared<init_tree_node>());
    walker->next.back()->prev = walker;
    walker = walker->next.back();
}

void gen_quads::exit_init_node() {
    walker = walker->prev;
}

void gen_quads::add_init_node_v() {
    walker->init_vals->push_back(semi_stk.top());
    semi_stk.pop();
}
```

<!-- TODO: 思路 -->