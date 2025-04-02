# Travelport Insights Production Modules

This document provides a comprehensive listing of all production modules in the Travelport Insights codebase. Only modules marked as [P] (Production) in the full ModuleCategorization document are included here.

Each production module is essential for the core functioning of Travelport Insights in production environments.

## Core Application Files

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `src/insights/__init__.py` | Package initialization | Initializes the Insights package, setting up import paths and exposing public interfaces. Establishes version information and core module exports. | Core | 100% | 0 | - |
| `src/insights/__main__.py` | Entry point for running as a module | Enables the package to be run directly with `python -m insights`. Parses command-line arguments and routes to the appropriate functionality. | Core | 100% | 0 | - |
| `src/insights/main.py` | Main application entry point | Primary entry point that initializes the application context, sets up logging, loads configurations, and starts the core processing pipeline. | Core | 100% | 0 | - |
| `insights.py` | Core application functionality | Provides high-level API for interacting with the Insights platform. Contains factory methods for creating processors and data sources. | Core | 100% | 0 | - |
| `src/insights/config/__init__.py` | Configuration management | Initializes the configuration system, providing access to configuration values across the application. | Core | 100% | 0 | - |

## Project Configuration and Setup

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `config/production.yaml` | Production configuration | Contains all production-specific configuration values including paths, system settings, logging configuration, and performance parameters. | Configuration | 100% | 0 | - |
| `config/logging_config.yaml` | Logging configuration | Defines the logging structure for the application, including log levels, formats, and rotation policies for production use. | Configuration | 100% | 0 | - |
| `config/paths.yaml` | Path configuration | Specifies all file system paths used by the application for data processing, outputs, and temporary storage. | Configuration | 100% | 0 | - |

## Core Processing Modules

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `src/insights/core/__init__.py` | Core package initialization | Initializes the core processing package and exposes the main processing interfaces. | Processing | 100% | 0 | - |
| `src/insights/core/batch.py` | Batch definition and management | Defines the Batch data structure used for processing data in manageable chunks. Includes metadata tracking and state management. | Processing | 100% | 0 | - |
| `src/insights/core/batch_processor.py` | Processes data in batches | Implements the batch processing logic that handles data in chunks for efficient memory usage and parallel processing capabilities. | Processing | 100% | 0 | - |
| `src/insights/core/km.py` | Knowledge management core | Implements the Knowledge Management system for storing and retrieving processing rules and business logic. | Processing | 100% | 0 | - |
| `src/insights/core/km_handler.py` | Knowledge management handler | Handles loading, validation, and application of knowledge base rules to incoming data. | Processing | 100% | 0 | - |
| `src/insights/core/pipeline.py` | Data processing pipeline | Defines the processing pipeline that coordinates the flow of data through various processing steps. | Processing | 100% | 0 | - |
| `src/insights/core/processor.py` | Base processor implementation | Provides the abstract base class that all data processors must implement, ensuring consistent interfaces. | Processing | 100% | 0 | Various |
| `src/insights/core/system_processor.py` | System-specific processing | Contains base classes for system-specific processing implementations that extend the core processor. | Processing | 100% | 0 | - |

## Data Management Modules

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `src/insights/data/__init__.py` | Data package initialization | Initializes the data management subsystem and exposes data handling interfaces. | Data | 100% | 0 | - |
| `src/insights/data/data_access.py` | Data access layer | Provides a unified interface for accessing data from various sources with appropriate caching and optimization. | Data | 90% | 4 | - |
| `src/insights/data/data_source.py` | Data source base class | Defines the abstract interface that all data sources must implement for consistent data access. | Data | 95% | 2 | - |
| `src/insights/data/data_source_manager.py` | Manages data sources | Coordinates multiple data sources, handles failover, and provides aggregation capabilities. | Data | 90% | 4 | - |
| `src/insights/data/loader.py` | Data loading functionality | Handles loading data from various formats and sources with appropriate validation and transformation. | Data | 90% | 3 | - |
| `src/insights/data/saver.py` | Data saving functionality | Manages saving processed data to various destinations with appropriate formatting and error handling. | Data | 100% | 0 | - |
| `src/insights/data/transformer.py` | Data transformation | Contains utilities for transforming data between different formats and structures. | Data | 100% | 0 | - |
| `src/insights/data/types.py` | Data type definitions | Defines data structures and types used throughout the data processing pipeline. | Data | 95% | 1 | - |
| `src/insights/data/validator.py` | Data validation | Provides validation rules and utilities to ensure data integrity before and after processing. | Data | 100% | 0 | - |

### Data Exporters and Sources

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `src/insights/data/exporters/__init__.py` | Exporters package | Initializes the exporters subsystem for outputting data in various formats. | Data | 100% | 0 | - |
| `src/insights/data/exporters/csv_exporter.py` | CSV export functionality | Implements export capability to CSV format with appropriate formatting and optimization. | Data | 100% | 0 | - |
| `src/insights/data/exporters/exporter_base.py` | Base exporter functionality | Defines the common interface and shared functionality for all data exporters. | Data | 100% | 0 | - |
| `src/insights/data/exporters/json_exporter.py` | JSON export functionality | Implements export capability to JSON format with appropriate formatting options and performance optimizations. | Data | 100% | 0 | - |
| `src/insights/data/sources/__init__.py` | Data sources package | Initializes the data sources subsystem for reading data from various sources. | Data | 100% | 0 | - |
| `src/insights/data/sources/local_data_source.py` | Local data source | Implements data access from local file systems with appropriate caching and optimization strategies. | Data | 100% | 0 | - |
| `data_source_factory.py` | Factory for creating data sources | Provides factory methods to create appropriate data source instances based on configuration. | Data | 100% | 0 | - |

## Maintenance and Tools

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `src/insights/tools/__init__.py` | Tools package initialization | Initializes the maintenance tools package and exposes tool interfaces. | Maintenance | 100% | 0 | - |
| `src/insights/tools/file_scheduler.py` | Schedules and manages file operations | Handles scheduling and coordination of file operations for data processing with appropriate locking. | Maintenance | 90% | 3 | - |
| `src/insights/tools/daily_maintenance.py` | Daily maintenance operations | Implements routine maintenance tasks like log rotation, temporary file cleanup, and health checks. | Maintenance | 90% | 4 | - |
| `src/insights/tools/manage_logs.py` | Manages log files | Handles log file rotation, archiving, and cleanup to prevent disk space issues in production. | Maintenance | 100% | 0 | - |
| `src/insights/tools/run_daily.py` | Runs daily operations | Coordinates and executes the daily processing jobs according to schedules and dependencies. | Maintenance | 90% | 4 | - |
| `scripts/health_check.py` | System health check | Comprehensive health check script that validates system status, connectivity, and resource availability. | Maintenance | 100% | 0 | - |

## Utility Modules

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `src/insights/utils/__init__.py` | Utilities package initialization | Initializes the utilities package and exposes common utility functions. | Utility | 100% | 0 | - |
| `src/insights/utils/error_manager.py` | Error management utilities | Provides standardized error handling, logging, and recovery mechanisms. | ErrorHandling | 100% | 0 | - |
| `src/insights/utils/file_utils.py` | File utility functions | Contains utilities for file operations, path management, and file system interactions. | Utility | 100% | 0 | - |
| `src/insights/utils/logging_utils.py` | Logging utilities | Implements enhanced logging capabilities with context tracking and performance monitoring. | Utility | 100% | 0 | - |
| `src/insights/utils/validation.py` | Validation utilities | Contains common validation functions used across the application. | Utility | 100% | 0 | - |

## Monitoring and Status Modules

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `src/insights/monitoring/__init__.py` | Monitoring package initialization | Initializes the monitoring subsystem that tracks system health and performance. | Monitoring | 100% | 0 | - |
| `src/insights/monitoring/alert_manager.py` | Manages system alerts | Implements alert generation, filtering, and notification for system events and anomalies. | Monitoring | 100% | 0 | - |
| `src/insights/monitoring/alerts.py` | Alert definitions and utilities | Defines alert types, thresholds, and handling logic for different system conditions. | Monitoring | 100% | 0 | - |

## Command Line Interface Modules

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `src/insights/cli/__init__.py` | CLI package initialization | Initializes the command-line interface subsystem. | CLI | 100% | 0 | - |
| `src/insights/cli/base.py` | Base CLI functionality | Provides the foundation for command-line interface components with argument parsing. | CLI | 100% | 0 | - |
| `src/insights/cli/commands.py` | Command definitions | Defines the available CLI commands and their parameters. | CLI | 100% | 0 | - |
| `src/insights/cli/main.py` | Main CLI entry point | Primary entry point for the command-line interface that handles command routing and execution. | CLI | 100% | 0 | - |
| `src/insights/cli/main_cli.py` | Main CLI implementation | Implements the main CLI functionality including command parsing and dispatch. | CLI | 100% | 0 | - |
| `src/insights/cli/plugin_base.py` | Base plugin functionality | Defines the plugin architecture for extending CLI capabilities. | CLI | 100% | 0 | - |

## System-Specific Modules

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `src/insights/systems/__init__.py` | Systems package initialization | Initializes the system-specific processing modules. | SystemSpecific | 100% | 0 | - |
| `src/insights/systems/processor_base.py` | Base system processor | Defines the abstract interface that all system-specific processors must implement. | SystemSpecific | 100% | 0 | - |
| `src/insights/systems/pcg.py` | PCG system processor | Implements processing for the PCG system with specific business rules and transformations. | SystemSpecific | 100% | 0 | `processor_base` |
| `src/insights/systems/pre.py` | PRE system processor | Implements processing for the PRE system with specific business rules and transformations. | SystemSpecific | 100% | 0 | `processor_base` |
| `src/insights/systems/vss.py` | VSS system processor | Implements processing for the VSS system with specific business rules and transformations. | SystemSpecific | 100% | 0 | - |

## Reporting Modules

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `src/insights/reporting/__init__.py` | Reporting package initialization | Initializes the reporting subsystem for generating business intelligence outputs. | Reporting | 100% | 0 | - |
| `src/insights/reporting/export.py` | Report export functionality | Handles exporting reports to various formats with appropriate formatting and structure. | Reporting | 100% | 0 | - |
| `src/insights/reporting/generator.py` | Report generation | Implements core report generation logic with templating and data aggregation capabilities. | Reporting | 100% | 0 | - |
| `src/insights/reporting/report_exporter.py` | Report export manager | Coordinates export of reports to multiple formats and destinations. | Reporting | 100% | 0 | - |
| `src/insights/reporting/report_generator.py` | Report generator implementation | Provides concrete implementations of report generation with specific formatting and rules. | Reporting | 100% | 0 | - |
| `src/insights/reporting/types.py` | Report type definitions | Defines data structures for report specifications and outputs. | Reporting | 100% | 0 | - |

## Input/Output Modules

| Module | Description | Detailed Purpose | Category | Completion&nbsp;% | Hours | Dependencies |
|:-------|:------------|:-----------------|:---------|:-----------------:|:-----:|:-------------|
| `src/insights/io/__init__.py` | IO package initialization | Initializes the input/output subsystem for standardized data access. | IO | 100% | 0 | - |
| `src/insights/io/extractors.py` | Data extraction utilities | Implements data extraction from various structured and unstructured sources. | IO | 100% | 0 | - |
| `src/insights/io/formatters.py` | Data formatting utilities | Provides formatting capabilities for different output requirements. | IO | 100% | 0 | - |
| `src/insights/io/parsers.py` | Data parsing utilities | Contains parsers for various data formats with appropriate validation. | IO | 100% | 0 | - |
| `src/insights/io/readers.py` | Data reading utilities | Implements optimized readers for different file formats and data sources. | IO | 100% | 0 | - |
| `src/insights/io/writers.py` | Data writing utilities | Provides writers for outputting data to various destinations and formats. | IO | 100% | 0 | - |

## Notes on Production Modules

- All modules listed here are essential for production operation of Travelport Insights.
- These modules have been validated for production use and security compliance.
- Modules not listed here (those marked as [D] Development or [O] Optional in the full categorization) are not required for core production functionality.
- All listed production modules have reached 100% completion status or have minimal remaining work clearly identified.
- The modules' organization follows the standard Python package structure to ensure maintainability and clear dependencies.

## Connection to Deployment Pipeline

These production modules are deployed through the CD Pipeline as described in the CI/CD Guide, following these steps:

1. Building a Python wheel package containing these modules
2. Deploying to Test environment for initial validation
3. Deploying to Staging with smoke tests
4. Deploying to Production with full health checks
5. Post-deployment validation using health check scripts

For the complete deployment process, please refer to the Production Deployment Guide.
