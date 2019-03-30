---
title: Azure AD uygulama proxy'si kullanarak yayımlanmış uygulamalar için özel bir ana sayfa ayarlama | Microsoft Docs
description: Azure AD uygulama ara sunucusu bağlayıcıları hakkında temel kavramları kapsar
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/08/2017
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0880ad2ab02fad574f5204741b0fa03e4ef0338
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648072"
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Azure AD uygulama proxy'si kullanarak yayımlanmış uygulamalar için özel bir ana sayfa ayarlayın

Bu makalede, özel bir ana sayfa kullanıcılara yönlendirmek için uygulamaları yapılandırma anlatılmaktadır. Uygulama Ara sunucusu ile bir uygulama yayımladığınızda, kullanıcılarınız ilk görmelisiniz sayfası yer almayan bir iç URL ancak bazen ayarlayın. Böylece uygulamaları eriştiklerinde, kullanıcılarınızın doğru sayfasına gidin. özel bir ana sayfa ayarlayın. Azure Active Directory erişim paneli veya Office 365 uygulama başlatıcısında uygulamaya erişmek olup olmadığını, kullanıcılarınızın sizin ayarladığınız özel giriş sayfasını görürsünüz.

Kullanıcı uygulamayı başlattığında, varsayılan olarak yayımlanan uygulama için kök etki alanı URL'si yönlendirilirsiniz. Giriş sayfası, genellikle giriş sayfası URL'si ayarlanır. Uygulama kullanıcıları uygulama içinde belirli bir sayfada yerleşmesi istediğinizde özel ana sayfa URL'lerini tanımlamak için Azure AD PowerShell modülünü kullanın. 

Bir şirket özel bir ana sayfa ayarlamalı neden bir örneği aşağıdadır:
- Kurumsal ağınızda kullanıcılar Git `https://ExpenseApp/login/login.aspx` oturum açmak ve uygulamanıza erişmek için.
- Uygulama proxy'si Klasör yapısındaki en üst düzeyinde erişmesi gereken görüntüleri gibi diğer varlıklar olduğundan ile uygulamayı yayımlama `https://ExpenseApp` İç URL.
- Varsayılan dış URL `https://ExpenseApp-contoso.msappproxy.net`, hangi kullanıcılarınıza oturum açma sayfası olması değil.  
- Ayarlama `https://ExpenseApp-contoso.msappproxy.net/login/login.aspx` olarak giriş sayfası URL'si. 

>[!NOTE]
>Kullanıcıların yayımlanan uygulamalara erişmesini sağlamak, uygulamalar görüntülenecek [Azure AD erişim paneli](../user-help/active-directory-saas-access-panel-introduction.md) ve [Office 365 uygulama başlatıcısında](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).

## <a name="before-you-start"></a>Başlamadan önce

Giriş sayfası URL'si ayarlamadan önce aşağıdaki gereksinimleri göz önünde bulundurun:

* Belirttiğiniz yolun kök etki alanı URL bir alt etki alanı yolu olduğundan emin olun.

  Kök etki alanı URL'si, örneğin, ise https://apps.contoso.com/app1/, yapılandırdığınız giriş sayfası URL'si ile başlamalıdır https://apps.contoso.com/app1/.

* Yayımlanan uygulama için bir değişiklik yaparsanız, değişiklik giriş sayfası URL'si değerini sıfırlayabilir. Giriş sayfası URL'si güncelleştirdiğinizde, bağlantı uygulama gelecekte yeniden denetlemek ve gerekirse güncelleştirin.

## <a name="change-the-home-page-in-the-azure-portal"></a>Azure portalında giriş sayfasını değiştirme

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gidin **Azure Active Directory** > **uygulama kayıtları** ve uygulamanızı listeden seçin. 
3. Seçin **özellikleri** ayarlarından.
4. Güncelleştirme **giriş sayfası URL'si** yeni yolunuzla alan. 

   ![Yeni giriş sayfası URL'sini girin](./media/application-proxy-configure-custom-home-page/homepage.png)

5. Seçin **Kaydet**

## <a name="change-the-home-page-with-powershell"></a>PowerShell ile giriş sayfasını değiştirme

### <a name="install-the-azure-ad-powershell-module"></a>Azure AD PowerShell modülünü yükleme

PowerShell kullanarak bir özel giriş sayfası URL'si tanımlamadan önce Azure AD PowerShell modülünü yükleyin. Paketten indirebileceğiniz [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), Graph API uç noktası kullanır. 

Paketi yüklemek için aşağıdaki adımları izleyin:

1. Standart bir PowerShell penceresi açın ve ardından aşağıdaki komutu çalıştırın:

    ```powershell
     Install-Module -Name AzureAD
    ```

    Komutu yönetici olmayan çalıştırıyorsanız, kullanın `-scope currentuser` seçeneği.
2. Yükleme sırasında seçin **Y** iki paketlerini Nuget.org adresinden yükleyin. Her iki paketi de gereklidir. 

### <a name="find-the-objectid-of-the-app"></a>ObjectID uygulamanın Bul

ObjectID uygulamanın alın ve ardından uygulamayı giriş sayfası arayın.

1. Aynı PowerShell penceresinde, Azure AD modülünü içeri aktarın.

    ```powershell
    Import-Module AzureAD
    ```

2. Azure AD modülünü için Kiracı Yöneticisi olarak oturum açın.

    ```powershell
    Connect-AzureAD
    ```

3. Kendi giriş sayfası URL'sini temel alarak uygulamayı bulun. URL giderek portalda bulabilirsiniz **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları**. Bu örnekte *sharepoint iddemo*.

    ```powershell
    Get-AzureADApplication | Where-Object { $_.Homepage -like "sharepoint-iddemo" } | Format-List DisplayName, Homepage, ObjectID
    ```

4. Aşağıda gösterildiği gibi bir sonuç almanız gerekir. Sonraki bölümde kullanmak için objectID GUID kopyalayın.

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-the-home-page-url"></a>Giriş sayfası URL'sini güncelleştirme

Giriş sayfası URL'si oluşturun ve bu değeri ile uygulamanızı güncelleştirin. Bu komutları çalıştırmak için aynı PowerShell penceresi kullanmaya devam edin. Veya yeni bir PowerShell penceresi kullanıyorsanız, Azure AD modülünü kullanarak tekrar oturum açma `Connect-AzureAD`. 

1. Doğru uygulamanız ve yerine onaylayın *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* ve önceki bölümde kopyaladığınız objectID ile.

    ```powershell
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

   Uygulama onayladıktan sonra giriş sayfası, şu şekilde güncelleştirmek hazırsınız.

2. Yapmak istediğiniz değişiklikleri tutmak için bir boş uygulama nesnesi oluşturun. Bu değişken, güncelleştirmek istediğiniz değerleri tutar. Hiçbir şey bu adımda oluşturulur.

    ```powershell
    $appnew = New-Object "Microsoft.Open.AzureAD.Model.Application"
    ```

3. Giriş sayfası URL'si istediğiniz değere ayarlayın. Değer yayımlanan bir uygulamanın bir alt etki alanı yolu olmalıdır. Örneğin, giriş sayfası URL değiştirirseniz `https://sharepoint-iddemo.msappproxy.net/` için `https://sharepoint-iddemo.msappproxy.net/hybrid/`, uygulama kullanıcılarının doğrudan özel giriş sayfasına gidin.

    ```powershell
    $homepage = "https://sharepoint-iddemo.msappproxy.net/hybrid/"
    ```

4. İçinde kopyaladığınız GUID (objectID) kullanılarak güncelleştirme olun "1. adım: ObjectID uygulamanın bulun."

    ```powershell
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```

5. Değişiklik başarılı olduğunu doğrulamak için uygulamayı yeniden başlatın.

    ```powershell
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
>Giriş sayfası URL'si uygulamaya yaptığınız tüm değişiklikler sıfırlayabilir. Giriş sayfası URL'nizi sıfırlarsa, geri ayarlamak için bu bölümdeki adımları yineleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD uygulama ara sunucusu ile SharePoint uzaktan erişimi etkinleştirme](application-proxy-integrate-with-sharepoint-server.md)
- [Azure portalında uygulama ara sunucusunu etkinleştirme](application-proxy-add-on-premises-application.md)