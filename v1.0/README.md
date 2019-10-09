## 说明
当前最新版本为nanoDAP-wl v2.0。
![nanoDAP-wl-v1.0-a](https://github.com/wuxx/nanoDAP-wireless/blob/master/v1.0/doc/nanoDAP-wl-v1.0-a.jpg)
![nanoDAP-wl-v1.0-b](https://github.com/wuxx/nanoDAP-wireless/blob/master/v1.0/doc/nanoDAP-wl-v1.0-b.jpg)

nanoDAP-wl v1.0 版本硬件开源，供爱好者自行制作研究学习。  
感兴趣的朋友请加入QQ群 698319017 交流学习。
另外剩余部分PCB单板，需要的朋友请在群内索取，剩余PCB数目不多，只需出PCB成本+运费即可。
固件针对个人开放，每人限两套，请在群内向群主索取即可（请提供芯片ID，群主会生成好镜像发给你）。

如何获取芯片ID：

```
void stm32f1_get_cpuid(void)
{
      uint32_t idcode[3];
      idcode[0] = *(volatile uint32_t*)(0x1FFFF7E8);
      idcode[1] = *(volatile uint32_t*)(0x1FFFF7EC);
      idcode[2] = *(volatile uint32_t*)(0x1FFFF7F0);
      printf("0x%08x 0x%08x 0x%08x", idcode[0], idcode[1], idcode[2]);
}
```

## 对比
特性|v1.0 | v2.0|说明|
----|----|----|-----|
传输速度 | 10KB/s | 40KB/s |v2.0使用性能更强的esp8266实现无线传输，当前传输速度为保守速度，还可进一步优化|
虚拟串口 | 不支持  | 支持|由于v1.0采用nrf24l01模块进行无线传输，实现虚拟串口难度较高，故v1.0暂时不支持虚拟串口|
传输距离 | 5m    | 10m|v2.0无线模块带25db PA，实际上传输距离更远，信号质量也更稳定|
供电 | 100mA   | 300mA | v2.0无线模块需要较高的启动电流|
传输建立时间| 2s   | 5s|v2.0考虑启动电源稳定性，wifi搜索配对时间，当前建立传输时间为5s左右|
