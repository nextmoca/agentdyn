## About this branch (`next-moca`)

This is the `next-moca` branch of `nextmoca/agentdyn`, a fork of [SaFo-Lab/AgentDyn](https://github.com/SaFo-Lab/AgentDyn). `main` mirrors upstream unchanged.

What this branch adds, on top of upstream commit `5353cf76`:

- A `next_moca` defense (`src/agentdojo/agent_pipeline/next_moca.py`, wired in `agent_pipeline.py`). The element is copied unchanged from [nextmoca/agentdojo](https://github.com/nextmoca/agentdojo/tree/next-moca). It sits after the tool executor, sends each batch of tool results to the hosted Next Moca service, and passes on the context the service returns. On a timeout, an HTTP error, a malformed response or a server pass-through directive, the model sees the original tool output unchanged. The element is transport only: it does not score, rank or rewrite tool output itself.

This branch is the code behind the published GPT-4o AgentDyn result (`shopping`, `github` and `dailylife`, `important_instructions`, 560 attacked tasks per arm, November 2026). Results, run configuration and reproduction steps are in [context-selection-bench](https://github.com/nextmoca/context-selection-bench), under `csbench/suites/agentdyn`.

To run the defense: set `NEXT_MOCA_API_KEY` (and optionally `NEXT_MOCA_URL`, `NEXT_MOCA_OPERATING_POINT`), then pass `--defense next_moca` to `python -m agentdojo.scripts.benchmark`.

AgentDyn is the work of Hao Li, Ruoyao Wen, Shanghao Shi, Ning Zhang, Yevgeniy Vorobeychik and Chaowei Xiao, built on AgentDojo, and released under the MIT License (see `LICENSE`). Please cite their paper, below, when you use the benchmark.

---

# AgentDyn: Are Your Agent Security Defenses Deployable in Real-World Dynamic Environments?

[Hao Li](https://leolee99.github.io/), [Ruoyao Wen](https://github.com/ruoyaow/), [Shanghao Shi](https://shishishi123.github.io/), [Ning Zhang](https://cybersecurity.seas.wustl.edu/index.html), [Yevgeniy Vorobeychik](https://vorobeychik.com/), [Chaowei Xiao](https://xiaocw11.github.io/).

The official implementation of the paper "[AgentDyn: Are Your Agent Security Defenses Deployable in Real-World Dynamic Environments?](https://arxiv.org/pdf/2602.03117)".

AgentDyn is a dynamic, open-ended agent security benchmark featuring 60 challenging open-ended user tasks and 560 injection test cases across the Shopping, GitHub, and Daily Life scenarios. It is built on top of the [AgentDojo](https://github.com/ethz-spylab/agentdojo) framework. A huge thanks to the AgentDojo team for their admirable contribution to the community!

## Quickstart

```bash
pip install -e .
```


## Running the benchmark

For adaptability, we support an evaluation script same as AgentDojo's. Documentation on how to use the script can be obtained with the `--help` flag.

For example, to run the `shopping` suite , with `gpt-4o-2024-08-06` as the LLM, the tool filter as a defense, and the attack with important_instructions, run the following command:

```bash
python -m agentdojo.scripts.benchmark -s shopping \
    --model GPT_4O_2024_08_06 \
    --defense tool_filter --attack important_instructions
```

To run with external defenses integrated in this repo layout, you can directly use:


Before running, please export your API key, through:

1. OpenAI Model: export OPENAI_API_KEY=XXX
2. Google Model: export GOOGLE_API_KEY=XXX
3. Open-sourced Models (Qwen, LlaMA, other supported models): export OPENROUTER_API_KEY=XXX

## Supported settings

#### Available Suites:
AgentDyn supports `shopping`,`github`, and `dailylife` suites, as well as the original four suites from AgentDojo (`banking`,`slack`, `travel` and `workspace`).

#### Available Models: 
We evaluate the following models in our paper: ``GPT_4O_MINI_2024_07_18``, ``GPT_4O_2024_08_06``, ``GEMINI_2_5_FLASH``, ``GEMINI_2_5_PRO``, ``LLAMA_3_3_70B``, ``QWEN3_235B``, ``GPT_5_1_2025_11_13``, ``GPT_5_MINI_2025_08_07``. 
Other models supported by AgentDojo are also compatible.

#### Available Defenses: 
In addition to the original defenses in AgentDojo, we provide support for [PIGuard](https://aclanthology.org/2025.acl-long.1468.pdf) and [PromptGuard2](https://huggingface.co/meta-llama/Llama-Prompt-Guard-2-86M). We also support directly invoking external defenses in this workspace: [CaMeL](https://github.com/google-research/camel-prompt-injection), [Progent](https://github.com/sunblaze-ucb/progent), [DRIFT](https://github.com/SaFo-Lab/DRIFT),.

The complete list of defenses supported in our paper includes: ``repeat_user_prompt``, ``spotlighting_with_delimiting``, ``tool_filter``, ``transformers_pi_detector``, ``piguard_detector``, ``prompt_guard_2_detector``, ``camel``,  ``progent``, ``drift``.


## Inspect Results

To review the results reported in our paper, please refer to the log files in the ``(runs/)``.

## References

If you find this work useful in your research or applications, we appreciate that if you can kindly cite:

```
@articles{AgentDyn,
  title={AgentDyn: Are Your Agent Security Defenses Deployable in Real-World Dynamic Environments?},
  author={Hao Li and Ruoyao Wen and Shanghao Shi and Ning Zhang and Yevgeniy Vorobeychik and Chaowei Xiao},
  journal = {arXiv},
  eprint = {2602.03117},
  year={2026}
}
```
