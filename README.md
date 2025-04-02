# Travelport Insights Production Flow Chart

This document provides a flow chart of the production execution paths in Travelport Insights, showing command execution, module dependencies, and processing flows.

## Entry Point Commands

```mermaid
flowchart TD
    CLI["Command Line Interface<br/><code>$ python -m insights</code>"] --> MainEntry["src/insights/__main__.py"]
    CLI2["Daily Processing<br/><code>$ insights-daily</code>"] --> DailyMaint["src/insights/tools/daily_maintenance.py"]
    CLI3["System Monitoring<br/><code>$ insights monitor</code>"] --> MonitorCommand["src/insights/cli/commands.py<br/>monitor_command()"]
    CLI4["Report Generation<br/><code>$ python -m insights.reporting</code>"] --> ReportingMain["src/insights/reporting/__main__.py"]
    CLI5["Data Processing<br/><code>$ insights --system apo --date 20230101</code>"] --> MainCLI["src/insights/cli/main_cli.py"]
```

## Main Application Flow

```mermaid
flowchart TD
    MainEntry["src/insights/__main__.py"] --> MainModule["src/insights/main.py"]
    MainModule --> ConfigLoader["src/insights/config/__init__.py<br/>load_config()"]
    MainModule --> ErrorHandlers["src/insights/utils/error_handlers.py"]
    ConfigLoader --> ProcessorInit["src/insights/core/processor.py<br/>initialize_processor()"]
    
    ProcessorInit --> CoreProcessor["src/insights/core/processor.py<br/>Processor class"]
    CoreProcessor --> Pipeline["src/insights/core/pipeline.py<br/>ProcessingPipeline"]
    
    Pipeline --> BatchProcessor["src/insights/core/batch_processor.py<br/>BatchProcessor"]
    BatchProcessor --> Batch["src/insights/core/batch.py<br/>Batch class"]
    
    CoreProcessor --> SystemProcessor["src/insights/core/system_processor.py<br/>SystemProcessor"]
    
    subgraph DataAccess["Data Access Layer"]
        DataSourceManager["src/insights/data/data_source_manager.py<br/>DataSourceManager"]
        DataSource["src/insights/data/data_source.py<br/>DataSource"]
        LocalDataSource["src/insights/data/sources/local_data_source.py<br/>LocalDataSource"]
        DataSourceFactory["data_source_factory.py<br/>create_data_source()"]
    end
    
    CoreProcessor --> DataSourceFactory
    DataSourceFactory --> DataSourceManager
    DataSourceManager --> DataSource
    DataSource --> LocalDataSource
```

## System-Specific Processing

```mermaid
flowchart TD
    SystemProcessor["src/insights/core/system_processor.py<br/>SystemProcessor"] --> ProcessorBase["src/insights/systems/processor_base.py<br/>BaseProcessor"]
    
    ProcessorBase --> APO["src/insights/systems/apo/processor.py<br/>APOProcessor"]
    ProcessorBase --> PCG["src/insights/systems/pcg/processor.py<br/>PCGProcessor"]
    ProcessorBase --> PRE["src/insights/systems/pre/processor.py<br/>PREProcessor"]
    
    APO --> KMHandler["src/insights/core/km_handler.py<br/>KMHandler"]
    PCG --> KMHandler
    PRE --> KMHandler
    
    KMHandler --> KM["src/insights/core/km.py<br/>KnowledgeManagement"]
```

## Data Transformation and Validation

```mermaid
flowchart TD
    BatchProcessor["src/insights/core/batch_processor.py"] --> DataLoader["src/insights/data/loader.py<br/>DataLoader"]
    BatchProcessor --> Transformer["src/insights/data/transformer.py<br/>transform()"]
    BatchProcessor --> Validator["src/insights/data/validator.py<br/>validate()"]
    
    Transformer --> DataTypes["src/insights/data/types.py<br/>DataTypes"]
    Validator --> DataTypes
    
    BatchProcessor --> Saver["src/insights/data/saver.py<br/>save()"]
    
    Saver --> Exporters["src/insights/data/exporters"]
    Exporters --> ExporterBase["src/insights/data/exporters/exporter_base.py<br/>ExporterBase"]
    ExporterBase --> CSVExporter["src/insights/data/exporters/csv_exporter.py<br/>CSVExporter"]
    ExporterBase --> JSONExporter["src/insights/data/exporters/json_exporter.py<br/>JSONExporter"]
```

## Reporting and Output Generation

```mermaid
flowchart TD
    ReportingMain["src/insights/reporting/__main__.py"] --> ReportGenerator["src/insights/reporting/report_generator.py<br/>ReportGenerator"]
    ReportGenerator --> ReportTypes["src/insights/reporting/types.py<br/>ReportTypes"]
    ReportGenerator --> Generator["src/insights/reporting/generator.py<br/>generate()"]
    
    ReportGenerator --> Exporter["src/insights/reporting/report_exporter.py<br/>ReportExporter"]
    Exporter --> Export["src/insights/reporting/export.py<br/>export_report()"]
    
    Export --> IOWriters["src/insights/io/writers.py<br/>write_file()"]
    Export --> IOFormatters["src/insights/io/formatters.py<br/>format_output()"]
```

## Monitoring and Maintenance

```mermaid
flowchart TD
    MonitorCommand["src/insights/cli/commands.py<br/>monitor_command()"] --> Monitor["src/insights/monitoring/status_monitor.py<br/>StatusMonitor"]
    
    Monitor --> AlertManager["src/insights/monitoring/alert_manager.py<br/>AlertManager"]
    AlertManager --> Alerts["src/insights/monitoring/alerts.py<br/>Alerts"]
    
    DailyMaint["src/insights/tools/daily_maintenance.py"] --> RunDaily["src/insights/tools/run_daily.py<br/>run_daily()"]
    DailyMaint --> ManageLogs["src/insights/tools/manage_logs.py<br/>rotate_logs()"]
    
    RunDaily --> FileScheduler["src/insights/tools/file_scheduler.py<br/>FileScheduler"]
```

## Error Handling Flow

```mermaid
flowchart TD
    ErrorHandlers["src/insights/utils/error_handlers.py"] --> ErrorDecorator["@with_error_handling<br/>decorators"]
    ErrorHandlers --> ErrorContext["with error_context()<br/>context managers"]
    
    ErrorDecorator --> ErrorManager["src/insights/utils/error_manager.py<br/>handle_error()"]
    ErrorContext --> ErrorManager
    
    ErrorManager --> ErrorCorrelation["src/insights/utils/error_correlation.py<br/>correlate_errors()"]
```

## Production Execution Examples

### Daily Processing Workflow

1. System automatically executes the daily processing job:
   ```bash
   insights-daily
   ```

2. The `daily_maintenance.py` module loads and executes:
   - Rotating log files
   - Cleaning temporary files
   - Running the scheduler for file operations

3. The system then executes the main processing for each system:
   ```bash
   python -m insights --system apo --date yesterday
   python -m insights --system pcg --date yesterday
   python -m insights --system pre --date yesterday
   ```

4. Each processing job:
   - Loads the appropriate configuration
   - Initializes the core processor
   - Creates the processing pipeline
   - Loads data from the appropriate sources
   - Processes data in batches
   - Applies knowledge management rules
   - Saves outputs to the configured locations

5. After processing, generates reports:
   ```bash
   python -m insights.reporting --type daily
   ```

6. Finally, performs health checks:
   ```bash
   python scripts/health_check.py
   ```

## Configuration Dependencies

All modules depend on the configuration subsystem:

```mermaid
flowchart TD
    Config["src/insights/config/__init__.py<br/>load_config()"] --> ProdConfig["config/production.yaml"]
    Config --> LoggingConfig["config/logging_config.yaml"]
    Config --> PathsConfig["config/paths.yaml"]
```

## Key Utilities Used Throughout the System

```mermaid
flowchart TD
    Utils["src/insights/utils/"] --> ErrorHandling["src/insights/utils/error_handling.py"]
    Utils --> Validation["src/insights/utils/validation.py"]
    Utils --> FileUtils["src/insights/utils/file_utils.py"]
    Utils --> LoggingUtils["src/insights/utils/logging_utils.py"]
    Utils --> CircuitBreaker["src/insights/utils/circuit_breaker.py"]
```

## Notes on Production Module Flow

- The entry points shown are the primary production execution paths
- All modules are connected through the central configuration system
- Error handling is pervasive throughout all modules using the standard framework
- Data flows from sources through processors and transformers to exporters
- System-specific processors inherit from the base processor but implement custom logic
- The monitoring subsystem runs in parallel to the main processing flow
- Maintenance tools operate on a schedule to ensure system health
