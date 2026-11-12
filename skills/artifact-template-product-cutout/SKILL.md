---
name: artifact-template-product-cutout
description: "Create an image using the Product Cutout 透明投影商品图 template and its retained reference file. Use when the user selects this template, names Product Cutout 透明投影商品图, or explicitly invokes $artifact-template-product-cutout. 根据商品链接或原图，生成同款透明背景、柔和落地投影、统一视觉尺寸的方形商品 PNG；默认主体安全区 66%、投影安全区 82%，保留商品造型、颜色、材质和图案。"
---

# Product Cutout 透明投影商品图

把商品链接或商品原图处理成可直接用于网页的透明 PNG，复用随附参考成品的透明背景和柔和投影，按下方视觉尺寸规范统一留白与大小。保留参考文件不变。

## Workflow

1. 读取 `artifact-template.json`，解析本 skill 内的 `assets/reference.png` 并查看图片。它只提供质感、透明边缘和阴影样式；主体占比以本 skill 的视觉尺寸规范为准，杯子不是后续商品的内容模板。
2. 获取本次商品的真实原图。用户给商品链接时，读取当前页面，优先选择最高分辨率、主体完整、无遮挡的独立商品照；有赞页面可从初始商品数据的图片列表取原图，不使用缩略图替代高清原图。先查看候选图，确认商品和款式。默认保留一个完整商品，不带包装盒或道具；套装或用户指定组合则保留对应组合。链接打不开且无法取得原图时，明确缺少原图并请用户提供图片，不能按商品名称猜造。
3. 使用 $imagegen 的内置图片编辑流程。需要支持图片编辑、参考图输入和透明背景输出的 image_gen 工具；该工具不可用时明确说明依赖，不假装已生成图片。从首次生成就带入下方 66% 主体、82% 可见投影和轻微类别大小差异的要求。先查看本地原图，将本次原图标为 EDIT TARGET、保留成品标为 STYLE REFERENCE，按下方提示词替换商品描述后调用。设置 `transparent_background: true`；全部输入均有路径时用 `referenced_image_paths`。不要让样式参考里的黑色、杯形或浮雕转移到新商品。
4. 对照原图检查商品轮廓、配件、图案、品牌文字及颜色；检查主体完整、无白边、无额外物件、阴影自然。读取实际文件尺寸和 Alpha 通道，确认背景透明，不能把白底或棋盘格当透明；依据下方规范核对主体及投影范围，不把提示词中的比例当作实际达标证据。发现明显变化时，以原图为准定向修正；文字或复杂图案无法忠实保留时说明限制，不宣称像素级无损。
5. 保留生成文件的 Alpha，保存为有意义的新版本 PNG；用户明确要求覆盖时按其要求执行，默认不覆盖原图。默认方形画布，按下方安全区统一主体视觉体量。若超界或明显失衡，修正尺寸、位置和透明留白，保持商品与投影完整，再检查后交付。尺寸以工具实际输出为准；本参考为 1254 × 1254，不承诺工具必定生成相同尺寸。
6. 根据当前项目的 AGENTS.md 保存成品和简短制作记录（来源、提示词、工具、尺寸）。遵循用户指定的保存位置和当前项目约定。交付图片和可用下载链接；只生成图片不构成修改、发布网站的授权。

## 默认效果

- 真实 Alpha 透明背景，无背景平面、矩形底色或烘焙棋盘格。
- 保留本次商品原本颜色、材质、拍摄角度、比例、文字和图案，不重新设计商品。
- 保留自然摄影光感与清晰抗锯齿边缘，无白色光晕。
- 紧贴底部的小范围中性灰接触阴影，轻柔向右延伸并渐隐为透明。避免悬浮、硬黑投影、镜面倒影或大面积灰雾。
- 不额外加标题、价格、标识、装饰、包装盒或道具。

## 视觉尺寸规范（默认）

比例均按画布宽、高分别计算，不是面积占比。以 W × H 画布为例：

| 范围 | 安全区域 | 四周最小留白 |
| --- | --- | --- |
| 完整商品主体（含提手、挂绳等配件） | 中间 66% × 66%，即 x=17%–83%、y=17%–83% | 17% |
| 商品与可见投影的整体范围 | 中间 82% × 82%，即 x=9%–91%、y=9%–91% | 9% |
| 极淡的半透明边缘与投影尾部 | 中间 98% × 98% | 1% |

1000 × 1000 画布对应主体 660 × 660、含可见投影 820 × 820 的最大区域。安全线仅用于检查，不能画进成品；透明留白不是白色描边。

- 66% 是上限，不要求所有商品填满。保持原比例，商品及其原有投影一起等比缩放、移动，不拉伸、不裁掉投影、不改变单件或套装内部比例。
- 按形状、轮廓面积和视觉体量统一大小：饱满宽大的产品适当收小，细长或镂空产品可适当放大，但所有商品均受安全区上限约束。小挂件、徽章等略小，背包、抱枕等略大，仅允许轻微差异，不按真实物理尺寸同比例展示。
- 水平视觉居中；视觉中心优先放在 (50%W, 49%H)，约向上偏 1%。安全留白和完整投影优先于精确居中。
- 参考图和历史示例可能比此规范更满；它们只提供质感、透明边缘和投影参考，不能覆盖这里的尺寸要求。
- 需要批量统一或量化检查时，读取 [references/visual-sizing.md](references/visual-sizing.md)。不要把“缩小 20%”作为反复执行的步骤，应每次按最终画布与安全区计算。

## 图片编辑提示词

按本次原图填写方括号；若用户指定偏离上述效果，则调整对应要求。

```text
Edit / background removal, ecommerce product asset.
Image 1 is the EDIT TARGET: [describe the actual product and its identifying details].
Image 2 is ONLY a STYLE REFERENCE for true alpha transparency, clean edges and subtle contact shadow. Its product size and padding do not override the numerical layout rules below. Do not transfer the reference product's color, shape, material, motifs or branding to the target.
Create ONE square transparent PNG showing only the exact product from image 1. Remove the background and unrelated props. Preserve the original silhouette, proportions, viewpoint, colors, materials, all text, logos, patterns and their arrangement, and photographic lighting. Do not redesign or invent details.
Keep the COMPLETE product, including handles, cords and accessories, inside the central 66% of both canvas width and height: x=17%–83%, y=17%–83%. This is a maximum bounding region, not a required size and not 66% of the image area. Preserve aspect ratio. Balance apparent visual mass across products: make compact, dense silhouettes a little smaller and slender or open silhouettes a little larger within the same limits; small accessories should feel slightly smaller than bags or cushions, never dramatically different.
Center the product visually near x=50%, y=49%. Keep the combined product and visible shadow inside the central 82% of both dimensions, with at least 9% clearance on every side. Retain very faint shadow tails inside the central 98%, leaving at least 1% clearance. Respect all safe regions even if this requires a slightly smaller product or a small positional adjustment. Do not draw safe-area guides, borders or white outlines.
Add or retain a small realistic diffuse neutral-gray contact shadow beneath the base, gently extending right and fading to fully transparent. Keep it subtle and grounded, without a visible floor or reflection. Preserve the whole product and its complete shadow; never crop either.
True alpha transparency outside the product and semi-transparent shadow. No rectangular backdrop, no baked-in checkerboard, no extra objects or text. Crisp natural antialiased edges without white halos. Photorealistic and faithful to the edit target.
```

## Fidelity

本次商品原图决定商品外观；保留成品决定透明边缘和投影风格，安全区与视觉体量由本 skill 的尺寸规范决定。用户的具体要求优先。生成式编辑可能改变细节，必须对照原图检查，不把「要求保持」当作「已完全保持」的证据。
