![GitHub tag (latest SemVer)](https://img.shields.io/github/tag/zerokol/eFLL.svg)
![GitHub](https://img.shields.io/github/license/zerokol/eFLL.svg)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/cf8ec18693d54d0d9437e4f198339195)](https://www.codacy.com/gh/zerokol/eFLL/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=zerokol/eFLL&amp;utm_campaign=Badge_Grade)
![GitHub top language](https://img.shields.io/github/languages/top/zerokol/eFLL.svg)
![GitHub search hit counter](https://img.shields.io/github/search/zerokol/eFLL/fuzzy.svg)
![GitHub last commit (branch)](https://img.shields.io/github/last-commit/zerokol/eFLL/master.svg)

## eFLL (Embedded Fuzzy Logic Library)

eFLL (Embedded Fuzzy Logic Library) is a standard library for Embedded Systems to implement easy and efficient Fuzzy Systems.

### Highlights

- ✅ **Broad MCU coverage** – validated on Arduino boards and STM32 系列（F0/F1/F4）等 Cortex-M 平台，支持浮点或定点模式。
- ⚙️ **轻量级依赖** – 仅依赖标准 `stdlib.h`，适用于资源受限的裸机或 RTOS 项目。
- 📚 **完善文档** – 在 [`docs/`](./docs) 目录中提供集成指南与行业案例，帮助快速落地。

如需快速在 STM32 或其他 ARM 平台上部署，请参考下文的配置步骤与关键 API 使用范式。

Para informações avançadas, documentação e exemplos de uso em PORTUGUÊS: [eFLL - Uma Biblioteca Fuzzy para Arduino e Sistemas Embarcados](https://blog.alvesoaj.com/2012/09/arduinofuzzy-uma-biblioteca-fuzzy-para.html)

For advanced information, documentation, and usage examples in ENGLISH: [eFLL - A Fuzzy Library for Arduino and Embedded Systems](https://blog.alvesoaj.com/2012/09/arduinofuzzy-fuzzy-library-for-arduino.html)

## Characteristics

Written in C++/C, uses only standard C language library "stdlib.h", so eFLL is a library designed not only to Arduino, but any Embedded System or not how have your commands written in C.

It has no explicit limitations on quantity of Fuzzy, Fuzzy Rules, Inputs or Outputs, these limited processing power and storage of each microcontroller

It uses the process:

(MAX-MIN) and (Mamdani Minimum) for inference and composition, (CENTER OF AREA) to defuzzification in a continuous universe.

Tested with [GTest](http://code.google.com/p/googletest/) for C, Google Inc.

## STM32 / Embedded Integration Quickstart

1. **Fetch sources**：使用 `git submodule` 或直接复制 `src` 文件，将 `Fuzzy*` 源码放入 `Core/Src`（STM32CubeIDE）或其他构建系统目录。
2. **配置编译选项**：
   - 启用 `-std=gnu++11` 及与目标 MCU 匹配的 `-mcpu`/`-mfloat-abi` 参数。
   - 在无 FPU 的设备上，通过宏控制切换到定点缩放模式，具体策略见 [`docs/integration_guide.md`](./docs/integration_guide.md)。
3. **初始化内存**：在系统启动阶段实例化 `Fuzzy`、`FuzzyInput`、`FuzzyOutput` 等对象，将隶属度参数声明为 `const` 或 `constexpr`，保证常量位于 Flash。
4. **周期调用**：在主循环或 RTOS 周期任务中按固定采样周期执行：采集传感器 → 归一化 → `setInput()` → `fuzzify()` → `defuzzify()` → 输出执行器。
5. **线程安全**：若多个任务共享同一 `Fuzzy` 实例，请使用互斥量保护调用序列，详见 [`docs/integration_guide.md`](./docs/integration_guide.md)。

> 传统的通用安装步骤请参考下方 Arduino 部分或直接查看 `Makefile`。

## Key API Usage Patterns

使用 eFLL 进行推理通常遵循以下范式：

```cpp
Fuzzy fuzzy;

// 1. 定义输入、输出与集合
FuzzyInput temperature(1);
temperature.addFuzzySet(&cold);
// ...

// 2. 设置输入数值
fuzzy.setInput(temperature.getId(), currentTemperature);

// 3. 触发推理
fuzzy.fuzzify();

// 4. 获取输出
float duty = fuzzy.defuzzify(heater.getId());
```

- **`setInput(id, value)`**：在每个采样周期调用，为每个输入提供 crisp 值。可以结合平台 HAL 的传感器接口进行归一化。
- **`fuzzify()`**：执行规则匹配、推理与合成，需在所有输入设置完成后调用。
- **`defuzzify(id)`**：获取指定输出的 crisp 结果，可在取值后执行限幅或滤波。

更多集成细节（内存布局、时序设计、线程安全）详见 [`docs/integration_guide.md`](./docs/integration_guide.md)。

## How to install (general use)

**Step 1:** Go to the official project page on GitHub (Here)

**Step 2:** Make a clone of the project using Git or download at Download on the button "Download as zip."

**Step 3:** Clone or unzip (For safety, rename the folder to "eFLL") the files into some folder

**Step 4:** Compile and link it to your code (See Makefile)

## How to install (and import to use with Arduino)

### Easy Way

**Step 1:** Open the Arduino IDE

**Step 2:** In main menu, go to SKETCH >> INCLUDE LIBRARY >> MANAGE LIBRARIES

**Step 3:** Search for "eFLL" or "Fuzzy"

**Step 4:** eFLL will appear in the list, to finish, just click in INSTALL, now you can include eFLL to your sketchs

### Old Way

**Step 1:** Go to the official project page on GitHub (Here)

**Step 2:** Make a clone of the project using Git or download at Download on the button "Download as zip."

**Step 3:** Clone or unzip (For safety, rename the folder to "eFLL") the files into Arduino libraries' folder:

Ubuntu (/usr/share/arduino/libraries/) if installed via apt-get, if not, on Windows, Mac or Linux (where you downloaded the Arduino IDE, the Library folder is inside)

**Ok! The library is ready to be used!**

If the installation of the library has been successfully held, to import the library is easy:

**Step 4:** Open your Arduino IDE, check out the tab on the top menu SKETCH → LIBRARY → Import eFLL

## Brief Documentation

![Class Diagram](https://raw.githubusercontent.com/zerokol/eFLL/master/uml/class-diagram.png)

**Fuzzy object** - This object includes all the Fuzzy System, through it, you can manipulate the Fuzzy Sets, Linguistic Rules, inputs and outputs.

**FuzzyInput** object - This object groups all entries Fuzzy Sets that belongs to the same domain.

**FuzzyOutput** object - This object is similar to FuzzyInput, is used to group all output Fuzzy Sets thar belongs to the same domain.

**FuzzySet** object - This is one of the main objects of Fuzzy Library, with each set is possible to model the system in question. Currently the library supports triangular membership functions, trapezoidal and singleton, which are assembled based on points A, B, C and D, they are passed by parameter in its constructor FuzzySet(float a, float b, float c, float d).

**FuzzyRule** object - This object is used to mount the base rule of Fuzzy object, which contains one or more of this object. Instantiated with FuzzyRule fr = new FuzzyRule (ID, antecedent, consequent).

**FuzzyRuleAntecedent** object - This object is used to compound the object FuzzyRule, responsible for assembling the antecedent of the conditional expression of a FuzzyRule.

**FuzzyRuleConsequent** object - This object is used to render the object FuzzyRule, responsible for assembling the output expression of a FuzzyRule.

## Tips

These are all eFLL library objects that are used in the process. The next step, generally interactive is handled by three methods of the Fuzzy Class first:

`bool setInput(int id, float value);`

It is used to pass the Crispe input value to the system note that the first parameter is the FuzzyInput object' ID which parameter value is intended.

`bool fuzzify();`

It is used to start the fuzzification process, composition and inference.

And finally:

`float defuzzify(int id);`

## REFERENCES

**Authors:** AJ Alves <alvesoaj@icloud.com>; **Co authors:** Dr. Ricardo Lira <ricardor_usp@yahoo.com.br>, Msc. Marvin Lemos <marvinlemos@gmail.com>, Douglas S. Kridi <douglaskridi@gmail.com>, Kannya Leal <kannyal@hotmail.com>

## Special Thanks to Contributors

[@mikebutrimov](https://github.com/mikebutrimov), [@tzikis](https://github.com/tzikis), [@na7an](https://github.com/na7an)

## LICENSE

MIT License
