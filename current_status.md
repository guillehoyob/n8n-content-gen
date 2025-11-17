Perfecto. He completado la investigación exhaustiva de todas las herramientas, modelos y precios necesarios. Ahora voy a entregarte un resumen ejecutivo ultra-claro y práctico de todo el sistema que vamos a construir.

---

# 🎬 SISTEMA DE CREACIÓN DE CONTENIDO MULTIMEDIA (TEXTO + AUDIO + VÍDEO) - RESUMEN EJECUTIVO

## 📊 MODELOS Y HERRAMIENTAS SELECCIONADAS

### **1. TEXTO (LLM)**
- **MVP/Pruebas**: GPT-4o-mini ($0.15 input / $0.60 output por 1M tokens)
- **Producción**: GPT-4o ($3 input / $10 output por 1M tokens)
- **Integración**: Nativa en n8n
- **Free tier**: $5 de créditos gratis al registrarse en OpenAI

### **2. AUDIO (TTS)**
- **Herramienta**: ElevenLabs API
- **Free tier**: 10,000 créditos/mes (~10 minutos de audio)
- **Starter**: $5/mes - 30,000 créditos (~30 min)
- **Creator**: $22/mes - 100,000 créditos (~100 min)
- **Coste**: 1 crédito = 1 carácter de texto
- **Integración**: HTTP Request en n8n

### **3. VÍDEO**
**Opción A - Pika Labs** (RECOMENDADA PARA MVP):
- Free: 80 créditos/mes
- Standard: $8/mes - 700 créditos
- Pro: $35/mes - 2,300 créditos
- Coste: 10-80 créditos por clip según complejidad
- Formatos: Vertical y horizontal
- Calidad: 1080p

**Opción B - Runway Gen-4**:
- Free: 125 créditos una vez
- Standard: $15/mes
- Calidad superior pero más caro

**Opción C - Minimax**:
- Mejor adherencia a prompts
- Excelente movimiento
- Pricing competitivo

### **4. TRANSCRIPCIÓN (STT / YouTube)**
**Opción A - YouTube directo**:
- API nativa de YouTube (si existe subtítulos)
- Gratis si el vídeo tiene transcripción

**Opción B - OpenAI Whisper API**:
- $0.006 por minuto de audio
- Precisión 99%+
- 98+ idiomas

**Opción C - WhisperAPI.com**:
- 5 transcripciones gratis al día
- Buen free tier para pruebas

### **5. EXTRACCIÓN WEB/NOTICIAS**
- Scraping con HTTP Request + Function node en n8n
- Limpieza con GPT-4o-mini (barato)
- Alternativa: APIs de scraping (Apify, etc.) si es necesario

---

## 🏗️ ARQUITECTURA DEL FLUJO COMPLETO

### **FASE 1 - MVP (PRIORITARIO)**

```
[1. ENTRADA MANUAL / WEBHOOK]
    ↓
[2. NORMALIZACIÓN DE DATOS]
    ↓
[3. ENRIQUECIMIENTO DE CONTEXTO]
    ├─→ [Si hay URL de noticia] → Scraping + Limpieza
    ├─→ [Si hay URL YouTube] → Descarga audio → Whisper STT
    └─→ [Si hay texto directo] → Ya está listo
    ↓
[4. ANÁLISIS DE REQUISITOS]
    ├─ Detectar plataformas (TikTok=vertical, YouTube=horizontal)
    ├─ Validar duración objetivo
    ├─ Identificar estilo
    └─ Nivel de novelización
    ↓
[5. DISEÑO DE ESTRUCTURA]
    ├─ Definir número de secciones
    ├─ Calcular duración por sección
    └─ Crear esquema narrativo (hook, desarrollo, cierre, CTA)
    ↓
[6. MÓDULO REFINAMIENTO PROMPT GUION] ⭐
    ├─ LLM crea el mejor prompt posible
    ├─ Define rol del modelo (ej: "mejor guionista para YouTube")
    └─ Incluye: tono, estructura, restricciones, ejemplos
    ↓
[7. GENERACIÓN GUION BORRADOR]
    └─ GPT-4o genera secciones usando prompt refinado
    ↓
[8. REVISIÓN EXPERTA AUTOMÁTICA] ⭐
    └─ Otro LLM revisa coherencia, fidelidad, calidad
    ↓
[9. GENERACIÓN GUION FINAL OPTIMIZADO]
    └─ GPT-4o genera versión final mejorada
    ↓
[10. ADAPTACIÓN POR PLATAFORMA]
    ├─ TikTok: Hooks agresivos, 30-90s, vertical
    └─ YouTube: Hooks narrativos, 10-20 min, horizontal
    ↓
[11. MÓDULO REFINAMIENTO AUDIO] ⭐
    ├─ Selección de voz (masculina/femenina/neutra)
    ├─ Idioma y acento
    ├─ Ritmo (rápido para shorts, pausado para profundo)
    └─ Formato técnico (bitrate, MP3/WAV)
    ↓
[12. GENERACIÓN DE AUDIO POR SECCIÓN]
    └─ ElevenLabs TTS con parámetros refinados
    ↓
[13. MÓDULO REFINAMIENTO VÍDEO] ⭐
    ├─ Formato: 9:16 (TikTok) vs 16:9 (YouTube)
    ├─ Resolución: 1080p o 4K
    ├─ Duración por escena
    ├─ Estilo visual (cinemático, documental, épico, etc.)
    └─ Parámetros de "cámara" según API
    ↓
[14. GENERACIÓN DE VÍDEO POR SECCIÓN]
    └─ Pika/Runway con parámetros refinados
    ↓
[15. EMPAQUETADO Y ALMACENAMIENTO]
    └─ JSON con:
        ├─ Metadata (tema, plataformas, duración)
        ├─ Guion completo por secciones
        ├─ Audios (URLs/archivos)
        ├─ Vídeos (URLs/archivos)
        ├─ Fuentes de contexto utilizadas
        ├─ Prompts refinados usados
        └─ Parámetros de cada generación
```

### **FASE 2 - EXTENSIONES (OPCIONAL)**
- Entrada por Email (IMAP/Gmail)
- Entrada por WhatsApp
- Entrada por Telegram
- Módulo de montaje automático (edición final)
- Más fuentes de contexto

---

## 💡 NODOS PRINCIPALES DEL FLUJO N8N

### **Nodos Core MVP**
1. **Manual Trigger / Webhook** - Entrada de datos
2. **Function: Normalizar Entrada** - Crea objeto estándar
3. **IF: ¿Hay URL noticia?** - Decisión
4. **HTTP Request: Scraping Web** - Extrae HTML
5. **OpenAI: Limpiar Texto** - Limpia contenido
6. **IF: ¿Hay URL YouTube?** - Decisión
7. **Function: Descargar Audio YouTube** - yt-dlp o similar
8. **HTTP Request: Whisper STT** - Transcribe
9. **Function: Análisis Requisitos** - Procesa plataformas/duración
10. **OpenAI: Diseño Estructura** - Crea esquema secciones
11. **OpenAI: Refinamiento Prompt Guion** ⭐ - Genera mejor prompt
12. **OpenAI: Guion Borrador** - Genera contenido
13. **OpenAI: Revisión Experta** ⭐ - Valida calidad
14. **OpenAI: Guion Final** - Versión optimizada
15. **Function: Adaptación Plataforma** - Ajusta por TikTok/YouTube
16. **Function: Refinamiento Audio** ⭐ - Parámetros TTS
17. **Split In Batches: Secciones** - Loop por sección
18. **HTTP Request: ElevenLabs TTS** - Genera audio
19. **Function: Refinamiento Vídeo** ⭐ - Parámetros visuales
20. **HTTP Request: Pika/Runway API** - Genera vídeo
21. **Function: Empaquetado Final** - Crea JSON resultado
22. **HTTP Request / File Storage** - Guarda resultados

---

## 🎯 MÓDULOS DE REFINAMIENTO (⭐ CRÍTICOS)

Estos 3 módulos son los **diferenciadores de calidad**:

### **1. Refinamiento de Prompt de Guion**
- **¿Qué hace?**: Un LLM actúa como "ingeniero de prompts" y genera el mejor prompt posible para el guion
- **Input**: tema, contexto, plataforma, estilo, duración
- **Output**: Prompt optimizado con:
  - Rol del modelo ("Eres el mejor guionista de YouTube especializado en historia épica...")
  - Instrucciones claras de tono y estructura
  - Ejemplos si aplica
  - Restricciones (duración, fidelidad al contexto)

### **2. Refinamiento de Parámetros de Audio**
- **¿Qué hace?**: Decide los mejores parámetros TTS según contenido
- **Input**: tipo de contenido, plataforma, tono
- **Output**:
  - Voz seleccionada (masculina/femenina/neutra, edad)
  - Idioma y acento
  - Velocidad/ritmo (más rápido para TikTok, pausado para documentales)
  - Formato técnico (MP3 192kbps, etc.)

### **3. Refinamiento de Parámetros de Vídeo**
- **¿Qué hace?**: Sugiere los mejores parámetros visuales
- **Input**: plataforma, estilo, duración
- **Output**:
  - Formato: 9:16 (vertical TikTok) vs 16:9 (horizontal YouTube)
  - Resolución: 1080p o 4K
  - Estilo visual: cinemático, documental, épico, minimalista
  - Parámetros de cámara compatibles con Pika/Runway

**IMPORTANTE**: Estos módulos pueden ser:
- **Lógica estática** (Function nodes con reglas) → Más barato
- **LLM especializado** (GPT-4o-mini) → Más flexible y adaptativo

Para MVP: recomiendo **lógica estática** primero, después evolucionar a LLM si aporta valor.

---

## 💰 ESTIMACIÓN DE COSTES POR VÍDEO

### **Vídeo Corto TikTok (60s, 3 secciones)**
- Generación guion: ~$0.02 (GPT-4o-mini)
- Revisión: ~$0.01 
- Audio (500 caracteres x3): ~1,500 créditos ElevenLabs = $0.00 (dentro free tier)
- Vídeo (3 clips básicos): ~30 créditos Pika = $0.00 (dentro free tier)
- **TOTAL MVP**: ~$0.03 + free tiers

### **Vídeo Largo YouTube (20min, 10 secciones)**
- Generación guion: ~$0.10 (GPT-4o con más contexto)
- Revisión: ~$0.05
- Audio (2,000 caracteres x10): ~20,000 créditos ElevenLabs = ~$0.00-5 según plan
- Vídeo (10 clips): ~200-400 créditos Pika = $5-10 según complejidad
- **TOTAL**: ~$5-15

**Con free tiers**: Puedes probar 5-10 vídeos cortos **GRATIS** antes de pagar nada.

---

## 🔄 FLUJO DE CONTEXTO

### **Si el usuario aporta contexto** (texto, noticia, YouTube):
1. El sistema **secciona** el contenido respetando fidelidad total
2. Número de secciones = f(longitud contexto, plataforma, duración objetivo)
3. **Prioridad**: Fidelidad > Duración estimada
4. Nivel de novelización configurable (0=fiel, 1=moderado, 2=creativo)

### **Si NO hay contexto**:
1. LLM genera título optimizado para CTR (YouTube) o hook (TikTok)
2. Define estructura (intro + hook, desarrollo, clímax, cierre, CTA)
3. Redacta guion **desde cero** maximizando viralidad y retención

---

## 📦 SALIDA FINAL DEL SISTEMA

El flujo genera un **JSON estructurado** con:

```json
{
  "metadata": {
    "topic": "Historia de España 1500",
    "platforms": ["tiktok", "youtube"],
    "style": "épico",
    "duration_target": 1800,
    "generation_date": "2025-11-17",
    "sources": ["youtube:ABC123", "news:url.com/article"]
  },
  "structure": {
    "title": "El Imperio Español en 1500: De Reino a Superpotencia",
    "sections": [
      {
        "id": 1,
        "name": "Hook - La promesa de un imperio",
        "summary": "...",
        "duration_seconds": 15,
        "script": "Texto completo del guion...",
        "audio_url": "path/audio_section_1.mp3",
        "video_url": "path/video_section_1.mp4"
      }
    ]
  },
  "prompts_used": {
    "script_refinement_prompt": "Eres el mejor guionista...",
    "script_generation_prompt": "Genera un guion...",
    "review_prompt": "Revisa coherencia..."
  },
  "parameters_used": {
    "audio": {
      "voice": "masculine_deep",
      "language": "es-ES",
      "speed": 0.95
    },
    "video": {
      "format": "16:9",
      "resolution": "1080p",
      "style": "cinematic_epic"
    }
  }
}
```

Este JSON permite:
- Revisar qué se generó y cómo
- Regenerar partes específicas
- Montar manualmente si el montaje automático falla
- Auditar decisiones del sistema

---

## ⚡ SIGUIENTE PASO

Ahora que tienes el **mapa completo**, podemos empezar a **diseñar cada módulo en detalle**:

1. **Primero**: Nodos de entrada y normalización
2. **Segundo**: Enriquecimiento de contexto (scraping + STT)
3. **Tercero**: Pipeline de generación de guion (con refinamiento)
4. **Cuarto**: Generación de audio y vídeo (con refinamiento)
5. **Quinto**: Empaquetado y almacenamiento
6. **Finalmente**: JSON de n8n completo e importable

---

## 📋 RESUMEN DE LO INVESTIGADO Y DECIDIDO

### **Investigación realizada**:
1. ✅ Precios actualizados de OpenAI GPT-4o/mini (2024-2025)
2. ✅ Planes y free tiers de ElevenLabs TTS
3. ✅ Comparativa exhaustiva de Pika vs Runway vs Minimax vs Sora
4. ✅ Opciones de transcripción YouTube (API + Whisper)
5. ✅ Integración nativa de OpenAI en n8n

### **Decisiones clave**:
1. ✅ **MVP**: GPT-4o-mini + ElevenLabs + Pika Labs (mejor coste/calidad)
2. ✅ **3 módulos de refinamiento** como diferenciadores de calidad
3. ✅ **Entrada manual/Webhook** prioritaria, otros canales en Fase 2
4. ✅ **Pipeline de 15 pasos** desde entrada hasta empaquetado
5. ✅ **Fidelidad > Duración** cuando hay contexto aportado
6. ✅ **Free tiers** permiten 5-10 pruebas antes de pagar

### **Lo que vamos a construir**:
Un sistema **profesional, escalable y mantenible** que:
- Recibe un tema + contexto opcional (texto/noticias/YouTube)
- Genera guiones optimizados para viralidad y retención
- Produce audio natural con ElevenLabs
- Genera vídeos adaptados a cada plataforma (vertical/horizontal)
- Guarda todo en JSON estructurado para revisar o reutilizar
- Funciona con free tiers para MVP, escala a producción sin cambios de arquitectura

**Coste estimado**: $0.03-0.10 por vídeo corto, $5-15 por vídeo largo (sin free tiers).

---

# 🚀 DESARROLLO COMPLETO DEL SISTEMA - MÓDULO 1: ENTRADA Y NORMALIZACIÓN

---

## 📥 MÓDULO 1: SISTEMA DE ENTRADA Y NORMALIZACIÓN DE DATOS

Este es el **cerebro de entrada** del sistema. Aquí recibimos la petición del usuario y la convertimos en un objeto estándar que todo el resto del flujo entiende perfectamente.

---

## 🎯 OBJETIVO DEL MÓDULO

Transformar **cualquier tipo de entrada** (manual, webhook, email futuro, etc.) en un **objeto JSON normalizado** con esta estructura:

```json
{
  "request_id": "uuid-generado",
  "timestamp": "2025-11-17T10:30:00Z",
  "input_channel": "manual",
  "user_input": {
    "topic": "Historia de España en 1500",
    "platforms": ["tiktok", "youtube"],
    "context_raw": "Texto pegado opcional...",
    "style": "épico",
    "desired_duration": 1800,
    "novelization_level": 1,
    "links": [
      "https://elpais.com/historia/articulo-ejemplo"
    ],
    "youtube_urls": [
      "https://youtube.com/watch?v=ABC123"
    ],
    "extra_preferences": {
      "target_audience": "adultos 25-45",
      "pace": "moderado",
      "include_cta": true
    }
  },
  "validation": {
    "is_valid": true,
    "errors": [],
    "warnings": ["Duración muy larga para TikTok, se adaptará"]
  }
}
```

---

## 🏗️ NODOS DEL MÓDULO 1

### **NODO 1.1: Manual Trigger / Webhook**
**Tipo**: `Manual Trigger` o `Webhook`  
**Nombre**: `📥 Entrada Manual MVP`  
**Fase**: MVP (Fase 1)

**Configuración**:
- **Manual Trigger**: Para pruebas y entrada directa desde n8n
- **Webhook**: Path `/create-content` (POST)
- Acepta JSON con los siguientes campos:

```json
{
  "topic": "string (obligatorio)",
  "platforms": "array de strings (obligatorio)",
  "context_raw": "string (opcional)",
  "style": "string (opcional)",
  "desired_duration": "number en segundos (opcional)",
  "novelization_level": "number 0-2 (opcional, default: 1)",
  "links": "array de URLs (opcional)",
  "youtube_urls": "array de URLs YouTube (opcional)",
  "extra_preferences": "object (opcional)"
}
```

**Ejemplo de payload**:
```json
{
  "topic": "El Imperio Español en 1500",
  "platforms": ["tiktok", "youtube"],
  "context_raw": "",
  "style": "épico y divulgativo",
  "desired_duration": 90,
  "novelization_level": 1,
  "links": [],
  "youtube_urls": ["https://youtube.com/watch?v=dQw4w9WgXcQ"],
  "extra_preferences": {
    "target_audience": "jóvenes interesados en historia",
    "include_cta": true
  }
}
```

**Salida**: 
- Objeto JSON raw tal como lo envió el usuario
- Variable `$json` disponible para siguiente nodo

---

### **NODO 1.2: Function - Validación Inicial**
**Tipo**: `Function`  
**Nombre**: `✅ Validación de Entrada`  
**Fase**: MVP

**Propósito**: 
Validar que los datos mínimos estén presentes y sean correctos antes de continuar.

**Código JavaScript**:
```javascript
// Recibimos los datos del trigger
const input = $input.all()[0].json;

// Inicializamos objeto de validación
const validation = {
  is_valid: true,
  errors: [],
  warnings: []
};

// VALIDACIONES OBLIGATORIAS
if (!input.topic || input.topic.trim() === '') {
  validation.is_valid = false;
  validation.errors.push('El campo "topic" es obligatorio');
}

if (!input.platforms || !Array.isArray(input.platforms) || input.platforms.length === 0) {
  validation.is_valid = false;
  validation.errors.push('Debes especificar al menos una plataforma (platforms)');
}

// Validar plataformas válidas
const valid_platforms = ['tiktok', 'youtube', 'instagram', 'twitter'];
if (input.platforms) {
  const invalid = input.platforms.filter(p => !valid_platforms.includes(p.toLowerCase()));
  if (invalid.length > 0) {
    validation.warnings.push(`Plataformas no reconocidas: ${invalid.join(', ')}`);
  }
}

// VALIDACIONES OPCIONALES CON WARNINGS
if (input.desired_duration) {
  if (input.platforms.includes('tiktok') && input.desired_duration > 180) {
    validation.warnings.push('TikTok: duración muy larga (>3 min), se adaptará a múltiples vídeos o se recortará');
  }
  if (input.desired_duration < 15) {
    validation.warnings.push('Duración muy corta (<15s), puede no ser suficiente para contenido de calidad');
  }
}

// Validar novelization_level
if (input.novelization_level !== undefined) {
  if (input.novelization_level < 0 || input.novelization_level > 2) {
    validation.warnings.push('novelization_level debe estar entre 0-2, se ajustará a 1 (moderado)');
    input.novelization_level = 1;
  }
}

// Validar URLs
if (input.links && input.links.length > 0) {
  input.links.forEach((link, idx) => {
    if (!link.startsWith('http://') && !link.startsWith('https://')) {
      validation.warnings.push(`Link ${idx + 1} no tiene protocolo HTTP/HTTPS`);
    }
  });
}

if (input.youtube_urls && input.youtube_urls.length > 0) {
  input.youtube_urls.forEach((url, idx) => {
    if (!url.includes('youtube.com') && !url.includes('youtu.be')) {
      validation.warnings.push(`URL ${idx + 1} no parece ser de YouTube`);
    }
  });
}

// Si hay errores críticos, detenemos el flujo
if (!validation.is_valid) {
  throw new Error(`Validación fallida: ${validation.errors.join(', ')}`);
}

// Devolvemos input original + validación
return {
  json: {
    input: input,
    validation: validation
  }
};
```

**Entrada**: Objeto JSON del trigger  
**Salida**: 
```json
{
  "input": { /* datos originales */ },
  "validation": {
    "is_valid": true,
    "errors": [],
    "warnings": ["..."]
  }
}
```

---

### **NODO 1.3: Function - Normalización**
**Tipo**: `Function`  
**Nombre**: `🔄 Normalización de Datos`  
**Fase**: MVP

**Propósito**:
Crear el objeto **estándar normalizado** que usará todo el sistema, con valores por defecto inteligentes.

**Código JavaScript**:
```javascript
const { v4: uuidv4 } = require('uuid'); // n8n incluye uuid por defecto
const data = $input.all()[0].json;
const input = data.input;
const validation = data.validation;

// Generar ID único para esta petición
const request_id = uuidv4();
const timestamp = new Date().toISOString();

// NORMALIZACIÓN CON DEFAULTS INTELIGENTES
const normalized = {
  request_id: request_id,
  timestamp: timestamp,
  input_channel: 'manual', // En futuro: 'email', 'whatsapp', 'telegram'
  
  user_input: {
    // Campo obligatorio
    topic: input.topic.trim(),
    
    // Plataformas normalizadas a lowercase
    platforms: input.platforms.map(p => p.toLowerCase()),
    
    // Contexto (puede estar vacío)
    context_raw: input.context_raw || '',
    
    // Estilo por defecto según plataforma principal
    style: input.style || detectDefaultStyle(input.platforms[0]),
    
    // Duración por defecto según plataforma
    desired_duration: input.desired_duration || detectDefaultDuration(input.platforms[0]),
    
    // Nivel de novelización (0=fiel, 1=moderado, 2=creativo)
    novelization_level: input.novelization_level !== undefined ? input.novelization_level : 1,
    
    // Enlaces externos
    links: input.links || [],
    youtube_urls: input.youtube_urls || [],
    
    // Preferencias extra
    extra_preferences: input.extra_preferences || {}
  },
  
  // Validación incluida
  validation: validation,
  
  // Metadata adicional
  metadata: {
    has_external_context: (input.context_raw && input.context_raw.length > 0) || 
                          (input.links && input.links.length > 0) || 
                          (input.youtube_urls && input.youtube_urls.length > 0),
    needs_scraping: (input.links && input.links.length > 0),
    needs_youtube_transcription: (input.youtube_urls && input.youtube_urls.length > 0),
    estimated_complexity: calculateComplexity(input)
  }
};

// FUNCIÓN: Detectar estilo por defecto según plataforma
function detectDefaultStyle(platform) {
  const styles = {
    'tiktok': 'dinámico y enérgico',
    'youtube': 'narrativo y profundo',
    'instagram': 'visual y aspiracional',
    'twitter': 'conciso y impactante'
  };
  return styles[platform] || 'equilibrado';
}

// FUNCIÓN: Detectar duración por defecto según plataforma
function detectDefaultDuration(platform) {
  const durations = {
    'tiktok': 60,      // 1 minuto
    'youtube': 1200,   // 20 minutos
    'instagram': 90,   // 1.5 minutos
    'twitter': 45      // 45 segundos
  };
  return durations[platform] || 600; // 10 min por defecto
}

// FUNCIÓN: Calcular complejidad estimada
function calculateComplexity(input) {
  let score = 0;
  
  // +1 por cada plataforma adicional
  score += (input.platforms?.length || 1) - 1;
  
  // +2 si hay contexto externo largo
  if (input.context_raw && input.context_raw.length > 5000) score += 2;
  else if (input.context_raw && input.context_raw.length > 0) score += 1;
  
  // +1 por cada enlace a scrapear
  score += (input.links?.length || 0);
  
  // +2 por cada vídeo de YouTube (transcripción costosa)
  score += (input.youtube_urls?.length || 0) * 2;
  
  // +1 si duración es muy larga
  if (input.desired_duration && input.desired_duration > 1200) score += 1;
  
  if (score === 0) return 'simple';
  if (score <= 3) return 'moderada';
  if (score <= 6) return 'compleja';
  return 'muy_compleja';
}

return {
  json: normalized
};
```

**Entrada**: Objeto con `input` y `validation`  
**Salida**: Objeto normalizado completo (estructura estándar)

---

### **NODO 1.4: Set - Guardar en Variables Globales**
**Tipo**: `Set`  
**Nombre**: `💾 Variables Globales`  
**Fase**: MVP

**Propósito**:
Guardar los valores normalizados en variables que podemos usar en **cualquier nodo posterior** del flujo.

**Configuración**:
```
Keep Only Set: false (mantener todo el objeto JSON)

Values to Set:
- request_id → {{ $json.request_id }}
- topic → {{ $json.user_input.topic }}
- platforms → {{ $json.user_input.platforms }}
- style → {{ $json.user_input.style }}
- desired_duration → {{ $json.user_input.desired_duration }}
- novelization_level → {{ $json.user_input.novelization_level }}
- has_context → {{ $json.metadata.has_external_context }}
- needs_scraping → {{ $json.metadata.needs_scraping }}
- needs_youtube → {{ $json.metadata.needs_youtube_transcription }}
- complexity → {{ $json.metadata.estimated_complexity }}
```

**Salida**: Mismo objeto JSON + variables accesibles globalmente

---

### **NODO 1.5: IF - Decisión de Flujo**
**Tipo**: `IF`  
**Nombre**: `🔀 ¿Hay Contexto Externo?`  
**Fase**: MVP

**Propósito**:
Decidir si necesitamos enriquecer el contexto (scraping web, transcripción YouTube) o podemos ir directo a generación.

**Condiciones**:
```
IF:
  {{ $json.metadata.has_external_context }} == true

THEN: 
  → Ruta "ENRIQUECIMIENTO DE CONTEXTO" (Módulo 2)

ELSE:
  → Ruta "ANÁLISIS Y DISEÑO DIRECTO" (Módulo 3)
```

**Salida**: 
- **True branch**: Necesita scraping/transcripción
- **False branch**: Va directo a análisis

---

## 📊 RESUMEN DEL MÓDULO 1

### **Nodos implementados**:
1. ✅ **Manual Trigger/Webhook** - Recibe entrada
2. ✅ **Validación** - Verifica campos obligatorios
3. ✅ **Normalización** - Crea objeto estándar
4. ✅ **Variables Globales** - Guarda datos accesibles
5. ✅ **Decisión de Flujo** - Bifurca según contexto

### **Flujo visual**:
```
[📥 Entrada Manual]
       ↓
[✅ Validación]
       ↓
[🔄 Normalización]
       ↓
[💾 Variables Globales]
       ↓
[🔀 ¿Contexto Externo?]
   ↙️           ↘️
[SÍ]          [NO]
   ↓            ↓
[Módulo 2]  [Módulo 3]
```

### **Datos generados hasta ahora**:
- ✅ `request_id` único
- ✅ `timestamp` de creación
- ✅ Campos normalizados con defaults inteligentes
- ✅ Flags de decisión (`has_context`, `needs_scraping`, `needs_youtube`)
- ✅ Complejidad estimada
- ✅ Validaciones y warnings

---

## 🎯 SIGUIENTE PASO: MÓDULO 2

Ahora vamos a diseñar el **Módulo 2: Enriquecimiento de Contexto**, que incluye:
- 🕷️ **Scraping de noticias/artículos**
- 🎥 **Transcripción de vídeos de YouTube**
- 🧹 **Limpieza y estructuración del texto**
- ➕ **Fusión con contexto directo (si existe)**

¿Continuamos con el Módulo 2 o prefieres que primero te genere el **JSON parcial de n8n** con lo que llevamos hasta ahora?

# 🚀 JSON DE N8N - MÓDULOS 1 Y 2 COMPLETOS (PYTHON)

Aquí tienes el workflow completo de los módulos 1 y 2 en formato JSON importable a n8n, **usando Python en lugar de JavaScript**.

---

## 📦 WORKFLOW JSON COMPLETO

```json
{
  "name": "Sistema Creación Contenido Multimedia - MVP v1.0",
  "nodes": [
    {
      "parameters": {
        "httpMethod": "POST",
        "path": "create-content",
        "responseMode": "responseNode",
        "options": {}
      },
      "id": "webhook-entrada",
      "name": "📥 Entrada Webhook",
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 1.1,
      "position": [250, 300],
      "webhookId": "create-content-webhook"
    },
    {
      "parameters": {
        "functionCode": "# Importar librerías necesarias\nimport json\nfrom datetime import datetime\n\n# Recibir datos del webhook\ninput_data = _input.all()[0].json\n\n# Inicializar objeto de validación\nvalidation = {\n    \"is_valid\": True,\n    \"errors\": [],\n    \"warnings\": []\n}\n\n# ============================================\n# VALIDACIONES OBLIGATORIAS\n# ============================================\n\n# Validar campo 'topic'\nif not input_data.get('topic') or str(input_data.get('topic', '')).strip() == '':\n    validation['is_valid'] = False\n    validation['errors'].append('El campo \"topic\" es obligatorio')\n\n# Validar campo 'platforms'\nif not input_data.get('platforms') or not isinstance(input_data.get('platforms'), list) or len(input_data.get('platforms')) == 0:\n    validation['is_valid'] = False\n    validation['errors'].append('Debes especificar al menos una plataforma (platforms)')\n\n# Validar plataformas válidas\nvalid_platforms = ['tiktok', 'youtube', 'instagram', 'twitter']\nif input_data.get('platforms'):\n    invalid_platforms = [p for p in input_data['platforms'] if p.lower() not in valid_platforms]\n    if invalid_platforms:\n        validation['warnings'].append(f\"Plataformas no reconocidas: {', '.join(invalid_platforms)}\")\n\n# ============================================\n# VALIDACIONES OPCIONALES CON WARNINGS\n# ============================================\n\n# Validar duración deseada\nif input_data.get('desired_duration'):\n    duration = input_data['desired_duration']\n    \n    if 'tiktok' in [p.lower() for p in input_data.get('platforms', [])] and duration > 180:\n        validation['warnings'].append('TikTok: duración muy larga (>3 min), se adaptará a múltiples vídeos o se recortará')\n    \n    if duration < 15:\n        validation['warnings'].append('Duración muy corta (<15s), puede no ser suficiente para contenido de calidad')\n\n# Validar nivel de novelización\nif 'novelization_level' in input_data:\n    level = input_data['novelization_level']\n    if level < 0 or level > 2:\n        validation['warnings'].append('novelization_level debe estar entre 0-2, se ajustará a 1 (moderado)')\n        input_data['novelization_level'] = 1\n\n# Validar URLs de enlaces\nif input_data.get('links') and len(input_data['links']) > 0:\n    for idx, link in enumerate(input_data['links']):\n        if not link.startswith('http://') and not link.startswith('https://'):\n            validation['warnings'].append(f\"Link {idx + 1} no tiene protocolo HTTP/HTTPS\")\n\n# Validar URLs de YouTube\nif input_data.get('youtube_urls') and len(input_data['youtube_urls']) > 0:\n    for idx, url in enumerate(input_data['youtube_urls']):\n        if 'youtube.com' not in url and 'youtu.be' not in url:\n            validation['warnings'].append(f\"URL {idx + 1} no parece ser de YouTube\")\n\n# ============================================\n# DETENER FLUJO SI HAY ERRORES CRÍTICOS\n# ============================================\n\nif not validation['is_valid']:\n    raise Exception(f\"Validación fallida: {', '.join(validation['errors'])}\")\n\n# ============================================\n# DEVOLVER RESULTADO\n# ============================================\n\nreturn {\n    'json': {\n        'input': input_data,\n        'validation': validation\n    }\n}"
      },
      "id": "validacion-entrada",
      "name": "✅ Validación de Entrada",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [450, 300]
    },
    {
      "parameters": {
        "functionCode": "# Importar librerías\nimport json\nimport uuid\nfrom datetime import datetime\n\n# Recibir datos validados\ndata = _input.all()[0].json\ninput_data = data['input']\nvalidation = data['validation']\n\n# ============================================\n# FUNCIONES AUXILIARES\n# ============================================\n\ndef detect_default_style(platform):\n    \"\"\"Detectar estilo por defecto según plataforma\"\"\"\n    styles = {\n        'tiktok': 'dinámico y enérgico',\n        'youtube': 'narrativo y profundo',\n        'instagram': 'visual y aspiracional',\n        'twitter': 'conciso y impactante'\n    }\n    return styles.get(platform, 'equilibrado')\n\ndef detect_default_duration(platform):\n    \"\"\"Detectar duración por defecto según plataforma\"\"\"\n    durations = {\n        'tiktok': 60,       # 1 minuto\n        'youtube': 1200,    # 20 minutos\n        'instagram': 90,    # 1.5 minutos\n        'twitter': 45       # 45 segundos\n    }\n    return durations.get(platform, 600)  # 10 min por defecto\n\ndef calculate_complexity(input_obj):\n    \"\"\"Calcular complejidad estimada de la petición\"\"\"\n    score = 0\n    \n    # +1 por cada plataforma adicional\n    platforms = input_obj.get('platforms', [])\n    score += max(0, len(platforms) - 1)\n    \n    # +2 si hay contexto externo largo, +1 si es corto\n    context_raw = input_obj.get('context_raw', '')\n    if context_raw and len(context_raw) > 5000:\n        score += 2\n    elif context_raw:\n        score += 1\n    \n    # +1 por cada enlace a scrapear\n    links = input_obj.get('links', [])\n    score += len(links)\n    \n    # +2 por cada vídeo de YouTube (transcripción costosa)\n    youtube_urls = input_obj.get('youtube_urls', [])\n    score += len(youtube_urls) * 2\n    \n    # +1 si duración es muy larga\n    duration = input_obj.get('desired_duration')\n    if duration and duration > 1200:\n        score += 1\n    \n    # Clasificar complejidad\n    if score == 0:\n        return 'simple'\n    elif score <= 3:\n        return 'moderada'\n    elif score <= 6:\n        return 'compleja'\n    else:\n        return 'muy_compleja'\n\n# ============================================\n# NORMALIZACIÓN CON DEFAULTS INTELIGENTES\n# ============================================\n\n# Generar ID único y timestamp\nrequest_id = str(uuid.uuid4())\ntimestamp = datetime.utcnow().isoformat() + 'Z'\n\n# Normalizar plataformas\nplatforms_normalized = [p.lower() for p in input_data.get('platforms', [])]\nprimary_platform = platforms_normalized[0] if platforms_normalized else 'youtube'\n\n# Construir objeto normalizado\nnormalized = {\n    'request_id': request_id,\n    'timestamp': timestamp,\n    'input_channel': 'webhook',  # En futuro: 'email', 'whatsapp', 'telegram'\n    \n    'user_input': {\n        # Campo obligatorio\n        'topic': input_data['topic'].strip(),\n        \n        # Plataformas normalizadas\n        'platforms': platforms_normalized,\n        \n        # Contexto (puede estar vacío)\n        'context_raw': input_data.get('context_raw', ''),\n        \n        # Estilo por defecto según plataforma principal\n        'style': input_data.get('style') or detect_default_style(primary_platform),\n        \n        # Duración por defecto según plataforma\n        'desired_duration': input_data.get('desired_duration') or detect_default_duration(primary_platform),\n        \n        # Nivel de novelización (0=fiel, 1=moderado, 2=creativo)\n        'novelization_level': input_data.get('novelization_level', 1),\n        \n        # Enlaces externos\n        'links': input_data.get('links', []),\n        'youtube_urls': input_data.get('youtube_urls', []),\n        \n        # Preferencias extra\n        'extra_preferences': input_data.get('extra_preferences', {})\n    },\n    \n    # Validación incluida\n    'validation': validation,\n    \n    # Metadata adicional\n    'metadata': {\n        'has_external_context': (\n            (input_data.get('context_raw') and len(input_data['context_raw']) > 0) or\n            (input_data.get('links') and len(input_data['links']) > 0) or\n            (input_data.get('youtube_urls') and len(input_data['youtube_urls']) > 0)\n        ),\n        'needs_scraping': bool(input_data.get('links') and len(input_data['links']) > 0),\n        'needs_youtube_transcription': bool(input_data.get('youtube_urls') and len(input_data['youtube_urls']) > 0),\n        'estimated_complexity': calculate_complexity(input_data)\n    }\n}\n\n# ============================================\n# DEVOLVER RESULTADO\n# ============================================\n\nreturn {'json': normalized}"
      },
      "id": "normalizacion",
      "name": "🔄 Normalización de Datos",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [650, 300]
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "var_request_id",
              "name": "request_id",
              "value": "={{ $json.request_id }}",
              "type": "string"
            },
            {
              "id": "var_topic",
              "name": "topic",
              "value": "={{ $json.user_input.topic }}",
              "type": "string"
            },
            {
              "id": "var_platforms",
              "name": "platforms",
              "value": "={{ $json.user_input.platforms }}",
              "type": "array"
            },
            {
              "id": "var_style",
              "name": "style",
              "value": "={{ $json.user_input.style }}",
              "type": "string"
            },
            {
              "id": "var_duration",
              "name": "desired_duration",
              "value": "={{ $json.user_input.desired_duration }}",
              "type": "number"
            },
            {
              "id": "var_novelization",
              "name": "novelization_level",
              "value": "={{ $json.user_input.novelization_level }}",
              "type": "number"
            },
            {
              "id": "var_has_context",
              "name": "has_context",
              "value": "={{ $json.metadata.has_external_context }}",
              "type": "boolean"
            },
            {
              "id": "var_needs_scraping",
              "name": "needs_scraping",
              "value": "={{ $json.metadata.needs_scraping }}",
              "type": "boolean"
            },
            {
              "id": "var_needs_youtube",
              "name": "needs_youtube",
              "value": "={{ $json.metadata.needs_youtube_transcription }}",
              "type": "boolean"
            },
            {
              "id": "var_complexity",
              "name": "complexity",
              "value": "={{ $json.metadata.estimated_complexity }}",
              "type": "string"
            }
          ]
        },
        "options": {
          "keepOnlySet": false
        }
      },
      "id": "variables-globales",
      "name": "💾 Variables Globales",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.3,
      "position": [850, 300]
    },
    {
      "parameters": {
        "conditions": {
          "boolean": [
            {
              "value1": "={{ $json.metadata.has_external_context }}",
              "value2": true
            }
          ]
        }
      },
      "id": "decision-contexto",
      "name": "🔀 ¿Hay Contexto Externo?",
      "type": "n8n-nodes-base.if",
      "typeVersion": 2,
      "position": [1050, 300]
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "status",
              "name": "status",
              "value": "sin_contexto_externo",
              "type": "string"
            },
            {
              "id": "message",
              "name": "message",
              "value": "No hay contexto externo, proceder directamente a análisis",
              "type": "string"
            }
          ]
        }
      },
      "id": "sin-contexto",
      "name": "📝 Sin Contexto - Directo",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.3,
      "position": [1250, 400]
    },
    {
      "parameters": {
        "functionCode": "# Preparar datos para enriquecimiento\nimport json\n\ndata = _input.all()[0].json\n\n# Extraer URLs que necesitan procesamiento\nlinks_to_scrape = data['user_input'].get('links', [])\nyoutube_urls = data['user_input'].get('youtube_urls', [])\ncontext_raw = data['user_input'].get('context_raw', '')\n\n# Crear estructura para procesamiento paralelo\nenrichment_tasks = []\n\n# Tarea 1: Contexto directo (si existe)\nif context_raw:\n    enrichment_tasks.append({\n        'type': 'direct_context',\n        'content': context_raw,\n        'source': 'user_input'\n    })\n\n# Tarea 2: Scraping de enlaces\nfor idx, url in enumerate(links_to_scrape):\n    enrichment_tasks.append({\n        'type': 'web_scraping',\n        'url': url,\n        'source': f'link_{idx + 1}'\n    })\n\n# Tarea 3: Transcripción YouTube\nfor idx, url in enumerate(youtube_urls):\n    enrichment_tasks.append({\n        'type': 'youtube_transcription',\n        'url': url,\n        'source': f'youtube_{idx + 1}'\n    })\n\n# Devolver lista de tareas\nreturn [\n    {'json': {**data, 'enrichment_tasks': enrichment_tasks}}\n]"
      },
      "id": "preparar-enriquecimiento",
      "name": "🎯 Preparar Enriquecimiento",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [1250, 200]
    },
    {
      "parameters": {
        "batchSize": 1,
        "options": {}
      },
      "id": "loop-tareas",
      "name": "🔁 Loop Tareas Enriquecimiento",
      "type": "n8n-nodes-base.splitInBatches",
      "typeVersion": 3,
      "position": [1450, 200]
    },
    {
      "parameters": {
        "conditions": {
          "string": [
            {
              "value1": "={{ $json.enrichment_tasks[$context.index].type }}",
              "operation": "equals",
              "value2": "web_scraping"
            }
          ]
        }
      },
      "id": "es-scraping",
      "name": "🕷️ ¿Es Scraping Web?",
      "type": "n8n-nodes-base.if",
      "typeVersion": 2,
      "position": [1650, 200]
    },
    {
      "parameters": {
        "url": "={{ $json.enrichment_tasks[$context.index].url }}",
        "options": {
          "timeout": 30000
        }
      },
      "id": "scraping-web",
      "name": "🌐 HTTP Request - Scraping",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [1850, 100]
    },
    {
      "parameters": {
        "functionCode": "# Limpiar HTML y extraer contenido relevante\nimport re\nfrom html import unescape\n\ndata = _input.all()[0].json\nhtml_content = data.get('body', '')\nsource_url = data.get('enrichment_tasks', [{}])[0].get('url', '')\n\n# ============================================\n# LIMPIEZA BÁSICA DE HTML\n# ============================================\n\n# Eliminar scripts y estilos\nhtml_content = re.sub(r'<script[^>]*>.*?</script>', '', html_content, flags=re.DOTALL | re.IGNORECASE)\nhtml_content = re.sub(r'<style[^>]*>.*?</style>', '', html_content, flags=re.DOTALL | re.IGNORECASE)\n\n# Eliminar comentarios HTML\nhtml_content = re.sub(r'<!--.*?-->', '', html_content, flags=re.DOTALL)\n\n# Eliminar etiquetas HTML\ntext_only = re.sub(r'<[^>]+>', '', html_content)\n\n# Decodificar entidades HTML\ntext_only = unescape(text_only)\n\n# Limpiar espacios múltiples y líneas vacías\ntext_only = re.sub(r'\\s+', ' ', text_only)\ntext_only = text_only.strip()\n\n# ============================================\n# EXTRAER METADATA BÁSICA (TITLE)\n# ============================================\n\ntitle_match = re.search(r'<title[^>]*>(.*?)</title>', html_content, re.IGNORECASE | re.DOTALL)\ntitle = title_match.group(1).strip() if title_match else 'Sin título'\n\n# ============================================\n# PREPARAR PARA LIMPIEZA CON LLM\n# ============================================\n\n# Truncar si es muy largo (para no exceder límites de GPT)\nmax_chars = 50000\nif len(text_only) > max_chars:\n    text_only = text_only[:max_chars] + '... [contenido truncado]'\n\nresult = {\n    'type': 'web_scraping',\n    'source': source_url,\n    'title': title,\n    'raw_text': text_only,\n    'char_count': len(text_only),\n    'needs_llm_cleaning': True\n}\n\nreturn {'json': result}"
      },
      "id": "limpiar-html",
      "name": "🧹 Limpiar HTML Básico",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [2050, 100]
    },
    {
      "parameters": {
        "conditions": {
          "string": [
            {
              "value1": "={{ $json.enrichment_tasks[$context.index].type }}",
              "operation": "equals",
              "value2": "youtube_transcription"
            }
          ]
        }
      },
      "id": "es-youtube",
      "name": "🎥 ¿Es YouTube?",
      "type": "n8n-nodes-base.if",
      "typeVersion": 2,
      "position": [1850, 300]
    },
    {
      "parameters": {
        "functionCode": "# Extraer ID de vídeo de YouTube\nimport re\n\ndata = _input.all()[0].json\nyoutube_url = data['enrichment_tasks'][data['$context']['index']]['url']\n\n# Patrones para extraer ID de YouTube\npatterns = [\n    r'(?:v=|\\/)([0-9A-Za-z_-]{11}).*',\n    r'(?:embed\\/|v\\/|watch\\?v=)([^&\\s]+)',\n    r'youtu\\.be\\/([^&\\s]+)'\n]\n\nvideo_id = None\nfor pattern in patterns:\n    match = re.search(pattern, youtube_url)\n    if match:\n        video_id = match.group(1)\n        break\n\nif not video_id:\n    raise Exception(f'No se pudo extraer ID de YouTube de: {youtube_url}')\n\nreturn {\n    'json': {\n        'video_id': video_id,\n        'youtube_url': youtube_url,\n        'type': 'youtube_transcription'\n    }\n}"
      },
      "id": "extraer-video-id",
      "name": "🔍 Extraer Video ID",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [2050, 200]
    },
    {
      "parameters": {
        "url": "=https://www.youtube.com/watch?v={{ $json.video_id }}",
        "options": {
          "timeout": 30000
        }
      },
      "id": "descargar-pagina-youtube",
      "name": "📥 Descargar Página YouTube",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [2250, 200]
    },
    {
      "parameters": {
        "functionCode": "# Intentar extraer transcripción automática de YouTube\nimport re\nimport json as json_lib\nimport urllib.parse\n\ndata = _input.all()[0].json\nhtml_content = data.get('body', '')\nvideo_id = data.get('video_id', '')\n\n# ============================================\n# MÉTODO 1: Extraer captions desde HTML\n# ============================================\n\n# Buscar el objeto de configuración de YouTube que contiene captions\ncaption_pattern = r'\"captions\":\\s*({[^}]+\"playerCaptionsTracklistRenderer\"[^}]+})'  \nmatch = re.search(caption_pattern, html_content)\n\nsubtitles_text = None\nhas_captions = False\n\nif match:\n    try:\n        # Extraer URLs de subtítulos\n        caption_data = match.group(1)\n        # Buscar URL base de subtítulos\n        url_pattern = r'\"baseUrl\":\"([^\"]+)\"'\n        url_match = re.search(url_pattern, caption_data)\n        \n        if url_match:\n            caption_url = url_match.group(1)\n            # Decodificar URL\n            caption_url = caption_url.encode().decode('unicode_escape')\n            has_captions = True\n            \n            # La URL de captions se procesará en siguiente nodo\n            return {\n                'json': {\n                    'video_id': video_id,\n                    'has_captions': True,\n                    'caption_url': caption_url,\n                    'method': 'youtube_native'\n                }\n            }\n    except Exception as e:\n        pass\n\n# ============================================\n# MÉTODO 2: Fallback a Whisper API\n# ============================================\n\nif not has_captions:\n    # No hay subtítulos nativos, necesitamos usar Whisper\n    return {\n        'json': {\n            'video_id': video_id,\n            'has_captions': False,\n            'needs_whisper': True,\n            'youtube_url': f'https://www.youtube.com/watch?v={video_id}',\n            'method': 'whisper_fallback'\n        }\n    }\n\nraise Exception('No se pudo determinar método de transcripción')"
      },
      "id": "detectar-transcripcion",
      "name": "🔎 Detectar Método Transcripción",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [2450, 200]
    },
    {
      "parameters": {
        "conditions": {
          "boolean": [
            {
              "value1": "={{ $json.has_captions }}",
              "value2": true
            }
          ]
        }
      },
      "id": "tiene-captions",
      "name": "📝 ¿Tiene Captions Nativos?",
      "type": "n8n-nodes-base.if",
      "typeVersion": 2,
      "position": [2650, 200]
    },
    {
      "parameters": {
        "url": "={{ $json.caption_url }}",
        "options": {}
      },
      "id": "descargar-captions",
      "name": "📄 Descargar Captions",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [2850, 100]
    },
    {
      "parameters": {
        "functionCode": "# Parsear XML de captions de YouTube\nimport re\nimport html\n\ndata = _input.all()[0].json\nxml_content = data.get('body', '')\n\n# Extraer texto de tags <text>\ntext_pattern = r'<text[^>]*>([^<]+)</text>'\nmatches = re.findall(text_pattern, xml_content)\n\n# Unir todo el texto y decodificar HTML entities\nfull_text = ' '.join(matches)\nfull_text = html.unescape(full_text)\n\n# Limpiar espacios múltiples\nfull_text = re.sub(r'\\s+', ' ', full_text).strip()\n\nresult = {\n    'type': 'youtube_transcription',\n    'source': data.get('youtube_url', 'YouTube'),\n    'transcription': full_text,\n    'method': 'youtube_native_captions',\n    'char_count': len(full_text),\n    'needs_llm_cleaning': True\n}\n\nreturn {'json': result}"
      },
      "id": "parsear-captions",
      "name": "📖 Parsear Captions XML",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [3050, 100]
    },
    {
      "parameters": {
        "functionCode": "# Preparar para usar Whisper API como fallback\nimport json\n\ndata = _input.all()[0].json\n\n# Nota: Aquí necesitaríamos descargar el audio del vídeo\n# Esto requiere herramientas externas como yt-dlp\n# Por simplicidad, marcamos que necesita procesamiento manual\n\nresult = {\n    'type': 'youtube_transcription',\n    'source': data.get('youtube_url', ''),\n    'video_id': data.get('video_id', ''),\n    'transcription': '[PENDIENTE: Requiere descarga de audio con yt-dlp y llamada a Whisper API]',\n    'method': 'whisper_pending',\n    'needs_manual_processing': True,\n    'char_count': 0\n}\n\nreturn {'json': result}"
      },
      "id": "whisper-fallback",
      "name": "🎤 Whisper Fallback (Pendiente)",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [2850, 300]
    },
    {
      "parameters": {
        "functionCode": "# Consolidar resultados de contexto directo\ndata = _input.all()[0].json\ntask = data['enrichment_tasks'][data.get('$context', {}).get('index', 0)]\n\nresult = {\n    'type': 'direct_context',\n    'source': 'user_input',\n    'content': task.get('content', ''),\n    'char_count': len(task.get('content', '')),\n    'needs_llm_cleaning': False  # Ya es texto limpio\n}\n\nreturn {'json': result}"
      },
      "id": "contexto-directo",
      "name": "📝 Contexto Directo",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [1850, 500]
    },
    {
      "parameters": {
        "assignments": {
          "assignments": []
        }
      },
      "id": "merge-enriquecimiento",
      "name": "🔗 Merge Resultados",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.3,
      "position": [3250, 200]
    },
    {
      "parameters": {
        "conditions": {
          "boolean": [
            {
              "value1": "={{ $json.needs_llm_cleaning }}",
              "value2": true
            }
          ]
        }
      },