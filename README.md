# FilaCero 🩺🤖

> **Agentes en todas partes** — *Hackathon Global AI Tinkerers 2026 (Asunción)*  
> *"Tú no deberías tener que quedarte buscando un turno. El agente lo busca por ti."*

---

## 📌 Descripción del Proyecto

**FilaCero** es un agente inteligente integrado en **Telegram** diseñado para resolver una de las problemáticas de acceso a la salud pública y privada más frustrantes: la búsqueda incesante de turnos médicos cancelados o liberados dinámicamente.

En lugar de obligar al usuario a refrescar una plataforma web o realizar constantes llamadas telefónicas, el usuario simplemente le indica su necesidad en lenguaje natural a través de Telegram (ej. *"Necesito turno para pediatría en Ingavi"*). 

El agente:
1. **Interpreta el lenguaje natural** mediante la API de **OpenAI**, extrayendo la especialidad y el centro médico deseado.
2. **Consulta el sistema de turnos**. Si hay disponibilidad inmediata, emite la confirmación.
3. **Activa vigilancia en segundo plano (asíncrona)**: Si no hay disponibilidad inmediata, el agente asume la tarea. Cuando un turno se libera por cancelación o actualización de agenda, el agente notifica **proactivamente** al usuario.

---

## 🏗️ Arquitectura del Sistema

```text
[ Usuario ]
    │
    ▼ (Mensaje en Lenguaje Natural)
[ Telegram Bot API ]
    │
    ▼
[ Bot Server (Python) ] ──(Petición NLU)──► [ OpenAI API ]
    │                                          │ (Especialidad / Clínica)
    ▼                                          ▼
[ IPS Mock / Simulador ] ◄─────────────────────┘
    │
    ├── Turno disponible ──────► Confirmación Inmediata en Telegram
    │
    └── Sin turno ─────────────► Tarea Asíncrona en Segundo Plano (Asyncio / Trigger.dev)
                                       │
                                       ▼ (Detección de cancelación)
                                 [ Notificación Proactiva Telegram ]
```

---

## 🛠️ Tech Stack & Herramientas

- **Lenguaje Principal:** Python 3.10+
- **Bot Platform:** `python-telegram-bot` (v20+)
- **NLU / IA:** OpenAI API (`gpt-4o-mini` / `gpt-4o`)
- **Procesamiento Asíncrono:** `asyncio` / `Trigger.dev`
- **Simulador de Disponibilidad:** IPS Mock determinista en Python
- **Gestión de Entorno:** `python-dotenv`

---

## 📁 Estructura del Repositorio

```text
filacero-bot/
├── main.py              # Punto de entrada y orquestación del bot
├── bot.py               # Handlers de Telegram, comandos y UX conversacional
├── openai_service.py    # Extracción estructurada de entidades vía OpenAI
├── ips_mock.py          # Simulador determinista de disponibilidad de turnos
├── requirements.txt     # Dependencias del proyecto
├── .env.example         # Plantilla de variables de entorno
├── .gitignore           # Archivos ignorados por Git
└── README.md            # Documentación principal
```

---

## ⚡ Instalación y Configuración Local

### 1. Requisitos Previos
- Python 3.10 o superior instalado.
- Un Bot Token obtenido a través de [@BotFather](https://t.me/BotFather) en Telegram.
- Una API Key activa de [OpenAI](https://platform.openai.com/).

### 2. Clonar el Repositorio
```bash
git clone https://github.com/TU_USUARIO/filacero-bot.git
cd filacero-bot
```

### 3. Crear y Activar Entorno Virtual
```bash
# Linux/macOS
python3 -m venv venv
source venv/bin/activate

# Windows
python -m venv venv
venv\Scripts\activate
```

### 4. Instalar Dependencias
```bash
pip install -r requirements.txt
```

### 5. Configurar Variables de Entorno
Copia la plantilla `.env.example` a `.env` y completa tus credenciales:
```bash
cp .env.example .env
```

Editar `.env`:
```env
TELEGRAM_TOKEN=tu_token_de_telegram_aqui
OPENAI_API_KEY=tu_openai_api_key_aqui
```

### 6. Ejecutar el Bot
```bash
python main.py
```

---

## 🎮 Comandos y Modo Demo

- `/start` - Inicia la interacción con el agente y muestra el mensaje de bienvenida.
- `/demo` - Ejecuta un flujo determinista y acelerado de prueba (utiliza OpenAI para parsing y acelera la liberación del turno en el simulador para demostración en vivo).
- `/cancelar` - Cancela cualquier tarea de vigilancia asíncrona activa en el chat.

---

## 👥 Equipo de Desarrollo (6 Miembros)

- **Alex** - Backend & Integración OpenAI
- **Elías** - Infraestructura, Mock & Tareas Asíncronas
- **Miguel** - Documentación, Demo & Presentación
- **Nico** - QA & Testing de Flujos
- **Matías** - Media & Entrega Final

---

## ⚠️ Nota de Exención y Alcance Tecnológico

*Este prototipo fue construido durante el **Hackathon Global AI Tinkerers 2026 (Capítulo Asunción)**. Utiliza un mock local determinista (`ips_mock.py`) para simular la agenda y liberación de citas del sistema médico. No realiza reservas reales en sistemas informáticos externos de salud.*
