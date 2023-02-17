# 建造者模式

假设有个类，有多个成员变量是可配置项，构造这个类的对象有一下几个方法：

1. 成员变量放入构造函数，但会造成构造函数复杂，且有参数传递错误的可能

2. 可以把初始化时必要的变量放入构造函数，其他参数通过 `set()` 函数来设置，让使用者自主选择填写或不填写，但没有办法做参数之间的检验，同时暴露 `set()` 函数还会导致参数可能在初始化后再次被修改

3. 将参数的构造放入一个外部的建造者对象里，建造者返回一个已经构建好的完整对象

建造者模式可以更精细的控制构建过程，建造者模式先一步一步设置建造者的变量，然后再一次性地创建对象，让对象一直处于有效状态

用户只通过指定复杂对象的类型和内容就可以构建它们，用户不需要知道内部的具体构建细节

```cpp
/*
Builder 抽象建造者，为创建一个 Product 对象的各个部件指定的抽象接口
ConcreteBuilder 具体建造者
Director 指挥者，按照一定顺序完成产品对象的构建
Product 产品角色
*/

// 要构建的对象
class Graphic {
public:
    Graphic() {}

    // 常变属性的设置接口
    void SetShape(string strShape) {
        m_strShape = strShape;
    }

    void SetColor(string strColor) {
        m_strColor = strColor;
    }

    // 图形的验证方法
    void Show() {
        cout << m_strColor << m_strShape << endl;
    }

private:
    string m_strShape;
    string m_strColor;
};

//建造者抽象类
class Builder {
public:
    Builder(): m_pGraphic(NULL) {}

    // 创建空白图形
    void CreateGraphic() {
        if (NULL == m_pGraphic) {
            m_pGraphic = new Graphic();
        }
    }

    // 获取描绘完成的图形
    Graphic* GetGraphic() {
        return m_pGraphic;
    }

    // 留给派生类实现的描绘过程
    virtual void DrawShape() = 0;
    virtual void DrawColor() = 0;

protected:
    Graphic* m_pGraphic;
};

class GreenCircleBuilder: public Builder {
public:
    // 根据类的功能实现描绘过程
    void DrawShape() {
        if (NULL != m_pGraphic) {
            m_pGraphic->SetShape("Circle");
        }
    }

    void DrawColor() {
        if (NULL != m_pGraphic) {
            m_pGraphic->SetColor("Green");
        }
    }
};

class RedRectangleBuilder: public Builder {
public:
    // 根据类的功能实现描绘过程
    void DrawShape() {
        if (NULL != m_pGraphic) {
            m_pGraphic->SetShape("Rectangle");
        }
    }

    void DrawColor() {
        if (NULL != m_pGraphic) {
            m_pGraphic->SetColor("Red");
        }
    }
};

class BlueTriangleBuilder: public Builder {
public:
    // 根据类的功能实现描绘过程
    void DrawShape() {
        if (NULL != m_pGraphic) {
            m_pGraphic->SetShape("Triangle");
        }
    }

    void DrawColor() {
        if (NULL != m_pGraphic) {
            m_pGraphic->SetColor("Blue");
        }
    }
};

// 导演类
class Director {
public:
    Director(): m_pBuilder(NULL) {}

    // 根据需求设置对应的建造者
    void SetBuilder(Builder& pBuilder) {
        m_pBuilder = &pBuilder;
    }

    // 通过建造者获得描绘完成的图形
    Graphic* DrawGraphic() {
        if (NULL == m_pBuilder) {
            return NULL;
        }
        //建造过程为不变的因素
        m_pBuilder->CreateGraphic();
        m_pBuilder->DrawShape();
        m_pBuilder->DrawColor();
        return m_pBuilder->GetGraphic();
    }

private:
    Builder* m_pBuilder;
};

int main()
{
    Director MyDirector;
    // 根据导演的不同需求，分别设置不同的建造者，获得满足需求的图形
    GreenCircleBuilder BuilderGC; // 绿色圆形的建造者
    MyDirector.SetBuilder(BuilderGC);
    Graphic* pGraphicGC = MyDirector.DrawGraphic(); // 导演使用建造者画图
    pGraphicGC->Show();

    RedRectangleBuilder BuilderRB; // 红色矩形的建造者
    MyDirector.SetBuilder(BuilderRB);
    Graphic* pGraphicRB = MyDirector.DrawGraphic(); // 导演使用建造者画图
    pGraphicRB->Show();

    BlueTriangleBuilder BuilderBT; // 蓝色三角的建造者
    MyDirector.SetBuilder(BuilderBT);
    Graphic* pGraphicBT = MyDirector.DrawGraphic(); // 导演使用建造者画图
    pGraphicBT->Show();

    // 资源回收
    if (NULL == pGraphicGC) {
        delete pGraphicGC;
        pGraphicGC = NULL;
    }
    if (NULL == pGraphicRB) {
        delete pGraphicRB;
        pGraphicRB = NULL;
    }
    if (NULL == pGraphicBT) {
        delete pGraphicBT;
        pGraphicBT = NULL;
    }
    return 0;
}
```

工厂模式是用来创建不同但是相关类型的对象（继承同一父类或者接口的一组子类），由给定的参数来决定创建哪种类型的对象

建造者模式是用来创建一种类型的复杂对象，通过设置不同的可选参数，定制化地创建不同的对象