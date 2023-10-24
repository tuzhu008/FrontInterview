# 自动化测试

前端测试主要分为 3 种：单元测试（Unit Test）、集成测试（Integration Test）、UI 测试（UI Test）

## 单元测试

测试工具 Mocha, Ava, Karma, Jest, Jasmine 等。

### 主流测试工具比较

| 框架 | 断言 | 仿真 | 快照 | 异步测试 |
|-----|-----|-----|-----|-----|
| Mocha | 默认不支持，可配置 | 默认不支持，可配置	 | 默认不支持，可配置	 | 友好 |
| Ava | 默认支持	| 不支持，需第三方配置 | 默认支持	| 友好 |
| Jasmine | 默认支持 | 默认支持	 | 默认支持	 | 不友好 |
| Jest | 默认支持	 | 默认支持	 | 默认支持	 | 友好 |
| Karma | 不支持，需第三方配置 | 不支持，需第三方配置	 | 不支持，需第三方配置	 | 不支持，需第三方配置 |

## 集成测试

集成测试（Integration Test）和 UI 测试（UI Test）有 ReactTestUtils, Test Render, Enzyme, React-Testing-Library, Vue-Test-Utils 等。

## UI 测试

## 测试思想

### TDD：Test-Driven Development（测试驱动开发）

TDD：Test-Driven Development（测试驱动开发）：TDD 则要求在编写某个功能的代码之前先编写测试代码，然后只编写使测试通过的功能代码，通过测试来推动整个开发的进行

### BDD：Behavior-Driven Development（行为驱动开发）

BDD：Behavior-Driven Development（行为驱动开发）：BDD 可以让项目成员（甚至是不懂编程的）使用自然语言来描述系统功能和业务逻辑，从而根据这些描述步骤进行系统自动化的测试



