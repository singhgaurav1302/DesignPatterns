## Factory Design Pattern


#### Factory Method
A utility class that creates an instance of a class from a family of derived classes
Separate the usage of object from construction of object

#### Problem
We want to decide at run time what object is to be created based on some configuration or application parameter. When we write the code, we do not know what class should be instantiated


#### UML
![factory_method_uml](http://www.plantuml.com/plantuml/svg/BSux3i8m38VnlQSe5wXvR4mT41iIDq0cDKcangdiVq3SdWTCt_ZxHWSRMfaxPCpI7pcWshC_2LATkcwLDSnjaZu1Y--9Z1z3p4ZjsbmiL8KeXb0BUTQO8ZVZ-sQttj91F4SzCo2cJeKTOlG7wFRhfNoXOVaiZABJkoy0)


#### Code

```
#include <iostream>
#include <memory>

class Shape {
public:
    virtual ~Shape() = default;
    virtual void draw() = 0;
};

class Rectangle : public Shape {
public:
    void draw() override { std::cout << "Draw Rectangle\n"; }
};

class Square : public Shape {
public:
    void draw() override { std::cout << "Draw Square\n"; }
};

class Triangle : public Shape {
public:
    void draw() override { std::cout << "Draw Triangle\n"; }
};
    
class ShapeFactory {
public:
    enum class ShapeType { RECTANGLE, SQUARE, TRIANGLE };
    static std::unique_ptr<Shape> createShape(ShapeType type) {
        switch (type) {
            case ShapeType::RECTANGLE:
                return std::make_unique<Rectangle>();
            case ShapeType::SQUARE:
                return std::make_unique<Square>();
            case ShapeType::TRIANGLE:
                return std::make_unique<Square>();
        }
        return nullptr;
    }
};
    
int main()
{
    ShapeFactory::createShape(ShapeFactory::ShapeType::RECTANGLE)->draw();
    ShapeFactory::createShape(ShapeFactory::ShapeType::SQUARE)->draw();
    ShapeFactory::createShape(ShapeFactory::ShapeType::TRIANGLE)->draw();
}
```
