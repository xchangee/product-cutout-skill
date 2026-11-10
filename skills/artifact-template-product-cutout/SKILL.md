---
name: artifact-template-product-cutout
description: "Create an image using the Product Cutout 透明投影商品图 template and its retained reference file. Use when the user selects this template, names Product Cutout 透明投影商品图, or explicitly invokes $artifact-template-product-cutout. 根据商品链接或原图，生成同款透明背景、柔和落地投影、方形留白的网页商品 PNG；保留商品造型、颜色、材质和图案。"
---

# Product Cutout 透明投影商品图

把商品链接或商品原图处理成可直接用于网页的透明 PNG，复用随附参考成品的透明背景、留白和柔和投影。保留参考文件不变。

## Workflow

1. 读取 `artifact-template.json`，解析本 skill 内的 `assets/reference.png` 并查看图片。它只提供排版、透明边缘和阴影样式；杯子不是后续商品的内容模板。
2. 获取本次商品的真实原图。用户给商品链接时，读取当前页面，优先选择最高分辨率、主体完整、无遮挡的独立商品照；有赞页面可从初始商品数据的图片列表取原图，不使用缩略图替代高清原图。先查看候选图，确认商品和款式。默认保留一个完整商品，不带包装盒或道具；套装或用户指定组合则保留对应组合。链接打不开且无法取得原图时，明确缺少原图并请用户提供图片，不能按商品名称猜造。
3. 使用 $imagegen 的内置图片编辑流程。需要支持图片编辑、参考图输入和透明背景输出的 image_gen 工具；该工具不可用时明确说明依赖，不假装已生成图片。先查看本地原图，将本次原图标为 EDIT TARGET、保留成品标为 STYLE REFERENCE，按下方提示词替换商品描述后调用。设置 `transparent_background: true`；全部输入均有路径时用 `referenced_image_paths`。不要让样式参考里的黑色、杯形或浮雕转移到新商品。
4. 对照原图检查商品轮廓、配件、图案、品牌文字及颜色；检查主体完整、无白边、无额外物件、阴影自然。读取实际文件尺寸和 Alpha 通道，确认背景透明，不能把白底或棋盘格当透明。发现明显变化时，以原图为准定向修正；文字或复杂图案无法忠实保留时说明限制，不宣称像素级无损。
5. 保留生成文件的 Alpha，保存为有意义的新版本 PNG，不覆盖原图。默认方形画布，主体约占最长边的 75–82%，居中且四周留白，底部阴影不裁切。尺寸以工具实际输出为准；本参考为 1254 × 1254，不承诺工具必定生成相同尺寸。
6. 根据当前项目的 AGENTS.md 保存成品和简短制作记录（来源、提示词、工具、尺寸）。遵循用户指定的保存位置和当前项目约定。交付图片和可用下载链接；只生成图片不构成修改、发布网站的授权。

## 默认效果

- 真实 Alpha 透明背景，无背景平面、矩形底色或烘焙棋盘格。
- 保留本次商品原本颜色、材质、拍摄角度、比例、文字和图案，不重新设计商品。
- 保留自然摄影光感与清晰抗锯齿边缘，无白色光晕。
- 紧贴底部的小范围中性灰接触阴影，轻柔向右延伸并渐隐为透明。避免悬浮、硬黑投影、镜面倒影或大面积灰雾。
- 不额外加标题、价格、标识、装饰、包装盒或道具。

## 图片编辑提示词

按本次原图填写方括号；若用户指定偏离上述效果，则调整对应要求。

```text
Edit / background removal, ecommerce product asset.
Image 1 is the EDIT TARGET: [describe the actual product and its identifying details].
Image 2 is ONLY a STYLE REFERENCE for true alpha transparency, square-canvas padding, clean edges and subtle contact shadow. Do not transfer the reference product's color, shape, material, motifs or branding to the target.
Create ONE square transparent PNG showing only the exact product from image 1. Remove the background and unrelated props. Preserve the original silhouette, proportions, viewpoint, colors, materials, all text, logos, patterns and their arrangement, and photographic lighting. Do not redesign or invent details.
Center the entire product with generous transparent padding, occupying approximately 75–82 percent of the canvas along its longest dimension. Keep all parts visible. Add or retain a small realistic diffuse neutral-gray contact shadow beneath the base, gently extending right and fading to fully transparent. Keep the shadow subtle and grounded, without a visible floor or reflection.
True alpha transparency outside the product and semi-transparent shadow. No rectangular backdrop, no baked-in checkerboard, no extra objects or text. Crisp natural antialiased edges without white halos. Photorealistic and faithful to the edit target.
```

## Fidelity

本次商品原图决定商品外观；保留成品决定透明、留白和投影风格。用户的具体要求优先。生成式编辑可能改变细节，必须对照原图检查，不把「要求保持」当作「已完全保持」的证据。
