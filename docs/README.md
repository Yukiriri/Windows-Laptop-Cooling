# Windows-Cooling-Tuning
一套游戏本温度压制操作，适用于Windows  
如果你想，台式也可以用  
  
由于笔记本CPU设计的冗余电压给的很大，再加上出厂即灰烬，散热技术也没有新突破，三管齐下就轻松顶着90度跑了  
想要可观降低发热，就需要把达到边际效应的频率给取舍掉，也就是损失性能换温度  
如果你的笔记本不支持限制功率，或者不想用工具限制，就可以用这套操作  

> [!IMPORTANT]
> 全文提到的cmd命令都需要管理员身份运行  

> [!TIP]
> 修改电源计划之前，推荐把电源计划重置默认  
> 也推荐使用`平衡`电源计划  

> [!NOTE]
> 仓库的`tools`文件夹里有写好的一键脚本版  

# CPU温度压制
cmd命令一览：
- 设置插电CPU频率上限
    ```
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "75b0ae3f-bce0-45a7-8c89-c9611c25e100" 3000
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "75b0ae3f-bce0-45a7-8c89-c9611c25e101" 3000
    powercfg -SetAcValueIndex SCHEME_CURRENT SUB_PROCESSOR "75b0ae3f-bce0-45a7-8c89-c9611c25e102" 3000
    ```
- 设置离电CPU频率上限
    ```
    powercfg -SetDcValueIndex SCHEME_CURRENT SUB_PROCESSOR "75b0ae3f-bce0-45a7-8c89-c9611c25e100" 3000
    powercfg -SetDcValueIndex SCHEME_CURRENT SUB_PROCESSOR "75b0ae3f-bce0-45a7-8c89-c9611c25e101" 3000
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

> [!NOTE]
> 影响温度的不是只有频率，也有硅脂老化的情况  
> 如果压频率无效，就该考虑换硅脂了  

> [!TIP]
> 可以写进bat更方便修改和运行  

> [!NOTE]
> 运行一次即整个电源计划生效，不需要加入开机自启  
> 除非切换电源计划，才需要给新电源计划重新运行一次  

# GPU温度压制
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

> [!IMPORTANT]
> 仅限NVIDIA  
> 还没研究过A卡相关的工具套件，但用[MSI小飞机](https://www.msi.com/Landing/afterburner/graphics-cards)也可以实现A卡限制频率  

> [!NOTE]
> 这个修改仅够维持本次开机，如果要永久生效，需要加入开机自启  
> 严格来说只维持本次驱动会话，驱动状态更改后就被重置  
