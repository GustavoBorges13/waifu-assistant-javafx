# Waifu Assistente
Waifu Assistente, uma IA que conversa periodicamente com você conforme realiza perguntas para ela tanto via console, microfone (whisper) ou na twitch.

[![Latest Stable Version](https://img.shields.io/badge/version-v0.0.0.1-blue)](https://github.com/GustavoBorges13/waifu-assistant-javafx/releases)
[![GitHub last commit](https://img.shields.io/github/last-commit/GustavoBorges13/waifu-assistant-javafx)](https://github.com/GustavoBorges13/waifu-assistant-javafx/commits/main)
<!---[![Build Status](https://app.travis-ci.com/GustavoBorges13/RunBlocker.svg?branch=main)](https://app.travis-ci.com/GustavoBorges13/RunBlocker)-->

```diff
- Em desenvolvimento!
+ Autor: Gustavo Silva
# Projeto solo
```

## Resumo funcionamento (Em desenvolvimento)
O projeto funciona da seguinte forma, será uma interface gráfica onde aberto pela primeira vez solicitará dados de API nois quais tais credenciais serão armazenadas diretamente no ambiente de variaveis do windows para manter a segurança do mesmo.
O programa na proxima vez que for aberto irá procurar por essas chaves para prosseguir na inicialização.
Após aberto o usuário terá um painel interativo para escolher se deseja enviar comandos via texto para o chatboot, ou usar o chat da twitch ou o próprio microfone.
1. Caso seja usado o console, o texto sera enviado para o chatboot chatanywhere que irá gerar uma resposta com base na configuração que o usuario escolher tais como a personalidade, o chatboot retorna a mensagem, a mensagem será utilizada para conversão em audio via elevenlabs que irá gerar um audio em .mp3, posteriormente o audio será convertido para .wav e acionado a classe de conexão com o VTubeStudio para executar comandos de movimentação da personagem e reprodução de audio que serão realizados em threads.
2. Caso seja usado o chat via twitch, será realizado a captura do texto que contem o comando "!TTS" e enviado para o chatboot chatanywhere que irá gerar uma resposta com base na configuração que o usuario escolher tais como a personalidade, o chatboot retorna a mensagem, a mensagem será utilizada para conversão em audio via elevenlabs que irá gerar um audio em .mp3, posteriormente o audio será convertido para .wav e acionado a classe de conexão com o VTubeStudio para executar comandos de movimentação da personagem e reprodução de audio que serão realizados em threads.
3. Caso seja usado o microfone, será realizado o speech-to-text para converto a fala em texto e enviado para o chatboot chatanywhere que irá gerar uma resposta com base na configuração que o usuario escolher tais como a personalidade, o chatboot retorna a mensagem, a mensagem será utilizada para conversão em audio via elevenlabs que irá gerar um audio em .mp3, posteriormente o audio será convertido para .wav e acionado a classe de conexão com o VTubeStudio para executar comandos de movimentação da personagem e reprodução de audio que serão realizados em threads.

Sobre outras implementações que eu penso no futuro:
- Menu para controlar algumas funcionalidades da personagem no VTubeStudio (pouco viavel);
- Banco de dados para gerar um historico de mensagens e audios gerados que servirão para deeplearning, consultar quantas requisições foram realizadas ao longo do dia, verificar erros (serve como log), separar o nome de quem enviou o comando na twitch da resposta, assim a gente consegue ver quantas pessoas fora o desenvolvedor utilizou o mesmo para consultas de estatisticas (bem provavel que eu faça isso);
- Botões uteis para ligar, desligar, enviar, deletar e etc.

## Ambiente Virtual
Utilizado Eclipse IDE 2024 na linguagem JAVA.
Interface gráfica com JavaFX (SDK) e scenebuilder para construção.
Compatível com Windows 11 64bits.

## Instalação (Não disponivel no momento)


## APIs utilizadas
### Para o Chatbot, utilizado ChatAnyWhere - GPT_API_free
- Repositorio: [https://github.com/chatanywhere/GPT_API_free](https://github.com/chatanywhere/GPT_API_free);
- Status Credito API: [https://api.chatanywhere.tech](https://api.chatanywhere.tech);
- Status Disponibilidade API: [https://status.chatanywhere.tech](https://status.chatanywhere.tech);
- Base_url: [https://api.chatanywhere.tech/v1/chat/completions](https://api.chatanywhere.tech/v1/chat/completions);
- model: gpt-3.5-turbo (essa que uso atualmente para varias requisições) | gpt-4o-mini | gpt-4 (3 chamadas por dia).

### Para text-to-speech, utilizado ElevenLabs API
- API site: [https://elevenlabs.io/](https://elevenlabs.io/);
- Documentação API: [https://elevenlabs.io/docs/introduction](https://elevenlabs.io/docs/introduction);
- Vozes utilizadas para portugues-br: Gabby e Michele (preferida);
- Vozes utilizadas para english-US: Rebecca - British mediations;
- model: eleven_multilingual_v2 (a melhor), eleven_turbo_v2, eleven_monolingual_v1;
- json object config: voice_settings -> stability: 0.0, similarity_boost: 1.0, output_format: mp3_44100_128;
- *Observação: usei javax.sound.sampled para redirecionar o som para outra entrada de dispositivo mas para isso tem que converter .mp3 para .wav para manter compatibilidade. Infelizmente elevenlabs nao oferece suporte para .wav, apenas para MP3, PCM (S16LE) e μ-law (mulaw).

### Para a personagem Waifu, utilizado VTube Studio API
- Precisa habilitar nas configurações do aplicativo o uso de API (habilitar plugins) na porta 8001.
- Apos realizar a primeira requisição, precisa aceitar no aplicativo a request, e capturar o token para armazenar posteriormente pra nao ficar precisando mais aceitar toda hora.
- Feito isso, o uso dessa API, permite enviar dados para o VTube Studio, acionar parametros, comandos e muito mais conforme na documentação [https://github.com/DenchiSoft/VTubeStudio](https://github.com/DenchiSoft/VTubeStudio)

### Para reconhecimento de fala (Whisper) - Em processo de aprendizagem
- Estou tentando ver a possibilidade de integrar com o projeto atual esse repositorio [https://github.com/openai/whisper](https://github.com/openai/whisper);
- Isso serveria para capturar meu audio (microfone) e converter em texto que seria utilizado posteriormente no chatbot para gerar uma resposta.
  
## Programas dependentes
VB-CABLE Virtual Audio Device disponivel em[ https://vb-audio.com/Cable/](https://vb-audio.com/Cable/). Utilizado para emular um dispositivo virtual em output e input. Onde no programa será convertido audios .mp3 para .wav e redirecionados para o dispositivo virtual alvo.
Abaixo está um exemplo do codigo usado para capturar os dispositivos na maquina:
```diff
        import javax.sound.sampled.*;
         ...
        // Listando todos os mixers (dispositivos de saída)
        Mixer.Info[] mixers = AudioSystem.getMixerInfo();
        System.out.println("Dispositivos de áudio disponíveis:");
        for (int i = 0; i < mixers.length; i++) {
            System.out.println(i + ": " + mixers[i].getName() + " - " + mixers[i].getDescription());
        }

        // Selecione o índice do dispositivo que você quer usar
        int deviceIndex = 15; // Altere este índice para o dispositivo desejado

```
Exemplo de retorno:
```diff
Dispositivos de áudio disponíveis:
0: Port Speakers (Realtek(R) Audio) - Port Mixer
1: Port LG HDR WFHD (NVIDIA High Defini - Port Mixer
2: Port Speakers (fifine Microphone) - Port Mixer
3: Port Realtek Digital Output (Realtek - Port Mixer
4: Port 24G2E1 (NVIDIA High Definition  - Port Mixer
5: Port CABLE Input (VB-Audio Virtual C - Port Mixer
6: Port Microphone (fifine Microphone) - Port Mixer
7: Port CABLE Output (VB-Audio Virtual  - Port Mixer
8: Port Microphone (Realtek(R) Audio) - Port Mixer
9: Primary Sound Driver - Direct Audio Device: DirectSound Playback
10: Speakers (Realtek(R) Audio) - Direct Audio Device: DirectSound Playback
11: LG HDR WFHD (NVIDIA High Definition Audio) - Direct Audio Device: DirectSound Playback
12: Speakers (fifine Microphone) - Direct Audio Device: DirectSound Playback
13: Realtek Digital Output (Realtek(R) Audio) - Direct Audio Device: DirectSound Playback
14: 24G2E1 (NVIDIA High Definition Audio) - Direct Audio Device: DirectSound Playback
15: CABLE Input (VB-Audio Virtual Cable) - Direct Audio Device: DirectSound Playback
16: Primary Sound Capture Driver - Direct Audio Device: DirectSound Capture
17: Microphone (fifine Microphone) - Direct Audio Device: DirectSound Capture
18: CABLE Output (VB-Audio Virtual Cable) - Direct Audio Device: DirectSound Capture
19: Microphone (Realtek(R) Audio) - Direct Audio Device: DirectSound Capture
```
Ali no caso nosso é o indice 15 nosso dispositivo alvo para tocar os audios.
