# Windows-Laptop-Cooling
一套游戏本温度压制操作，适用于Windows  
如果你想，台式也可以用  

> [!IMPORTANT]
> 全文提到的cmd命令都需要管理员身份运行  

> [!TIP]
> 修改电源计划之前，推荐把电源计划重置默认  

# CPU温度压制
笔记本CPU唯一能控制的只有限制最高频率  
由于笔记本CPU设计的冗余电压给的很大，再加上出厂即灰烬，散热技术也没有新突破，三管齐下就轻松顶着90度跑了  
现在要把达到边际效应的频率给取舍掉，才能可观降低发热  

> [!NOTE]
> 影响温度的不是只有频率，也有硅脂老化的情况  
> 如果压频率无效，就该考虑换硅脂了  

cmd命令一览：
- 设置插电CPU频率上限
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "75b0ae3f-bce0-45a7-8c89-c9611c25e100" 3000
    ```
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "75b0ae3f-bce0-45a7-8c89-c9611c25e101" 3000
    ```
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "75b0ae3f-bce0-45a7-8c89-c9611c25e102" 3000
    ```
- 设置离电CPU频率上限
    ```
    powercfg -SetDcValueIndex SCHEME_CURRENT SUB_PROCESSOR "75b0ae3f-bce0-45a7-8c89-c9611c25e100" 3000
    ```
    ```
    powercfg -SetDcValueIndex SCHEME_CURRENT SUB_PROCESSOR "75b0ae3f-bce0-45a7-8c89-c9611c25e101" 3000
    ```
    ```
    powercfg -SetDcValueIndex SCHEME_CURRENT SUB_PROCESSOR "75b0ae3f-bce0-45a7-8c89-c9611c25e102" 3000
    ```
- 让修改生效
    ```
    powercfg -SetActive SCHEME_CURRENT
    ```
其中，`3000`代表最高上限，单位为`MHZ`  
如果要恢复为无上限，改成`0`  

> [!NOTE]
> `3000`只是个参考，每个人的机器的发热和散热能力都不同，按照你能接受的温度范围调整  
> 调大则增加性能上限，也增加发热和功耗，反之亦然  

> [!TIP]
> 可以写进bat更方便修改和运行  

> [!NOTE]
> 运行一次即整个电源计划生效，不需要加入开机自启  
> 除非切换电源计划，才需要给新电源计划重新运行一次  

# GPU温度压制
笔记本GPU也没有什么可以调整的空间，还是老套路，限制最高频率  

> [!IMPORTANT]
> 仅限NVIDIA  
> 还没研究过A卡相关的工具套件，但用[MSI小飞机](https://www.msi.com/Landing/afterburner/graphics-cards)也可以实现A卡限制频率  

cmd命令一览：
- 设置GPU核心频率上限
    ```
    nvidia-smi -lgc 0,1500
    ```
- 恢复默认
    ```
    nvidia-smi -rgc
    ```
其中，`1500`代表最高上限，单位为`MHZ`  
还是老套路，按照能接受的温度范围调整  

> [!NOTE]
> 这个修改仅够维持本次开机，如果要永久生效，需要加入开机自启  
> 严格来说只维持本次驱动会话，驱动状态更改后就被重置  

# CPU频率压制
让Windows重新接管CPU调频，闲时能下降到更低频率  
能略微提升一些节能，也能减少一些不必要的高频带来的发热  
  
cmd命令一览：
- 处理器性能自主模式
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "8baa4a8a-14c6-4451-8e8b-14bdbd197537" 0
    ```
- 最小处理器状态
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "893dee8e-2bef-41e0-89c6-b55d0929964c" 0
    ```
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "893dee8e-2bef-41e0-89c6-b55d0929964d" 0
    ```
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "893dee8e-2bef-41e0-89c6-b55d0929964e" 0
    ```
- 处理器性能内核休止分配阈值
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "4bdaf4e9-d103-46d7-a5f0-6280121616ef" 50
    ```
- 处理器性能提高阈值
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "06cadf0e-64ed-448a-8927-ce7bf90eb35d" 50
    ```
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "06cadf0e-64ed-448a-8927-ce7bf90eb35e" 50
    ```
- 处理器性能降低阈值
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "12a0ab44-fe28-4fa9-b3bd-4b64f44960a6" 90
    ```
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "12a0ab44-fe28-4fa9-b3bd-4b64f44960a7" 90
    ```
- 处理器性能提升策略
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "465e1f50-b610-473a-ab58-00d1077dc418" 1
    ```
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "465e1f50-b610-473a-ab58-00d1077dc419" 1
    ```
- 处理器性能降低策略
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "40fbefc7-2e9d-4d25-a185-0cfd8574bac6" 1
    ```
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "40fbefc7-2e9d-4d25-a185-0cfd8574bac7" 1
    ```

> [!NOTE]
> 这些修改并不影响最高性能上限  

> [!NOTE]
> 这些修改作用的是插电电源计划
