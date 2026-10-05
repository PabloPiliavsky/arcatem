---
name: afip-arca-webservices
description: Experto técnico en Web Services SOAP de ARCA / ex-AFIP (Argentina). Usar siempre que se planifique, diseñe, programe o depure la autenticación WSAA (TRA, firma CMS PKCS#7, Token/Sign de 12h), facturación electrónica WSFEv1 (RG 4291, FECAESolicitar, FECompUltimoAutorizado, FECompConsultar, FEDummy), condición de IVA del receptor (RG 5616), o errores SOAP de ARCA en homologación y producción.
---

# AFIP / ARCA Web Services Technical Expert Skill

## Rol y Propósito
Actúas como **Arquitecto y Especialista Técnico Senior en Integración con los Web Services de ARCA (ex-AFIP)** de la República Argentina. Tu responsabilidad es garantizar que toda la comunicación SOAP 1.1/1.2, firma criptográfica CMS (PKCS#7), manejo de Tickets de Acceso (TA) y emisión de comprobantes electrónicos cumpla estrictamente con `docs/constitution.md` y con los manuales y WSDLs oficiales alojados en esta misma carpeta (`.agent/skills/AFIP_WebServices_Expert_Skill/`).

---

## Documentación Oficial y WSDLs Locales Disponibles
Antes de asumir cualquier nombre de nodo XML o parámetro, consulta los archivos de referencia incluidos en este directorio:
- **Especificación y Manuales WSAA / Certificados**:
  - `Especificacion_Tecnica_WSAA_1.2.2.pdf`
  - `WSAAmanualDev.pdf`
  - `WSASS_manual.pdf` (Autogestión de certificados en Homologación)
  - `wsaa_obtener_certificado_produccion.pdf` y `wsaa_asociar_certificado_a_wsn_produccion.pdf`
  - `ADMINREL.DelegarWS.pdf`
- **Definiciones WSDL Exactas**:
  - `WSAA (Autenticación - Homologación).md` y `WSAA (Autenticación - Producción).md`
  - `WSFEv1 (Facturación - Homologación).md` y `WSFEv1 (Facturación - Producción).md`
  - `WSMTXCA (Matriz-Detalle - Homologación).md` y `WSMTXCA (Matriz-Detalle - Producción).md`
  - `WSBFEv1 (Bonos Fiscales - Homologación).md` y `WSSEG (Seguros de Caución - Homologación).md`

---

## Principios y Mejores Prácticas (Reglas de Oro de `constitution.md`)

### 1. Aislamiento Estricto de Entornos
Nunca mezclar certificados ni URLs entre Homologación y Producción. Configurar mediante `process.env.ARCA_ENV`:

| Servicio | Entorno Homologación (`homologacion`) | Entorno Producción (`produccion`) |
| :--- | :--- | :--- |
| **WSAA (`LoginCms`)** | `https://wsaahomo.afip.gov.ar/ws/services/LoginCms` | `https://wsaa.afip.gov.ar/ws/services/LoginCms` |
| **WSFEv1 (`ServiceSoap`)** | `https://wswhomo.afip.gov.ar/wsfev1/service.asmx` | `https://servicios1.afip.gov.ar/wsfev1/service.asmx` |

### 2. Autenticación WSAA y Caché de 12 Horas
1. **Construcción del TRA (`LoginTicketRequest.xml`)**:
   - `uniqueId`: Entero sin signo de 32 bits (`Math.floor(Date.now() / 1000)`).
   - `generationTime`: Fecha actual menos 10 minutos (en formato ISO 8601 con offset `-03:00`) para evitar rechazos por desincronización de reloj NTP.
   - `expirationTime`: Fecha actual más 12 horas (en formato ISO 8601 con offset `-03:00`).
   - `service`: `wsfe` (minúsculas, sin espacios).
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <loginTicketRequest version="1.0">
     <header>
       <uniqueId>1728150000</uniqueId>
       <generationTime>2026-10-05T16:50:00-03:00</generationTime>
       <expirationTime>2026-10-06T05:00:00-03:00</expirationTime>
     </header>
     <service>wsfe</service>
   </loginTicketRequest>
   ```
2. **Firma CMS (PKCS#7) Nativa / OpenSSL**:
   - Firmar el archivo o buffer `TRA.xml` usando el certificado X.509 (`.pem`/`.crt`) y la clave privada (`.key`) en formato CMS binario codificado en Base64 (equivalente a `openssl cms -sign -in TRA.xml -signer cert.pem -inkey private.key -nodetach -outform PEM`, quitando las cabeceras `-----BEGIN CMS-----` y `-----END CMS-----`).
3. **Llamada SOAP a `loginCms`**:
   ```xml
   <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsaa="http://wsaa.view.sua.dvadac.desein.afip.gov">
     <soapenv:Header/>
     <soapenv:Body>
       <wsaa:loginCms>
         <wsaa:in0>{CMS_BASE64_SIN_HEADERS}</wsaa:in0>
       </wsaa:loginCms>
     </soapenv:Body>
   </soapenv:Envelope>
   ```
4. **Caché Obligatorio del Ticket de Acceso (TA)**:
   - El XML devuelto dentro de `<loginCmsReturn>` contiene `<token>`, `<sign>` y `<expirationTime>`.
   - Guardar en caché persistente (MySQL / archivo local protegido). Reutilizar siempre mientras `Date.now() < Date.parse(expirationTime) - 5 * 60 * 1000`.
   - **Error `coe.alreadyAuthenticated`**: Ocurre si se solicita un nuevo TA cuando el anterior sigue vigente. Por eso la persistencia del TA es obligatoria.

### 3. Métodos Críticos de `WSFEv1` (Namespace `http://ar.gov.afip.dif.FEV1/`)

#### A. `FEDummy` (Salud de Servidores)
No requiere `<Auth>`. Verifica que `<AppServer>`, `<DbServer>` y `<AuthServer>` respondan `OK`.

#### B. `FECompUltimoAutorizado` (Sincronización de Numeración)
Obtiene el último `CbteNro` autorizado para un `PtoVta` y `CbteTipo`. El próximo comprobante a emitir será `CbteNro + 1`.
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ar="http://ar.gov.afip.dif.FEV1/">
  <soapenv:Header/>
  <soapenv:Body>
    <ar:FECompUltimoAutorizado>
      <ar:Auth>
        <ar:Token>{TOKEN}</ar:Token>
        <ar:Sign>{SIGN}</ar:Sign>
        <ar:Cuit>{CUIT_EMISOR}</ar:Cuit>
      </ar:Auth>
      <ar:PtoVta>{PTO_VTA}</ar:PtoVta>
      <ar:CbteTipo>{CBTE_TIPO}</ar:CbteTipo>
    </ar:FECompUltimoAutorizado>
  </soapenv:Body>
</soapenv:Envelope>
```

#### C. `FECAESolicitar` (Emisión de Comprobante y Obtención de CAE)
Estructura obligatoria con soporte para `CondicionIVAReceptorId` (RG 5616):
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ar="http://ar.gov.afip.dif.FEV1/">
  <soapenv:Header/>
  <soapenv:Body>
    <ar:FECAESolicitar>
      <ar:Auth>
        <ar:Token>{TOKEN}</ar:Token>
        <ar:Sign>{SIGN}</ar:Sign>
        <ar:Cuit>{CUIT_EMISOR}</ar:Cuit>
      </ar:Auth>
      <ar:FeCAEReq>
        <ar:FeCabReq>
          <ar:CantReg>1</ar:CantReg>
          <ar:PtoVta>{PTO_VTA}</ar:PtoVta>
          <ar:CbteTipo>{CBTE_TIPO}</ar:CbteTipo>
        </ar:FeCabReq>
        <ar:FeDetReq>
          <ar:FECAEDetRequest>
            <ar:Concepto>{CONCEPTO}</ar:Concepto>
            <ar:DocTipo>{DOC_TIPO}</ar:DocTipo>
            <ar:DocNro>{DOC_NRO}</ar:DocNro>
            <ar:CbteDesde>{CBTE_NRO}</ar:CbteDesde>
            <ar:CbteHasta>{CBTE_NRO}</ar:CbteHasta>
            <ar:CbteFch>{YYYYMMDD}</ar:CbteFch>
            <ar:ImpTotal>{IMP_TOTAL}</ar:ImpTotal>
            <ar:ImpTotConc>{IMP_TOT_CONC}</ar:ImpTotConc>
            <ar:ImpNeto>{IMP_NETO}</ar:ImpNeto>
            <ar:ImpOpEx>{IMP_OP_EX}</ar:ImpOpEx>
            <ar:ImpTrib>{IMP_TRIB}</ar:ImpTrib>
            <ar:ImpIVA>{IMP_IVA}</ar:ImpIVA>
            <ar:MonId>PES</ar:MonId>
            <ar:MonCotiz>1</ar:MonCotiz>
            <ar:CondicionIVAReceptorId>{CONDICION_IVA_RECEPTOR_ID}</ar:CondicionIVAReceptorId>
            <ar:Iva>
              <ar:AlicIva>
                <ar:Id>5</ar:Id>
                <ar:BaseImp>{BASE_IMP}</ar:BaseImp>
                <ar:Importe>{IMPORTE_IVA}</ar:Importe>
              </ar:AlicIva>
            </ar:Iva>
          </ar:FECAEDetRequest>
        </ar:FeDetReq>
      </ar:FeCAEReq>
    </ar:FECAESolicitar>
  </soapenv:Body>
</soapenv:Envelope>
```
> **Nota para Factura C (`CbteTipo = 11, 12, 13` - Monotributo)**: En comprobantes Clase C **NO** se informa el nodo `<ar:Iva>` y `<ar:ImpIVA>` debe ser `0.00`. Todo el subtotal va en `<ar:ImpNeto>` (o `<ar:ImpTotConc>`/`<ar:ImpOpEx>` según corresponda).
> **Nota para Servicios (`Concepto = 2` o `3`)**: Es obligatorio informar `<ar:FchServDesde>`, `<ar:FchServHasta>` y `<ar:FchVtoPago>` en formato `YYYYMMDD`.

#### D. `FECompConsultar` (Protocolo Obligatorio ante Timeouts)
Si `FECAESolicitar` sufre timeout o corte de red, es **obligatorio** invocar `FECompConsultar` pasando `CbteTipo`, `CbteNro` y `PtoVta` antes de reintentar. Si devuelve `Resultado = 'A'` y `CodAutorizacion`, se recupera el CAE y se marca la factura como aprobada en MySQL.

---

## Tablas de Referencia Rápida (WSFEv1)

### Tipos de Comprobante (`CbteTipo`)
- `1`: Factura A | `2`: Nota de Débito A | `3`: Nota de Crédito A
- `6`: Factura B | `7`: Nota de Débito B | `8`: Nota de Crédito B
- `11`: Factura C | `12`: Nota de Débito C | `13`: Nota de Crédito C

### Tipos de Documento (`DocTipo`)
- `80`: CUIT | `86`: CUIL | `96`: DNI | `99`: Sin identificar / Consumidor Final

### Alícuotas de IVA (`AlicIva.Id`)
- `3`: 0% | `4`: 10.5% | `5`: 21% | `6`: 27% | `8`: 5% | `9`: 2.5%

### Condición frente al IVA del Receptor (`CondicionIVAReceptorId` - RG 5616)
- `1`: IVA Responsable Inscripto (Receptor en Factura A / M)
- `4`: IVA Sujeto Exento (Receptor en Factura B / C)
- `5`: Consumidor Final (Receptor en Factura B / C)
- `6`: Responsable Monotributo (Receptor en Factura A / B / C)
- `7`: Sujeto No Categorizado
- `8`: Proveedor del Exterior
- `9`: Cliente del Exterior
- `10`: IVA Liberado – Ley Nº 19.640
- `13`: Monotributista Social
- `15`: IVA No Alcanzado
- `16`: Monotributo Trabajador Independiente Promovido

---

## Lo que NO se debe hacer (Anti-Patrones Prohibidos)
1. **NUNCA** invocar `loginCms` en cada venta. Agota el cupo de WSAA y lanza `coe.alreadyAuthenticated`.
2. **NUNCA** reintentar `FECAESolicitar` tras un error de red sin consultar antes `FECompConsultar`.
3. **NUNCA** enviar importes con más de 2 decimales o cuya suma (`ImpNeto + ImpTotConc + ImpOpEx + ImpTrib + ImpIVA`) difiera en centavos de `ImpTotal`.
4. **NUNCA** enviar el array `<Iva>` en comprobantes Tipo C (`11`, `12`, `13`).
5. **NUNCA** exponer archivos `.key` o `.pem` ni el `<Token>`/`<Sign>` al frontend.
