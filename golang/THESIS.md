
- Language
  - Настільки швидкий у вивченні наскільки складний у використанні
  - 2 з половиною концепції
  
  - Структури
  - Ф-ції білдери
  - Функції до типів можуть бути скрізь в одному пекейджі
  - Аліаси типів які мають методи. Зручно для ддд, де багато value objects.
  - Дуже подобається гнучкість типів тайпскрипта
  - Немає наслідування, є ембедінг. Композиція а не генералізація
  - Go method overload унікальні імена
  
  - Інтерфейси класні
  
  - Функції Фірст класс сітізенс
  - Default parameters фції білдери
  - Дефер
  
  - Теги в полів
  - Обрізані ніполноценние дженеріки (рус флаг)
  - До дженеріків інтерфейси як в джс
  - Ніли, за шо. Але також є цікаві кейси, наприклад виклик функцій працює
  - Генерація рулить
  - Розповісти про них і сказати що ви вже знаєте голенг. Титри мем

- Error handling
  - Exceptions?
  - Осознаний хендлінг помилок. Ігнор помилок. Немає стектрецсу
  - Паніки, та можливість їх ловити з деферами
  - Паніки в бібліотеках

- Tools
  - Генерація доки, з екзамплами та посиланнями на код
  - Ембедінг, наприклад міграцій
  - Go vet, go fmt
  - Go generate
  - Go build flags
  - Go test
  - Race conditions detection

- Common things
  - Орм, ні не чули
  - Трейсінг

- Dependency management
  - Дурнуваті назви пекейджей що мають в назві локейшн
  - Зручний версіонінг додатку з тегами чи без них
  - Вендорінг залежностей
  - Пінінг версій, привіт нода
  - Тест залежності компілюються разом
  - Go mod proxy

- Performance
  - Використання стеку, так як це структури. Але деякі використовують поцнтери скрізь, що пропагує паніки
  - Продуктивність, привіт нода, бувай раст. Четабельність важливіше, інше можна закидати ресурсами
  - Якщо нам була важлива продуктивність ми би не писали на джаваскрипті серверну частину
  - Багктопотоклвіть, чи скоріше рцтинність, привіт нода, раст свої імплементації
  - Горутини та канали приклад з хрон джобою

- Conventions
  - Один довгий файл то є добре
  - Пропагується правило акама, щодо абстракцій, при цьому вербоус код
  - Контексти, кенселейшн
  - Дуже сильно опирається на коментарі. Екстеншн що нагадує змінити коментар, коли ти змін код біля нього

- Should you go?
  - Дуже мінімалістична мова, що допомагає писати простий як двері код та не заплутатися в абстракціях
  - Не підходить для початківця, хто починає вивчати з простіших мов (js, python), але підійде на заміну c в університеті
  - Кажуть що простіше вводити новачків в роботу системи
  - Зручний тулінг
  - Але не потрібно зациклюватися, бо за час що ви проведете з go, можете розучитися писати на більш 
  - Не став би використовувати для всіх задач, як це можна провернути з js (gui, cli, std через зменшення абстракцій краще підтримує linux bad windows support in std)


- Щоб уникнути Wtf, не робіть ні не треба
- The good the bad the ugly
- Makefile