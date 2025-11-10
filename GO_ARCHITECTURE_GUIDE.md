## Princípios de Código (Rob Pike)

### 1. Simplicidade
- Não adicione complexidade desnecessária
- Se algo pode ser simples, mantenha simples
- Evite abstrações prematuras

### 2. Clareza sobre Cleverness
- Código deve ser óbvio e fácil de entender
- Prefira nomes descritivos a comentários explicativos
- A clareza é mais importante que a elegância

### 3. Composição sobre Herança
- Use structs simples e composição
- Interfaces pequenas e focadas
- Evite hierarquias complexas

### 4. Erros são Valores
- Trate erros explicitamente
- Não esconda erros
- Use `if err != nil` sem vergonha

### 5. Concorrência com Channels
- Não se comunique compartilhando memórias; compartilhe memórias se comunicando.
- Use channels para coordenação
- Goroutines devem ser óbvias no código

### 6. Menos é Mais
- Menos código = menos bugs
- Delete código que não adiciona valor
- Prefira stdlib a dependências externas quando possível

## 7. Estrutura de Projeto e Package-Oriented Design (POD)

O Go adota uma filosofia de design centrada em **pacotes (packages)** — chamada de **Package-Oriented Design (POD)**.  
Essa abordagem, defendida por **Rob Pike**, **Dave Cheney** e pela própria equipe da linguagem, propõe que o código deve ser organizado **em torno do comportamento e propósito** dos pacotes, e não por camadas genéricas (como MVC, domain/service/repository etc).

> “Good Go programs are built from packages that have clear boundaries, simple APIs, and minimal coupling.”  
> — *Dave Cheney, Go and Package Focused Design*

### 🧠 Princípios do Package-Oriented Design

- **Pacotes são a unidade fundamental de design.**  
  O foco é em *como os pacotes se comunicam e se isolam*, não em classes ou camadas.

- **Cada pacote deve ter um propósito claro e autocontido.**  
  Ele deve resolver um problema específico e ser pequeno o suficiente para ser entendido rapidamente.

- **Dependências fluem para dentro.**  
  Um pacote pode depender de outros, mas apenas em uma direção clara e previsível.  
  Pacotes de alto nível (como `cmd`) dependem de pacotes de domínio (`internal`), e nunca o contrário.

- **Interfaces são definidas onde são usadas.**  
  Essa é uma prática essencial no Go e reduz o acoplamento entre pacotes.

- **Visibilidade é uma ferramenta de design.**  
  O sistema de visibilidade do Go (nomes maiúsculos e minúsculos) e o uso do diretório `internal/` reforçam o encapsulamento.

---

## 🗂️ Estrutura de Projeto Idiomática no Go

A comunidade Go adota um layout amplamente aceito que reflete esses princípios de POD:

### `/cmd`
**Pontos de entrada da aplicação.**

Cada subdiretório em `/cmd` representa um **binário executável** (por exemplo: `api`, `worker`, `cli`, `consumer`).

**Responsabilidades:**
- Orquestrar a inicialização da aplicação.
- Carregar configurações e dependências.
- Conectar o “mundo externo” (HTTP, fila, CLI) com o domínio interno.
- **Não contém lógica de negócio.**

> Pense em `/cmd` como o ponto de integração e orquestração das funcionalidades.

---

### `/internal`
**Domínio e lógica específica do projeto.**

Contém o código que **não deve ser reutilizado fora deste projeto**.  
É onde vivem as **regras de negócio, modelos e fluxos centrais**.

**Responsabilidades:**
- Implementar as funcionalidades principais e regras específicas.
- Manter isolamento das dependências externas.
- Fornecer APIs internas limpas e bem definidas.

> Tudo em `internal` é privado ao repositório, e o compilador do Go reforça essa regra.

---

### `/pkg`
**Bibliotecas genéricas e reutilizáveis.**

Código que pode ser **importado e usado em outros projetos**, sem depender do domínio específico.

**Responsabilidades:**
- Oferecer componentes genéricos e independentes.
- Ser estável, testado e sem dependências internas.
- Facilitar o reuso e o compartilhamento entre times.

> Exemplo: loggers, wrappers, utilitários, middlewares, clientes HTTP genéricos.

---

## ⚙️ Relação entre os diretórios

| Diretório | Tipo de Código | Responsabilidade | Reutilização |
|------------|----------------|------------------|---------------|
| `/cmd` | Pontos de entrada (binários) | Orquestração e integração | Não reutilizável |
| `/internal` | Domínio e regras de negócio | Implementação e isolamento | Privado ao projeto |
| `/pkg` | Bibliotecas genéricas | Reuso e compartilhamento | Público e reutilizável |

---

## REGRAS DE OURO
    ✅ internal/ → internal/   (PERMITIDO)
    ✅ internal/ → pkg/        (PERMITIDO)
    ✅ pkg/ → pkg/             (PERMITIDO)
    ❌ pkg/ → internal/        (PROIBIDO) 
    ❌ Dependências circulares (PROIBIDO)

## 📚 Referências

- [Dave Cheney — Go and Package Focused Design (Gopher Academy Blog, 2016)](https://blog.gopheracademy.com/advent-2016/go-and-package-focused-design/)
- [Dave Cheney — The Zen of Go (2020)](https://dave.cheney.net/2020/02/23/the-zen-of-go)
- [Dave Cheney — SOLID Go Design (2016)](https://dave.cheney.net/2016/08/20/solid-go-design)
- [Rob Pike — Go at Google: Language Design in the Service of Software Engineering (2012)](https://go.dev/talks/2012/splash.article)
- [Go Wiki — Project Layout Recommendations](https://github.com/golang-standards/project-layout)

---

> **Resumo:**  
> O Go defende simplicidade, isolamento e clareza.  
> O Package-Oriented Design é o reflexo direto disso — pequenos pacotes com responsabilidades claras, separados por propósito, e uma estrutura (`/cmd`, `/internal`, `/pkg`) que guia a legibilidade, a manutenção e a escalabilidade do código.
