# 6. Pozos petroleros

<img src="./img/parte 2/ej6.png">

**Pozo** (<ins>#pozo</ins>, nombre, latitud, longitud, fecha_prod)

**Realiza** (#pozo, <ins>#monitoreo</ins>)

**Monitoreo** (<ins>#monitoreo</ins>, fecha, método)

**Mide** (<ins>#monitoreo, #parámetro</ins>)

**Parámetros** (<ins>#parámetro</ins>, nombre, valor_ref)

**Usa** (<ins>#monitoreo, #parámetro, #serie</ins>, valor)

**Analógico** (<ins>#serie</ins>, fecha_calibracion)

**Digital** (<ins>#serie</ins>, marca, modelo)

# 7. Entrenamientos

<img src="./img/parte 2/ej7.png">

**Usuario** (<ins>email</ins>, nombre, peso, altura)

**Realiza** (<ins>email, #entrenamiento</ins>)

**Entrenamiento** (<ins>#entrenamiento</ins>, tiempo_total, calorías)

**Correr** (<ins>#entrenamiento</ins>, velocidad)

**Tiene** (<ins>email, #entrenamiento, #objetivo</ins>)

**Objetivo** (<ins>#objetivo</ins>, tiempo, porcentaje_obtenido)

**Obtiene** (<ins>email, #logro</ins>, fecha_obtención)

**Logro** (<ins>#logro</ins>, nombre, descripción)