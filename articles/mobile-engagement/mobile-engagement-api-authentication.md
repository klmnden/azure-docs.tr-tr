---
title: "Mobil katılım REST API'leri ile kimlik doğrulaması"
description: "Azure Mobile Engagement REST API'leri ile kimlik doğrulaması açıklar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: b05181d9252c0a804648e01b4058019278ae5abe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Mobil katılım REST API'leri ile kimlik doğrulaması
## <a name="overview"></a>Genel Bakış
Bu belge, geçerli bir AAD Oauth Mobile Engagement REST API'leri ile kimlik doğrulaması belirteci alma açıklar. 

Geçerli bir Azure aboneliğinizin olması ve aşağıdakilerden birini kullanarak bir Mobile Engagement uygulaması oluşturdunuz varsayılır bizim [Geliştirici öğreticileri](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Kimlik Doğrulaması
Bir Microsoft Azure Active Directory token kimlik doğrulaması için kullanılan OAuth tabanlı. 

Kimlik doğrulaması bir API isteğinin sırada bir authorization üstbilgisi aşağıdaki biçimi olan her istek için eklenmesi gerekir:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Azure Active Directory belirteçleri 1 saat içinde süresi dolacak.
> 
> 

Bir belirteç almak üzere birkaç yolu vardır. API'ler genellikle bir bulut hizmetinden adlı olduğundan, bir API anahtarı kullanmak istediğiniz. API anahtarını Azure terminolojisi içinde bir hizmet asıl parola adı verilir. El ile ayarlama bir yolu aşağıdaki yordamda açıklanmıştır.

### <a name="one-time-setup-using-script"></a>Tek seferlik kurulumu (kod kullanılarak)
Kurulum için en az zaman alır, ancak en izin verilen Varsayılanları kullanan bir PowerShell Betiği kullanılarak Kurulumu gerçekleştirmek için aşağıdaki yönergeleri kümesi izlemelisiniz. İsteğe bağlı olarak, aynı zamanda yönergelerini izleyebilirsiniz [el ile kuruluma](mobile-engagement-api-authentication-manual.md) doğrudan Azure portal ve yüksekse yapılandırma bunu. 

1. Azure PowerShell en son sürümünü alın [burada](http://aka.ms/webpi-azps). Yükleme yönergeleri ile ilgili daha fazla bilgi için bu gördüğünüz [bağlantı](/powershell/azure/overview).  
2. Azure PowerShell yüklendikten sonra sahip olduğunuzdan emin olmak için aşağıdaki komutları kullanın **Azure Modülü** yüklü:
   
    a. Azure PowerShell modülü mevcut modülleri, listesinde kullanılabilir olduğundan emin olun. 
   
        Get-Module –ListAvailable 
   
    ![Mevcut Azure modülleri][1]
   
    b. Azure PowerShell modülü yukarıdaki listede bulamazsanız, aşağıdaki çalıştırmanız gerekir:
   
        Import-Module Azure 
3. Oturum açma için Azure kaynak yöneticisi için aşağıdaki komutu çalıştırın ve Azure hesabınız için kullanıcı adı ve parola sağlayarak powershell'den: 
   
        Login-AzureRmAccount
4. Birden çok aboneliğiniz varsa sonra aşağıdaki çalıştırmanız gerekir:
   
    a. Tüm aboneliklerinizi listesini almak ve kullanmak istediğiniz aboneliği Subscriptionıd kopyalayın. Bu abonelik API'leri kullanılarak ile etkileşimde olacak mobil katılım uygulamasını olduğu adla aynı olduğundan emin olun. 
   
        Get-AzureRmSubscription
   
    b. Kullanılacak aboneliği yapılandırmak için Subscriptionıd sağlayarak aşağıdaki komutu çalıştırın.
   
        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Metni kopyalayın [yeni AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) yerel makinenize betik ve PowerShell cmdlet'ini olarak kaydedin (örneğin `APIAuth.ps1`) ve bu yürütme `.\APIAuth.ps1`. 
6. Komut dosyası için bir giriş sağlama ister **principalName**. Active Directory uygulamanız (örneğin APIAuth) oluşturmak için kullanmak istediğiniz uygun bir ad sağlayın. 
7. Komut dosyası tamamlandıktan sonra aşağıdaki görüntüleyecektir program aracılığıyla AD ile kimlik doğrulaması için gereken dört değerden böylece bunları kopyalamak emin olun. 
   
    **Tenantıd**, **Subscriptionıd**, **ApplicationId**, ve **gizli**.
   
    Tenantıd olarak kullanacağınız `{TENANT_ID}`, ApplicationId olarak `{CLIENT_ID}` ve gizli olarak `{CLIENT_SECRET}`.
   
   > [!NOTE]
   > Varsayılan güvenlik ilkenizin bir PowerShell komut dosyalarının çalışmasını engelleyebilir. Bu durumda, geçici olarak komut dosyası yürütme aşağıdaki komutu kullanarak izin vermek için yürütme ilkesi yapılandırın:
   > 
   > Set-ExecutionPolicy RemoteSigned
   > 
   > 
8. İşte nasıl PS cmdlet'leri kümesini aşağıdaki gibidir. 
   
    ![][3]
9. Azure Yönetim Portalı'nda yeni bir AD uygulamasını adlı komut dosyasına verdiğiniz adla oluşturulduğunu denetleyin **principalName** altında **Şirketimin sahip olduğu uygulamalar Göster**.
   
    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Geçerli bir belirteci almak için adımlar
1. API aşağıdaki parametrelerle çağırın ve KİRACI değiştirdiğinizden emin olun\_kimliği, istemci\_kimliği ve istemci\_gizli anahtarı:
   
   * **İstek URL'si** olarak *https://login.microsoftonline.com/ {KİRACI\_kimliği} / oauth2/belirteci*
   * **HTTP Content-Type üstbilgisi** olarak *uygulama/x-www-form-urlencoded*
   * **HTTP isteği gövdesinin** olarak *vermek\_türü = istemci\_kimlik bilgileri & client_id = {istemci\_kimliği} & client_secret = {istemci\_gizli} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*
     
     Bir örnek isteği verilmiştir:
     
       POST / {TENANT_ID} / oauth2/token HTTP/1.1 ana: login.microsoftonline.com Content-Type: uygulama/x-www-form-urlencoded grant_type client_credentials & client_id = {CLIENT_ID} = & client_secret {CLIENT_SECRET} = & çözüm kaynağına https % 3A % = 2f%2Fmanagement.Core.Windows.NET%2f
     
     Bir örnek yanıt şöyledir:
     
       HTTP/1.1 200 Tamam Content-Type: uygulama/json; Charset = utf-8 İçerik-Uzunluk: 1234
     
       {"token_type": "Bearer", "expires_in": "3599", "expires_on": "1445395811", "not_before": "144 5391911","kaynak": "https://management.core.windows.net/", "access_token": {ACCESS_TOKEN}}
     
     Bu örnek POST parametreleri, URL kodlaması dahil `resource` değerdir gerçekten `https://management.core.windows.net/`. Ayrıca URL dikkatli olun kodlamak `{CLIENT_SECRET}` gibi özel karakterleri içerebilir.
     
     > [!NOTE]
     > Test etmek için bir HTTP istemci aracı gibi kullanabilirsiniz [Fiddler](http://www.telerik.com/fiddler) veya [Chrome Postman uzantısı](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 
     > 
     > 
2. Artık her API çağrısı yetkilendirme istek üstbilgisi içerir:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    Döndürülen bir 401 durum kodu alırsanız, yanıt gövdesini denetleyin, belirtecin süresi doldu size. Bu durumda, yeni belirteç alın.

## <a name="using-the-apis"></a>API'ler kullanma
Geçerli bir belirteci sahip olduğunuza göre API çağrılarını yapmaya hazır olursunuz.

1. Her API isteği önceki bölümde aldığınız bir geçerli, süresi dolmamış belirteci geçmesi gerekir.
2. Bazı parametreler, uygulamanızın tanımlayan URI isteği içine takın gerekecektir. İstek URI'si aşağıdaki gibi görünür
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    Parametreleri Al, uygulama adına tıklayın ve Pano ve tüm 3 parametrelerle aşağıdaki gibi bir sayfa görürsünüz.
   
   * **1** `{subscription-id}`
   * **2** `{app-collection}`
   * **3** `{app-resource-name}`
   * **4** bilgisayarınızı kaynak grubu adı olması geçmeye **MobileEngagement** yeni bir tane oluşturduğunuz sürece. 
     
     ![Mobile Engagement API URI parametreleri][2]

> [!NOTE]
> <br/>
> 
> 1. Önceki yönelik API'ler, bu gibi API kök adresi yoksay.<br/>
> 2. Azure Klasik portalı kullanarak uygulama oluşturduysanız, uygulama adı kendisini farklı uygulama kaynağı adı kullanmanız gerekir. Azure Portalı'nda uygulama oluşturduysanız, uygulama adı kendisini (Uygulama kaynağı adı ve yeni portalında oluşturulan uygulamalar için uygulama adı arasında hiçbir ayrım yoktur) kullanmanız gerekir.  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



