# Hysteria2 部署脚本

基于 [diandongyun/node 的 hysteria2.sh](https://github.com/diandongyun/node/blob/0f555913dcc9748384c16ce0d1387df04eb45d90/hysteria2.sh) 保存的个人维护版本，供需要搭建 Hysteria2 节点的用户参考。

## 本版本的改动

- 删除第三方 `transfer` 程序的下载与执行。
- 删除包含服务器 IP、端口、认证密码及导入链接的配置上传逻辑。
- 保留其余原版部署流程、参数和界面。

移除上传功能不等于完成全面安全加固。脚本仍会联网下载 Hysteria2 安装脚本、查询公网 IP、安装依赖并执行测速。

## 使用前须知

- 面向使用 systemd 的 Linux 服务器；原脚本包含 Debian/Ubuntu、CentOS/Fedora 分支，未在各发行版实际部署验证。
- 需要 root 权限。建议先在可重建的测试服务器上使用。
- 原流程会重置 Debian/Ubuntu 的 UFW 规则，仅明确开放 SSH 22 和生成的 UDP 端口。非 22 端口的 SSH 或现有业务可能被阻断。
- 原流程会修改全局网络参数、覆盖已有 Hysteria2 配置与证书，并启用、重启服务。请先备份，确保可通过云厂商控制台恢复。
- 原版客户端关闭 TLS 证书验证，私钥权限设置也尚未加固。请勿把本版本视为适合生产环境的完整安全方案。
- 原版“端口跳跃”配置尚未按官方文档修正；客户端 SOCKS5 和 HTTP 均占用 1080 端口的问题也仍保留。

## 下载与运行

先下载到服务器并阅读脚本：

```bash
curl -fL --proto '=https' --tlsv1.2 \
  https://raw.githubusercontent.com/Jacob-Jacob98/HY2/main/hysteria2.sh \
  -o hysteria2.sh
less hysteria2.sh
```

确认当前服务器适合运行后，再执行：

```bash
sudo bash ./hysteria2.sh
```

使用 root 登录时，可执行 `bash ./hysteria2.sh`。执行脚本会实际修改服务器。

## 配置与服务

- 服务端配置：`/etc/hysteria/config.yaml`
- 客户端配置：`/opt/hysteria2_client.yaml`
- 服务名：`hysteria-server.service`
- 运行完成后，终端会显示连接信息和 Shadowrocket 导入链接。配置及链接包含密码，请自行妥善保管，不要提交到公开仓库。

查看运行状态与日志：

```bash
systemctl status hysteria-server.service
journalctl -u hysteria-server.service --no-pager -n 50
```

## 来源与验证

原作者与上游仓库：[diandongyun/node](https://github.com/diandongyun/node)。

基线提交：`0f555913dcc9748384c16ce0d1387df04eb45d90`  
原文件 Git blob：`3eb662faf83f08f6f3b9a65644ed20f918e61364`

本次只进行了代码差异核对与 Bash 静态语法检查，没有运行部署脚本或实测节点连接。上游代码的权利归原作者所有，本仓库未另行替上游代码指定许可证。
