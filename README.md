# TRIBE v2 — Cognitive Load Demo

Sandbox isolado para testar predição de carga cognitiva via TRIBE v2 sobre
blocos de conflito de merge. **Não integra ao `mlmg/` ainda** — é um
experimento standalone enquanto se valida se o sinal é útil.

## Estrutura

```
tribe/
├── tribe_demo.ipynb        # Notebook principal (rodar no Colab)
├── conflicts/              # Estímulos: arquivos .txt com marcadores
│   ├── 01-method-conflict.txt   (conflito textual trivial)
│   └── scenario_38.txt          (paradigm clash — espera-se carga maior)
└── README.md
```

## Como rodar (Colab Free)

1. No Colab, **File → Upload notebook** → escolher `tribe_demo.ipynb`.
2. No painel lateral (ícone de pasta), **upload da pasta `conflicts/`**
   (ou arrastar os `.txt` individualmente para o diretório raiz da sessão,
   ajustando o caminho na célula 3).
3. **Runtime → Change runtime type → T4 GPU** (free).
4. **Runtime → Run all**.

A primeira execução baixa os pesos do TRIBE (~vários GB) — pode levar
alguns minutos. Predições subsequentes usam cache local da sessão.

## Saída esperada

Tabela com colunas:

| conflict           | timesteps | vertices | mean_load | peak_load |
|--------------------|-----------|----------|-----------|-----------|
| scenario_38        | ...       | ~20000   | maior     | maior     |
| 01-method-conflict | ...       | ~20000   | menor     | menor     |

Também salva `tribe_results.csv` que pode ser baixado.

## Hipótese sob teste

Conflitos de paradigm clash (scenario_38: exceções vs booleano+log) devem
produzir maior ativação predita do que conflitos textuais triviais
(01-method-conflict: duas variantes da mesma linha). Se confirmado, TRIBE
serve como sinal para o roteamento adaptativo descrito em
`../NEUROFEEDBACK_ARCHITECTURE.md`.

## Adicionar novos conflitos

Dropar mais arquivos `.txt` em `conflicts/`. Cada arquivo deve conter o
bloco com marcadores `<<<<<<<`, `=======`, `>>>>>>>` — exatamente como o
desenvolvedor veria no editor.

## Limitações conhecidas desta fase

- Score é agregação global (média de `|ativação|`). Versão principled usa
  máscara da rede frontoparietal (Yeo-7) — fica para próximo passo.
- Carga não normalizada por tamanho do estímulo — scenario_38 é
  intrinsecamente maior, parte do score pode ser efeito de duração.
- TRIBE converte texto→fala internamente; código pode soar estranho na
  voz sintetizada. Alternativa futura: renderizar diff como imagem e usar
  o branch de vídeo do TRIBE.
- Modelo prediz fMRI de um cérebro "médio", não do usuário atual. Não é
  neurofeedback no sentido estrito — é roteamento por dificuldade
  predita.

## Licença do TRIBE v2

CC BY-NC. Uso acadêmico/pesquisa OK; uso comercial requer outra licença.
