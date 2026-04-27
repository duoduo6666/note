如果你说的是 **“现代还能用的 BIOS 中断文档/资料”**，那要先分清两件事：

> **现代 PC 已经主要是 UEFI，不再推荐把“BIOS 中断”当主接口。**  
> 传统 `INT 10h / INT 13h / INT 15h / INT 16h` 这套，**只在 Legacy BIOS / CSM / 实模式阶段**还有意义。([OSDev Wiki](https://wiki.osdev.org/Porting_to_UEFI?utm_source=chatgpt.com "Porting to UEFI - OSDev Wiki"))

---

# 结论先说

## 1）如果你要查“传统 BIOS 中断”

最权威、最经典的还是：

### ① Ralf Brown’s Interrupt List（RBIL）—— **事实上的圣经**

- 收录了大量 **实模式 BIOS 中断 / DOS 中断 / 端口 / CMOS / 硬件信息**
    
- OSDev 明确说它是一个关于 **Real Mode BIOS interrupt calls** 的大型汇编资料库
    
- **最后正式版本是 v61，更新停在 2000 年 7 月 16 日**（很老，但仍然是最全的）([OSDev Wiki](https://wiki.osdev.org/Ralf_Brown%27s_Interrupt_List?utm_source=chatgpt.com "Ralf Brown's Interrupt List - OSDev Wiki"))
    

**适合：**

- `INT 10h` 显示
    
- `INT 13h` 磁盘
    
- `INT 15h` E820 / 系统服务
    
- `INT 16h` 键盘
    
- `INT 1Ah` RTC/时钟
    

---

### ② HelpPC（你刚看的这个）

- 更像 **“快速查表版”**
    
- 没 RBIL 全，但**更容易看**
    
- 很适合先查调用格式，再去 RBIL 看边角兼容性
    

**建议用法：**

- **先 HelpPC 看调用格式**
    
- **再 RBIL 看兼容性/坑点**
    

---

### ③ OSDev Wiki（现代 OS 开发视角）

它不是“中断号大全”，但很适合你这种做底层的人：

- BIOS boot 流程
    
- 实模式限制
    
- `INT 13h` 磁盘访问
    
- `INT 15h, E820h`
    
- 切保护模式/长模式后怎么办
    
- UEFI 替代方案
    

OSDev 也明确提醒：  
**BIOS 适合早期引导，真正的 OS 最终应该自己驱动硬件，不要长期依赖 BIOS。**([OSDev Wiki](https://wiki.osdev.org/Real_Mode_OS_Warning?utm_source=chatgpt.com "Real Mode OS Warning - OSDev Wiki"))

---

# 2）如果你说“现代 BIOS 中断”——其实应该看 **UEFI 文档**

现代机器大多数是 **UEFI**，不是传统 BIOS。  
OSDev 说明：UEFI 已经接管了 PC 固件职责，**CSM（兼容传统 BIOS）并不可靠，也不未来可持续**。([OSDev Wiki](https://wiki.osdev.org/Porting_to_UEFI?utm_source=chatgpt.com "Porting to UEFI - OSDev Wiki"))

所以：

- **传统 BIOS 中断文档** → 看 **RBIL / HelpPC**
    
- **现代固件开发文档** → 看 **UEFI Specification**
    

---

# 最推荐的“现代可用资料组合”

## A. 查传统 BIOS 中断（bootloader / 实模式）

**优先级：**

1. **HelpPC**（快查）
    
2. **Ralf Brown’s Interrupt List**（最全）
    
3. **OSDev Wiki 对应专题**（现代视角+坑）
    

---

## B. 做现代 PC 引导 / 真机兼容

**优先级：**

1. **UEFI Specification**
    
2. **EDK II 文档**
    
3. **OSDev UEFI / Porting to UEFI**
    
4. 只把 BIOS 中断当“兼容层知识”
    

---

# 对你最有用的现实建议（很重要）

你这种喜欢底层、可能会碰 bootloader / 模拟器的人，我建议你这样分：

### 如果你要写 **16 位 boot sector / stage1**

看：

- `INT 10h`
    
- `INT 13h`
    
- `INT 16h`
    
- `INT 15h E820`
    

### 如果你要写 **现代真机可启动 loader**

尽量学：

- **UEFI Boot Services**
    
- **GOP（替代 INT 10h 文本/显示）**
    
- **EFI_SIMPLE_FILE_SYSTEM_PROTOCOL**
    
- **EFI_BLOCK_IO_PROTOCOL**
    

因为现代机器上：

- 传统文本模式不一定有
    
- CSM 可能没有
    
- `INT 10h/13h` 可能根本不可用（纯 UEFI）
    

OSDev 明确提到：  
UEFI **不支持传统 CGA text mode**，要改用图形模式。([OSDev Wiki](https://wiki.osdev.org/Porting_to_UEFI?utm_source=chatgpt.com "Porting to UEFI - OSDev Wiki"))

---

# 一句话总结

## 你要的“现代 BIOS 中断文档”其实分两类：

- **传统 BIOS 中断文档（经典）**  
    → **RBIL + HelpPC + OSDev**
    
- **现代 PC 固件接口（真正现代）**  
    → **UEFI Spec + EDK II + OSDev UEFI**
    

---

# 我给你的直接推荐（按实用度）

### 传统 BIOS 中断：

- **HelpPC**：快速查
    
- **Ralf Brown’s Interrupt List**：最全、最权威（但老）([OSDev Wiki](https://wiki.osdev.org/Ralf_Brown%27s_Interrupt_List?utm_source=chatgpt.com "Ralf Brown's Interrupt List - OSDev Wiki"))
    
- **OSDev Wiki**：适合现代 OSDev 理解([OSDev Wiki](https://wiki.osdev.org/Porting_to_UEFI?utm_source=chatgpt.com "Porting to UEFI - OSDev Wiki"))
    

### 现代固件：

- **UEFI Specification**
    
- **EDK II**
    
- **OSDev Porting to UEFI**([OSDev Wiki](https://wiki.osdev.org/Porting_to_UEFI?utm_source=chatgpt.com "Porting to UEFI - OSDev Wiki"))
    

---

如果你愿意，我下一条可以直接给你整理一份：

## **“现代最有用的 BIOS/UEFI 文档清单（附学习顺序）”**

我可以按你这种风格专门排成：

**16位 bootloader → E820 → 切保护模式 → UEFI 入门 → 兼容真机** 的路线图。