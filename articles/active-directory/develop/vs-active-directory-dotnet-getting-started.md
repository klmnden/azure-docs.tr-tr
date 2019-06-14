---
title: Azure AD'de Visual Studio .NET MVC projelerini kullanmaya başlama
description: Bağlı hizmetler nasıl bağlanmak veya Visual Studio kullanarak Azure AD'yi oluşturduktan sonra Azure Active Directory içinde .NET MVC projelerini kullanmaya başlama
services: active-directory
author: ghogen
manager: douge
ms.assetid: 1c8b6a58-5144-4965-a905-625b9ee7b22b
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6bbbef3f48eb55c863fdd286113297d79f9b9e02
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60296852"
---
# <a name="getting-started-with-azure-active-directory-aspnet-mvc-projects"></a>Azure Active Directory (ASP.NET MVC projeleri) ile çalışmaya başlama

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-dotnet-getting-started.md)
> - [Ne oldu](vs-active-directory-dotnet-what-happened.md)

Active Directory bir ASP.NET MVC projeye ekledikten sonra bu makalede ek yönergeler sunulmuştur **Proje > bağlı hizmetler** Visual Studio'nun komutu. Hizmet projenize henüz eklediyseniz, herhangi bir zamanda bunu yapabilirsiniz.

Bkz: [MVC projeme ne oldu?](vs-active-directory-dotnet-what-happened.md) projenize bağlı hizmet ekleme sırasında yapılan değişiklikler için.

## <a name="requiring-authentication-to-access-controllers"></a>Erişim denetleyicileri kimlik doğrulaması gerektiren

Projenizdeki tüm denetleyicileri ile donatılmış `[Authorize]` özniteliği. Bu öznitelik bu denetleyicileri erişmeden önce doğrulanmasını gerektirir. Anonim olarak erişim için denetleyici izin vermek için bu öznitelik denetleyicisinden kaldırın. Daha ayrıntılı bir düzeyde izinleri ayarlamak istiyorsanız, denetleyici sınıfı için uygulama yerine yetkilendirme gerektiren her yöntem için özniteliğini uygulayın.

## <a name="adding-signin--signout-controls"></a>Ekleme Signın / SignOut denetler

Görünümünüze Signın/SignOut denetimler eklemek için kullanabileceğiniz `_LoginPartial.cshtml` görünümlerinizde birine işlevselliği eklemek için kısmi görünüm. İşte bir örnek standart eklenen işlevlerin `_Layout.cshtml` görünümü. (Sayı ile sınıf gezinti çubuğunu Daralt içindeki son öğeden unutmayın):

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

- [Azure Active Directory için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme](quickstart-v1-aspnet-webapp.md)
