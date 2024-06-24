# _**unique_ptr<>-ის გამოყენება ვირტუალურ ფუნქციებთან. ვირტუალური დესტრუქტორები.**_

ნედლი პოინტერების შემთხვევაში ფუძე-კლასის პოინტერი ინიჭებს წარმოებული კლასის პოინტერს და ვირტუალური ფუნქციების საშუალებით არკვევს თუ რომელი კლასის ფუნქციები გაუშვას. სწორედ ამაზე არის დაფუძნებულია პოლიმორფიზმი:
```cpp
int main()
{
  Mammal** p = new Mammal*[3];

  p[0] = new Cat("Cicqna");
  p[1] = new Dog("Cuga");
  p[2] = new Dog("Muria");

  for (int i = 0; i < 3; i++)
    p[i]->Speak();
  for (int i = 0; i<3; i++)
    delete p[i];
  delete[] p;
}
```
> [!IMPORTANT]
> ეს კოდი დაბეჭდავს: Cat Cicqna: Meow! Dog Cuga: Woof! Dog Muria: Woof!

ჭკვიანი პოინტერების შექმნისას ნედლი პოინტერების ასეთ ძლიერ მხარეს უყურადღებოდ არავინ დატოვებდა. ```ფუძე-კლასის ჭკვიან პოინტერს შეუძლია მიინიჭოს წარმოებული კლასის ჭკვიანი პოინტერი.``` ვირტუალობის მექანიზმი ძალაში რჩება. შედეგად, შეგვიძლია ბოლო მაგალითის პროგრამა-დრაივერი ასე შევცვალოთ:
```cpp
int main()
{
  vector<unique_ptr<Mammal>> v(3);
  v[0] = make_unique<Cat>(Cat("Cicqna"));
  v[1] = make_unique<Dog>(Dog("Cuga"));
  v[2] = make_unique<Dog>(Dog("Muria"));
  for (int i = 0; i < 3; i++)
    v[i]->Speak();
}
```
> [!IMPORTANT]
> ეს კოდიც დაბეჭდავს: Cat Cicqna: Meow! Dog Cuga: Woof! Dog Muria: Woof!
___
თუ გვაქ ვირტუალური ფუნქციები, აუცილებლად უნდა გვქონდეს ვირტუალური დესტრუქტორი, წინააღმდეგ შემთხვევაში პროგრამის ბოლოს მარტო წაიშლება ფუძე-კლასის ობიექტები და არა წარმოებული კლასები. როდესაც ვიყენებთ ჭკვიან პოინტერებს, ობიექტები დესტრუქტორს გაუშვებენ პროგრამის ბოლოს, დანარჩენი სხვა კოდის გაშვების შემდეგ.
```cpp
int main()
{
  vector<unique_ptr<Mammal>> v(4);

  v[0] = make_unique<Cat> ("Cicqna");
  v[1] = make_unique<Dog> ("Cuga");
  v[2] = make_unique<Dog> ("Muria");
  v[3] = make_unique<Mammal> ("Unknown");

  for (int i = 0; i < 4; i++)
    v[i]->Speak();
  cout << "Oops..." << endl;
}
```
> [!IMPORTANT]
> ეს კოდიც დაბეჭდავს:
>
> Cat Cicqna: Meow!
>
> Dog Cuga: Woof!
>
> Dog Muria: Woof!
>
> Mammal Unknown speak!
>
> Oops...
>
> Mammal Unknownis under destruction...
>
> Dog Muriais under destruction...
>
> Mammal Muriais under destruction...
>
> Dog Cugais under destruction...
>
> Mammal Cugais under destruction...
>
> Cat Cicqnais under destruction...
>
> Mammal Cicqnais under destruction...
