# new და delete ოპერატორები ცვლადებთან და ობიექტებთან. ძირითადი ფაქტები და თვალსაჩინო მაგალითები.

new და delete ოპერატორები გამოიყენება მეხსიერების დინამიურად გამოყოფის და
გათავისუფლებისთვის

> [!CAUTION]
> დინამიურად შექმნილი ცვლადების გათავისუბლება აუცილებელია და პროგრამისტის მოვალეობაა!

### new და delete ოპერატორები ცვლადებთან
```cpp
int* num_ptr = new int(10);

//...

delete num_ptr;
```
new - თი შექმნილი ცვლადებს Heap-ზე გამოეყოფათ ადგილი


### new და delete ოპერატორები ობიექტებთან
```cpp
class Shape {
    //...   
};

int main() {
    Shape* shape_ptr = new Shape();
    
    //...

    delete shape_ptr;
}
```
ობიექტების ინიციალიზებისთვის new-ს გამოყენების დროს, ობიექტის კონსტრუქტორების
გამოძახება ხდება