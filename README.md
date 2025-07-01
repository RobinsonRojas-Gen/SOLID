
# SOLID
Los principios SOLID son un conjunto de 5 fundamento que guían a los desarrolladores programación orienta da a objetos, fueron introducidos por Robert J. Martin y son esenciales para el desarrollo de software de calidad. Los 5 principios son:

S — Single Responsibility Principle (Principio de responsabilidad única)

O — Open-Closed Principle (Principio Abierto-Cerrado)

L — Liskov Substitution Principle (Principio de sustitución de Liskov)

I — Interface Segregation Principle (Principio de segregación de la interfaz)

D — Dependency Inversion Principle (Principio de inversión de dependencia)

--------------------------------------------------------------

S – Single Responsibility Principle (Responsabilidad Única)

Una clase debe tener una sola razón para cambiar.

✅ Que haga una sola cosa y lo haga bien.

❌ Mal ejemplo:
java
Copiar
Editar
public class Reporte {
    public String generarContenido() { return "contenido"; }

    public void guardarEnArchivo(String contenido) {
        // Lógica para guardar
    }

    public void enviarPorEmail(String contenido) {
        // Lógica para enviar
    }
}
✅ Buen ejemplo:
java
Copiar
Editar
public class GeneradorReporte {
    public String generarContenido() {
        return "contenido";
    }
}

public class GuardadorArchivo {
    public void guardar(String contenido) {
        // Guardar en archivo
    }
}

public class EnviadorEmail {
    public void enviar(String contenido) {
        // Enviar por correo
    }
}

--------------------------------------------------------------

O - Open/Closed Principle (OCP) (El Principio Abierto-Cerrado)

'El principio de apertura y cierre exige que las clases deban estar abiertas a la extensión y cerradas a la modificación'

Modificación significa cambiar el código de una clase existente y extensión significa agregar una nueva funcionalidad.

'Deberíamos poder agregar nuevas funciones sin tocar el código existente para la clase'


✅ Código ejemplo:

// Interfaz base
interface Shape {
    double area();
}

// Clase que implementa la interfaz
class Circle implements Shape {
    double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    public double area() {
        return Math.PI * radius * radius;
    }
}

// Otra clase que implementa la interfaz
class Rectangle implements Shape {
    double width, height;

    Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    public double area() {
        return width * height;
    }
}

// Calculadora que cumple con OCP
class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.area();
    }
}




✅ Agregar: Triangle sin modificar AreaCalculator.

// Interfaz base
interface Shape {
    double area();
}

// Círculo
class Circle implements Shape {
    double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    public double area() {
        return Math.PI * radius * radius;
    }
}

// Rectángulo
class Rectangle implements Shape {
    double width, height;

    Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    public double area() {
        return width * height;
    }
}

// ✅ Nueva clase agregada: Triángulo
class Triangle implements Shape {
    double base, height;

    Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }

    public double area() {
        return (base * height) / 2;
    }
}

// Calculadora
class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.area();
    }
}


✅ Uso:

public class Main {
    public static void main(String[] args) {
        Shape circle = new Circle(5);
        Shape rectangle = new Rectangle(4, 6);
        Shape triangle = new Triangle(3, 7); // ✅ Nuevo triángulo

        AreaCalculator calculator = new AreaCalculator();

        System.out.println("Área del círculo: " + calculator.calculateArea(circle));
        System.out.println("Área del rectángulo: " + calculator.calculateArea(rectangle));
        System.out.println("Área del triángulo: " + calculator.calculateArea(triangle)); // ✅ Uso sin modificar calculadora
    }
}

Se agregó Triangle sin tocar el código de AreaCalculator.

Extensión sin modificación = se cumple el Principio OCP.

--------------------------------------------------------------

L — Liskov Substitution Principle (Principio de sustitución de Liskov)

## *"Las clases hijas deben poder sustituir a sus clases padre sin afectar el comportamiento del programa.”*

Según este principio, nuestras clases derivadas deben poder sustituir a sus clases base sin alterar el comportamiento del programa.

```java
public class Rectangulo {
    protected int ancho, alto;
    public void setAncho(int ancho) { this.ancho = ancho; }
    public void setAlto(int alto) { this.alto = alto; }
}

public class Cuadrado extends Rectangulo {
    @Override
    public void setAncho(int ancho) { this.ancho = this.alto = ancho; }
    @Override
    public void setAlto(int alto) { this.alto = this.ancho = alto; }
}
```

❌ Un `Cuadrado` no se comporta como un `Rectángulo`. Si un método espera un `Rectángulo` y cambia el ancho sin tocar el alto, **el resultado será inesperado**.

```java
public abstract class Forma {
    public abstract int calcularArea();
}

public class Rectangulo extends Forma {
    private int ancho, alto;
    public Rectangulo(int ancho, int alto) { this.ancho = ancho; this.alto = alto; }
    public int calcularArea() { return ancho * alto; }
}

ublic class Cuadrado extends Forma {
    private int lado;
    public Cuadrado(int lado) { this.lado = lado; }
    public int calcularArea() { return lado * lado; }
}

```
--------------------------------------------------------------

✅ `Cuadrado` y `Rectangulo` **son tratados como formas independientes**, cumpliendo LSP.

🔹 I - Interface Segregation Principle (ISP)
Principio de Segregación de Interfaces

No se debe forzar a una clase a implementar interfaces que no necesita.

Ejemplo incorrecto:

java
Copiar
Editar
interface Worker {
    void work();
    void eat();
}

class Robot implements Worker {
    public void work() {
        System.out.println("Robot working");
    }

    public void eat() {
        // No aplica, pero lo debe implementar igual
    }
}
Ejemplo correcto:

java
Copiar
Editar
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class Human implements Workable, Eatable {
    public void work() { System.out.println("Human working"); }
    public void eat() { System.out.println("Human eating"); }
}

class Robot implements Workable {
    public void work() { System.out.println("Robot working"); }
}

--------------------------------------------------------------

🔹 D - Dependency Inversion Principle (DIP)
Principio de Inversión de Dependencias

Las clases deben depender de abstracciones (interfaces), no de implementaciones concretas.

Ejemplo incorrecto:

java
Copiar
Editar
class LightBulb {
    public void turnOn() { System.out.println("Bulb on"); }
    public void turnOff() { System.out.println("Bulb off"); }
}

class Switch {
    private LightBulb bulb = new LightBulb();

    public void operate() {
        bulb.turnOn();
    }
}
Ejemplo correcto:

java
Copiar
Editar
interface Switchable {
    void turnOn();
    void turnOff();
}

class LightBulb implements Switchable {
    public void turnOn() { System.out.println("Bulb on"); }
    public void turnOff() { System.out.println("Bulb off"); }
}

class Switch {
    private Switchable device;

    public Switch(Switchable device) {
        this.device = device;
    }

    public void operate() {
        device.turnOn();
    }
}
