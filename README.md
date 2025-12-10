# AI-Telegram-Expense-Tracker-Registro-y-Reportes-Autom-ticos-con-Ollama
Un sistema automatizado en n8n que convierte mensajes de texto natural en Telegram en registros de gastos estructurados utilizando IA local (Ollama/Phi3) y PostgreSQL. Incluye validación de usuarios, categorización inteligente y generación automática de reportes semanales y mensuales.

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

1.  **n8n** (Self-hosted recomendado para conectar con Ollama localmente).
2.  **PostgreSQL**:
    * Tabla `permissions`: Para validar `user_id` de Telegram.
    * Tabla `expenses`: Para guardar los gastos (`concepto`, `cantidad`, `category_id`, `created_at`).
    * Tabla `categories`: Para mapear nombres de categorías.
3.  **Ollama**: Corriendo localmente o en red, con el modelo `phi3` descargado (`ollama pull phi3`).
4.  **Bot de Telegram**: Token del bot obtenido via BotFather.

## ⚙️ Cómo Funciona

1.  **Recepción:** El trigger de Telegram recibe un mensaje de texto.
2.  **Validación:** Consulta PostgreSQL para verificar si el usuario está autorizado.
3.  **Extracción (IA):** Envía el texto a Ollama con un prompt estricto para recibir un JSON limpio (`concepto`, `cantidad`, `categoria`).
4.  **Lógica de Fallos:**
    * Si faltan datos, solicita al usuario que los ingrese manualmente.
    * Si todo es correcto, inserta el registro en PostgreSQL.
5.  **Feedback:** Confirma al usuario el registro con un mensaje formateado.
6.  **Cronjobs:** Los triggers de horario ejecutan consultas SQL periódicas para agrupar gastos y enviarte el resumen financiero.
