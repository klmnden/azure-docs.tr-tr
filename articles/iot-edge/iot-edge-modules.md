---
title: "Azure IOT kenar modülleri anlama | Microsoft Docs"
description: "Azure IOT kenar modülleri ve nasıl yapılandırılacağı hakkında bilgi edinin"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 02/15/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 0f3ce7496427b6975eb4ac476e7d1737321ed2e9
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="understand-azure-iot-edge-modules---preview"></a>Azure IOT kenar modülleri anlamak - Önizleme

Azure IOT kenar dağıtmanıza ve yönetmenize olanak sağlar biçiminde kenar iş mantığına *modülleri*. Azure IOT kenar modülleri IOT kenar tarafından yönetilen hesaplamanın en küçük birim olduğunuz ve Azure Hizmetleri (örneğin, Azure akış analizi) veya kendi çözüm özgü kod içerebilir. Anlamak için nasıl modülleri geliştirilen, dağıtılan ve korunan bir modül yapmak dört kavramsal parça düşünmek yardımcı olur:

* A **modülü görüntü** bir modül tanımlar ve yazılımı içeren bir paket.
* A **modül örneğinin** belirli modülü görüntünün bir IOT kenar cihazda çalışan hesaplama birimidir. Modül örneğinin IOT kenar çalışma zamanı tarafından başlatılır.
* A **modül kimliği** her modül örneğine ilişkili IOT Hub depolanan bilgileri (güvenlik kimlik bilgileri dahil) bir parçasıdır.
* A **modülü twin** meta verileri, yapılandırmaları ve koşullar dahil olmak üzere bir modül örneğinin durumu bilgilerini içeren IOT Hub depolanan bir JSON belge. 

## <a name="module-images-and-instances"></a>Modül görüntüler ve örnekler

IOT kenar modülü görüntü yönetimi, güvenlik ve IOT kenar çalışma zamanı iletişimi özelliklerini yararlanmak uygulamalar içerir. Kendi modülü görüntüleri geliştirmek veya bir Azure akış analizi gibi desteklenen bir Azure hizmet verebilirsiniz.
Görüntüleri bulutta mevcut ve bunlar güncelleştirildi, değiştirilebilir ve farklı çözümlerinde dağıtılır. Örneğin, üretim satırı çıkışı tahmin etmek öğrenme makine kullanan bir modül bilgisayar görme bir drone denetlemek için kullandığı bir modül'den ayrı bir görüntü olarak var. 

Bir modül görüntüsü bir aygıta dağıtılan ve IOT kenar çalışma zamanı tarafından başlatılan her zaman bu modülü yeni bir örneğini oluşturulur. Dünya farklı kısımlarını iki aygıtları aynı modülü görüntüsü kullanabilirsiniz; Ancak, modül cihazda başlatıldığında her kendi modül örneğinin sahip olacaktır. 

![Modül bulut - görüntülerinde cihazlarda modülü örnekleri][1]

Uygulamasında, bir havuzda kapsayıcı görüntüleri olarak modülleri görüntüler var ve modülü örnekleri cihazlarda kapsayıcılardır. Kullanım örnekleri için Azure IOT kenar arttıkça, yeni türlerini modülü görüntüler ve örnekleri oluşturulur. Örneğin, kısıtlı kaynak aygıtları dinamik bağlantı kitaplıkları ve yürütülebilir dosyalar örnekleri mevcut modülü görüntüleri gerektirmesi için kapsayıcılarını çalıştıramazsınız. 

## <a name="module-identities"></a>Modül kimlikleri

Yeni bir modül örneğinin IOT kenar çalışma zamanı tarafından oluşturulduğunda, karşılık gelen bir modül kimliği ile ilişkili bir örneğidir. Modül kimliği IOT hub'da depolanır ve adresleme ve güvenlik kapsamı, tüm yerel ve bulut iletişimleri için belirli bir modül örneğinin olarak kullanılmaz.
Cihaz kimliğini bağlıdır modülü örneği ile ilişkili kimlik hangi örneğinin çalıştığından ve sağladığınız bu modül, çözümünüz adı. Örneğin, çağırırsanız `insight` bir Azure akış analizi ve kullanan bir modül dağıtmak, olarak adlandırılan bir aygıtta `Hannover01`, IOT kenar çalışma zamanı adlı karşılık gelen bir modül kimliği oluşturur `/devices/Hannover01/modules/insight`.

Açıkçası, senaryolarda bir modül görüntüde birden çok kez aynı aygıt dağıtmak gerektiğinde farklı adlar aynı görüntü birden çok kez dağıtabilirsiniz.

![Modül kimlikleri benzersiz][2]

## <a name="module-twins"></a>Modül çiftlerini

Her modül örneğinin modülü örneği yapılandırmak için kullanabileceğiniz karşılık gelen bir modül twin de vardır. Örnek ve twin birbiriyle modül kimliği ile ilişkilidir. 

Bir modül twin modülü bilgisi ve yapılandırma özelliklerini depolayan bir JSON belgedir. Bu kavram parallels [cihaz çifti] [ lnk-device-twin] IOT hub'dan kavram. Bir modül twin yapısını tam olarak bir cihaz çifti ile aynıdır. Her iki çiftlerini türleri ile etkileşim kurmak için kullanılan API'leri aynı değildir. İkisi arasındaki tek fark, istemci SDK'sı örneği oluşturmak için kullanılan kimliğidir. 

```csharp
// Create a DeviceClient object. This DeviceClient will act on behalf of a 
// module since it is created with a module’s connection string instead 
// of a device connection string. 
DeviceClient client = new DeviceClient.CreateFromConnectionString(moduleConnectionString, settings); 
await client.OpenAsync(); 
 
// Get the model twin 
Twin twin = await client.GetTwinAsync(); 
```

## <a name="next-steps"></a>Sonraki adımlar
 - [Azure IOT kenar çalışma zamanı ve mimarisini anlama][lnk-runtime]

<!-- Images -->
[1]: ./media/iot-edge-modules/image_instance.png
[2]: ./media/iot-edge-modules/identity.png

<!-- Links -->
[lnk-device-identity]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-runtime]: iot-edge-runtime.md