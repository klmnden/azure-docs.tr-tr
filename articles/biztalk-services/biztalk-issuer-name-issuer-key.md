---
title: Verenin adı ve verenin anahtarı BizTalk Services | Microsoft Docs
description: Service Bus'ı veya BizTalk Services, Access Control (ACS) için verenin adı ve verenin anahtarı alma hakkında bilgi edinin. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: eb5b4b3741b064a934833b3094c69db85e9ccabb
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51238718"
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk Services: Verenin Adı ve Verenin Anahtarı

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services, Service Bus verenin adı ve verenin anahtarı ve erişim denetimi verenin adı ve verenin anahtarı kullanır. Bu avantajlar şunlardır:

| Görev | Hangi verenin adı ve verenin anahtarı |
| --- | --- |
| Uygulamanızı Visual Studio'dan dağıtma |Erişim denetimi verenin adı ve verenin anahtarı |
| Azure BizTalk Hizmetleri portalını yapılandırma |Erişim denetimi verenin adı ve verenin anahtarı |
| Visual Studio'daki BizTalk bağdaştırıcı Hizmetleri ile LOB geçişleri oluşturma |Hizmet veri yolu yayıncı adı ve verenin anahtarı |

Bu konu, verenin adı ve verenin anahtarı alma adımları listeler. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Erişim denetimi verenin adı ve verenin anahtarı
Erişim denetimi verenin adı ve verenin anahtarı için aşağıdaki adımları kullanılır:

* Azure BizTalk hizmeti uygulamanızı Visual Studio'da oluşturulan: başarıyla Visual Studio'daki BizTalk hizmeti uygulamanızı Azure'a dağıtmak için erişim denetimi verenin adı ve verenin anahtarı girin. 
* Azure BizTalk Hizmetleri portalını: BizTalk hizmeti oluşturma ve BizTalk Hizmetleri portalını açın, erişim denetimi verenin adı ve verenin anahtarı otomatik olarak aynı erişim denetimi değerlerle dağıtımlarınız için kaydedilir.

### <a name="get-the-access-control-issuer-name-and-issuer-key"></a>Erişim denetimi verenin adı ve verenin anahtarı alma

ACS kimlik doğrulaması için kullanmak ve sertifikayı verenin adı ve verenin anahtarı değerlerini almak için genel adımlar şunlardır:

1. Yükleme [Azure Powershell cmdlet'lerini](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Azure hesabınızı ekleyin: `Add-AzureAccount`
3. Abonelik adınız döndürür: `get-azuresubscription`
4. Aboneliğinizi seçin: `select-azuresubscription <name of your subscription>` 
5. Yeni bir ad alanı oluşturun: `new-azuresbnamespace <name for the service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`

    Örnek:    `new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`
      
5. (Bu işlem birkaç dakika sürebilir) yeni ACS ad alanı oluşturulduğunda, verenin adı ve verenin anahtarı değerleri bağlantı dizesinde listelenmiştir: 

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

Özetlersek:  
Verenin adı SharedSecretIssuer =  
Yayıncı SharedSecretKey =

Daha açık [yeni AzureSBNamespace](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azuresbnamespace) cmdlet'i. 

## <a name="service-bus-issuer-name-and-issuer-key"></a>Hizmet veri yolu yayıncı adı ve verenin anahtarı
Hizmet veri yolu yayıncı adı ve verenin anahtarı BizTalk bağdaştırıcı Hizmetleri tarafından kullanılır. BizTalk Services projenizde Visual Studio, bir şirket içi iş kolu (LOB) sistemine bağlamak için BizTalk bağdaştırıcı Hizmetleri kullanın. Bağlanmak için LOB geçişi oluşturmak ve LOB sistemi ayrıntılarınızı girin. Bunu yaparken Service Bus verenin adı ve verenin anahtarı da girin.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Service Bus verenin adı ve verenin anahtarı almak için
1. [Azure Portal](http://portal.azure.com) oturum açın.
2. Arama **Service Bus**, ad alanınızı seçin. 
3. Açık **paylaşılan erişim ilkeleri** özellikler, ilkenizi seçin ve görüntüleme **bağlantı dizesi** adını ve anahtar değerlerini için.  

## <a name="next"></a>Sonraki
Ek Azure BizTalk Services konular:

* [Azure BizTalk Hizmetleri SDK'sını yükleme](https://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Öğretici: Azure BizTalk Hizmetleri](https://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](https://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk Hizmetleri](https://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Ayrıca Bkz.
* [Nasıl yapılır: hizmet kimlikleri yapılandırmak için ACS yönetim hizmeti kullanın](https://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [BizTalk Services: Geliştirici, temel, standart ve Premium sürümler grafiği](https://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: sağlama](https://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Durum Grafiğini hazırlama](https://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Pano, İzleyici ve Ölçek sekmeleri](https://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Yedekleme ve Geri Yükleme](https://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Azaltma](https://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

