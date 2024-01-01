* **课件例题4-4**

  ```c++
  #include <iostream>
  #include <cmath>
  using namespace std;
  class Point {	//Point类定义
  public:
  	Point(int xx = 0, int yy = 0) {
  		x = xx;
  		y = yy;
  	}
  	Point(Point &p);
  	int getX() { return x; }
  	int getY() { return y; }
  private:
  	int x, y;
  };
  Point::Point(Point &p) {	//复制构造函数的实现
  	x = p.x;
  	y = p.y;
  	cout << "Calling the copy constructor of Point" << endl;
  }
  class Line {	//Line类的定义
  public:	//外部接口
  	Line(Point xp1, Point xp2);
  	Line(Line &l);
  	double getLen() { return len; }
  private:	//私有数据成员
  	Point p1, p2;	//Point类的对象p1,p2
  	double len;
  };
  //组合类的构造函数
  Line::Line(Point xp1, Point xp2) : p1(xp1), p2(xp2) {
  	cout << "Calling constructor of Line" << endl;
  	double x = static_cast<double>(p1.getX() - p2.getX());
  	double y = static_cast<double>(p1.getY() - p2.getY());
  	len = sqrt(x * x + y * y);
  }
  Line::Line (Line &l): p1(l.p1), p2(l.p2) {//组合类的复制构造函数
  	cout << "Calling the copy constructor of Line" << endl;
  	len = l.len;
  }
  //主函数
  int main() {
  	Point myp1(1, 1), myp2(4, 5);	//建立Point类的对象
  	Line line(myp1, myp2);	//建立Line类的对象
  	Line line2(line);	//利用复制构造函数建立一个新对象
  	cout << "The length of the line is: ";
  	cout << line.getLen() << endl;
  	cout << "The length of the line2 is: ";
  	cout << line2.getLen() << endl;
  	return 0;
  }
  ```

  ```c++
  运行结果如下：
  Calling the copy constructor of Point
  Calling the copy constructor of Point
  Calling the copy constructor of Point
  Calling the copy constructor of Point
  Calling constructor of Line
  Calling the copy constructor of Point
  Calling the copy constructor of Point
  Calling the copy constructor of Line
  The length of the line is: 5
  The length of the line2 is: 5
  ```

* **试卷二.5**

  ```c++
  5.已知下列程序的运行结果如图1，根据对象d的内存结构如图2，在程序的相应位置填空。
  class Base1 {
  public:
  	int var1;
  	void fun() { cout << "Member of Base1" << endl; }
  };
  class Base2 {
  public:
  	int var2;
  	void fun() { cout << "Member of Base2" << endl; }
  };
  class Derived : public Base1, public Base2 {
  public:
  	int var;
  	void fun() { cout << "Member of Derived" << endl; }
  };
  int main() {
  	Derived d;
  	Derived *p = &d;		
  	_d.var__= 0;   //（1）通过对象d访问Derived类成员
  	__d.fun__();    //（2）通过对象d访问Derived类成员函数
  	__p->Base2::var2__= 2;  //（3）通过指针p访问对象d的Base2基类成员
  	_p->Base1::fun_(); //（4）通过指针p访问对象d 的Base1的成员函数
  	return 0;
  }
  d.var   d.fun   p->Base2::var2   p->Base1::fun
  ```

* **实验三.3**

  ```c++
  3. 请编写测试主函数，测试各个类的内存分配情况，并根据测试结果绘制各个类的内存分配示意图。
  #include <iostream>
  using namespace std;
  class A      //基类A
  {public:
      int dataA;
  };
  class B : (virtual)  public A
  {
  public:
      int dataB;
  };
  
  class C : (virtual)  public A
  {
  public:
      int dataC;
  };
  
  class D :  public B,public C
  {
  public:
      int dataD;
  };
  //编写测试主函数，测试各个类的内存分配情况
  int main()
  {
      D *d=new D;
      d->dataA=10;
      d->dataB=100;
      d->dataC=1000;
      d->dataD=10000;
      B *b=d;
      C *c=d;
      A *fromB=(B*)d;
      A *fromC=(C*)d;
      // cout <<(int*)d<<endl;
        // A *fromC=(C*)d;
      cout<<"d的地址:"<<(int*)d<<endl;
      cout<<"b的地址:"<<(int*)b<<endl;
      cout<<"c的地址:"<<(int*)c<<endl;
      cout<<"fromB的地址:"<<(int*)fromB<<endl;
      cout<<"fromC的地址:"<<(int*)fromC<<endl;
      cout<<endl;
      D d1;
      B b1;
      C c1;
      A a;
      cout<<"D类对象的内存大小:"<<sizeof(d1)<<endl;
      cout<<"B类对象的内存大小:"<<sizeof(b1)<<endl;
      cout<<"C类对象的内存大小:"<<sizeof(c1)<<endl;
      cout<<"A类对象的内存大小:"<<sizeof(a)<<endl;
      return 0;}
  ```

  ![image-20240102015205532](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20240102015205532.png)

* **作业4.二**

  ```c++
  #include <iostream>
  using namespace std;
  class Complex; 
  Complex operator + (const Complex &c1, const Complex &c2);	//运算符+重载非成员函数
  Complex operator - (const Complex &c1, const Complex &c2);	//运算符-重载非成员函数
  Complex operator * (const Complex &c1, const Complex &c2);	//运算符*重载非成员函数
  class Complex {	//复数类定义
  public:	//外部接口
  	Complex(double r = 0.0, double i = 0.0) : real(r), imag(i) { }	//构造函数
  	friend Complex operator + (const Complex &c1, const Complex &c2);	//运算符+重载成员函数
  	friend Complex operator - (const Complex &c1, const Complex &c2);	//运算符-重载成员函数
  	friend Complex operator * (const Complex &c1, const Complex &c2);
  	void display() const;	//输出复数
  private:	//私有数据成员
  	double real;	//复数实部
  	double imag;	//复数虚部
  };
  Complex operator + (const Complex &c1, const Complex &c2){	//重载运算符函数实现
  	return Complex(c1.real + c2.real, c1.imag + c2.imag); //创建一个临时无名对象作为返回值
  }
  Complex operator - (const Complex &c1, const Complex &c2){	//重载运算符函数实现
  	return Complex(c1.real - c2.real, c1.imag - c2.imag); //创建一个临时无名对象作为返回值
  }
  Complex operator * (const Complex &c1, const Complex &c2){	//重载运算符函数实现
  	return Complex(c1.real*c2.real-c1.imag*c2.imag, c1.real*c2.imag+c1.imag*c2.real); //创建一个临时无名对象作为返回值
  }
  void Complex::display() const {
  	cout << "(" << real << ", " << imag << ")" << endl;
  }
  int main() {	//主函数
  	Complex c1(5, 4), c2(2, 10), c3;	//定义复数类的对象
  	cout << "c1 = "; c1.display();
  	cout << "c2 = "; c2.display();
  	c3 = c1 - c2;	//使用重载运算符完成复数减法
  	cout << "c3 = c1 - c2 = "; c3.display();
  	c3 = c1 + c2;	//使用重载运算符完成复数加法
  	cout << "c3 = c1 + c2 = "; c3.display();
  	c3 = c1 * c2;	//使用重载运算符完成复数乘法
  	cout << "c3 = c1 * c2 = "; c3.display();
  	return 0;
  }
  ```

  

* **试卷四.2**

  ```c++
  2.根据下面给定的类的定义和运行结果，结合游戏描述，编写一个塔防类小游戏，分别给出利用多态和利用函数重载两种方法实现该程序运行结果的源代码和运行结果截图。（10分）
  游戏描述如下：
  （1）设计一个英雄类，英雄类下派生有英雄A类，英雄B类，英雄C类…，每个类的英雄打斗时都会流血，血量会减少，如英雄A类有方法：void blood(){cout<<"A的血量减一"<<endl;}；
  （2）设计一个防御塔类，英雄进塔会遭到攻击，被攻击时会流血，血量会减少；class tower{void beat(hero *p){… …}
  （3）运行结果如下：
      A的血量减一
  B的血量减一
  C的血量减一
      
  #include <iostream>
  using namespace std;
  class A{
  public:
  	void blood()
  	{
  		cout << "A的血量减一" << endl;
  	}
  };
  class B{
  public:
  	void blood()
  	{
  		cout << "B的血量减一" << endl;
  	}
  };
  class C{
  public:
  	void blood()
  	{
  		cout << "C的血量减一" << endl;
  	}
  };
  //tower相当于防御塔类，英雄进塔会遭到攻击 
  class tower {
  public:
  	 void beat(A *p)
  		{
  			p->blood();
  		}
  
  	  void beat(B *p)
  		{
  			p->blood();
  		}
  	  void beat(C *p)
  	  {
  		  p->blood();
  	  }
  
  };
  int main()
  {
  	A a;
  	B b;
  	C c;
  	tower t;
  	t.beat(&a);//防御塔攻击a  
  	t.beat(&b);//防御塔攻击b 
  	t.beat(&c);//防御塔攻击c
  }
  ```

* **实验六**

  * 1 理解并掌握函数模板的定义和使用方法

    ```c++
    #include <iostream>
    using namespace std;
    template <typename T1, typename T2, typename T3>
    T1 add(T2 a, T3 b)
    {
        T1 sum ;
        sum = static_cast<T1>(a + b);
        return sum;
    }
    
    int main()
    {
        int x = 12;
        float y = 46.9;
        //cout << add(x, y) << endl;	//error，无法自动推导函数返回值
        cout << add<float>(x, y) << endl;	//返回值在第一个类型参数中指定
        cout << add<int, int, float>(x, y) << endl;
        return 0;
    }
    ```

    ```
    58.9
    58
    ```

    

  * 2.理解并掌握类模板的定义和使用方法

    ```c++
    #include <iostream>
    using namespace std;
    
    /**
     * 定义一个矩形类模板Rect
     * 成员函数：calcArea()、calePerimeter()
     * 数据成员：m_length、m_height
     */
    template<typename T>
    class Rect
    {
    public:
       Rect(T length,T height);
       T calcArea();
       T calePerimeter();
    public:
        T m_length;
        T m_height;
    };
    //类属性赋值
    template<typename T>
    Rect<T>::Rect(T length,T height)
    {
        m_length = length;
        m_height = height;
    }
    
    //面积方法实现
    template<typename T>
    T Rect<T>::calcArea()
    {    return m_length * m_height;	}
    
    //周长方法实现
    template<typename T>
    T Rect<T>::calePerimeter()
    {    return ( m_length + m_height) * 2;	}
    int main(void)
    {
        Rect<int> rect(3, 6);
        cout << rect.calcArea() << endl;
        cout << rect.calePerimeter() << endl;
        return 0;
    }
    ```

    ```
    18
    18
    ```

    

  * 3.理解并掌握模板类的应用和实例化方法

    ```c++
    template <class T>
    Array<T>::Array(const Array<T>& a) {
        size = a.size;
        list = new T[size];
        for (int i = 0; i < size; ++i) {
            list[i] = a.list[i];
        }
    }
    
    template <class T>
    Array<T>& Array<T>::operator=(const Array<T>& rhs) {
        if (&rhs != this) {
            if (size != rhs.size) {
                delete [] list;
                size = rhs.size;
                list = new T[size];
            }
            for (int i = 0; i < size; ++i) {
                list[i] = rhs.list[i];
            }
        }
        return *this;
    }
    
    template <class T>
    T& Array<T>::operator[](int i) {
        assert(i >= 0 && i < size);
        return list[i];
    }
    
    template <class T>
    const T& Array<T>::operator[](int i) const {
        assert(i >= 0 && i < size);
        return list[i];
    }
    
    template <class T>
    Array<T>::operator T*() {
        return list;
    }
    
    template <class T>
    Array<T>::operator const T*() const {
        return list;
    }
    
    template <class T>
    int Array<T>::getSize() const {
        return size;
    }
    
    template <class T>
    void Array<T>::resize(int sz) {
        assert(sz >= 0);
    
        if (sz == size)
            return;
    
        T* newList = new T[sz];
        int n = (sz < size) ? sz : size;
        for (int i = 0; i < n; ++i) {
            newList[i] = list[i];
        }
        delete[] list;
        list = newList;
        size = sz;
    }
    ```
  
* **实验七**
  

    * 1理解并掌握函数模板全特化的定义和使用方法

      ```c++
      template <typename T, typename U>
      void tfunc(T& a, U& b)
      {
      	std::cout << "tfunc 泛化版本函数" << std::endl;
      }
      template <>
      void tfunc(int& a, int& b)
      {
      	std::cout << "tfunc 全特化版本函数" << std::endl;
      }
      ```
    
      ```c++
      tfunc 泛化版本函数
      tfunc 全特化版本函数
      ```
    
      
    
    * 2理解并掌握类模板全特化的定义和使用方法
    
      ```c++
      template <typename T, typename U>
      class TC
      {
      public:
      	TC()
      	{
      		std::cout << "泛化版本构造函数" << std::endl;
      	}
      	void funtest()
      	{
      		std::cout << "泛化版本成员函数" << std::endl;
      	}
      };
      
      template<>
      class TC<int, int>
      {
      public:
      	TC()
      	{
      		std::cout << "TC<int, int> 全特化版本构造函数" << std::endl;
      	}
      	void funtest()
      	{
      		std::cout << "TC<int, int> 全特化版本成员函数" << std::endl;
      	}
      };
      
      template<>
      class TC<double, double>
      {
      public:
      	TC()
      	{
      		std::cout << "TC < double, double> 全特化版本构造函数" << std::endl;
      	}
      	void funtest()
      	{
      		std::cout << "TC < double, double> 全特化版本成员函数" << std::endl;
      	}
      };
      ```
    
      ```
      泛化版本构造函数
      泛化版本成员函数
      TC<int, int> 全特化版本构造函数
      TC<int, int> 全特化版本成员函数
      TC < double, double> 全特化版本构造函数
      TC < double, double> 全特化版本成员函数
      ```
    
      
    
    * 3理解并掌握类模板偏特化的定义和使用方法
    
      ```c++
      template <typename T, typename U, typename W>
      class TC2
      {
      public:
      	void funtest()
      	{
      		std::cout << "泛化版本成员函数" << std::endl;
      	}
      };
      
      template <typename U>
      class TC2<int, U, double>
      {
      public:
      	void funtest()
      	{
      		std::cout << "<int, U, double> 偏特化版本成员函数" << std::endl;
      	}
      };
      ```
    
      ```
      泛化版本成员函数
      <int, U, double> 偏特化版本成员函数
      ```
    
    * 4请根据下面程序主函数的调用情况和程序运行结果，定义一个相应的偏特化类模板，按照注释要求完善程序,给出程序源代码及程序运行结果截图。
    
      ```c++
      template <typename T>
      class TC3
      {
      public:
      	void funtest()
      	{
      		std::cout << "泛化版本成员函数" << std::endl;
      	}
      };
      
      template <typename T>
      class TC3<const T>
      {
      public:	
      	void funtest()
      	{
      		std::cout << "const T偏特化版本成员函数" << std::endl;
      	}
      };
      
      template <typename T>
      class TC3<T&>
      {
      public:
      	void funtest()
      	{
      		std::cout << "T&偏特化版本成员函数" << std::endl;
      	}
      };
      
      template <typename T>
      class TC3<T*>
      {
      public:
      	void funtest()
      	{
      		std::cout << "T *偏特化版本成员函数" << std::endl;
      	}
      };
      ```
    
      ```c++
      main中的代码：
      TC3<int> tint3;
      	tint3.funtest();
      	
      泛化版本成员函数(main中T3填个int)
      const T偏特化版本成员函数(main中T3填个const int)
      T&偏特化版本成员函数(main中T3填个int&)
      T *偏特化版本成员函数(main中T3填个int*)
      ```
    
      



* **实验八**
    * 1：根据上述square和cubic仿函数，定义一个能够实现求n次方的仿函数模板，n由仿函数参数输入。

      ```c++
      template<typename T>
      class Pow {
      private:
          int exponent;
      
      public:
          Pow(int exp) : exponent(exp) {}
      
          T operator()(T input)
          {
              return pow(input, exponent);
          }
      };
      ```
    
    * 2：仿照TEMPLATE accumulate补充traverse函数模板，traverse能够遍历容器序列并进行相应的操作。
    
      ```c++
      template<typename Iterator, typename Function>
      void traverse(Iterator begin, Iterator end, ostream& out, Function func)
      {
          for (; begin != end; ++begin) {
              out << func(*begin) << " ";
          }
      }
      
      //在main()中，对于n次方，调用traverse的方法是
          traverse(s.begin(), s.end(), cout, Pow<int>(n));
      //别的一此类推，只是动最后一个选项，即Pow<int>(n)
      ```

* **复习要点1补充1及练习**

  * 补充1

    ```c++
    （1）
       
    1.在Shape.h里面
    //增加
    class Circle{
    };
    
    2.在MainForm.cpp里面的private属性里面
    //加一个
    vector<Circle> circleVector;
    
    3.在方法：void MainForm::OnMouseUp(const MouseEventArgs& e)中if (rdoLine.Checked)的判断里面
    //改变
    	else if (...){
    		//...
    		circleVector.push_back(circle);
    	}
    4.在方法：void MainForm::OnPaint(const PaintEventArgs& e)中增加
        //改变
    	//针对圆形
    	for (int i = 0; i < circleVector.size(); i++){
    		e.Graphics.DrawCircle(Pens.Red,
    			circleVector[i]);
    	}
    ```

    ```c++
    （2）
    1.在Shape2.h里面
        class Shape{    //抽象基类
    public:
    	virtual void Draw(const Graphics& g)=0;  //纯虚函数
    	virtual ~Shape() { }   //虚析构函数
    };
    2.在Line/Rect/Circle类里面
        //实现自己的Draw，负责画自己
    	virtual void Draw(const Graphics& g){
    		g.Draw____(Pens.Red, 
    			//具体类具体加
                      );
    	}
    3.在MainForm2.cpp中的private属性里面添加
        //针对所有形状
    	vector<Shape*> shapeVector;   //必须设置为抽象基类的指针，否则无法实现多态
    4.在MainForm2.cpp中的public属性里面添加
        ~MainFrom(){
            for(int i = 0;i<shapeVector.size();++i){
                delete shapeVector[i];
            }
    5.void MainForm::OnMouseUp(const MouseEventArgs& e){
    	p2.x = e.X;
    	p2.y = e.Y;
    	if (rdoLine.Checked){
            /*
    		shapeVector.push_back(new Line(p1,p2));
            */
    	}
    	else if (rdoRect.Checked){
    		int width = abs(p2.x - p1.x);
    		int height = abs(p2.y - p1.y);
            /*
    		shapeVector.push_back(new Rect(p1, width, height));
    		*/
    	}
    	//改变
    	/*
    	else if (...){
    		shapeVector.push_back(circle);
    	}
    	*/
    	this->Refresh();
    	Form::OnMouseUp(e);
    }
    void MainForm::OnPaint(const PaintEventArgs& e){
    	//针对所有形状
    	for (int i = 0; i < shapeVector.size(); i++){
            /*
    		shapeVector[i]->Draw(e.Graphics); //多态调用，各负其责
    		
    		*/
    	}
    	//...
    	Form::OnPaint(e);
    }
    ```

    ```c++
    （3）
    问题1采用的结构化的分而治之的方法在未来面向变化时，需要不断地随着变化而更改，程序的复杂性会不断增加；
    问题2采用的面向对象的抽象的解决方法，具有更好的复用性和可扩展性，将变化所带来的影响减为最小，体现了抽象类能够隔离变化的功能
    ```

    

  * 练习

    ```c++
    阅读下列程序，按照要求完成下列题目：
    （1）如果现在工厂接到更多订单，需要增加处理器型号，如：TYPEC、TYPED等，请用工厂模式的虚函数机制重新设计该程序，以实现上述需求的变化；
    （2）如果现在工厂扩大生产规模，需要开设两个工厂，一个专门用来生产A型号的单核多核处理器，而另一个工厂专门用来生产B型号的单核多核处理器，请重新设计程序，以实现上述需求的变化。
    ```

    ```c++
    #include <iostream>
    using namespace std;
    enum CTYPE { TYPEA, TYPEB };
    class Product
    {
    public:
    	virtual void Show() = 0;
    };
    //生产A类型
    class ProductA :public Product
    {
    public:
    	void Show()
    	{
    		cout << "生产A" << endl;
    	}
    };
    //生产B类型
    class ProductB :public Product
    {
    public:
    	void Show()
    	{
    		cout << "生产B" << endl;
    	}
    };
    class Factory
    {
    public:
    	Product* Create(enum CTYPE type)
    	{
    		if (type == TYPEA)
    			return new ProductA();
    		else if (type == TYPEB)
    			return new ProductB();
    		else
    			return NULL;
    	}
    };
    int main()
    {
    	Factory p;
    	p.Create(TYPEA)->Show();
    	p.Create(TYPEB)->Show();
    }
    ```

    ```
    生产A
    生产B
    ```

    