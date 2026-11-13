# Bitacora de tecnicas avanzadas 
 
Laboratorio 07: Tecnicas Avanzadas de Prompting. 
Herramienta de IA usada: (escribe aqui cual usaste) 
 
## Ejercicio 2: Zero-shot, one-shot y few-shot 

| Tipo | Aciertos (de 5) | Formato de la respuesta | Todas con el mismo formato (Si/No) | 
|------|-----------------|-------------------------|-----------------------------------| 
| Zero-shot |+ - N - + | Sin datos, pero da un argumento valido.| Si| 
| One-shot |+ - N - + | Acuerdo al ejemplo tiene su teoria y muestra resultados.| No| 
| Few-shot |+ - N - + | Argumentos claros y entendimiento del problema confirmados.| No| 

 
## Ejercicio 3: Chain of Thought 

| Pedido | Respuesta de la IA | Muestra los pasos (Si/No) | Correcta (Si/No) | 
|--------|--------------------|---------------------------|------------------| 
| Directo | 318.6| No| Si| 
| Paso a paso | El cliente paga en total S/ 318.60.| Si| Si| 

## Ejercicio 4: Role prompting 
 | Version | Vocabulario (sencillo/tecnico) | Usa ejemplos o codigo | A quien le sirve mas | 
|---------|-------------------------------|-----------------------|---------------------| 
| A. Sin rol | Sencillo| Ninguno| En general| 
| B. Rol docente | Sencillo| Uno| Un estudiante| 
| C. Rol senior | Tecnico| Uno| Un veterano| 

## Ejercicio 5: Descomposicion 
Con prompt sin paso a paso: 
inventario = {}
```java
def agregar_producto(nombre, precio, cantidad):
    inventario[nombre] = {"precio": precio, "cantidad": cantidad}
    print(f"Producto '{nombre}' agregado.")

def mostrar_inventario():
    for nombre, datos in inventario.items():
        print(f"{nombre} - Precio: S/ {datos['precio']} - Stock: {datos['cantidad']}")

def vender_producto(nombre, cantidad):
    if nombre in inventario and inventario[nombre]["cantidad"] >= cantidad:
        inventario[nombre]["cantidad"] -= cantidad
        print(f"Venta realizada. Quedan {inventario[nombre]['cantidad']} unidades.")
    else:
        print("Stock insuficiente o producto no encontrado.")

# Ejemplo de uso
agregar_producto("Arroz", 4.50, 20)
vender_producto("Arroz", 3)
mostrar_inventario()
```

Con prompt paso a paso, utilizando descomposicion:
```java
public class Producto {
    private String codigo;
    private String nombre;
    private String categoria;
    private double precioUnitario;
    private int stockActual;
    private int stockMinimo;

    // Constructor
    public Producto(String codigo, String nombre, String categoria, double precioUnitario, int stockActual, int stockMinimo) {
        this.codigo = codigo;
        this.nombre = nombre;
        this.categoria = categoria;
        this.precioUnitario = precioUnitario;
        this.stockActual = stockActual;
        this.stockMinimo = stockMinimo;
    }

    // Métodos Getters y Setters
    public String getCodigo() {
        return codigo;
    }

    public void setCodigo(String codigo) {
        this.codigo = codigo;
    }

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public String getCategoria() {
        return categoria;
    }

    public void setCategoria(String categoria) {
        this.categoria = categoria;
    }

    public double getPrecioUnitario() {
        return precioUnitario;
    }

    public void setPrecioUnitario(double precioUnitario) {
        this.precioUnitario = precioUnitario;
    }

    public int getStockActual() {
        return stockActual;
    }

    public void setStockActual(int stockActual) {
        this.stockActual = stockActual;
    }

    public int getStockMinimo() {
        return stockMinimo;
    }

    public void setStockMinimo(int stockMinimo) {
        this.stockMinimo = stockMinimo;
    }
}
```
## Ejercicio 6: Prompt estructurado y autocritica 
 | Preguntas propuestas | Resultados | 
|---------|---------------------| 
|¿Tiene las 4 columnas pedidas?  | Si|  
|¿Incluye el bloqueo después de 3 intentos?  | Si|
|¿Incluye casos con campos vacíos?  | Si|
|¿Indica qué casos agregó en la autocrítica?  | Si|
|¿Hay algún caso repetido o que no tenga sentido?  | No|


Prompt Estructurado:
```text
<rol>Actua como analista de pruebas de software.</rol> 
<contexto>Login web con correo y contrasena. La cuenta se bloquea despues de 3 intentos fallidos.</contexto> 
<tarea>Piensa paso a paso que puede fallar y escribe 6 casos de prueba.</tarea> 
<formato>Tabla con las columnas: ID, escenario, datos de entrada, resultado esperado.</formato> 
```

Mensaje de autocrítica:
```text
Revisión de la tabla anterior:
• Campos vacíos: Ya estaba cubierto en el CP-06.
• Correo sin @: Ya estaba cubierto en el CP-05 (usuario.empresacom).
• Contraseña con espacios: Faltaba. No incluí ningún caso límite que evalúe espacios en blanco al inicio, al final o una contraseña compuesta solo por espacios. Además, faltó probar un campo vacío de forma individual (correo lleno y contraseña vacía) y caracteres especiales / inyección SQL.
Casos límite agregados:
Se incorporan los casos CP-07, CP-08 y CP-09 para cubrir las brechas detectadas:

ID	Escenario	Datos de entrada	Resultado esperado
CP-07	(Agregado) Contraseña con espacios al inicio/final o solo espacios	Correo: usuario@empresa.com
Contraseña: "   " o " ClaveValida123! "	El sistema no hace recorte automático (trim) silencioso; rechaza el acceso por credenciales inválidas o notifica que los espacios no son válidos.
CP-08	(Agregado) Correo válido pero campo de contraseña vacío	Correo: usuario@empresa.com
Contraseña: "" (vacío)	El sistema bloquea el envío en el formulario, marca la contraseña como campo obligatorio y no suma un intento fallido.
CP-09	(Agregado) Intento de inyección SQL en el campo de correo	Correo: ' OR '1'='1
Contraseña: cualquiera	El sistema rechaza la entrada por formato inválido o sanitiza la consulta, impidiendo el acceso no autorizado.


Comparativa final de este último bloque

Para que tengas lista la conclusión de Prompt Estructurado y Autocrítica para tu profesor:

Enfoque	Calidad de salida	Cobertura de casos borde	Control de formato
Prompt Básico	Genérica y superficial.	Baja (solo casos obvios).	Texto libre desordenado.
Prompt Estructurado (XML)	Profesional, enfocada en la regla de negocio (3 intentos).	Media-alta (guiada por pensamiento paso a paso).	Tabla estricta con 4 columnas exactas.
+ Autocrítica	Exhaustiva y auditada.	Muy alta (detecta y rellena los huecos que faltaban).	Mantiene la tabla y resalta los cambios.
```

