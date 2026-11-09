# Editor V1 no Windows e em vídeo vertical (9:16)

Ajuste grátis para a skill **Editor V1**, do **NextPhase**: a skill que pega um vídeo bruto em que você fala
para a câmera e o Claude Code edita sozinho, em estilo documentário, com imagens reais, legendas palavra por
palavra e som em cada corte.

A skill original foi feita para **Mac/Linux (no Windows, só pelo WSL)** e só sai em **16:9 (deitado)**.
Este patch faz duas coisas:

1. **Roda direto no Windows**, sem WSL (Git Bash + Python do Windows).
2. **Sai em vídeo vertical 9:16 (1080×1920)** para Shorts, Reels e TikTok, com a variável `EV1_FORMAT=9x16`.
   Sem ela, a skill continua igual à original (16:9).

> ⚠️ **A skill Editor V1 NÃO está neste repositório.** Ela é do NextPhase e é distribuída por ele, de graça.
> Baixe a original no canal do criador (link na descrição do vídeo
> [My NEW AI Editor Completely FREE](https://www.youtube.com/watch?v=S5Y3Hp2P740)). Aqui tem só o nosso ajuste.

## Como usar (sem saber programar)

1. Instale a Editor V1 seguindo o "SETUP-PROMPT" que vem com ela. No Windows, avise no prompt:
   *"estou no Windows, instala sem WSL"*.
2. Baixe o arquivo `editor-v1-windows-vertical.patch` deste repositório.
3. Abra o Claude Code e cole:

```
Aplique o patch editor-v1-windows-vertical.patch (está na minha pasta Downloads) na skill
~/.claude/skills/editor-v1 com "patch -p1". Antes, faça uma cópia da pasta da skill.
Depois rode o teste de 7 segundos da skill para conferir que funciona.
```

4. Para editar um vídeo vertical, peça: *"edita esse vídeo com a Editor V1 em 9:16"*. O Claude usa
   `EV1_FORMAT=9x16` em todos os passos.

## O que o patch muda

| arquivo | mudança |
|---|---|
| `components/ev1_devices.py`, `components/ev1_more.py` | largura/altura vêm de `EV1_FORMAT` (1920×1080 ou 1080×1920); tamanhos fixos viram proporcionais; CSS extra para legendas e palavras grandes no vertical |
| `scripts/ev1_clip.py`, `scripts/ev1_treat.py`, `scripts/matte.py` | cortes, imagens e recorte no formato escolhido |
| `scripts/env.sh` | acha o Python do venv no Windows (`venv/Scripts/python.exe`) e liga `PYTHONUTF8` |
| `scripts/preview.py`, `scripts/transcribe.py`, `scripts/capture.mjs` | chamam o `npx` do jeito que funciona no Windows |

## Dicas que aprendemos usando no Windows

- **Recorte (você na frente com a palavra gigante atrás):** no render, o vídeo VP9 com transparência saiu com
  fundo **preto**. Solução: extrair o recorte como **um PNG por quadro**
  (`ffmpeg -c:v libvpx-vp9 -i matte.webm m_%03d.png`) e mostrar uma imagem por quadro.
- **Vídeo do iPhone é HDR:** converta para SDR antes, senão o render fica muito lento.
- **Transcrição:** se o `hyperframes transcribe` falhar, o faster-whisper gera o mesmo `words.json`
  (`[{"w": palavra, "s": início, "e": fim}]`).
- Comece com 10 segundos de vídeo, e confira as fontes e as licenças que a skill marca antes de publicar.

## Créditos e licença

- **Editor V1:** © NextPhase. Grátis para usar; o autor pede para não revender nem reempacotar como produto pago.
  Por isso ela não está aqui.
- **Este patch:** MIT (arquivo `LICENSE`). Feito com o Claude Code.
