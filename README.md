
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
