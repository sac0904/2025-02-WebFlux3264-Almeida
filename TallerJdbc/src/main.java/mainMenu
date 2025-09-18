package App;

import java.sql.SQLException;
import java.util.List;
import java.util.Scanner;

class MainMenu {
    private static final Scanner scanner = new Scanner(System.in);
    private static final EstudianteDAO dao = new EstudianteDAO();

    public static void main(String[] args) throws SQLException {
        while (true) {
            printMenu();
            int opcion = readInt("Selecciona una opción: ");
            switch (opcion) {
                case 1: insertarEstudiante(); break;
                case 2: actualizarEstudiante(); break;
                case 3: eliminarEstudiante(); break;
                case 4: consultarTodos(); break;
                case 5: consultarPorEmail(); break;
                case 6:
                    System.out.println("Saliendo. ¡Hasta luego!");
                    scanner.close();
                    System.exit(0);
                default:
                    System.out.println("Opción inválida. Intenta otra vez.");
            }
        }
    }

    private static void printMenu() {
        System.out.println("\n--- MENU TallerJDBC ---");
        System.out.println("1. Insertar Estudiante");
        System.out.println("2. Actualizar Estudiante (por correo)");
        System.out.println("3. Eliminar Estudiante (por correo)");
        System.out.println("4. Consultar todos los estudiantes");
        System.out.println("5. Consultar Estudiante por email");
        System.out.println("6. Salir del programa");
    }

    private static void insertarEstudiante() throws SQLException {
        System.out.println("\n--- Insertar Estudiante ---");
        String nombre = readString("Nombre: ");
        String apellido = readString("Apellido: ");
        String correo = readString("Correo (único): ");
        int edad = readInt("Edad: ");
        EstadoCivil estado = leerEstadoCivil();
        Estudiante e = new Estudiante(nombre, apellido, correo, edad, estado);
        boolean ok = dao.insertar(e);
        System.out.println(ok ? "✅ Insertado correctamente." : " No se pudo insertar.");
    }

    private static void actualizarEstudiante() throws SQLException {
        System.out.println("\n--- Actualizar Estudiante ---");
        String correo = readString("Correo del estudiante a actualizar: ");
        Estudiante existente = dao.obtenerPorCorreo(correo);
        if (existente == null) {
            System.out.println("No existe estudiante con ese correo.");
            return;
        }
        System.out.println("Estudiante actual: " + existente);
        String nombre = readString("Nuevo nombre: ");
        String apellido = readString("Nuevo apellido: ");
        int edad = readInt("Nueva edad: ");
        EstadoCivil estado = leerEstadoCivil();
        Estudiante s = new Estudiante(nombre, apellido, correo, edad, estado);
        boolean ok = dao.actualizarPorCorreo(correo, s);
        System.out.println(ok ? "✅ Actualizado correctamente." : "No se pudo actualizar.");
    }

    private static void eliminarEstudiante() throws SQLException {
        System.out.println("\n--- Eliminar Estudiante ---");
        String correo = readString("Correo del estudiante a eliminar: ");
        Estudiante s = dao.obtenerPorCorreo(correo);
        if (s == null) {
            System.out.println("No existe estudiante con ese correo.");
            return;
        }
        String confirm = readString("Confirma eliminar a " + s.getNombre() + " " + s.getApellido() + " (s/n): ");
        if (confirm.equalsIgnoreCase("s")) {
            boolean ok = dao.eliminarPorCorreo(correo);
            System.out.println(ok ? "✅ Eliminado correctamente." : "No se pudo eliminar.");
        } else {
            System.out.println("Eliminación cancelada.");
        }
    }

    private static void consultarTodos() throws SQLException {
        System.out.println("\n--- Lista de Estudiantes ---");
        List<Estudiante> lista = dao.obtenerTodos();
        if (lista.isEmpty()) {
            System.out.println("No hay estudiantes registrados.");
            return;
        }
        for (Estudiante s : lista) {
            System.out.println(s);
        }
    }

    private static void consultarPorEmail() throws SQLException {
        System.out.println("\n--- Consultar por email ---");
        String correo = readString("Correo: ");
        Estudiante s = dao.obtenerPorCorreo(correo);
        if (s == null) {
            System.out.println("No encontrado.");
        } else {
            System.out.println(s);
        }
    }

    private static int readInt(String prompt) {
        while (true) {
            try {
                System.out.print(prompt);
                String line = scanner.nextLine().trim();
                return Integer.parseInt(line);
            } catch (NumberFormatException e) {
                System.out.println("Entrada no válida. Ingresa un número.");
            }
        }
    }

    private static String readString(String prompt) {
        System.out.print(prompt);
        return scanner.nextLine().trim();
    }

    private static EstadoCivil leerEstadoCivil() {
        while (true) {
            System.out.println("Estado civil (ingresa el número):");
            EstadoCivil[] arr = EstadoCivil.values();
            for (int i = 0; i < arr.length; i++) {
                System.out.printf("%d. %s%n", i, arr[i].name());
            }
            int sel = readInt("Opción: ");
            if (sel >= 0 && sel < arr.length) {
                return arr[sel];
            }
            System.out.println("Selección inválida. Intenta de nuevo.");
        }
    }
}
