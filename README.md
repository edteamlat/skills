# EDteam Agent Skills - Proyecto Educativo

Bienvenido a **EDteam Agent Skills**, un proyecto educativo diseñado para que aprendas cómo funcionan los **agent skills** en Claude Code, su estructura interna, y cómo crearlos con ejemplos prácticos.

## ¿Qué es un Agent Skill?

Un **agent skill** es una extensión especial para Claude Code que amplía las capacidades del asistente. Actúa como un módulo independiente que:

- **Añade nuevas funcionalidades**: Permite que Claude realice tareas específicas que no hace de forma nativa
- **Se activa automáticamente**: Se invoca mediante comandos (`/skill-name`) o se dispara según criterios definidos
- **Es reutilizable**: Se puede compartir entre proyectos y usuarios
- **Funciona con herramientas existentes**: Ejecuta scripts, llama APIs, procesa información

### Ejemplo real del proyecto

La skill **`movie-finder`** permite buscar películas en la API de TMDB:
- Sin la skill: No puedo buscar películas
- Con la skill: Puedo ejecutar `/movie-finder` para búsquedas inteligentes de películas

## Estructura de un Agent Skill

Cada agent skill sigue una estructura estándar:

```
skill-name/
├── SKILL.md              # Metadatos y lógica de funcionamiento
├── README.md             # Instrucciones de instalación
├── scripts/              # Scripts ejecutables (bash, python, etc.)
│   └── movies.sh         # Script principal
├── assets/               # Plantillas y recursos
│   └── movie-response-template.md
├── references/           # Documentación detallada
│   └── how-to-use-movies-finder.md
└── .env.example          # Variables de entorno de ejemplo
```

## Anatomía del SKILL.md

Es el corazón de cada skill. Contiene:

```yaml
---
name: movie-finder
description: "Busca películas en TMDB cuando el usuario quiera..."
allowed-tools: Bash(bash:*)
---
```

**Secciones:**
1. **Frontmatter YAML**: Metadatos (nombre, descripción, herramientas permitidas)
2. **Documentación de uso**: Explica cómo invocar el script y sus parámetros
3. **Casos de uso**: Ejemplos de cuándo activarse y qué comandos ejecutar
4. **Flujo recomendado**: Orden de pasos para resolver problemas complejos

## Ejemplo práctico: Movie Finder

### 📁 Estructura

```
movie-finder/
├── SKILL.md                           # Define cómo funciona
├── scripts/movies.sh                  # Script que consulta TMDB
├── assets/movie-response-template.md  # Template para respuestas
└── references/how-to-use-movies-finder.md  # Referencia completa
```

### 🎬 Casos de uso

#### 1. Buscar una película por título

```bash
bash scripts/movies.sh search --query "Interstellar"
```

Se activa cuando el usuario dice: *"busca Interstellar"*, *"encuentra películas de sci-fi"*

#### 2. Obtener detalles completos

```bash
bash scripts/movies.sh details --id 157336 --append credits,videos
```

Incluye: sinopsis, reparto, videos, recomendaciones

#### 3. Descubrir películas con filtros

```bash
# Películas de acción con puntuación > 8
bash scripts/movies.sh discover \
  --with-genres 28 \
  --vote-average-gte 8 \
  --sort-by vote_average.desc

# Comedias en español de 2024
bash scripts/movies.sh discover \
  --with-original-language es \
  --primary-release-year 2024
```

#### 4. Buscar por ID externo (IMDB)

```bash
bash scripts/movies.sh find --id tt0816692 --source imdb_id
```

## Instalación

### Opción 1: Desde Claude Code (recomendado)

```bash
/plugin marketplace add edteamlat/skills
/plugin install movie-finder@edteam-agent-skills
```

### Opción 2: Usando la herramienta skills.sh

```bash
npx skills edteamlat/skills --skill movie-finder
```

## Cómo crear tu propio skill

### Paso 1: Crear la estructura

```
tu-skill/
├── SKILL.md
├── README.md
├── scripts/
│   └── main.sh
└── references/
```

### Paso 2: Definir el SKILL.md

```yaml
---
name: tu-skill
description: "Descripción clara de qué hace. Menciona cuándo se activa."
allowed-tools: Bash(bash:*)
---

# Tu Skill

## Descripción

Explica qué hace tu skill...

## Casos de uso

### Caso 1: ...
Cuando el usuario dice: "..."

\`\`\`bash
bash scripts/main.sh comando --opcion valor
\`\`\`
```

### Paso 3: Crear los scripts

Implementa los scripts que ejecutarán la lógica:
- Pueden ser bash, Python, JavaScript, etc.
- Deben ser auto-contenidos
- Manejar errores gracefully

### Paso 4: Documentar y compartir

- Añade ejemplos claros
- Documenta todas las opciones
- Prueba desde Claude Code

## Conceptos clave

| Concepto | Descripción |
|----------|------------|
| **Activación** | La skill se activa automáticamente cuando detecta palabras clave o cuando el usuario ejecuta `/nombre-skill` |
| **Herramientas permitidas** | Define qué puede hacer: Bash, Python, APIs, etc. (en `allowed-tools`) |
| **Descripción** | Explica cuándo activarse y qué palabras clave disparan la skill |
| **Scripts** | El código que realmente ejecuta las acciones |
| **Templates** | Plantillas para formatear respuestas de forma consistente |

## Flujo de ejecución

```
1. Usuario dice algo → 
2. Claude detecta palabras clave →
3. Lee SKILL.md para entender qué hacer →
4. Ejecuta el script bash/python indicado →
5. Procesa resultados →
6. Usa template para formatear respuesta →
7. Responde al usuario
```

## Archivos importantes

### SKILL.md
- **Qué es**: Manual de instrucciones para Claude
- **Quién lo lee**: Claude Code (no los usuarios)
- **Contenido**: Metadatos, casos de uso, ejemplos de comandos

### README.md
- **Qué es**: Guía para instalación y uso inicial
- **Quién lo lee**: Los usuarios
- **Contenido**: Cómo instalar, comandos básicos

### scripts/
- **Qué son**: Programas ejecutables que hacen el trabajo
- **Lenguaje**: Bash, Python, Node.js, etc.
- **Responsabilidad**: Lógica de negocio pura

### assets/
- **Qué contiene**: Templates, ejemplos, recursos
- **Ejemplo**: `movie-response-template.md` para formatear respuestas

### references/
- **Qué contiene**: Documentación completa y exhaustiva
- **Ejemplo**: `how-to-use-movies-finder.md` con todos los parámetros

## Aprende desarrollando

Este proyecto es ideal para aprender porque:

✅ Incluye un ejemplo completo funcional (`movie-finder`)  
✅ Documentación paso a paso en SKILL.md  
✅ Scripts reales que usan APIs  
✅ Estructura estándar que puedes replicas  
✅ Casos de uso claros y variados  

## Próximos pasos

1. **Instala la skill**: Cópiala a tu proyecto de Claude Code
2. **Prueba los comandos**: Ejecuta ejemplos de `movie-finder/SKILL.md`
3. **Estudia la estructura**: Lee cómo está organizada
4. **Crea la tuya**: Usa `movie-finder` como plantilla para tu primer skill
5. **Comparte**: Añade tu skill al marketplace de EDteam

## Recursos

- 📖 [Documentación de movie-finder](./skills/movie-finder/SKILL.md)
- 🎬 [Referencia completa de comandos](./skills/movie-finder/references/how-to-use-movies-finder.md)
- 🔧 [Marketplace de plugins](https://claude.ai/plugins)

## Ayuda y soporte

¿Preguntas sobre agent skills? 
- Revisa los ejemplos en `movie-finder/SKILL.md`
- Consulta `references/` para documentación detallada
- Prueba los comandos en tu Claude Code

---

**Creado por** EDteam | **Licencia** MIT | **Versión** 1.0.0
