<div align="center">

<img src="assets/logo-escuro.png" alt="Rota 60" width="120">

# Rota 60

**Protótipo navegável de cadastro e solicitação de corrida** — mobilidade acessível para pessoas idosas.

![status](https://img.shields.io/badge/status-prot%C3%B3tipo-ff7a1a)
![stack](https://img.shields.io/badge/stack-HTML%20%2B%20React%2018%20(runtime)-ff7a1a)
![build](https://img.shields.io/badge/build-nenhum-ff7a1a)
![licença](https://img.shields.io/badge/licen%C3%A7a-n%C3%A3o%20definida-lightgrey)

</div>

---

## Sobre o projeto

O **Rota 60** é um serviço de caronas pensado para o público idoso. Este repositório contém o
**protótipo navegável das telas de cadastro e solicitação de corrida** — 19 telas cobrindo três perfis de usuário e o primeiro fluxo de mobilidade — usado para
validar fluxo, linguagem e acessibilidade antes da implementação do aplicativo real.

O protótipo é **funcional**: os formulários validam campos, navegam entre etapas, alternam tema e
persistem perfis e pedidos de corrida no `localStorage`. Não há backend.

### Decisões de acessibilidade

O sistema de design de base (**Nocturne**) é compacto por natureza. A densidade foi deliberadamente
ampliada para o público-alvo:

| Diretriz | Valor |
| --- | --- |
| Corpo de texto | a partir de **22px** |
| Alvos de toque | **64–76px** |
| Densidade de formulário | **um campo por linha** |
| Leitura em voz alta | `SpeechSynthesis` em `pt-BR` (botão 🔊 em todas as telas) |
| Temas | claro e escuro, com contraste ajustado por tema |
| Senha | código de **4 dígitos** em teclado numérico ampliado |

### Identidade visual

Laranja da marca `#ff7a1a` sobre fundo neutro, tipografia **Inter**. Toda a paleta está centralizada
em variáveis CSS (`--r60-*`) no topo de `Rota60 Cadastro.dc.html` — é o único lugar a editar para
mudar cores.

---

## Como inicializar

### Pré-requisitos

- **Git**
- **Python 3** ou **Node.js** — apenas para servir arquivos estáticos
- **Conexão com a internet** — React, ReactDOM e Babel são carregados do CDN unpkg em tempo de execução

> **Não há `package.json`, `npm install` ou etapa de build.** O projeto é HTML estático + um runtime
> que transpila JSX no navegador.

### Passo a passo

```bash
# 1. Clone o repositório
git clone https://github.com/augustolobo18/Rota-60.git
cd Rota-60

# 2. Suba um servidor HTTP local (escolha uma opção)
python3 -m http.server 5500
#   ou
npx serve -l 5500
```

### 3. Abra no navegador

```
http://localhost:5500/Rota60%20Cadastro.dc.html
```

> O nome do arquivo contém um espaço — use `%20` na URL, ou navegue até ele pelo índice de arquivos
> que o servidor exibe em `http://localhost:5500`.

### ❗ Por que não abrir com duplo clique

Abrir o arquivo direto do disco (`file://`) **não funciona**. O runtime usa `fetch()` para carregar e
transpilar `android-frame.jsx`, e o protocolo `file://` bloqueia essa requisição por política de
origem. O servidor HTTP local é obrigatório.

---

## Usando o protótipo

Ao carregar, a página exibe uma barra de controles acima do frame do dispositivo:

| Controle | Função |
| --- | --- |
| **Passo a passo (3 telas)** | Cadastro do idoso dividido em três etapas |
| **Tela única (rolagem)** | Mesmo cadastro em uma tela contínua — variante para teste A/B |
| **Tema claro / escuro** | Alterna o tema (a escolha persiste no `localStorage`) |
| **Reiniciar** | Limpa o formulário e volta à tela inicial |

Abaixo do frame há um **mapa de telas** numerado que permite pular direto para qualquer uma das 19
telas sem percorrer o fluxo.

### Fluxos implementados

**Idoso** — 5 telas
`Boas-vindas → Identificação → Endereço + acessibilidade → Foto + senha → Conta criada`

**Solicitação de corrida** — 4 telas
`Endereço de saída → Destino → Revisão → Corrida solicitada`

A origem pode ser a casa cadastrada, a localização atual do aparelho (quando o navegador permite) ou um endereço digitado. O destino pode ser digitado ou escolhido entre destinos frequentes de demonstração. Antes de confirmar, o usuário revisa os dois endereços e a necessidade de acessibilidade.

**Responsável** — 5 telas
`Dados pessoais → Convite pelo CPF do idoso (telefone como alternativa) → Aguardando → Aprovação (na tela do idoso) → Vínculo concluído`

**Motorista** — 5 telas
`Dados + selfie → CNH, CRLV e antecedentes → Veículo + 3 fotos + acessibilidade → Dados bancários → Em análise`

### Perfis fictícios

Três perfis de demonstração são semeados no `localStorage` na primeira carga: **Neusa Aparecida**
(idosa), **Carlos Souza** (responsável) e **Josué Ferreira** (motorista). Acessíveis pela tela de
boas-vindas.

---

## Estrutura do repositório

```
Rota-60/
├── Rota60 Cadastro.dc.html   # Protótipo completo: telas, estilos e lógica de estado
├── support.js                # Runtime dc — carrega React/Babel do CDN e monta a página
├── android-frame.jsx         # Frame de dispositivo Android (Material 3), transpilado no navegador
├── assets/
│   ├── logo-claro.png        # Logo para o tema claro
│   └── logo-escuro.png       # Logo para o tema escuro
├── uploads/                  # Arquivos originais da logo (fonte)
└── _ds/nocturne-.../         # Sistema de design Nocturne
    ├── styles.css            # Tokens (cores, tipografia, espaçamento, sombras) + componentes
    ├── _ds_bundle.js         # Bundle de componentes do sistema
    ├── _ds_manifest.json     # Manifesto legível por máquina dos tokens e componentes
    └── readme.md             # Guia de uso do sistema de design
```

### Arquitetura em uma frase

`Rota60 Cadastro.dc.html` declara o markup dentro de `<x-dc>` e a lógica em um `<script type="text/x-dc">`
(uma classe `Component extends DCLogic`). O `support.js` faz o parse desse documento, injeta React 18
via CDN, transpila o JSX auxiliar com Babel standalone e renderiza tudo em `#dc-root`.

---

## Personalização

### Trocar cores

Edite o bloco `:root` no topo de `Rota60 Cadastro.dc.html`. O tema claro sobrescreve os mesmos nomes
em `[data-tema="claro"]`:

```css
:root{
  --r60-accent:#ff7a1a;   /* laranja da marca */
  --r60-bg:#17171a;       /* fundo das telas  */
  --r60-surface:#232326;  /* cartões e campos */
  /* ... */
}
```

### Trocar a logo

Substitua os arquivos em `assets/` mantendo os nomes, ou atualize a variável `--r60-logo` em cada
tema. As logos usam `mix-blend-mode` (`lighten` no escuro, `darken` no claro) para que o fundo da
imagem desapareça sobre a superfície da tela.

---

## Estado atual e limitações

- **Sem backend.** Nenhum dado sai do navegador; a persistência é `localStorage`
  (`rota60.perfis`, `rota60.tema`, `rota60.corridas`).
- **Uploads simulados.** Foto, selfie, CNH, CRLV e fotos do veículo são alternados por toque —
  nenhum arquivo é realmente enviado.
- **Validação básica.** Campos obrigatórios e confirmação de senha; não há validação de CPF,
  CNH ou placa.
- **Dependência de CDN.** Sem internet, a página não renderiza.
- **Solicitação de corrida ainda é simulada.** O pedido é salvo no navegador, mas não há despacho real para motoristas.
- **Sem geocodificação/mapa.** A localização atual pode usar a API de geolocalização do navegador, porém ainda não há conversão de coordenadas para endereço nem traçado de rota.
- **Sem preço/pagamento.** O protótipo não calcula tarifa nem processa pagamento.
- **Sem acompanhamento em tempo real.** Busca de motorista e acompanhamento da viagem ainda são estados simulados.

---

## Roadmap

- [ ] Validação de CPF, CNH e placa (Mercosul)
- [ ] Máscaras de entrada para telefone, CPF e CEP
- [ ] Integração com API de CEP para preenchimento de endereço
- [ ] Backend de autenticação, persistência real e despacho de corridas
- [ ] Integração com geocodificação/mapas e sugestões reais de endereço
- [ ] Integração com provedor de mobilidade para estimativa, solicitação e acompanhamento de corrida
- [ ] Auditoria de acessibilidade com leitores de tela (TalkBack / VoiceOver)
- [ ] Migração do protótipo para React Native ou PWA

---

## Contribuindo

```bash
git checkout main
git pull
git checkout -b feat/minha-alteracao
# ... suas alterações ...
git commit -m "feat: descrição da alteração"
git push origin feat/minha-alteracao
```

Abra o Pull Request com o branch `main` como base. Commits seguem
[Conventional Commits](https://www.conventionalcommits.org/pt-br/).

---

## Licença

Ainda não definida. Até que uma licença seja adicionada, todos os direitos são reservados.

---

<div align="center">

Desenvolvido por **[Augusto Lobo](https://github.com/augustolobo18)**

</div>
