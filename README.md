# GPU UNLOCK — RTX 3070 on Windows

**Русский** · [English](#english)

Полная скорость тензорных ядер (FP16 / BF16 / TF32) на **GeForce RTX 3070 под Windows**.
NVIDIA урезает её на GeForce вдвое. Эта сборка снимает ограничение при загрузке с флешки, до запуска Windows.
Драйвер не патчится, BIOS видеокарты не прошивается.

Это **модифицированная версия** [impacifist/Windows-GPU-UNLOCK](https://github.com/impacifist/Windows-GPU-UNLOCK),
Windows-порта [Zagitta/gpu-unlock](https://github.com/Zagitta/gpu-unlock). Все заслуги за сам анлок принадлежат им.
Здесь добавлено:

- поддержка **RTX 3070 (PCI `10de:2484`, 8 GB)** в сборщике локального профиля (чип GA104, тот же, что у уже проверенной 3070 Ti);
- `-Card 3070` в бенчмарке, чтобы можно было замерить скорость до и после;
- пошаговый гайд для Windows на русском и английском.

## Результат (проверено на одной машине)

Zotac RTX 3070 (`10de:2484`, subsystem `19da:2617`) · Gigabyte B460M D2VX SI (BIOS FE) · i5-10400F (без iGPU) · драйвер 617.42 · cuBLAS 12.6, GEMM 4096×4096, медиана трёх прогонов:

| Режим | До | После | |
|---|---:|---:|---:|
| FP16 Tensor, FP32 accumulate | 38.7 TFLOPS | **73.4 TFLOPS** | **+90%** |
| BF16 Tensor, FP32 accumulate | 38.8 TFLOPS | **73.4 TFLOPS** | **+89%** |
| TF32 Tensor | 18.1 TFLOPS | **38.1 TFLOPS** | **+110%** |
| FP32 (контроль, не должен меняться) | 13.5 TFLOPS | 12.9 TFLOPS | шум |

В реальных задачах, по данным Zagitta: Stable Diffusion или ComfyUI в FP16/BF16 быстрее на 30–55%, prefill в LLM на 25–55%, LoRA-обучение на 25–50%.
**Игры, генерация токенов и квантованные GGUF (llama.cpp, Ollama) не ускоряются.**

👉 **Гайд: [docs/RTX3070.ru.md](docs/RTX3070.ru.md)**

## ⚠️ Прочитай перед запуском

- Анлок **проверен на одной машине**. Скрипты собирают профиль под твою конкретную машину и прогоняют около 19 проверок, но это всё ещё эксперимент.
- Анлок **сбрасывается** при перезагрузке, сне и выключении. Его нужно делать при каждой холодной загрузке с флешки.
- Если что-то пошло не так, достаточно выключить ПК кнопкой и загрузиться без флешки. Карта вернётся в заводское состояние, потому что прошивка видеокарты не меняется.
- **Готовых `.EFI` и прошивок NVIDIA здесь нет и не будет.** Каждый собирает сам из исходников. Если кто-то предлагает готовый «инсталлер GPU unlock», не запускай его.
- Всё делаешь на свой риск. Гарантий никаких (GPL-2.0, раздел NO WARRANTY).

---

## English

Full-rate tensor cores (FP16 / BF16 / TF32) on a **GeForce RTX 3070 under Windows**. NVIDIA caps them at half rate on GeForce;
this removes the cap at boot from a USB stick, before Windows starts. No patched driver, no VBIOS flash.

A **modified version** of [impacifist/Windows-GPU-UNLOCK](https://github.com/impacifist/Windows-GPU-UNLOCK), the Windows port of
[Zagitta/gpu-unlock](https://github.com/Zagitta/gpu-unlock). All credit for the unlock itself goes to them. This repo adds:

- **RTX 3070 (PCI `10de:2484`, 8 GB)** admission in the local-profile generator (GA104, same chip as the reviewed 3070 Ti profile);
- `-Card 3070` for the before/after benchmark;
- a step-by-step Windows guide.

Results on one machine (table above): FP16/BF16 **38.8 → 73.4 TFLOPS (+89–90%)**, TF32 **18.1 → 38.1 (+110%)**, FP32 control unchanged.

👉 **Guide: [docs/RTX3070.en.md](docs/RTX3070.en.md)**

**Warnings:** hardware-tested on one machine only. The unlock resets on reboot, sleep and power-off. To recover, power off and boot without the stick.
No prebuilt EFI binaries or NVIDIA firmware are distributed here, build from source. No warranty.

---

Original project documentation: [README.upstream.md](README.upstream.md) · License: [GPL-2.0-only](LICENSE) · Third-party notices: [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)
