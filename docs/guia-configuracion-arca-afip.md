# Guía Paso a Paso: Qué Hacer y Qué Datos Obtener en tu Cuenta de ARCA (ex-AFIP)

Esta guía resume exactamente qué datos necesitas recopilar de tu cuenta de **ARCA (con Clave Fiscal Nivel 3 o superior)** y cómo generar los certificados digitales para **Homologación (Pruebas)** y **Producción (Facturación Real)** usando OpenSSL.

---

## 1. Cómo Buscar tus Datos Fiscales Dentro de tu Cuenta de ARCA (Paso a Paso)

Ingresa a **[https://auth.afip.gob.ar/contribuyente_/](https://auth.afip.gob.ar/contribuyente_/)** con tu CUIT y Clave Fiscal. Aquí tienes dónde encontrar exactamente cada variable de tu archivo `.env`:

### A. En el servicio **"Sistema Registral"** (Aquí están casi todos los datos)
1. En el buscador superior de tu panel de ARCA, escribe **"Sistema Registral"** y entra.
2. Si representas a más de una persona o empresa, selecciona tu CUIT.
3. Ve a **"Consulta"** $\rightarrow$ **"Datos Registrales"** (o **"Constancia de Inscripción"**):
   - **`ARCA_CUIT`**: Está arriba a la izquierda (son los 11 dígitos, escríbelos en el `.env` **sin guiones**, ej: `20345678901`).
   - **`EMISOR_RAZON_SOCIAL`**: Figura bajo *"Apellido y Nombre"* o *"Denominación / Razón Social"*.
   - **`EMISOR_CONDICION_IVA`**: Mira la sección **"Impuestos / Regímenes Nacionales"**:
     - Si dice *"IVA"* activo $\rightarrow$ pon `RESPONSABLE_INSCRIPTO`.
     - Si dice *"Monotributo"* activo $\rightarrow$ pon `MONOTRIBUTO`.
     - Si dice *"IVA Exento"* $\rightarrow$ pon `EXENTO`.
   - **`EMISOR_DOMICILIO_COMERCIAL`**: En la pestaña **"Domicilios"**, busca el que dice **"Fiscal"** o **"Locales y Establecimientos (Comercial)"** y copia la calle, número y localidad.
   - **`EMISOR_INICIO_ACTIVIDADES`**: En la pestaña **"Actividades Económicas"**, verás tu actividad principal y al lado el mes/año de inicio (ej. `05/2021` $\rightarrow$ en el `.env` pon `2021-05-01`).
   - **`EMISOR_IIBB` (Ingresos Brutos)**: En la misma Constancia de Inscripción (o en tu constancia de ARBA / AGIP / Rentas de tu provincia) figura tu Nº de Inscripción de Ingresos Brutos (suele ser tu mismo CUIT o un número de 10 dígitos). Si no estás inscripto aún o estás haciendo pruebas en Homologación, puedes dejar `Exento` o tu número de CUIT.

### B. En el servicio **"Administración de Puntos de Venta y Domicilios"** (`ARCA_PTO_VTA`)
1. **Si vas a probar en HOMOLOGACIÓN (`ARCA_ENV=homologacion`)**:
   - **No necesitas buscar ni crear nada en este menú**. Deja `ARCA_PTO_VTA=1` en el `.env`, porque el servidor de pruebas de ARCA acepta cualquier número de punto de venta automáticamente.
2. **Si vas a facturar en PRODUCCIÓN (`ARCA_ENV=produccion`)**:
   - En el buscador de tu panel de ARCA, escribe **"Administración de Puntos de Venta y Domicilios"** y entra.
   - Haz clic en el nombre de tu empresa $\rightarrow$ botón **"A/B/M de Puntos de Venta"**.
   - Ahí verás la tabla con tus puntos de venta actuales (por ejemplo, `00001 - Factura en Línea`).
   - El número que tenga como Sistema **"RECE para aplicativo y web services"** (o **"Factura Electrónica - Monotributo - Web Services"**) es el que va en `ARCA_PTO_VTA` (sin los ceros de adelante, ej. si es `00002`, pon `2`).

---

## 2. Paso a Paso para HOMOLOGACIÓN (Entorno de Pruebas Gratis)

ARCA provee un entorno de pruebas (`wswhomo.afip.gov.ar`) donde puedes emitir miles de facturas de prueba con CAE ficticio pero con las mismas validaciones que Producción.

### Paso 2.1: Habilitar el servicio "WSASS - Autogestión Certificados Homologación"
1. Entra a tu cuenta de ARCA con Clave Fiscal.
2. Ve a **"Administrador de Relaciones de Clave Fiscal"** -> **"Adherir Servicio"**.
3. Haz clic en el logo de **ARCA / AFIP** -> **"Servicios Interactivos"**.
4. Busca y selecciona **"WSASS - Autogestión Certificados Homologación"** y confirma la adhesión.
5. Cierra sesión y vuelve a entrar para verlo en tu panel principal.

### Paso 2.2: Generar tu Clave Privada (`.key`) y Pedido CSR (`.csr`) con OpenSSL en tu PC
Abre una terminal en una carpeta segura de tu computadora y ejecuta estos dos comandos con OpenSSL:

```bash
# 1. Generar la clave privada RSA de 2048 bits (¡NUNCA compartir este archivo!)
openssl genrsa -out privada_homo.key 2048

# 2. Generar el pedido de certificado (CSR) reemplazando TU_CUIT y TU_EMPRESA
openssl req -new -key privada_homo.key -subj "/C=AR/O=TU_EMPRESA/CN=arcatem_homo/serialNumber=CUIT 20123456789" -out pedido_homo.csr
```
> **Muy importante**: En `serialNumber=CUIT 20123456789` debe ir la palabra `CUIT` seguida de un espacio y los 11 números sin guiones.

### Paso 2.3: Subir el CSR a WSASS y Descargar el Certificado (`.pem` / `.crt`)
1. Entra al servicio **"WSASS - Autogestión Certificados Homologación"** en ARCA.
2. Ve al menú **"Nuevo Certificado"** (o *"Crear DN y obtener certificado"*).
3. En **Nombre simbólico del DN**, escribe por ejemplo: `arcatem_homo`.
4. Abre el archivo `pedido_homo.csr` con el Bloc de Notas, copia todo su texto (incluyendo `-----BEGIN CERTIFICATE REQUEST-----` y `-----END CERTIFICATE REQUEST-----`) y pégalo en el cuadro de texto de WSASS.
5. Haz clic en **"Crear DN y obtener Certificado"**.
6. ARCA te mostrará en pantalla un bloque de texto que empieza con `-----BEGIN CERTIFICATE-----`. Cópialo y guárdalo en un archivo llamado **`certificado_homo.pem`**.

### Paso 2.4: Asociar tu Certificado al Web Service `wsfe` en WSASS
1. Dentro de **WSASS**, ve al menú **"Crear autorización a servicio"** (a la izquierda).
2. En **Certificado (DN)**, selecciona el que acabas de crear (`arcatem_homo`).
3. En **Servicio al que desea acceder**, selecciona **`wsfe - Facturación Electrónica`**.
4. Haz clic en **"Crear Autorización"**.
5. ¡Listo! En Homologación no necesitas dar de alta un Punto de Venta en el padrón; puedes enviar cualquier `PtoVta` (por ejemplo `1` o `4`) en las pruebas.

---

## 3. Paso a Paso para PRODUCCIÓN (Cuando vayas a Emitir Facturas Reales)

Cuando el sistema esté probado en Homologación y quieras pasar a Producción:

### Paso 3.1: Generar Clave Privada y CSR de Producción
```bash
openssl genrsa -out privada_prod.key 2048
openssl req -new -key privada_prod.key -subj "/C=AR/O=TU_EMPRESA/CN=arcatem_prod/serialNumber=CUIT 20123456789" -out pedido_prod.csr
```

### Paso 3.2: Obtener el Certificado de Producción en ARCA
1. En el portal de ARCA, entra a **"Administrador de Relaciones de Clave Fiscal"** -> **"Adherir Servicio"** -> **ARCA** -> **"Servicios Interactivos"** -> **"Administración de Certificados Digitales"**.
2. Entra a **"Administración de Certificados Digitales"**, haz clic en **"Agregar Alias"** (ej. `arcatem_prod`), sube el archivo `pedido_prod.csr` y descarga el certificado firmado **`certificado_prod.crt`** (o `.pem`).

### Paso 3.3: Vincular el Certificado al Web Service de Facturación Electrónica (`wsfe`)
1. Ve a **"Administrador de Relaciones de Clave Fiscal"** -> **"Nueva Relación"**.
2. Haz clic en **"Buscar"** -> Logo **ARCA** -> **"WebServices"** -> Selecciona **"Factura Electrónica"** (`wsfe`).
3. En **"Representante"**, haz clic en **"Buscar"**, despliega el menú **"Computador Fiscal"**, elige tu alias (`arcatem_prod`) y confirma.

### Paso 3.4: Dar de Alta el Punto de Venta Web Service en ARCA
1. Entra al servicio **"Administración de Puntos de Venta y Domicilios"** en el portal de ARCA.
2. Selecciona tu empresa -> **"A/B/M de Puntos de Venta"** -> **"Agregar"**.
3. Elige un número de Punto de Venta nuevo (por ejemplo `2` o `3` si el `1` ya lo usabas para Factura en Línea manual).
4. En **"Sistema"**, selecciona:
   - Si eres **Responsable Inscripto**: *"RECE para aplicativo y web services"* (o *"Factura Electrónica - Monotributo - Web Services"* si eres **Monotributista**).
5. Asocia el domicilio comercial y guarda.

---

## 4. Resumen de Archivos que Necesitará Nuestro Backend (`.env`)
Una vez que tengas lo anterior, nuestro backend solo necesitará que coloques los dos archivos (`privada_homo.key` y `certificado_homo.pem`) en una carpeta local ignorada por Git y configures tu `.env`:
```env
ARCA_ENV=homologacion
ARCA_CUIT=20123456789
ARCA_PTO_VTA=1
ARCA_CERT_PATH=./certs/certificado_homo.pem
ARCA_KEY_PATH=./certs/privada_homo.key
```
