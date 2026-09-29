# Reprodução computacional de resultados selecionados da tese

**Versão do pacote: 1.1.0.** Este repositório distribui um pacote arquivado com dados preservados, scripts, valores esperados e registros de verificação. Ele permite recalcular resultados selecionados de uma tese sobre arquitetura de dados para gêmeos digitais.

O pacote não constitui reprodução integral da tese, repetição da campanha física, reexecução do modelo térmico histórico ou recalibração de parâmetros. Não requer conexão com Kafka, Amazon RDS ou a bancada experimental.

## Obter o material

| Arquivo | Conteúdo |
|---|---|
| [reprodutibilidade_tese_v1.1.0.zip](reprodutibilidade_tese_v1.1.0.zip) | Pacote completo: dados, scripts, documentação e evidências preservadas |
| [reprodutibilidade_tese_v1.1.0.zip.sha256](reprodutibilidade_tese_v1.1.0.zip.sha256) | Resumo SHA-256 do ZIP |
| [reprodutibilidade_tese_v1.1.0.inventario.json](reprodutibilidade_tese_v1.1.0.inventario.json) | Inventário dos 87 arquivos internos, com tamanhos e hashes |

Esta forma de distribuição mantém o pacote dentro do ZIP; os scripts não estão disponíveis como arquivos avulsos na raiz do repositório. Baixe e extraia o ZIP para consultar ou executar o conteúdo. A ausência de visualização dos CSVs no site não significa ausência dos dados no pacote.

O ZIP original tem **5.142.186 bytes** e SHA-256:

```text
498f51313ed4871c8823f7b50f37ce534393970ad75d7f4b16b051d08a0afdf6
```

O inventário registra os arquivos e o estado da preparação original. Expressões como "cópia local" ou "sem publicação" nos documentos internos descrevem aquele momento, anterior à disponibilização deste repositório. O ZIP não foi recomposto para alterar essa documentação histórica.

## Escopo dos resultados

Na execução preservada de **21/09/2026**, a versão 1.1.0 registrou:

- 91 valores reproduzidos nas precisões declaradas;
- 106 testes de software aprovados: 91 comparações de valores e 15 verificações auxiliares;
- zero falhas, erros ou casos não executados na suíte registrada.

Essas contagens descrevem a execução documentada, não uma execução automática realizada ao acessar o repositório. Os testes não correspondem a novos ensaios físicos ou a 106 validações científicas independentes. Os registros estão em `verificacao/v1.1.0/`, dentro do pacote.

| Tema | Correspondência documentada na tese v09 | Identificador legado dos scripts |
|---|---|---|
| Latência | Tabela 5.2, nove ensaios selecionados | 5.1 |
| Erro térmico | Tabela 5.3, somente E13 e E14 | 5.2 |
| Concordância | Tabela 5.6 e métricas do texto | 5.5 |
| Recuperação | Tabela 5.7 e indicadores complementares | 5.6 |
| Supervisão | Tabela 5.8 e métricas do texto | 5.7 |

Os identificadores dos scripts foram preservados. Eles não devem ser interpretados como a numeração atual de todas as versões do manuscrito.

## Conferir e executar no Windows

Ambiente de referência: **Python 3.11**. Os cálculos utilizam a biblioteca padrão; as dependências dos testes são fixadas em `requirements.txt`. Execute os comandos em PowerShell, um por vez, interrompendo em caso de erro.

Na pasta em que o ZIP foi obtido, confira o hash:

```powershell
Get-FileHash -LiteralPath '.\reprodutibilidade_tese_v1.1.0.zip' -Algorithm SHA256
```

O resultado deve coincidir com o SHA-256 acima. Extraia para uma pasta nova:

```powershell
Expand-Archive -LiteralPath '.\reprodutibilidade_tese_v1.1.0.zip' -DestinationPath '.\pacote_extraido'
Set-Location -LiteralPath '.\pacote_extraido\reprodutibilidade_tese'
```

Crie o ambiente e execute:

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -B scripts\gerar_tudo.py
.\.venv\Scripts\python.exe -B scripts\verificar_resultados.py
.\.venv\Scripts\python.exe -B -m pytest testes -q --junitxml=saida\testes.xml
.\.venv\Scripts\python.exe -B scripts\auditar_pacote.py
```

A instalação das dependências requer internet. As análises usam exclusivamente arquivos locais. As novas saídas ficam em `saida/`; os registros preservados não são substituídos por esses comandos. O README interno apresenta também orientações para Linux e macOS.

## Limitações de interpretação

- As métricas térmicas de E13 e E14 usam previsões históricas arquivadas e temperaturas processadas correspondentes. O pacote não reexecuta o modelo que gerou as previsões nem fornece uma medição metrológica independente da temperatura.
- A referência GPIO registrada nos conjuntos comparáveis é um contato manual do operador, não uma medição automática da energização do motor. E14 não contém referência GPIO registrada.
- As decisões de E14 correspondem à supervisão em modo shadow, com atuação física manual; não demonstram controle automático por relé.
- A recuperação dos conjuntos bufferizados auditados não comprova, por si só, entrega integral de todos os eventos da campanha. As conciliações preservadas delimitam os conjuntos examinados.
- O desvio residual entre os relógios não foi caracterizado independentemente. Isso não comprova ausência de sincronização.
- A cobertura não inclui todas as tabelas da tese, todos os ensaios térmicos nem os pacotes completos dos Apêndices C e D.
- A varredura preservada se limita aos padrões procurados. Não é uma certificação de anonimato ou ausência de qualquer informação sensível.

## Licença e identificação da versão

O pacote preservado contém `LICENCA_A_DEFINIR.md`; nenhuma licença de reutilização foi concedida automaticamente. A definição das condições aplicáveis a código, dados e documentação permanece sob responsabilidade dos titulares. A eventual divulgação pública do repositório não substitui essa definição.

Para identificar o material utilizado, registre a versão **1.1.0**, o SHA-256 do ZIP e o commit ou a release efetivamente consultados. Não há DOI atribuído neste README. Alterações posteriores no pacote devem receber outra identificação, sem substituir silenciosamente o ZIP desta versão.
