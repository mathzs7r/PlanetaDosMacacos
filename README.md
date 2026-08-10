# Concorrência de Cursos em Cotas Públicas — UEPG / UTFPR-PG (2016–2025)

Website de análise descritiva da concorrência (candidatos por vaga) em **cotas públicas**
no Vestibular de Verão da UEPG (2016–2025) e no Processo Seletivo da UTFPR — Câmpus Ponta
Grossa (2023–2025), com notas mínimas de aprovação e salário médio das profissões.

Além do painel principal (gráfico de linha comparando os 6 cursos da UEPG, tabela
filtrável e as 8 questões de análise), há **12 páginas individuais de curso** geradas por
um template único (`curso.js`), acessíveis pelo menu lateral.

## Links

- Repositório: <https://github.com/mathzs7r/concorrencia-cotas-uepg>
- GitHub Pages: <https://mathzs7r.github.io/concorrencia-cotas-uepg/>

## Tecnologias

- HTML5
- CSS com **Bootstrap 5** (via CDN) + `style.css`
- JavaScript (ES Modules, sem framework)
- **Chart.js 4** (gráfico de linha)

## Estrutura

```
/
├── index.html          # painel principal (hero, KPIs, filtros, gráfico, tabela, questões, fontes)
├── curso1..6.html      # páginas dos cursos da UTFPR-PG (carregam curso.js)
├── curso11..66.html    # páginas dos cursos da UEPG (carregam curso.js)
├── style.css           # identidade visual (tema escuro) sobre o Bootstrap
├── script.js           # painel principal: estatísticas, gráfico e tabela
├── curso.js            # template dinâmico das páginas de curso
├── nav.js              # menu lateral e rodapé compartilhados
├── util.js             # formatadores pt-BR, paletas, estatística e tema do Chart.js
├── db.js               # base UEPG (export const db) — 49 cursos, 2016–2025
├── db_utfpr.js         # base UTFPR-PG (export const dbUtfpr) — 6 cursos, 2023–2025
└── README.md
```

O menu lateral, o rodapé, as tabelas, os gráficos e as leituras analíticas das páginas de
curso são gerados em JavaScript a partir das bases — não há dados duplicados no HTML.

### Formato do `db.js`

```js
export const db = {
  cursos: [
    {
      id: 1,
      nome: "Administração",
      modalidade: "Bacharelado",
      salariosAtuais: [{ cargo: "...", salario: 5081.07, referencia: "..." }],
      cotas: [
        {
          ano: 2016,
          tipoCota: [
            { tipo: "Escola Pública", vagas: 12, candidatos: 189, notaMinima: 2559 },
          ],
        },
      ],
      analise: "texto descritivo gerado a partir dos próprios dados",
    },
  ],
};
```

Tipos de cota disponíveis: `Universal`, `Escola Pública`, `Escola Pública - Negros`,
`Negros` e `PcD`. O site usa `Escola Pública` como padrão e permite alternar a cota.

## Cursos analisados

Foram selecionados 6 cursos com série histórica completa nas 10 edições (2016–2025):

| Curso | Cargo usado para o salário médio | Salário médio mensal |
| --- | --- | --- |
| Medicina | Médico Clínico (CBO 2251-25) | R$ 10.048,57 |
| Direito (Matutino) | Advogado (CBO 2410-05) | R$ 5.660,53 |
| Enfermagem | Enfermeiro (CBO 2235-05) | R$ 4.475,06 |
| Engenharia Civil | Engenheiro Civil (CBO 2142-05) | R$ 9.733,56 |
| Engenharia de Computação | Engenheiro de Softwares Computacionais (CBO 2122-05) | R$ 14.430,84 |
| Administração | Administrador (CBO 2521-05) | R$ 5.081,07 |

As páginas individuais cobrem ainda Engenharia de Software e Farmácia (UEPG) e os seis
cursos da UTFPR-PG: Ciências Biológicas, Engenharia de Produção, Análise e Desenvolvimento
de Sistemas, Engenharia Mecânica, Automação Industrial e Ciência da Computação.

## Fontes dos dados

- **Concorrência, vagas, inscritos e notas mínimas**: informativos oficiais do Vestibular
  de Verão da UEPG (edições 2016 a 2025) — Comissão Permanente de Seleção da UEPG,
  <https://www.uepg.br/cps/>. Dados consolidados no arquivo `db.js`.
- **Salário médio (UEPG)**: Portal Salário (<https://www.salario.com.br/>), com base no
  CAGED/MTE — média salarial CLT no Brasil por CBO, consulta em agosto de 2026.
- **Concorrência da UTFPR-PG**: relatórios de concorrência do Processo Seletivo da UTFPR —
  Câmpus Ponta Grossa, edições 2023, 2024 e 2025, consolidados em `db_utfpr.js`. A série é
  mais curta que a da UEPG e cobre apenas a cota de ampla concorrência (Universal).
- **Salário médio (UTFPR-PG)**: estimativa do grupo a partir de médias de mercado
  publicadas (CAGED/MTE e Glassdoor Brasil) — valores aproximados, não oficiais.

## Principais resultados (cota Escola Pública, 2016–2025)

| Questão | Resposta |
| --- | --- |
| Maior concorrência | Medicina — média de 90,51 candidatos/vaga |
| Menor concorrência | Engenharia de Computação — média de 5,66 candidatos/vaga |
| Maior crescimento | Medicina — de 102,83 para 126,80 candidatos/vaga (+23,3%) |
| Quedas | Engenharia Civil (−76,1%), Enfermagem (−31,7%), Direito (−25,0%), Engenharia de Computação (−23,4%), Administração (−21,4%) |
| Mais estável | Administração — coeficiente de variação de 33,7% |
| Ano mais concorrido | 2017 — 35,90 candidatos/vaga em média |
| Concorrência × nota mínima | Correlação de Pearson r = 0,861 (forte e positiva) |
| Concorrência × salário médio | r = 0,136 — salário alto não implica maior procura |

Todos os números exibidos no site são calculados em tempo de execução a partir do `db.js`,
inclusive as respostas das questões de análise (que se atualizam ao trocar o tipo de cota).

## Como executar localmente

O `db.js` é um módulo ES, portanto é necessário servir os arquivos por HTTP (abrir o
`index.html` direto pelo `file://` bloqueia o `import`):

```bash
python3 -m http.server 8000
# abra http://localhost:8000
```

## Publicação

O site é estático e publicado via **GitHub Pages** (branch `main`, pasta raiz).

## Identidade visual

Tema escuro com fundo `#0c0f14`, superfícies `#1c2232`, acentos `#4daf8a` (verde) e
`#5b9cf6` (azul), tipografia DM Serif Display (títulos), DM Sans (texto) e JetBrains Mono
(rótulos e números). Os tokens estão em `:root` no `style.css` e são reaproveitados pelo
Bootstrap e pelo Chart.js.
