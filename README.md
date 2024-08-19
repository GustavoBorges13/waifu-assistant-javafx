# Waifu Assistente
Waifu Assistente é uma IA interativa que conversa periodicamente com você e responde a perguntas feitas via console, microfone (usando Whisper) ou no chat da Twitch.

[![Latest Stable Version](https://img.shields.io/badge/version-v0.0.1-blue)](https://github.com/GustavoBorges13/waifu-assistant-javafx/releases)
[![GitHub last commit](https://img.shields.io/github/last-commit/GustavoBorges13/waifu-assistant-javafx)](https://github.com/GustavoBorges13/waifu-assistant-javafx/commits/main)
<!---[![Build Status](https://app.travis-ci.com/GustavoBorges13/RunBlocker.svg?branch=main)](https://app.travis-ci.com/GustavoBorges13/RunBlocker)-->

```diff
- Versão mais recente: Em desenvolvimento!
+ Autor: Gustavo Silva
# Projeto solo
```

## Arquitetura e Fluxo de Dados
A arquitetura do Waifu Assistente segue um modelo modular e orientado a eventos, onde cada componente tem uma responsabilidade clara. O fluxo de dados e as comunicações entre os módulos são gerenciados de forma eficiente para garantir uma interação suave e responsiva. Abaixo está uma visão geral da estrutura e do funcionamento do código:
```
Imagem em breve !
```

## Inspiração
Este projeto foi inspirado em várias ferramentas e projetos da comunidade, que forneceram uma base sólida e ideias inovadoras para o desenvolvimento do Waifu Assistente. Alguns dos principais projetos e tecnologias que influenciaram este trabalho incluem:

- OpenAI GPT Models: A arquitetura GPT-3.5 serviu como base para a construção do chatbot.
- ElevenLabs: Inspirou a integração de tecnologia avançada de conversão de texto em fala.
- VTube Studio: Provedor de API para controle de personagens virtuais.
- Whisper by OpenAI: Influência fundamental para a conversão de fala em texto no projeto.
- Outros projetos de assistentes virtuais e VTubers, que pavimentaram o caminho para a combinação de interatividade em tempo real com modelos de IA.

## Resumo do Funcionamento (Em desenvolvimento)
O projeto funciona da seguinte forma: Na primeira vez que o programa é aberto, ele solicita as credenciais de API, que são armazenadas diretamente nas variáveis de ambiente do Windows para garantir a segurança. Nas próximas execuções, o programa buscará essas chaves automaticamente para prosseguir com a inicialização. Após o início, o usuário terá um painel interativo onde poderá escolher entre enviar comandos via texto para o chatbot, usar o chat da Twitch ou o próprio microfone.

### Modos de Interação
- Console: O texto inserido é enviado ao chatbot ChatAnywhere, que gera uma resposta baseada na configuração escolhida pelo usuário, como a personalidade. A resposta é convertida em áudio via ElevenLabs, gerando um arquivo MP3. Esse áudio é então convertido para WAV e enviado à classe de conexão com o VTube Studio, que executa comandos de movimentação da personagem e reprodução do áudio em threads separadas.

- Chat da Twitch: O programa captura comandos no chat que contêm "!TTS" e envia o texto ao ChatAnywhere. A resposta segue o mesmo fluxo de conversão em áudio e interação com o VTube Studio.

- Microfone: A fala é convertida em texto usando Speech-to-Text e enviada ao ChatAnywhere, seguindo o fluxo de interação descrito acima.

### Futuras Implementações
Menu para controlar algumas funcionalidades da personagem no VTube Studio (pouco viável).
Banco de dados para armazenar o histórico de mensagens e áudios gerados, que servirão para aprendizado de máquina, consulta de estatísticas, e logs de erros.
Botões úteis para ligar, desligar, enviar, deletar, etc.

### Ambiente de Desenvolvimento
- IDE: Eclipse IDE 2024;
- Linguagem: Java;
- Interface Gráfica: JavaFX com SceneBuilder;
- Compatível com: Windows 11 64 bits.

### Instalação
Ainda não disponível.

### APIs utilizadas
#### 1) Para o Chatbot, utilizado ChatAnyWhere - GPT_API_free
- Repositorio: [https://github.com/chatanywhere/GPT_API_free](https://github.com/chatanywhere/GPT_API_free);
- Status Credito API: [https://api.chatanywhere.tech](https://api.chatanywhere.tech);
- Status Disponibilidade API: [https://status.chatanywhere.tech](https://status.chatanywhere.tech);
- Base_url: [https://api.chatanywhere.tech/v1/chat/completions](https://api.chatanywhere.tech/v1/chat/completions);
- model: gpt-3.5-turbo (essa que uso atualmente para varias requisições) | gpt-4o-mini | gpt-4 (3 chamadas por dia).

#### 2) Para text-to-speech, utilizado ElevenLabs API
- API site: [https://elevenlabs.io/](https://elevenlabs.io/);
- Documentação API: [https://elevenlabs.io/docs/introduction](https://elevenlabs.io/docs/introduction);
- Vozes utilizadas para portugues-br: Gabby e Michele (preferida);
- Vozes utilizadas para english-US: Rebecca - British mediations;
- model: eleven_multilingual_v2 (a melhor), eleven_turbo_v2, eleven_monolingual_v1;
- json object config: voice_settings -> stability: 0.0, similarity_boost: 1.0, output_format: mp3_44100_128;

Observação: o áudio é convertido de MP3 para WAV para compatibilidade com a API do VTube Studio, usando javax.sound.sampled. A ElevenLabs não suporta diretamente o formato WAV, apenas MP3, PCM (S16LE) e μ-law (mulaw).

#### 3) Para a personagem Waifu, utilizado VTube Studio API
- Configuração API: Habilite a API nas configurações do VTube Studio (porta 8001) em General Settings. Após a primeira requisição, aceite a solicitação no aplicativo e armazene o token gerado para futuras execuções conforme a imagem abaixo
  
  ![image](https://github.com/user-attachments/assets/04a2424f-9e5f-4325-abaf-11f6acf65c3f)

  ![image](https://github.com/user-attachments/assets/a614de5c-3f7f-4bcf-9b89-94107a8702b4)

- Configuração Saída do Microfone: em General Settings -> Microphone Settings, precisa marcar a caixa "Use microphone" e selecionar a saída para CABLE Output (VB-Audio Virtual Cable) conforme a imagem abaixo.
  
  ![image](https://github.com/user-attachments/assets/d2864f93-71d8-46e5-ac7c-aef4f418f1ba)

- Configuração do movimento da boca (lip-sync): em Model Settings -> Mouth Open, alterar a Input para VoiceVolume para mexer a boca de acordo com o audio enviado ao Virtual Cable conforme a imagem abaixo.

  ![image](https://github.com/user-attachments/assets/fb74a338-3b42-4834-849d-3382675888de)
 
- Documentação da API: [https://github.com/DenchiSoft/VTubeStudio](https://github.com/DenchiSoft/VTubeStudio)

#### 4) Para reconhecimento de fala (Whisper) - Em processo de aprendizagem
- Repositório: [https://github.com/openai/whisper](https://github.com/openai/whisper);
- Função: Captura de áudio do microfone e conversão em texto para uso no chatbot.
  
### Programas dependentes
VB-CABLE Virtual Audio Device: Disponível em VB-Audio. Usado para emular um dispositivo de áudio virtual para redirecionar o som do programa para um dispositivo alvo. Abaixo está um exemplo de código para capturar dispositivos de áudio disponíveis:
```diff
        import javax.sound.sampled.*;
         ...
        // Listando todos os mixers (dispositivos de saída)
        Mixer.Info[] mixers = AudioSystem.getMixerInfo();
        System.out.println("Dispositivos de áudio disponíveis:");
        for (int i = 0; i < mixers.length; i++) {
            System.out.println(i + ": " + mixers[i].getName() + " - " + mixers[i].getDescription());
        }

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
Neste exemplo, o índice 15 corresponde ao nosso dispositivo alvo para reprodução de áudio.
