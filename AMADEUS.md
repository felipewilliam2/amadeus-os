# Amadeus OS

Fork pessoal do [TECHO5](https://github.com/HuskerMinion/techo5) para o Echo Show 5 (1ª geração,
`checkers`) da casa do Felipe. É um projeto de hobby, sem prazo, para diversão e aprendizado.

O TECHO5 já resolve a parte difícil: kernel, áudio, tela, palavra de ativação, integração com o
Home Assistant e atualização A/B com volta automática. O Amadeus OS acrescenta personalização por cima.

## Objetivos

- Personalizar o máximo possível a tela e o comportamento do aparelho.
- Mostrar o Hermes e os bots (Amadeus, Ravi, Íris, Otto, Luna) trabalhando.
- Manter o acesso fácil ao dashboard do Home Assistant: relógio como tela inicial, dashboard ao
  deslizar o dedo a partir da borda esquerda.

## Organização dos branches

| Branch | Para quê |
|---|---|
| `main` | Cópia fiel do TECHO5. Só recebe **Sync fork**; nunca recebe commits nossos. |
| `amadeus` | Branch padrão. Tudo o que é do Amadeus OS. Recebe o `main` a cada versão nova do TECHO5. |
| `feat/*` a partir de `main` | Funcionalidades que podem ser oferecidas ao TECHO5 em pull request, sem nada pessoal. |

Regra: mexer o mínimo possível nos arquivos do TECHO5 e concentrar o que é nosso em arquivos
e pastas próprios. Assim, trazer versões novas continua fácil.

## Roadmap

| Fase | Entrega |
|---|---|
| 0. Fundação | Este fork, o processo de build e um build de teste instalado no slot reserva, com volta automática comprovada |
| 1. Protótipo da tela | A cara do Amadeus OS no navegador, sem tocar no aparelho |
| 2. Bots ao vivo | O aparelho mostra o que cada bot do Hermes está fazendo; o Home Assistant continua como plano B |
| 3. Personagem reativo | O Amadeus da cena reage à voz e aos alertas |
| 4. Polimento | Botão de mudo como "não perturbe", câmera desligada por padrão, temas |
| Depois | Oferecer ao TECHO5 o que fizer sentido para todos |

## Regras do repositório

- Repositório **público**. Nunca colocar aqui chaves, tokens, IPs da casa, `home-assistant.key`,
  backups de partições ou a imagem de boot do aparelho (`inputs/`, `backups/` e `build/` já são
  ignorados pelo git).
- Releases assinadas exigem uma chave própria (ver `docs/building.md`, seção *Releases*). Enquanto
  não houver uma, os testes no aparelho usam o daemon por `mount --bind` (some no reboot) ou o slot
  reserva.

## Build do daemon

```
cd echod
go test ./...
GOOS=linux GOARCH=arm GOARM=7 CGO_ENABLED=0 go build -trimpath -ldflags '-s -w' -o ../bin/echod-arm ./cmd/echod
```

Observação: em máquinas arm64, `TestFloatModelsUnchanged` (`internal/lib/tflite`) falha, provavelmente porque o
compilador funde multiplicação e soma (FMA) e os bits do ponto flutuante mudam. Na arquitetura do
aparelho (`GOARCH=arm`) o teste passa, então a falha não afeta o daemon do aparelho.

Licença: MIT, a mesma do TECHO5 (ver `LICENSE` e `NOTICE`).
