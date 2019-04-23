---
title: Hizmet, Azure IOT Hub cihazı sağlama hizmeti kavramları | Microsoft Docs
description: Hizmeti özgü cihaz sağlama hizmeti ve IOT Hub ile cihaz sağlama kavramları açıklar.
author: nberdy
ms.author: nberdy
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 4a4f53f991355e634e8139f9e90bec6c508a527d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59792756"
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>IOT Hub cihazı sağlama hizmeti kavramları

IOT Hub cihazı sağlama hizmeti, sıfır dokunma cihaz sağlama belirtilen bir IOT hub'ı yapılandırmak için kullandığınız bir IOT Hub için bir yardımcı hizmettir. Cihaz sağlama hizmeti ile yapabilecekleriniz [otomatik sağlama](concepts-auto-provisioning.md) milyonlarca cihazı güvenli ve ölçeklenebilir bir şekilde.

Cihaz sağlama iki bölümlü bir işlemdir. İlk bölümü, cihaz ile IOT çözümü tarafından arasında ilk bağlantı kurma *kaydetme* cihaz. İkinci bölümü, uygun uygulama *yapılandırma* çözümünün belirli gereksinimlerine göre cihaza. Her iki adım tamamladıktan sonra cihazın tamamen kaldırıldı *sağlanan*. Cihaz Sağlama Hizmeti, bu adımların ikisini de otomatikleştirerek cihaz için sorunsuz bir sağlama deneyimi sağlar.

Bu makalede sağlama kavramları yönetmek için en uygun genel bir fikir veren *hizmet*. Bu makalede katılan kişilikler için en uygun [bulut Kurulum adımı](about-iot-dps.md#cloud-setup-step) dağıtım için hazır bir aygıt alma.

## <a name="service-operations-endpoint"></a>Hizmet işlemleri uç noktası

Hizmet işlemleri uç noktası için hizmet ayarlarını yönetme ve koruma kayıt listesi uç noktadır. Bu uç noktaya, yalnızca Hizmet Yöneticisi tarafından kullanılır; cihazlar tarafından kullanılmaz.

## <a name="device-provisioning-endpoint"></a>Cihaz sağlama uç noktası

Cihaz sağlama uç noktası, tüm cihazlar için otomatik sağlamayı kullanın. uç noktasıdır. Tedarik zinciri senaryolarda cihazlar yeni bağlantı bilgileriyle reflash gereksinimini ortadan kaldırmak için tüm sağlama hizmeti örneklerini, URL aynıdır. Kimlik kapsamı Kiracı yalıtımı sağlar.

## <a name="linked-iot-hubs"></a>IoT hub'larına bağlanıldı

Cihaz sağlama hizmeti, yalnızca kendisine bağlanmış olan IOT hub'ları cihazlarına sağlayabilirsiniz. Bir IOT hub cihaz sağlama hizmeti örneğine bağlanma, IOT hub'ınızın cihaz kayıt hizmeti okuma/yazma izni verir; bağlantı ile bir cihaz sağlama hizmeti cihaz kimliği kayıt ve cihaz ikizinde başlangıç yapılandırmasını ayarlayın. Bağlı IOT hub'ları dilediğiniz Azure bölgesinde olabilir. Diğer Aboneliklerdeki hub'ları sağlama hizmetinize bağlanabilir.

## <a name="allocation-policy"></a>Ayırma İlkesi

Cihaz sağlama hizmeti cihazların bir IOT hub'ına nasıl atar ayarı hizmet düzeyi belirler. Desteklenen üç ayırma ilkesi vardır:

* **Eşit ağırlıklı dağılım**: bağlı IOT hub'lara cihaz sağlanma olasılığı. Varsayılan ayar. Yalnızca bir IoT hub'a aygıtları sağlıyorsanız bu ayarı değiştirmeyebilirsiniz.

* **En düşük gecikme**: cihazlar sağlanabilir bir IOT hub'ına cihaz için en düşük gecikme ile. Sağlama hizmeti birden çok IOT hub'larına bağlanıldı aynı düşük gecikme sağlar, cihazlar arasında bu hubs'a karıştırır.

* **Kayıt listesi aracılığıyla statik yapılandırma**: kayıt listesindeki istenen IOT hub'ın belirtimi, hizmet düzeyi ayırma ilkesinden önceliklidir öncelik alır.

## <a name="enrollment"></a>Kayıt

Bir kayıt veya cihaz üzerinden otomatik olarak sağlama kaydedebilir cihaz gruplarının kaydıdır. Kayıt, cihaz veya cihaz grubunuz, dahil olmak üzere ilgili bilgileri içerir:
- [kanıtlama mekanizması](concepts-security.md#attestation-mechanism) cihaz tarafından kullanılan
- İsteğe bağlı ilk istenen yapılandırma
- İstenen IOT hub'ı
- İstenen cihaz kimliği

Cihaz sağlama hizmeti tarafından desteklenen kayıtları iki tür vardır:

### <a name="enrollment-group"></a>Kayıt grubu

Kayıt grubu, belirli bir kanıtlama mekanizmasını paylaşan cihazlar grubudur. Kayıt gruptaki tüm cihazlar aynı kök tarafından imzalanan X.509 sertifikaları veya ara sertifika yetkilisi (CA) sunar. Kayıt grupları yalnızca X.509 kanıtlama mekanizması olarak kullanabilirsiniz. Kayıt grubu adı ve sertifika adı alfasayısal, küçük harf olmalıdır ve kısa çizgi içerebilir.

> [!TIP]
> Aynı kiracıya giden tüm cihazlar veya çok sayıda istenen bir ilk yapılandırmayı paylaşan cihazlar için kayıt grubu kullanılması önerilir.

### <a name="individual-enrollment"></a>Bireysel kayıt

Bireysel kayıt kaydedebilir, tek bir cihaz için bir girdidir. Bireysel kayıtlar, kanıtlama mekanizması olarak X.509 yaprak sertifikalarını veya SAS belirteçlerini (Başlangıç, fiziksel ya da sanal TPM'de) kullanabilir. Bireysel kayıt kayıt Kimliğini alfasayısal, küçük harf ve kısa çizgi içerebilir. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

> [!TIP]
> Benzersiz ilk yapılandırma gerektiren cihazlar için veya yalnızca TPM kanıtlama aracılığıyla SAS belirteçleri kullanarak kimlik doğrulaması cihazlar için bireysel kayıtların kullanılmasını öneririz.

## <a name="registration"></a>Kayıt

Bir kaydı, bir cihazın başarıyla kaydetme ve sağlama bir IOT hub cihaz sağlama hizmeti aracılığıyla kaydıdır. Kayıt kayıtları otomatik olarak oluşturulur; silinebilir, ancak bunlar güncelleştirilemiyor.

## <a name="operations"></a>İşlemler

Cihaz sağlama hizmeti faturalandırma birimi işlemlerdir. Bir yönerge hizmetine başarılı olarak tamamlanmasına bir işlemdir. Cihaz kayıtları ve yeniden kayıtları işlemlere dahildir. Ayrıca kayıt liste girdilerini ekleme ve güncelleştirme gibi hizmet tarafı değişiklikleri de işlemlere dahildir.
