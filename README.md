# 中文版说明

开发安全的智能合约是一项非常有挑战性的任务，OpenZeppelin无疑是这方面的先行者。鉴于中文OpenZeppelin的文档非常少，在阅读OpenZeppelin的官方文档时便萌生了翻译该文档的想法。

欢迎各位对OpenZeppelin感兴趣的同学积极参与到文档的翻译中来，为推动中文世界的智能合约安全贡献一份力量！！！

OpenZeppelin 文档生成器
====================================

OpenZeppelin API 文档的原文链接 https://openzeppelin.org/api/docs/.

## 生成文档

要重新生成文档，请执行：

```sh
npm run gen-docs
```

如果修改了样式, header links, footer, 和静态内容，你可以自动生成所有的OpenZeppelin API 文档，通过执行一下代码生成对应tag的文档：

```sh
npm run bump-docs -- <tag>
```

例如:

```sh
npm run bump-docs -- v1.7.0
```

这会自动执行如下命令：

* 在给定OpenZeppelin的tag下执行[solidity-docgen](https://github.com/spalladino/solidity-docgen)。
* 产生新的对应OpenZeppelin发行tag的Docusaurus版本文档。
* 创建Docusaurus工程, 生成结果在`docs/website/build`中。
