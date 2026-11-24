# Microduck 接线图（3D）· 打印耗材

可旋转、逐步高亮的接线图。非电子的打印件按耗材上色，悬停显示材料，点击后在左下角写出耗材名称。舵机、电路板、IMU、主控、摄像头、电池和导线保持原样。

本地打开（页面要加载模型，不能直接双击 `index.html`）：

```bash
cd docs/wiring3d
python3 -m http.server 8765
# 浏览器打开 http://127.0.0.1:8765/
```

## 改耗材对应关系

只改 `app.js` 里的 `filamentMap`。右边的值可以是 `'PLA'`、`'PETG'`、`'ABS'`、`'TPU'`。颜色和图例名字在上面的 `FILAMENTS` 里。

不要把电子件写进这张表。没写进表的网格（金属轴承、舵机、镜头、电池等）不会上色，也不会出现耗材标签。

当前默认：

| 耗材 | 用在哪些件 | 依据 |
|---|---|---|
| PLA | 左右壳、头壳、脸、下巴、脚、脚底、香蕉板锁扣 | CAD 打印工程和 BOM 材料栏为 PLA |
| PETG | 躯干、腿、髋、踝、颈、支架、轴承滚轮 | 采购清单写明结构件可用 PLA 或 PETG，这里把受力件标成更韧的 PETG |
| TPU | 软下巴 `jaw_soft`、软嘴顶部 `soft_mouth_top` | BOM 备注「建议改用 TPU 软料」 |
| ABS | 未默认分配 | `FILAMENTS` 里已留好。例如把 `motor_support` 改成 `'ABS'` 就会进图例 |

脚底若改打软底，把 `sole_left` 和 `sole_right` 改成 `'TPU'`。想让结构件也全部用 PLA，把表里的 `'PETG'` 换成 `'PLA'`。

眼睛 `noenoeil` 在 BOM 里是 PLA 打印件，但模型里和摄像头绑在同一组。按需求保持摄像头原样，所以没有单独标耗材。

## 部署到 GitHub Pages

页面里的脚本、模型和贴图都是相对路径，下面两种放法都可以。仓库要公开（或使用付费的私有 Pages）。

### 这个目录作为站点根

1. 在 GitHub 新建一个空仓库。
2. 把 `docs/wiring3d/` **里面**的文件放到仓库根目录，让 `index.html` 和 `app.js`、`lib/`、`model/` 在同一层。
3. 推上去：

```bash
cd docs/wiring3d
git init
git add index.html app.js README.md preview.png lib model
git commit -m "Publish the Microduck wiring viewer with filament labels."
git branch -M main
git remote add origin https://github.com/<你的用户名>/<仓库名>.git
git push -u origin main
```

4. 打开仓库的 **Settings → Pages**。
5. **Build and deployment** 选 **Deploy from a branch**。
6. Branch 选 `main`，Folder 选 `/ (root)`，保存。
7. 等一分钟，打开 `https://<你的用户名>.github.io/<仓库名>/`。

### 放回原仓库的 docs/wiring3d

1. 用这里的 `docs/wiring3d` 覆盖 [microduck-replica](https://github.com/fanhao375/microduck-replica) 里的同名目录。
2. 推到 `main`（或 `master`）。
3. **Settings → Pages** 里 Branch 选该分支，Folder 选 `/docs`。
4. 打开 `https://<你的用户名>.github.io/microduck-replica/wiring3d/`。

原项目现在的在线地址是 <https://fanhao375.github.io/microduck-replica/wiring3d/>。那是上游仓库，不会自动带上这次的耗材改动。

## 说明

- 鸭子身体是站姿；接线的颜色只用来区分；HUB 的前后位置、摄像头和 ToF 小板的外形为示意。
- 耗材颜色可以在左上角关掉「耗材着色」。关掉之后，悬停和点击仍然显示材料名称。
- 点舵机、板子或线时，仍会按原来的步骤高亮并移动镜头。点打印件只显示耗材，不打断当前步骤。
- 舵机接口位置取自 HD-1910 的 STEP 模型；HAT 接口位置取自 HAT 的 PCB 模型；香蕉板、HUB 的板面取自生产文件。

## 模型和资料来源

| 内容 | 来源 |
|---|---|
| 鸭子机械模型 | Pollen Robotics Microduck 开源仿真模型（Apache-2.0），转成网页可用的格式 |
| HAT | Pollen Robotics 开源设计（Apache-2.0） |
| 主控 | Radxa ZERO 3W 官方 3D 模型 |
| 舵机 | 飞特 HD-1910-C001 官方 STEP |
| IMU 小板 | 本项目自研 |
| HUB、香蕉板 | 社区开源设计（Apache-2.0） |
| 耗材对应 | [CAD 仓库打印说明](https://github.com/fanhao375/microduck-replica-cad) 与本仓库机械采购清单 |
| 三维引擎 | three.js（MIT） |
