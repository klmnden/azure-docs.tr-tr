---
title: Azure AD uygulama proxy'si aracılığıyla yayımlanan uygulamalar için özel bir ana sayfa ayarlama | Microsoft Docs
description: Azure AD uygulama proxy'si bağlayıcılar hakkında temel bilgiler yer almaktadır
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: a1801af582e9e4bfa82dab4b5916c164fcf3bb76
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161990"
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Azure AD uygulama proxy'si kullanarak yayımlanan uygulamalar için özel bir ana sayfa ayarlayın

Bu makalede, özel bir ana sayfa kullanıcıları yönlendirmek için uygulamaları yapılandırma anlatılmaktadır. Uygulama proxy'si ile bir uygulama yayımladığınızda, kullanıcılarınız ilk görmelisiniz sayfa olmayan bir iç URL ancak bazen ayarlayın. Özel bir ana sayfa uygulamaları eriştiklerinde, kullanıcılarınızın sağ sayfasına gidin şekilde ayarlayın. Uygulama Azure Active Directory erişim paneli veya Office 365 uygulama Başlatıcı erişim olup olmadığını kullanıcılarınızın, ayarlayabileceğiniz özel giriş sayfasını görürsünüz.

Kullanıcıların uygulama başlattığında, varsayılan olarak yayımlanan uygulama kök etki alanı URL'si yönlendirilirsiniz. Giriş sayfası genellikle giriş sayfası URL'si olarak ayarlanır. Azure AD PowerShell modülü, uygulama kullanıcılarınızın uygulama içinde belirli bir sayfada güden istediğinizde özel giriş sayfası URL'leri tanımlamak için kullanın. 

Burada, şirket özel bir ana sayfa ayarlayacaktır neden bir örneği verilmiştir:
- Kullanıcıların gidin, şirket ağı içinde *https://ExpenseApp/login/login.aspx* oturum açın ve uygulamanıza erişmek için.
- Uygulama proxy'si klasör yapısını en üst düzeyde erişmesi görüntüleri gibi diğer varlıklar olduğundan ile uygulama yayımlama *https://ExpenseApp* İç URL olarak.
- Varsayılan dış URL *https://ExpenseApp-contoso.msappproxy.net*, değil almakta kullanıcılarınıza oturum açma sayfası.  
- Ayarlama *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* giriş sayfası URL'si olarak. 

>[!NOTE]
>Kullanıcıların yayımlanan uygulamalara erişim vermek, uygulamaları görüntülenen [Azure AD erişim paneli](../active-directory-saas-access-panel-introduction.md) ve [Office 365 uygulama Başlatıcı](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).

## <a name="before-you-start"></a>Başlamadan önce

Giriş sayfası URL'si ayarlamadan önce aşağıdaki gereksinimleri göz önünde bulundurun:

* Belirttiğiniz yolda kök etki alanı URL'si bir alt etki alanı yolu olduğundan emin olun.

  Kök etki alanı URL'si, örneğin, ise https://apps.contoso.com/app1/, yapılandırdığınız giriş sayfası URL'si ile başlamalıdır https://apps.contoso.com/app1/.

* Yayımlanan uygulama bir değişiklik yaparsanız, değişiklik giriş sayfası URL'si değerini sıfırlayabilir. Giriş sayfası URL'si, uygulama gelecekte yeniden denetle güncelleştirdiğinizde ve gerekirse güncelleştirin.

## <a name="change-the-home-page-in-the-azure-portal"></a>Azure portalında giriş sayfasını değiştirme

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gidin **Azure Active Directory** > **uygulama kayıtlar** ve uygulamanızı listeden seçin. 
3. Seçin **özellikleri** ayarlarından.
4. Güncelleştirme **giriş sayfası URL'si** yeni yol ile alan. 

   ![Yeni giriş sayfası URL'si belirtin](./media/application-proxy-configure-custom-home-page/homepage.png)

5. Seçin **Kaydet**

## <a name="change-the-home-page-with-powershell"></a>PowerShell ile giriş sayfasını değiştirme

### <a name="install-the-azure-ad-powershell-module"></a>Azure AD PowerShell modülünü yükleyin

PowerShell kullanarak özel giriş sayfası URL'si tanımlamadan önce Azure AD PowerShell modülünü yükleyin. Paketten indirebilirsiniz [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), Graph API uç noktası kullanır. 

Paketi yüklemek için aşağıdaki adımları izleyin:

1. Standart bir PowerShell penceresi açın ve aşağıdaki komutu çalıştırın:

    ```
     Install-Module -Name AzureAD
    ```
    Bir yönetici olmayan komutu çalıştırıyorsanız kullanmak `-scope currentuser` seçeneği.
2. Yükleme sırasında seçin **Y** Nuget.org iki paketleri yüklemek için. Her iki paketin de gereklidir. 

### <a name="find-the-objectid-of-the-app"></a>Uygulama objectID Bul

Uygulama objectID alın ve sonra uygulama için kendi giriş sayfasına göre arayın.

1. Aynı PowerShell penceresinde Azure AD modülünü içeri aktarın.

    ```
    Import-Module AzureAD
    ```

2. Azure AD modülünü Kiracı yönetici olarak oturum açın.

    ```
    Connect-AzureAD
    ```
3. Giriş sayfası URL'sini tabanlı uygulamayı bulun. Giderek URL portalında bulabilirsiniz **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları**. Bu örnekte *sharepoint iddemo*.

    ```
    Get-AzureADApplication | where { $_.Homepage -like “sharepoint-iddemo” } | fl DisplayName, Homepage, ObjectID
    ```
4. Burada gösterilen benzer bir sonuç almak. Sonraki bölümde kullanmak için objectID GUID kopyalayın.

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-the-home-page-url"></a>Giriş sayfası URL'si güncelleştir

Giriş sayfası URL'si oluşturun ve bu değeri ile uygulamanızı güncelleştirin. Bu komutları çalıştırmak üzere aynı PowerShell penceresini kullanarak devam edin. Veya yeni bir PowerShell penceresi kullanıyorsanız, Azure AD modülü kullanarak yeniden oturum açma `Connect-AzureAD`. 

1. Doğru uygulamanız ve Değiştir onaylayın *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* önceki bölümünde kopyaladığınız objectID ile.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 Uygulama Onaylandı, giriş sayfası aşağıdaki gibi güncelleştirmek hazırsınız.

2. Yapmak istediğiniz değişiklikleri tutmak için bir boş uygulama nesnesi oluşturun. Bu değişken, güncelleştirmek istediğiniz değerleri tutar. Hiçbir şey bu adımda oluşturulur.

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. Giriş sayfası URL'si istediğiniz değerine ayarlayın. Değer yayımlanan uygulama bir alt yolu olması gerekir. Örneğin, giriş sayfası URL'den değiştirirseniz *https://sharepoint-iddemo.msappproxy.net/* için *https://sharepoint-iddemo.msappproxy.net/hybrid/*, uygulama kullanıcılarınızın doğrudan özel giriş sayfasına gidin.

    ```
    $homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. İçinde kopyaladığınız GUID (objectID) kullanarak güncelleştirme yapmak "1. adım: uygulama objectID bulunamıyor."

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. Değiştirme başarılı olduğunu doğrulamak için uygulamayı yeniden başlatın.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
>Uygulama için yaptığınız tüm değişiklikler, giriş sayfası URL'si sıfırlayabilir. Giriş sayfası URL'nizi sıfırlar, geri ayarlamak için bu bölümdeki adımları yineleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [SharePoint Azure AD uygulama proxy'si ile uzaktan erişimi etkinleştir](application-proxy-integrate-with-sharepoint-server.md)
- [Azure portalında uygulama ara sunucusunu etkinleştirme](application-proxy-enable.md)
