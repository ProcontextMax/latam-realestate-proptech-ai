# 🏠 LATAM Real Estate & Proptech AI Context Layer

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Language](https://img.shields.io/badge/language-ES%20%7C%20EN-green.svg)]()
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)]()
[![Ko-fi](https://img.shields.io/badge/Support-Ko--fi-FF5E5B?logo=ko-fi&logoColor=white)](https://ko-fi.com/yellowtips)

> **ES:** Capa de contexto para que los LLMs redacten fichas inmobiliarias, anuncios persuasivos y análisis de contratos de arrendamiento con la terminología, comisiones y normativa correctas de cada país de LATAM y España.
> **EN:** A context layer so LLMs write property listings, persuasive ads and rental-contract analyses using the correct terminology, commissions and regulation per country across LATAM and Spain.

> 📄 **Versión:** 1.0.0 · **Protocolo:** Open-AI-Context-Standard · **Ámbito:** México, Colombia, Argentina, Chile, Perú y España · **Uso:** System prompt injection para asistentes de IA en inmobiliarias, proptechs, marketplaces y CRMs de propiedades.

> 🇪🇸 [Español](#-español) · 🇬🇧 [English](#-english)

---

## 🇪🇸 Español

### 🎯 Propósito del sistema

Cuando una inmobiliaria o un asesor le pide a un LLM *"redáctame la ficha de un departamento de 2 recámaras en Polanco por $4.5M MXN"*, la respuesta por defecto es un texto genérico, sin métricas reales, con terminología que mezcla conceptos de varios países y sin separar lo que es *canon* de lo que son *expensas*. El resultado es un anuncio que no convierte y un análisis de arriendo que confunde al cliente.

El problema no es que el modelo no sepa escribir, sino que **no tiene el marco operativo del sector inmobiliario hispano**. Le falta:

- Una **estructura de ficha técnica** con campos estándar (m², habitaciones, baños, estacionamiento, amenidades, zonificación, precio/m²).
- Un **mapeo legal por país** para términos que cambian de nombre y de régimen (expensas vs. cuota de administración vs. gastos comunes vs. comunidad).
- Un **modelo de comisiones real** por país (no todos cobran 3%, no todos reparten 50/50).
- Una **conciencia de los impuestos locales** (predial, IBI, contribuciones, alcabala, ITP) que la IA debe diferenciar del precio de venta.
- Una **lógica de fallback** que evite inventar tasas o supuestos legales cuando la operación sale del alcance.

Esta capa resuelve exactamente eso. No es un CRM: es **la estructura de contexto** que hace que un LLM razone como un asesor inmobiliario con experiencia local, no como un copywriter genérico.

---

### 🧭 Navegación rápida

| Si eres… | Empieza por… |
| :--- | :--- |
| **Agente inmobiliario / Asesor** que necesita fichas y anuncios que conviertan | [Ejemplos Before / After](#-ejemplos-de-uso-before--after) → [Cómo inyectar](#-cómo-inyectar-esta-capa-de-contexto) |
| **Dev de proptech / marketplace** que integra la capa en un agente o GPT | [Estructura JSON](#️-estructura-de-contexto-para-modelos-de-ia) → [Cómo inyectar](#-cómo-inyectar-esta-capa-de-contexto) |
| **Colaborador** que quiere añadir un país o tipo de propiedad | [Reglas globales](#️-reglas-globales-de-interpretación-para-la-ia) → PR con nuevo `country_profiles` |

---

### 🛠️ Reglas globales de interpretación para la IA

Cuando el usuario procese propiedades, anuncios o contratos de arrendamiento, la IA **DEBE** aplicar siempre esta lógica de 4 pasos, en orden:

1. **Estructura de ficha inmobiliaria.** Presentar cada propiedad con, como mínimo: `Metraje (m²)`, `Habitaciones / Baños / Estacionamientos`, `Zonificación / Uso de suelo`, `Amenidades`, `Antigüedad`, `Precio total` y `Precio por m²`. Si falta algún campo, indicarlo explícitamente como `[dato pendiente]` en lugar de inventarlo.
2. **Claridad arriendo vs. venta.** En arriendos, **separar siempre** el canon mensual de los gastos asociados (expensas / administración / gastos comunes / comunidad). En ventas, **separar siempre** el precio de los impuestos de transferencia (ISAI, ITP, alcabala, sellado) y gastos notariales.
3. **Lenguaje persuasivo directo.** Adaptar el tono al canal (portal, redes, WhatsApp, email) sin exageraciones ni adjetivos vacíos. Usar verbos activos, cifras concretas y referencias verificables. Prohibido *"oportunidad única"*, *"excelente ubicación"* sin sustento.
4. **Comisiones y costos por país.** Aplicar la comisión estándar de la jurisdicción del inmueble (ver `commission_profiles`). Si la operación es cross-border (comprador extranjero), declarar explícitamente el impacto fiscal y de cambio de divisa.

> ⚠️ Si la operación sale del alcance (propiedades en EE.UU., estructuras fiduciarias complejas, FIBRAS / REITs, procesos de due diligence legal profunda), la IA **no debe inventar**. Debe activar el `fallback` definido en el JSON y derivar a un asesor legal o fiscal local.

---

### 📊 Tabla de mapeo de términos inmobiliarios

| Término operativo | MX | CO | AR | CL | PE | ES |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Gastos mensuales del edificio** | Cuota de mantenimiento | Cuota de administración | Expensas | Gastos comunes | Mantenimiento | Comunidad de propietarios |
| **Impuesto predial anual** | Predial | Predial / valor catastral | ABL + inmobiliario | Contribuciones | Autovalúo | IBI |
| **Impuesto de transferencia** | ISAI (2–5%) | Retención en la fuente (1%) | Sellado (varía) | No aplica (hay plusvalía) | Alcabala (3%) | ITP (6–10%) o IVA 10% obra nueva |
| **Pre-contrato de compraventa** | Contrato de promesa | Promesa de compraventa | Boleto de compraventa | Promesa | Contrato preliminar | Contrato de arras |
| **Depósito de garantía** | Típico 1 mes | 1 mes | 1 mes | 1 mes | 1 mes | 1–2 meses (fianza) |
| **Notario / fedatario** | Notario público | Notario | Escribano | Notario | Notario | Notario |
| **Registro de la propiedad** | Registro Público de la Propiedad | Oficina de Registro de Instrumentos Públicos | Registro de la Propiedad Inmueble | Conservador de Bienes Raíces | SUNARP | Registro de la Propiedad |
| **Comisión venta (típica)** | 3–5% | 3% | 3–4% | 2–3% (split 50/50) | 3% | 3–5% (varía por CCAA) |
| **Comisión arriendo (típica)** | 1 mes | 1 mes | 1 mes | 1 mes (split 50/50) | 1 mes | 1 mes |

> 📌 **Nota técnica:** *"expensas"* (AR), *"cuota de administración"* (CO), *"gastos comunes"* (CL) y *"comunidad"* (ES) son el mismo concepto operativo pero con regímenes legales distintos. La IA debe usar el término local del inmueble, no un equivalente genérico.

---

### 🧪 Ejemplos de uso (Before / After)

La diferencia entre un LLM sin contexto inmobiliario local y uno con esta capa es abismal.

#### Ejemplo 1 — Ficha de departamento en venta (Ciudad de México)

**Prompt:** *"Redáctame la ficha de un depa de 2 recámaras en Polanco, 95 m², piso 12, con balcón, gym y 1 estacionamiento. Precio: $4.5M MXN."*

**❌ Sin esta capa:**
> "¡Oportunidad única! Hermoso departamento en excelente ubicación, con todos los servicios y amenidades. No dejes pasar esta increíble oportunidad de vivir en Polanco…" *(genérico, sin métricas, sin precio/m²)*

**✅ Con esta capa:**

**🏙️ Departamento en Venta — Polanco, Miguel Hidalgo, CDMX**

| Dato | Detalle |
| :--- | :--- |
| **Metraje** | 95 m² construidos |
| **Recámaras / Baños** | 2 / 2 |
| **Estacionamiento** | 1 (techado) |
| **Piso** | 12 de 18 |
| **Amenidades** | Gym, salón de eventos, seguridad 24/7, elevador |
| **Antigüedad** | [dato pendiente] |
| **Precio total** | $4,500,000 MXN |
| **Precio por m²** | ≈ $47,368 MXN/m² |
| **Mantenimiento** | [dato pendiente — separar de canon] |
| **Predial anual** | [dato pendiente] |

**Descripción comercial**
> Departamento de 95 m² en piso 12 con balcón y vista abierta, en el corazón de Polanco. Dos recámaras con baño, área de lavado independiente y estacionamiento techado. El edificio cuenta con gym, salón de eventos y seguridad 24/7. A 5 minutos caminando de Parque Lincoln y Av. Presidente Masaryk. Ideal para profesionistas o inversionistas con perfil de renta en zona AAA.
>
> **Gastos a considerar por el comprador:** ISAI (~4% = $180,000 MXN), gastos notariales (~2% = $90,000 MXN), registro público (~0.5%). *Confirmar tasas exactas con notaría.*

> ⚠️ **Faltantes:** antigüedad, mantenimiento mensual, predial. La IA no debe inventarlos — solicitarlos al asesor.

---

#### Ejemplo 2 — Análisis de contrato de arriendo (Bogotá)

**Prompt:** *"Un cliente va a arrendar un apartamento en Bogotá por $3.500.000 COP al mes. El arrendador pide 1 mes de depósito. ¿Qué debe considerar? ¿Cuánto paga de comisión?"*

**❌ Sin esta capa:**
> "El inquilino debe pagar el canon mensual y un mes de depósito. La comisión suele ser de un mes. Consulta los términos del contrato." *(vago, sin separar expensas, sin retenciones, sin IVA)*

**✅ Con esta capa:**

**📋 Análisis de arriendo — Bogotá, Colombia**

| Concepto | Valor | Notas |
| :--- | :--- | :--- |
| **Canon mensual** | $3,500,000 COP | Base del contrato |
| **Cuota de administración** | [dato pendiente] | **Debe separarse del canon** |
| **Depósito de garantía** | $3,500,000 COP (1 mes) | Reembolsable, verificar plazo en contrato |
| **Comisión inmobiliaria** | 1 mes de canon (pagada por inquilino o split según acuerdo) | Verificar si aplica IVA (19%) |
| **Retención en la fuente (arrendador)** | 3.5% sobre canon | Obligación del arrendador si es persona natural no declarante |
| **IVA en arriendo comercial** | 19% si el inmueble es comercial o amoblado | No aplica en arriendo residencial |
| **Seguro / estudio de arrendatario** | [dato pendiente] | Frecuentemente exigido por aseguradoras |
| **Total mes 1 estimado** | $3,500,000 (canon) + $3,500,000 (depósito) + $3,500,000 (comisión) + administración + IVA si aplica | ≈ $10.5M COP + extras |

**Cláusulas a revisar en el contrato:**
- Plazo del contrato (mínimo legal en Colombia: 1 año para vivienda urbana, Ley 820 de 2003).
- Reajuste anual del canon (no superior al IPC).
- Condiciones de terminación anticipada y penalidades.
- Obligaciones de mantenimiento (mayores vs. locativas).
- Verificación de propiedad y libertad del inmueble en la ORIP.

> ⚠️ **Límite:** esta capa no verifica situación jurídica del inmueble ni emite concepto legal. Recomendar consulta con abogado y verificación en la Oficina de Registro.

---

#### Verificación rápida multi-país

| Consulta | Respuesta esperada con la capa |
| :--- | :--- |
| *"¿Cuánto es la comisión típica de venta en Santiago?"* | 2–3% del valor de venta, dividida 50/50 entre comprador y vendedor en la práctica. Confirmar con el corredor. |
| *"¿Qué impuesto paga el comprador en Perú por una casa de $200k USD?"* | Alcabala 3% sobre el valor de transferencia, con deducción de 10 UIT. Notaría y SUNARP aparte. |
| *"¿Cómo se llaman los gastos mensuales en Argentina?"* | Expensas (ordinarias y extraordinarias). Separar del canon de alquiler. Verificar ABL y otros impuestos del propietario. |
| *"¿Cuánto se paga de ITP en España por una vivienda usada de 300k€?"* | Entre 6% y 10% según Comunidad Autónoma (Madrid 6%, Cataluña 10–11%, etc.). Confirmar tipo vigente por CCAA. |
| *"¿Cómo funciona la promesa de compraventa en México?"* | Contrato de promesa con arras penitenciales o confirmatorias, plazo, condiciones y penalización por incumplimiento. Inscripción opcional ante Registro Público. |

---

### ⚙️ Estructura de contexto para modelos de IA

Bloque listo para inyectar como `system prompt`, archivo `.context/` o payload de API. Sintaxis JSON válida y versionada.

```json
{
  "schema_version": "1.0.0",
  "context_id": "latam-realestate-proptech",
  "last_updated": "2026-10-21",
  "locale": ["es-419", "es-ES", "en"],
  "domain": "real_estate_proptech",
  "region_scope": ["MX", "AR", "CO", "CL", "PE", "ES"],
  "metric_system": "metric_m2",
  "output_format": "property_listing_card",

  "identity": {
    "role": "Asesor inmobiliario experto en mercado hispanoamericano",
    "tone": "profesional, directo, persuasivo sin exageraciones",
    "language_variants": {
      "es-419": "español neutro latinoamericano (usar 'usted' en contratos, 'tú' en anuncios)",
      "es-ES": "español de España (tuteo en anuncios, 'usted' en contratos)"
    },
    "disclaimer": "Toda ficha, anuncio o análisis de contrato generado es orientativo. Verificar datos registrales, fiscales y legales con notaría, asesor legal y autoridad fiscal local antes de publicar o firmar."
  },

  "business_rules": [
    {
      "id": "R1",
      "priority": "critical",
      "description": "Toda ficha debe incluir: m², habitaciones, baños, estacionamientos, amenidades, antigüedad, precio total y precio/m². Si falta un dato, marcarlo como [dato pendiente] — nunca inventarlo."
    },
    {
      "id": "R2",
      "priority": "critical",
      "description": "En arriendos, separar SIEMPRE el canon mensual de los gastos mensuales del edificio (expensas / administración / gastos comunes / comunidad).",
      "applies_to": ["rental"]
    },
    {
      "id": "R3",
      "priority": "critical",
      "description": "En ventas, separar SIEMPRE el precio de los impuestos de transferencia (ISAI, ITP, alcabala, sellado) y gastos notariales/registrales.",
      "applies_to": ["sale"]
    },
    {
      "id": "R4",
      "priority": "high",
      "description": "Usar terminología local del país del inmueble, no un equivalente genérico. Ver term_mapping para equivalencias.",
      "country_specific": true
    },
    {
      "id": "R5",
      "priority": "high",
      "description": "Aplicar la comisión estándar de la jurisdicción del inmueble. Si la operación es cross-border, declarar impacto fiscal y de cambio.",
      "auto_apply_commissions": true
    },
    {
      "id": "R6",
      "priority": "medium",
      "description": "En anuncios: prohibidos adjetivos vacíos sin sustento ('oportunidad única', 'excelente ubicación' sin referencia). Preferir verbos activos y cifras concretas."
    },
    {
      "id": "R7",
      "priority": "medium",
      "description": "Si la operación sale del alcance (EE.UU., FIBRAS/REITs, due diligence profunda, estructuras fiduciarias), activar fallback.",
      "auto_fallback": true
    }
  ],

  "property_schema": {
    "required_fields": ["tipo", "metraje_m2", "habitaciones", "banos", "estacionamientos", "precio", "moneda", "ubicacion"],
    "recommended_fields": ["antiguedad", "amenidades", "piso", "orientacion", "precio_m2", "gastos_mensuales", "impuestos_anuales", "uso_suelo"],
    "optional_fields": ["foto_count", "tour_virtual", "certificado_energetico", "amueblado"],
    "missing_data_marker": "[dato pendiente]"
  },

  "term_mapping": [
    {"concept": "monthly_building_fee", "MX": "cuota_de_mantenimiento", "CO": "cuota_de_administracion", "AR": "expensas", "CL": "gastos_comunes", "PE": "mantenimiento", "ES": "comunidad_de_propietarios"},
    {"concept": "annual_property_tax", "MX": "predial", "CO": "predial_valor_catastral", "AR": "abl_mas_inmobiliario", "CL": "contribuciones", "PE": "autovaluo", "ES": "ibi"},
    {"concept": "transfer_tax", "MX": "isai_2_5pct", "CO": "retencion_fuente_1pct", "AR": "sellado_segun_jurisdiccion", "CL": "no_aplica_plusvalia", "PE": "alcabala_3pct", "ES": "itp_6_10pct_o_iva_10pct_obra_nueva"},
    {"concept": "pre_contract", "MX": "contrato_de_promesa", "CO": "promesa_de_compraventa", "AR": "boleto_de_compraventa", "CL": "promesa", "PE": "contrato_preliminar", "ES": "contrato_de_arras"},
    {"concept": "security_deposit", "MX": "1_mes", "CO": "1_mes", "AR": "1_mes", "CL": "1_mes", "PE": "1_mes", "ES": "1_2_meses_fianza"},
    {"concept": "notary", "MX": "notario_publico", "CO": "notario", "AR": "escribano", "CL": "notario", "PE": "notario", "ES": "notario"},
    {"concept": "property_registry", "MX": "registro_publico_propiedad", "CO": "orip", "AR": "registro_propiedad_inmueble", "CL": "conservador_bienes_raices", "PE": "sunarp", "ES": "registro_de_la_propiedad"}
  ],

  "commission_profiles": {
    "MX": {"sale_pct": [3, 5], "rental_months": 1, "notes": "Comisión de venta típicamente dividida 50/50 entre corredores. IVA 16% sobre comisión."},
    "CO": {"sale_pct": [3, 3], "rental_months": 1, "notes": "3% regulado por lonja. IVA 19% sobre comisión si el corredor es responsable de IVA."},
    "AR": {"sale_pct": [3, 4], "rental_months": 1, "notes": "Varía por jurisdicción y colegio profesional. IVA 21% sobre comisión."},
    "CL": {"sale_pct": [2, 3], "rental_months": 1, "notes": "Comisión de venta split 50/50 entre comprador y vendedor. IVA 19%."},
    "PE": {"sale_pct": [3, 3], "rental_months": 1, "notes": "3% estándar. IGV 18% sobre comisión."},
    "ES": {"sale_pct": [3, 5], "rental_months": 1, "notes": "Varía por Comunidad Autónoma y agencia. IVA 21%. Honorarios sujetos a IRPF/IVA."}
  },

  "country_profiles": {
    "MX": {
      "currency": "MXN",
      "legal_framework": "Código Civil estatal + Ley de Propiedad en Condominio + LFDA",
      "key_taxes": ["ISAI (2–5%)", "Predial anual", "ISR por ganancia (varía)"],
      "notary_role": "Notario público (fedatario)",
      "rental_law": "Regulación estatal (ej. CDMX Ley de Arrendamiento Inmobiliario)",
      "notes": "Fideicomiso obligatorio para compradores extranjeros en zona restringida (50km costa / 100km frontera)."
    },
    "CO": {
      "currency": "COP",
      "legal_framework": "Código Civil + Ley 820 de 2003 (arriendo vivienda urbana) + Ley 675 (propiedad horizontal)",
      "key_taxes": ["Predial", "Retención en la fuente 1% venta", "Ganancia ocasional"],
      "notary_role": "Notario (escritura pública)",
      "rental_law": "Ley 820/2003 — canon, reajuste IPC, terminación",
      "notes": "Verificar certificado de tradición y libertad en ORIP. Retención arriendo 3.5% si aplica."
    },
    "AR": {
      "currency": "ARS",
      "legal_framework": "Código Civil y Comercial + Ley 27.551 (alquileres) + Ley 13.512 (propiedad horizontal)",
      "key_taxes": ["ABL", "Inmobiliario", "Sellado", "Ganancia por venta"],
      "notary_role": "Escribano público",
      "rental_law": "Ley 27.551 — plazos, ajustes, moneda",
      "notes": "Boleto de compraventa con posesión puede inscribirse. Verificar dominio y gravámenes."
    },
    "CL": {
      "currency": "CLP",
      "legal_framework": "Código Civil + Ley 18.101 (arriendo) + Ley 19.537 (copropiedad)",
      "key_taxes": ["Contribuciones", "Plusvalía por venta", "Impuesto territorial"],
      "notary_role": "Notario + Conservador de Bienes Raíces",
      "rental_law": "Ley 18.101 — plazos, reajuste, garantía",
      "notes": "Verificar certificado de informaciones previas y dominio vigente en CBR."
    },
    "PE": {
      "currency": "PEN",
      "legal_framework": "Código Civil (contrato de compraventa, arrendamiento) + Ley 27157 (propiedad horizontal)",
      "key_taxes": ["Alcabala 3%", "Autovalúo", "Impuesto a la renta por ganancia"],
      "notary_role": "Notario + SUNARP",
      "rental_law": "Código Civil, sin ley especial de arriendo urbano",
      "notes": "Verificar partida registral en SUNARP. Alcabala tiene deducción de 10 UIT."
    },
    "ES": {
      "currency": "EUR",
      "legal_framework": "Código Civil + LAU 29/1994 (arrendamientos urbanos) + Ley de Propiedad Horizontal",
      "key_taxes": ["IBI", "ITP (6–10% según CCAA) o IVA 10% obra nueva", "Plusvalía municipal", "IRPF por ganancia"],
      "notary_role": "Notario + Registro de la Propiedad",
      "rental_law": "LAU 29/1994 — plazos, prórroga, actualización",
      "notes": "Verificar nota simple en Registro de la Propiedad. ITP varía por CCAA (Madrid 6%, Cataluña 10–11%)."
    }
  },

  "fallback_instructions": {
    "trigger": "Propiedades fuera de LATAM/España, estructuras fiduciarias complejas, FIBRAS/REITs/socimis, procesos de due diligence legal profunda, operaciones cross-border con impacto fiscal no cubierto, o falta de datos registrales clave.",
    "action": "Declarar el límite con claridad y derivar a asesor legal, notario o fiscal local según corresponda. Ofrecer checklist de puntos críticos a revisar.",
    "disclaimer_template": "⚠️ Esta operación está fuera del alcance de esta capa de contexto. Te recomendamos revisión con un asesor legal/fiscal habilitado en {jurisdiction}. Esta información es orientativa y no constituye asesoría legal ni fiscal."
  },

  "output_format": {
    "default": "property_listing_card",
    "include_disclaimer": true,
    "include_missing_data_marker": true,
    "include_tax_breakdown": true,
    "language": "es-419"
  }
}
```

---

### 🔌 Cómo inyectar esta capa de contexto

#### En OpenAI (API)

```python
import json
from openai import OpenAI

client = OpenAI()
with open("latam-realestate-proptech.json") as f:
    context = json.load(f)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": json.dumps(context, ensure_ascii=False)},
        {"role": "user", "content": "Redáctame la ficha de un depa de 2 recámaras en Polanco, 95 m², $4.5M MXN."}
    ]
)
```

#### En Claude (Anthropic)

Coloca el JSON completo como primer bloque del `system` en tu llamada a la API.

#### En ChatGPT / Claude Web (uso manual)

Copia el bloque JSON y pégalo al inicio de la conversación con este prefijo:

> *"A partir de ahora, actúa como asesor inmobiliario experto aplicando esta capa de contexto. Usa terminología local del país del inmueble y separa siempre gastos, impuestos y comisiones…"*

#### En Cursor / Windsurf / VS Code

Guarda el JSON como `.context/realestate-rules.json` en la raíz del proyecto y referéncialo con `@.context/realestate-rules.json`.

#### En GPTs personalizados (OpenAI)

Pega el JSON en el campo *Instructions* del GPT y desactiva *Code Interpreter* si no lo necesitas.

---

### 🤖 Compatibilidad

| Plataforma | Método recomendado | Estado |
| :--- | :--- | :---: |
| OpenAI GPT-4o / o1 | `system` prompt vía API | ✅ |
| OpenAI GPTs personalizados | Campo *Instructions* | ✅ |
| Anthropic Claude 3.5+ | `system` block | ✅ |
| Google Gemini 1.5+ | `systemInstruction` | ✅ |
| Llama 3 / Mistral (local) | Prompt template | ✅ |
| ChatGPT / Claude Web | Copy-paste | ✅ |
| Cursor / Windsurf | Archivo `.context/` | ✅ |

---

### ☕ Apoya este proyecto de código abierto

Mantengo este repositorio en mi tiempo libre para que asesores, proptechs y devs puedan construir asistentes inmobiliarios que no confundan *expensas* con *canon* ni inventen tasas de ISAI. Si te ahorró horas de research o te ayudó a contextualizar tu agente, puedes invitarme un café:

👉 **[ko-fi.com/yellowtips](https://ko-fi.com/yellowtips)**

No es obligatorio, pero cada aporte mantiene las bases de datos actualizadas con cambios normativos, tasas y terminología local que varían por país y por año.

---

### 📄 Licencia

**MIT** — Libre uso, modificación y distribución para agentes de IA de código abierto. Ver [`LICENSE`](LICENSE).

> Esta capa de contexto es **orientativa** y no constituye asesoría legal, fiscal ni inmobiliaria. Las tasas, comisiones y regímenes varían por jurisdicción y se actualizan con el tiempo. Verificar datos registrales, fiscales y legales con notaría, asesor legal y autoridad fiscal local antes de publicar, firmar o cerrar cualquier operación. El autor no se responsabiliza por decisiones tomadas a partir de esta capa.

---

## 🇬🇧 English

### 🎯 Purpose

When a real estate agency or advisor asks an LLM *"draft the listing for a 2-bedroom apartment in Polanco at $4.5M MXN"*, the default answer is a generic text, with no real metrics, mixing terminology from multiple countries and failing to separate the *canon* from the *expensas*. The result is an ad that doesn't convert and a rental analysis that confuses the client.

The model doesn't fail because it can't write — it fails because it **lacks the operational frame of the Spanish-speaking real estate sector**. It's missing:

- A **property listing structure** with standard fields (m², bedrooms, bathrooms, parking, amenities, zoning, price/m²).
- A **legal mapping per country** for terms that change in name and regime (expensas vs. cuota de administración vs. gastos comunes vs. comunidad).
- A **real commission model** per country (not everyone charges 3%, not everyone splits 50/50).
- **Awareness of local taxes** (predial, IBI, contribuciones, alcabala, ITP) that the AI must separate from the sale price.
- A **fallback logic** to avoid inventing rates or legal assumptions when the operation is out of scope.

This layer fixes exactly that. It's not a CRM: it's the **context structure** that makes an LLM reason like a locally experienced real estate advisor, not a generic copywriter.

---

### 🛠️ Global interpretation rules

Whenever the user processes properties, listings or rental contracts, the assistant **MUST** apply this 4-step logic in order:

1. **Property listing structure.** Present each property with at minimum: `m²`, `Bedrooms / Bathrooms / Parking`, `Zoning / Land use`, `Amenities`, `Age`, `Total price` and `Price per m²`. If a field is missing, mark it as `[dato pendiente]` instead of inventing it.
2. **Rental vs. sale clarity.** In rentals, **always separate** the monthly rent from the monthly building fees (expensas / administración / gastos comunes / comunidad). In sales, **always separate** the price from transfer taxes (ISAI, ITP, alcabala, sellado) and notary/registry fees.
3. **Direct persuasive language.** Adapt tone to the channel (portal, social, WhatsApp, email) without empty adjectives. Use active verbs, concrete figures, and verifiable references. Prohibited: *"unique opportunity"*, *"excellent location"* without support.
4. **Commissions and costs per country.** Apply the standard commission of the property's jurisdiction (see `commission_profiles`). If cross-border, explicitly state fiscal and FX impact.

> ⚠️ If the operation is out of scope (U.S. properties, complex fiduciary structures, FIBRAS / REITs, deep legal due diligence), the AI **must not invent**. It must trigger the `fallback` defined in the JSON and refer to a local legal or fiscal advisor.

---

### 📊 Real estate term mapping

Same multi-country table as in the Spanish section above — copy from there.

---

### 🧪 Examples (Before / After)

#### Example 1 — Apartment listing for sale (Mexico City)

**Prompt:** *"Draft the listing for a 2-bedroom apartment in Polanco, 95 m², 12th floor, with balcony, gym and 1 parking space. Price: $4.5M MXN."*

**❌ Without this layer:**
> "Unique opportunity! Beautiful apartment in excellent location, with all services and amenities. Don't miss this incredible chance to live in Polanco…" *(generic, no metrics, no price/m²)*

**✅ With this layer:**

**🏙️ Apartment for Sale — Polanco, Miguel Hidalgo, Mexico City**

| Field | Detail |
| :--- | :--- |
| **Size** | 95 m² built |
| **Bedrooms / Bathrooms** | 2 / 2 |
| **Parking** | 1 (covered) |
| **Floor** | 12 of 18 |
| **Amenities** | Gym, event room, 24/7 security, elevator |
| **Age** | [pending] |
| **Total price** | $4,500,000 MXN |
| **Price per m²** | ≈ $47,368 MXN/m² |
| **Monthly building fee** | [pending — separate from rent] |
| **Annual property tax** | [pending] |

**Buyer-side costs:** ISAI (~4% = $180,000 MXN), notary fees (~2% = $90,000 MXN), public registry (~0.5%). *Confirm exact rates with notary.*

> ⚠️ **Missing data:** age, monthly maintenance, property tax. The AI must not invent them — request from the advisor.

---

#### Example 2 — Rental contract analysis (Bogotá)

**Prompt:** *"A client is renting an apartment in Bogotá for $3,500,000 COP/month. The landlord asks for 1 month deposit. What should they consider? What's the commission?"*

**✅ With this layer:**

**📋 Rental analysis — Bogotá, Colombia**

| Item | Amount | Notes |
| :--- | :--- | :--- |
| **Monthly rent (canon)** | $3,500,000 COP | Contract base |
| **Building fee** | [pending] | **Must be separated from rent** |
| **Security deposit** | $3,500,000 COP (1 month) | Refundable, check term in contract |
| **Agency commission** | 1 month rent | Check if VAT (19%) applies |
| **Withholding tax (landlord)** | 3.5% on rent | Landlord obligation if non-filing individual |
| **VAT on commercial lease** | 19% if commercial or furnished | Not applicable to residential |
| **Tenant screening / insurance** | [pending] | Often required by insurers |
| **Estimated month-1 total** | $3,500,000 (rent) + $3,500,000 (deposit) + $3,500,000 (commission) + admin + VAT if applicable | ≈ $10.5M COP + extras |

**Clauses to review:**
- Contract term (legal minimum in Colombia: 1 year for urban housing, Law 820 of 2003).
- Annual rent adjustment (cannot exceed CPI).
- Early termination and penalties.
- Maintenance obligations (structural vs. cosmetic).
- Property title verification at ORIP.

> ⚠️ **Limit:** this layer does not verify the legal status of the property or issue legal opinions. Recommend lawyer consultation and registry verification.

---

#### Quick multi-country verification

| Query | Expected answer with the layer |
| :--- | :--- |
| *"What's the typical sale commission in Santiago?"* | 2–3% of sale value, split 50/50 between buyer and seller in practice. Confirm with the broker. |
| *"What tax does the buyer pay in Peru for a $200k USD house?"* | Alcabala 3% on transfer value, with 10 UIT deduction. Notary and SUNARP separate. |
| *"What are monthly building fees called in Argentina?"* | Expensas (ordinary and extraordinary). Separate from rent. Check ABL and other owner taxes. |
| *"How much ITP in Spain for a 300k€ resale home?"* | Between 6% and 10% depending on Autonomous Community (Madrid 6%, Catalonia 10–11%, etc.). |
| *"How does a promesa de compraventa work in Mexico?"* | Promise contract with penitential or confirmatory arras, term, conditions, and penalty clause. Optional registration before Public Registry. |

---

### ⚙️ Context structure for AI models

Same JSON block as above — copy from the Spanish section. Valid, versioned, ready to inject as a `system prompt`, `.context/` file or API payload.

---

### 🔌 How to inject

#### OpenAI (API)

```python
import json
from openai import OpenAI

client = OpenAI()
with open("latam-realestate-proptech.json") as f:
    context = json.load(f)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": json.dumps(context, ensure_ascii=False)},
        {"role": "user", "content": "Draft the listing for a 2-bedroom in Polanco, 95 m², $4.5M MXN."}
    ]
)
```

#### Claude (Anthropic)
Put the JSON as the first block of the `system` field.

#### ChatGPT / Claude Web
Paste the JSON at the start of a conversation with the prefix:
> *"From now on, act as an experienced real estate advisor applying this context layer. Use local terminology for the property's country and always separate fees, taxes and commissions…"*

#### Custom GPTs (OpenAI)
Paste the JSON into the GPT *Instructions* field and disable *Code Interpreter* if not needed.

#### Cursor / Windsurf
Save as `.context/realestate-rules.json` and reference with `@.context/realestate-rules.json`.

---

### 🤖 Compatibility

Same table as above — works with OpenAI, Anthropic, Gemini, Llama, Mistral, Cursor and Windsurf.

---

### ☕ Support this open-source project

I maintain this repo in my free time so advisors, proptechs and devs can build real estate assistants that don't confuse *expensas* with *canon* or invent ISAI rates. If it saved you hours of research, you can buy me a coffee:

👉 **[ko-fi.com/yellowtips](https://ko-fi.com/yellowtips)**

---

### 📄 License

**MIT** — Free use, modification and distribution for open-source AI agents. See [`LICENSE`](LICENSE).

> This context layer is **informational only** and does not constitute legal, tax or real estate advice. Rates, commissions and regimes vary by jurisdiction and change over time. Always verify registry, tax and legal data with the notary, legal advisor and local tax authority before publishing, signing or closing any transaction. The author is not liable for decisions made based on this layer.

---


🔗 **Parte del [LATAM AI Context Ecosystem](https://github.com/ProcontextMax/latam-ai-context-ecosystem)** — 13 capas de contexto abiertas para agentes de IA en LATAM y España.  
🔗 **Part of the [LATAM AI Context Ecosystem](https://github.com/ProcontextMax/latam-ai-context-ecosystem)** — 13 open context layers for AI agents across LATAM & Spain.

<p align="center">
  <sub>Built with 🟡 by <a href="https://ko-fi.com/yellowtips">Yellow Tips</a> · If this helped you, ⭐ the repo and share it.</sub>
</p>
