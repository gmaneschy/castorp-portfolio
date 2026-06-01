# Castorp

> Plataforma desktop de conversação com agentes de IA local, síntese de voz e avatares interativos.

[![Python](https://img.shields.io/badge/Python-3.11-3572A5?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![PySide6](https://img.shields.io/badge/PySide6-GUI-41CD52?style=flat-square&logo=qt&logoColor=white)](https://doc.qt.io/qtforpython)
[![llama.cpp](https://img.shields.io/badge/llama.cpp-GGUF-8B4513?style=flat-square)](https://github.com/ggerganov/llama.cpp)
[![License](https://img.shields.io/badge/licença-privada-red?style=flat-square)]()

---

## O que é o Castorp?

Castorp é uma plataforma desktop para conversação com LLMs rodando **100% localmente**, combinada com síntese de voz em tempo real e avatares interativos em 2D, Live2D e VRM. A plataforma aceita a maioria dos modelos no formato GGUF e oferece controle completo sobre voz, aparência e comportamento do agente — sem dependência de nuvem para inferência.

---

## Demo

> *(Insira aqui um GIF ou screenshot da interface em funcionamento)*
>
> [![Demo do Castorp](assets/screenshot-main.png)](https://linkedin.com/SEU_LINK_AQUI)
>
> 📺 [Assistir vídeo completo no LinkedIn](https://linkedin.com/SEU_LINK_AQUI)

---

## Funcionalidades

- **LLM local via llama.cpp** — suporte à maioria dos modelos GGUF; memória persistente auto-incrementável pelo modelo e editável pelo usuário
- **Síntese de voz (TTS)** — três engines: Supertonic 2, Kokoro e StyleTTS2; suporte a clonagem de voz e pipeline RVC aplicável a qualquer voz
- **Avatares interativos** — 2D (imagens e GIFs), Live2D (hit areas, overlay, animações) e VRM 3D (lipsync, auto-blink, iluminação com HDR/EXR, arquivos GLB)
- **Visão e OCR** — análise de imagens enviadas pelo usuário e capturas de tela automáticas via SmolVLM
- **APIs externas integradas** — WorldTime, Wikipedia, Weather, TMDB, Google Books, Jokes e geração de imagens via Cloudflare Worker (SDXL, Lightning, DreamShaper)
- **Interface totalmente customizável** — temas, transparência de widgets, fullscreen, modos de borda animados, desacoplamento de input e overlay de avatares

---

## Arquitetura

A plataforma é organizada em cinco camadas independentes:

```
Interface PySide6
    └── LLM local (llama.cpp · modelos GGUF)
            └── TTS Engine (Supertonic 2 · Kokoro · StyleTTS2 · RVC)
                    └── Motor de avatar (2D · Live2D · VRM)
                            └── APIs externas (dados · geração de imagens)
```

![Diagrama de arquitetura](assets/architecture.svg)

> Runtime dos avatares Live2D e VRM executado via JavaScript embarcado; restante do backend em Python.

---

## Diagramas de código

Os diagramas abaixo foram gerados automaticamente com [code2flow](https://github.com/scottrogowski/code2flow) a partir do código-fonte.

### AgentWorker — fluxo principal do agente

Ponto de entrada da plataforma. Orquestra eventos, OCR, memória, TTS e gatilhos autônomos.

![AgentWorker](assets/agent_worker.png)

### TTS Engine — síntese de voz

Três engines de TTS com interfaces unificadas. `SupertonicTTSEngine`, `StyleTTS2Engine` e `KokoroTTSEngine` compartilham o mesmo contrato de API interna.

![TTS Engine](assets/tts_engine.png)

### LLM Loader — carregamento do modelo

Pipeline de carregamento de modelos GGUF: detecção de formato de chat, leitura de metadados e validação de templates.

![LLM Loader](<img width="1429" height="360" alt="llm_loader" src="https://github.com/user-attachments/assets/89b02d8d-8819-40d6-8869-2e47c1b6fbbb" />)

### LLM Inference — inferência e streaming

Classe `AgentLenaInference` responsável por geração de texto, síntese em streaming e carregamento dinâmico do engine de TTS.

![LLM Inference](<img width="974" height="784" alt="llm_inference" src="https://github.com/user-attachments/assets/5174f979-3dd5-40c1-8e7e-a7f683d26e09" />)


---

## Stack técnico

| Camada | Tecnologias |
|---|---|
| Interface | PySide6 |
| LLM | llama.cpp, modelos GGUF, SmolVLM |
| Voz | Supertonic 2, Kokoro, StyleTTS2, RVC |
| Avatares | Live2D Cubism SDK, VRM, Three.js (runtime JS) |
| APIs | WorldTime, OpenWeather, TMDB, Wikipedia, Google Books, Cloudflare Workers AI |
| Backend | Python 3.11 |
