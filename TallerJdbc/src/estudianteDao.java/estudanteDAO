package App;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

class EstudianteDAO {

    public boolean insertar(Estudiante e) throws SQLException {
        String sql = "INSERT INTO estudiantes (nombre, apellido, correo, edad, estado_civil) VALUES (?,?,?,?,?)";
        Connection conn = App.ConexionDB.getConnection();
        if (conn == null) {
            System.out.println("No hay conexión a la BD.");
            return false;
        }
        try (PreparedStatement ps = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS)) {
            ps.setString(1, e.getNombre());
            ps.setString(2, e.getApellido());
            ps.setString(3, e.getCorreo());
            ps.setInt(4, e.getEdad());
            ps.setString(5, e.getEstadoCivil().name()); // este convierte el enum en su nombre de texto exacto

            int rows = ps.executeUpdate();
            if (rows > 0) {
                try (ResultSet rs = ps.getGeneratedKeys()) {
                    if (rs.next()) e.setId(rs.getInt(1));
                }
                return true;
            }
        } catch (SQLIntegrityConstraintViolationException ex) {
            System.out.println("Error: ya existe un estudiante con ese correo.");
        } catch (SQLException ex) {
            System.out.println("Error SQL al insertar: " + ex.getMessage());
        } finally {
            try { conn.close(); } catch (SQLException ignored) {}
        }
        return false;
    }

    public boolean actualizarPorCorreo(String correoOriginal, Estudiante e) throws SQLException {
        String sql = "UPDATE estudiantes SET nombre=?, apellido=?, edad=?, estado_civil=? WHERE correo=?";
        Connection conn = App.ConexionDB.getConnection();
        if (conn == null) {
            System.out.println("No hay conexión a la BD.");
            return false;
        }
        try (PreparedStatement ps = conn.prepareStatement(sql)) { //es como el mensajero que lleva esos datos a la base de datos.
            ps.setString(1, e.getNombre());
            ps.setString(2, e.getApellido());
            ps.setInt(3, e.getEdad());
            ps.setString(4, e.getEstadoCivil().name()); // <---- CAMBIO
            ps.setString(5, correoOriginal);

            //le meto los datos que vienen del objeto Estudiante e. lo de arriba

            return ps.executeUpdate() > 0;
        } catch (SQLException ex) {
            System.out.println("Error SQL al actualizar: " + ex.getMessage());
        } finally {
            try { conn.close(); } catch (SQLException ignored) {}
        }
        return false;
    }

    public boolean eliminarPorCorreo(String correo) throws SQLException {
        String sql = "DELETE FROM estudiantes WHERE correo=?";
        Connection conn = App.ConexionDB.getConnection();
        if (conn == null) {
            System.out.println("No hay conexión a la BD.");
            return false;
        }
        try (PreparedStatement ps = conn.prepareStatement(sql)) {
            ps.setString(1, correo);
            return ps.executeUpdate() > 0;
        } catch (SQLException ex) {
            System.out.println("Error SQL al eliminar: " + ex.getMessage());
        } finally {
            try { conn.close(); } catch (SQLException ignored) {}
        }
        return false;
    }

    public List<Estudiante> obtenerTodos() throws SQLException {
        List<Estudiante> lista = new ArrayList<>();
        String sql = "SELECT * FROM estudiantes";
        Connection conn = App.ConexionDB.getConnection();
        if (conn == null) {
            System.out.println("No hay conexión a la BD.");
            return lista;
        }
        try (Statement st = conn.createStatement();
             ResultSet rs = st.executeQuery(sql)) {

            while (rs.next()) {
                Estudiante e = new Estudiante(
                        rs.getInt("id"),
                        rs.getString("nombre"),
                        rs.getString("apellido"),
                        rs.getString("correo"),
                        rs.getInt("edad"),
                        EstadoCivil.valueOf(rs.getString("estado_civil")) // <---- CAMBIO
                );
                lista.add(e);
            }
        } catch (SQLException ex) {
            System.out.println("Error SQL al leer todos: " + ex.getMessage());
        } finally {
            try { conn.close(); } catch (SQLException ignored) {}
        }
        return lista;
    }

    public Estudiante obtenerPorCorreo(String correo) throws SQLException {
        String sql = "SELECT * FROM estudiantes WHERE correo=?";
        Connection conn = App.ConexionDB.getConnection();
        if (conn == null) {
            System.out.println("No hay conexión a la BD.");
            return null;
        }
        try (PreparedStatement ps = conn.prepareStatement(sql)) {
            ps.setString(1, correo);
            try (ResultSet rs = ps.executeQuery()) {
                if (rs.next()) {
                    return new Estudiante(
                            rs.getInt("id"),
                            rs.getString("nombre"),
                            rs.getString("apellido"),
                            rs.getString("correo"),
                            rs.getInt("edad"),
                            EstadoCivil.valueOf(rs.getString("estado_civil")) // <---- CAMBIO
                    );
                }
            }
        } catch (SQLException ex) {
            System.out.println("Error SQL al buscar por correo: " + ex.getMessage());
        } finally {
            try { conn.close(); } catch (SQLException ignored) {}
        }
        return null;
    }
}
