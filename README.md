# 🤖 URA Cognitiva com Reconhecimento de Voz (Speech-to-Text)

Este repositório contém o Projeto Final da disciplina "Audio Recognition" do **MBA em Data Science & AI da FIAP (10DTSR)**.

O objetivo foi desenvolver um protótipo de **URA (Unidade de Resposta Audível)**, ou *IVR (Interactive Voice Response)*, para a fintech "QuantumFinance". O sistema é capaz de gerar suas próprias falas (Text-to-Speech) e compreender os comandos de voz do usuário (Speech-to-Text) para direcionar o atendimento.

## ▶️ Vídeo de Demonstração

Assista ao funcionamento do projeto em ação:

**[https://youtu.be/4Y_TNF__iaE](https://youtu.be/4Y_TNF__iaE)**

---

## 🏛️ Arquitetura da Solução e Fluxo de Execução

O projeto simula um atendimento telefônico automatizado. O fluxo de interação é o seguinte:

1.  **Geração de Voz (TTS):** Primeiro, o sistema usa a biblioteca `gTTS` (Google Text-to-Speech) para converter todas as frases do bot (boas-vindas, confirmações, erro) em arquivos `.mp3`.
2.  **Início do Atendimento:** O bot inicia a interação tocando o áudio de `boas_vindas.mp3`, apresentando as opções ao usuário (Saldo, Compra Internacional, Atendente, Sair).
3.  **Entrada do Usuário:** O usuário fornece um comando de voz (simulado através do **upload de um arquivo de áudio** no Google Colab).
4.  **Reconhecimento de Fala (STT):** O sistema carrega o arquivo de áudio do usuário e utiliza a biblioteca `SpeechRecognition` (com a API do Google) para transcrever a fala para texto.
5.  **Processamento (NLU Simples):** O texto transcrito é analisado em busca de palavras-chave ("saldo", "compra", "atendente", "sair").
6.  **Resposta do Bot:**
    * Se uma palavra-chave é encontrada, o bot toca o áudio de confirmação correspondente (`confirmacao_saldo.mp3`, etc.).
    * Se nenhuma palavra-chave é identificada, o bot toca o áudio de `erro.mp3` e pede ao usuário que tente novamente (retornando ao Passo 3).
    * Se o usuário diz "sair", o bot toca `despedida.mp3` e encerra a simulação.

Este é o fluxo visual do pipeline:


    A[Início: Geração de Áudios (TTS)] --> B(Bot toca "boas_vindas.mp3")
    B --> C(Usuário fornece arquivo de áudio)
    C --> D[Sistema Transcreve (STT com Google API)]
    D --> E{Texto do Usuário}
    E --> F[Processamento de Palavras-Chave]
    
    F -- "saldo" --> G(Bot toca "confirmacao_saldo.mp3")
    F -- "compra" --> H(Bot toca "confirmacao_compra.mp3")
    F -- "atendente" --> I(Bot toca "confirmacao_atendente.mp3")
    
    F -- "sair" --> L(Bot toca "despedida.mp3")
    F -- Nenhuma chave --> K(Bot toca "erro.mp3")
    
    K --> B
    G --> L
    H --> L
    I --> L
    L --> M[Fim do Atendimento]
