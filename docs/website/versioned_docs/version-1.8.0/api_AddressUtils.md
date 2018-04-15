---
id: version-1.8.0-AddressUtils
title: AddressUtils
original_id: AddressUtils
---

<div class="contract-doc"><div class="contract"><h2 class="contract-header"><span class="contract-kind">library</span> AddressUtils</h2><p class="description">有关地址的内联函数工具库。</p><div class="source">源码链接: <a href="https://github.com/OpenZeppelin/zeppelin-solidity/blob/v1.8.0/contracts/AddressUtils.sol" target="_blank">AddressUtils.sol</a></div></div><div class="index"><h2>Index</h2><ul><li><a href="AddressUtils.html#isContract">isContract</a></li></ul></div><div class="reference"><h2>Reference</h2><div class="functions"><h3>Functions</h3><ul><li><div class="item function"><span id="isContract" class="anchor-marker"></span><h4 class="name">isContract</h4><div class="body"><code class="signature">function <strong>isContract</strong><span>(address addr) </span><span>internal </span><span>view </span><span>returns  (bool) </span></code><hr/><div class="description">
<p>Returns whether there is code in the target address, This function will return false if invoked during the constructor of a contract, as the code is not actually created until after the constructor finishes.</p>
<p>返回目标地址是否含有代码, 如果一个合约的构造函数被调用则该函数将返回false, 因为代码不会实际的创建直到合约的构造函数执行完成。</p>
</div><dl><dt><span class="label-parameters">参数:</span></dt><dd><div><code>addr</code> - address类型：要检查的地址</div></dd><dt><span class="label-return">返回值:</span></dt><dd>目标地址是否含有代码</dd></dl></div></div></li></ul></div></div></div>
