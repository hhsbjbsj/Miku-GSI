# Miku UI Android 16 GSI (ARM64 A/B) - Treble Fixed Edition

[![GitHub Release](https://img.shields.io/github/v/release/hhsbjbsj/Miku-GSI?style=for-the-badge&color=blue)](https://github.com/hhsbjbsj/Miku-GSI/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/hhsbjbsj/Miku-GSI/total?style=for-the-badge&color=green)](https://github.com/hhsbjbsj/Miku-GSI/releases)

这是基于 **Miku UI (Android 16 / Baklava 25Q2)** 构筑并深度适配老旧高通架构（MSM8937 / SM6125 等）的通用系统镜像 (Generic System Image)。
经过针对 Adreno GPU 驱动、SELinux、Codec2 与 Treble 底层引导套件的全面工程修复，彻底解决了在特定机型上由于 Vulkan 驱动不稳定而引发的**开机进桌面反复软重启**等核心痛点。

---

## 📱 实机验证通过环境

- **基准验证设备**: OPPO A57 (高通 MSM8937 / MSM8940)
- **内核要求**: 搭载后移 Linux 5.4 BPF 子系统的 4.9 内核（Android 16 强依赖 eBPF 机制）
- **实测表现**: 开机秒进第二屏并顺利进入系统桌面，OpenGL ES / HWUI 硬件加速流畅，无反复软重启，无崩溃异常。

---

## 🛠️ 核心修复与技术特性

1. **Adreno 610 / 505 GPU 防崩溃与防软重启优化**
   - 注入 
o.hwui.use_vulkan=0，强行将 Android 16 默认的 Vulkan UI 渲染回退至高度稳定的 OpenGL ES 硬件加速管线；
   - 优化 SurfaceFlinger 缓冲区调度（debug.sf.latch_unsignaled=1）与色彩配置。
2. **多媒体与编解码修复 (Media Codec2)**
   - 注入 debug.stagefright.c2inputsurface=-1，全面兼容老旧高通 HIDL C2 / OMX 视频输入流接口。
3. **全套 Treble 核心引导套件**
   - 内置 TrebleDroid 引导套件：/system/bin/rw-system.sh、ndk-detect、
esetprop_phh、usybox_phh、phh-on-boot.sh 等；
   - 注入 /system/phh 专属 OEM 按键映射文件（共 24 款主流机型触控板与按键适配）；
   - 注入 27 款厂商 RRO Overlay 适配包（/system/product/overlay/），覆盖 OPPO / Realme / OnePlus 等设备。
4. **原生纯净框架保障**
   - 100% 保持 Miku UI 原厂 Java 类库与核心 APEX 签名完整性，无混用第三方不兼容包，系统进程健康无冲突。

---

## ⚡ 刷入指南 (Flashing Guide)

### 1. 准备工作
- 确保 Bootloader 已解锁；
- 手机进入 Fastboot 模式并连接电脑。

### 2. 刷入命令
前往 [Releases 页面](https://github.com/hhsbjbsj/Miku-GSI/releases/latest) 下载 miku_ui_arm64_ab_treble_fixed.img.zip 并解压得到 miku_treble_fixed.img：

`ash
# 刷入系统镜像
fastboot flash system miku_treble_fixed.img

# 如设备有 vbmeta 校验需求（如 OPPO A11x 等），建议关闭校验：
fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img

# 格式化 data 并重启
fastboot -w
fastboot reboot
`

---

## 📦 镜像校验码 (Checksum)

- **ZIP 压缩包**: miku_ui_arm64_ab_treble_fixed.img.zip (约 1.19 GB)
- **解压后文件**: miku_treble_fixed.img (2,467,966,976 字节 / 2.29 GiB)
- **解压后 SHA256**:
  `	ext
  b967600717df5f2d2e8486ba28d8ce2a9c25148ce5cef85d538c576761707767
  `

---

## 💖 致谢与开源项目
- [Miku UI Project](https://github.com/Miku-UI)
- [TrebleDroid / phhusson](https://github.com/TrebleDroid)
- [AxionAOSP GSI](https://github.com/AxionAOSP)
