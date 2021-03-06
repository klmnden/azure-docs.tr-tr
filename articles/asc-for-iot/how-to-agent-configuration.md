---
title: Azure Güvenlik Merkezi Önizleme IOT aracısı için yapılandırma | Microsoft Docs
description: Aracıları kullanmak için IOT için Azure Güvenlik Merkezi ile yapılandırmayı öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: f95c445a-4f0d-4198-9c6c-d01446473bd0
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 39539bb14877208e5f6af957e735a136b077f16a
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618275"
---
# <a name="tutorial-configure-security-agents"></a>Öğretici: Güvenlik aracılarını yapılandırma

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, Azure Güvenlik Merkezi (ASC) IOT güvenlik aracısının açıklanmaktadır. değiştirmek için bunları ASC IOT güvenliği için denetleyicisilerinin nasıl.

> [!div class="checklist"]
> * Güvenlik aracılarını yapılandırma
> * İkiz özelliklerini düzenleyerek aracı davranışını değiştirme
> * Varsayılan yapılandırma keşfedin

## <a name="agents"></a>Aracılar

ASC IOT güvenlik aracılar için IOT cihazlarından veri toplamak ve algılanan güvenlik açıklarını gidermek için güvenlik eylemleri gerçekleştirebilirsiniz. Güvenlik aracı yapılandırması modül ikizi özelliklerini özelleştirebileceğiniz bir dizi kullanılarak denetlenebilir. Genel olarak, bu özellikler için ikincil güncelleştirmeleri daha azdır.  

ASC IOT'ın güvenlik aracı ikizi yapılandırma nesnesi için bir .json biçimi nesnesidir. Yapılandırma nesnesi, aracıyı davranışını denetlemek için tanımlayabileceğiniz denetlenebilir özellikler kümesidir. 

Bu yapılandırmalar, aracının gerekli her senaryo için özelleştirmenize yardımcı olur. Örneğin, otomatik olarak bazı olaylar hariç veya güç tüketimini en düşük düzeyde tutmak mümkün bu özelliklerini yapılandırarak.  

IOT güvenlik aracı yapılandırması için ASC kullanın [şema](https://aka.ms/iot-security-github-module-schema) değişiklik yapma.  

## <a name="configuration-objects"></a>Yapılandırma nesneleri 

IOT güvenliği aracısı için her ASC ilgili özelliği bulunan aracı yapılandırma nesnesi, istenen özellikler bölümü içinde **azureiotsecurity** modülü. 

Yapılandırmasını değiştirmek için oluşturma ve bu nesne içine değiştirme **azureiotsecurity** modül ikizi kimlik. 

Aracı yapılandırma nesnesi, mevcut değilse **azureiotsecurity** modül ikizi, tüm güvenlik aracı özellik değerlerini varsayılan olarak ayarlanır. 

```json
"desired": {
  "azureiot*com^securityAgentConfiguration^1*0*0": {
  } 
}
```

Aracı yapılandırma değişikliklerinizi bu karşı doğrulayın [şema](https://aka.ms/iot-security-github-module-schema).
Aracı, yapılandırma nesnesini şema eşleşmiyorsa başlatmaz.

## <a name="configuration-schema-and-validation"></a>Yapılandırma şeması ve doğrulama 

Buna karşı aracı yapılandırması emin [şema](https://aka.ms/iot-security-github-module-schema). Bir aracı, yapılandırma nesnesini şema eşleşmiyorsa başlatmaz.

 
Aracı çalışırken, yapılandırma nesnesini (yapılandırma şeması eşleşmiyor) geçerli olmayan bir yapılandırma değiştirildiyse, aracı geçersiz yapılandırma yoksayar ve geçerli yapılandırmasını kullanarak devam eder. 

## <a name="editing-a-property"></a>Özellik düzenleme 

Tüm özel özellikleri içinde aracı yapılandırma nesnesi içinde ayarlanmalıdır **azureiotsecurity** modül ikizi.
Varsayılan özellik değeri kullanmak için yapılandırma nesnesinden özelliği kaldırın.

### <a name="setting-a-property"></a>Bir özellik ayarlama

1. IOT hub'ınızdaki bulun ve değiştirmek istediğiniz cihazı seçin.

1. Cihazınızda ve üzerinde tıklatın **azureiotsecurity** modülü.

1. Tıklayarak **modül kimliği İkizi**.

1. Güvenlik modülüne istenen özelliklerini düzenleyin.
   
   Örneğin, yüksek öncelikli olarak bağlantı olayları yapılandırın ve 7 dakikada yüksek öncelikli olayları toplamak için şu yapılandırmayı kullanın.
   
   ```json
    "desired": {
      "azureiot*com^securityAgentConfiguration^1*0*0": {
        "highPriorityMessageFrequency": "PT7M",    
        "eventPriorityConnectionCreate": "High" 
      } 
    }, 
    ```

1. **Kaydet**’e tıklayın.

### <a name="using-a-default-value"></a>Varsayılan değer kullanma

Varsayılan özellik değeri kullanmak için yapılandırma nesnesinden özelliği kaldırın.

## <a name="default-properties"></a>Varsayılan Özellikler 

Aşağıdaki tabloda, IOT güvenlik aracıları ASC denetlenebilir özelliklerini içerir.

Varsayılan değerler doğru şemasında kullanılabilir [Github](https://aka.ms/iot-security-module-default).

| Ad| Durum | Geçerli değerler| Varsayılan değerler| Açıklama |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|highPriorityMessageFrequency|Gerekli: false |Geçerli değerler: ISO 8601 biçimli süresi |Varsayılan değer: PT7M |En fazla süre önce yüksek öncelikli iletiler gönderilir.|
|lowPriorityMessageFrequency |Gerekli: false|Geçerli değerler: ISO 8601 biçimli süresi |Varsayılan değer: PT5H |En fazla süre önce düşük öncelikli iletiler gönderilir.| 
|snapshotFrequency |Gerekli: false|ISO 8601 biçimli, geçerli değerler: süresi |Varsayılan değer PT13H |Cihaz durumu anlık görüntülerin oluşturulması için zaman aralığı.| 
|maxLocalCacheSizeInBytes |Gerekli: false |Geçerli değerler: |Varsayılan değer: 2560000 8192 daha büyük | Bir aracı ileti önbelleği için izin verilen en fazla depolama (bayt cinsinden). En fazla ileti gönderilmeden önce cihazda iletileri depolamak için izin verilen alanı miktarı.| 
|maxMessageSizeInBytes |Gerekli: false |Geçerli değerler: 8192, 262144 değerinden daha büyük pozitif bir sayı |Varsayılan değer: 204800 |Aracıya bulut ileti boyutu izin verilen en yüksek. Bu ayar, her iletiye gönderilen en yüksek veri miktarını denetler. |
|eventPriority$ {EventName} |Gerekli: false |Geçerli değerler: Yüksek, düşük, kapalı |Varsayılan değer: |Her aracı önceliğini oluşturulan olay | 

### <a name="supported-security-events"></a>Desteklenen güvenlik olayları

|Olay adı| ÖzellikAdı | Default Value| Anlık görüntü olay| Durum ayrıntıları  |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|Tanılama olayı|eventPriorityDiagnostic| Kapalı| False| Aracı tanılama olayları ilişkili. Bu olay için ayrıntılı günlük kaydını kullanın.| 
|yapılandırma hatası |eventPriorityConfigurationError |Düşük |False |Aracı yapılandırması ayrıştırılamadı. Şemadaki yapılandırmasını doğrulayın.| 
|Bırakılan olay istatistikleri |eventPriorityDroppedEventsStatistics |Düşük |Doğru|Olay istatistikleri Aracısı ilgili. |
|İleti istatistikleri|eventPriorityMessageStatistics |Düşük |Doğru |Aracı ileti istatistikleri ilgili. |
|Bağlı donanım|eventPriorityConnectedHardware |Düşük |Doğru |Tüm donanım anlık görüntüsünü cihaza bağlı.|
|Dinleme bağlantı noktaları|eventPriorityListeningPorts |Yüksek |Doğru |Cihazdaki tüm açık dinleme bağlantı noktalarını anlık görüntüsünü.|
|İşlem Oluştur |eventPriorityProcessCreate |Düşük |False |Denetimleri oluşturma cihazda işleyin.|
|İşlemi Sonlandır|eventPriorityProcessTerminate |Düşük |False |Denetimleri cihazda sonlandırma işlemi.| 
|Sistem bilgileri |eventPrioritySystemInformation |Düşük |Doğru |Sistem bilgileri anlık görüntüsünü (örneğin: İşletim sistemi veya CPU).| 
|Yerel Kullanıcılar| eventPriorityLocalUsers |Yüksek |Doğru|Kayıtlı yerel kullanıcıların sistemi içinde bir anlık görüntü. |
|Oturum Aç|  eventPriorityLogin |Yüksek|False|Cihaz (yerel ve uzak oturumları) için oturum açma olaylarını denetleyin.|
|Bağlantı oluşturma |eventPriorityConnectionCreate|Düşük|False|Cihaz için ile oluşturulan TCP bağlantıları denetler. |
|Güvenlik duvarı yapılandırması| eventPriorityFirewallConfiguration|Düşük|Doğru|Cihaz güvenlik duvarı yapılandırması (güvenlik duvarı kuralları), anlık görüntü. |
|İşletim sistemi temeli| eventPriorityOSBaseline| Düşük|Doğru|Cihaz işletim sistemi temel anlık görüntüsünü denetleyin.|
 

## <a name="next-steps"></a>Sonraki adımlar

- [ASC için IOT önerilerini anlama](concept-recommendations.md)
- [ASC IOT uyarılar için keşfetmek](concept-security-alerts.md)
- [Erişim ham güvenlik verileri](how-to-security-data-access.md)
