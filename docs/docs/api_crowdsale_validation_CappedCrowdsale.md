---
id: crowdsale_validation_CappedCrowdsale
title: CappedCrowdsale
---

<div class="contract-doc"><div class="contract"><h2 class="contract-header"><span class="contract-kind">contract</span> CappedCrowdsale</h2><p class="base-contracts"><span>is</span> <a href="crowdsale_Crowdsale.html">Crowdsale</a></p><p class="description">Crowdsale with a limit for total contributions.</p><div class="source">Source: <a href="https://github.com/OpenZeppelin/zeppelin-solidity/blob/v1.7.0/contracts/crowdsale/validation/CappedCrowdsale.sol" target="_blank">crowdsale/validation/CappedCrowdsale.sol</a></div></div><div class="index"><h2>Index</h2><ul><li><a href="crowdsale_validation_CappedCrowdsale.html#CappedCrowdsale">CappedCrowdsale</a></li><li><a href="crowdsale_validation_CappedCrowdsale.html#_preValidatePurchase">_preValidatePurchase</a></li><li><a href="crowdsale_validation_CappedCrowdsale.html#capReached">capReached</a></li></ul></div><div class="reference"><h2>Reference</h2><div class="functions"><h3>Functions</h3><ul><li><div class="item function"><span id="CappedCrowdsale" class="anchor-marker"></span><h4 class="name">CappedCrowdsale</h4><div class="body"><code class="signature">function <strong>CappedCrowdsale</strong><span>(uint256 _cap) </span><span>public </span></code><hr/><div class="description"><p>Constructor, takes maximum amount of wei accepted in the crowdsale.</p></div><dl><dt><span class="label-parameters">Parameters:</span></dt><dd><div><code>_cap</code> - Max amount of wei to be contributed</div></dd></dl></div></div></li><li><div class="item function"><span id="_preValidatePurchase" class="anchor-marker"></span><h4 class="name">_preValidatePurchase</h4><div class="body"><code class="signature">function <strong>_preValidatePurchase</strong><span>(address _beneficiary, uint256 _weiAmount) </span><span>internal </span></code><hr/><div class="description"><p>Extend parent behavior requiring purchase to respect the funding cap.</p></div><dl><dt><span class="label-parameters">Parameters:</span></dt><dd><div><code>_beneficiary</code> - Token purchaser</div><div><code>_weiAmount</code> - Amount of wei contributed</div></dd></dl></div></div></li><li><div class="item function"><span id="capReached" class="anchor-marker"></span><h4 class="name">capReached</h4><div class="body"><code class="signature">function <strong>capReached</strong><span>() </span><span>public </span><span>view </span><span>returns  (bool) </span></code><hr/><div class="description"><p>Checks whether the cap has been reached.</p></div><dl><dt><span class="label-return">Returns:</span></dt><dd>Whether the cap was reached</dd></dl></div></div></li></ul></div></div></div>