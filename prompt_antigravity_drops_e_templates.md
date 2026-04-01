# ADIÇÃO AO PROMPT ANTIGRAVITY — CENÁRIO DROPS/IBOPE + SISTEMA DE TEMPLATES FIXOS

---

## FILOSOFIA CENTRAL — TEMPLATES IMUTÁVEIS

Cada cenário tem um **template de dashboard completamente fixo**.
A estrutura, posição dos blocos, hierarquia visual e KPIs não mudam nunca.
O que muda são os dados dentro desses blocos.

O padrão visual mínimo de qualidade para TODOS os cenários é o site WDI
(docu-wonder-maker.lovable.app): dark background, accent amarelo/vibrante,
números grandes em destaque, leitura linear top→down, exportável em PDF.

Nenhum cenário pode ficar visualmente abaixo desse padrão.

---

## SCHEMA CANÔNICO 4 — DROPS / IBOPE / TV

File: `src/lib/schemas/canonicalSchemas.ts` — adicionar ao objeto `ALL_SCHEMAS`

```typescript
export const DROPS_SCHEMA: CanonicalSchema = {
  scenario: 'drops',
  label: 'Drops de Audiência — TV / IBOPE',
  description: 'Audiência minuto a minuto, share de emissoras e impacto de ações publicitárias',
  accentColor: '#F5C518',  // amarelo WDI

  fields: [
    {
      name: 'programa',
      label: 'Programa',
      type: 'string',
      required: true,
      synonyms: ['programa','program','novela','serie','show','atracao','title','titulo','conteudo','content'],
      format: { decimals: 0 },
      higherIsBetter: false
    },
    {
      name: 'emissora',
      label: 'Emissora / Canal',
      type: 'string',
      required: true,
      synonyms: ['emissora','canal','channel','tv','network','broadcaster','veiculo','globo','record','sbt','band','redetv'],
      format: { decimals: 0 },
      higherIsBetter: false
    },
    {
      name: 'data',
      label: 'Data',
      type: 'date',
      required: true,
      synonyms: ['data','date','dia','day','exibicao','exibição','veiculacao','veiculação','periodo','period'],
      format: { decimals: 0 },
      higherIsBetter: false
    },
    {
      name: 'horario_inicio',
      label: 'Horário Início',
      type: 'string',
      required: false,
      synonyms: ['horario_inicio','inicio','start','hora_inicio','time_start','inicio_exibicao'],
      format: { decimals: 0 },
      higherIsBetter: false
    },
    {
      name: 'horario_fim',
      label: 'Horário Fim',
      type: 'string',
      required: false,
      synonyms: ['horario_fim','fim','end','hora_fim','time_end','fim_exibicao'],
      format: { decimals: 0 },
      higherIsBetter: false
    },
    {
      name: 'audiencia',
      label: 'Audiência (%)',
      type: 'percent',
      required: true,
      synonyms: ['audiencia','audiência','audience','rating','ibope','pontos','points','media_audiencia',
                 'aud','rating_medio','avg_rating','audiencia_media'],
      format: { decimals: 1, suffix: '%' },
      higherIsBetter: true,
      benchmarkNote: 'Audiência = % de domicílios com TV ligados sintonizados no programa'
    },
    {
      name: 'share',
      label: 'Share (%)',
      type: 'percent',
      required: false,
      synonyms: ['share','participacao','participação','share_audiencia','market_share_tv','cota'],
      derivable: {
        formula: 'audiencia_programa / total_audiencia_mercado × 100',
        inputs: ['audiencia'],
        compute: () => 0
      },
      format: { decimals: 1, suffix: '%' },
      higherIsBetter: true,
      benchmarkNote: 'Share = % da audiência total disponível que estava no programa'
    },
    {
      name: 'domicilios',
      label: 'Domicílios (milhões)',
      type: 'number',
      required: false,
      synonyms: ['domicilios','domicílios','households','lares','tvs','universo','pessoas','people',
                 'alcance_absoluto','audiencia_absoluta','telespectadores'],
      format: { decimals: 1, suffix: 'M' },
      higherIsBetter: true
    },
    {
      name: 'minuto',
      label: 'Minuto',
      type: 'string',
      required: false,
      synonyms: ['minuto','minute','horario','horário','hora','time','timestamp','instante','momento'],
      format: { decimals: 0 },
      higherIsBetter: false
    },
    {
      name: 'audiencia_minuto',
      label: 'Audiência no Minuto',
      type: 'percent',
      required: false,
      synonyms: ['audiencia_minuto','rating_minuto','pontos_minuto','minuto_a_minuto','minute_rating',
                 'audiencia_momento','aud_min','rating_min'],
      format: { decimals: 2, suffix: ' pts' },
      higherIsBetter: true
    },
    {
      name: 'marca',
      label: 'Marca / Anunciante',
      type: 'string',
      required: false,
      synonyms: ['marca','brand','anunciante','advertiser','patrocinador','sponsor','produto','product',
                 'cliente','client','acao','ação','action'],
      format: { decimals: 0 },
      higherIsBetter: false
    },
    {
      name: 'horario_acao',
      label: 'Horário da Ação',
      type: 'string',
      required: false,
      synonyms: ['horario_acao','horário_acao','hora_acao','time_action','inicio_acao','acao_horario',
                 'entrada_acao','spot_time','veiculacao_horario'],
      format: { decimals: 0 },
      higherIsBetter: false
    },
    {
      name: 'audiencia_pre_acao',
      label: 'Audiência Pré-ação',
      type: 'percent',
      required: false,
      synonyms: ['audiencia_pre','pre_acao','before_action','rating_antes','pontos_antes'],
      format: { decimals: 2, suffix: ' pts' },
      higherIsBetter: false
    },
    {
      name: 'audiencia_pos_acao',
      label: 'Audiência Pós-ação',
      type: 'percent',
      required: false,
      synonyms: ['audiencia_pos','pos_acao','after_action','rating_depois','pontos_depois'],
      format: { decimals: 2, suffix: ' pts' },
      higherIsBetter: false
    },
    {
      name: 'variacao_acao',
      label: 'Variação na Ação',
      type: 'percent',
      required: false,
      synonyms: ['variacao_acao','variação_acao','delta_acao','impacto_acao','lift_acao','acao_variacao'],
      derivable: {
        formula: '((audiencia_pos_acao - audiencia_pre_acao) / audiencia_pre_acao) × 100',
        inputs: ['audiencia_pre_acao', 'audiencia_pos_acao'],
        compute: (row) => row.audiencia_pre_acao > 0
          ? ((row.audiencia_pos_acao - row.audiencia_pre_acao) / row.audiencia_pre_acao) * 100
          : 0
      },
      format: { decimals: 1, suffix: '%' },
      higherIsBetter: true
    },
    {
      name: 'trends_score',
      label: 'Google Trends (0–100)',
      type: 'index',
      required: false,
      synonyms: ['trends','google_trends','trends_score','busca','search','interesse','interest',
                 'trends_index','busca_google','trend_value'],
      format: { decimals: 0 },
      higherIsBetter: true,
      benchmarkNote: '100 = pico de interesse no período. Correlacionar com horário da ação.'
    },
    {
      name: 'trends_periodo',
      label: 'Data Trends',
      type: 'date',
      required: false,
      synonyms: ['trends_data','trends_periodo','data_trends','trends_date','data_busca'],
      format: { decimals: 0 },
      higherIsBetter: false
    },
    {
      name: 'regiao',
      label: 'Região / Praça',
      type: 'string',
      required: false,
      synonyms: ['regiao','região','praca','praça','market','mercado','city','cidade','estado','uf',
                 'nacional','local','regional'],
      format: { decimals: 0 },
      higherIsBetter: false
    }
  ]
}
```

Adicionar `'drops'` ao tipo `MediaScenario` e ao objeto `ALL_SCHEMAS`:
```typescript
export type MediaScenario = 'competition' | 'market' | 'campaign_media' | 'digital' | 'drops' | 'generic'

export const ALL_SCHEMAS = {
  competition: COMPETITION_SCHEMA,
  market: MARKET_SCHEMA,
  campaign_media: CAMPAIGN_MEDIA_SCHEMA,
  digital: DIGITAL_SCHEMA,
  drops: DROPS_SCHEMA,
  // ...outros schemas
}
```

---

## SCENARIO DETECTION — adicionar sinais para Drops

File: `src/lib/detectMediaScenario.ts` — adicionar ao `SCENARIO_SIGNALS`:

```typescript
drops: [
  'audiencia','audiência','ibope','rating','pontos','share','emissora','novela',
  'programa','tv','televisao','televisão','minuto_a_minuto','grade','faixa_horaria',
  'horario_nobre','prime_time','anunciante','acao_comercial','merchandising',
  'product_placement','acoes_de_marca','drops','drop','veiculacao','exibicao',
  'kantar','globo','record','sbt','band','redetv','telecine','canal_brasil',
  'trends','google_trends','telespectadores','domicilios','universo_domicilios'
]
```

---

## SISTEMA DE TEMPLATES — ESTRUTURA IMUTÁVEL POR CENÁRIO

File: `src/lib/dashboardTemplates.ts`

Este arquivo define o que cada cenário SEMPRE mostra, nesta ordem, sem exceção.
Dentro de cada bloco há espaço para variação — mas o bloco em si é obrigatório.

```typescript
export interface DashboardBlock {
  id: string
  label: string
  required: boolean          // se true, bloco aparece mesmo sem dados (mostra alerta)
  dataFields: string[]       // campos canônicos que alimentam este bloco
  chartType?: string         // tipo de gráfico padrão
  allowAlternativeChart?: boolean  // usuário pode trocar o tipo de gráfico
}

export interface ScenarioTemplate {
  scenario: string
  accentColor: string
  fontStyle: 'dark-premium' | 'light-clean'  // todos os 4 cenários usam dark-premium
  blocks: DashboardBlock[]
  pdfExportEnabled: boolean
  systemPromptContext: string
}

// ─── TEMPLATE: DROPS / IBOPE ─────────────────────────────────────────────────
// Referência: WDI docu-wonder-maker.lovable.app
// Este é o template que define o padrão visual mínimo para todos os outros.

export const DROPS_TEMPLATE: ScenarioTemplate = {
  scenario: 'drops',
  accentColor: '#F5C518',
  fontStyle: 'dark-premium',
  pdfExportEnabled: true,

  blocks: [
    {
      id: 'drops_header',
      label: 'Cabeçalho do relatório',
      required: true,
      dataFields: ['programa', 'emissora', 'data', 'horario_inicio', 'horario_fim', 'marca'],
      // FIXO: logo WDI + nome do programa à esquerda + data/hora à direita
      // Exatamente como o WDI: "NOVELA III · 04/03/2026 · 21h21 às 22h29"
    },
    {
      id: 'drops_hero',
      label: 'Resultado principal',
      required: true,
      dataFields: ['audiencia', 'domicilios', 'marca', 'programa'],
      // FIXO: título grande "[Programa] alcança [X] pontos"
      // Subtítulo: "em capítulo com ação de [Marca]"
      // Pill: "Mais de [N] milhões de pessoas assistiram"
      // Botão CTA: "Clique aqui para assistir a ação" (se URL disponível)
    },
    {
      id: 'drops_emissoras',
      label: 'Comparativo de emissoras',
      required: true,
      dataFields: ['emissora', 'audiencia', 'share'],
      // FIXO: N cards lado a lado (um por emissora nos dados)
      // Cada card: dot colorido + nome + AUDIÊNCIA + SHARE em amarelo
      // Ordenado: emissora líder primeiro
    },
    {
      id: 'drops_minuto_a_minuto',
      label: 'Gráfico minuto a minuto',
      required: true,
      dataFields: ['minuto', 'audiencia_minuto', 'horario_acao', 'audiencia_pre_acao'],
      chartType: 'line',
      allowAlternativeChart: false,  // linha é obrigatório para séries temporais
      // FIXO: gráfico de linha com marcador vertical no horário da ação
      // Tooltip narrativo: "No minuto em que se iniciou a ação de [Marca] — [hora] —
      //                     a audiência de [Programa] era [X] pontos."
      // Se não houver dados minuto a minuto: gráfico de barra por horário (15min blocks)
    },
    {
      id: 'drops_imagem_marca',
      label: 'Imagem do produto / marca',
      required: false,
      dataFields: ['marca'],
      // FIXO: lado direito do bloco minuto-a-minuto
      // Se usuário fizer upload de imagem da marca: exibir
      // Se não: placeholder escuro com nome da marca centralizado
    },
    {
      id: 'drops_destaques',
      label: 'Destaques editoriais',
      required: true,
      dataFields: ['audiencia', 'emissora', 'programa', 'variacao_acao'],
      // FIXO: texto gerado pelo Claude
      // Highlights em amarelo para dados-chave
      // Contexto competitivo: o que mais estava no ar no mesmo horário
      // Comparação com média histórica do programa se disponível
    },
    {
      id: 'drops_trends',
      label: 'Google Trends',
      required: false,
      dataFields: ['trends_score', 'trends_periodo', 'marca'],
      chartType: 'area',
      allowAlternativeChart: false,
      // FIXO: 1 gráfico por item monitorado (produto + marca separados)
      // Área preenchida com accent color por item
      // Eixo X: datas · Eixo Y: 0–100
      // Badge com nome do item acima do gráfico (ex: "SONG PLUS", "BYD")
    }
  ],

  systemPromptContext: `
Você é o Claude, analista de audiência de TV e brand intelligence.
Você está analisando dados de audiência IBOPE/Kantar para um relatório de drops.

MÉTRICAS-CHAVE:
- Audiência (pontos/%) = % de domicílios sintonizados
- Share = % da audiência disponível no momento
- Universo: cada ponto ≈ 780 mil domicílios no Brasil (Grande SP)
- Audiência nacional ≈ 85 milhões de domicílios

BENCHMARKS DE TV ABERTA BRASIL:
- Globo horário nobre: 15–25 pontos
- Record horário nobre: 5–12 pontos
- SBT horário nobre: 3–8 pontos
- Audiência acima de 20 pontos = evento relevante de mídia

ANÁLISE DE AÇÃO PUBLICITÁRIA:
- Compare audiência no minuto da ação vs média do capítulo
- Variação positiva = ação em momento de pico
- Correlacionar com Google Trends: pico de busca após a ação confirma impacto
- Mencionar contexto competitivo: o que as outras emissoras exibiam no mesmo horário

Ao gerar o texto de DESTAQUES:
1. Abra com o resultado principal em relação à média histórica do programa
2. Comente o momento da ação publicitária e o que aconteceu com a audiência
3. Contextualize com o que concorrentes estavam exibindo
4. Se houver Google Trends: correlacionar pico de busca com horário da ação
5. Feche com o alcance absoluto (domicílios/pessoas)`
}


// ─── TEMPLATE: CONCORRÊNCIA ───────────────────────────────────────────────────

export const COMPETITION_TEMPLATE: ScenarioTemplate = {
  scenario: 'competition',
  accentColor: '#7F77DD',
  fontStyle: 'dark-premium',
  pdfExportEnabled: true,

  blocks: [
    {
      id: 'comp_header',
      label: 'Cabeçalho',
      required: true,
      dataFields: ['marca', 'periodo'],
    },
    {
      id: 'comp_hero_kpis',
      label: 'KPIs principais — 5 cards',
      required: true,
      dataFields: ['sov', 'sos', 'investimento', 'presenca_dias', 'awareness'],
      // FIXO: 5 cards em linha, números grandes, accent color nos valores
      // Cada card: label muted acima + valor grande + variação vs período anterior
    },
    {
      id: 'comp_sov_ranking',
      label: 'Ranking SOV por marca',
      required: true,
      dataFields: ['marca', 'sov', 'investimento'],
      chartType: 'bar',
      allowAlternativeChart: true,
    },
    {
      id: 'comp_sov_evolution',
      label: 'Evolução temporal do SOV',
      required: true,
      dataFields: ['periodo', 'sov', 'marca'],
      chartType: 'line',
      allowAlternativeChart: false,
    },
    {
      id: 'comp_channel_mix',
      label: 'Mix de canais por marca',
      required: false,
      dataFields: ['canal', 'investimento', 'marca'],
      chartType: 'bar',
      allowAlternativeChart: true,
    },
    {
      id: 'comp_insights',
      label: 'Análise competitiva',
      required: true,
      dataFields: ['sov', 'sos', 'marca', 'presenca_dias'],
      // Texto gerado pelo Claude: ESOV, gaps, oportunidades, wearout de marcas concorrentes
    }
  ],

  systemPromptContext: `
Você é um Head de Mídia analisando concorrência.
Regra ESOV: SOV ideal ≥ SOM. ESOV = SOV - SOM. ESOV positivo → crescimento de marca.
Identifique marcas com ESOV negativo (sub-investindo) e positivo (sobre-investindo).
Destaque janelas temporais onde concorrentes reduziram presença (oportunidade de share).`
}


// ─── TEMPLATE: CAMPANHAS ON + OFFLINE ────────────────────────────────────────

export const CAMPAIGN_MEDIA_TEMPLATE: ScenarioTemplate = {
  scenario: 'campaign_media',
  accentColor: '#D85A30',
  fontStyle: 'dark-premium',
  pdfExportEnabled: true,

  blocks: [
    {
      id: 'media_header',
      label: 'Cabeçalho do plano',
      required: true,
      dataFields: ['periodo', 'canal'],
    },
    {
      id: 'media_hero_kpis',
      label: 'KPIs principais — 5 cards',
      required: true,
      dataFields: ['grp', 'alcance', 'frequencia', 'cpp', 'roi'],
    },
    {
      id: 'media_grp_by_channel',
      label: 'GRP por canal',
      required: true,
      dataFields: ['canal', 'grp'],
      chartType: 'bar',
      allowAlternativeChart: true,
    },
    {
      id: 'media_investment_pie',
      label: 'Distribuição de investimento',
      required: true,
      dataFields: ['canal', 'investimento'],
      chartType: 'pie',
      allowAlternativeChart: true,
    },
    {
      id: 'media_weekly_grp',
      label: 'Pressão de mídia semanal',
      required: true,
      dataFields: ['periodo', 'grp'],
      chartType: 'bar',
      allowAlternativeChart: false,
    },
    {
      id: 'media_reach_curve',
      label: 'Alcance acumulado',
      required: false,
      dataFields: ['periodo', 'alcance'],
      chartType: 'line',
      allowAlternativeChart: false,
    },
    {
      id: 'media_insights',
      label: 'Análise do plano',
      required: true,
      dataFields: ['grp', 'alcance', 'frequencia', 'cpp'],
      // Texto gerado pelo Claude: eficiência do plano, wearout, CPP comparativo
    }
  ],

  systemPromptContext: `
Você é um Head de Mídia avaliando plano On+Offline.
GRP eficaz: 400–600/mês (FMCG) ou 800–1200 (lançamento).
Alcance mínimo: 70% target. Frequência ideal: 3–5x. Acima 7x = wearout.
CPP menor = melhor eficiência. Alerte semanas com GRP abaixo do necessário.`
}


// ─── TEMPLATE: DIGITAL ───────────────────────────────────────────────────────

export const DIGITAL_TEMPLATE: ScenarioTemplate = {
  scenario: 'digital',
  accentColor: '#378ADD',
  fontStyle: 'dark-premium',
  pdfExportEnabled: true,

  blocks: [
    {
      id: 'digital_header',
      label: 'Cabeçalho',
      required: true,
      dataFields: ['periodo', 'plataforma'],
    },
    {
      id: 'digital_hero_kpis',
      label: 'KPIs principais — 5 cards',
      required: true,
      dataFields: ['roas', 'ctr', 'cpa', 'taxa_conversao', 'cpm'],
    },
    {
      id: 'digital_roas_by_platform',
      label: 'ROAS por plataforma',
      required: true,
      dataFields: ['plataforma', 'roas'],
      chartType: 'bar',
      allowAlternativeChart: true,
    },
    {
      id: 'digital_funnel',
      label: 'Funil de conversão',
      required: true,
      dataFields: ['impressoes', 'cliques', 'conversoes'],
      chartType: 'bar',
      allowAlternativeChart: false,
    },
    {
      id: 'digital_kpi_evolution',
      label: 'Evolução de KPIs',
      required: true,
      dataFields: ['periodo', 'ctr', 'roas'],
      chartType: 'line',
      allowAlternativeChart: false,
    },
    {
      id: 'digital_spend_vs_conv',
      label: 'Investimento vs Conversões',
      required: false,
      dataFields: ['investimento', 'conversoes', 'plataforma'],
      chartType: 'scatter',
      allowAlternativeChart: true,
    },
    {
      id: 'digital_insights',
      label: 'Análise de performance',
      required: true,
      dataFields: ['roas', 'ctr', 'cpa', 'plataforma'],
      // Texto gerado pelo Claude: qual canal priorizar, onde otimizar
    }
  ],

  systemPromptContext: `
Você é um Head de Performance Digital.
ROAS ideal: e-commerce >4x | leads >2x.
CTR Search 3–8% | Social 0.5–2% | Display 0.1–0.3%.
CPM Social R$15–60 | Display R$5–20 | YouTube R$20–50.
Se CPA sobe: verifique CPM (leilão) ou CTR (criativo). Sugira realocação de budget.`
}

export const ALL_TEMPLATES: Record<string, ScenarioTemplate> = {
  competition: COMPETITION_TEMPLATE,
  campaign_media: CAMPAIGN_MEDIA_TEMPLATE,
  digital: DIGITAL_TEMPLATE,
  drops: DROPS_TEMPLATE
}
```

---

## DESIGN SYSTEM VISUAL — PADRÃO WDI PARA TODOS OS CENÁRIOS

File: `src/styles/scenarioTheme.ts`

O visual dark-premium é o padrão para todos os 4 cenários.
Cada cenário herda este base e só muda o accent color.

```typescript
export const DARK_PREMIUM_THEME = {
  // Backgrounds
  bgPage:    '#0f0f1a',   // fundo da página — navy quase preto
  bgSurface: '#1a1a2e',   // fundo dos cards principais
  bgCard:    '#252540',   // fundo dos cards internos / KPI cards
  bgCardHover: '#2e2e50',

  // Texto
  textPrimary:   '#f0f0f0',
  textSecondary: '#a0a0b8',
  textMuted:     '#606078',

  // Bordas
  border:        'rgba(255,255,255,0.08)',
  borderHover:   'rgba(255,255,255,0.16)',

  // Accent por cenário (sobrescreve o accent do tema base)
  accents: {
    drops:          '#F5C518',  // amarelo WDI
    competition:    '#7F77DD',  // purple
    campaign_media: '#D85A30',  // coral
    digital:        '#378ADD',  // blue
  },

  // Tipografia
  fontHeadline:  '"Plus Jakarta Sans", "Inter", sans-serif',
  fontBody:      '"Plus Jakarta Sans", "Inter", sans-serif',
  fontMono:      '"JetBrains Mono", monospace',

  // Tamanhos fixos por hierarquia
  fontHero:    '48px',   // número principal do hero (22,7 pontos)
  fontH1:      '32px',   // título do programa
  fontH2:      '20px',   // títulos de seção
  fontH3:      '16px',   // títulos de card
  fontLabel:   '11px',   // labels dos KPIs (AUDIÊNCIA, SHARE)
  fontSizeBody:'14px',
  fontSmall:   '12px',

  // Radii
  radiusCard:  '12px',
  radiusInner: '8px',
  radiusPill:  '999px',
}
```

---

## COMPONENTE: DashboardRenderer

File: `src/components/DashboardRenderer.tsx`

Este componente é o coração do sistema. Ele recebe um `MergedDataset` e um
`ScenarioTemplate`, e renderiza o dashboard bloco por bloco na ordem definida
pelo template. Não há lógica de "qual bloco mostrar" — todos são mostrados
sempre, na ordem do array `blocks`.

```typescript
interface DashboardRendererProps {
  dataset: MergedDataset
  template: ScenarioTemplate
  onExportPDF: () => void
}

export function DashboardRenderer({ dataset, template, onExportPDF }: DashboardRendererProps) {
  const schema = dataset.schema
  const data = dataset.canonicalData
  const accent = template.accentColor

  return (
    <div
      style={{
        background: DARK_PREMIUM_THEME.bgPage,
        minHeight: '100vh',
        color: DARK_PREMIUM_THEME.textPrimary,
        fontFamily: DARK_PREMIUM_THEME.fontHeadline,
        padding: '0',
      }}
      id="dashboard-export-root"
    >
      {template.blocks.map(block => (
        <DashboardBlock
          key={block.id}
          block={block}
          data={data}
          schema={schema}
          accent={accent}
          missingFields={dataset.missingFields}
        />
      ))}

      {/* Botão PDF fixo no canto inferior direito — igual ao WDI */}
      {template.pdfExportEnabled && (
        <button
          data-pdf-btn
          onClick={onExportPDF}
          style={{
            position: 'fixed',
            bottom: '24px',
            right: '24px',
            background: accent,
            color: '#000',
            border: 'none',
            borderRadius: '999px',
            padding: '12px 24px',
            fontWeight: '700',
            fontSize: '14px',
            cursor: 'pointer',
            display: 'flex',
            alignItems: 'center',
            gap: '8px',
            zIndex: 100,
            boxShadow: `0 4px 20px ${accent}44`,
          }}
        >
          ↓ Baixar PDF
        </button>
      )}
    </div>
  )
}
```

---

## BLOCO DROPS: HERO KPI — implementação de referência

File: `src/components/blocks/DropsHeroBlock.tsx`

```typescript
// Renderiza o bloco hero no estilo WDI:
// Título grande + número em destaque + subtítulo + pill de alcance + CTA

export function DropsHeroBlock({ data, accent, schema }) {
  const mainEmissora = [...data]
    .sort((a, b) => Number(b.audiencia) - Number(a.audiencia))[0]

  const programa    = mainEmissora?.programa ?? '—'
  const audiencia   = Number(mainEmissora?.audiencia ?? 0).toFixed(1)
  const domicilios  = Number(mainEmissora?.domicilios ?? 0)
  const marca       = mainEmissora?.marca ?? ''

  const domText = domicilios > 0
    ? `Mais de ${domicilios.toFixed(1)} milhões de pessoas assistiram`
    : null

  return (
    <div style={{
      textAlign: 'center',
      padding: '64px 48px 48px',
      background: DARK_PREMIUM_THEME.bgSurface,
    }}>
      {/* H1 — título + número em accent color */}
      <h1 style={{
        fontSize: DARK_PREMIUM_THEME.fontH1,
        fontWeight: 800,
        margin: '0 0 12px',
        lineHeight: 1.1,
        color: DARK_PREMIUM_THEME.textPrimary,
      }}>
        {programa} alcança{' '}
        <span style={{ color: accent }}>{audiencia} pontos</span>
      </h1>

      {/* Subtítulo com marca */}
      {marca && (
        <p style={{ color: DARK_PREMIUM_THEME.textSecondary, fontSize: '18px', margin: '0 0 32px' }}>
          em capítulo com ação de <strong style={{ color: DARK_PREMIUM_THEME.textPrimary }}>{marca}</strong>
        </p>
      )}

      {/* Pill de alcance + botão CTA */}
      <div style={{ display: 'flex', justifyContent: 'center', gap: '16px', flexWrap: 'wrap' }}>
        {domText && (
          <div style={{
            background: DARK_PREMIUM_THEME.bgCard,
            border: `1px solid ${DARK_PREMIUM_THEME.border}`,
            borderRadius: DARK_PREMIUM_THEME.radiusPill,
            padding: '10px 20px',
            fontSize: '14px',
            color: DARK_PREMIUM_THEME.textSecondary,
          }}>
            👥 {domText}
          </div>
        )}
        {/* CTA — só aparece se URL da ação estiver nos dados extras */}
      </div>
    </div>
  )
}
```

---

## BLOCO DROPS: EMISSORAS — cards comparativos

```typescript
export function DropsEmissorasBlock({ data, accent }) {
  // Agrupa por emissora e pega o valor médio/máximo de audiência e share
  const byEmissora = groupBy(data, 'emissora')
  const emissoras = Object.entries(byEmissora)
    .map(([emissora, rows]) => ({
      emissora,
      audiencia: avg(rows.map(r => Number(r.audiencia))),
      share:     avg(rows.map(r => Number(r.share))),
    }))
    .sort((a, b) => b.audiencia - a.audiencia)

  // Dot color por emissora
  const EMISSORA_COLORS: Record<string, string> = {
    'globo':    '#2563eb',
    'record':   '#16a34a',
    'sbt':      '#ea580c',
    'band':     '#7c3aed',
    'redetv':   '#db2777',
  }

  return (
    <div style={{
      display: 'grid',
      gridTemplateColumns: `repeat(${Math.min(emissoras.length, 4)}, 1fr)`,
      gap: '16px',
      padding: '0 48px 48px',
    }}>
      {emissoras.map(e => {
        const dotColor = EMISSORA_COLORS[e.emissora.toLowerCase()] ?? '#888'
        return (
          <div key={e.emissora} style={{
            background: DARK_PREMIUM_THEME.bgCard,
            borderRadius: DARK_PREMIUM_THEME.radiusCard,
            padding: '24px',
          }}>
            {/* Nome com dot */}
            <div style={{ display: 'flex', alignItems: 'center', gap: '8px', marginBottom: '20px' }}>
              <span style={{
                width: '10px', height: '10px',
                borderRadius: '50%',
                background: dotColor,
                display: 'inline-block',
              }}/>
              <span style={{ fontWeight: 600, fontSize: '16px' }}>{e.emissora}</span>
            </div>
            {/* KPIs: Audiência + Share */}
            <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: '12px' }}>
              <div>
                <div style={{ fontSize: '11px', color: DARK_PREMIUM_THEME.textMuted, letterSpacing: '0.08em', marginBottom: '4px' }}>
                  AUDIÊNCIA
                </div>
                <div style={{ fontSize: '28px', fontWeight: 800, color: accent }}>
                  {e.audiencia.toFixed(1)}%
                </div>
              </div>
              <div>
                <div style={{ fontSize: '11px', color: DARK_PREMIUM_THEME.textMuted, letterSpacing: '0.08em', marginBottom: '4px' }}>
                  SHARE
                </div>
                <div style={{ fontSize: '28px', fontWeight: 800, color: accent }}>
                  {e.share.toFixed(1)}%
                </div>
              </div>
            </div>
          </div>
        )
      })}
    </div>
  )
}
```

---

## BLOCO DROPS: MINUTO A MINUTO — gráfico de linha com marcador de ação

```typescript
import { LineChart, Line, XAxis, YAxis, Tooltip, ReferenceLine, ResponsiveContainer } from 'recharts'

export function DropsMinutoBlock({ data, accent, schema }) {
  // Usa dados canônicos: minuto + audiencia_minuto + horario_acao
  const minuteData = data
    .filter(r => r.minuto && r.audiencia_minuto)
    .sort((a, b) => String(a.minuto).localeCompare(String(b.minuto)))
    .map(r => ({
      time: String(r.minuto),
      rating: Number(r.audiencia_minuto),
    }))

  const actionTime = data.find(r => r.horario_acao)?.horario_acao as string
  const actionRating = data.find(r => r.horario_acao)?.audiencia_pre_acao
  const marca = data.find(r => r.marca)?.marca as string

  // Tooltip customizado — narrativo como no WDI
  const CustomTooltip = ({ active, payload, label }: any) => {
    if (!active || !payload?.length) return null
    const isActionMoment = label === actionTime
    return (
      <div style={{
        background: DARK_PREMIUM_THEME.bgCard,
        border: `1px solid ${isActionMoment ? accent : DARK_PREMIUM_THEME.border}`,
        borderRadius: '8px',
        padding: '12px 16px',
        maxWidth: '280px',
      }}>
        {isActionMoment && marca ? (
          <p style={{ color: DARK_PREMIUM_THEME.textSecondary, fontSize: '12px', margin: 0 }}>
            No minuto em que se iniciou a ação de{' '}
            <strong style={{ color: DARK_PREMIUM_THEME.textPrimary }}>{marca}</strong>{' '}
            — {label} — a audiência era{' '}
            <strong style={{ color: accent }}>{payload[0].value.toFixed(2)} pontos</strong>.
          </p>
        ) : (
          <p style={{ margin: 0, fontSize: '13px' }}>
            <strong style={{ color: accent }}>{payload[0].value.toFixed(2)} pts</strong>
            {' '}às {label}
          </p>
        )}
      </div>
    )
  }

  return (
    <div style={{ padding: '0 48px 48px' }}>
      <div style={{
        display: 'grid',
        gridTemplateColumns: minuteData.length > 0 ? '1fr auto' : '1fr',
        gap: '24px',
        background: DARK_PREMIUM_THEME.bgCard,
        borderRadius: DARK_PREMIUM_THEME.radiusCard,
        padding: '24px',
      }}>
        <div>
          <div style={{ display: 'flex', alignItems: 'center', gap: '12px', marginBottom: '20px' }}>
            <span style={{ fontSize: '16px' }}>⏱</span>
            <span style={{ fontWeight: 700, fontSize: '16px' }}>Minuto a Minuto</span>
            {data[0]?.data && (
              <span style={{ color: DARK_PREMIUM_THEME.textMuted, fontSize: '12px' }}>
                {String(data[0].data)} · {data[0].horario_inicio ?? ''} às {data[0].horario_fim ?? ''}
              </span>
            )}
          </div>

          {minuteData.length > 0 ? (
            <ResponsiveContainer width="100%" height={220}>
              <LineChart data={minuteData} margin={{ top: 4, right: 16, left: 0, bottom: 0 }}>
                <XAxis dataKey="time" tick={{ fill: DARK_PREMIUM_THEME.textMuted, fontSize: 11 }} />
                <YAxis tick={{ fill: DARK_PREMIUM_THEME.textMuted, fontSize: 11 }} domain={['auto', 'auto']} />
                <Tooltip content={<CustomTooltip />} />
                {actionTime && (
                  <ReferenceLine
                    x={actionTime}
                    stroke={accent}
                    strokeDasharray="4 2"
                    label={{ value: marca ?? 'Ação', fill: accent, fontSize: 11, position: 'top' }}
                  />
                )}
                <Line
                  type="monotone"
                  dataKey="rating"
                  stroke={accent}
                  strokeWidth={2}
                  dot={false}
                  activeDot={{ r: 5, fill: accent }}
                />
              </LineChart>
            </ResponsiveContainer>
          ) : (
            <EmptyState
              icon="chart"
              title="Dados minuto a minuto não encontrados"
              description="Esperado: colunas 'minuto' e 'audiencia_minuto'"
            />
          )}
        </div>

        {/* Imagem da marca — lado direito */}
        <div style={{
          width: '200px',
          background: DARK_PREMIUM_THEME.bgSurface,
          borderRadius: DARK_PREMIUM_THEME.radiusInner,
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
          padding: '16px',
          minHeight: '160px',
        }}>
          {/* Se houver URL de imagem nos dados extras: <img> */}
          {/* Senão: nome da marca centralizado */}
          <span style={{ color: accent, fontWeight: 700, fontSize: '18px', textAlign: 'center' }}>
            {marca ?? ''}
          </span>
        </div>
      </div>
    </div>
  )
}
```

---

## EXPORTAÇÃO PDF — padrão WDI

File: `src/lib/exportPDF.ts`

```typescript
import html2canvas from 'html2canvas'
import jsPDF from 'jspdf'

export async function exportDashboardToPDF(
  rootElementId: string,
  filename: string
): Promise<void> {
  const element = document.getElementById(rootElementId)
  if (!element) return

  // Remove o botão PDF antes de capturar
  const pdfBtn = document.querySelector('[data-pdf-btn]') as HTMLElement
  if (pdfBtn) pdfBtn.style.display = 'none'

  const canvas = await html2canvas(element, {
    backgroundColor: '#0f0f1a',
    scale: 2,  // alta resolução
    useCORS: true,
    allowTaint: false,
    scrollX: 0,
    scrollY: -window.scrollY,
  })

  if (pdfBtn) pdfBtn.style.display = ''

  const imgData = canvas.toDataURL('image/png')
  const pdf = new jsPDF({
    orientation: 'portrait',
    unit: 'px',
    format: [canvas.width / 2, canvas.height / 2],
  })

  pdf.addImage(imgData, 'PNG', 0, 0, canvas.width / 2, canvas.height / 2)
  pdf.save(filename)
}
```

Instalar dependências:
```bash
npm install html2canvas jspdf
npm install -D @types/html2canvas
```

---

## ADIÇÃO AO BUILD ORDER

Inserir entre os passos 5 e 6 do build order original:

```
5b. DROPS_SCHEMA + DROPS_TEMPLATE + DROPS_TEMPLATE_BLOCKS
5c. ALL_TEMPLATES + DashboardRenderer + DARK_PREMIUM_THEME
5d. Blocos específicos do Drops: DropsHeroBlock, DropsEmissorasBlock,
    DropsMinutoBlock, DropsDestaquesBlock, DropsTrendsBlock
5e. Adaptar blocos dos outros 3 cenários para herdar o DARK_PREMIUM_THEME
    (mesma estrutura visual, só accent color muda)
5f. exportDashboardToPDF com html2canvas + jsPDF
```
