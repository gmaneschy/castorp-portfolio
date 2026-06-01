# Castorp

> Plataforma desktop de conversação com agentes de IA local, síntese de voz e avatares interativos — 100% privativa, com a maioria das funcionalidades rodando offline.

[![Python](https://img.shields.io/badge/Python-3.11-3572A5?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![PySide6](https://img.shields.io/badge/PySide6-GUI-41CD52?style=flat-square&logo=qt&logoColor=white)](https://doc.qt.io/qtforpython)
[![llama.cpp](https://img.shields.io/badge/llama.cpp-GGUF-8B4513?style=flat-square)](https://github.com/ggerganov/llama.cpp)
[![License](https://img.shields.io/badge/licença-privada-red?style=flat-square)]()

---

## O que é o Castorp?

O Castorp é uma plataforma desktop de conversação com agentes de IA construída do zero em Python/PySide6. Toda a inferência de LLM, síntese de voz e renderização de avatar acontece localmente — sem nenhum dado enviado a serviços externos. APIs externas são usadas apenas para funcionalidades opcionais como clima, busca e geração de imagens.

---

## Demo

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6a34eb57-b857-4631-a206-4504e4e122ea" />

Assista ao vídeo completo no Vimeo: https://vimeo.com/1197456048?fl=pl&fe=sh

---

## Funcionalidades

### LLM local
- Inferência via **llama.cpp**, compatível com a maioria dos modelos GGUF
- **Memória persistente** auto-incrementável pelo próprio modelo e editável pelo usuário
- Detecção automática de formato de chat e leitura de metadados do modelo

### Síntese de voz (TTS)
- Três engines com interface unificada: **Supertonic 3**, **Kokoro** e **StyleTTS2**
- **Clonagem de voz** via StyleTTS2
- Pipeline **RVC** aplicável a qualquer voz sintetizada

### Visão e OCR
- Análise de imagens enviadas pelo usuário via **SmolVLM2**
- Captura automática de screenshot da tela para enriquecimento de contexto do LLM

### Avatares interativos
- **2D** — imagens estáticas e GIFs com animações e expressões
- **Live2D** — hit areas, overlay, animações, expressões e suporte a vozes por avatar
- **VRM 3D** — lipsync sincronizado ao áudio TTS, auto-blink, carregamento de arquivos `.glb`; environment map via arquivos `.hdr` e `.exr`; parâmetros de iluminação de ambiente configuráveis

> Runtime dos avatares Live2D e VRM executado via JavaScript embarcado (Three.js + Cubism SDK); restante do backend em Python puro.

### APIs externas integradas
WorldTime · OpenWeather · Wikipedia · TMDB · Google Books · Jokes · **Cloudflare Workers AI** (geração de imagens com SDXL, Lightning e Dreamshaper)

### Interface totalmente customizável
- Inclusão de imagens e GIFs personalizados
- Modos animados de borda e transparência por widget
- Fullscreen, desacoplamento de input e overlay de avatares Live2D/VRM
- Sistema de temas configurável

---

## Arquitetura

```
Interface PySide6
    └── LLM local (llama.cpp · modelos GGUF · SmolVLM2)
            └── TTS Engine (Supertonic 3 · Kokoro · StyleTTS2 · RVC)
                    └── Motor de avatar (2D · Live2D · VRM)
                            └── APIs externas (dados · geração de imagens)
```

<img width="2125" height="2875" alt="castorp_arquitetura_geral" src="https://github.com/user-attachments/assets/5ae307a5-2bb9-4e42-ba89-9bcd5dd54261" />

---

## Diagramas de código

Gerados automaticamente com [code2flow](https://github.com/scottrogowski/code2flow) a partir do código-fonte.

### AgentWorker — fluxo principal do agente
Ponto de entrada da plataforma. Orquestra eventos, OCR, memória, TTS e gatilhos autônomos.

<img width="1640" height="827" alt="agent_worker" src="https://github.com/user-attachments/assets/289da369-fe8d-42eb-a696-9080126c4d7c" />

### TTS Engine — síntese de voz
`SupertonicTTSEngine`, `StyleTTS2Engine` e `KokoroTTSEngine` com interface interna unificada.

<img width="1282" height="1552" alt="tts_engine" src="https://github.com/user-attachments/assets/7852961a-97f9-4f0c-86ec-0d3448c79897" />

### LLM Loader — carregamento do modelo
Pipeline de carregamento GGUF: detecção de formato de chat, leitura de metadados e validação de templates.

<img width="1429" height="360" alt="llm_loader" src="https://github.com/user-attachments/assets/89b02d8d-8819-40d6-8869-2e47c1b6fbbb" />

### LLM Inference — inferência e streaming
Classe `AgentLenaInference`: geração de texto, síntese em streaming e carregamento dinâmico do engine TTS.

<img width="974" height="784" alt="llm_inference" src="https://github.com/user-attachments/assets/5174f979-3dd5-40c1-8e7e-a7f683d26e09" />

---

## Stack técnico

| Camada | Tecnologias |
|---|---|
| Interface | PySide6 |
| LLM | llama.cpp (GGUF), SmolVLM2 |
| Voz | Supertonic 3, Kokoro, StyleTTS2, RVC |
| Avatares | Live2D Cubism SDK, VRM, Three.js |
| Visão | SmolVLM2 (OCR + análise de imagem) |
| APIs externas | WorldTime, OpenWeather, TMDB, Wikipedia, Google Books, Cloudflare Workers AI |
| Backend | Python 3.11 |
