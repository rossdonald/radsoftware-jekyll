---
title: Release Notes - Azimuth Improvements and New Integrations
subtitle: Edited in the Net cms.
excerpt: Vis accumsan feugiat adipiscing nisl amet adipiscing accumsan blandit
  accumsan sapien blandit ac amet faucibus aliquet placerat commodo.
layout: post
date: 2017-03-26
thumb_img_path: images/stock/1_thumb.jpg
---
Changed the stylesheet for code blocks now uses `Native` styles.

As below if you use `AccessTokenFormat` then you see that it will need to implement `ISecureDataFormat` to be valid.

## Vis accumsan feugiat 

Adipiscing *nisl* amet **adipiscing** accumsan blandit accumsan sapien blandit ac amet faucibus aliquet placerat commodo. Interdum ante aliquet commodo accumsan vis phasellus adipiscing. Ornare a in lacinia. Vestibulum accumsan ac metus massa tempor. Accumsan in lacinia ornare massa amet. 

**Indented Code Block**

    return new UserInfoViewModel
    {
        Email = User.Identity.GetUserName(),
        HasRegistered = externalLogin == null,
        LoginProvider = externalLogin != null ? externalLogin.LoginProvider : null
    };

> A Quote. Ac interdum ac non praesent. Cubilia lacinia interdum massa faucibus blandit nullam. Accumsan phasellus nunc integer. Accumsan euismod nunc adipiscing lacinia erat ut sit. Arcu amet. Id massa aliquet arcu accumsan lorem amet accumsan.

```c#
public ISecureDataFormat<AuthenticationTicket> AccessTokenFormat { get; private set; }

// GET api/Account/UserInfo
[HostAuthentication(DefaultAuthenticationTypes.ExternalBearer)]
[Route("UserInfo")]
public UserInfoViewModel GetUserInfo()
{
    ExternalLoginData externalLogin = ExternalLoginData.FromIdentity(User.Identity as ClaimsIdentity);

    return new UserInfoViewModel
    {
        Email = User.Identity.GetUserName(),
        HasRegistered = externalLogin == null,
        LoginProvider = externalLogin != null ? externalLogin.LoginProvider : null
    };
}

// POST api/Account/Logout
[Route("Logout")]
public IHttpActionResult Logout()
{
    Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    return Ok();
}
```

Amet nibh adipiscing adipiscing. Commodo ante vis placerat interdum massa massa primis. Tempus condimentum tempus non ac varius cubilia adipiscing placerat lorem turpis at. Aliquet lorem porttitor interdum. Amet lacus. Aliquam lobortis faucibus blandit ac phasellus. In amet magna non interdum volutpat porttitor metus a ante ac neque. Nisi turpis. Commodo col. Interdum adipiscing mollis ut aliquam id ante adipiscing commodo integer arcu amet Ac interdum ac non praesent. Cubilia lacinia interdum massa faucibus blandit nullam. Accumsan phasellus nunc integer. Accumsan euismod nunc adipiscing lacinia erat ut sit. Arcu amet. Id massa aliquet arcu accumsan lorem amet accumsan commodo odio cubilia ac eu interdum placerat placerat arcu commodo lobortis adipiscing semper ornare pellentesque.

Javascript code

```js
// This has a complexity linear to the value of the code. The
// assumption is that looking up astral identifier characters is
// rare.
function isInAstralSet(code, set) {
  var pos = 0x10000;
  for (var i = 0; i < set.length; i += 2) {
    pos += set[i];
    if (pos > code) { return false }
    pos += set[i + 1];
    if (pos >= code) { return true }
  }
}
```

Some TypeScript

```ts
import ProductDto from "./ProductDto";

export default class CategoryDto {
  categoryId: number = 0;

  categoryName: string = "";

  description: string | null = null;

  picture: string | null = null;

  productList: ProductDto[] = [];
}
```

Amet nibh adipiscing adipiscing. Commodo ante vis placerat interdum massa massa primis. Tempus condimentum tempus non ac varius cubilia adipiscing placerat lorem turpis at. Aliquet lorem porttitor interdum. Amet lacus. Aliquam lobortis faucibus blandit ac phasellus. In amet magna non interdum volutpat porttitor metus a ante ac neque. Nisi turpis. Commodo col. Interdum adipiscing mollis ut aliquam id ante adipiscing commodo integer arcu amet blandit adipiscing arcu ante.