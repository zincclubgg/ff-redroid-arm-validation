# redroid arm64 + Free Fire — validation lab

Repositório **público mínimo e descartável** com o único objetivo de validar, num
runner ARM64 gratuito do GitHub Actions, se o **Free Fire roda em redroid arm64 nativo**
(passa do anti-cheat que crashava no x86 com `PR_SET_SECCOMP`).

**Não contém** lógica de negócio, segredos, nem o código do produto — só o teste.

## Como usar

1. Crie um repositório **público** no GitHub e suba estes arquivos.
2. **Actions → Validate redroid arm64 + Free Fire → Run workflow**.
3. Informe `apk_url` = URL de download direto do APK universal do Free Fire
   (qualquer host que o runner consiga baixar via `curl -L`).
4. Ao terminar, baixe o artefato **validation-results** e veja:
   - `result.txt` → `FF_ALIVE` (passou) ou `FF_DIED`.
   - `grep.log` → presença de `seccomp`/`SIGABRT`/`SIGSEGV` (se vazio = bom sinal).
   - `screen.png` → o que estava na tela.
   - O **Summary** do run já mostra o veredito.

## O que prova

- **`FF_ALIVE` + sem `PR_SET_SECCOMP`** → ARM nativo resolve o crash; o modelo redroid
  é viável. Seguimos pro host ARM persistente (Hetzner/Oracle).
- **`FF_DIED` com seccomp/abort** → o problema não era só a tradução; investigar.

## Riscos conhecidos
- O kernel do runner pode não ter o módulo `binder` → redroid não boota (o log do passo
  "Load binder" mostra). Se ocorrer, adaptamos.
- GPU em software (`gpu_mode=guest`) → o FF pode renderizar devagar; mas o teste do
  anti-cheat (seccomp) acontece independ? da renderização.
