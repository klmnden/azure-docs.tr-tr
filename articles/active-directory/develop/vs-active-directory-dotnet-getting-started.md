---
title: "Visual Studio .NET MVC projelerinde Azure AD ile çalışmaya başlama | Microsoft Docs"
description: "Azure Active Directory .NET MVC projelerinde bağlanma veya Visual Studio kullanarak Azure AD oluşturduktan sonra kullanmaya başlamak nasıl bağlı Hizmetleri"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1c8b6a58-5144-4965-a905-625b9ee7b22b
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 07fa4655d9e7ad74cae33391d55c7c9be1d446a6
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="getting-started-with-azure-active-directory-aspnet-mvc-projects"></a>Azure Active Directory (ASP.NET MVC projeleri) ile çalışmaya başlama

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-dotnet-getting-started.md)
> - [Ne oldu](vs-active-directory-dotnet-what-happened.md)

Bir ASP.NET MVC projesinin için Active Directory ekledikten sonra bu makalede, ek yönergeler sağlanmaktadır. **Proje > Bağlantılı Hizmetler** Visual Studio komutu. Projeniz için hizmet henüz eklediyseniz, herhangi bir zamanda bunu yapabilirsiniz.

Bkz: [MVC proje için ne?](vs-active-directory-dotnet-what-happened.md) projenize bağlı hizmet eklerken değişikliklerinin.

## <a name="requiring-authentication-to-access-controllers"></a>Erişim denetleyicileri kimlik doğrulaması gerektiren

Projenizdeki tüm denetleyicileri ile donatılan `[Authorize]` özniteliği. Bu öznitelik kullanıcıya bu denetleyicileri erişmeden önce kimlik doğrulaması gerektirir. Denetleyici anonim erişime izin vermek için bu öznitelik denetleyicisinden kaldırın. Daha ayrıntılı bir düzeyde izinleri ayarlamak istiyorsanız, denetleyici sınıfı için uygulama yerine yetkilendirme gerektiren her yöntemi için özniteliğini uygulayın.

## <a name="adding-signin--signout-controls"></a>Oturum açma ekleme / SignOut denetler

Signın/SignOut denetimleri görünümünüze eklemek için kullanabileceğiniz `_LoginPartial.cshtml` işlevselliği görünümlerinizi birine eklemek için kısmi görünüm. Standart eklenen işlevselliği bir örneği burada verilmiştir `_Layout.cshtml` görünümü. (Div sınıfı navbar-Daralt ile son öğesi unutmayın):

```html
<!DOCTYPE html>
 <html>
 <head>
     <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@ViewBag.Title - My ASP.NET Application</title>
    @Styles.Render("~/Content/css")
    @Scripts.Render("~/bundles/modernizr")
</head>
<body>
    <div class="navbar navbar-inverse navbar-fixed-top">
        <div class="container">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
            </div>
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                </ul>
                @Html.Partial("_LoginPartial")
            </div>
        </div>
    </div>
    <div class="container body-content">
        @RenderBody() 
        <hr />
        <footer>
            <p>&copy; @DateTime.Now.Year - My ASP.NET Application</p>
        </footer>
    </div>
    @Scripts.Render("~/bundles/jquery")
    @Scripts.Render("~/bundles/bootstrap")
    @RenderSection("scripts", required: false)
</body>
</html>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md)
- [Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme](guidedsetups/active-directory-aspnetwebapp-v1.md)
