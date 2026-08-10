# Concorrência de Cursos em Cotas Públicas — UEPG (2016–2025)

Website de análise descritiva da concorrência (candidatos por vaga) em **cotas de escola
pública** no Vestibular de Verão da UEPG (campus Ponta Grossa), entre 2016 e 2025, com
notas mínimas de aprovação e salário médio das profissões correspondentes.

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
├── index.html   # estrutura da página (navbar, filtros, gráfico, tabela, análise, fontes)
├── style.css    # estilos próprios complementares ao Bootstrap
├── script.js    # leitura do db.js, cálculos estatísticos, gráfico e render da tabela
├── db.js        # base de dados (export const db) com os 49 cursos e o histórico de cotas
└── README.md
```

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

## Fontes dos dados

- **Concorrência, vagas, inscritos e notas mínimas**: informativos oficiais do Vestibular
  de Verão da UEPG (edições 2016 a 2025) — Comissão Permanente de Seleção da UEPG,
  <https://www.uepg.br/cps/>. Dados consolidados no arquivo `db.js`.
- **Salário médio**: Portal Salário (<https://www.salario.com.br/>), com base no
  CAGED/MTE — média salarial CLT no Brasil por CBO, consulta em agosto de 2026.

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
