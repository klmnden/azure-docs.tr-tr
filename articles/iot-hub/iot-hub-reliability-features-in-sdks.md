---
title: Bağlantı ve Azure IOT Hub cihaz SDK'ları kullanarak güvenilir Mesajlaşma yönetme
description: Cihaz bağlantısı ve Azure IOT Hub cihazı SDK'ları kullanırken Mesajlaşma artırmayı öğrenin
services: iot-hub
author: yzhong94
ms.author: yizhon
ms.date: 07/07/2018
ms.topic: article
ms.service: iot-hub
ms.openlocfilehash: 838d0cd4f40666bc3fced22a607b9f94f27b08d3
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67535500"
---
# <a name="manage-connectivity-and-reliable-messaging-by-using-azure-iot-hub-device-sdks"></a>Bağlantı ve Azure IOT Hub cihaz SDK'ları kullanarak güvenilir Mesajlaşma yönetme

Bu makalede yardımcı olmak için üst düzey rehberlik sağlanır karşı daha dayanıklı olan cihaz uygulamaları tasarlayın. Azure IOT cihaz SDK'ları bağlantısı ve güvenilir bir Mesajlaşma özelliklerini yararlanmak nasıl gösterir. Bu kılavuzun aşağıdaki senaryolarda yönetmenize yardımcı olmaktır:

* Ağ bağlantısı düzeltme

* Farklı ağ bağlantıları arasında geçiş yapma

* Hizmet geçici bağlantı hataları nedeniyle yeniden bağlanıyor

Uygulama Ayrıntıları dile göre değişiklik gösterebilir. Daha fazla bilgi için belirli SDK'sı ve API belgelerine bakın:

* [C/Python/iOS SDK'sı](https://github.com/azure/azure-iot-sdk-c)

* [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)

* [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)

* [Düğüm SDK'sı](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)

## <a name="designing-for-resiliency"></a>Dayanıklı olacak şekilde tasarlama

IOT cihazları (örneğin, GSM veya uydu) sürekli olmayan veya kararsız ağ bağlantılarında genellikle kullanır. Cihazları aralıklı hizmet kullanılabilirliği ve altyapı düzeyinde veya geçici hatalar nedeniyle bulut tabanlı hizmetler ile etkileşim kurduğunuzda hatalar oluşabilir. Bağlantı, yeniden bağlantı ve ileti gönderme ve alma için yeniden deneme mantığı için mekanizmaları yönetmek bir cihaz üzerinde çalışan bir uygulama vardır. Ayrıca, yeniden deneme stratejisi gereksinimleri yoğun bir şekilde bağlıdır cihazın IOT senaryosu, bağlam, özellikleri.

Azure IOT Hub cihazı SDK'ları bağlanma ve bulut-cihaz ve CİHAZDAN buluta iletişim kurarken basitleştirmek için hedeflenir. Bu SDK'ları, Azure IOT Hub ve kapsamlı ileti gönderme ve alma için seçenekleri bağlanmak için güçlü bir yol sağlar. Geliştiriciler, belirli bir senaryo için daha iyi bir yeniden deneme stratejisi özelleştirmek için varolan uygulama de değiştirebilirsiniz.

Bağlantı ve güvenilir Mesajlaşma destekleyen ilgili SDK özelliklerinin aşağıdaki bölümlerde ele alınmıştır.

## <a name="connection-and-retry"></a>Bağlantı ve yeniden deneyin

Bu bölüm, bağlantıları yönetirken yeniden bağlantı ve yeniden deneme desenleri için genel bir bakış sağlar. Bu cihaz uygulamanızın farklı yeniden deneme İlkesi'ni kullanarak Uygulama Kılavuzu ayrıntıları ve ilgili API'lerinden cihaz SDK'ları listeler.

### <a name="error-patterns"></a>Hata desenleri

Birçok düzeyde bağlantı hataları oluşabilir:

* Ağ hataları: bağlantısı kesildi ve yuva adı çözümleme hataları

* Protokol düzeyinde HTTP, AMQP ve MQTT taşıma için hataları: ayrılmış bağlantılar veya oturum süresi doldu

* Ya da yerel hatalarından kaynaklanan hatalar uygulama düzeyi: Geçersiz kimlik bilgileri veya hizmet davranışı (örneğin, kotasını veya azaltma)

Cihaz SDK'ları, tüm üç düzeyde hataları algılayın. İşletim sistemi ile ilgili hataları ve donanım hataları algılanmaz ve cihaz SDK'ları tarafından işlenir. SDK'sı tasarım dayanır [işleme geçici hata Kılavuzu](/azure/architecture/best-practices/transient-faults#general-guidelines) Azure Mimari Merkezi.

### <a name="retry-patterns"></a>Desenler yeniden deneyin

Bağlantı hatalar algılandığında, aşağıdaki adımları yeniden deneme işlemi açıklanmaktadır:

1. SDK'sı, ağ, protokolü veya uygulama hata ve ilgili hatayı algılar.

2. SDK'sı hata filtre hata türü belirlemek ve bir yeniden deneme gerekip gerekmediğini karar vermek için kullanır.

3. SDK'sı tanımlıyorsa bir **kurtarılamaz hata**, bağlantısı gibi işlemleri göndermek ve almak durdurulur. SDK'sı, kullanıcıya bildirir. Kurtarılamaz hatalar bir kimlik doğrulama hatası ve hatalı uç nokta hata verilebilir.

4. SDK'sı tanımlıyorsa bir **kurtarılamaz bir hata**, tanımlı zaman aşımı sona erdiğinde kadar belirtilen yeniden deneme ilkesine göre yeniden dener.  SDK'sını kullanan Not **Üstsel geri alma ile değişimi** varsayılan yeniden deneme ilkesi.
5. Tanımlanan zaman aşımı süresi dolduğunda, SDK'sı, bağlanmak veya göndermeye çalışırken durdurur. Bunu kullanıcıya bildirir.

6. SDK'sı kullanıcının bağlantı durumu değişiklikleri almak amacıyla bir geri çağırma eklemesine izin verir.

SDK'ları üç yeniden deneme ilkelerine sağlar:

* **Üstel geri alma değişimi ile**: Bu varsayılan yeniden deneme ilkesi başlangıcında agresif ve en büyük gecikme ulaşana dek zaman içinde yavaşlamasına eğilimindedir. Tasarım dayanır [yeniden deneme Kılavuzu Azure Mimari Merkezi](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific). 

* **Özel bir yeniden deneme**: SDK bazı diller için daha iyi senaryonuz için uygun olan ve ardından RetryPolicy ekleme bir özel bir yeniden deneme ilkesi tasarlayabilirsiniz. Özel bir yeniden deneme üzerinde C SDK'sı yoktur.

* **Yeniden deneme yok**: "Yeniden deneme mantığının devre dışı bırakan yeniden deneme yok," için yeniden deneme ilkesi ayarlayabilirsiniz. Bağlantının kurulduğu varsayılarak kez bağlanmak ve bu kez, bir ileti göndermek SDK'sı çalışır. Bu ilke genellikle bant genişliği veya maliyet konuları ile senaryolarda kullanılır. Bu seçeneği belirlerseniz, gönderemiyor iletileri kaybolur ve geri alınamaz.

### <a name="retry-policy-apis"></a>Yeniden deneme ilkesi API'leri

   | SDK | SetRetryPolicy yöntemi | İlke uygulamaları | Uygulama kılavuzu |
   |-----|----------------------|--|--|
   |  C/Python/iOS  | [IOTHUB_CLIENT_RESULT IoTHubClient_SetRetryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/2018-05-04/iothub_client/inc/iothub_client.h#L188)        | **Varsayılan**: [IOTHUB_CLIENT_RETRY_EXPONENTIAL_BACKOFF](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Özel:** kullanım kullanılabilir [retryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Yeniden deneme:** [IOTHUB_CLIENT_RETRY_NONE](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)  | [C/Python/iOS uygulaması](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#)  |
   | Java| [SetRetryPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.deviceclientconfig.setretrypolicy?view=azure-java-stable)        | **Varsayılan**: [ExponentialBackoffWithJitter sınıfı](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)<BR>**Özel:** uygulamak [RetryPolicy arabirimi](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/RetryPolicy.java)<BR>**Yeniden deneme:** [NoRetry sınıfı](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)  | [Java uygulaması](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md) |
   | .NET| [DeviceClient.SetRetryPolicy](/dotnet/api/microsoft.azure.devices.client.deviceclient.setretrypolicy?view=azure-dotnet) | **Varsayılan**: [ExponentialBackoff sınıfı](/dotnet/api/microsoft.azure.devices.client.exponentialbackoff?view=azure-dotnet)<BR>**Özel:** uygulamak [IRetryPolicy arabirimi](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.iretrypolicy?view=azure-dotnet)<BR>**Yeniden deneme:** [NoRetry sınıfı](/dotnet/api/microsoft.azure.devices.client.noretry?view=azure-dotnet) | [C# uygulaması](https://github.com/Azure/azure-iot-sdk-csharp) | |
   | Düğüm| [SetRetryPolicy](/javascript/api/azure-iot-device/client?view=azure-iot-typescript-latest) | **Varsayılan**: [ExponentialBackoffWithJitter sınıfı](/javascript/api/azure-iot-common/exponentialbackoffwithjitter?view=azure-iot-typescript-latest)<BR>**Özel:** uygulamak [RetryPolicy arabirimi](/javascript/api/azure-iot-common/retrypolicy?view=azure-iot-typescript-latest)<BR>**Yeniden deneme:** [NoRetry sınıfı](/javascript/api/azure-iot-common/noretry?view=azure-iot-typescript-latest) | [Düğüm uygulama](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them) |

Aşağıdaki kod örnekleri, bu akış gösterilmektedir:

#### <a name="net-implementation-guidance"></a>.NET Uygulama Kılavuzu

Aşağıdaki kod örneği, tanımlama ve varsayılan yeniden deneme ilkesi ayarlama işlemi gösterilmektedir:

   ```csharp
   // define/set default retry policy
   IRetryPolicy retryPolicy = new ExponentialBackoff(int.MaxValue, TimeSpan.FromMilliseconds(100), TimeSpan.FromSeconds(10), TimeSpan.FromMilliseconds(100));
   SetRetryPolicy(retryPolicy);
   ```

Yüksek CPU kullanımından kaçınmak için kod hemen başarısız olursa yeniden deneme sayısı azaltılır. Örneğin, hiçbir ağ veya hedef yolu. Sonraki yeniden deneme yürütmek için minimum süre 1 saniyedir.

Hizmet ile azaltma bir hata verirse yeniden deneme ilkesi farklıdır ve ortak API aracılığıyla değiştirilemez:

   ```csharp
   // throttled retry policy
   IRetryPolicy retryPolicy = new ExponentialBackoff(RetryCount, TimeSpan.FromSeconds(10), 
     TimeSpan.FromSeconds(60), TimeSpan.FromSeconds(5)); SetRetryPolicy(retryPolicy);
   ```

Yeniden deneme mekanizmasını sonra durdurur `DefaultOperationTimeoutInMilliseconds`, şu anda 4 dakika olarak ayarlanmış.

#### <a name="other-languages-implementation-guidance"></a>Diğer diller Uygulama Kılavuzu

Diğer dillerde kod örnekleri için aşağıdaki uygulama belgeleri gözden geçirin. Depo yeniden deneme ilkesi API'leri kullanımını gösteren örnekler içerir.

* [C/Python/iOS SDK'sı](https://github.com/azure/azure-iot-sdk-c)

* [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/retrypolicy.md)

* [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)

* [Düğüm SDK'sı](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)

## <a name="next-steps"></a>Sonraki adımlar

* [Cihaz ve hizmet SDK’ları kullanma](./iot-hub-devguide-sdks.md)

* [C için IoT cihaz SDK'sını kullanma](./iot-hub-device-sdk-c-intro.md)

* [Kısıtlanmış cihazlar için geliştirme](./iot-hub-devguide-develop-for-constrained-devices.md)

* [Mobil cihazlar için geliştirme](./iot-hub-how-to-develop-for-mobile-devices.md)

* [Sorun giderme cihaz bağlantısını keser](iot-hub-troubleshoot-connectivity.md)
