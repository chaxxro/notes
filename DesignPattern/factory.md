# 工厂模式

工厂模式分为：简单工厂、工厂方法和抽象工厂三种，其中简单工厂、工厂方法原理比较简单也在实际中用的比较多

当创建逻辑比较复杂，是一个大工程的时候，我们就考虑使用工厂模式，封装对象的创建过程，将对象的创建和使用相分离

- 类似规则配置解析的例子，代码中存在 if-else 分支判断，动态地根据不同的类型创建不同的对象。针对这种情况，我们就考虑使用工厂模式，将这一大坨 if-else 创建对象的代码抽离出来，放到工厂类中

- 尽管我们不需要根据不同的类型创建不同的对象，但是单个对象本身的创建过程比较复杂，比如前面提到的要组合其他类对象，做各种初始化操作将对象的创建过程封装到工厂类中

## 简单工厂

- 简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类

- 工厂类封装了创建具体产品对象的函数，客户端可以免除直接创建产品对象的责任，而仅仅消费产品

- 客户端无须知道所创建的具体产品类的类名，只需要知道具体产品类所对应的参数即可

- 扩展性非常差，新增产品的时候，需要去修改工厂类，在产品类型较多时，有可能造成工厂逻辑过于复杂，不利于系统的扩展和维护

```cpp
/*
工厂类：工厂模式的核心类，会定义一个用于创建指定的具体实例对象的接口
抽象产品类：是具体产品类的继承的父类或实现的接口
具体产品类：工厂类所创建的对象就是此具体产品实例
*/
// 鞋子抽象类
class Shoes {
public:
    virtual ~Shoes() {}
    virtual void Show() = 0;
};

// 耐克鞋子
class NiKeShoes : public Shoes {
public:
    void Show() {}
};

// 阿迪达斯鞋子
class AdidasShoes : public Shoes {
public:
    void Show() {}
};

// 李宁鞋子
class LiNingShoes : public Shoes {
public:
    void Show() {}
};

enum SHOES_TYPE {
    NIKE,
    LINING,
    ADIDAS
};

// 总鞋厂
class ShoesFactory {
public:
    // 根据鞋子类型创建对应的鞋子对象
    Shoes *CreateShoes(SHOES_TYPE type) {
        switch (type) {
        case NIKE:
            return new NiKeShoes();
            break;
        case LINING:
            return new LiNingShoes();
            break;
        case ADIDAS:
            return new AdidasShoes();
            break;
        default:
            return NULL;
            break;
        }
    }
};

int main() {
    ShoesFactory factory;
    auto shoe = factory.CreateShoes(NIKE);
    return 0;
}
```

## 工厂方法

- 工厂方法模式抽象出了工厂类，提供创建具体产品的接口，交由子类去实现，使得工厂方法模式可以允许系统在不修改工厂角色的情况下引进新产品

- 工厂方法模式的应用并不只是为了封装具体产品对象的创建，而是要把具体产品对象的创建放到具体工厂类实现，每新增一个产品，就需要增加一个对应的产品的具体工厂类。相比简单工厂模式而言，工厂方法模式需要更多的类定义


```cpp
/*
抽象工厂类：工厂方法模式的核心类，提供创建具体产品的接口，由具体工厂类实现
具体工厂类：继承于抽象工厂，实现创建对应具体产品对象的方式
抽象产品类：它是具体产品继承的父类（基类）
具体产品类：具体工厂所创建的对象，就是此类
*/
// 总鞋厂
class ShoesFactory {
public:
    virtual Shoes *CreateShoes() = 0;
    virtual ~ShoesFactory() {}
};

// 耐克生产者/生产链
class NiKeProducer : public ShoesFactory {
public:
    Shoes *CreateShoes() {
        return new NiKeShoes();
    }
};

// 阿迪达斯生产者/生产链
class AdidasProducer : public ShoesFactory {
public:
    Shoes *CreateShoes() {
        return new AdidasShoes();
    }
};

// 李宁生产者/生产链
class LiNingProducer : public ShoesFactory {
public:
    Shoes *CreateShoes() {
        return new LiNingShoes();
    }
};

int main() {
    LiNingProducer factory ;
    auto shoe = factory.CreateShoes();
    return 0;
}
```

## 抽象工厂

- 在工厂方法模式中具体工厂负责生产具体的产品，每一个具体工厂对应一种具体产品，工厂方法也具有唯一性

- 当系统所提供的工厂所需生产的具体产品并不是一个简单的对象，而是多个位于不同产品等级结构中属于不同类型的具体产品时需要使用抽象工厂模式

- 当一个工厂等级结构可以创建出分属于不同产品等级结构的一个产品族中的所有对象时，抽象工厂模式比工厂方法模式更为简单、有效率

```cpp
/*
抽象工厂类：工厂方法模式的核心类，提供创建具体产品的接口，由具体工厂类实现
具体工厂类：继承于抽象工厂，实现创建对应具体产品对象的方式
抽象产品类：它是具体产品继承的父类（基类）
具体产品类：具体工厂所创建的对象，就是此类
*/
// 基类 衣服
class Clothe {
public:
    virtual void Show() = 0;
    virtual ~Clothe() {}
};

// 耐克衣服
class NiKeClothe : public Clothe {
public:
    void Show() {}
};

// 基类 鞋子
class Shoes {
public:
    virtual void Show() = 0;
    virtual ~Shoes() {}
};

// 耐克鞋子
class NiKeShoes : public Shoes {
public:
    void Show() {}
};

// 总厂
class Factory {
public:
    virtual Shoes *CreateShoes() = 0;
	virtual Clothe *CreateClothe() = 0;
    virtual ~Factory() {}
};

// 耐克生产者/生产链
class NiKeProducer : public Factory {
public:
    Shoes *CreateShoes() {
        return new NiKeShoes();
    }
	
	Clothe *CreateClothe() {
        return new NiKeClothe();
    }
};
```