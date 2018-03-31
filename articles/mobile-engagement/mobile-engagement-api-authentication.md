---
title: Mobil katılım REST API'leri ile kimlik doğrulaması
description: Azure Mobile Engagement REST API'leri ile kimlik doğrulaması açıklar
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 5979ded9afaa31054f835b5f16fe525809f5730d
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Mobil katılım REST API'leri ile kimlik doğrulaması
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

## <a name="overview"></a>Genel Bakış

Bu belge, geçerli bir Azure Active Directory (Azure AD) OAuth Mobile Engagement REST API'leri ile kimlik doğrulaması belirteci alma açıklar.

Geçerli bir Azure aboneliğiniz ve Mobile Engagement uygulaması birini kullanarak oluşturduysanız Bu yordam varsayar [Geliştirici öğreticileri](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Kimlik Doğrulaması

Microsoft Azure Active Directory tabanlı OAuth belirteci kimlik doğrulaması için kullanılır. 

Bir API isteği kimliğini doğrulamak için her istek için bir authorization üstbilgisi eklenmesi gerekir. Yetkilendirme üst bilgi aşağıdaki biçimdedir:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Azure Active Directory belirteçleri bir saat içinde süresi dolacak.
> 
> 

Bir belirteç almak üzere birkaç yolu vardır. API'ler bir bulut hizmetinden denir çünkü bir API anahtarı kullanmak istediğiniz. API anahtarını Azure terminolojisi içinde bir hizmet sorumlusu parola olarak adlandırılır. Aşağıdaki yordam el ile ayarlamak için yollarından biri açıklanır.

### <a name="one-time-setup-using-a-script"></a>Tek seferlik kurulumu (bir komut dosyası kullanarak)

Bir PowerShell komut dosyası kullanarak kurulumu gerçekleştirmek için aşağıdaki yönergelerde yer adımları uygulayın. Bir PowerShell komut dosyası kurulum için en az miktarda zaman gerektiriyor, ancak izin verilen en Varsayılanları kullanır. 

İsteğe bağlı olarak, aynı zamanda yönergelerini izleyebilirsiniz [el ile kuruluma](mobile-engagement-api-authentication-manual.md) doğrudan bunu Azure portalından. Azure portalından ayarlarken, daha ayrıntılı bir yapılandırma yapabilirsiniz.

1. Azure PowerShell ile en son sürümünü alın [karşıdan](http://aka.ms/webpi-azps). Yükleme yönergeleri hakkında daha fazla bilgi için bkz: [bu genel bakışta](/powershell/azure/overview).

2. PowerShell yüklendikten sonra olmasını sağlamak için aşağıdaki komutları kullanın **Azure Modülü** yüklü:

    a. Azure PowerShell modülü mevcut modülleri, listesinde kullanılabilir olduğundan emin olun.

        Get-Module –ListAvailable

    ![Mevcut Azure modülleri][1]

    b. Azure PowerShell modülü önceki listede bulamazsanız, çalıştırmanız gerekir:

        Import-Module Azure
3. Azure Resource Manager için aşağıdaki komutu çalıştırarak Powershell'den oturum açın. Azure hesabınız için kullanıcı adını ve parolasını sağlayın: 

        Login-AzureRmAccount
4. Birden çok aboneliğiniz varsa, aşağıdaki adımları uygulayın:

    a. Tüm Aboneliklerin listesini alın. Ardından kopyalama **Subscriptionıd** kullanmak istediğiniz abonelik. Bu abonelik Mobile Engagement uygulaması olduğundan emin olun. API'leri ile etkileşim kurmak için bu uygulamayı kullanmak için adımıdır. 

        Get-AzureRmSubscription

    b. Aşağıdaki komutu çalıştırın. Sağlamak **Subscriptionıd** kullanacağınız abonelik yapılandırmak için:

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Metni kopyalayın [yeni AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) yerel makinenize komut dosyası. PowerShell cmdlet'ini olarak kaydedin (örneğin, `APIAuth.ps1`), ve çalıştırın.

         `.\APIAuth.ps1`.

6. Komut dosyası için bir giriş sağlama ister **principalName**. Active Directory uygulamanız için (örneğin, APIAuth) kullanmak istediğiniz uygun bir ad sağlayın. 

7. Betiğin çalışması bittikten sonra aşağıdaki dört değerden görüntüler. Program aracılığıyla Active Directory ile kimlik doğrulaması gerektiği için bunları kopyaladığınızdan emin olun: 

   - **TenantId**
   - **SubscriptionId**
   - **ApplicationId**
   - **Gizli dizi**

   Tenantıd olarak kullandığınız `{TENANT_ID}`, ApplicationId olarak `{CLIENT_ID}` ve gizli olarak `{CLIENT_SECRET}`.

   > [!NOTE]
   > Varsayılan güvenlik ilkeniz PowerShell betikleri çalışmasını engelleyebilir. Bu durumda, geçici olarak komut dosyası yürütme izin vermek için yürütme İlkesi yapılandırmak için aşağıdaki komutu kullanın:
   > 
   > Set-ExecutionPolicy RemoteSigned
8. PowerShell cmdlet'leri kümesini nasıl göründüğünü aşağıda verilmiştir.
    ![PowerShell cmdlet'leri][3]
9. Azure portalında Active Directory'ye seçin Git **uygulama kayıtlar**, ve arama emin olmak, uygulamanız için bulunmaktadır.
    ![Uygulamanız için arama][4]

### <a name="steps-to-get-a-valid-token"></a>Geçerli bir belirteci almak için adımlar

1. API aşağıdaki parametrelerle çağırın. Değiştirdiğinizden emin olun **KİRACI\_kimliği**, **istemci\_kimliği**, ve **istemci\_gizli**:
   
   * **İstek URL'si** olarak `https://login.microsoftonline.com/{TENANT_ID}/oauth2/token`

   * **HTTP Content-Type üstbilgisi** olarak `application/x-www-form-urlencoded`
   
   * **HTTP istek gövdesi** olarak `grant_type=client\_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&resource=https%3A%2F%2Fmanagement.core.windows.net%2F`
     
    Bir örnek isteği verilmiştir:
    ```
    POST /{TENANT_ID}/oauth2/token HTTP/1.1
    Host: login.microsoftonline.com
    Content-Type: application/x-www-form-urlencoded
    grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
    urce=https%3A%2F%2Fmanagement.core.windows.net%2F
    ```
    Bir örnek yanıt aşağıdadır:
    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Content-Length: 1234

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
    5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}
    ```
     Bu örnek, POST parametreleri URL kodlaması içeren `resource` değerdir gerçekten `https://management.core.windows.net/`. Ayrıca URL-kodlamak için dikkatli olun `{CLIENT_SECRET}`, özel karakterler içerebilir.

     > [!NOTE]
     > Test etmek için bir HTTP istemci aracı gibi kullanabileceğiniz [Fiddler](http://www.telerik.com/fiddler) veya [Chrome Postman uzantısı](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop). 
     > 
     > 
2. Artık her API çağrısı yetkilendirme istek üstbilgisi içerir:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    İsteğiniz bir 401 durum kodu döndürürse, yanıt gövdesini denetleyin. Bu belirtecin süresi söyleyebilir. Bu durumda, yeni belirteç alın.

## <a name="use-the-apis"></a>API'leri kullanın
Geçerli bir belirteci sahip olduğunuza göre API çağrılarını yapmaya hazır olursunuz.

1. Her API isteği geçerli, süresi dolmamış belirteç göndermesi gerekir. Önceki bölümde süresi dolmamış belirteci alınır.

2. Bazı parametreler, uygulamanızın tanımlayan URI isteğine takın. İstek URI'si aşağıdaki kod gibi görünür:
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    Parametreleri almak için uygulamanızın adı seçin. Ardından **Pano**. Aşağıdaki gibi tüm üç parametre içeren bir sayfa görürsünüz:
   
   * **1** `{subscription-id}`
   * **2** `{app-collection}`
   * **3** `{app-resource-name}`
   * **4** bilgisayarınızı kaynak grubu adı olması geçmeye **MobileEngagement** yeni bir tane oluşturduğunuz sürece. 

> [!NOTE]
> Önceki API'ler olduğundan API kök adresi yoksay.
> 
> Azure portalını kullanarak uygulama oluşturduysanız, uygulama adı kendisini farklı uygulama kaynak adını kullanın gerekir. Azure portalında uygulama oluşturduysanız, uygulama adı kullanmanız gerekir. (Uygulama kaynağı adı ve yeni Portalı'nda oluşturulan uygulamaları için uygulama adı arasında hiçbir ayrım yoktur.)
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/search-app.png



