---
layout: post
title:  "Git中的AutoCRLF与SafeCRLF"
date:   2016-11-21 09:18:34 +0800
tags: git
---

### 一、AutoCRLF

提交时转换为LF，检出时转换为CRLF  
```bash
git config --global core.autocrlf true   
```

提交时转换为LF，检出时不转换    
```bash
git config --global core.autocrlf input   
```

提交检出均不转换  
```bash
git config --global core.autocrlf false
```

### 二、SafeCRLF

拒绝提交包含混合换行符的文件    
```bash
git config --global core.safecrlf true   
```    

允许提交包含混合换行符的文件    
```bash
git config --global core.safecrlf false   
```

提交包含混合换行符的文件时给出警告  
```bash
git config --global core.safecrlf warn
```