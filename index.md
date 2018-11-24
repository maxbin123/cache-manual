 
## Необходимые изменения в шаблонах 

### Default

#### index.html

```smarty
     </ul>
     {/strip}

     <div class="clear-both"></div>
 </nav>

 <div class="column wa-flex-box middle">
 <!--nocache1-->
     {$_hide_cart = $wa->globals("hideCart")}
     {if $wa->shop && empty($_hide_cart)}
         <!-- cart -->
         {$cart_total = $wa->shop->cart->total()}
         <div id="cart" class="cart{if !$cart_total} empty{/if}">
             <a href="{$_cart_url}" class="cart-summary">
                 <i class="cart-icon"></i>
                 <strong class="cart-total">{wa_currency_html($cart_total, $wa->shop->currency())}</strong>
             </a>
             <div id="cart-content">
                 {* <div class="cart-just-added">
                    %s is now in your shopping cart
                 </div> *}
             </div>
             <a href="{$_cart_url}" class="cart-to-checkout" style="display: none;">
                 [s`View cart`]
             </a>
         </div>
     {/if}

     {if $wa->isAuthEnabled()}
         <!-- user auth -->
         {strip}
         <ul class="auth" id="js-header-auth-wrapper">
             {if $wa->user()->isAuth()}
                 {if $wa->myUrl()}
                     <li{if $wa->globals('isMyAccount')} class="bold"{/if}>
                         <a href="{$wa->myUrl()}" class="not-visited"><i class="icon16 userpic20 float-left" style="background-image: url('{$wa->user()->getPhoto2x(20)}');"></i> <strong>{$wa->user('firstname')|default:'[`My account`]'}</strong></a>
                     </li>
                 {else}
                     <li><strong>{$wa->user('firstname')|default:'[`My account`]'}</strong></li>
                 {/if}
                 <li><a href="?logout" class="not-visited">[s`Log out`]</a></li>
             {else}
                 <li><a href="{$wa->loginUrl()}" class="not-visited">[s`Log in`]</a></li>
                 <li><a href="{$wa->signupUrl()}" class="not-visited">[s`Sign up`]</a></li>
             {/if}
         </ul>
         {/strip}
     {/if}
  <!--nocache1-->
     <button id="mobile-nav-toggle"></button>

```

Там же в index.html проверьте, если есть блок (появился в последних версиях), то его тоже надо окружить тегами:

```smarty
{if $wa->shop}
<!--nocache2-->
    {if method_exists($wa->shop, 'checkout')}
        {$_cart_url = $wa->shop->checkout()->cartUrl()}
    {else}
        {$_cart_url = $wa->getUrl('shop/frontend/cart')}
    {/if}
<!--nocache2-->
{/if}
{/strip}
```

### Гипермаркет

