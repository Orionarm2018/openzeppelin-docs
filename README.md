# 中文版说明

开发安全的智能合约是一项非常有挑战性的任务，OpenZeppelin无疑是这方面的先行者。鉴于中文OpenZeppelin的文档非常少，在阅读OpenZeppelin的官方文档时变萌生了翻译该文档的想法。

欢迎各位对OpenZeppelin感兴趣的同学积极参与到文档的翻译中来，为推动中文世界的智能合约安全贡献一份力量！！！

OpenZeppelin 文档生成器
====================================

OpenZeppelin API 文档的原文链接 https://openzeppelin.org/api/docs/.

## 生成文档

要重新生成文档，请执行：

```sh
npm run gen-docs
```

如果修改了样式, header links, footer, 和静态内容，你可以自动生成所有的OpenZeppelin API 文档 - one per contract in the codebase - by running:

```sh
npm run bump-docs -- <tag>
```

例如:

```sh
npm run bump-docs -- v1.7.0
```

这会自动执行如下命令：

* Run [solidity-docgen](https://github.com/spalladino/solidity-docgen) on the OpenZeppelin codebase at the given tag.
* Generate a new Docusaurus version matching the OpenZeppelin release tag.
* Build the Docusaurus project, yielding the result in `docs/website/build`.
