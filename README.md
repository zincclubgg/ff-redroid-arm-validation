# redroid arm64 + Free Fire validation lab

Repositorio publico minimo e descartavel para validar, num runner ARM64 do
GitHub Actions, se o Free Fire inicia em redroid arm64 nativo.

Nao contem logica de negocio, segredos, nem o codigo do produto. O APK deve
ficar em uma release draft chamada `ff-apk-v1`, como asset `freefire.apk`.

## Como usar

1. Suba `freefire.apk` em uma release draft.
2. Actions -> Validate redroid arm64 + Free Fire -> Run workflow.
3. Use `release_tag=ff-apk-v1`.
4. Use o default `redroid/redroid:12.0.0-latest`, que possui variante `linux/arm64`.
5. Ao terminar, baixe o artefato `validation-results` e veja:
   - `result.txt` -> `FF_ALIVE` ou `FF_DIED`.
   - `grep.log` -> sinais de `seccomp`, `SIGABRT`, `SIGSEGV`, `FATAL`.
   - `screen.png` -> tela capturada.

## O que prova

- `FF_ALIVE` sem crash/seccomp no log: ARM nativo removeu o crash observado no x86.
- `FF_DIED` com crash/seccomp: redroid arm64 nao resolveu a incompatibilidade.

## Riscos conhecidos

- O kernel do runner pode nao ter binder; nesse caso redroid nao boota.
- GPU em software (`gpu_mode=guest`) pode renderizar devagar.
