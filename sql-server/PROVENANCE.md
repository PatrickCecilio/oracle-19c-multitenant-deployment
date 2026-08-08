# Proveniência dos scripts

Todos os arquivos técnicos abaixo foram copiados sem alteração de conteúdo. Os nomes dos arquivos foram preservados; portanto, cada item é rastreável pela combinação de pasta de destino, origem e nome original.

| Destino | Origem | Arquivos | Competência validada | Código alterado |
| --- | --- | --- | --- | --- |
| `SQL-Agent-Jobs/` | Sql-Server-DBA-Toolbox-main/Agent Jobs | 4 arquivos da pasta de origem | SQL Server Agent e Jobs | Não |
| `Backup-Restore/` | Sql-Server-DBA-Toolbox-main/Backup and Restore | 5 arquivos da pasta de origem | SQL Server Backup e Restore | Não |
| `Backup-Restore/sp_BlitzBackups.sql` | SQL-Server-First-Responder-Kit-dev/sp_BlitzBackups.sql | `sp_BlitzBackups.sql` | SQL Server Backup e Restore | Não |
| `Query-Store/` | Sql-Server-DBA-Toolbox-main/Query Store | 7 arquivos da pasta de origem | Query Store, Execution Plans e performance | Não |
| `Query-Store/` | Sql-Server-DBA-Toolbox-main/Configuration | `Find databases with non-standard Query Store settings.sql`; `Set all databases to have Query Store enabled with query_capture_mode set to Auto.sql` | Query Store | Não |
| `Query-Store/sp_QueryStoreCleanup.sql` | DarlingData-main/sp_QueryStoreCleanup/sp_QueryStoreCleanup.sql | `sp_QueryStoreCleanup.sql` | Query Store, Execution Plans e Statistics | Não |
| `Statistics-Index-Maintenance/` | Sql-Server-DBA-Toolbox-main/Statistics | 3 arquivos da pasta de origem | Statistics | Não |
| `Statistics-Index-Maintenance/` | Sql-Server-DBA-Toolbox-main/Indexing | 7 arquivos de Missing Index, uso de índice, fragmentação e fillfactor | Missing Index; manutenção de índices | Não |
| `Statistics-Index-Maintenance/` | Sql-Server-DBA-Toolbox-main/Configuration | `Set AUTO_CREATE_STATISTICS and AUTO_UPDATE_STATISTICS ON for all databases.sql`; `Set fillfactor to 100.sql` | Statistics; manutenção de índices | Não |
| `Monitoring-Extended-Events/` | Sql-Server-DBA-Toolbox-main/Deadlocks | `List deadlocks using the system_health Extended Events session.sql` | Extended Events, monitoramento e diagnóstico | Não |
| `Monitoring-Extended-Events/` | Sql-Server-DBA-Toolbox-main/Extended Events | 13 arquivos da pasta de origem | Extended Events, monitoramento e diagnóstico | Não |
| `Performance-Execution-Plans/` | Sql-Server-DBA-Toolbox-main/Plan Cache | 9 arquivos da pasta de origem | Execution Plans e performance | Não |
| `Performance-Execution-Plans/` | Sql-Server-DBA-Toolbox-main/Performance | 9 arquivos de análise de carga, cache, consultas, fillfactor e waits | Performance, diagnóstico, Execution Plans e manutenção de índices | Não |

## Evidência curricular

As inclusões foram justificadas exclusivamente por registros explícitos no currículo profissional: “SQL Server Management Studio (SSMS)”, “SQL Server Agent e criação de Jobs”, “SQL Server Backup e Restore”, “Execution Plans, Query Store e Missing Index”, “Statistics e Rebuild/Reorganize Index”, “SQL Server Profiler e Extended Events” e “Replication e monitoramento”. O currículo também registra conhecimentos práticos em “administração, manutenção, performance e diagnóstico em Microsoft SQL Server”.

## Dependências e limites

Alguns scripts podem exigir permissões elevadas, objetos existentes, versões específicas do SQL Server ou ferramentas de seus autores originais. Essas condições não foram removidas, substituídas ou simuladas neste portfólio.
