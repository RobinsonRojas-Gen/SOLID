# Principios SOLID

Los principios SOLID son un conjunto de **5 fundamentos** que guían a los desarrolladores en la programación orientada a objetos. Fueron introducidos por **Robert C. Martin** y son esenciales para el desarrollo de software de calidad, mantenible y escalable.

## ¿Qué son los principios SOLID?

**SOLID** es un acrónimo que representa:

- **S** — Single Responsibility Principle (Principio de Responsabilidad Única)
- **O** — Open-Closed Principle (Principio Abierto-Cerrado)
- **L** — Liskov Substitution Principle (Principio de Sustitución de Liskov)
- **I** — Interface Segregation Principle (Principio de Segregación de Interfaces)
- **D** — Dependency Inversion Principle (Principio de Inversión de Dependencias)

---

## 🎯 S - Single Responsibility Principle (SRP)

### Definición
**"Una clase debe tener una sola razón para cambiar"**

Cada clase debe tener una única responsabilidad y hacerlo bien.

### ❌ Ejemplo Incorrecto
```java
public class Reporte {
    public String generarContenido() { 
        return "contenido"; 
    }

    public void guardarEnArchivo(String contenido) {
        // Lógica para guardar
    }

    public void enviarPorEmail(String contenido) {
        // Lógica para enviar
    }
}
```
**Problema:** La clase `Reporte` tiene tres responsabilidades diferentes.

### ✅ Ejemplo Correcto
```java
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
```
**Beneficio:** Cada clase tiene una sola responsabilidad y es más fácil de mantener.

---

## 🔓 O - Open/Closed Principle (OCP)

### Definición
**"Las clases deben estar abiertas para extensión, pero cerradas para modificación"**

Deberíamos poder agregar nuevas funcionalidades sin modificar el código existente.

### ✅ Ejemplo Correcto
```java
// Interfaz base
interface Shape {
    double area();
}

// Implementaciones existentes
class Circle implements Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    public double area() {
        return Math.PI * radius * radius;
    }
}

class Rectangle implements Shape {
    private double width, height;

    public Rectangle(double width, double height) {
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
```

### 🚀 Extensión sin Modificación
```java
// ✅ Nueva clase agregada sin modificar código existente
class Triangle implements Shape {
    private double base, height;

    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }

    public double area() {
        return (base * height) / 2;
    }
}

// Uso
public class Main {
    public static void main(String[] args) {
        Shape circle = new Circle(5);
        Shape rectangle = new Rectangle(4, 6);
        Shape triangle = new Triangle(3, 7); // ✅ Nuevo sin modificar AreaCalculator

        AreaCalculator calculator = new AreaCalculator();

        System.out.println("Área del círculo: " + calculator.calculateArea(circle));
        System.out.println("Área del rectángulo: " + calculator.calculateArea(rectangle));
        System.out.println("Área del triángulo: " + calculator.calculateArea(triangle));
    }
}
```

---

## 🔄 L - Liskov Substitution Principle (LSP)

### Definición
**"Las clases derivadas deben poder sustituir a sus clases base sin afectar el comportamiento del programa"**

### ❌ Ejemplo Incorrecto
```java
public class Rectangulo {
    protected int ancho, alto;
    
    public void setAncho(int ancho) { this.ancho = ancho; }
    public void setAlto(int alto) { this.alto = alto; }
    public int calcularArea() { return ancho * alto; }
}

public class Cuadrado extends Rectangulo {
    @Override
    public void setAncho(int ancho) { 
        this.ancho = this.alto = ancho; // Modifica ambos
    }
    
    @Override
    public void setAlto(int alto) { 
        this.alto = this.ancho = alto; // Modifica ambos
    }
}
```
**Problema:** `Cuadrado` no se comporta como un `Rectangulo` al cambiar solo una dimensión.

### ✅ Ejemplo Correcto
```java
public abstract class Forma {
    public abstract int calcularArea();
}

public class Rectangulo extends Forma {
    private int ancho, alto;
    
    public Rectangulo(int ancho, int alto) { 
        this.ancho = ancho; 
        this.alto = alto; 
    }
    
    public int calcularArea() { 
        return ancho * alto; 
    }
}

public class Cuadrado extends Forma {
    private int lado;
    
    public Cuadrado(int lado) { 
        this.lado = lado; 
    }
    
    public int calcularArea() { 
        return lado * lado; 
    }
}
```
**Beneficio:** Ambas clases son tratadas como formas independientes y cumplen LSP.

---

## 🔌 I - Interface Segregation Principle (ISP)

### Definición
**"No se debe forzar a una clase a implementar interfaces que no necesita"**

### ❌ Ejemplo Incorrecto
```java
interface Worker {
    void work();
    void eat();
}

class Robot implements Worker {
    public void work() {
        System.out.println("Robot working");
    }

    public void eat() {
        // ❌ Los robots no comen, pero deben implementar este método
        throw new UnsupportedOperationException("Robots don't eat");
    }
}
```

### ✅ Ejemplo Correcto
```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class Human implements Workable, Eatable {
    public void work() { 
        System.out.println("Human working"); 
    }
    
    public void eat() { 
        System.out.println("Human eating"); 
    }
}

class Robot implements Workable {
    public void work() { 
        System.out.println("Robot working"); 
    }
    // ✅ No necesita implementar eat()
}
```

---

## 🔁 D - Dependency Inversion Principle (DIP)

### Definición
**"Las clases deben depender de abstracciones (interfaces), no de implementaciones concretas"**

### ❌ Ejemplo Incorrecto
```java
class LightBulb {
    public void turnOn() { System.out.println("Bulb on"); }
    public void turnOff() { System.out.println("Bulb off"); }
}

class Switch {
    private LightBulb bulb = new LightBulb(); // ❌ Dependencia directa

    public void operate() {
        bulb.turnOn();
    }
}
```
**Problema:** `Switch` está acoplado a `LightBulb` específicamente.

### ✅ Ejemplo Correcto
```java
interface Switchable {
    void turnOn();
    void turnOff();
}

class LightBulb implements Switchable {
    public void turnOn() { System.out.println("Bulb on"); }
    public void turnOff() { System.out.println("Bulb off"); }
}

class Fan implements Switchable {
    public void turnOn() { System.out.println("Fan on"); }
    public void turnOff() { System.out.println("Fan off"); }
}

class Switch {
    private Switchable device; // ✅ Depende de la abstracción

    public Switch(Switchable device) {
        this.device = device;
    }

    public void operate() {
        device.turnOn();
    }
}

// Uso
public class Main {
    public static void main(String[] args) {
        Switch lightSwitch = new Switch(new LightBulb());
        Switch fanSwitch = new Switch(new Fan());
        
        lightSwitch.operate(); // "Bulb on"
        fanSwitch.operate();   // "Fan on"
    }
}
```

---

## 🎯 Beneficios de Aplicar SOLID

- **Código más mantenible:** Cambios localizados y controlados
- **Mayor flexibilidad:** Fácil extensión sin modificar código existente
- **Mejor testabilidad:** Clases con responsabilidades claras
- **Menor acoplamiento:** Componentes independientes
- **Mayor cohesión:** Cada clase tiene un propósito específico

## 📚 Recursos Adicionales

- [Clean Code por Robert C. Martin](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
- [Principios SOLID en la práctica](https://www.baeldung.com/solid-principles)
- [Refactoring: Improving the Design of Existing Code](https://martinfowler.com/books/refactoring.html)

---

*Los principios SOLID son fundamentales para escribir código de calidad. Aplicarlos desde el inicio del proyecto facilitará el mantenimiento y la evolución del software.*
