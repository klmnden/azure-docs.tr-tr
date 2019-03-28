---
title: Nasıl yapılır yönergeleri bir ASC ASC, IOT modül ikizi için IOT Önizleme değiştirmek için | Microsoft Docs
description: IOT için bir ASC ASC IOT güvenlik modül ikizi için değiştirme hakkında bilgi edinin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 1bc5dc86-0f33-4625-b3d3-f9b6c1a54e14
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 4bb25db7feddea66e75d9dd069aaec7785faf738
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541955"
---
# <a name="modify-an-asc-for-iot-module-twin"></a>Bir ASC IOT modül ikizi için değiştirme

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, varolan yapılandırmasını değiştirmek açıklanmaktadır **AzureIoTSecurity modül ikizi** varolan bir aygıt için. 

Bkz: [IOT modül için bir ASC oluşturma](quickstart-create-security-twin.md) yeni bir cihaz için yeni bir güvenlik modülü yapma hakkında bilgi edinmek için.  

## <a name="modification-considerations"></a>Değiştirme konuları

> [!IMPORTANT]
> Her yapılandırma çifti yapılandırmasında varsayılan yapılandırmayı geçersiz kılar. Belirli bir yapılandırma artık ikizi yapılandırmada mevcut değilse varsayılan yapılandırmayı kullanılır. 

## <a name="configuration-schema-and-validation"></a>Yapılandırma şeması ve doğrulama 

Buna karşı aracı yapılandırması emin [şema](https://github.com/Azure/asc-for-iot/schema/security_module_twin). Bir aracı, yapılandırma nesnesini şema eşleşmiyorsa başlatmaz.

 
Aracı çalışırken, yapılandırma nesnesini (yapılandırma şeması eşleşmiyor) geçerli olmayan bir yapılandırma değiştirildiyse, aracı geçersiz yapılandırma yoksayar ve geçerli yapılandırmasını kullanarak devam eder. 

## <a name="edit-a-property"></a>Bir özelliğini Düzenle  

Aracı yapılandırma nesnesinin AzureIoTSecurity modül ikizi içinde içindeki tüm özel özelliklerini ayarlayın. 

Özellik ayarlamak için yapılandırma nesnesi istenen değeri içeren özellik anahtarı ekleyin. Varsayılan özellik değeri kullanmak için yapılandırma nesnesinden özelliği kaldırın:

```json
"desired": { //AzureIoTSecurity Module Identity Twin – desired properties section  
  "azureiot*com^securityAgentConfiguration^1*0*0": { //ASC for IoT Agent 
      // configuration section  
    "hubResourceId": "/subscriptions/82392767-31d3-4bd2-883d-9b60596f5f42/resourceGroups/myResourceGroup/providers/Microsoft.Devices/IotHubs/myIotHub",     
    "lowPriorityMessageFrequency": "PT1H",     
    "highPriorityMessageFrequency": "PT7M",    
    "eventPriorityFirewallConfiguration": "High",     
    "eventPriorityConnectionCreate": "Off" 
  } 
}, 
```

## <a name="properties"></a>Özellikler 
Aşağıdaki tabloda, tüm IOT aracıları ASC denetleyen yapılandırılabilir özellikleri içerir. 
          

| Ad| Durum | Geçerli değerler| Varsayılan değerler| Açıklama |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|highPriorityMessageFrequency|Gerekli: false |Geçerli değerler: ISO 8601 biçimli süresi |Varsayılan değer: PT7M |En fazla süre önce yüksek öncelikli iletiler gönderilir.|
|lowPriorityMessageFrequency |Gerekli: false|Geçerli değerler: ISO 8601 biçimli süresi |Varsayılan değer: PT5H |En fazla süre önce düşük öncelikli iletiler gönderilir.| 
|snapshotFrequency |Gerekli: false|ISO 8601 biçimli, geçerli değerler: süresi |Varsayılan değer PT13H |Cihaz durumu anlık görüntülerin oluşturulması için zaman aralığı.| 
|maxLocalCacheSizeInBytes |Gerekli: false |Geçerli değerler: |Varsayılan değer: 2560000 8192 daha büyük | Bir aracı ileti önbelleği için izin verilen en fazla depolama (bayt cinsinden). En fazla ileti gönderilmeden önce cihazda iletileri depolamak için izin verilen alanı miktarı.| 
|maxMessageSizeInBytes |Gerekli: false |Geçerli değerler: 8192, 262144 değerinden daha büyük pozitif bir sayı |Varsayılan değer: 204800 |Aracıya bulut ileti boyutu izin verilen en yüksek. Bu ayar, her iletiye gönderilen en yüksek veri miktarını denetler. |
|eventPriority$ {EventName} |Gerekli: false |Geçerli değerler: Yüksek, düşük, kapalı |Varsayılan değer: |Her aracı önceliğini olay oluşturulur. | 
|

### <a name="events"></a>Olaylar

Aşağıdaki listede olan tüm olayları IOT Aracısı ASC cihazlarınızdan toplayabilirsiniz. AzureIotSecurity modül ikizi, bu olay toplanır ve önceliklerine çözümünüzdeki karar yapılandırmak için kullanın. 
 
|Olay adı| ÖzellikAdı | Varsayılan değer| Anlık görüntü olay| Ayrıntılı durumu  |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|Tanılama olayı|eventPriorityDiagnostic| Kapalı| False| Aracı tanılama olayları ilişkili. Bu olay için ayrıntılı günlük kaydını kullanın.| 
yapılandırma hatası |eventPriorityConfigurationError |Düşük |False |Aracı yapılandırması ayrıştırılamadı. Şemadaki yapılandırmasını doğrulayın.| 
|Bırakılan olay istatistikleri |eventPriorityDroppedEventsStatistics |Düşük |True|Olay istatistikleri Aracısı ilgili. |
|İleti istatistikleri|eventPriorityMessageStatistics |Düşük |True |Aracı ileti istatistikleri ilgili. |
|Bağlı donanım|eventPriorityConnectedHardware |Düşük |True |Tüm donanım anlık görüntüsünü cihaza bağlı.|
|Dinleme bağlantı noktaları|eventPriorityListeningPorts |Yüksek |True |Cihazdaki tüm açık dinleme bağlantı noktalarını anlık görüntüsünü.|
| İşlem Oluştur |eventPriorityProcessCreate |Düşük |False |Denetimleri oluşturma cihazda işleyin.|
| İşlemi Sonlandır|eventPriorityProcessTerminate |Düşük |False |Denetimleri cihazda sonlandırma işlemi.| 
 Sistem bilgileri |eventPrioritySystemInformation |Düşük |True |İşletim sistemi veya CPU gibi sistem bilgilerini anlık görüntüsünü.|
|Yerel Kullanıcılar| eventPriorityLocalUsers |Yüksek |True|Kayıtlı yerel kullanıcıların sistemi içinde bir anlık görüntü. |
|Oturum Aç|  eventPriorityLogin |Yüksek|False|Oturum açma olaylarını cihaza (yerel ve Uzak Oturumlar) denetler.|
|Bağlantı oluşturma |eventPriorityConnectionCreate|Düşük|False|Cihaz için ile oluşturulan TCP bağlantıları denetler. |
|Güvenlik duvarı yapılandırması| eventPriorityFirewallConfiguration|Düşük|True|Cihaz güvenlik duvarı yapılandırması (güvenlik duvarı kuralları), anlık görüntü. |
|İşletim sistemi temeli| eventPriorityOSBaseline| Düşük|True|Cihaz işletim sistemi temel anlık görüntüsünü denetleyin.| 
|


## <a name="next-steps"></a>Sonraki adımlar

- IOT için ASC okuma [genel bakış](overview.md)
- ASC hakkında bilgi edinmek için IOT [mimarisi](architecture.md)
- Anlama ve keşfetme [ASC IOT uyarılar için](concept-security-alerts.md)
- Nasıl erişmek keşfetmek, [güvenlik verileri](how-to-security-data-access.md)
