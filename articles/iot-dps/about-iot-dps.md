---
title: "Azure IOT Hub cihaz hizmet sağlama genel bakış | Microsoft Docs"
description: "Azure IOT Hub ve cihaz sağlama hizmeti ile cihaz sağlamayı açıklar"
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 12/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 021ff1299321ae1aece3a77fc61129517c85697b
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="provisioning-devices-with-azure-iot-hub-device-provisioning-service-preview"></a>Azure IOT Hub cihaz sağlama hizmeti (Önizleme) ile hazırlama cihazları
Microsoft Azure tümleşik genel bulut Hizmetleri tüm IOT çözüm ihtiyaçlarınız için zengin bir dizi sağlar. IOT Hub cihaz hizmeti sağlama IOT zero touch, yalnızca zaman sağ IOT hub'ına insan etkileşimi gerektirmeden sağlama etkinleştiren müşteriler sağlamak için milyonlarca aygıtların güvenli ve ölçeklenebilir etkinleştirme Hub için bir yardımcı hizmetidir şekilde.

## <a name="when-to-use-device-provisioning-service"></a>Cihaz sağlama hizmeti kullanmak ne zaman
Aygıt hizmeti sağlama bağlı ve IOT Hub'ına aşağıdaki gibi yapılandırılmış alma aygıtları için mükemmel bir seçim olduğu birçok sağlama senaryo vardır:

* Zero touch cmdlet'e kod IOT Hub bağlantı bilgilerini (ilk kurulum) fabrikada olmadan tek bir IOT çözüm sağlama
* Cihazlar arasında çok sayıda hub Dengeleme yükleme
* Aygıtların satış işlem verilere (çoklu müşteri Mimarisi) dayalı kendi sahibinin IOT çözüm bağlanma
* Kullanım örneği (Çözüm yalıtım) bağlı olarak belirli bir IOT çözüm aygıtları bağlanma
* Bir cihaz IOT hub'ına en düşük gecikme (coğrafi-parçalama) ile bağlanma
* Yeniden sağlama aygıt değişikliği dayalı.
* (X.509 sertifikalarını bağlanmak için kullanmadığınızda) IOT Hub'ına bağlanmak için aygıt tarafından kullanılan anahtarların alınıyor

## <a name="behind-the-scenes"></a>Arka planda
Önceki bölümde listelenen tüm senaryoları yapılabilir zero touch ile aynı akışını sağlama için sağlama hizmetini kullanarak. El ile adımlar sağlama geleneksel birçoğu cihaz sağlama IOT cihazları dağıtmak ve el ile hata riskini azaltmak için gereken süreyi azaltmak üzere hizmetiyle otomatik değildir. Sağlanan bir aygıtı almak için arka planda neler olup bittiğini bir açıklaması verilmiştir. İlk adım el ile aşağıdaki adımların tümünü otomatik değildir.

![Temel sağlama akışı](./media/about-iot-dps/dps-provisioning-flow.png)

1. Aygıt üreticisi cihaz kayıt bilgileri Azure portalında kayıt listesine ekler.
2. Cihaz fabrikada ayarlamak sağlama Hizmeti uç noktası ile iletişim kurar. Cihaz kimliğini kanıtlamak için tanımlama bilgilerini sağlama hizmeti geçirir.
3. Kayıt kimliği ve anahtarı nonce bir sınama kullanarak kayıt liste girdisi karşı doğrulayarak sağlama hizmeti cihazın kimliğini doğrular ([Güvenilir Platform Modülü'nü](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)) veya standart X.509 doğrulama () X.509).
4. Sağlama hizmeti bir IOT hub cihaz kaydeder ve cihazın doldurur [twin durumu istenen](../iot-hub/iot-hub-devguide-device-twins.md).
5. IOT hub cihaz kimlik bilgileri sağlama hizmetine döndürür.
6. Sağlama hizmeti cihaza IOT hub bağlantı bilgilerini döndürür. Cihaz IOT hub'ına doğrudan veri göndermeye şimdi başlayabilirsiniz.
7. Cihaz IOT hub'ına bağlanır.
8. Cihaz IOT hub, cihaz çiftine istenen durumu alır.

## <a name="provisioning-process"></a>Sağlama işlemi
Aygıt hizmeti sağlama bağımsız olarak yapılabilir bir bölümü aldığı bir aygıtı dağıtım sürecinde iki ayrı adım vardır:

* **Üretim adım** hangi cihaz oluşturulur ve fabrikada hazırlanan içinde ve
* **Bulut Kurulum adım** aygıt hizmeti sağlama sağlama otomatik için yapılandırıldığı içinde.

Her iki adımları var olan üretim ve dağıtım işlemleri ile sorunsuz bir şekilde uygun. Aygıt hizmeti sağlama bile çok fazla cihaza bağlantı bilgilerini almak için el ile iş ile ilgili bazı dağıtım işlemlerini basitleştirir.

### <a name="manufacturing-step"></a>Üretim adım
Bu adım, tüm üretim satırda ne olduğu hakkında bağlıdır. Bu adımda dahil edilen roller Silikon Tasarımcısı, Silikon üreticisi, Tümleştirici ve/veya aygıtın son üreticisine bulunur. Bu adım, donanım oluşturma ile ilgilidir.

Aygıt hizmeti sağlama üretim işlemindeki yeni adım sunmaz; Bunun yerine, ilk yazılım ve (ideal) HSM cihaza yükler varolan adım içine bağlar. Açık mı sırasında kendi bağlantı bilgisi/IOT çözüm atamayı almak için sağlama hizmeti çağrıları için bu adımda bir cihaz kimliği oluşturma yerine cihaz yalnızca sağlama hizmeti bilgilerle programlanmış.

Ayrıca bu adımda, cihaz dağıtıcı/tanımlayıcı önemli bilgiler ile işleci üreticisi sağlar. Bu, tüm cihazlar bir TPM onay anahtarını ortak kısmını her TPM aygıttan ayıklanması için cihaz dağıtıcı/işleci tarafından sağlanan bir imzalama sertifikası üretilen bir X.509 sertifikası olduğunu onayladıktan olarak basit olabilir. Bu hizmetler günümüzde birçok Silikon üreticileri tarafından sunulur.

### <a name="cloud-setup-step"></a>Bulut Kurulum adım
Bu adım, uygun otomatik sağlama için bulut yapılandırılıyor hakkında bağlıdır. Genellikle iki tür kullanıcı bulut Kurulum adımında dahil olan: aygıtları nasıl (aygıt işleci) başlangıçta ayarlanması gereken bilen birisi ve başka birinin nasıl IOT hub'ları arasında (Çözüm işleci) Bölünecek aygıtlardır bilen.

Sağlama tek seferlik bir ilk kurulum gerçekleşmelidir yoktur ve bu görev genelde çözüm operatör tarafından işlenir. Sağlama hizmet yapılandırıldıktan sonra kullanım örneği değiştirilmediği sürece değiştirilecek yok.

Otomatik sağlama için hizmet yapılandırıldıktan sonra cihazları kaydetmeye hazırlanmalıdır. Bu adım kimin aygıtları, istenen yapılandırma bilir ve kendi IOT hub için arama geldiğinde sağlama hizmeti düzgün şekilde aygıtın kimliğine onaylar emin olmanızı sorumlu aygıt işleci tarafından gerçekleştirilir. Cihaz işleci üreticisinden tanımlayıcı anahtar bilgileri alır ve kayıt listesine ekler. Yeni girişler eklendiğinde veya varolan girişleri aygıtlar hakkında en son bilgilerle güncelleştirildi kayıt listesine sonraki güncelleştirmeler olabilir.

## <a name="registration-and-provisioning"></a>Kayıt ve hazırlama
*Sağlama* terimi kullanılır endüstri bağlı olarak çeşitli şeyler anlamına gelir. IOT cihazları kendi bulut çözümü için sağlama bağlamında, sağlama iki bölümden oluşan bir işlemdir:

1. İlk bölümü, cihaz kaydederek cihaz IOT çözüm arasındaki ilk bağlantı saptamaktır.
2. İkinci bölümü cihazın kayıtlı çözümünün belirli gereksinimlerine göre uygun yapılandırma uygulanıyor.

Her iki bu iki adımı yalnızca tamamladıktan sonra size aygıt tam olarak sağlandıktan söyleyebilirsiniz. Bazı bulut Hizmetleri yalnızca IOT çözüm uç cihazlara kayıt sağlama işleminin ilk adımı sağlar ancak başlangıç yapılandırmasını sağlamaz. Aygıt hizmeti sağlama ve cihaz için sağlama bir deneyim sağlamak için her iki adımın otomatikleştirir.

## <a name="features-of-the-device-provisioning-service"></a>Hizmet sağlama özellikleri
Aygıt hizmeti sağlama sağlama cihazlar için ideal yapan birçok özelliğe sahiptir.

* **Kanıtlama güvenli** X.509 ve TPM tabanlı kimlikleri için desteği.
* **Kayıt listesi** aygıtları/grupların bir kayıt noktada aygıtların tam kayıt içeren. Kayıt listesi, bunu kaydeder ve herhangi bir zamanda güncelleştirilebilir sonra cihazın istenen yapılandırma hakkında bilgi içerir.
* **Birden çok ayırma ilkeleri** nasıl IOT hub'ları senaryolarınızı desteklemek için cihazları cihaz hizmeti sağlama atar denetlemek için.
* **İzleme ve tanılama günlüklerini** her şeyi düzgün çalıştığından emin olmak için.
* **Çok hub Destek** birden fazla IOT hub'ına aygıtları atamak cihaz sağlama hizmeti sağlar. Cihaz sağlama hizmeti için hub birden çok Azure abonelikleri arasında iletişim kurabilirsiniz.
* **Çapraz bölge desteği** IOT hub'ları diğer bölgelerdeki aygıtları atamak cihaz sağlama hizmeti sağlar.

Kavramları ve cihaz sağlamayı de dahil edilen özellikler hakkında daha fazla bilgiyi [aygıt kavramları](concepts-device.md), [hizmeti kavramları](concepts-service.md), ve [güvenlik kavramları](concepts-security.md).

## <a name="cross-platform-support"></a>Platformlar arası desteği
Cihaz sağlama hizmeti, tüm Azure IOT Hizmetleri gibi çeşitli işletim sistemleri ile platformlar arası çalışır. Azure teklifleri açık kaynak SDK'ları içinde çeşitli [diller](https://github.com/Azure/azure-iot-sdks) kolaylaştırmak için bağlanan cihazların hizmetini ve yönetme. Aygıt hizmeti sağlama cihazları bağlamak için aşağıdaki protokollerini destekler:

* HTTPS
* AMQP
* AMQP websockets üzerinden
* MQTT
* Websockets üzerinden MQTT

Aygıt hizmeti sağlama, yalnızca hizmet işlemleri için HTTPS bağlantılarını destekler.

## <a name="regions"></a>Bölgeler
Cihaz sağlama hizmet birçok bölgede kullanılamıyor. Var olan ve yeni güncelleştirilmiş listesini duyurdu bölgeler adresinde tüm hizmetleri korumak [Azure bölgeleri](https://azure.microsoft.com/regions/). Aygıt hizmeti sağlama kullanılabilir olduğu görebilirsiniz [Azure durum](https://azure.microsoft.com/status/) sayfası.

> [!NOTE]
> Cihaz sağlama genel ve bir konuma bağlı hizmetidir. Ancak, aygıt hizmeti sağlama profiliyle ilişkili meta veri yer alacağı bir bölge belirtmeniz gerekir.

## <a name="availability"></a>Kullanılabilirlik
Biz % 99,9 korumak aygıt hizmeti sağlama ve hizmet düzeyi sözleşmesi için [SLA okuma](https://azure.microsoft.com/support/legal/sla/iot-hub/). [Azure SLA](https://azure.microsoft.com/support/legal/sla/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

## <a name="quotas"></a>Kotalar
Her Azure aboneliği IOT çözümünüzün kapsamını etkileyebilir yerinde varsayılan kota sınırları vardır. Geçerli bir abonelik başına temelinde sağlama 10 cihaz Hizmetleri abonelik başına sınırlıdır.

Kota sınırları hakkında daha fazla ayrıntı için:

* [Azure aboneliği hizmet sınırları](../azure-subscription-service-limits.md)

## <a name="related-azure-components"></a>İlişkili Azure bileşenleri
Cihaz sağlama hizmeti ile Azure IOT Hub cihaz sağlamayı otomatikleştirir. Daha fazla bilgi edinmek [IOT hub'ı](https://docs.microsoft.com/en-us/azure/iot-hub/).

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure IOT cihazları hazırlamaya genel bakış var. Sonraki adım, bir uçtan uca bir IOT senaryosu denemektir.
> [!div class="nextstepaction"]
> [IOT Hub cihaz sağlama hizmetini Azure portalıyla ayarlama](quick-setup-auto-provision.md)
> [oluşturma ve sağlama bir sanal cihaz](quick-create-simulated-device.md)
> [sağlama cihaz ayarlama](tutorial-set-up-device.md)
