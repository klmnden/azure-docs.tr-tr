---
title: Bir ASC IOT aracı Önizleme için yapılandırma | Microsoft Docs
description: Kullanılacak Aracılar ASC ile IOT için yapılandırmayı öğrenin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: f95c445a-4f0d-4198-9c6c-d01446473bd0
ms.service: ascforiot
ms.devlang: na
ms.topic: concept
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 3a3806c73ef254abab91bd28cd8761917371235f
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541925"
---
# <a name="configure-security-agents"></a>Güvenlik aracılarını yapılandırma

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede bir aracı kullanmak için IOT ASC ile yapılandırmak bir açıklama sağlar.

## <a name="agents"></a>Aracılar

ASC IOT güvenlik aracılar için IOT cihazlarından veri toplamak ve algılanan güvenlik açıklarını gidermek için güvenlik eylemleri gerçekleştirebilirsiniz. Güvenlik aracı yapılandırması modül ikizi özelliklerini özelleştirebileceğiniz bir dizi kullanılarak denetlenebilir. Genel olarak, bu özellikler için ikincil güncelleştirmeleri daha azdır.  

ASC IOT'ın güvenlik aracı ikizi yapılandırma nesnesi için bir .json biçimi nesnesidir. Yapılandırma nesnesi, aracıyı davranışını denetlemek için tanımlayabileceğiniz denetlenebilir özellikler kümesidir. 

Bu yapılandırmalar, aracının gerekli her senaryo için özelleştirmenize yardımcı olur. Örneğin, otomatik olarak bazı olaylar hariç veya güç tüketimini en düşük düzeyde tutmak mümkün bu özelliklerini yapılandırarak.  

IOT güvenlik aracı yapılandırması için ASC kullanın [şema](https://github.com/azure/asc-for-iot-schemas/security/module/twin) değişiklik yapma.  

## <a name="configuration-objects"></a>Yapılandırma nesneleri 

Özelliği, istenen özellikler bölümünü azureiotsecurity modülü içinde aracı yapılandırma nesnesi içinde bulunan her ASC IOT güvenlik aracı için ilgili. 

Yapılandırmasını değiştirmek için oluşturma ve bu nesnenin içinde azureiotsecurity modül ikizi kimliğini değiştirin. Aracı yapılandırma nesnesi içinde azureiotsecurity modül ikizi mevcut değilse, varsayılan olarak tüm güvenlik aracı özellik değerlerini ayarlayın. 

```json
"desired": { //azureiotsecurity Module Identity Twin – desired properties section  
  "azureiot*com^securityAgentConfiguration^1*0*0": { //Agent configuration object 
  … 
} 
}
```

Aracı yapılandırma değişikliklerinizi bu karşı doğrulayın [şema](https://aka.ms/iot-security-github-module-schema).
Aracı, yapılandırma nesnesini şema eşleşmiyorsa başlatmaz.


## <a name="editing-a-property"></a>Özellik düzenleme 

Tüm özel özellikler, aracı yapılandırma nesnesi içinde azureiotsecurity modül ikizi içinde ayarlanmalıdır. 

Bir özelliği ayarlamak, varsayılan değeri geçersiz kılar. Özellik ayarlamak için yapılandırma nesnesi istenen değeri içeren özellik anahtarı ekleyin. 

Varsayılan özellik değeri kullanmak için yapılandırma nesnesinden özelliği kaldırın. 

```json
"desired": { //azureiotsecurity Module Identity Twin – desired properties section  
  "azureiot*com^securityAgentConfiguration^1*0*0": { //ASC for IoT Agent 
      // configuration section  
    "lowPriorityMessageFrequency": "PT1H",     
    "highPriorityMessageFrequency": "PT7M",    
    "eventPriorityFirewallConfiguration": "High",     
    "eventPriorityConnectionCreate": "High" 
  } 
}, 
```

## <a name="default-properties"></a>Varsayılan Özellikler 
IOT güvenlik aracıları ASC denetleyen denetlenebilir özellikler kümesidir.

Varsayılan değerler doğru şemasında kullanılabilir [Github](https://aka.ms/iot-security-module-default).

| Ad| Durum | Geçerli değerler| Varsayılan değerler| Açıklama |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|highPriorityMessageFrequency|Gerekli: false |Geçerli değerler: ISO 8601 biçimli süresi |Varsayılan değer: PT7M |En fazla süre önce yüksek öncelikli iletiler gönderilir.|
|lowPriorityMessageFrequency |Gerekli: false|Geçerli değerler: ISO 8601 biçimli süresi |Varsayılan değer: PT5H |En fazla süre önce düşük öncelikli iletiler gönderilir.| 
|snapshotFrequency |Gerekli: false|ISO 8601 biçimli, geçerli değerler: süresi |Varsayılan değer PT13H |Cihaz durumu anlık görüntülerin oluşturulması için zaman aralığı.| 
|maxLocalCacheSizeInBytes |Gerekli: false |Geçerli değerler: |Varsayılan değer: 2560000 8192 daha büyük | Bir aracı ileti önbelleği için izin verilen en fazla depolama (bayt cinsinden). En fazla ileti gönderilmeden önce cihazda iletileri depolamak için izin verilen alanı miktarı.| 
|maxMessageSizeInBytes |Gerekli: false |Geçerli değerler: 8192, 262144 değerinden daha büyük pozitif bir sayı |Varsayılan değer: 204800 |Aracıya bulut ileti boyutu izin verilen en yüksek. Bu ayar, her iletiye gönderilen en yüksek veri miktarını denetler. |
|eventPriority$ {EventName} |Gerekli: false |Geçerli değerler: Yüksek, düşük, kapalı |Varsayılan değer: |Her aracı önceliğini oluşturulan olay | 

### <a name="events"></a>Olaylar

|Olay adı| ÖzellikAdı | Varsayılan Değer| Anlık görüntü olay| Durum ayrıntıları  |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|Tanılama olayı|eventPriorityDiagnostic| Kapalı| False| Aracı tanılama olayları ilişkili. Bu olay için ayrıntılı günlük kaydını kullanın.| 
|yapılandırma hatası |eventPriorityConfigurationError |Düşük |False |Aracı yapılandırması ayrıştırılamadı. Şemadaki yapılandırmasını doğrulayın.| 
|Bırakılan olay istatistikleri |eventPriorityDroppedEventsStatistics |Düşük |True|Olay istatistikleri Aracısı ilgili. |
|İleti istatistikleri|eventPriorityMessageStatistics |Düşük |True |Aracı ileti istatistikleri ilgili. |
|Bağlı donanım|eventPriorityConnectedHardware |Düşük |True |Tüm donanım anlık görüntüsünü cihaza bağlı.|
|Dinleme bağlantı noktaları|eventPriorityListeningPorts |Yüksek |True |Cihazdaki tüm açık dinleme bağlantı noktalarını anlık görüntüsünü.|
|İşlem Oluştur |eventPriorityProcessCreate |Düşük |False |Denetimleri oluşturma cihazda işleyin.|
|İşlemi Sonlandır|eventPriorityProcessTerminate |Düşük |False |Denetimleri cihazda sonlandırma işlemi.| 
|Sistem bilgileri |eventPrioritySystemInformation |Düşük |True |Sistem bilgileri anlık görüntüsünü (örneğin: İşletim sistemi veya CPU).| 
|Yerel Kullanıcılar| eventPriorityLocalUsers |Yüksek |True|Kayıtlı yerel kullanıcıların sistemi içinde bir anlık görüntü. |
|Oturum Aç|  eventPriorityLogin |Yüksek|False|Cihaz (yerel ve uzak oturumları) için oturum açma olaylarını denetleyin.|
|Bağlantı oluşturma |eventPriorityConnectionCreate|Düşük|False|Cihaz için ile oluşturulan TCP bağlantıları denetler. |
|Güvenlik duvarı yapılandırması| eventPriorityFirewallConfiguration|Düşük|True|Cihaz güvenlik duvarı yapılandırması (güvenlik duvarı kuralları), anlık görüntü. |
|İşletim sistemi temeli| eventPriorityOSBaseline| Düşük|True|Cihaz işletim sistemi temel anlık görüntüsünü denetleyin.|
 

## <a name="see-also"></a>Ayrıca Bkz.

- [ASC için IOT önerilerini anlama](concept-recommendations.md)
- [ASC IOT uyarılar için keşfetmek](concept-security-alerts.md)
- [Erişim ham güvenlik verileri](how-to-security-data-access.md)