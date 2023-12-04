# Important
This project only accepts **bug issues** and **pull requests**, and does not provide assistance in use  

# Luck System
LucaSystem ~~engine galgame **Emulator**~~  
LucaSystem Engine Parsing Tool

## Usage Instructions: [Usage](Usage.md)
## Plugin Manual: [Plugin](Plugin.md)

## LucaSystem Parsing Progress Completed

### Luca Pak; Packaging Files

- Export completed
- Import completed
    - Only supports replacing file data

### Luca CZImage; Image Files

#### CZ0

- Export completed 32-bit
- Import completed 32-bit

#### CZ1

- Export completed 8-bit
- Import completed 8-bit

#### CZ2

- Export completed 8-bit
- Import completed 8-bit

#### CZ3

- Export completed 32-bit 24-bit
- Import completed 32-bit 24-bit

#### CZ4

- Completed within LucaSystemTools

#### CZ5

- Not encountered (chưa gặp)

### Luca Script; Script Files

- Export completed
- Import completed
- ~~Simple emulation execution~~
- Supports Plugin Extensions（gpython）
  - Non-standard Python, syntax similar to Python 3.4, lacks many built-in libraries and some features, but basic usage is fine
  - Plugin Manual [Plugin](Plugin.md)

#### Notes

Based on time, LucaSystem's script types can be divided into three versions. Currently, only version V3, the latest version, is being studied. LucaSystemTools support script parsing for V2 versions.

| Type  |  Length | Name | Description                                 | 
|-----|-------|-----|------------------------------------|
| uint16 |  2  | len | 	Code length                               |
| uint8 |  1  | opcode | Instruction index                               |
| uint8 |  1  | flag | A flag, values 0~3                        |
| []uint16 |  2 * n  | data0 | Unknown parameters, where n=flag (flag<3), n=2 (flag=3) |
| params |  len -4 -2*n  | params | Parameters                                 |
| uint8 |  k  | align | Alignment padding, where k=len%2                     |

### Luca Font; Font Files

- Parsing completed
- Can be used easily to generate images of specified text
- Export completed
- Import and creation completed

#### info files

- Export completed
- Import completed

### Luca OggPak; Audio Packaging

- Export completed

## Supported Games at Present
1. 《LOOPERS》 Steam
2. LB_EN:《Little Busters! English Edition》 Steam
3. SP:《Summer Pockets》 Nintendo Switch

## Currently supported commands

- MESSAGE (LB_EN、SP、LOOPERS)
- SELECT (LB_EN、SP)
- IMAGELOAD (LB_EN、SP)

- BATTLE (LB_EN)
- EQU
- EQUN
- EQUV
- ADD
- RANDOM
- IFN
- IFY
- GOTO
- JUMP
- FARCALL
- GOSUB

The data for the remaining commands is either unprocessed or not yet parsed.

## Changelog

### 2.2.1 (2023.12.4)
- 支持[CartagraHD](https://vndb.org/r78712)脚本导入导出（未测试）

### 2.2.0 (2023.12.3)
- 支持CZ2的导入（未实际测试）

### 2.1.0 (2023.11.28)
- 支持CZ2的导出

### 2023.10.7
- Supports LOOPERS import and export (tested)
- Supports Plugin extension to accommodate any game
- Built-in SummerPockets (untested) and LOOPERS default Plugin plugins and OPCODE
- Removed emulator-related code

### 6.26
- 完全重构cmd使用方式
  - 暂不支持script脚本的cmd调用
- 支持24位cz3图像，修复缺少Colorblock值导致的错误
- font插入新字符改为追加替换模式，总字符数增加或保持不变

### 3.15
- 修复cz图像导出时alpha通道异常的问题

### 3.11
- 修复script导入导出交互bug
- 测试部分交互
- 新增Usage文档

### 3.03
- 完整的控制台交互接口（未测试）
- 帮助文档

### 2.17
- 统一cz、info、font、pak、script的接口
- 完善测试用例

### 2.10 
- 统一接口规范

### 2.9
- 修复script导入导出中换行、空行的问题
- Merge AEBus pr
  - 1. Fixed situation when LuckSystem would stop parsing scripts after finding END opcode
  - 2. Added handling of TASK, SAYAVOICETEXT, VARSTR_SET opcodes, and fixed handling of BATTLE opcode.
  - 3. Added opcode names for LB_EN, changed first three opcodes to EQU, EQUN, EQUV as specified in LITBUS_WIN32.exe, added handling of these opcodes in LB_EN.go

### 1.25
- 完成pak导入导出交互

### 1.22
- 完成CZ1导入
- 完成CZ0导出导入
- 支持LB_EN BATTLE指令
- 修正PAK文件ID，与脚本中的ID对应
- 更换日志库为glog
- 引入tui库tview

### 1.21
- 完成LZW压缩
- 完成图像拆分算法
- 支持CZ3格式替换图像

### 2022.1.19

- 支持替换pak文件内容并打包
    - 不支持修改文件名和增加文件
- 不再以LucaSystem引擎模拟器为目标，现以替代LucaSystemTools项目为目标

### 8.13

- 项目更名为LuckSystem
    - 目标为实现LucaSystem引擎的模拟器

### 8.12

- 支持字库的加载
    - 字库info文件的解析与应用
    - 字库CZ1图像的解析
- 现已支持根据文字内容，按指定字体生成文字图像

### 8.11

- 支持动态加载pak中的文件
    - 加载pak仅加载pak文件头，内部文件需要时读取
- 支持音频文件的oggpak的解包
- 开始编写CZ图像解析
    - 完成通用lzw解压
    - 支持CZ3图像的加载

### 8.7

- 完美支持脚本导出为文本、导入为脚本
- 开始设计与编写模拟器主体

### 8.3

- 支持pak文件的加载

### 8.1

- 完成大部分导出模式功能
    - 解析文本
    - 合并导出参数和原脚本参数
    - 将文本中的数据合并到原脚本，并转为字节数据

### 7.28

- 完善导出模式，支持更多指令

### 7.27 累积

- 为虚拟机增加导入模式和导出模式
    - 导出模式：不执行引擎层代码，将脚本转为字符串并导出
    - 导入模式：开始设计与编写

### 7.13

- 增加engine结构，即引擎层，与虚拟机做区分
    - 虚拟机：执行脚本内容，保存、计算变量等逻辑相关操作
    - 引擎：执行模拟器的显示、交互等

### 7.12

- 支持表达式计算
    - 表达式的读取以及中缀表达式转后缀表达式
    - 后缀表达式的计算
- 引擎中使用内置数据类型，不在使用包装数据类型

### 7.11

- 重构代码结构，使用vm来处理脚本执行相关
- 增加context，在执行中传递变量表等数据
- 增加变量表，储存运行时变量
- 优化参数的读取
- 统一接口代码，虚拟机与引擎前端交互接口

### 6.30

- 支持多游戏
- 设计参数、函数等结构

### 6.28

- 框架设计与编写
- 第三方包的选择与测试
- 支持LB_EN基本解析

### Plan

- Support script parsing for more games using the LucaSystem engine
- Enhance engine functions
- Initial implementation of engine-level interactions
