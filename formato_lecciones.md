# Formato de Lecciones Interactivas

Especificación para crear lecciones compatibles con la aplicación de aprendizaje.

---

## Estructura Básica del JSON
```json
{
  "title": "Título de la lección (máx. 60 caracteres)",
  "steps": [
    // Array de pasos (entre 1 y 10 elementos)
  ]
}
```

---

## Tipos de Pasos

### 1. Paso Teórico (`text`)
```json
{
  "type": "text",
  "content": "Texto con explicaciones y ecuaciones LaTeX"
}
```

### 2. Problema Interactivo (`problem`)
```json
{
  "type": "problem",
  "question": "Pregunta con ecuaciones LaTeX",
  "options": [
    "Opción 1 con LaTeX",
    "Opción 2 con LaTeX",
    "Opción 3 con LaTeX"
  ],
  "answer": "Opción correcta (debe coincidir exactamente)",
  "explanation": "Explicación detallada con LaTeX"
}
```

---

## Sintaxis LaTeX
| Tipo          | Sintaxis          | Ejemplo JSON              | Renderizado         |
|---------------|-------------------|---------------------------|---------------------|
| Ecuación en línea | `\\(...\\)`       | `"Resuelve \\(x^2 = 4\\)"` | \(x^2 = 4\)        |
| Ecuación en bloque  | `\\[...\\]`       | `"Solución: \\[x = 2\\]"`  | \[x = 2\]          |

---

## Reglas Esenciales
1. **Estructuración:**
   - Máximo 3 pasos teóricos consecutivos
   - Cada problema debe tener exactamente 3 opciones

2. **Validación de Respuestas:**
   - El campo `answer` debe coincidir **exactamente** con una de las opciones
   - Se respetan mayúsculas/minúsculas y espacios

3. **Escapes en JSON:**
   ```json
   "options": [
     "\\[\\frac{1}{2}\\]",  // Correcto
     "\[\frac{1}{2}\]"      // Incorrecto
   ]
   ```

---

## Ejemplo Completo
```json
{
  "title": "Introducción al Cálculo",
  "steps": [
    {
      "type": "text",
      "content": "La derivada de una función \\[f(x)\\] se define como: \\[f'(x) = \\lim_{h \\to 0} \\frac{f(x+h) - f(x)}{h}\\]"
    },
    {
      "type": "problem",
      "question": "Deriva \\[f(x) = 3x^2 + 2x\\]",
      "options": [
        "\\[6x + 2\\]",
        "\\[3x + 2\\]",
        "\\[6x^2\\]"
      ],
      "answer": "\\[6x + 2\\]",
      "explanation": "Aplicando reglas básicas:\n1. Derivada de \\[3x^2\\] es \\[6x\\]\n2. Derivada de \\[2x\\] es \\[2\\]"
    }
  ]
}
```

---

## Recomendaciones para LLMs
```markdown
1. **Generación de Opciones:**
   - Usar errores comunes como distractores
   - Variar posición de la respuesta correcta aleatoriamente

2. **Estructuración de Contenido:**
   ```json
   {
     "type": "problem",
     "question": "¿Cuál es el valor de \\(\\pi\\) aproximado?",
     "options": [
       "\\[3.1416\\]", 
       "\\[2.7183\\]", 
       "\\[1.6180\\]"
     ],
     "answer": "\\[3.1416\\]",
     "explanation": "\\[\\pi\\] es la razón entre circunferencia y diámetro (~3.1416)"
   }
   ```

3. **Prompt Sugerido:**
   ```
   Genera una lección sobre [TEMA] con:
   - 1 introducción teórica con 2 ecuaciones
   - 2 problemas aplicados
   - Incluir errores comunes en las opciones
   - Nivel: [Básico/Intermedio/Avanzado]
   ```
```

---

## Errores Comunes
1. **En Opciones:**
   ```json
   // MAL: Respuesta no coincide exactamente
   "answer": "\\[x=2\\]"
   "options": ["\\[x = 2\\]", "..."]
   
   // BIEN: Coincidencia exacta
   "answer": "\\[x = 2\\]"
   ```

2. **Escapes Incorrectos:**
   ```json
   // MAL: Falta escape para la barra invertida
   "content": "Usa \[x\]"
   
   // BIEN: Escapes correctos
   "content": "Usa \\[x\\]"
   ```

---

**Versión**: 2.1  
**Última actualización**: 2023-11-15  
**Compatibilidad**: Aplicación v1.2+
