# Examen Web – Análisis y Visualización de Datos (GAD-2401)

Examen de opción múltiple sobre **Tema 1: Datos y su preprocesamiento** y **2.1.1 Coeficiente de Pearson**.

Asignatura: *Análisis y visualización de datos* · Clave: **GAD-2401** · Carrera: Ingeniería Informática (TecNM).

## Características

| Aspecto | Detalle |
|--------|---------|
| Preguntas | **40** de opción múltiple |
| Duración | **90 minutos** (temporizador visible) |
| Pantalla completa | Obligatoria al iniciar (si el navegador lo permite) |
| Anti-trampa | Minimizar / cambiar de pestaña, salir de fullscreen, copiar/cortar/pegar, clic derecho, atajos Ctrl+C/V/X/A/S/P |
| Límite | Más de **5 infracciones** → cierre automático y resultados |
| Navegación | Anterior / Siguiente + panel de números de pregunta |
| Estado | Verde = contestada · Borde azul = actual · Sin color = sin contestar |
| Resultados | Guardado en **Supabase** (configurable) |

## Temas cubiertos

- **1.1** Conjuntos de datos: alto volumen, alta variedad, alta velocidad  
- **1.2** Relaciones: representaciones, similitud/disimilitud, secuencias  
- **1.3** Preprocesamiento: muestreo, cuantificación, errores, filtrado, transformación, integración  
- **2.1.1** Coeficiente de correlación de Pearson  

## Cómo usar (local)

1. Clona o descarga este repositorio.
2. Abre `index.html` en Chrome, Edge o Firefox  
   **o** sirve la carpeta con un servidor local:

```bash
# Python
python -m http.server 8765

# Node (si tienes npx)
npx serve .
```

3. Entra a `http://localhost:8765`.
4. Escribe nombre y número de control (≥ 2 caracteres).
5. Pulsa **Iniciar examen en pantalla completa**.

> **Nota:** La pantalla completa funciona mejor servida por HTTP/HTTPS. Con `file://` algunos navegadores la bloquean; el examen permite continuar de todos modos.

## Configurar Supabase (opcional)

1. Crea un proyecto en [supabase.com](https://supabase.com).
2. En el **SQL Editor** ejecuta:

```sql
create table exam_results (
  id uuid primary key default gen_random_uuid(),
  student_name text not null,
  student_id text not null,
  score numeric not null,
  correct int not null,
  total int not null,
  answers jsonb,
  violations int default 0,
  duration_seconds int,
  finished_reason text,
  created_at timestamptz default now()
);

alter table exam_results enable row level security;

create policy "Allow anonymous inserts"
  on exam_results for insert
  to anon
  with check (true);
```

3. En **Project Settings → API** copia:
   - Project URL  
   - `anon` public key  

4. En `index.html` busca y reemplaza:

```js
const SUPABASE_URL = 'https://TU-PROYECTO.supabase.co';
const SUPABASE_ANON_KEY = 'TU-ANON-KEY';
```

Si dejas los valores de ejemplo, el examen funciona en **modo demo** (muestra el resultado y lo imprime en la consola, sin guardar en la base).

## Estructura del repositorio

```
examen-web/
├── index.html      # Aplicación completa (HTML + CSS + JS)
├── README.md       # Este archivo
└── .gitignore
```

## Subir a GitHub

```bash
cd examen-web
git init
git add .
git commit -m "Examen web GAD-2401: 40 preguntas, anti-trampa y Supabase"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/examen-gad-2401.git
git push -u origin main
```

Para publicarlo con **GitHub Pages**:

1. Repositorio → **Settings** → **Pages**.
2. Source: branch `main`, carpeta `/ (root)`.
3. En unos minutos estará en:  
   `https://TU-USUARIO.github.io/examen-gad-2401/`

## Seguridad

Este control es **del lado del cliente**. Un estudiante avanzado puede desactivar las detecciones con las herramientas de desarrollador.

Para un entorno más robusto se recomienda:

- Supervisión presencial o proctoring.
- Backend que valide respuestas y tokens de un solo uso.
- Restricción por red institucional.

## Licencia

Material educativo orientado a la asignatura GAD-2401. Úsalo y adáptalo libremente en contextos académicos.
