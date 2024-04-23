# როგორ განიმარტება იტერატორების [first,last) დიაპაზონი (range)? რა თვისებები უნდა ჰქონდეს ამ წყვილს, რომ დიაპაზონი კორექტულად იყოს განმარტებული?

`[first, last)` ნახევრად ღია დიაპაზონია რომელიც მოიცავს ყველა ელემენტს რომელიც
first-ზე მეტია ან ტოლია და last-ზე ნაკლებია. მაგალითად თუ `first` არის 0 და
`last` არის 5 მაშინ `[0, 5)` - დიაპაზონი შედგება შემდეგი რიცხვებისგან: [0, 1, 2, 3, 4]

ეს დიაპაზონი სწორად რომ იყოს განმარტებული უნდა აკმაყოფილობდეს შემდეგ პირობებს:

- `first` აუცილებლად ნაკლები ან ტოლი უნდა იყოს `last`-ზე
- `last` არ უნდა იყოს დიაპაზონში არსებული ელემენტების ნაწილი

