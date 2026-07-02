# INF780 - Verificación y Validación de Software
## Tarea 4: Pruebas de Rendimiento con Apache JMeter

Este repositorio contiene el diseño, ejecución y documentación de pruebas de rendimiento (carga, estrés y picos) sobre la API REST `movies-api` desarrollada en NestJS, TypeORM y PostgreSQL.

**Estudiante:** Ronaldinho Rodriguez Romero  
**Docente:** M. Sc. Huáscar Fedor Gonzales Guzmán  
**Institución:** Universidad Autónoma Tomás Frías (UATF)  
**Carrera:** Ingeniería Informática  

---

## 🚀 1. Cómo Levantar la API Localmente

Para poder ejecutar las pruebas de rendimiento, primero asegúrese de tener la API corriendo localmente en el puerto `3000`.

### Requisitos Previos
* Node.js (v18 o superior recomendado)
* PostgreSQL levantado y configurado
* Datos de prueba cargados (mínimo 200+ registros mediante seeder o factory para asegurar mediciones realistas)

### Pasos para iniciar el servidor
1. Clonar el repositorio del proyecto e ingresar al directorio de la API:
   ```bash
   cd movies-api
   ```
2. Instalar las dependencias del proyecto:
   ```bash
   npm install
   ```
3. Configurar las variables de entorno en un archivo `.env` (Base de datos, puerto, etc.).
4. Ejecutar las migraciones y/o seeders para poblar la base de datos:
   ```bash
   npm run seed
   ```
5. Iniciar la aplicación en modo de desarrollo o producción:
   ```bash
   npm run start:dev
   ```
6. Verificar el correcto funcionamiento ingresando a [http://localhost:3000/movies](http://localhost:3000/movies).

---

## 📂 2. Estructura del Repositorio

De acuerdo con los requisitos de la práctica, el proyecto se organiza de la siguiente manera:

```text
├── informe/
│   └── Informe_Tarea4.pdf    # Informe técnico final con capturas y análisis estadístico
├── jmx/
│   ├── smoke.jmx              # Plan Base (Smoke Test)
│   ├── carga.jmx              # Plan de Carga (50 usuarios concurrentes)
│   ├── estres.jmx             # Plan de Estrés (Escalado progresivo: 100, 200, 400 hilos)
│   └── picos.jmx              # Plan de Picos (Arranque brusco y s utilizados por CSV Data Set Config
└── README.md                  # Este archivo con instrucciones de reproducción
```

---

## ⚡ 3. Ejecución de las Pruebas en Modo No-GUI (Consola)

> ⚠️ **IMPORTANTE:** Las pruebas de carga, estrés y picos deben ejecutarse exclusivamente desde la línea de comandos para evitar que la interfaz gráfica (GUI) de JMeter consuma recursos locales y altere la validez de los tiempos de respuesta y percentiles obtenidos.

A continuación se detallan los comandos específicos para ejecutar cada plan de pruebas y generar automáticamente su respectivo **Dashboard HTML**.

### A. Plan Base (Smoke Test) - `smoke.jmx`
Verifica que el plan funciona con una línea base mínima (1 hilo, 5 iteraciones).
```bash
jmeter -n -t jmx/smoke.jmx -l informe/smoke_results.jtl -e -o informe/dashboard-smoke/
```

### B. Prueba de Carga - `carga.jmx`
Simula la carga esperada en producción con 50 usuarios virtuales distribuidos en un ramp-up de 30 segundos y 10 iteraciones, empleando temporizadores aleatorios gaussianos y carga de IDs desde archivo CSV.
```bash
jmeter -n -t jmx/carga.jmx -l informe/carga_results.jtl -e -o informe/dashboard-carga/
```

### C. Prueba de Estrés - `estres.jmx`
Incrementa de forma masiva los usuarios (100, 200 y 400 usuarios simultáneos) para determinar el punto de quiebre y el **punto de saturación** del sistema (límite con un Error % menor al 1%).
```bash
jmeter -n -t jmx/estres.jmx -l informe/estres_results.jtl -e -o informe/dashboard-estres/
```

### D. Prueba de Picos y Aserciones - `picos.jmx`
Aplica un aumento brusco de demanda (200 usuarios en apenas 5 segundos) para evaluar la resiliencia de la API. Incorpora aserciones de código de estado (200/201) y aserciones de duración estricta (umbral de 800 ms).
```bash
jmeter -n -t jmx/picos.jmx -l informe/picos_results.jtl -e -o informe/dashboard-picos/
```

### 📋 Parámetros utilizados en los comandos:
* `-n`: Ejecuta JMeter en modo No-GUI (consola).
* `-t`: Especifica la ruta del archivo del Plan de Pruebas (.jmx).
* `-l`: Especifica la ruta del archivo de resultados crudos (.jtl) a generar.
* `-e`: Genera el reporte en Dashboard HTML al finalizar el test.
* `-o`: Carpeta de salida donde se guardará el Dashboard estático interactivo.

---

## 🛠️ 4. Entorno de Pruebas Utilizado

Para fines de reproducibilidad de los resultados estadísticos presentados en el informe, se detalla el entorno tecnológico:
* **Hardware:** CPU AMD Ryzen 5 / Intel Core i7, 16 GB RAM, SSD NVMe.
* **Sistema Operativo:** Windows 11 / Linux Ubuntu 22.04 LTS.
* **Herramientas:** Apache JMeter 5.6.3, Java OpenJDK 17.
* **Tecnología API:** Node.js v20, NestJS, NestJS TypeORM, PostgreSQL 15.

---

## 📌 5. Entrega en Plataforma y Git Tag

Una vez verificado que todos los archivos están cargados correctamente, la versión de entrega queda congelada con la etiqueta de Git oficial:

```bash
git tag tarea4
git push origin --tags