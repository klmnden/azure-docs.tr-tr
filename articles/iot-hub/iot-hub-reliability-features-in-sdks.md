---
title: Bağlantı ve Azure IOT Hub cihazı SDK'larını kullanarak güvenilir Mesajlaşma yönetme
description: Cihaz bağlantısı ve Azure IOT Hub cihazı SDK'ları kullanırken Mesajlaşma artırmayı öğrenin
services: iot-hub
keywords: ''
author: yzhong94
ms.author: yizhon
ms.date: 07/07/2018
ms.topic: article
ms.service: iot-hub
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 7717c026b4c09f47159fe62640f9a2eedf409d30
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247690"
---
# <a name="how-to-manage-connectivity-and-reliable-messaging-using-azure-iot-hub-device-sdks"></a>Bağlantı ve Azure IOT Hub cihazı SDK'larını kullanarak güvenilir Mesajlaşma yönetme

Bu kılavuz, Azure IOT cihaz SDK'ları bağlantısı ve güvenilir bir Mesajlaşma özelliklerinin avantajlarından yararlanarak cihaz dayanıklı uygulamalar tasarlamaya yönelik ileri düzey rehberlik sağlar. Bu makalenin amacı, soruları yanıtlamanıza yardımcı olur ve bu senaryolar ele olmaktır:

- Ağ Bağlantısı Yönetimi
- farklı ağ bağlantıları arasında geçiş yönetme
- Hizmet geçici bağlantı hataları nedeniyle yeniden yönetme

Uygulama Ayrıntıları dile göre farklılık, bağlantılı API belgelerine ya da belirli SDK daha fazla ayrıntı için bkz.

- [C/Python/iOS SDK'sı](https://github.com/azure/azure-iot-sdk-c)
- [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [Düğüm SDK'sı](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)


## <a name="designing-for-resiliency"></a>Dayanıklı olacak şekilde tasarlama

IOT cihazları genellikle GSM veya uydu gibi sürekli olmayan ve/veya kararsız ağ bağlantıları kullanır. Ayrıca, bulut tabanlı hizmetler ile etkileşim kurulurken aralıklı hizmet kullanılabilirliği ve altyapı düzeyinde hataları (genellikle geçici hatalar adlandırılır) gibi geçici koşul nedeniyle hatalar oluşabilir. Bir cihaz üzerinde çalışan bir uygulama bağlantı ve yeniden bağlanmayı mekanizmaları yanı sıra, gönderme/alma iletileri için yeniden deneme mantığı yönetmek gerekir. Ayrıca, yeniden deneme stratejisi gereksinimleri yoğun olarak cihazın bağlam ve yetenekleri ve cihaz katıldığı IOT senaryonuza bağlıdır.

Azure IOT Hub cihazı SDK'ları bağlanma ve bulut-cihaz ve CİHAZDAN buluta bağlama ve gönderme/alma ve Azure IOT Hub'ından ileti güçlü ve kapsamlı bir yol sağlayarak iletişim kurarken basitleştirmek için hedeflenir. Geliştiriciler, belirli bir senaryo için doğru yeniden deneme stratejisi mevcut uygulamasına de değiştirebilirsiniz.

Bağlantı ve güvenilir Mesajlaşma destekleyen ilgili SDK özelliklerinin aşağıdaki bölümlerde ele alınmıştır.

## <a name="connection-and-retry"></a>Bağlantı ve yeniden deneyin

Bu bölümde, bağlantılar, cihaz SDK'ları farklı yeniden deneme ilkesi cihaz uygulamasını ve ilgili API'leri kullanmaya yönelik Uygulama Kılavuzu yönetirken yeniden bağlanma ve yeniden deneme desenleri için genel bir bakış sağlar.

### <a name="error-patterns"></a>Hata desenleri
Birçok düzeylerini bağlantı hataları oluşabilir:

-  Ağ bağlantısı kesilmiş bir yuva gibi hataları ve ad çözümleme hataları
- HTTP, AMQP ve MQTT protokolünü düzeyindeki hataları gibi bağlantıları ayrılmış veya süresi dolmuş oturumlar taşıma
- Geçersiz kimlik bilgileri gibi ya da yerel hatalardan kaynaklanır ya da hizmet kotasını veya azaltma gibi davranışları uygulama düzeyi hataları

Cihaz SDK'ları, tüm üç düzeyden birinde hataları algılayın.  İşletim sistemi ile ilgili hataları ve donanım hataları algılanmaz ve cihaz SDK'ları tarafından işlenir.  Tasarım dayanır [işleme geçici hata Kılavuzu](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults#general-guidelines) Azure Mimari Merkezi.

### <a name="retry-patterns"></a>Desenler yeniden deneyin

Bağlantı hatalar algılandığında bir yeniden deneme için genel işlem şöyledir: 
1. SDK'sı, ağ, protokolü veya uygulama hata ve ilgili hatayı algılar.
2. Hata türüne göre filtre hatası durumunda karar vermek için SDK'sını kullanan gerçekleştirilmesi gerekir yeniden deneyin.  Varsa bir **kurtarılamaz hata** tanımlanır (bağlantı ve gönderme ve alma) işlemleri SDK tarafından durdurulur ve SDK'sı kullanıcıya bildirim gönderir. SDK'sı tanımlayan ve, örneğin kurtarılamaz olduğunu belirlemek bir hata, bir kimlik doğrulama veya hatalı bir uç nokta hatası kurtarılamaz bir hata var.
3. Varsa bir **kurtarılamaz bir hata** olan tanımlandığında, tanımlı bir zaman aşımı süresi dolana kadar belirtilen yeniden deneme İlkesi kullanarak yeniden denemek SDK'sı başlar.
4. Tanımlanan zaman aşımı süresi dolduğunda, SDK'sı durdurur bağlanmak veya Gönder ve kullanıcıyı bilgilendirir.
5.  SDK'sı kullanıcının bağlantı durumu değişiklikleri almak amacıyla bir geri çağırma eklemesine izin verir. 

Üç yeniden deneme ilkelerini sağlanır:
- **Üstel geri alma değişimi ile**: uygulanan varsayılan yeniden deneme ilkesi budur.  Başında agresif olma eğilimindedir, yavaşlar ve ardından aşılmadığı bir en büyük gecikme denk gelir.  Tasarım dayanır [yeniden deneme Kılavuzu Azure Mimari Merkezi](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific).
- **Özel bir yeniden deneme**: özel bir yeniden deneme ilkesi uygulamak ve RetryPolicy seçtiğiniz dile bağlı olarak, ekleme. Senaryonuz için uygun bir yeniden deneme ilkesi tasarlayabilirsiniz.  Bu C SDK'sı üzerinde kullanılabilir değil.
- **Yeniden deneme yok**: "yeniden deneme mantığının devre dışı bırakan yeniden deneme yok," için yeniden deneme ilkesini ayarlamak için bir seçenek yoktur.  Bağlantının kurulduğu varsayılarak kez bağlanmak ve bu kez, bir ileti göndermek SDK'sı çalışır. Bu ilke genellikle durumlarda kullanılacak bant genişliği veya maliyet endişeleriniz olduğu.   Bu seçenek belirtildiyse, gönderemiyor iletileri kaybolur ve geri alınamaz. 

### <a name="retry-policy-apis"></a>Yeniden deneme ilkesi API'leri

   | SDK | SetRetryPolicy yöntemi | İlke uygulamaları | Uygulama kılavuzu |
   |-----|----------------------|--|--|
   |  C/Python/iOS  | [IOTHUB_CLIENT_RESULT IoTHubClient_SetRetryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/2018-05-04/iothub_client/inc/iothub_client.h#L188)        | **Varsayılan**: [IOTHUB_CLIENT_RETRY_EXPONENTIAL_BACKOFF](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Özel:** kullanım kullanılabilir [retryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Yeniden deneme yok:** [IOTHUB_CLIENT_RETRY_NONE](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)  | [C/Python/iOS uygulaması](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#)  |
   | Java| [SetRetryPolicy](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.sdk.iot.device._device_client_config.setretrypolicy?view=azure-java-stable)        | **Varsayılan**: [ExponentialBackoffWithJitter sınıfı](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)<BR>**Özel:** uygulamak [RetryPolicy arabirimi](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/RetryPolicy.java)<BR>**Yeniden deneme yok:** [NoRetry sınıfı](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)  | [Java uygulaması](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md) |[.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
   | .NET| [DeviceClient.SetRetryPolicy](/dotnet/api/microsoft.azure.devices.client.deviceclient.setretrypolicy?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_SetRetryPolicy_Microsoft_Azure_Devices_Client_IRetryPolicy) | **Varsayılan**: [ExponentialBackoff sınıfı](/dotnet/api/microsoft.azure.devices.client.exponentialbackoff?view=azure-dotnet)<BR>**Özel:** uygulamak [IRetryPolicy arabirimi](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.iretrypolicy?view=azure-dotnet)<BR>**Yeniden deneme yok:** [NoRetry sınıfı](/dotnet/api/microsoft.azure.devices.client.noretry?view=azure-dotnet) | [C# uygulaması]() |
   | Node| [setRetryPolicy](/javascript/api/azure-iot-device/client?view=azure-iot-typescript-latest#azure_iot_device_Client_setRetryPolicy) | **Varsayılan**: [ExponentialBackoffWithJitter sınıfı](/javascript/api/azure-iot-common/exponentialbackoffwithjitter?view=azure-iot-typescript-latest)<BR>**Özel:** uygulamak [RetryPolicy arabirimi](/javascript/api/azure-iot-common/retrypolicy?view=azure-iot-typescript-latest)<BR>**Yeniden deneme yok:** [NoRetry sınıfı](/javascript/api/azure-iot-common/noretry?view=azure-iot-typescript-latest) | [Düğüm uygulama](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them) |
   

Bu akış gösteren kod örnekleri aşağıda verilmiştir. 

#### <a name="net-implementation-guidance"></a>.NET Uygulama Kılavuzu

Aşağıdaki kod örneği, tanımlama ve varsayılan yeniden deneme ilkesi ayarlama işlemi gösterilmektedir:

   ```csharp
   # define/set default retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(int.MaxValue, TimeSpan.FromMilliseconds(100), TimeSpan.FromSeconds(10), TimeSpan.FromMilliseconds(100));
   SetRetryPolicy(retryPolicy);
   ```

Kod (örneğin ağ ya da hedef yol olduğunda hemen,) başarısız olursa yüksek CPU kullanımından kaçınmak için yeniden deneme sayısı azaltılır ve böylece sonraki yeniden deneme yürütmek için minimum süre 1 saniyedir. 

Hizmet ile azaltma bir hata yanıt veriyorsa yeniden deneme ilkesi farklıdır ve ortak API aracılığıyla değiştirilemez:

   ```csharp
   # throttled retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(RetryCount, TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(60), TimeSpan.FromSeconds(5));
   SetRetryPolicy(retryPolicy);
   ```

Yeniden deneme mekanizmasını sonra durdurulur `DefaultOperationTimeoutInMilliseconds`, şu anda 4 dakika olarak ayarlanmış.

#### <a name="other-languages-implementation-guidance"></a>Diğer diller Uygulama Kılavuzu
Diğer diller için uygulama aşağıdaki belgeleri gözden geçirin.  Kullanımını gösteren örnek deposunda sağlanan API ilke yeniden deneyin.
- [C/Python/iOS SDK'sı](https://github.com/azure/azure-iot-sdk-c)
- [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [Düğüm SDK'sı](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)

## <a name="next-steps"></a>Sonraki adımlar
- [Cihaz ve hizmet SDK’ları kullanma](.\iot-hub-devguide-sdks.md)
- [C için IoT cihaz SDK'sını kullanma](.\iot-hub-device-sdk-c-intro.md)
- [Kısıtlanmış cihazlar için geliştirme](.\iot-hub-devguide-develop-for-constrained-devices.md)
- [Mobil cihazlar için geliştirme](.\iot-hub-how-to-develop-for-mobile-devices.md)