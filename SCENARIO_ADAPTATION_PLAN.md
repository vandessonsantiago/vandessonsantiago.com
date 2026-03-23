# 🔍 ANÁLISE DO PROBLEMA: HeroGate não adapta o restante do site

## O Problema Identificado

### Cenário Atual (PROBLEMA):
```
Visitor clica em "Está em risco"
                    ↓
HeroGate mostra: "Você tem ~14 dias"
                    ↓
Mas então aparece:
  - State: "Essa análise é indicada para quem: [genérico pra ambos]"
  - Alert: "Seu tempo é limitado" [fala de timeline 14 dias - OK, mas genérico]
  - HowItWorks: "Entenda como atuamos" [processo genérico, não específico]
  - ProofSocial: "34 clientes" [não especifica se pré ou pós]
  - FAQ: "Quanto tempo tenho?" [responde 14d OU 48-72h, confuso]
```

**Resultado**: Site não faz sentido. Visitor escolhe "em risco" mas lê sobre apreensão também.

---

## A Solução Proposta

### 1. Guardar o Cenário Escolhido (localStorage)

```javascript
// HeroGate.astro script
function showHero(scenario) {
  localStorage.setItem('heroScenario', scenario);  // ← NOVO
  document.getElementById('siteContent').style.display = 'block';
  // ... resto do código
}

// Em cada componente:
const scenario = localStorage.getItem('heroScenario') || 'pre';
```

### 2. Adaptar cada componente ao scenario

#### State.astro
```
SE scenario === 'pre':
  "Essa análise é indicada para quem:"
  - Está com parcelas em atraso
  - Recebeu notificação do banco
  ✓ Está EM RISCO de apreensão
  - Dívida crescendo (não relevante pós)

SE scenario === 'post':
  "Essa análise é indicada para quem:"
  ✗ (Você JÁ foi apreendido, é tarde para prevenção)
  ✓ Teve veículo apreendido
  ✓ Quer recuperar documentação
  ✓ Quer negociar com banco APÓS apreensão
```

#### Alert.astro
```
SE scenario === 'pre':
  "Seu tempo é limitado" → Timeline 14 dias (MANTÉM)
  
SE scenario === 'post':
  "Você está em apreensão" → Timeline 48-72h (não 14 dias)
  Checklist diferente: "O que você precisa fazer AGORA (apreendido)"
```

#### HowItWorks.astro
```
SE scenario === 'pre':
  Passo 1: "Você envia documentos"
  Passo 2: "Identificamos PRAZO E OPÇÕES DE NEGOCIAÇÃO"
  Passo 3: "Plano de ação para PARAR apreensão"
  
SE scenario === 'post':
  Passo 1: "Você envia documentos + prova de apreensão"
  Passo 2: "Identificamos opções LEGAIS para recuperação"
  Passo 3: "Plano para NEGOCIAR com banco APÓS apreensão"
```

#### ProofSocial.astro
```
SE scenario === 'pre':
  Stats: "65% conseguiram renegociar ANTES de apreensão"
  Depoimento: João S. - "Consegui prorrogar prazo em 1 semana"
  
SE scenario === 'post':
  Stats: "45% conseguiram recuperar documentação após apreensão"
  Depoimento: Different client - "Consegui negociar 30 dias para buscar carro"
```

#### FAQ.astro
```
SE scenario === 'pre':
  Q1: "Como funciona a avaliação?" → resposta genérica (OK)
  Q2: "Quanto tempo tenho para agir?" → "14 dias antes de apreensão"
  Q3: "E se já foi apreendido?" → "Se acontecer, a avaliação ainda ajuda"
  
SE scenario === 'post':
  Q1: "Como funciona a avaliação?" → resposta genérica (OK)
  Q2: "Quanto tempo tenho para agir?" → "48-72 horas após apreensão"
  Q3: "Quais são minhas opções agora?" → "Negociação, documentação, legal"
```

---

## Implementação Técnica

### Padrão para cada componente:

```astro
---
// Em cada componente:
const scenario = Astro.props.scenario || 'pre';
// OU usar localStorage via script inline
---

{scenario === 'pre' ? (
  <div>... conteúdo PRÉ ...</div>
) : (
  <div>... conteúdo PÓS ...</div>
)}
```

### Alternativa: Layout.astro como wrapper

```astro
---
import DefaultLayout from "../layouts/DefaultLayout.astro";
export async function getStaticPaths() {
  // Gerar ambas as versões se for SSG
}
---

<DefaultLayout scenario={scenario}>
  {/* Componentes recebem scenario como prop */}
</DefaultLayout>
```

---

## Impacto da Solução

### Antes (PROBLEMA):
- ❌ Visitor escolhe cenário, resto do site ignora
- ❌ Mensagens genéricas/confusas
- ❌ Impacto estimado: -30% relevância

### Depois (SOLUÇÃO):
- ✅ Cada componente se adapta ao cenário
- ✅ Narrativa coerente pré-apreensão (14 dias de urgência)
- ✅ Narrativa coerente pós-apreensão (48-72h de urgência)
- ✅ Impacto estimado: +50% relevância (visitor sente que site é "para ele")

---

## Checklist de Implementação

- [ ] HeroGate: Guardar `scenario` em localStorage
- [ ] State: Condicionar bullets para cada scenario
- [ ] Alert: Timeline diferente (14d vs 48-72h) + checklist específico
- [ ] HowItWorks: Prazos e outcomes específicos para cada scenario
- [ ] ProofSocial: Stats/depoimento específicos
- [ ] FAQ: Respostas ajustadas para cada scenario
- [ ] DefaultLayout: Detectar scenario ao carregar e inicializar componentes
- [ ] Testar: Clicar em cada botão e verificar se TODA página adapta

---

## Questões a Responder

1. **Você quer que a página mude dinamicamente ao clicar?** (localStorage + JS)
   - Sim: Use localStorage (solução dinâmica)
   - Não: Use query params ou versões separadas

2. **Você quer depoimentos diferentes para cada scenario?**
   - Sim: Preciso de depoimento pós-apreensão adicional
   - Não: Manter mesmo depoimento (João S.) para ambos

3. **O site inteiro muda, ou volta ao topo ao clicar?**
   - Muda in-place: Use localStorage + script global
   - Volta ao topo: Recarreg a página com scenario

**Recomendação**: localStorage + script global = melhor UX
