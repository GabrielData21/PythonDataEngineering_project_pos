# Python Data Engineering — Projeto 1

Primeiro projeto da pós-graduação em **Engenharia de Dados e IA**: um pipeline de dados simples, em notebooks Python, que leva arquivos CSV brutos até uma tabela dimensional pronta para análise, usando **DuckDB** como banco local.

## Pipeline

```
landing/*.csv ──► ingestao ──► dados_duckdb.db ──► enriquecimento ──► refinamento ──► dim_produtos
```

| Etapa | Notebook | O que faz |
|---|---|---|
| 1. Ingestão | `scripts/ingestao.ipynb` | Lê os CSVs da pasta `landing/` e grava as tabelas brutas no DuckDB |
| 2. Enriquecimento | `scripts/enriquecimento.ipynb` | Trata os dados: atualiza registros, formata colunas e altera seus nomes |
| 3. Refinamento | `scripts/refinamento.ipynb` | Seleciona as colunas de `produtos` e cria a tabela `dim_produtos` |

## Estrutura

```
.
├── landing/              # arquivos brutos de entrada (CSV separado por ;)
├── scripts/              # notebooks de cada etapa do pipeline
├── dados_duckdb.db       # banco DuckDB gerado pela ingestão
└── requirements.txt      # dependências Python
```

## Tecnologias

- Python 3.15
- DuckDB
- pandas
- Jupyter (via VS Code)

## Como rodar

Testado no **Fedora**, com VS Code e a extensão Jupyter.

**1. Criar o ambiente virtual**

```bash
python3.15 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

**2. Abrir no VS Code**

Abra a **pasta raiz do projeto** (não a pasta `scripts/`) e selecione o kernel `.venv (Python 3.15)` em cada notebook.

**3. Executar os notebooks em ordem**

`ingestao` → `enriquecimento` → `refinamento`

## Observações importantes

- **Os notebooks rodam a partir da raiz do projeto.** O arquivo `.vscode/settings.json` configura o kernel para iniciar na raiz, então todos os caminhos são relativos a ela (`landing/...`, `dados_duckdb.db`). A primeira célula de conexão imprime o diretório atual para conferência.
- **O banco não é recriado por engano.** Se o `dados_duckdb.db` não for encontrado, os notebooks de enriquecimento e refinamento param com erro, em vez de criar um banco vazio.
- **Feche a conexão ao terminar.** O DuckDB bloqueia o arquivo enquanto uma conexão de escrita estiver aberta. Use `conn.close()` ou reinicie o kernel antes de rodar o próximo notebook.
- **O `.venv` não é portável.** Se o projeto mudar de pasta ou de máquina, recrie o ambiente com os comandos do passo 1.

## Próximos passos

Quem sabe? É um projeto simples, e as possibilidades de melhoria são muitas! Algumas ideias:

- Receber mais arquivos de entrada
- Criar schemas para as etapas da arquitetura
- Escrever pequenos testes unitários
- Implementar SCD (Slowly Changing Dimension)

## Autor

Gabriel Mendonça — Pós-graduação em Engenharia de Dados e IA
