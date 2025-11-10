# 🧠 Filosofia e Padrões de Engenharia

> “Simplicidade é uma escolha de engenharia.”

## 1. Princípios Fundamentais

- **Menos é mais.**  
  Faça menos, mas faça bem.  
  Cada linha de código, dependência ou abstração deve ter um motivo claro para existir.

- **Clareza sobre esperteza.**  
  Código legível vence código inteligente.  
  Se for preciso explicar demais, está errado.

- **Comportamento previsível.**  
  Prefira defaults seguros e resultados consistentes.

- **Dependências mínimas.**  
  Evite bibliotecas desnecessárias e frameworks pesados.

- **Software simples é software robusto.**  
  O código mais fácil de manter é aquele que você entende em 30 segundos.

---

## 2. Padrões de Código Go

- Siga **`gofmt`**, **`golangci-lint`** e o guia oficial [Effective Go](https://go.dev/doc/effective_go).
- Prefira **composição a herança**.
- **Interfaces pequenas** e definidas no ponto de uso.
- **Erros explícitos:**
```go
  if err != nil {
      return err
  }
```

- **Sem frameworks genéricos.**  
  Use bibliotecas pequenas e específicas.
- **Evite abstrações desnecessárias.**  
  Código direto é melhor que “helpers mágicos”.
- **Documente com exemplos, não com ensaios.**

---

## 3. Estrutura de Projeto

Estrutura padrão recomendada:

```bash
/cmd        -> binários (entradas principais)
/internal   -> lógica de negócio e implementações
/pkg        -> bibliotecas públicas e reutilizáveis
```

Regras:
- Tudo que é específico do projeto fica em `/internal`.
- `/pkg` só para código genérico, pensado para reuso real.
- Cada pacote deve ter **responsabilidade única** e API mínima.
- Prefira **funções pequenas e previsíveis** a “camadas arquiteturais”.

---

## 4. Filosofia de Arquitetura

- **Evite overengineering.**
- **Evite frameworks internos.**
- Cada componente deve **fazer uma coisa só, e bem.**
- **Evite camadas desnecessárias** — se a função serve, não crie um pacote.
- **Não abstraia prematuramente.**  
  Primeiro, faça funcionar. Depois, simplifique.
- **Prefira ferramentas simples:**  
  `make`, `docker`, `systemd`, scripts shell.
- **Desconfie da automação excessiva.**  
  Scripts > pipelines complexos.

---

## 5. Revisão e Cultura de Código

- Pull Requests curtos e com **propósito único**.
- Cada PR deve responder claramente: **“por que isso existe?”**
- O reviewer deve perguntar:
  > “Esse código é simples o suficiente para ser entendido rapidamente?”
- Evite refactors sem propósito.
- Decisões técnicas importantes devem ser registradas em **ADRs simples**.

### 🧩 Em resumo
#### Um ADR responde três perguntas:
```
- O que foi decidido?
- Por que decidimos isso?
- Quando e em que contexto?
``` 

Exemplo de ADR leve:
```
# ADR 001 - Uso de Redis como cache local
Data: 2025-11-10
Decisão: Adotado Redis apenas para cache de sessão.
Motivo: Evitar dependência extra em serviços não críticos.
```

---

## 6. Comunicação e Documentação

- Documente **comportamentos**, não intenções.
- Código deve **explicar a si mesmo**.
- **Comentários só quando o código não puder ser mais simples.**
- Escreva *README*s curtos, claros e com exemplos reais.
- **Comentários seguem o padrão Go.**  
  Use `godoc` para funções, structs e pacotes exportados.  
  Sempre descreva o comportamento de forma objetiva e em uma linha.

---

## 7. Mantra da Engenharia

> “Escreva programas que façam uma coisa só, e bem.”  
> “Desconfie da complexidade.”  
> “Códigos são para pessoas, não para máquinas.”  
> “Simplicidade é força.”

---

## 8. Referências

- [Suckless Philosophy](https://suckless.org/philosophy/)
- [Go Proverbs – Rob Pike](https://go-proverbs.github.io/)
- [Effective Go](https://go.dev/doc/effective_go)
