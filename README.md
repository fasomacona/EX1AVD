# Examen Web – Análisis y Visualización de Datos (GAD-2401)

Examen de opción múltiple sobre **Tema 1: Datos y su preprocesamiento** + **2.1.1 Coeficiente de Pearson**.

## Características

- **20 preguntas** de opción múltiple
- **Duración**: 90 minutos (temporizador visible)
- **Pantalla completa** obligatoria al iniciar
- Detección de infracciones:
  - Minimizar / cambiar de pestaña (`visibilitychange`)
  - Salir de pantalla completa
  - Copiar, cortar o pegar texto
  - Clic derecho
  - Atajos de teclado (Ctrl+C, Ctrl+V, etc.)
- **Máximo 5 infracciones** → el examen se cierra automáticamente y muestra el resultado
- Navegación libre (Anterior / Siguiente + panel de números de pregunta)
- Indicador visual del estado de cada pregunta:
  - Verde = contestada
  - Borde azul = pregunta actual
  - Sin color = sin contestar
- Resultados guardados en **Supabase**

## Cómo usar

1. Abre `index.html` en un navegador moderno (Chrome, Edge, Firefox).
2. Ingresa nombre completo y número de control.
3. Haz clic en **Iniciar examen en pantalla completa**.
4. Responde las preguntas. Puedes avanzar y retroceder libremente.
5. Al terminar (manual, por tiempo o por infracciones) se muestra el resultado y se intenta guardar en Supabase.

## Configurar Supabase

1. Crea un proyecto en [supabase.com](https://supabase.com).
2. En el SQL Editor ejecuta:

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

-- Política para permitir inserciones anónimas (ajusta según tus necesidades de seguridad)
alter table exam_results enable row level security;

create policy "Allow anonymous inserts"
  on exam_results for insert
  to anon
  with check (true);
```

3. Ve a **Project Settings → API** y copia:
   - Project URL
   - `anon` public key

4. En `index.html` reemplaza:

```js
const SUPABASE_URL = 'https://TU-PROYECTO.supabase.co';
const SUPABASE_ANON_KEY = 'TU-ANON-KEY';
```

Si dejas los valores de ejemplo, el examen funciona en **modo demo** (no guarda en la base de datos, solo muestra el resultado en consola).

## Estructura del archivo de resultados (JSON)

```json
{
  "student_name": "María López",
  "student_id": "2024123456",
  "score": 85,
  "correct": 17,
  "total": 20,
  "answers": [
    { "question_id": 1, "selected": 0, "correct": 0, "is_correct": true },
    ...
  ],
  "violations": 1,
  "duration_seconds": 3240,
  "finished_reason": "manual"
}
```

## Notas de seguridad

- Este es un control **del lado del cliente**. Un estudiante avanzado puede desactivar las detecciones con herramientas de desarrollador.
- Para un entorno más robusto se recomienda:
  - Servir el examen desde un backend que valide respuestas.
  - Usar proctoring profesional o supervisión presencial.
  - Restringir el acceso por IP o token de un solo uso.

## Temas cubiertos

| Tema | Contenido |
|------|-----------|
| 1.1 | Conjuntos de datos (volumen, variedad, velocidad) |
| 1.2 | Relaciones, similitud/disimilitud, secuencias |
| 1.3 | Preprocesamiento (muestreo, cuantificación, errores, filtrado, transformación, integración) |
| 2.1.1 | Coeficiente de correlación de Pearson |
