---
id: version-1.8.0-crowdsale_Crowdsale
title: Crowdsale
original_id: crowdsale_Crowdsale
---

<div class="contract-doc"><div class="contract"><h2 class="contract-header"><span class="contract-kind">合约</span> Crowdsale</h2><p class="description">Crowdsale is a base contract for managing a token crowdsale, allowing investors to purchase tokens with ether. This contract implements such functionality in its most fundamental form and can be extended to provide additional functionality and/or custom behavior. The external interface represents the basic interface for purchasing tokens, and conform the base architecture for crowdsales. They are *not* intended to be modified / overriden. The internal interface conforms the extensible and modifiable surface of crowdsales. Override the methods to add functionality. Consider using &#x27;super&#x27; where appropiate to concatenate behavior.</p>

<p class="description">Crowdsale合约（译者注：Crowdsale原意为众销，与此对应的是 Crowdfound众筹  ）是一个管理 token 众销的基础合约, 允许创建人使用ether购买通证（译者注：鉴于你知道的原因token已被用通证替代）。这个合约以最基本的形式实现了这些功能并可以被扩展以提供更多自定义功能。外部接口定义了购买通证和遵循通证发行的基础架构的接口。我们强烈不建议被修改或者重写这些接口。它的外部接口符合可扩展和可修改的surface of crowdsales。重写这些方法可以添加新的功能。使用 &#x27;super&#x27; 关键字以调用重写前的功能。</p>

<div class="source">源码: <a href="https://github.com/OpenZeppelin/zeppelin-solidity/blob/v1.8.0/contracts/crowdsale/Crowdsale.sol" target="_blank">crowdsale/Crowdsale.sol</a></div>

```js
pragma solidity ^0.4.18;

import "../token/ERC20/ERC20.sol";
import "../math/SafeMath.sol";

/**
 * @title Crowdsale
 * @dev Crowdsale is a base contract for managing a token crowdsale,
 * allowing investors to purchase tokens with ether. This contract implements
 * such functionality in its most fundamental form and can be extended to provide additional
 * functionality and/or custom behavior.
 * The external interface represents the basic interface for purchasing tokens, and conform
 * the base architecture for crowdsales. They are *not* intended to be modified / overriden.
 * The internal interface conforms the extensible and modifiable surface of crowdsales. Override 
 * the methods to add functionality. Consider using 'super' where appropiate to concatenate
 * behavior.
 */

contract Crowdsale {
  using SafeMath for uint256;

  // The token being sold
  ERC20 public token;

  // Address where funds are collected
  address public wallet;

  // How many token units a buyer gets per wei
  uint256 public rate;

  // Amount of wei raised
  uint256 public weiRaised;

  /**
   * Event for token purchase logging
   * @param purchaser who paid for the tokens
   * @param beneficiary who got the tokens
   * @param value weis paid for purchase
   * @param amount amount of tokens purchased
   */
  event TokenPurchase(address indexed purchaser, address indexed beneficiary, uint256 value, uint256 amount);

  /**
   * @param _rate Number of token units a buyer gets per wei
   * @param _wallet Address where collected funds will be forwarded to
   * @param _token Address of the token being sold
   */
  function Crowdsale(uint256 _rate, address _wallet, ERC20 _token) public {
    require(_rate > 0);
    require(_wallet != address(0));
    require(_token != address(0));

    rate = _rate;
    wallet = _wallet;
    token = _token;
  }

  // -----------------------------------------
  // Crowdsale external interface
  // -----------------------------------------

  /**
   * @dev fallback function ***不要重写此方法***
   */
  function () external payable {
    buyTokens(msg.sender);
  }

  /**
   * @dev low level token purchase ***不要重写此方法***
   * @param _beneficiary Address performing the token purchase
   */
  function buyTokens(address _beneficiary) public payable {

    uint256 weiAmount = msg.value;
    _preValidatePurchase(_beneficiary, weiAmount);

    // calculate token amount to be created
    uint256 tokens = _getTokenAmount(weiAmount);

    // update state
    weiRaised = weiRaised.add(weiAmount);

    _processPurchase(_beneficiary, tokens);
    TokenPurchase(msg.sender, _beneficiary, weiAmount, tokens);

    _updatePurchasingState(_beneficiary, weiAmount);

    _forwardFunds();
    _postValidatePurchase(_beneficiary, weiAmount);
  }

  // -----------------------------------------
  // Internal interface (extensible)
  // -----------------------------------------

  /**
   * @dev Validation of an incoming purchase. Use require statements to revert state when conditions are not met. Use super to concatenate validations.
   * @param _beneficiary Address performing the token purchase
   * @param _weiAmount Value in wei involved in the purchase
   */
  function _preValidatePurchase(address _beneficiary, uint256 _weiAmount) internal {
    require(_beneficiary != address(0));
    require(_weiAmount != 0);
  }

  /**
   * @dev Validation of an executed purchase. Observe state and use revert statements to undo rollback when valid conditions are not met.
   * @param _beneficiary Address performing the token purchase
   * @param _weiAmount Value in wei involved in the purchase
   */
  function _postValidatePurchase(address _beneficiary, uint256 _weiAmount) internal {
    // optional override
  }

  /**
   * @dev Source of tokens. Override this method to modify the way in which the crowdsale ultimately gets and sends its tokens.
   * @param _beneficiary Address performing the token purchase
   * @param _tokenAmount Number of tokens to be emitted
   */
  function _deliverTokens(address _beneficiary, uint256 _tokenAmount) internal {
    token.transfer(_beneficiary, _tokenAmount);
  }

  /**
   * @dev Executed when a purchase has been validated and is ready to be executed. Not necessarily emits/sends tokens.
   * @param _beneficiary Address receiving the tokens
   * @param _tokenAmount Number of tokens to be purchased
   */
  function _processPurchase(address _beneficiary, uint256 _tokenAmount) internal {
    _deliverTokens(_beneficiary, _tokenAmount);
  }

  /**
   * @dev Override for extensions that require an internal state to check for validity (current user contributions, etc.)
   * @param _beneficiary Address receiving the tokens
   * @param _weiAmount Value in wei involved in the purchase
   */
  function _updatePurchasingState(address _beneficiary, uint256 _weiAmount) internal {
    // optional override
  }

  /**
   * @dev Override to extend the way in which ether is converted to tokens.
   * @param _weiAmount Value in wei to be converted into tokens
   * @return Number of tokens that can be purchased with the specified _weiAmount
   */
  function _getTokenAmount(uint256 _weiAmount) internal view returns (uint256) {
    return _weiAmount.mul(rate);
  }

  /**
   * @dev Determines how ETH is stored/forwarded on purchases.
   */
  function _forwardFunds() internal {
    wallet.transfer(msg.value);
  }
}
```
</div><div class="index"><h2>Index</h2><ul><li><a href="crowdsale_Crowdsale.html#Crowdsale">Crowdsale</a></li><li><a href="crowdsale_Crowdsale.html#TokenPurchase">TokenPurchase</a></li><li><a href="crowdsale_Crowdsale.html#_deliverTokens">_deliverTokens</a></li><li><a href="crowdsale_Crowdsale.html#_forwardFunds">_forwardFunds</a></li><li><a href="crowdsale_Crowdsale.html#_getTokenAmount">_getTokenAmount</a></li><li><a href="crowdsale_Crowdsale.html#_postValidatePurchase">_postValidatePurchase</a></li><li><a href="crowdsale_Crowdsale.html#_preValidatePurchase">_preValidatePurchase</a></li><li><a href="crowdsale_Crowdsale.html#_processPurchase">_processPurchase</a></li><li><a href="crowdsale_Crowdsale.html#_updatePurchasingState">_updatePurchasingState</a></li><li><a href="crowdsale_Crowdsale.html#buyTokens">buyTokens</a></li><li><a href="crowdsale_Crowdsale.html#">fallback</a></li></ul></div><div class="reference"><h2>Reference</h2><div class="events"><h3>Events</h3><ul><li><div class="item event"><span id="TokenPurchase" class="anchor-marker"></span><h4 class="name">TokenPurchase</h4><div class="body"><code class="signature">event <strong>TokenPurchase</strong><span>(address purchaser, address beneficiary, uint256 value, uint256 amount) </span></code><hr/><div class="description"><p>Event for token purchase logging.</p></div><dl><dt><span class="label-parameters">Parameters:</span></dt><dd><div><code>purchaser</code> - who paid for the tokens</div><div><code>beneficiary</code> - who got the tokens</div><div><code>value</code> - weis paid for purchase</div><div><code>amount</code> - amount of tokens purchased</div></dd></dl></div></div></li></ul></div><div class="functions"><h3>Functions</h3><ul><li><div class="item function"><span id="Crowdsale" class="anchor-marker"></span><h4 class="name">Crowdsale</h4><div class="body"><code class="signature">function <strong>Crowdsale</strong><span>(uint256 _rate, address _wallet, ERC20 _token) </span><span>public </span></code><hr/><dl><dt><span class="label-parameters">Parameters:</span></dt><dd><div><code>_rate</code> - Number of token units a buyer gets per wei</div><div><code>_wallet</code> - Address where collected funds will be forwarded to</div><div><code>_token</code> - Address of the token being sold</div></dd></dl></div></div></li><li><div class="item function"><span id="_deliverTokens" class="anchor-marker"></span><h4 class="name">_deliverTokens</h4><div class="body"><code class="signature">function <strong>_deliverTokens</strong><span>(address _beneficiary, uint256 _tokenAmount) </span><span>internal </span></code><hr/><div class="description"><p>Source of tokens. Override this method to modify the way in which the crowdsale ultimately gets and sends its tokens.</p></div><dl><dt><span class="label-parameters">Parameters:</span></dt><dd><div><code>_beneficiary</code> - Address performing the token purchase</div><div><code>_tokenAmount</code> - Number of tokens to be emitted</div></dd></dl></div></div></li><li><div class="item function"><span id="_forwardFunds" class="anchor-marker"></span><h4 class="name">_forwardFunds</h4><div class="body"><code class="signature">function <strong>_forwardFunds</strong><span>() </span><span>internal </span></code><hr/><div class="description"><p>Determines how ETH is stored/forwarded on purchases.</p></div></div></div></li><li><div class="item function"><span id="_getTokenAmount" class="anchor-marker"></span><h4 class="name">_getTokenAmount</h4><div class="body"><code class="signature">function <strong>_getTokenAmount</strong><span>(uint256 _weiAmount) </span><span>internal </span><span>view </span><span>returns  (uint256) </span></code><hr/><div class="description"><p>Override to extend the way in which ether is converted to tokens.</p></div><dl><dt><span class="label-parameters">Parameters:</span></dt><dd><div><code>_weiAmount</code> - Value in wei to be converted into tokens</div></dd><dt><span class="label-return">Returns:</span></dt><dd>Number of tokens that can be purchased with the specified _weiAmount</dd></dl></div></div></li><li><div class="item function"><span id="_postValidatePurchase" class="anchor-marker"></span><h4 class="name">_postValidatePurchase</h4><div class="body"><code class="signature">function <strong>_postValidatePurchase</strong><span>(address _beneficiary, uint256 _weiAmount) </span><span>internal </span></code><hr/><div class="description"><p>Validation of an executed purchase. Observe state and use revert statements to undo rollback when valid conditions are not met.</p></div><dl><dt><span class="label-parameters">Parameters:</span></dt><dd><div><code>_beneficiary</code> - Address performing the token purchase</div><div><code>_weiAmount</code> - Value in wei involved in the purchase</div></dd></dl></div></div></li><li><div class="item function"><span id="_preValidatePurchase" class="anchor-marker"></span><h4 class="name">_preValidatePurchase</h4><div class="body"><code class="signature">function <strong>_preValidatePurchase</strong><span>(address _beneficiary, uint256 _weiAmount) </span><span>internal </span></code><hr/><div class="description"><p>Validation of an incoming purchase. Use require statements to revert state when conditions are not met. Use super to concatenate validations.</p></div><dl><dt><span class="label-parameters">Parameters:</span></dt><dd><div><code>_beneficiary</code> - Address performing the token purchase</div><div><code>_weiAmount</code> - Value in wei involved in the purchase</div></dd></dl></div></div></li><li><div class="item function"><span id="_processPurchase" class="anchor-marker"></span><h4 class="name">_processPurchase</h4><div class="body"><code class="signature">function <strong>_processPurchase</strong><span>(address _beneficiary, uint256 _tokenAmount) </span><span>internal </span></code><hr/><div class="description"><p>Executed when a purchase has been validated and is ready to be executed. Not necessarily emits/sends tokens.</p></div><dl><dt><span class="label-parameters">Parameters:</span></dt><dd><div><code>_beneficiary</code> - Address receiving the tokens</div><div><code>_tokenAmount</code> - Number of tokens to be purchased</div></dd></dl></div></div></li><li><div class="item function"><span id="_updatePurchasingState" class="anchor-marker"></span><h4 class="name">_updatePurchasingState</h4><div class="body"><code class="signature">function <strong>_updatePurchasingState</strong><span>(address _beneficiary, uint256 _weiAmount) </span><span>internal </span></code><hr/><div class="description"><p>Override for extensions that require an internal state to check for validity (current user contributions, etc.).</p></div><dl><dt><span class="label-parameters">Parameters:</span></dt><dd><div><code>_beneficiary</code> - Address receiving the tokens</div><div><code>_weiAmount</code> - Value in wei involved in the purchase</div></dd></dl></div></div></li><li><div class="item function"><span id="buyTokens" class="anchor-marker"></span><h4 class="name">buyTokens</h4><div class="body"><code class="signature">function <strong>buyTokens</strong><span>(address _beneficiary) </span><span>public </span><span>payable </span></code><hr/><div class="description"><p>Low level token purchase ***不要重写此方法***.</p></div><dl><dt><span class="label-parameters">Parameters:</span></dt><dd><div><code>_beneficiary</code> - Address performing the token purchase</div></dd></dl></div></div></li><li><div class="item function"><span id="fallback" class="anchor-marker"></span><h4 class="name">fallback</h4><div class="body"><code class="signature">function <strong></strong><span>() </span><span>external </span><span>payable </span></code><hr/><div class="description"><p>Fallback function ***不要重写此方法***.</p></div></div></div></li></ul></div></div></div>
