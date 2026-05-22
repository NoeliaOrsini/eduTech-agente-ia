# 🎓 Agente IA Edutech - EduTech Buddy

Este proyecto nace como parte de la **#InmersionONE Agentes de IA**. El objetivo es proporcionar un asistente virtual inteligente para la comunidad educativa (**EduTech Innovation School**), capaz de gestionar consultas sobre inasistencias, notas y reglamentos escolares de forma segura y privada.

---

## 🛠️ Tecnologías Utilizadas

* **n8n:** Orquestador de flujos de trabajo.
* **Cohere:** Modelo de lenguaje (LLM).
* **Telegram API:** Interfaz de usuario.
* **MySQL (Railway):** Almacenamiento de datos académicos vinculados por `id_telegram`.
* **Gist (GitHub):** Gestión del reglamento escolar.

---

## 🏗️ Arquitectura y Flujos de Trabajo

### 1. Carga de Reglamento (Gist + Vector Store)
El primer paso es ingerir la información del reglamento. Este flujo toma el contenido alojado en un Gist y lo procesa mediante *embeddings* para que el agente tenga contexto.

![Carga de Reglamento](assets/flujo_1.png)

*Configuración del Vector Store:*
![Configuración del Vector Store](assets/simple_vector.png)

### 2. Chatbot Principal (El corazón del Agente)
Este flujo orquesta la interacción. Aquí es donde integramos el **Prompt de Sistema** que garantiza la seguridad de los datos.

![Flujo Principal](assets/flujo_2.png)

*Configuración del System Prompt:*
![Prompt de Sistema](assets/prompt_agente.png)

💡 El agente cuenta con un motor de decisión inteligente que actúa según el tipo de consulta:

* **Consultas Académicas/Generales:** Cuando el usuario pregunta sobre el reglamento escolar, el agente consulta automáticamente el **Vector Store** (Flujo 1), donde reside la información estructurada de las normas institucionales.
* **Consultas de Información Personal:** Cuando el usuario solicita notas o información de un alumno específico, el agente ejecuta una validación de seguridad contra la base de datos **MySQL**. El sistema verifica el `id_telegram` del remitente antes de exponer cualquier dato sensible, garantizando la privacidad y cumpliendo con el protocolo de autenticación.

---

## 🛡️ Pruebas de Funcionamiento y Seguridad

Para garantizar que el bot fuera "impenetrable", realicé pruebas de validación con mi base de datos personalizada:

**◈ Validación de Identidad:**
El sistema detecta quién soy a través de mi `id_telegram` en la base de datos MySQL.
![Prueba de ID](assets/prueba_id.png)
![Tabla de Alumnos](assets/tabla_alumnos.png)

**◈ Prueba de Privacidad:**
Intenté solicitar información de otros alumnos y el sistema, al detectar que no soy el tutor autorizado para esos registros, bloqueó la respuesta automáticamente.
![Prueba de Seguridad](assets/prueba_seguridad.png)

**◈ Prueba de Distracción:**
El bot responde perfectamente sobre el reglamento académico cuando se le consulta.
![Respuesta del Reglamento](assets/politicas.png)

---

## 🚀 Cómo configurar tu entorno

1. **Base de Datos:** Crea una tabla en MySQL con una columna obligatoria `id_telegram`.
2. **Reglamento:** Crea un archivo de texto, súbelo a un [GitHub Gist](https://gist.github.com/) y usa la URL *Raw*.
3. **Importación:** Importa los archivos JSON de los flujos en tu instancia de n8n.
4. **Credenciales:** Configura tus claves de Telegram, Cohere y la conexión a MySQL.

> **Nota:** Los archivos .json no incluyen credenciales por seguridad. Para que el bot funcione, deberás configurar tus propias API Keys de Cohere, el Token de Telegram y la conexión a tu base de datos MySQL en los nodos correspondientes. No olvides primero correr el flujo 1 antes de realizar las pruebas; cuando esté listo, presioná "Publish" en el flujo 2 y tu agente estará listo para funcionar en el mundo real.

---

## 🚧 Estado del Proyecto
Actualmente el bot es completamente funcional y seguro. El flujo se encuentra en producción, operando de manera autónoma. 

Próximamente, estaré implementando una capa de optimización mediante un nodo **SWITCH** para filtrar consultas genéricas fuera del contexto escolar, optimizando así el consumo de tokens y mejorando la eficiencia del ruteo.

---

### 💼 Acerca de mí
Este proyecto fue desarrollado con pasión y dedicación como parte de la **#InmersionONE Agentes de IA**.

**Noelia Orsini**
*Programadora | Abogada | Counselor*

[🔗 Conectá conmigo en LinkedIn](https://www.linkedin.com/in/noelia-orsini)
