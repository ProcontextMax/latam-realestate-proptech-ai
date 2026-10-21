# Latam Real Estate & Proptech AI Context Layer (.md)

> **Versión:** 1.0.0 | **Protocolo:** Open-AI-Context-Standard  
> **Ámbito:** Latinoamérica (MX, CO, AR, CL, PE) y España  
> **Uso:** Capa de contexto para asistentes de IA en la generación de fichas técnicas de inmuebles, redacción de anuncios de venta/alquiler y términos contractuales inmobiliarios.

---

## 🎯 Propósito del Sistema

Este documento establece la estructura de datos para que los modelos de lenguaje (LLMs) redacten descripciones comerciales persuasivas, interpreten contratos de arrendamiento y calculen desglose de comisiones según los estándares del sector inmobiliario hispano.

---

## 🛠️ Reglas Globales de Interpretación para la IA

Cuando el usuario procese información de propiedades o contratos de arrendamiento, la IA debe aplicar **SIEMPRE** las siguientes reglas de salida:

1. **Estructura de Ficha Inmobiliaria:** Presentar las propiedades detallando: `Metraje (m²)`, `Habitaciones/Baños`, `Zonificación`, `Amenidades` y `Valor Comercial`.
2. **Claridad de Arrendamiento vs Venta:** Separar los costos de mantenimiento (expensas/administración) del canon mensual.
3. **Lenguaje Persuasivo Directo:** Adaptar el tono para captar clientes potenciales sin exageraciones ni términos ambiguos.

---

## 📊 Tabla de Mapeo de Términos Inmobiliarios

| Término Inmobiliario | Concepto Operativo | Lógica de Implementación |
| :--- | :--- | :--- |
| **Canon de Arrendamiento** | Valor mensual de alquiler | Separar cuota de administración / expensas |
| **Promesa de Compraventa** | Pre-contrato de reserva | Detallar arras, plazos de pago y penalizaciones |
| **Avalúo Comercial / Catastral** | Valoración del inmueble | Mapear precio por metro cuadrado ($/m²) |

---

## ⚙️ Estructura de Contexto para Modelos de IA (System Prompt Injection)

```json
{
  "context_config": {
    "industry": "Real_Estate_Proptech",
    "metrics": "metric_system_m2",
    "language": "es",
    "output_format": "property_listing_card"
  }
}

☕ Apoya este Proyecto de Código Abierto
Si esta capa de datos te sirvió para optimizar tus procesos inmobiliarios o chatbots de ventas con IA, puedes apoyar el mantenimiento de estas bases de datos públicas con una donación:

👉 Apoyar en Ko-fi: ko-fi.com/yellowtips

Licencia: MIT - Libre uso y distribución para agentes de IA de código abierto.
