---
name: pos-fiscal-qr-print
description: Especialista en generación de Código QR Oficial de ARCA (RG 4892), firma criptográfica CMS (PKCS#7 con OpenSSL/node:crypto), validación de CUIT (Módulo 11) y diseño de comprobantes gráficos (A4 y ticket térmico 80mm) con CAE. Usar al implementar la firma del TRA, el generador de QR o las vistas de impresión de facturas en React/Node.
---

# Criptografía CMS, Código QR Oficial (RG 4892) e Impresión Fiscal POS

## Principios y Mejores Prácticas

### 1. Regla de Oro (según `docs/constitution.md`)
Todo comprobante autorizado por ARCA debe almacenar su CAE, fecha de vencimiento de CAE y generar el **Código QR Oficial obligatorio (RG 4892)** sin recurrir a librerías pagas, junto con la validación previa de CUIT/CUIL por algoritmo Módulo 11.

### 2. Patrones Recomendados

#### A. Especificación Oficial del Código QR de ARCA (RG 4892)
La URL codificada dentro del QR debe tener exactamente el formato:
`https://www.afip.gob.ar/fe/qr/?p={BASE64_JSON}`

Donde `{BASE64_JSON}` es la codificación en Base64 (UTF-8) de un JSON con esta estructura exacta:
```javascript
export default function buildArcaQrUrl(voucher) {
  const payload = {
    ver: 1,
    fecha: voucher.fechaEmisionIso, // "YYYY-MM-DD"
    cuit: Number(voucher.cuitEmisor), // 11 dígitos sin guiones (numérico)
    ptoVta: Number(voucher.ptoVta),
    tipoCmp: Number(voucher.cbteTipo),
    nroCmp: Number(voucher.cbteNro),
    importe: Number(voucher.impTotal.toFixed(2)),
    moneda: voucher.monId || 'PES',
    ctz: Number(voucher.monCotiz || 1),
    tipoDocRec: Number(voucher.docTipoReceptor),
    nroDocRec: Number(voucher.docNroReceptor),
    tipoCodAut: 'E', // 'E' para CAE, 'A' para CAEA
    codAut: Number(voucher.cae)
  }
  const base64Payload = Buffer.from(JSON.stringify(payload), 'utf-8').toString('base64')
  return `https://www.afip.gob.ar/fe/qr/?p=${base64Payload}`
}
```

#### B. Firma Criptográfica CMS (PKCS#7) sin Dependencias Pagas
Para firmar el `LoginTicketRequest.xml` (TRA) en el backend Node.js usando OpenSSL nativo del sistema o `node-forge`:
```javascript
import { execFile } from 'node:child_process'
import { promisify } from 'node:util'

const execFileAsync = promisify(execFile)

export default async function signTraWithOpenSsl(traPath, certPath, keyPath) {
  const { stdout } = await execFileAsync('openssl', [
    'cms', '-sign',
    '-in', traPath,
    '-signer', certPath,
    '-inkey', keyPath,
    '-nodetach',
    '-outform', 'PEM'
  ])
  return stdout
    .replace(/-----BEGIN CMS-----/g, '')
    .replace(/-----END CMS-----/g, '')
    .replace(/\r?\n|\r/g, '')
    .trim()
}
```

#### C. Validación de CUIT / CUIL (Algoritmo Módulo 11)
Validar tanto en Frontend como en Backend antes de enviar a `WSFEv1` para evitar rechazos SOAP innecesarios:
```javascript
export default function isValidCuit(cuitRaw) {
  const cuit = String(cuitRaw).replace(/\D/g, '')
  if (cuit.length !== 11) return false
  const multipliers = [5, 4, 3, 2, 7, 6, 5, 4, 3, 2]
  const sum = multipliers.reduce((acc, mult, idx) => acc + Number(cuit[idx]) * mult, 0)
  const mod = 11 - (sum % 11)
  const checkDigit = mod === 11 ? 0 : mod === 10 ? 9 : mod
  return checkDigit === Number(cuit[10])
}
```

#### D. Requisitos del Comprobante Gráfico (A4 y Ticket Térmico 80mm)
- Letra del comprobante destacada en cabecera (`A`, `B` o `C`) con su código numérico (`COD. 001`, `COD. 006`, `COD. 011`).
- Datos del Emisor (Razón Social, CUIT, Ingresos Brutos, Inicio de Actividades, Condición frente al IVA).
- Datos del Receptor (Nombre/Razón Social, Tipo y Nro de Documento, Condición frente al IVA según RG 5616).
- En **Factura A**: Discriminación obligatoria de Subtotal Neto e IVA por alícuota (`21%`, `10.5%`, etc.).
- En **Factura B**: Régimen de Transparencia Fiscal al Consumidor (Ley 27.743 / RG 5614) informando el "IVA Contenido" en el pie del comprobante sin discriminarlo en el precio unitario.
- Pie fiscal obligatorio: Imagen del **Código QR Oficial**, leyenda *"Comprobante Autorizado"*, número de **CAE** y **Fecha de Vencimiento de CAE**.

### 3. Lo que NO se debe hacer
- **NUNCA** enviar el `cuit` o `codAut` como strings con guiones dentro del JSON del QR de ARCA; la especificación técnica exige tipo numérico (`Number`).
- **NUNCA** incluir saltos de línea ni cabeceras `-----BEGIN CMS-----` dentro del nodo `<in0>` de `loginCms`.
