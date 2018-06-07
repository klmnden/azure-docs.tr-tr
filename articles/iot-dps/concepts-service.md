---
title: Hizmet Azure IOT Hub cihaz sağlama hizmeti kavramları | Microsoft Docs
description: Hizmet dağıtım noktaları ve IOT Hub ile cihazları özgü bir sağlama kavramlarını açıklar
author: nberdy
ms.author: nberdy
ms.date: 03/30/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 2908e08e36f41ebb8a154e7c490e5c6719d911be
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628309"
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>IOT Hub cihaz sağlama hizmeti kavramları

IOT Hub cihaz hizmeti sağlama, IOT Hub'de belirtilen bir IOT hub'ına sağlama zero touch Cihazınızı yapılandırmak üzere kullanmak için bir yardımcı hizmetidir. Cihaz sağlama hizmeti ile yapabilecekleriniz [otomatik sağlama](concepts-auto-provisioning.md) milyonlarca cihaza güvenli ve ölçeklenebilir bir şekilde.

Cihaz sağlama iki bölümden oluşan bir işlemdir. İlk bölümü cihaz IOT çözümü tarafından arasındaki ilk bağlantı kurma *kaydetme* aygıt. İkinci bölümü uygun uygulama *yapılandırma* çözümü belirli gereksinimlerine bağlı olarak aygıta. Her iki adımları tamamladıktan sonra aygıtın tam olarak sağlandıktan *sağlanan*. Cihaz Sağlama Hizmeti, bu adımların ikisini de otomatikleştirerek cihaz için sorunsuz bir sağlama deneyimi sağlar.

Bu makalede yönetmek için en uygun sağlama kavramlarına genel bir fikir veren *hizmet*. Bu makalede kişiler dahil edilen en uygun [bulut Kurulum adım](about-iot-dps.md#cloud-setup-step) bir aygıtı dağıtıma hazır alamazsınız.

## <a name="service-operations-endpoint"></a>Hizmet işlemleri uç noktası

Hizmet işlemleri uç hizmet ayarlarını yönetme ve kayıt listesi korumaya yönelik uç noktadır. Bu uç noktayı yalnızca Hizmet Yöneticisi tarafından kullanılır; cihazlar tarafından kullanılmaz.

## <a name="device-provisioning-endpoint"></a>Cihaz sağlama uç noktası

Cihaz sağlama uç nokta tüm aygıtlar için otomatik olarak sağlama kullanmak tek uç noktadır. Yeni bağlantı bilgilerini aygıtlarla tedarik zinciri senaryolarda reflash gereğini ortadan kaldırmak için tüm sağlama hizmet örnekleri, URL aynıdır. [Kimliği kapsam](#id-scope) Kiracı yalıtımı sağlar.

## <a name="linked-iot-hubs"></a>IoT hub'larına bağlanıldı

Cihaz sağlama hizmeti yalnızca ona bağlı IOT hub'ları cihazlara sağlayabilirsiniz. Bir IOT hub cihaz sağlama hizmetine bağlama IOT hub'ın cihaz kayıt hizmeti okuma/yazma izni verir; Aygıt hizmeti sağlama bağlantısıyla bir cihaz kimliği kayıt ve cihaz çiftine başlangıç yapılandırmasını ayarlayın. Bağlantılı IOT hub'ları Azure herhangi bir bölgede olabilir. Sağlama hizmetinize diğer abonelikler hub bağlantı.

## <a name="allocation-policy"></a>Ayırma İlkesi

Hizmet ayarı düzeyi aygıt hizmeti sağlama aygıtları bir IOT hub'ına nasıl atar belirler. Desteklenen üç ayırma ilkesi vardır:
* **Dağıtım'eşit ağırlıklı**: bağlantılı IOT hub'ları için sağlaması yapılan aygıtlar eşit şekilde etkileyebilir. Varsayılan ayar. Yalnızca bir IoT hub'a aygıtları sağlıyorsanız bu ayarı değiştirmeyebilirsiniz.
* **En düşük gecikme süresine**: cihazları sağlanan bir IOT hub'ına cihaz için en düşük gecikme süresine sahip. Birden çok bağlantılı IOT hub'ları aynı en düşük gecikme sağlar, sağlama hizmeti aygıtlar bu hubs arasında karıştırır.
* **Kayıt listesi aracılığıyla statik Yapılandırması**: İstenen IOT hub'ı kayıt listesinde belirtimi hizmet düzeyi ayırma ilkesine göre öncelik alır.

## <a name="enrollment"></a>Kayıt

Bir kayıt aygıtları veya aracılığıyla otomatik olarak sağlama kaydedebilir aygıt gruplarına kaydıdır. Kaydı cihaz veya cihaz grubunu dahil olmak üzere, ilgili bilgileri içerir:
- [kanıtlama mekanizması](concepts-security.md#attestation-mechanism) aygıt tarafından kullanılan
- İsteğe bağlı ilk istenen yapılandırma
- İstenen IOT hub'ı
- İstenen cihaz kimliği

Cihaz sağlama hizmeti tarafından desteklenen kayıtları iki tür vardır:

### <a name="enrollment-group"></a>Kayıt grubu

Bir kayıt grubu belirli kanıtlama mekanizması paylaşan aygıtları grubudur. Kayıt gruptaki tüm cihazlar aynı kök veya ara CA tarafından imzalanmış X.509 sertifikalarını sunar. Kayıt grupları yalnızca X.509 kanıtlama mekanizması kullanabilirsiniz. Kayıt grup adı ve sertifika adı alfasayısal, küçük harf olmalıdır ve kısa çizgi içerebilir.

> [!TIP]
> Çok sayıda istenen ilk yapılandırma paylaşan aygıtları veya cihazları bir kayıt grubunu kullanarak aynı Kiracı tüm gitmeyi öneririz.

### <a name="individual-enrollment"></a>Tek tek kayıt

Tek bir kayıt kaydedebilir tek bir cihaz için bir giriştir. Tek tek kayıtları kanıtlama mekanizmaları X.509 yaprak sertifikalar veya SAS belirteçlerinden (fiziksel veya sanal TPM) kullanabilir. Tek bir kayıt kayıt Kimliğini alfasayısal, küçük harf ve kısa çizgi içerebilir. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

> [!TIP]
> Benzersiz ilk yapılandırmaları gerektirir cihazlar için veya yalnızca TPM kanıtlama aracılığıyla SAS belirteci kullanarak kimlik doğrulayabilir cihazlar için tek tek kayıtları kullanmanızı öneririz.

## <a name="registration"></a>Kayıt

Bir kaydı bir cihaz kaydetme/sağlamayı başarıyla cihaz sağlama hizmeti üzerinden bir IOT Hub'ına kaydıdır. Kayıt kayıtları otomatik olarak oluşturulur; silinebilmesi için ancak bunlar güncelleştirilemiyor.

## <a name="operations"></a>İşlemler

Aygıt hizmeti sağlama fatura birimi işlemleridir. Bir yönerge hizmetine başarılı tamamlanması bir işlemdir. Cihaz kayıtları ve yeniden kayıtları işlemlere dahildir. Ayrıca kayıt liste girdilerini ekleme ve güncelleştirme gibi hizmet tarafı değişiklikleri de işlemlere dahildir.
