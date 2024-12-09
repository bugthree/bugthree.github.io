---
layout: post
title: "MeshVS in DRAWEXE"
date: 2024-12-09
description: "MeshVS"
tag: OCCT
author: bugthree
---

## meshVS服务
MeshVS专门是用来使用OCCT的可视化模块来显示网格，**MeshVS** (Mesh Visualization Service) component provides flexible means of displaying meshes with associated pre- and post- processor data.
- 读取STL作为网格数据初始化
```bash
# meshfromstl
vinit View1
meshfromstl mesh myfile.stl # 导入指定STL,并直接显示网格
```
- 使用不同的显示风格
```bash
vsetdispmode mesh 2 # 显示风格 1 - wireframe 2 - shading mode, or 3 for shrink mode
```

- 使用不同的选择模式(使用occt7.8 测试失败)
```bash
vselmode mesh 1
# 0 – selection of mesh as whole;
# 1 – node selection;
# 2 – 0D elements (not supported in STL);
# 4_– links (not supported in STL);
# 8 – faces;
# 16 – volumes (not supported in STL);
# 256 – groups (not supported in STL).
```