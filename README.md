# 💰 AI Telegram Expense Tracker

Este workflow de **n8n** transforma tu chat de Telegram en un asistente financiero personal inteligente. Utiliza **IA Local (Ollama con Phi3)** para interpretar lenguaje natural, extraer datos de gastos y guardarlos automáticamente en una base de datos **PostgreSQL**.

Además, el sistema genera y envía resúmenes de gastos semanales y mensuales automáticamente.

## 🚀 Funcionalidades Principales

* **🗣️ Procesamiento de Lenguaje Natural:** Simplemente escribe "Cena con amigos 45 euros" o "Gasolina 30". El sistema extrae el *concepto*, la *cantidad* y asigna la *categoría* automáticamente.
* **🤖 Inteligencia Artificial Local:** Conectado a **Ollama** (modelo Phi3) para privacidad total y cero coste por API.
* **🛡️ Seguridad y Permisos:** Verifica contra la base de datos si el ID de usuario de Telegram tiene permisos para registrar gastos.
* **🔄 Flujo Interactivo:** Si la IA no detecta la cantidad o el concepto, el bot pregunta al usuario para completar la información faltante.
* **📊 Reportes Automáticos:**
    * **Semanal:** Resumen enviado cada lunes a las 07:30 AM.
    * **Mensual:** Resumen enviado el día 1 de cada mes a las 09:00 PM.
    * Incluye totales y desglose porcentual por categorías.

## 🛠️ Requisitos Técnicos

Para usar este workflow necesitas:

## 🛠️ Requisitos Técnicos

Para desplegar este workflow necesitas:

1.  **n8n**: Versión Self-hosted (recomendado para conectar con Ollama en la misma red/máquina).
2.  **PostgreSQL**: Base de datos con las siguientes tablas:
    * `permissions`: Para validar el `user_id` de Telegram.
    * `expenses`: Para guardar los gastos (`concepto`, `cantidad`, `category_id`, `created_at`).
    * `categories`: Para mapear los nombres de las categorías.
3.  **Ollama**: Ejecutándose localmente o en red, con el modelo `phi3` descargado (comando: `ollama pull phi3`).
4.  **Bot de Telegram**: Token del bot generado con BotFather.
5.  **Ngrok (o Cloudflare Tunnel)**: Necesario si tu n8n está en local (localhost). Telegram necesita una URL pública (HTTPS) para enviar los mensajes al Webhook de n8n.

## ⚙️ Cómo Funciona

1.  **Recepción:** El trigger de Telegram recibe un mensaje de texto.
2.  **Validación:** Consulta PostgreSQL para verificar si el usuario está autorizado.
3.  **Extracción (IA):** Envía el texto a Ollama con un prompt estricto para recibir un JSON limpio (`concepto`, `cantidad`, `categoria`).
4.  **Lógica de Fallos:**
    * Si faltan datos, solicita al usuario que los ingrese manualmente.
    * Si todo es correcto, inserta el registro en PostgreSQL.
5.  **Feedback:** Confirma al usuario el registro con un mensaje formateado.
6.  **Cronjobs:** Los triggers de horario ejecutan consultas SQL periódicas para agrupar gastos y enviarte el resumen financiero.
