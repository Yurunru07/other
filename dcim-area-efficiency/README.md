# DCIM 面积效率改进提案

[下载并打开带图 HTML](index.html) · [原始 HTML 文件](https://raw.githubusercontent.com/Yurunru07/other/main/dcim-area-efficiency/index.html)

这是单文件、离线可用的中文汇报。下载 index.html 后用浏览器打开即可；右上角可打印或另存为 PDF。页面包含交互容量计算器，无外部图片、脚本或字体依赖。

## 两项改进

1. **权重 SRAM 存 FP4，读取后转 INT5。** 每个权重由 5 bit 降到 4 bit，数据容量减少 20%。按当前存储模型的配置，32 个双 PE 的 bank 总数可由 160 降为 128；容量由 160 降为 128 KiB。
2. **输出 SRAM 存 INT14 块部分和，读取后恢复 FP32。** 基线为每个 32 元素块存一项 FP32 部分和。改存 INT14 补码后，部分和数据位减少 56.25%；若按 INT16 对齐，则减少 50%。

每块必须保留对应 scale，读取后恢复 FP32 块结果，并保持原四块归约的顺序与数值规则。独立保存原始 A/W scale 时，报告示例中的输出总位容量降幅分别为 30.86%（INT14）和 24.61%（INT16）。

## 报告内容

- Native v1 阵列组织及 MXFP4 / INT8 输出格式；
- 两项改动的数据通路前后对比；
- 存储位宽、字宽对齐和 scale 元数据测算；
- 可调整输入的交互容量计算器；
- INT8 兼容、译码复制、读侧吞吐和 SRAM 宏映射条件；
- 后续数值等价和面积效率验证计划。

这些数字是数据容量估算，尚未作为综合后面积、功耗或吞吐实测结果验收。两项百分比不可直接相加。实际芯片面积收益需计入 SRAM 外围、scale 保留、译码器和控制逻辑，并在相同频率与吞吐下比较。

## 可用于 PPT 的矢量插图

| 文件 | 内容 |
|---|---|
| [concept.svg](figures/concept.svg) | 建议数据流概念图 |
| [before_after.svg](figures/before_after.svg) | 权重与输出路径前后对比 |
| [output_bits.svg](figures/output_bits.svg) | 每列四块结果 128 → 56 bit |
| [capacity_comparison.svg](figures/capacity_comparison.svg) | 默认联合数据容量示例 |

报告生成于 2026-10-29；基线说明及理论容量计算已核对，桌面和移动端已进行显示检查。共享设计 RTL 未修改。
