# 📘 Documentación de Práctica 3: Aplicación ToDo

## 📌 ¿Qué es esta aplicación?

Esta es una aplicación de lista de tareas (ToDo) que fue mejorada con nuevas funciones. La aplicación original ya permitía crear y gestionar tareas, pero ahora tiene características adicionales que la hacen más completa y fácil de usar.

## ✨ ¿Qué hay de nuevo?

Se agregaron tres nuevas características principales:

1. **Barra de menú** - Para navegar más fácilmente por la aplicación
2. **Lista de usuarios** - Para ver todos los usuarios registrados
3. **Perfil de usuario** - Para ver los datos detallados de cada usuario

---

## 🧭 Barra de Menú

### ¿Qué hace?

La barra de menú es como el panel de control de la aplicación. Aparece en la parte superior de las páginas y cambia dependiendo de si el usuario ha iniciado sesión o no.

### ¿Qué botones tiene?

**Cuando NO has iniciado sesión:**

- **ToDoList**: Te lleva a la página principal con información sobre la aplicación
- **Login**: Te lleva a la página para iniciar sesión
- **Registrarse**: Te lleva a la página para crear una cuenta nueva

**Cuando SÍ has iniciado sesión:**

- **ToDoList**: Te lleva a la página principal
- **Tareas**: Te lleva a tu lista personal de tareas
- **Tu nombre** (menú desplegable): Al hacer clic muestra opciones como "Cuenta" y "Cerrar sesión"

### Código importante:

La barra de menú se creó con este código HTML:

```html
<nav class="navbar navbar-expand-lg navbar-light bg-info">
  <a class="navbar-brand" href="#">Menu</a>
  <div class="collapse navbar-collapse" id="navbarNavDropdown">
    <ul class="navbar-nav" style="width:100%">
      <li class="nav-item active">
        <a
          class="nav-link"
          th:href="@{/usuarios/{id}/aboutIn(id=${usuario.id})}"
          >ToDoList</a
        >
      </li>
      <li class="nav-item">
        <a class="nav-link" th:href="@{/usuarios/{id}/tareas(id=${usuario.id})}"
          >Tareas</a
        >
      </li>
      <li class="nav-item dropdown" style="margin-left:70%">
        <a
          class="nav-link dropdown-toggle"
          href="#"
          th:text="${usuario.nombre}"
        ></a>
        <div class="dropdown-menu">
          <a class="dropdown-item" href="#">Cuenta</a>
          <a class="dropdown-item" href="/logout">Cerrar sesión</a>
        </div>
      </li>
    </ul>
  </div>
</nav>
```

---

## 👥 Lista de Usuarios

### ¿Qué hace?

Esta página muestra una tabla con todos los usuarios que se han registrado en la aplicación. Es útil para ver quién más está usando la aplicación.

### ¿Qué información muestra?

Para cada usuario puedes ver:

- Su número de identificación (ID)
- Su correo electrónico
- Un botón para ver más detalles de su perfil

### ¿Cómo acceder?

Se puede acceder escribiendo `/registrados` en la dirección del navegador.

### Código importante:

Para mostrar esta lista se creó un controlador que busca todos los usuarios:

```java
@GetMapping("/registrados")
public String listadoUsuarios(Model model, HttpSession session) {
    List<UsuarioData> usuarios = usuarioService.getAll();
    model.addAttribute("usuarios", usuarios);
    return "listaUsuarios";
}
```

---

## 🧑‍💼 Perfil de Usuario

### ¿Qué hace?

Esta página muestra información detallada sobre un usuario específico. Es como una "tarjeta de presentación" digital.

### ¿Qué información muestra?

- Nombre completo del usuario
- Número de identificación (ID)
- Correo electrónico
- Fecha de nacimiento

### ¿Cómo acceder?

Desde la lista de usuarios, haciendo clic en el botón "Descripción" de cualquier usuario.

### Código importante:

El controlador que maneja esta página:

```java
@GetMapping("/registrados/{id}")
public String descripcionUsuario(@PathVariable(value="id") Long idUsuario, Model model, HttpSession session) {
    UsuarioData usuario = usuarioService.findById(idUsuario);
    model.addAttribute("usuario", usuario);
    return "descripcionUsuario";
}
```

---

## 🔧 Cambios técnicos realizados

### Nuevos archivos creados:

- **aboutIn.html**: Página principal para usuarios que han iniciado sesión
- **listaUsuarios.html**: Página que muestra todos los usuarios
- **descripcionUsuario.html**: Página con detalles de cada usuario

### Nuevos controladores:

- **HomeController**: Maneja la navegación de la página principal
- **ListarUsuariosController**: Maneja la lista y perfiles de usuarios

### Nuevos métodos:

Se agregó un método en UsuarioService para obtener todos los usuarios:

```java
@Transactional(readOnly = true)
public List<UsuarioData> getAll(){
    List<Usuario> p = (List<Usuario>) usuarioRepository.findAll();
    if(p==null) {
        throw new TareaServiceException("No hay usuarios");
    }
    List<UsuarioData> usuarios = p.stream()
        .map(usuario -> modelMapper.map(usuario, UsuarioData.class))
        .collect(Collectors.toList());
    return usuarios;
}
```

---

## ✅ Verificación de funcionamiento

Para asegurar que todo funciona correctamente, se crearon pruebas automáticas que verifican:

- Que la barra de menú aparezca correctamente
- Que la lista de usuarios se cargue sin errores
- Que los perfiles de usuario muestren la información correcta
- Que todas las redirecciones funcionen como deben

## 🎯 Resumen

Estas mejoras hacen que la aplicación sea más completa y fácil de usar. Los usuarios ahora pueden:

- Navegar más fácilmente con la barra de menú
- Ver qué otros usuarios están registrados
- Conocer más detalles sobre cada usuario

La aplicación mantiene todas sus funciones originales de gestión de tareas, pero ahora tiene un aspecto más profesional y características sociales básicas.
