# Implantação e atualização do Oracle Database 19c Multitenant

## Visão geral

Este projeto documenta a implantação do Oracle Database 19c em Oracle Linux, a criação de uma arquitetura Multitenant com CDB/PDB e a aplicação de um Release Update com OPatch e datapatch.

## Objetivo

Demonstrar um fluxo reproduzível para:

- validar o sistema operacional e o armazenamento;
- preparar os pré-requisitos do Oracle Database 19c;
- instalar o software em modo silencioso;
- criar e validar uma CDB com PDB;
- atualizar o Oracle Home com um Release Update;
- aplicar as alterações SQL com datapatch;
- confirmar a versão e o registro do patch nos containers.

## Tecnologias utilizadas

- Oracle Database 19c Enterprise Edition
- Oracle Multitenant (CDB/PDB)
- Oracle Database Configuration Assistant (DBCA)
- OPatch e datapatch
- SQL*Plus
- Oracle Linux 8
- Shell

## Arquitetura e fluxo

```mermaid
flowchart LR
    A["Oracle Linux e storage"] --> B["Pré-requisitos do Oracle 19c"]
    B --> C["Instalação silenciosa do software"]
    C --> D["Criação da CDB e PDB com DBCA"]
    D --> E["Atualização do OPatch"]
    E --> F["Aplicação do Release Update"]
    F --> G["Execução do datapatch"]
    G --> H["Validação no CDB e na PDB"]
```

## Estrutura do repositório

```text
.
├── README.md
├── .gitignore
├── docs/
│   └── procedure.md
├── images/
│   ├── 01-os-and-storage-validation.png
│   ├── 02-oracle-preinstall-package.png
│   ├── 03-silent-software-installation.png
│   ├── 04-multitenant-database-created.png
│   ├── 05-release-update-applied.png
│   ├── 06-datapatch-completed.png
│   └── 07-release-update-validation.png
├── scripts/
│   └── shell/
│       └── install-software.example.sh
└── sql/
    └── validate-release-update.sql
```

## Procedimento executado

1. Validação dos filesystems e da versão do Oracle Linux.
2. Instalação do pacote `oracle-database-preinstall-19c`.
3. Preparação do Oracle Base, Oracle Home, inventário, área de dados, stage e área de recuperação.
4. Instalação silenciosa do software com `runInstaller`.
5. Execução dos scripts de root solicitados pelo instalador.
6. Criação silenciosa da CDB e da PDB com DBCA.
7. Validação da instância, dos containers e do estado da PDB.
8. Atualização do OPatch.
9. Aplicação do Release Update ao Oracle Home.
10. Inicialização do banco e execução do datapatch.
11. Validação do patch em `CDB_REGISTRY_SQLPATCH` e da versão em `V$VERSION`.

O [procedimento detalhado](docs/procedure.md) relaciona cada etapa às evidências disponíveis.

## Principais comandos

### Classificação de impacto

| Nível | Etapa deste projeto | Critério de execução |
|---|---|---|
| D0 — observação | `df -h`, `SHOW PDBS`, consulta a `V$VERSION` e `CDB_REGISTRY_SQLPATCH` | Confirmar host, CDB/PDB e versão antes e depois da mudança. |
| D1 — preparação | Instalação de pré requisitos e criação de diretórios | Conferir sistema operacional, espaço, proprietário e caminhos. |
| D2 — indisponibilidade | Instalação, criação da CDB/PDB e aplicação de Release Update com OPatch/datapatch | Usar janela, backup, plano de retorno e validação binária e SQL. |
| D3 — recuperação destrutiva | Não faz parte do fluxo principal de implantação | Consultar o [índice de risco operacional](https://github.com/PatrickCecilio/biblioteca-tecnica-dba/blob/main/COMANDOS-POR-RISCO.md) antes de usar procedimentos de restore, flashback ou remoção. |

Validação inicial:

```bash
df -h
hostname
cat /etc/os-release
```

Pré-requisito:

```bash
yum install -y oracle-database-preinstall-19c
```

Validação dos containers:

```sql
SHOW PDBS;
SELECT banner_full FROM v$version;
```

O exemplo de instalação em [`scripts/shell/install-software.example.sh`](scripts/shell/install-software.example.sh) utiliza variáveis fictícias.

## Validação

As evidências registram:

- conclusão da instalação do software, com avisos;
- conclusão da criação do banco;
- presença do processo PMON;
- CDB/PDB abertas após a atualização;
- aplicação do Release Update pelo OPatch;
- conclusão do datapatch;
- status `SUCCESS` no registro SQL do patch;
- versão final 19.28 do Oracle Database.

### Matriz de evidências

| Marco | Evidência publicada | Critério observado |
|---|---|---|
| Instalação e criação da CDB/PDB | [Instalação silenciosa](images/03-silent-software-installation.png) e [ambiente Multitenant](images/04-multitenant-database-created.png) | Software e banco criados no laboratório. |
| Atualização binária e SQL | [OPatch](images/05-release-update-applied.png) e [datapatch](images/06-datapatch-completed.png) | Release Update aplicado e etapa SQL concluída. |
| Resultado final | [Consulta de validação](images/07-release-update-validation.png) | `CDB_REGISTRY_SQLPATCH` registra `SUCCESS` para o patch `37960098` nos containers 1 e 3; `V$VERSION` mostra Oracle 19c 19.28. |

As imagens comprovam o estado exibido no laboratório. Este projeto não publica medição de duração da mudança, RPO ou RTO; esses indicadores exigem cronologia e teste de recuperação registrados.

## Problemas encontrados

### Ausência de DISPLAY

O instalador gráfico e o DBCA não puderam iniciar porque a sessão não possuía uma variável `DISPLAY` válida.

### Avisos de pré-requisitos

A instalação silenciosa registrou os avisos `INS-32047` e `INS-13014`, relacionados ao inventário central e a requisitos opcionais.

### Componente inválido durante o datapatch

O datapatch indicou um componente SDO inválido na PDB seed e recomendou a recompilação com `utlrp.sql` caso isso provocasse falha. A evidência mostra a conclusão do patch, mas não comprova uma execução posterior do `utlrp.sql`.

## Soluções aplicadas

- substituição do fluxo gráfico pela instalação silenciosa;
- uso do DBCA em modo silencioso;
- atualização do OPatch antes da aplicação do Release Update;
- execução do datapatch após a atualização binária;
- validação no dicionário de dados e em `V$VERSION`.

## Resultados

- Oracle Database 19c instalado e validado;
- arquitetura Multitenant criada e aberta;
- Release Update aplicado ao Oracle Home;
- alterações SQL aplicadas nos containers;
- versão 19.28 confirmada nas evidências.

## Evidências

### Sistema operacional e armazenamento

![Validação do sistema operacional e storage](images/01-os-and-storage-validation.png)

### Instalação do pacote de pré-requisitos

![Instalação do pacote oracle-database-preinstall-19c](images/02-oracle-preinstall-package.png)

### Instalação silenciosa

![Instalação silenciosa do Oracle Database 19c](images/03-silent-software-installation.png)

### Criação do ambiente Multitenant

![Criação da CDB e PDB](images/04-multitenant-database-created.png)

### Aplicação do Release Update

![Release Update aplicado com OPatch](images/05-release-update-applied.png)

### Execução do datapatch

![Conclusão do datapatch nos containers](images/06-datapatch-completed.png)

### Validação final

![Validação do Release Update e da versão](images/07-release-update-validation.png)

## Boas práticas aplicadas

- separação entre software, dados, stage, recuperação e backup;
- instalação silenciosa documentada;
- preservação da arquitetura Multitenant durante a atualização;
- parada controlada do banco antes do patch binário;
- execução do datapatch após a atualização do Oracle Home;
- validação do resultado no dicionário de dados;

## Aprendizados demonstrados

- preparação de Oracle Linux para Oracle Database;
- instalação automatizável do Oracle 19c;
- administração de CDB/PDB;
- atualização do Oracle Home;
- diagnóstico de avisos de instalação e patch;
- validação técnica de uma atualização.

## Autor

Patrick Cecílio

GitHub: [PatrickCecilio](https://github.com/PatrickCecilio)

LinkedIn: a adicionar

## English summary

This repository documents an Oracle Database 19c Multitenant deployment on Oracle Linux, including silent software installation, CDB/PDB creation, Release Update application with OPatch and datapatch, and final validation. All screenshots and examples were sanitized before publication.
