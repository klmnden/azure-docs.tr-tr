---
title: "Verenin adı ve verenin anahtarı BizTalk Services | Microsoft Docs"
description: "Hizmet veri yolu veya BizTalk Services erişim denetimi (ACS) verenin adı ve verenin anahtarı almak öğrenin. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 18eac72d75680ab12c4a0bea9dfc5ac8a5fce566
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk Services: Verenin Adı ve Verenin Anahtarı

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services, hizmet veri yolu verenin adı ve verenin anahtarı ve erişim denetimi verenin adı ve verenin anahtarı kullanır. Bu avantajlar şunlardır:

| Görev | Hangi verenin adı ve verenin anahtarı |
| --- | --- |
| Uygulamanızı Visual Studio'dan dağıtma |Erişim denetimi verenin adı ve verenin anahtarı |
| Azure BizTalk Services Portalı'nı yapılandırma |Erişim denetimi verenin adı ve verenin anahtarı |
| Visual Studio'da BizTalk Bağdaştırıcısı hizmetleriyle LOB geçişler oluşturma |Hizmet veri yolu verenin adı ve verenin anahtarı |

Bu konu verenin adı ve verenin anahtarı almak için adımlar listelenmektedir. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Erişim denetimi verenin adı ve verenin anahtarı
Erişim denetimi verenin adı ve verenin anahtarı aşağıdaki tarafından kullanılır:

* Visual Studio'da oluşturulan Azure BizTalk hizmeti uygulamanız: başarıyla Visual Studio'da BizTalk hizmeti uygulamanızı Azure'a dağıtmak için erişim denetimi verenin adı ve verenin anahtarı girin. 
* Azure BizTalk Services portalı: BizTalk hizmeti oluşturma ve BizTalk Services Portalı'nı açın, erişim denetimi verenin adı ve verenin anahtarı otomatik olarak dağıtımlarınızın aynı erişim denetimi değerleri ile kaydedilir.

### <a name="get-the-access-control-issuer-name-and-issuer-key"></a>Erişim denetimi verenin adı ve verenin anahtarı edinin

ACS kimlik doğrulaması için kullanmak ve verenin adı ve verenin anahtarı değerlerini almak için genel adımları içerir:

1. Yükleme [Azure Powershell cmdlet'lerini](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Azure hesabınızda ekleyin:`Add-AzureAccount`
3. Abonelik adı döndürün:`get-azuresubscription`
4. Aboneliğinizi seçin:`select-azuresubscription <name of your subscription>` 
5. Yeni bir ad alanı oluşturun:`new-azuresbnamespace <name for the service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`

    Örnek:`new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`
      
5. Yeni ACS ad alanı (Bu, birkaç dakika sürebilir) oluşturulduğunda, verenin adı ve verenin anahtarı değerlerini bağlantı dizesinde listelenmiştir: 

    ```
    Name                  : biztalksbnamespace
    Region                : South Central US
    DefaultKey            : abcdefghijklmnopqrstuvwxyz
    Status                : Active
    CreatedAt             : 10/18/2016 9:36:30 PM
    AcsManagementEndpoint : https://biztalksbnamespace-sb.accesscontrol.windows.net/
    ServiceBusEndpoint    : https://biztalksbnamespace.servicebus.windows.net/
    ConnectionString      : Endpoint=sb://biztalksbnamespace.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue=abcdefghijklmnopqrstuvwxyz
    NamespaceType         : Messaging
    ```

Özetlemek için:  
Verenin adı SharedSecretIssuer =  
Verenin anahtarı SharedSecretKey =

Daha açık [yeni AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) cmdlet'i. 

## <a name="service-bus-issuer-name-and-issuer-key"></a>Hizmet veri yolu verenin adı ve verenin anahtarı
Hizmet veri yolu verenin adı ve verenin anahtarı BizTalk Bağdaştırıcısı Hizmetleri tarafından kullanılır. BizTalk Services projenizde Visual Studio, bir şirket içi iş kolu (LOB) sistemine bağlamak için BizTalk Bağdaştırıcısı Hizmetleri kullanın. Bağlanmak için LOB geçişi oluşturmak ve LOB Sistem bilgilerinizi girin. Bunu yaparken hizmet veri yolu verenin adı ve verenin anahtarı da girin.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Hizmet veri yolu verenin adı ve verenin anahtarı almak için
1. [Azure Portal](http://portal.azure.com) oturum açın.
2. Arama **Service Bus**, ad alanınızı seçin. 
3. Açık **paylaşılan erişim ilkeleri** özelliklerini seçin, ilke ve görüntüleme **bağlantı dizesi** adı ve anahtar değerleri.  

## <a name="next"></a>Sonraki
Ek Azure BizTalk Services konuları:

* [Azure BizTalk Services SDK yükleme](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Öğretici: Azure BizTalk Hizmetleri](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk Hizmetleri](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Ayrıca Bkz.
* [Nasıl yapılır: hizmet kimlikleri yapılandırmak için ACS yönetim hizmeti kullanın](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [BizTalk Services: Geliştirici, temel, standart ve Premium sürümler grafiği](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: sağlama](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Durum Grafiğini hazırlama](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Pano, İzleyici ve Ölçek sekmeleri](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Yedekleme ve Geri Yükleme](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Azaltma](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

