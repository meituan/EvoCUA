<div align="center">

# EvoCUA: Evolving Computer Use Agent

**🥇 OSWorld 开源模型 No.1 | 擅长计算机操作的通用多模态大模型**

[![Model](https://img.shields.io/badge/🤗%20HuggingFace-EvoCUA--32B-blue)](https://huggingface.co/meituan/EvoCUA-32B-20260105)
[![Model](https://img.shields.io/badge/🤗%20HuggingFace-EvoCUA--8B-blue)](https://huggingface.co/meituan/EvoCUA-8B-20260105)
[![OSWorld Score](https://img.shields.io/badge/OSWorld-56.7%25-brightgreen)](https://os-world.github.io/)
[![License](https://img.shields.io/badge/License-Apache%202.0-orange)](./LICENSE)

[English](./README.md) | [中文](./README_CN.md)

<img src="assets/images/osworld_leaderboard.png" width="700" alt="OSWorld Leaderboard">

**🥇 开源模型第一 | OSWorld 排行榜（2026年1月）**

</div>

---

## 📢 更新日志

- **2026.01.13**：发布 [EvoCUA-8B-20260105](https://huggingface.co/meituan/EvoCUA-8B-20260105) — OSWorld 得分 **46.06%**，**以更小的参数量实现与 72B 级别开源模型相当的性能！** 🆕
- **2026.01.05**：发布 [EvoCUA-32B-20260105](https://huggingface.co/meituan/EvoCUA-32B-20260105)，OSWorld 得分 **56.7%**，登顶**开源模型榜首** 🥇

---

## 🌟 亮点

- 🥇 **开源模型榜首**：在 OSWorld 上达到 **56.7%** 的任务完成率，位居**所有开源模型第一**。
- 📈 **性能显著提升**：相比 OpenCUA-72B 提升 11.7%（45.0%→56.7%），相比 Qwen3-VL Thinking 提升 15.1%（41.6%→56.7%），且**参数更少、步数减半**。
- 🖥️ **端到端多轮自动化**：仅凭屏幕截图和自然语言指令，即可流畅操作 Chrome、Excel、PPT、VSCode 等各类软件，完成复杂任务。
- 🧠 **创新训练范式**：采用独特的数据合成与训练方法，在大幅提升 Computer Use 能力的同时，保持了模型的通用多模态能力。

---

## 📊 性能对比

| 排名 | 模型 | 开源/闭源 | 类型 | 最大步数 | 得分 |
|------|------|-----------|------|----------|------|
| 1 | Claude-sonnet-4-5 | 🔒 闭源 | 通用模型 | 100 | 62.9% |
| 2 | Seed-1.8 | 🔒 闭源 | 通用模型 | 100 | 61.9% |
| 3 | Claude-sonnet-4-5 | 🔒 闭源 | 通用模型 | 50 | 58.1% |
| **4** | **EvoCUA-20260105 (Ours)** | **🟢 开源** | **通用模型** | **50** | **56.7% 🥇** |
| 5 | DeepMiner-Mano-72B | 🔒 闭源 | 专用模型 | 100 | 53.9% |
| 6 | UI-TARS-2-2509 | 🔒 闭源 | 通用模型 | 100 | 53.1% |
| 7 | EvoCUA (Previous) | 🔒 闭源 | 通用模型 | 50 | 50.3% |
| **8** | **EvoCUA-8B-20260105 (Ours)** | **🟢 开源** | **通用模型** | **50** | **46.1%** |
| 9 | OpenCUA-72B | 🟢 开源 | 专用模型 | 100 | 45.0% |
| ... | ... | ... | ... | ... | ... |
| 13 | Qwen3-VL-Flash | 🔒 闭源 | 通用模型 | 100 | 41.6% |

> EvoCUA 高居**开源模型第一**，仅需 **50 步**即可达到极具竞争力的效果。尽管如此，人类水平仍显著高于当前最佳模型，该领域仍有巨大的探索与提升空间。

---

## 🚀 快速开始

### 安装环境

推荐使用 Python 3.12。

```bash
git clone https://github.com/meituan/EvoCUA.git
cd EvoCUA
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 模型下载与部署

请先从 HuggingFace 下载 EvoCUA 模型权重，并使用 **vLLM** 将其部署为兼容 OpenAI 接口的推理服务。

推荐版本：
- torch: 2.8.0+cu126
- transformers: 4.57.3
- vllm: 0.11.0

```bash
# 1) 下载模型权重
huggingface-cli download meituan/EvoCUA-32B-20260105 \
  --local-dir /path/to/EvoCUA-32B \
  --local-dir-use-symlinks False

# 2) 启动 vLLM 推理服务（建议使用单独的环境）
vllm serve /path/to/EvoCUA-32B \
  --served-model-name EvoCUA \
  --host 0.0.0.0 \
  --port 8080 \
  --tensor-parallel-size 2

# 3) 设置环境变量
# 环境变量可通过 .env 文件进行配置（请参考 env.template）：
cp env.template .env
# 编辑 .env 文件，填入您的具体配置，例如：
export OPENAI_API_KEY="dummy"
export OPENAI_BASE_URL="http://127.0.0.1:8080/v1"
```

### 在 OSWorld 上运行评测

```bash
python3 run_multienv_evocua.py \
  --headless \
  --provider_name aws \
  --observation_type screenshot \
  --model EvoCUA-S2 \
  --result_dir ./evocua_results \
  --test_all_meta_path evaluation_examples/test_nogdrive.json \
  --max_steps 50 \
  --num_envs 30 \
  --temperature 0.01 \
  --max_history_turns 4 \
  --coordinate_type relative \
  --resize_factor 32 \
  --prompt_style S2
```

---

## 📁 项目结构

```
EvoCUA/
├── run_multienv_evocua.py      # 评测入口（支持多环境并行）
├── lib_run_single.py           # 单任务 Rollout 执行逻辑（含轨迹记录、截图、录屏、评分）
├── lib_results_logger.py       # 评测结果实时汇总（写入 results.json）
├── desktop_env/                # OSWorld 环境端实现
│   ├── providers/              # 虚拟机提供商接口（AWS/VMware/Docker 等）
│   ├── controllers/            # 环境控制器
│   └── evaluators/             # 任务评估器
├── mm_agents/
│   └── evocua/                 # EvoCUA Agent 核心代码（Prompt构建、输出解析、动作生成）
└── evaluation_examples/        # OSWorld 任务配置集
```

---


## 📖 关于 OSWorld

[OSWorld](https://os-world.github.io/) 是 Computer Use Agent 领域最具影响力的基准测试，被 **OpenAI、Anthropic、字节跳动 Seed、月之暗面、智谱 AI、阶跃星辰** 等众多顶尖 AI 团队广泛采用。OSWorld 通过模拟真实桌面环境中的多轮交互，全面评估 Agent 完成实际计算机任务的能力。

---

## 🔗 相关资源

- 🤗 **模型权重**：
  - [meituan/EvoCUA-32B-20260105](https://huggingface.co/meituan/EvoCUA-32B-20260105) - OSWorld 得分：**56.7%** 🥇
  - [meituan/EvoCUA-8B-20260105](https://huggingface.co/meituan/EvoCUA-8B-20260105) - OSWorld 得分：**46.06%** 🆕
- 📊 **OSWorld 基准测试**：[os-world.github.io](https://os-world.github.io/)
- 📄 **技术报告**：即将发布，敬请期待！
- 🚀 **更多规格**：更多尺寸的模型正在路上！

---

## 🙏 致谢

我们诚挚感谢开源社区在 Computer Use Agent 领域的杰出贡献。特别感谢 **Xinyuan Wang**（[OpenCUA](https://github.com/xlang-ai/OpenCUA)）和 **Tianbao Xie**（[OSWorld](https://github.com/xlang-ai/OSWorld)）在评测、技术探讨及反馈方面提供的宝贵支持，他们的开创性工作极大地启发并推动了本项目的发展。我们致力于回馈社区，并将持续开源研究成果，与大家共同推动领域进步。

---

## 📝 引用

如果您觉得 EvoCUA 对您的研究有帮助，请考虑引用：

```bibtex
@misc{evocua2026,
  title={EvoCUA: Evolving Computer Use Agent},
  author={Chong Peng* and Taofeng Xue*},
  year={2026},
  url={https://github.com/meituan/EvoCUA},
  note={* Equal contribution}
}
```

---

## 📜 开源协议

本项目基于 Apache 2.0 协议开源，详见 [LICENSE](./LICENSE) 文件。

---

<div align="center">

**由美团 LongCat 团队用 ❤️ 打造**

</div>
