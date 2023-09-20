# 6. Pozos petroleros

<img src="./img/parte 2/ej6v2.png">

**Pozo** (<ins>#pozo</ins>, nombre, latitud, longitud, fecha_prod)

**Realiza** (#pozo, <ins>#monitoreo</ins>)

**Monitoreo** (<ins>#monitoreo</ins>, fecha, método)

**Mide** (<ins>#monitoreo, #parámetro</ins>)

**Parámetros** (<ins>#parámetro</ins>, nombre, valor_ref)

**Produce** (<ins>#monitoreo, #parámetro</ins>, #resultado)

**Resultado** (<ins>#resultado</ins>, valor)

**Usa** (<ins>#resultado</ins>, instrumento)

**Analógico** (<ins>#serie</ins>, fecha_calibracion)

**Digital** (<ins>#serie</ins>, marca, modelo)

# 7. Entrenamientos

<img src="./img/parte 2/ej7v2.png">

**Usuario** (<ins>email</ins>, nombre, peso, altura)

**Realiza** (<ins>email, #entrenamiento</ins>)

**Entrenamiento** (<ins>#entrenamiento</ins>, tiempo_total, calorías)

**Correr** (<ins>#entrenamiento</ins>, velocidad)

**Tiene** (<ins>email, #entrenamiento, #objetivo</ins>)

**Objetivo** (<ins>#objetivo</ins>, tiempo, porcentaje_obtenido)

**Gana** (<ins>email, #logro</ins>, fecha_obtención)

**Logro** (<ins>#logro</ins>, nombre, descripción)

**Obtiene** (<ins>#usuario, #logro, #premio</ins>)

**Premio** (<ins>#premio></ins>, fecha_obtención)

# 8. Empresa de Muebles

*// Considerando que un empleado puede ser responsable de más de un departamento*

<img src="./img/parte 2/ej8.png">


**Departamento** (<ins>#depto</ins>, nombre, prod_promedio)

**Trabaja** (<ins>#depto, dni</ins>)

**Responsable** (<ins>#depto</ins>, dni)

**Empleado** (<ins>dni</ins>, nombre, apellido, legajo)

**Realiza** (<ins>#depto, dni, #turno</ins>)

**Turno** (<ins>#turno</ins>, día_semana, hora_inicio, hora_fin)

**Especializa** (<ins>#depto</ins>, #mueble)

**Mueble** (<ins>#mueble</ins>, volumen, cant_horas)

**Formado_por** (<ins>#mueble, #material</ins>, cantidad)

**Material** (<ins>#mueble</ins>, nombre, stock_máximo)

# 9. Red Social

<img src="./img/parte 2/ej9.png">

**Usuario** (<ins>#usuario</ins>, username, nombre_completo, email)

**Participa_en** (<ins>#usuario, #álbum</ins>)

**Crea** (<ins>#álbum</ins>, #usuario)

**Álbum** (<ins>#álbum</ins>, nombre, fecha_creación, descripción)

**Sube** (<ins>#contenido</ins>, #álbum, #usuario)

**Contenido** (<ins>#contenido</ins>, fecha_publicación, comentario)

**Foto** (<ins>#contenido</ins>, resolución, formato)

**Video** (<ins>#contenido</ins>, duración)

**Contiene** (<ins>#publicación</ins>, #contenido)

**Publica** (<ins>#publicación</ins>, #usuario)

**Publicación** (<ins>#publicación</ins>, fecha)

**Tiene** (<ins>#publicación, #etiqueta</ins>)

**Etiqueta** (<ins>#etiqueta</ins>, nombre)

# 10. Hoteles

<img src="./img/parte 2/ej10v2.png">

**Hotel** (<ins>#hotel</ins>, nombre, estrellas, ubicación)

**Tiene** (<ins>#hotel, #habitación</ins>)

**Habitación** (<ins>#habitación</ins>, categoría, detalles, tipo)

**Publica** (<ins>#hotel, #habitación, #sitio</ins>)

**Sitio** (<ins>#sitio</ins>, nombre)

**Encuentra** (<ins>#hotel, #habitación, #sitio</ins>)

**Búsqueda** (<ins>#búsqueda</ins>, rango_fechas, cant_personas, precio)

**Busca** (<ins>#búsqueda, #usuario</ins>)

**Usuario** (<ins>#usuario</ins>, email, username, contraseña)

# 11. Red de Farmacias

<img src="./img/parte 2/ej11.png">

No voy a hacer el relacional disculpen💀