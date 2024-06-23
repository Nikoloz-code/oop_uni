# _**std::variant<>: გამოყენება, მოტივაცია, მაგალითები.**_

std::variant არის თარგიანი კლასი, რომელიც გამოიყენება რამოდენიმე მონაცემთა ტიპის ერთ ჯგუფად შენახვისთვის. variant-ის ობიექტი თავის სპეციალიზაციაში მითითებული ტიპებიდან მხოლოდ ერთის წარმომადგენელს ინახავს დროის ყოველ კონკრეტულ მომენტში. გამოყენების მაგალითი:
```cpp
#include<iostream>
#include<string>
#include<variant>

int main()
{
    std::variant<int,char,std::string> var;
    
    var = 'x';
    
    std::cout << std::get<char>(var) << '\n'; //დაბეჭდავს x-ს.
    
    //ამ ფუნქციით შეგვიძლია ვნახოთ თუ რომელი მონაცემთა ტიპს იყენებს variant-ი.
    // true (1) ნიშნავს რომ მონაცემთა ტიპი აქვიტირებულია, თუ false (0) მაშინ არ არის აქვტივირებული.
    
    std::cout << std::holds_alternative<char>(var) << '\n'; // ეს დაბეჭდავს 1-ს, რადგან ეხლა var-ში არის ჩაწერილი 'x'.
    std::cout << std::holds_alternative<int>(var) << '\n'; //ეს დაბეჭდავს 0-ს.
    std::cout << std::holds_alternative<std::string>(var) << '\n'; //ეს დაბეჭდავს 0-ს.
    
    ////////////////////////
    
    var = 12;
    
    std::cout << std::get<int>(var) << '\n'; //დაბეჭდავს 12-ს.
    
    std::cout << std::holds_alternative<char>(var) << '\n'; //ეს დაბეჭდავს 0-ს.
    std::cout << std::holds_alternative<int>(var) << '\n'; // ეს დაბეჭდავს 1-ს რადგან ეხლა var-ში არის ჩაწერილი 12.
    std::cout << std::holds_alternative<std::string>(var) << '\n'; //ეს დაბეჭდავს 0-ს.
}
```

ჩვენ შეგვიძლია variant გამოვიყენოთ პოლიმორფიზმში.

ხშირ შემთხვევაში, როდესაც ესმით შესრულების დროული (runtime) პოლიმორფიზმი, მაშინვე წარმოიდგენენ ვირტუალურ ფუნქციებს. თქვენ გამოვაცხადებთ ვირტუალურ ფუნქციას ფუძე კლასში და შემდეგ გადაწერთ მას წარმოებულ კლასებში. როდესაც ასეთ ფუნქციას იძახებთ ფუძე კლასის პოინტერით ან რეფერენსით, მაშინ კომპილერი ამ ტექნიკას განახორციელებს ვირტუალური ცხრილებით (v- ცხრილები). თითოეული კლასი, რომელსაც აქვს ვირტუალური მეთოდი, შეიცავს დამატებით ცხრილს, რომელიც მიუთითებს წევრის ფუნქციების მისამართებზე. ვირტუალურ მეთოდზე ყოველი გამოძახების წინ კომპილერმა უნდა ნახოს v- ცხრილი და იპოვოს წარმოებული ფუნქციის მისამართი.

```cpp
#include <iostream>
class Base
{
  public:
  virtual ~Base() = default;
  virtual void PrintName() const {
  std::cout << "calling Bases!\n";
  }
};

class Derived : public Base {
  public:
  void PrintName() const override {
  std::cout << "calling Derived!\n";
  }
};

class ExtraDerived : public Base {
public:
  void PrintName() const override {
  std::cout << "calling ExtraDerived!\n";
  }
};

int main()
{
    std::unique_ptr<Base> pObject = std::make_unique<Derived>();
    pObject->PrintName();
}
```

ამ მეთოდს აქვს თავისი ნაკლოვანებებები:
- ვირტუალური მეთოდი უნდა განისაზღვროს გამოძახებამდე, რაც იწვევს ზედნადებ ხარჯს წარმადობაზე კომპილერები ცდილობენ მაქსიმალურად მოახდინონ გამოძახების დევირტუალიზაცია, მაგრამ უმეტეს შემთხვევაში ეს შეუძლებელია.
- ვინაიდან მეთოდის გამოსაძახებლად გჭირდებათ პოინტერი, ჩვეულებრივ ეს ნიშნავს მეხსიერების დინამიკურ გამოყოფას, რაც წარმადობას უფრო ხარჯიანს ხდის.
- თუ გსურთ ახალი ვირტუალური მეთოდის დამატება, მაშინ უნდა გაიაროთ ფუძე და წარმოებული კლასები და ყველგან დაამატოთ ეს ახალი ფუნქცია
___
# ეხლა ვნახოთ თუ როგორ შეგვიძლია გამოვიყენოთ std::variant პოლიმორფიზმისთვის.

std::variant-ით, რომელიც ხელმისაწვდომია C++17 –დან, ახლა შეგიძლიათ უსაფრთხოდ გამოიყენოთ ტიპების გაერთიანებები და შეინახოთ მრავალი სხვადასხვა ტიპი ერთ ობიექტში. ფუძე კლასის მაჩვენებლის ნაცვლად, std::variant-ს შეუძლია შეინახოს ყველა ”წარმოებული” კლასი.
```cpp
#include <iostream>
#include <variant>

class Derived{
public:
  void PrintName() const {
  std::cout << "calling Derived!\n";
  }
};
class ExtraDerived {
public:
  void PrintName() const {
  std::cout << "calling ExtraDerived!\n";
  }
};

int main()
{
  std::variant<Derived, ExtraDerived> var; //ქმნის ვარიანტს რომელიც ინახავს ან Derived ან ExtraDerived-ს.
  
  struct CallPrintName {
    void operator()(const Derived& d) { d.PrintName(); } //თუ Derived, გამოიძახე ეს ფუნქცია.
    void operator()(const ExtraDerived& ed) { ed.PrintName(); } //თუ ExtraDerived, გამოიძახე ეს ფუნქცია.
  };
  
  std::visit(CallPrintName{}, var); //ის ფუნქცია იმუშავებს, რის ობიექტს visit ფუნქცია ნახავს.
}
```

std::variant -ის პოლიმორფიზმის უპირატესობები:
- მნიშვნელობის სემანტიკა, არაა მეხსიერების დინამიკური გამოყოფა;
- ახალი ”მეთოდის” დამატება მარტივია, თქვენ უნდა შექმნათ ახალი გამოძახებადი სტრუქტურა. არ არის საჭირო კლასების იმპლემენტაციის შეცვლა;
- ფუძე კლასი არ არის საჭირო, კლასები შეიძლება არ იყოს ერთმანეთთან დაკავშირებული;
