## 条款03：尽可能使用const
&emsp;&emsp;如果被指变量是常量，可以将关键字const放在类型之前，也可以放在类型之后、星号之前。这两种写法的意义相同，所以下列两个函数接受的参数类型是一样的：
```language
void f1(const Widget* pw);  //f1获得一个指针，指向一个常量的Widget对象。 
void f2(Widget const* pw);  //f2也是。
```

&emsp;&emsp;STL迭代器以指针为根据塑模出来，所以迭代器的作用就像个T*指针。声明迭代器为const就像声明指针为const一样（即声明一个T *const指针），表示这个迭代器不得指向不同的东西，但是它所指对象的值是可以改变的。如果你希望迭代器所指向的东西不可以被改动，可以使用const_iterator:
```language
std::vector<int> vec;
const std::vector<int>::iterator iter = vec.begin();
*iter = 10;  // ok:
++iter;      // error: iter is const
std::vector<int>::iterator cIter = vec.begin();
*cIter = 10;  // error: *cIter is const, cannot change
++cIter;	  // ok:
```

### const成员函数
&emsp;&emsp;将const用于成员函数的目的，是为了确认该成员函数可作用于const对象身上，同时该函数也可以作用于非const对象身上，无论类类型对象是不是const对象，都不可以在const成员函数中改变类数据成员。const成员函数中的const有两层含义：可以应用于const对象；不可改变类的数据成员。const对象不可以调用非const成员函数。
&emsp;&emsp;如果想在const成员函数中改变数据成员，一般情况下编译器会报错。你可以在数据成员类型名前添加关键字“mutable”，可以在const成员函数中改变被声明为mutable的数据成员。比如：
```language
class CTextBlock{
public:
	...
    std::size_t length() const;
private:
	char *pText;
    mutable std::size_t textLength;
    mutable bool lengthIsValid;
};
std::size_t CTextBlock::length() const
{
	if (!lengthIsValid){
    	textLength = std::strlen(pText);
        lengthIsValid = true;
    }
    
    return textLength;
}
```