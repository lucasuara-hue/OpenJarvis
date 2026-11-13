# Jarvis em Português — guia de instalação e uso

Um assistente de voz que ouve e fala em português, usando o framework OpenJarvis com o cérebro na nuvem (Claude, da Anthropic).

- Ouvir (fala para texto): faster-whisper, roda local, sem chave.
- Falar (texto para fala): Kokoro, voz PT-BR local, sem chave.
- Pensar: modelo Claude na nuvem — precisa de 1 chave só (ANTHROPIC_API_KEY).

IMPORTANTE: o modo voz usa microfone e alto-falante, então ele roda na sua máquina (Mac, Linux ou Windows), não num servidor.

## 1. Pré-requisitos
- macOS, Linux ou Windows (via WSL2). Windows nativo também funciona.
- Python 3.10 ou mais novo.
- Microfone e alto-falante funcionando.
- Uma chave da API da Anthropic: https://console.anthropic.com/ (API Keys).

## 2. Instalar o OpenJarvis
macOS / Linux / WSL2:
  curl -fsSL https://open-jarvis.github.io/OpenJarvis/install.sh | bash
Windows nativo (PowerShell):
  irm https://open-jarvis.github.io/OpenJarvis/install.ps1 | iex
Depois confirme:
  jarvis doctor

## 3. Instalar as dependências de voz
  uv pip install 'OpenJarvis[speech]'
No Linux, se der erro de áudio, instale antes: sudo apt install portaudio19-dev

## 4. Configurar o Jarvis em português
  mkdir -p ~/.openjarvis
  cp configs/openjarvis/examples/jarvis-pt-voz.toml ~/.openjarvis/config.toml
  cp configs/openjarvis/prompts/personas/jarvis-pt.md ~/.openjarvis/jarvis-pt.md

## 5. Colocar a chave da API
  export ANTHROPIC_API_KEY="sk-ant-...sua-chave-aqui..."
Dica: adicione essa linha ao seu ~/.bashrc ou ~/.zshrc para não repetir.

## 6. Rodar — falar com o Jarvis
  jarvis chat --voice
- Aperte Enter (sem digitar nada) para falar; ele grava até você fazer silêncio.
- Ou digite uma mensagem e aperte Enter.
- A resposta é falada em voz alta e também aparece escrita.
- Ctrl+C encerra.
Teste rápido por texto:
  jarvis ask "Quem é você e o que você faz?"

## 7. Personalizar
- Trocar a voz (campo voice_id): pm_alex (masc), pf_dora (fem), pm_santa.
- Respostas melhores: default_model = "claude-sonnet-4-6".
- Transcrição mais precisa: model = "medium" ou "large-v3"; com GPU, device = "cuda" e compute_type = "float16".
- Voz na nuvem premium: tts_backend = "elevenlabs" (com ELEVENLABS_API_KEY) ou "openai_tts" com voice_id = "nova" (com OPENAI_API_KEY).
- Cérebro 100% local: [engine] default = "ollama"; [intelligence] provider = "local", default_model = "qwen3.5:9b".

## 8. Problemas comuns
- "No speech-to-text backend available": uv pip install 'OpenJarvis[speech]'.
- "No TTS backend available": uv pip install kokoro.
- Erro de microfone: instale o áudio do sistema e cheque permissões.
- Responde em inglês: confira language = "pt" e o system_prompt_path.
- Chave não encontrada: exporte ANTHROPIC_API_KEY no mesmo terminal.
- Primeira fala demora: normal, o Kokoro baixa o modelo de voz na primeira vez.
