---
name: lanxiang-gu-feng-illustration
description: "兰香古风插画 Skill：将人物照片渲染成去脸化、朦胧、色块拼接的中国古典叙事插画；适用于把新照片转换成这种意境化插画风格时。"
---

# 兰香古风插画 Skill

将照片转换成一种“看得见意境，但不逐项复刻现实”的中国古典叙事插画。默认使用内置图像生成/编辑工具完成，不使用代码绘图替代。

## 核心判断

把照片和最终插画分成两层处理：

- 照片只提供叙事骨架：人物数量、动作、姿态、轮廓、关系、视线方向、空间层次、主光线和情绪。
- 插画负责重新组织视觉信息：去掉脸部识别细节，压缩色彩，拆成大色块，用轮廓、留白、虚实和色温传达意境。

不要把任务理解成“给照片加古风滤镜”或“精细临摹照片”。目标是让观者先通过色彩和结构感受到照片的情绪，再从姿态和轮廓读出发生了什么。

## 不可丢失的视觉语法

### 1. 去脸化

人物脸部应当是干净的皮肤色面、阴影色面或留白形状，只保留：

- 外轮廓，尤其是侧脸的额头、鼻尖、下巴和颈部转折；
- 一到两笔眉形，用来暗示方向和情绪；
- 必要时保留发际线、耳饰或头发边缘。

禁止生成写实的眼睛、瞳孔、眼睑、鼻孔、嘴唇、牙齿、泪痕和皮肤纹理。正面人物也不能靠五官表达情绪，应通过头部倾斜、手势、衣服形状、光线和色块关系表达。

### 2. 色块重构，而不是 1:1 还原

先从照片提取少量强色：通常 4–7 个主色面，例如墨黑、青蓝、朱红、米白、灰紫、暗棕、暗金。把衣服、建筑、头发、雪、灯光和道具重新组织成互相咬合的扁平形状。

保留最有叙事作用的色彩对比，舍弃细碎纹理、真实褶皱、复杂饰品和所有不影响情绪的物件。允许色彩偏离照片，只要能强化主体关系、前后景和情绪方向。

### 3. 形状优先于材质

用不规则的平涂/水粉/剪纸状形状建立人物和空间，再用少量纸张颗粒、干刷边缘、柔焦和半透明叠色增加朦胧感。不要使用油亮皮肤、摄影级反光、3D 体积或完整的写实渐变。

### 4. 用构图和动作传达情绪

优先保留：

- 两人之间的距离、对视、触碰、递物、挥手、拥抱等关系动作；
- 背影、侧影、跪坐、奔跑、骑马、躺卧等明确姿态；
- 门框、窗格、拱门、树枝、帘幕、流苏等具有框景作用的结构；
- 前景遮挡、中景人物、远景氛围形成的层次。

情绪不要靠脸部表演，而要靠动作线、色温、明暗、留白和空间压迫感表达。

### 5. 让照片“朦胧但可读”

主体轮廓、主要动作和关键色块要清楚；背景、次要人物和微小道具可以融入雾气或柔焦。轮廓不是全部勾死：只在头发、衣袖、手势、建筑边缘和光影交界处使用选择性线条。

### 6. 忽略照片上的文字

照片里的字幕、标题、贴纸、平台水印、按钮、时间戳和 UI 都是噪声，不能复制到最终图中。只读取文字背后的画面，不要把文字当作构图元素。

## 生成工作流

1. 识别输入角色：用户照片是 edit target；用户提供的插画样例、已确认的成品或风格板是 style reference。不要把风格参考中的具体人物、姿势或道具移植到新照片。
2. 先提取照片骨架：主体数量、画面方向、动作关系、关键轮廓、前中后景、主色和情绪。
3. 把骨架重写成视觉规格：去脸、4–7 个色块、选择性轮廓、纸张肌理、柔焦背景和明确的空间层次。
4. 默认输出 3:4 竖版。用户指定其他画幅时服从用户；改变画幅时重新设计裁切和主体位置，不拉伸人物。
5. 使用内置 `image_gen` 工具进行 style-transfer。对于本地照片，先用 `view_image` 确认图像可见，再调用图像工具。多张不同照片应分别生成，不要用一个 prompt 的多张 variant 代替不同资产。
6. 在 prompt 中明确写出：Image 1 是 edit target；其余图片是 style references；忽略所有文字和 UI；脸部只保留眉形与外轮廓；照片只提供构图和意境，不进行 1:1 复刻。
7. 检查输出；若脸部出现写实五官，下一轮只针对“更彻底去脸”修正；若画面过于像照片，下一轮只加强“色彩压缩和形状拼接”；不要同时加入大量无关新元素。
8. 原图不覆盖。项目内交付物保存到 workspace 的 `outputs/`；用户明确要求下载时，再复制到用户指定目录。

## 推荐 prompt 结构

```text
Use case: style-transfer
Asset type: finished poetic Chinese historical illustration
Input images: Image 1 is the edit target photo; Images 2–N are style references.
Output format: 3:4 vertical portrait unless the user specifies another ratio.
Ignore all text, subtitles, logos, stickers, watermarks, and UI in the edit target.

Preserve: <人物数量、动作、关系、轮廓、空间结构、主光线>
Rebuild: <用哪些 4–7 个主色块和哪些形状关系重新表达>
Face rule: blank face planes; only sparse eyebrow marks and outer silhouette contours;
no eyes, pupils, nose, nostrils, lips, teeth, or realistic facial anatomy.
Style: hand-painted 2D gouache/cut-paper shapes, selective ink contours,
soft atmospheric blur, paper grain, restrained ornament, quiet cinematic depth.
Constraints: no 1:1 photographic detail, no photorealism, no glossy 3D,
no anime eyes, no text, no watermark, no extra foreground narrative.
```

## 输出验收清单

- 是否仍能通过姿态、轮廓、服装色块和空间关系读出原照片的事件？
- 脸部是否只有眉形和轮廓，而不是完整五官？
- 是否由少量明显色块构成，而不是照片级材质和褶皱？
- 是否保留了原图最重要的动作、人物关系和情绪？
- 背景是否退到氛围层，主体是否有清晰的结构性？
- 是否完全移除了字幕、贴纸、水印和 UI？
- 画幅是否符合用户要求，且没有拉伸人物？

如果其中任一项不满足，优先做一次单变量修正：只改“去脸”“色块抽象”“构图裁切”或“文字清除”中的一个问题。
