# Log file of the release tests of SPARC4


## 20240521

Olá
Terminei de fazer os testes com a nova versão do ACS.
Todo o instrumento foi utilizado no modo real
De modo geral, tudo ocorreu bem. Consegui fazer diversas aquisições intercalando configurações. 
Algumas das configurações foram:
- Modos fotométrico e polarimétrico
- Modos síncrono e assíncrono
- Diferentes combinações de posição da waveplate
- Abas de object, flat, zero, bias
- Modo EM e convencional com e sem FT
- L2 com polarizer
- L4 com depolarizer
- Frames e ciclos
Além disso, chequei o header de algumas imagens 'chaves' e aparentemente tudo OK
Os arquivos de log do ACS estão sendo gerados como esperado (isto deve ajudar com o problema do canal 1)

O único problema que apareceu durante os testes foi a mensagem de erro no S4GUI:
5/21/2024 2:29:57 PM  ERROR MSG = CHANNEL ERROR < 15>: Initialization failed. S4GUI:HandleObservationSync.vi state 252.
Esta mensagem apareceu umas 3 vezes durante o teste, no momento em que soltava uma aquisição.
Eu corrigia isso mandando um comando ABORT e um START novamente.
Me parece que isto está relacionado com um problema de comunicação e não com os ACSs em si.
Dessa forma, acredito que esteja tudo certo para soltar esta release para a próxima missão.

## 20240611

Teste de release

Instrumento testado totalmente no modo real.

Baseline utilizada
ACS v1.48.4.
S4GUI v.2024.06.05 exe.
ICS 20240513 projeto.

Procedimentos
- Aquisicao de 2 s com o shutter closed.
- 1 aquisição para as abas OBJECT, DARK, ZERO, etc.
- Aquisição com frame-transfer on e off.
- Serie sincrona e assincrona (5 ciclos de 5 frames, fotometria).
- Modos convencional e EM.
- L2 + polarizer.
- L4 + depolarizer.
- Serie polarimetria (2 ciclos com 16 posicoes de L4).

Problemas
- Channel error <12> init failed. Precisei dar abort.
- Channel error <15> init failed. Precisei dar abort.
- Problema de comunicacao entre s4gui e guider, mesmo no projeto.
- Acquisition timeout.
- Ao passar para L4+depolarizer, a lamina nao terminou o posicionamento inicial. Um init individual resolveu. No ICS > (WPROT ACTIVE TIMEOUT 0.000 NONE -1).

ACS
- date-obs com a data em utc.
- nome da imagem com a data 20240611.
- sem erros nos arquivos de log.
- os headers refletem as mudancas de configuracao.
- nivel de contagens OK.

## 20240621

Teste de release
Instrumento testado totalmente no modo real.

Baseline utilizada
ACS v1.49.0
S4GUI v.2024.06.05 exe.
ICS 20240513 projeto.

Procedimentos
- Aquisicao de 2 s com o shutter closed.
- 1 aquisição para as abas OBJECT, DARK, ZERO, etc.
- Aquisição com frame-transfer on e off.
- Serie sincrona e assincrona (5 ciclos de 5 frames, fotometria).
- Modos convencional e EM.
- L2 + polarizer.
- L4 + depolarizer.
- Serie polarimetria (2 ciclos com 16 posicoes de L4).

Problemas
- ACS1 travou em ACTIVE. Precisei reiniciar o ACS. Ocorreu com a troca do modo FT
- CHANNEL ERROR <9> Init Failed.
- ERROR: acquisition timeout
- Depolarizer travou. Mensagem ICS: CALW ACTIVE BUSY 288 DEPOLARIZER 5. Precisei dar INIT

Mudanças no ACS
- O sufixo do nome da imagem foi adicionado à mensagem de status
- Refatoração do gerenciamento de erros
- Adição da rotina para encerrar o ambiente python ao parar o ACS

Obs
- A rede parecia bem instável, precisei abortar diversas aquisições porque um canal não recebia o EXPOSE

## 20240819

*Teste de release*

Instrumento testado totalmente no modo real.

*Baseline utilizada*
- ACS v1.51.1 exe.
- S4GUI v.2024.08.08 exe.
- ICS 20240807 exe.

*Procedimentos*
- 1 aquisição para as abas OBJECT, DARK, ZERO, etc.
- Aquisição com frame-transfer on e off.
- Serie sincrona e assincrona (5 ciclos de 5 frames, fotometria).
- Modos convencional e EM.
- L2 + polarizer.
- L4 + depolarizer.
- Serie polarimetria (2 ciclos com 16 posicoes de L4).

*Problemas*
- Acquisition timeout state 324 ocorreu diversas vezes ao longo do teste
- Channel error <4> suffix setup failed na aba DARK. Este erro aconteceu consistentemente. Aquisição não realizada
- Error some Chanel initialization failed (state 252)
- Start de uma série assíncrona travou diversas vezes

*S4ACS v1.51.1*
- Uso do projeto Communication Manager

  ## 20250411

*Teste de release*

sparc4 no modo real.

*Baseline*
- ACS v1.52.3 exe.
- S4GUI v1.0.4 exe.
- ICS 20250206 exe.

*Configurações utilizadas para a aquisição de imagens*
- Abas OBJECT, DARK, ZERO, etc.
- Serie sincrona e assincrona (5 ciclos de 5 frames, fotometria).
- L2 + polarizer.
- L4 + depolarizer (problema 1).
- Serie polarimetria (2 ciclos com 16 posicoes de L4).
- 1 ciclo + 4 posições de L4 + STOP posição 3.
- 1 ciclo + 4 posições de L4 + STOP posição 4 + nova série.

*Problemas*
1. S4GUI: State 302 - moving waveplate timeout. 

