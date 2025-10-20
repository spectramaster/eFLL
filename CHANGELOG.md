# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.5.0] - 2024-05-09

### Added

- Integration guide covering memory configuration, fixed-point模式、线程安全与时序设计。
- 温控案例文档，包含输入/输出映射、调试流程与注意事项。
- README 中的 STM32 快速配置流程与关键 API 使用范式。

### Changed

- 更新 Highlights，突出 STM32 支持与文档导航。

### Migration

- 若项目依赖旧版 README 的安装说明，请参考新增的 “STM32 / Embedded Integration Quickstart” 中的步骤；原有 Arduino 安装流程保持不变。

## [1.4.1] - 2021-07-06

- Improve fuzzy set calculations

## [1.1.0] - 2019-02-22

### Added

- library.properties file to submit for Arduino Library Defaults.

### Changed

- A complete code review, code formatting, translating and commenting .
- A Bug at FuzzyOutput was found, in the way of calculate builds the points of FuzzyComposition.
- Some methods was renamed and others created (for helps in tests).
- A complete check in tests to ensure library accuracy.

### Removed

- None.

## [1.0.10] - 2017-08-21

### Added

- CHANGELOG.md file to document this project.

### Changed

- The way that FuzzyOutput truncate trapezoidal functions, improve the accuracy.
- File headers with author references.

### Removed

- None.
