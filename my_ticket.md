# Билет 30

## Пример 1
```cpp
void foo(vector<int> foo_data) { sort(foo_data.begin(), foo_data.end(); .. };

int main() {
    vector<int> data = {1, 2, 3, 4, 5};
    foo(data);  // Тут скопируем, вектор ещё нужен.
    data.emplace_back(6);  // {1, 2, 3, 4, 5, 6}
    foo(std::move(data));  // Разрешили не копировать.
    data.clear();  // Вектор мог остаться непустым, не определено moved-from.
}
```

## Пример 2
Если объект можно копировать, то обычно rvalue ссылки не нужны. Если же вы в данном случае из написали, то возможно вы нарушили ожидания у большинства C++ программистов.
```cpp
void foo(Foo &&x);
Foo bar;
foo(bar);             // Не компилируется.
foo(Foo(bar));        // Надо явно копировать, но так не принято.
foo(std::move(bar));  // Явно мувать можно всегда.
```

## Пример 3

```cpp
void createCodesTable(const std::unique_ptr<HuffmanNode>& node) noexcept ;
```
```cpp
Node(Node left_, Node right_)
    : left(make_unique<Node>(std::move(left_))), .... {}
```
Конструктор от `unique_ptr` появляется только с целью оптимизиции:
```cpp
Node(unique_ptr<Node> left_, unique_ptr<Node> right_)
    : left(std::move(left_)), right(std::move(right_)) {}
```

## Пример 4
```cpp
struct string_view {  // C++17
    const char *data; size_t length;
    string_view substr(size_t pos = 0, size_t count = npos) const;
    ...
};
```

```cpp
template<typename T>
struct span {  // C++20
    const T *data; size_t length; 
    ...
}
```

## Пример 5
```cpp
struct SearchTreeNode {
    std::unique_ptr<SearchTreeNode> left, right;
    int key; std::string value;
};
```

## Пример 6
```cpp 
{
    std::unique_ptr<SomeData> ptr = ....;
    SomeData *ptr2 = ptr.get();
}  // unique_ptr сам вызовет delete
```

```cpp
{
    std::unique_ptr<SomeData> ptr = ....;
    SomeData *ptr2 = ptr.release();
}  // утечка
```



