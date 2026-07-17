# Procedimento técnico

## 1. Validação inicial

O trabalho começou com a inspeção dos filesystems, hostname e versão do sistema operacional. A evidência confirma Oracle Linux Server 8.10 e volumes separados para software/dados.

Evidência: `images/01-os-and-storage-validation.png`.

## 2. Pré-requisitos

O pacote `oracle-database-preinstall-19c` foi instalado para preparar dependências e parâmetros exigidos pelo Oracle Database 19c.

Evidência: `images/02-oracle-preinstall-package.png`.

## 3. Estrutura de diretórios

Foram preparados diretórios separados para:

- Oracle Base e Oracle Home;
- inventário central;
- datafiles;
- arquivos de stage e patches;
- backup;
- Fast Recovery Area.

Os nomes específicos do ambiente original não são reproduzidos neste repositório.

## 4. Instalação silenciosa

A tentativa de iniciar o instalador gráfico falhou por ausência de `DISPLAY`. O fluxo foi alterado para `runInstaller -silent`, usando o response file padrão do Oracle Home e a opção `INSTALL_DB_SWONLY`.

O instalador concluiu com avisos:

- `INS-32047`: inventário central não vazio;
- `INS-13014`: requisitos opcionais não atendidos.

A evidência registra a conclusão do setup e os scripts de root solicitados.

Evidência: `images/03-silent-software-installation.png`.

## 5. Criação da CDB/PDB

O DBCA gráfico também exigia `DISPLAY`. O banco foi criado em modo silencioso como container database, com uma pluggable database.

Após a criação:

- a entrada foi registrada em `/etc/oratab`;
- o processo PMON foi verificado;
- a PDB foi aberta e teve seu estado salvo.

Senhas e identificadores do banco não foram mantidos nas evidências públicas.

Evidência: `images/04-multitenant-database-created.png`.

## 6. Atualização do OPatch

O utilitário OPatch foi atualizado antes da aplicação do Release Update. A evidência original registrou a versão 12.2.0.1.47.

## 7. Aplicação do Release Update

O banco foi encerrado de forma controlada antes do patch binário. O Release Update 19.28 foi aplicado ao Oracle Home e o OPatch concluiu com sucesso.

Evidência: `images/05-release-update-applied.png`.

## 8. Datapatch

Depois da inicialização do banco, o datapatch atualizou os componentes SQL no CDB root, PDB seed e PDB de aplicação.

Foi registrado um aviso de componente SDO inválido na seed. A saída recomenda `utlrp.sql` caso o estado inválido provoque falha. As evidências não comprovam a execução dessa recompilação e, por isso, ela não é apresentada como etapa concluída.

Evidência: `images/06-datapatch-completed.png`.

## 9. Validação final

A validação consultou:

- `CDB_REGISTRY_SQLPATCH`, para confirmar ação e status do patch;
- `V$VERSION`, para confirmar a versão do Oracle Database;
- `SHOW PDBS`, para verificar a abertura dos containers.

O registro do patch aparece com status `SUCCESS` e a versão 19.28 é apresentada na saída final.

Evidência: `images/07-release-update-validation.png`.

## Observações

- Este material não substitui a documentação oficial da Oracle.
- Antes de atualizar outro ambiente, devem ser avaliados pré-requisitos, conflitos, espaço, backup, plano de rollback e notas do patch correspondente.
- Os exemplos não contêm senhas, IPs, hostnames ou nomes reais do ambiente.
