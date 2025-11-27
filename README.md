# Newifi3 D2 Padavan Auto-Build (Multi-Kernel)

基于 GitHub Actions 的 **Newifi3 D2 (新路由3)** Padavan 固件全自动化构建脚本。
本仓库采用智能分流逻辑，同时支持 **Hanwckf (3.4)** 和 **MeIsReallyBa (4.4)** 两个主流内核版本的编译，修复了在 Ubuntu 22.04 环境下的工具链与依赖问题。

## ✨ 核心特性 (Features)

  * **🔄 双内核自由切换**：
      * **Kernel 3.4 (Hanwckf)**: 极致稳定，无线信号优异。脚本自动注入 **vb1980 KVR 漫游补丁**，支持 802.11k/v/r 协议，适合组网。
      * **Kernel 4.4 (MeIsReallyBa)**: 较新的内核版本，支持 WireGuard 等现代协议，底层库更新。
  * **🛠️ 智能构建修复**：
      * 自动识别版本，动态切换机型代号 (`NEWIFI3` vs `NEWIFI`)。
      * 自动修复 Ubuntu 22.04 下的 `xz`、`mksquashfs`、`dropbear` 及 `e2fsprogs` 编译报错。
  * **🍃 极致纯净**：
      * 移除 Aria2、Transmission、各类代理插件，仅保留核心路由功能。
      * 开启 **Flow Offload (硬件加速)**，降低 CPU 占用，确保千兆跑满。
  * **📦 独立发布 (Release Isolation)**：
      * 利用 Tag 隔离机制 (e.g., `v20231127-3.4` / `v20231127-4.4`)，不同版本的固件发布互不覆盖，版本管理更清晰。

## 🚀 如何使用 (How to Use)

1.  **Fork** 本仓库到你的 GitHub 账号。
2.  进入仓库的 **Actions** 页面。
3.  点击左侧的 **Build Newifi3 D2 (Final Optimized)** (或你的 Workflow 名称)。
4.  点击右侧的 **Run workflow** 按钮。
5.  **关键步骤**：在下拉菜单 `Select Kernel Version` 中选择：
      * `3.4 (Hanwckf + KVR + 22.04 Fix)`: 推荐。稳定、信号好、支持 KVR 漫游。
      * `4.4 (MeIsReallyBa + Docker)`: 尝鲜。新内核，插件支持更丰富（但在本纯净脚本中已精简）。
6.  等待编译完成（约 15 分钟）。
7.  前往仓库的 **Releases** 页面下载对应日期的固件（`.trx` 文件）。

## ⚙️ 默认配置 (Default Settings)

  * **管理地址**: `192.168.123.1`
  * **用户名**: `admin`
  * **密码**: `admin`
  * **WiFi 密码**: `1234567890`
  * **WiFi SSID**: `PDCN` / `PDCN_5G`

## 🛠️ 自定义说明 (Customization)

如果需要修改插件开关，请编辑 `.github/workflows/xxx.yml` 文件，定位到 `Run Compilation Logic` 步骤：

  * **针对 4.4 版本**：脚本修改的是 `board.mk`。
  * **针对 3.4 版本**：脚本修改的是 `.config`。

示例（启用 Aria2）：

```bash
# 找到类似代码，将 =n 改为 =y
sed -i 's/CONFIG_FIRMWARE_INCLUDE_ARIA=n/CONFIG_FIRMWARE_INCLUDE_ARIA=y/g' ...
```

## 🔗 致谢 (Credits)

  * [hanwckf/rt-n56u](https://github.com/hanwckf/rt-n56u) (3.4 源码)
  * [MeIsReallyBa/padavan-4.4](https://github.com/MeIsReallyBa/padavan-4.4) (4.4 源码)
  * [vb1980/Padavan-KVR](https://github.com/vb1980/Padavan-KVR) (KVR 补丁)

-----

**⚠️ 免责声明**: 刷机有风险，请务必在 **Breed** 恢复控制台下进行刷写。本仓库仅提供自动化构建脚本，不对固件使用造成的设备损坏或数据丢失负责。
