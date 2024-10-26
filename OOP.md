Để hiểu rõ về các khái niệm **Inheritance** (kế thừa), **Polymorphism** (đa hình), **Abstraction** (trừu tượng hóa), và **Interface** trong lập trình hướng đối tượng (OOP), chúng ta sẽ đi từng khái niệm từ cơ bản đến nâng cao, với các ví dụ chi tiết bằng Java.

### 1. Inheritance (Kế thừa)
**Inheritance** là một cơ chế cho phép một lớp con (subclass) kế thừa các thuộc tính và phương thức từ một lớp cha (superclass). Inheritance giúp tái sử dụng mã nguồn và dễ dàng mở rộng ứng dụng.

#### Ví dụ cơ bản

Giả sử chúng ta có một lớp cha là `Animal`, và các lớp con như `Dog` và `Cat`.

```java
// Lớp cha
class Animal {
    String name;

    void eat() {
        System.out.println(name + " is eating.");
    }
}

// Lớp con kế thừa lớp Animal
class Dog extends Animal {
    void bark() {
        System.out.println(name + " is barking.");
    }
}

class Cat extends Animal {
    void meow() {
        System.out.println(name + " is meowing.");
    }
}

// Sử dụng
public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.name = "Buddy";
        dog.eat(); // Kế thừa từ lớp cha
        dog.bark();

        Cat cat = new Cat();
        cat.name = "Kitty";
        cat.eat(); // Kế thừa từ lớp cha
        cat.meow();
    }
}
```

Trong ví dụ trên, `Dog` và `Cat` đều kế thừa thuộc tính `name` và phương thức `eat()` từ lớp `Animal`.

#### Inheritance nâng cao: Kế thừa bậc nhiều và Gọi super()
Trong Java, kế thừa nhiều lớp trực tiếp không được hỗ trợ, nhưng có thể sử dụng nhiều lớp kế thừa theo cách lớp con kế thừa lớp con khác (Multi-level inheritance).

```java
class Mammal extends Animal {
    void breastfeed() {
        System.out.println(name + " is breastfeeding.");
    }
}

class Human extends Mammal {
    void speak() {
        System.out.println(name + " is speaking.");
    }
}

// Sử dụng super() để gọi phương thức của lớp cha
class Dog extends Animal {
    Dog(String name) {
        super.name = name;
    }

    @Override
    void eat() {
        System.out.println(name + " is eating dog food.");
    }
}
```

### 2. Polymorphism (Đa hình)
**Polymorphism** cho phép cùng một phương thức có thể hoạt động khác nhau tùy vào ngữ cảnh, giúp viết mã linh hoạt và mở rộng.

#### Ví dụ cơ bản với Overloading (Nạp chồng phương thức)

```java
class MathOperations {
    int add(int a, int b) {
        return a + b;
    }

    // Overloading: Nạp chồng với tham số khác nhau
    int add(int a, int b, int c) {
        return a + b + c;
    }
}

public class Main {
    public static void main(String[] args) {
        MathOperations math = new MathOperations();
        System.out.println(math.add(5, 10));       // 15
        System.out.println(math.add(5, 10, 20));   // 35
    }
}
```

#### Ví dụ nâng cao với Overriding (Ghi đè phương thức)

Overriding là khi lớp con định nghĩa lại phương thức của lớp cha để phù hợp với nhu cầu riêng.

```java
class Animal {
    void sound() {
        System.out.println("Some sound...");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Woof Woof");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Meow Meow");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog();
        Animal myCat = new Cat();
        myDog.sound(); // Kết quả: Woof Woof
        myCat.sound(); // Kết quả: Meow Meow
    }
}
```

### 3. Abstraction (Trừu tượng hóa)
**Abstraction** là khái niệm che giấu các chi tiết nội bộ của một lớp và chỉ để lộ ra các phương thức quan trọng mà bên ngoài có thể sử dụng. Trong Java, Abstraction có thể được thực hiện bằng cách sử dụng **abstract class** hoặc **interface**.

#### Ví dụ với Abstract Class

```java
abstract class Animal {
    String name;

    // Phương thức trừu tượng không có phần thân
    abstract void sound();

    void eat() {
        System.out.println(name + " is eating.");
    }
}

class Dog extends Animal {
    Dog(String name) {
        this.name = name;
    }

    @Override
    void sound() {
        System.out.println("Woof Woof");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy");
        dog.eat();
        dog.sound();
    }
}
```

### 4. Interface (Giao diện)
**Interface** là một loại lớp trừu tượng hoàn toàn trong Java. Tất cả các phương thức trong interface mặc định là trừu tượng, và các lớp thực hiện (`implements`) interface phải định nghĩa lại tất cả các phương thức đó.

#### Ví dụ cơ bản với Interface

```java
interface Animal {
    void eat();
    void sound();
}

class Dog implements Animal {
    public void eat() {
        System.out.println("Dog is eating.");
    }

    public void sound() {
        System.out.println("Woof Woof");
    }
}

class Cat implements Animal {
    public void eat() {
        System.out.println("Cat is eating.");
    }

    public void sound() {
        System.out.println("Meow Meow");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog();
        Animal myCat = new Cat();
        myDog.eat();
        myDog.sound();
        myCat.eat();
        myCat.sound();
    }
}
```

#### Interface nâng cao: Multiple Interface Implementation

Một lớp có thể thực hiện nhiều interface trong Java.

```java
interface CanRun {
    void run();
}

interface CanSwim {
    void swim();
}

class Dog implements Animal, CanRun {
    public void eat() {
        System.out.println("Dog is eating.");
    }

    public void sound() {
        System.out.println("Woof Woof");
    }

    public void run() {
        System.out.println("Dog is running.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.eat();
        dog.sound();
        dog.run();
    }
}
```

### Tổng kết

- **Inheritance**: Tái sử dụng mã nguồn, các lớp con kế thừa thuộc tính và phương thức từ lớp cha.
- **Polymorphism**: Cho phép các phương thức có hành vi khác nhau trong lớp con hoặc có thể thực hiện qua overloading và overriding.
- **Abstraction**: Trừu tượng hóa, chỉ cung cấp các phương thức quan trọng và che giấu chi tiết cài đặt.
- **Interface**: Định nghĩa hành vi mà các lớp có thể triển khai, cho phép đa kế thừa hành vi.

Bằng cách kết hợp cả bốn khái niệm trên, bạn có thể thiết kế các hệ thống linh hoạt, dễ bảo trì và mở rộng.
