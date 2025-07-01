
# SOLID
Los principios SOLID son un conjunto de 5 fundamento que guían a los desarrolladores programación orienta da a objetos, fueron introducidos por Robert J. Martin y son esenciales para el desarrollo de software de calidad. Los 5 principios son:

S — Single Responsibility Principle (Principio de responsabilidad única)

O — Open-Closed Principle (Principio Abierto-Cerrado)

L — Liskov Substitution Principle (Principio de sustitución de Liskov)

I — Interface Segregation Principle (Principio de segregación de la interfaz)

D — Dependency Inversion Principle (Principio de inversión de dependencia)

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
