# Builder Design Pattern
Separate the construction of a complex object from its representation so that the same construction process can create different objects representations

## Problem Statement
We want to construct a complex object, however we do not want to have a complex constructor member or one that would need many arguments

![Builder](http://www.plantuml.com/plantuml/svg/BOv1he9G34Ndh8A_0VhLcIEYBi0Tc0BDeycZaFPMjwy6HtToSyxf0-p8fJPGL6algNKIR-yCY5lJ_AcDDITfPs2BCv7pgokAEOSCyN4rYA4RruL2hSo5q_bvEFqVwIQXnNwae8K3udHrOUXgN6dOPOzjltJRjwCNcpxx1W00)

## Code

```
#include <iostream>
#include <memory>
using namespace std;

class Pizza {
  string crust_;
  string sauce_;
  string toppings_;
public:
  void setCrust(string crust) { crust_ = crust; }
  void setSauce(string sauce) { sauce_ = sauce; }
  void setToppings(string toppings) { toppings_ = toppings; }
  void open() {
    cout << "Pizza - Crust: " << crust_ 
         << ", Sauce: " << sauce_ 
         << ", Toppings: " << toppings_ << endl; }
};

class PizzaBuilder {
protected:
  unique_ptr<Pizza> pizza_;
public:
  void makePizza() { pizza_ = make_unique<Pizza>(); }
  Pizza* getPizza() { return pizza_.release(); }
  
  virtual void buildCrust() = 0;
  virtual void buildSauce() = 0;
  virtual void buildToppings() = 0;
};

class ItalianPizzaBuilder : public PizzaBuilder {
  void buildCrust() override { pizza_->setCrust("Italian"); }
  void buildSauce() override { pizza_->setSauce("Hot Chilly"); }
  void buildToppings() override { pizza_->setToppings("Olives, Jalpeno"); }
};

class HawaiianPizzaBuilder : public PizzaBuilder {
  void buildCrust() override { pizza_->setCrust("Normal"); }
  void buildSauce() override { pizza_->setSauce("Sweet Onion"); }
  void buildToppings() override { pizza_->setToppings("Mushrooms, Chicken"); }
};

class Cook {
  PizzaBuilder* pizzaBuilder_;
public:
  void openPizza() const { pizzaBuilder_->getPizza()->open(); }
  void preparePizza(PizzaBuilder* builder) {
    pizzaBuilder_ = builder;

    pizzaBuilder_->makePizza();

    pizzaBuilder_->buildCrust();
    pizzaBuilder_->buildSauce();
    pizzaBuilder_->buildToppings();
  }
};

int main() {
  Cook cook;
  ItalianPizzaBuilder italianPizzaBuilder;
  cook.preparePizza(&italianPizzaBuilder);
  cook.openPizza();
  
  HawaiianPizzaBuilder hawaiianPizzaBuilder;
  cook.preparePizza(&hawaiianPizzaBuilder);
  cook.openPizza();
  return 0;
}
```
