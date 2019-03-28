---
title: Kimlik doğrulama yöntemleri için IOT Önizleme ASC | Microsoft Docs
description: Kullanılabilir farklı kimlik doğrulama yöntemleri hakkında ASC IOT hizmeti için kullanırken öğrenin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 10b38f20-b755-48cc-8a88-69828c17a108
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 23bc4d0df1c8124ec225ac31239c7acb3f1ab546
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541820"
---
# <a name="security-agent-authentication-methods"></a>Güvenlik aracı kimlik doğrulama yöntemleri 

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede AzureIoTSecurity aracı ile IOT hub'ı ile kimlik doğrulaması yapmak için kullanabileceğiniz farklı kimlik doğrulama yöntemleri açıklanmaktadır.

IOT Hub, IOT için ASC için her cihaz eklenmedi için bir güvenlik modülü gereklidir. ASC IOT için cihazın kimliğini doğrulamak için iki yöntemden birini kullanabilirsiniz. Mevcut IOT çözümünüz için en uygun yöntemi seçin. 

> [!div class="checklist"]
> * Güvenlik modül seçeneği
> * Cihaz seçeneği

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

İki yöntem AzureIoTSecurity aracının kimlik doğrulaması gerçekleştirmek:

 - **Modül** kimlik doğrulama modu<br>
   Modül, cihaz ikizi bağımsız olarak doğrulanır.
   Bu kimlik doğrulama türü için Authentication.config dosyası tarafından tanımlanır için gereken bilgileri C# ve c için LocalConfiguration.json
        
 - **Cihaz** kimlik doğrulama modu<br>
    Bu yöntemde, güvenlik aracı ilk karşı cihazın kimliğini doğrular. İlk kimlik doğrulamasından sonra IOT Aracısı ASC gerçekleştirir **Rest** Rest API ile cihaz kimlik doğrulama verilerini kullanarak IOT Hub'ına çağırın. ASC IOT aracı için güvenlik modülü kimlik doğrulama yöntemi ve veri ardından IOT Hub'ından ister. Son adımda ASC IOT aracısı için IOT modülü ASC karşı kimlik doğrulaması gerçekleştirir.    

Bkz: [güvenlik aracı yükleme parametrelerini](#security-agent-installation-parameters) nasıl yapılandırılacağını öğrenin.
                                
## <a name="authentication-methods-known-limitations"></a>Kimlik doğrulama yöntemleri bilinen sınırlamalar

- **Modül** kimlik doğrulama modu, yalnızca simetrik anahtarlı kimlik doğrulamayı destekler.
- CA imzalı bir sertifika tarafından desteklenmiyor **cihaz** kimlik doğrulama modu.  

## <a name="security-agent-installation-parameters"></a>Güvenlik aracı yükleme parametreleri

Zaman [güvenlik aracısı dağıtma](select-deploy-agent.md), kimlik doğrulama ayrıntıları bağımsız değişken olarak sağlanmalıdır.
Bu bağımsız değişkenler aşağıdaki tabloda belirtilmiştir.


|Parametre|Açıklama|Seçenekler|
|---------|---------------|---------------|
|**Kimlik**|Kimlik doğrulaması modu| **Modül** veya **cihaz**|
|**type**|Kimlik doğrulaması türü|**SymmetricKey** veya **SelfSignedCertificate**|
|**dosya yolu**|Sertifika veya simetrik anahtarı içeren dosyanın tam yolunu mutlak| |
|**gatewayHostname**|IOT Hub'ın FQDN'si|Örnek: ContosoIotHub.azure-devices.net|
|**cihaz kimliği**|Cihaz Kimliği|Örnek: MyDevice1|
|**certificateLocationKind**|Sertifika depolama konumu|**YerelDosya** veya **Store**|


Güvenlik aracı yükleme betiği kullanarak, aşağıdaki yapılandırmayı otomatik olarak gerçekleştirilir.

Güvenlik aracı kimlik doğrulaması'nı el ile düzenlemek için yapılandırma dosyasını düzenleyin. 

## <a name="change-authentication-method-after-deployment"></a>Dağıtımdan sonra kimlik doğrulama yöntemini değiştirme

Bir güvenlik aracı yükleme betiği ile dağıtım yaparken, bir yapılandırma dosyası otomatik olarak oluşturulur.

Kimlik doğrulama yöntemleri dağıtımdan sonra değiştirmek için yapılandırma dosyasını el ile düzenlenmesi gerekli değildir.


### <a name="c-based-security-agent"></a>C#-tabanlı güvenlik aracı

Düzen _Authentication.config_ şu parametrelerle:

```xml
<Authentication>
  <add key="deviceId" value=""/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value=""/>
  <add key="type" value=""/>
  <add key="identity" value=""/>
  <add key="certificateLocationKind" value="" />
</Authentication>
```

### <a name="c-based-security-agent"></a>C tabanlı güvenlik aracı

Düzen _LocalConfiguration.json_ şu parametrelerle:

```json
"Authentication" : {
    "Identity" : "",
    "AuthenticationMethod" : "",
    "FilePath" : "",
    "DeviceId" : "",
    "HostName" : ""
}
```

## <a name="see-also"></a>Ayrıca bkz.
- [Güvenlik aracıları genel bakış](security-agent-architecture.md)
- [Güvenlik aracı dağıtma](select-deploy-agent.md)
- [Erişim ham güvenlik verileri](how-to-security-data-access.md)
