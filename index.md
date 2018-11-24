 
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

#### header.layout.html (в приложении Сайт - Дизайн)

```smarty
 <!--nocache1-->
 {if $wa->isAuthEnabled()}
     <div class="s-column right">

         <ul class="s-nav-list">
             {if $wa->user()->isAuth()}
                 <li>
                     {$_name = $wa->user("firstname")}
                     {$_url = $wa->getUrl('/frontend/myProfile')}
                     {if $wa->shop}
                         {$_url = $wa->getUrl('shop/frontend/myOrders')}
                     {/if}

                     <a class="s-link" href="{$_url}">
                         <i class="icon16 image" style="background-image: url('{$wa->user()->getPhoto2x(20)}');"></i>
                         <span>{$_name|escape}</span>
                     </a>
                 </li>
             {else}
                 <li>
                     {$_url = $wa->getUrl('/frontend/myProfile')}
                     {if $wa->shop}
                         {$_url = $wa->getUrl('shop/frontend/myOrders')}
                     {/if}
                     <a class="s-link" href="{$_url}">
                         <i class="svg-icon entrance size-16"></i> <span>[`Login`]</span>
                     </a>
                 </li>
                 <li>
                     <a href="{$wa->signupUrl()}">[`Sign up`]</a>
                 </li>
             {/if}
         </ul>

     </div>
 {/if}
 <!--nocache1-->
```
#### pane.html (в приложении Сайт - Дизайн)

```smarty
<div class="s-column right middle">
    {if $wa->shop}
    <!--nocache1-->
        {$_cart_total = $wa->shop->cart->total()}
        {$_cart_count = $wa->shop->cart->count()}
        {$_price = "[`Empty`]"}
        {if !empty($_cart_total)}
            {$_price = wa_currency_html($_cart_total, $wa->shop->currency())}
        {elseif !empty($_cart_count)}
            {$_price = wa_currency_html(0, $wa->shop->currency())}
        {/if}

        <div class="s-pane-item {if !empty($_cart_count)}with-hover{/if}">

            <div class="s-cart-wrapper {if empty($_cart_count)}is-empty{/if}" id="js-cart-wrapper">
                <span class="s-label"><i class="icon16 cart"></i> [`Cart`] </span>
                <span class="s-count js-cart-count">{if !empty($_cart_count)}{$_cart_count}{else}0{/if}</span>
                <span class="s-price js-cart-price">{$_price}</span>
                <a class="s-button" href="{$wa->getUrl('shop/frontend/cart')}">[`Place order`]</a>

                <script>
                    ( function($, waTheme) {
                        var $cart = $("#js-cart-wrapper"),
                            $item = $cart.closest(".s-pane-item");

                        waTheme.apps["shop"].cart = new window.waTheme.init.shop.Cart({
                            $wrapper: $cart,
                            count: {if !empty($_cart_count)}{$_cart_count|intval}{else}0{/if}
                        });

                        waTheme.apps["shop"].cart.onChange( function(cart) {
                            var hover_class = "with-hover";
                            if (cart.count > 0) {
                                $item.addClass(hover_class);
                            } else {
                                $item.removeClass(hover_class);
                            }
                        });
                    })(jQuery, window.waTheme);
                </script>
            </div>

            <a class="s-link" href="{$wa->getUrl('shop/frontend/cart')}" title="[`Cart`]"></a>

        </div>
    <!--nocache1-->    
    {/if}
```
