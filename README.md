# [SoC Lab] Lab1
###### tags: `SoC Lab`, `SOC Design`

| Student ID | Name |
|------------|------|
| 311551095  | 林聖博|

###### 附上此篇Hackmd Link：[https://hackmd.io/@Sheng08/H1hkiwMl6](https://hackmd.io/@Sheng08/H1hkiwMl6)

<!-- [ToC] -->

- [\[SoC Lab\] Lab1](#soc-lab-lab1)
  - [Lab 1](#lab-1)
  - [Tools Install Result](#tools-install-result)
  - [Introduction about the overall system](#introduction-about-the-overall-system)
    - [Step](#step)
    - [系統概述](#系統概述)
  - [Screen dump](#screen-dump)
    - [Screen dump (Other)](#screen-dump-other)
  - [Note](#memo-note)
    - [名詞解釋](#名詞解釋)
    - [觀念補充](#觀念補充)
  - [心得](#心得)
    - [未來預期目標](#未來預期目標)
  - [Troubleshooting](#troubleshooting)
  - [補充](#rocket-補充)


## Lab 1

* https://github.com/bol-edu/course-lab_1

## Tools Install Result
* vitis
    ![](https://i.imgur.com/qFXh02G.png)
* vitis_hls
    ![](https://i.imgur.com/iUrD86s.png)
* vivado
    ![](https://i.imgur.com/2Pp0xJ7.png)

## Introduction about the overall system

從實作 Lab1 過程中，完成跑過整個簡單的實驗流程，可將個階段步驟整合成以下流程：
![](https://i.imgur.com/6Irn9eG.png)


### Step
1. C Simulation:
    * 利用 C/C++ 撰寫欲達成目標邏輯算法 → 運行C Simulation工具 → 驗證程式碼正確性
2. C Synthesis:
    * 選擇合成設置 → 運行合成(Synthesis)工具 → 產生合成後的 report
3. Co-Simulation:
    * 使用C模擬的輸入/輸出 → 運行合成的HDL模擬 → 比較結果
4. Export RTL:
    * 從HLS工具中選擇輸出或導出RTL → 獲得HDL文件
5. Implementation:
    > 在這個階段，HDL 被轉換成 FPGA 或 ASIC 的實際物理結構。
    > 選擇特定的 FPGA/ASIC → 運行 Implementation
    1. Synthesis (within Implementation):
        * 將HDL 轉換為一個 Netlist，這個 Netlist 包含邏輯閘、Flip-flop等之間的連接
    2. Placement:
        * 決定 Netlist 中，每個元件在 FPGA 或 ASIC 物理布局上的位置
    3. Routing:
        * 確定如何在 FPGA 或 ASIC 中連接這些元件，以形成完整的設計
    4. Generate Bit-stream:
        * 創建一個代表整個 FPGA 配置的文件，可被下載到 FPGA 上執行

而 "Export RTL" 後，可進行 Block Design (系統集成)，使用圖形化介面連接不同的 IP 模組，也可以包含從 HLS 或其他來源導入的自定義 IP (Intellectual Property)。

### 系統概述

Lab1 使用的工具為 Vitis-HLS (Vitis High-Level Synthesis)，為基於 C++ 的開發工具，可以高階語言轉換為RTL語言。

使用此設計流程，開發者可直接利用 C/C++ 來描述演算法及Test Bench，執行 C Simulation 驗證設計的正確性。接著，透過高階合成工具，完成 C/C++ 轉換到 RTL。同時也需要考慮延時、時序以及資源的問題。然後，依照 RTL 設計流程，使用軟體工具進行 RTL 電路的合成、佈局以及佈線。最終，在系統層面上對整體設計進行驗證和修正。

![](https://i.imgur.com/6voejSt.png)
> Ref: https://xilinx.eetrend.com/blog/2022/100560242.html

## Screen dump

* Performance
    > C Synthesis
    ![](https://i.imgur.com/sqS0Kq0.png)

    > Co-Simulation
    ![](https://i.imgur.com/yVyeg8U.png)

    > `csynth.rpt`
    ![](https://i.imgur.com/EirFVqX.png)

    > `multip_2num_csynth.rpt`
    ![](https://i.imgur.com/pmOOGKm.png)

* Utilization
    ![](https://i.imgur.com/Upz7vnJ.jpg)

* Interface
    > C Synthesis
    ![](https://i.imgur.com/rWPimgK.png)

    > `multip_2num_csynth.rpt`
    ![](https://i.imgur.com/eMCcTeW.png)

* Co-simulation transcript/waveform
    ![](https://i.imgur.com/GNoYDEA.png)

* Jupyter Notebook execution result
    ![](https://i.imgur.com/Z0SM8eB.png)
    ![](https://i.imgur.com/PMtHHEu.png)
    > 由於截圖範圍大，因此解析度有點損失

### Screen dump (Other)
* C Synthesis
    ![](https://i.imgur.com/GAwRIqQ.png)

    ![](https://i.imgur.com/AHpbIWl.png)

    ![](https://i.imgur.com/7NjhZy7.png)

* Block Design
    ![](https://i.imgur.com/CUzzBjV.png)

    ![](https://i.imgur.com/qJ59LrN.jpg)

* Implement Design
    ![](https://i.imgur.com/uXLM9O7.png)

    ![](https://i.imgur.com/cYhjIWg.jpg)

## :memo: Note

### 名詞解釋

* **C Simulation:**
    - 目的: 驗證C/C++的算法邏輯正確性
    - 說明: 為初步的模擬階段，只涉及C/C\++程式，**不涉及任何硬體描述語言**。基本上，這階段確保C/C\++程式在未進行任何合成或硬體轉換的情況下運行正確性
    - 結果: 若發現錯誤，需要於這階段修改C/C++程式
* **C Synthesis:**
    - 目的: 將轉換C/C++到硬體描述語言 (HDL)，如 VHDL 或 Verilog。
    - 說明: 高階合成 (HLS) 工具會嘗試**將C/C\++程式碼轉化成硬體結構**。在這階段，可能需要進行優化或改善，以達到所需的**clock速度、資源使用或功耗**
    - 結果: 得到合成後的報告(`csynth.rpt`)，包括估計的資源使用、性能(Performance)等
* **Co-Simulation:**
    - 目的: 驗證合成後的HDL程式碼與C/C++原始碼的**行為**是否一致。
    - 說明: 在這階段，原始C/C++程式碼和其合成的HDL程式碼將**同時模擬**。這是確保合成後的HDL實現與原始演算邏輯在功能上保持一致
    - 結果: 若發現差異或問題，需回到C Synthesis階段，調整優化指令或更改C/C++程式，然後重新合成和模擬
* **Export RTL:**
    - 目的: 生成代表特定設計的硬體描述語言 (HDL)，如 VHDL 或 Verilog。
    - 說明: 在完成高階合成 (HLS) 後，已從其C/C++ 轉換為硬體結構。而 "Export RTL" 將硬體結構生成為可用於標準 FPGA 或 ASIC toolchain 的 HDL 程式碼的過程
    - 結果: 生成的 HDL 可以被整合到更大的系統中，或者直接使用在 FPGA 或 ASIC 設計中
* **IP (Intellectual Property):**
    - 目的: 封裝特定的設計或功能以便**重複使用**
    - 說明: IP 是指一個已經設計好的、可以重複使用的功能或子系統。IP 可為自己設計，亦可為第三方供應商提供。當設計被合成並驗證無誤後，可被封裝成一個 IP，以便在其他設計中重複使用或共享
    - 結果: 生成的 IP 模組通常以標準格式提供，如：Xilinx 的 AXI4 接口，允許被整合到更大的系統中

### 觀念補充
1.  在 HLS 中，"pragmas" 和 "directives" 通常指同樣的東西，都是用於指導合成過程的指令。允許優化和指導合成過程，而不必大量修改 C/C++ 的原始碼。
2.  reg (register) 為存儲單元。在 FPGA 設計中，register 是用來保存狀態或資料的基本元件，由`flip-flops`構成。
    * `regIP = ol.multip_2num_0`，代表 IP 核心的物件，可通過 Python 存取的 register
        * 每個在 FPGA overlay 中的 IP 都可利用其名稱於 Python 存取和控制
3. `Overlay`: 將 bitstream 載入到 FPGA，並將相應的硬體設計在 FPGA 上執行
    * MMIO (Memory-Mapped I/O): 允許 CPU 使用常規的記憶體存取指令，如：load 和 store
        * 特定 I/O 裝置的 register 或控制位會**映射(mapping)到系統的主記憶體地址空間中的某些位置**
    * 在 PYNQ 環境中，利用 MMIO 直接訪問 FPGA 上的 MMIO 地址空間，能夠讀取與寫入 FPGA 中的硬體 register
    * 使用 Overlay 載入 FPGA 設計時，各個 IP 核心可能有屬於自己的 MMIO 地址範圍，允許從 Python 存取和控制這些核心

## 心得
由於為資工系的學生，之前並未有機會接觸到SoC的相關知識和設計。在這學期的SoC課程中，首次深入接觸SoC的設計與相關知識。
於Lab1實作過程中，遇到了許多我不熟悉的專業名詞和實作原理。為了更好掌握這些新知識，因此投入時間學習和理解。而透過Lab1的初步實作更讓我體驗到工具的操作，如: vitis_hls，這都使我對SoC有了更深入的了解，從而加深了我的理解與實踐能力。

而本次實驗過程中，對於 OnlineFPGA 的租借機制與 infra 感到非常驚艷，利用所提供的平台服務，可快速建立驗證環境，並且可不侷限於理設備的實體實驗限制與配置的複雜性，利用遠端方式就可以快速完成 FPAG 的設計與驗證以及測試。

### 未來預期目標
- 目前可能有些名詞用語不夠精準，期望後續能逐漸了解其意義，並正確的使用名詞說明實作過程的體悟與想法

---

## Troubleshooting
* None


## :rocket: **補充**
後續更多資訊會補充於以下HackMD
* https://hackmd.io/@Sheng08/HkzeBuDyp


:pushpin: **TODO**
- [ ] 了解 ASIC
- [ ] 了解 正反器(Flip-flop) 與查找表(LUT)


<!-- ---

 ## 疑問

1. 若有 slack / time violation，可調整 period?
2. ap_ctrl_none 作用
3. 注意：若前面有跑 cosimulation 的話記得先還原成原本的 directive，重新Synthesis 再匯出 RTL。 為何
4. vivado 是EDA?
5. block design作用 又用到ipynb
6. Zynq == fpga?
7. 合成 (綜合) -->

<!-- * Synthesis
* directive
* C Simulation
* C Synthesis
* Co-Simulation
* IP
* HDL Wrapper
* bit-stream file 類似韌體執行檔？
* Placement
* Routing EDA?
* Routing design
* Implementation
* hw_handoff
* regIP 作用等
* vivado
* vitis
* vitis_hls -->