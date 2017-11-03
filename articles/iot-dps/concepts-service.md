---
title: "Hizmet Azure IOT Hub cihaz sağlama hizmeti kavramları | Microsoft Docs"
description: "Hizmet dağıtım noktaları ve IOT Hub ile cihazları özgü bir sağlama kavramlarını açıklar"
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 10/03/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 96c63e5d0379150ea619dbbe912a21e373f808af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>IOT Hub cihaz sağlama hizmeti kavramları

IOT Hub cihaz hizmeti sağlama, IOT Hub'de belirtilen bir IOT hub'ına sağlama zero touch Cihazınızı yapılandırmak üzere kullanmak için bir yardımcı hizmetidir. Cihaz sağlama hizmeti ile milyonlarca cihaza güvenli ve ölçeklenebilir bir şekilde sağlayabilirsiniz.

Cihaz sağlama iki bölümden oluşan bir işlemdir. İlk bölümü cihaz IOT çözümü tarafından arasındaki ilk bağlantı kurma *kaydetme* aygıt. İkinci bölümü uygun uygulama *yapılandırma* çözümü belirli gereksinimlerine bağlı olarak aygıta. Her iki adımları tamamladıktan sonra size aygıt tam olarak sağlandıktan diyebilirsiniz *sağlanan*. Aygıt hizmeti sağlama ve cihaz için sağlama bir deneyim sağlamak için her iki adımın otomatikleştirir.

Bu makalede yönetmek için en uygun sağlama kavramlarına genel bir fikir veren *hizmet*. Bu makalede kişiler dahil edilen en uygun [bulut Kurulum adım](about-iot-dps.md#cloud-setup-step) bir aygıtı dağıtıma hazır alamazsınız.

## <a name="service-operations-endpoint"></a>Hizmet işlemleri uç noktası

Hizmet işlemleri uç hizmet ayarlarını yönetme ve kayıt listesi korumaya yönelik uç noktadır. Bu uç noktayı yalnızca Hizmet Yöneticisi tarafından kullanılır; cihazlar tarafından kullanılmaz.

## <a name="device-provisioning-endpoint"></a>Cihaz sağlama uç noktası

Uç nokta sağlama cihaz tüm aygıtlara sağlamak için konuşun merkezi uç noktadır. URL tüm sağlama hizmetlerin yeni bağlantı bilgilerini aygıtlarla tedarik zinciri senaryolarda reflash gereğini ortadan kaldırmak aynıdır. [Kimliği kapsam](#id-scope) Kiracı yalıtımı sağlar.

## <a name="linked-iot-hubs"></a>Bağlantılı IOT hub'ları

Cihaz sağlama hizmeti yalnızca ona bağlı IOT hub'ları cihazlara sağlayabilirsiniz. Bir IOT hub cihaz sağlama hizmetine bağlama IOT hub'ın cihaz kayıt hizmeti okuma/yazma izni verir; Aygıt hizmeti sağlama bağlantısıyla bir cihaz kimliği kayıt ve cihaz çiftine başlangıç yapılandırmasını ayarlayın. Bağlantılı IOT hub'ları Azure herhangi bir bölgede olabilir. Sağlama hizmetinize diğer abonelikler hub bağlantı.

## <a name="allocation-policy"></a>Ayırma İlkesi

Hizmet ayarı düzeyi aygıt hizmeti sağlama aygıtları bir IOT hub'ına nasıl atar belirler. Üç desteklenen ayırma ilkeleri vardır:
* **Dağıtım'eşit ağırlıklı**: bağlantılı IOT hub'ları için sağlaması yapılan aygıtlar eşit şekilde etkileyebilir. Varsayılan ayar. Yalnızca bir IOT hub'ına aygıtları sağlıyorsanız, bu ayarı tutabilirsiniz.
* **En düşük gecikme süresine**: cihazları sağlanan bir IOT hub'ına cihaz için en düşük gecikme süresine sahip. Birden çok bağlantılı IOT hub'ları aynı en düşük gecikme sağlar, sağlama hizmeti aygıtlar bu hubs arasında karıştırır.
* **Kayıt listesi aracılığıyla statik Yapılandırması**: İstenen IOT hub'ı kayıt listesinde belirtimi hizmet düzeyi ayırma ilkesine göre öncelik alır.

## <a name="enrollment"></a>Kayıt

Bir kayıt aygıtları veya bir kayıt noktada aygıtları gruplarının kayıttır. Hub ve istenen cihaz kimliği, aygıtlar ve isteğe bağlı olarak ilk istenen yapılandırma IOT istenen için kanıtlama yöntemi de dahil olmak üzere, kaydı cihaz veya cihaz grubu hakkında bilgi içerir Cihaz sağlama hizmeti tarafından desteklenen kayıtları iki tür vardır.

### <a name="enrollment-group"></a>Kayıt grubu

Bir kayıt grubu belirli kanıtlama mekanizması paylaşan aygıtları grubudur. Kayıt gruptaki tüm cihazlar aynı kök CA tarafından imzalanmış X.509 sertifikalarını sunar. Kayıt grupları yalnızca X.509 kanıtlama mekanizması kullanabilirsiniz. Kayıt grup adı ve sertifika adı alfasayısal, küçük harf olmalıdır ve kısa çizgi içerebilir.

> [!TIP]
> Çok sayıda istenen ilk yapılandırma paylaşan aygıtları veya cihazları bir kayıt grubunu kullanarak aynı Kiracı tüm gitmeyi öneririz.

### <a name="individual-enrollment"></a>Tek tek kayıt

Tek bir kayıt kaydedebilir tek bir cihaz için bir giriştir. Tek tek kayıtları kanıtlama mekanizmaları X.509 sertifikalarını veya SAS belirteçlerinde (gerçek veya sanal TPM) kullanabilir. Tek bir kayıt kayıt Kimliğini alfasayısal, küçük harf ve kısa çizgi içerebilir. Tek tek kayıtları, belirtilen istenen IOT hub cihaz kimliği olabilir.

> [!TIP]
> Benzersiz ilk yapılandırmaları gerektirir cihazlar için veya TPM ya da sanal TPM aracılığıyla SAS belirteci kanıtlama mekanizması olarak yalnızca kullanabilirsiniz cihazlar için tek tek kayıtları kullanmanızı öneririz.

## <a name="registration"></a>Kayıt

Bir kaydı bir cihaz kaydetme/sağlamayı başarıyla cihaz sağlama hizmeti üzerinden bir IOT Hub'ına kaydıdır. Kayıt kayıtları otomatik olarak oluşturulur; silinebilmesi için ancak bunlar güncelleştirilemiyor.

## <a name="operations"></a>İşlemler

Aygıt hizmeti sağlama fatura birimi işlemleridir. Bir yönerge hizmetine başarılı tamamlanması bir işlemdir. Cihaz kayıtları ve yeniden kayıtları işlemlere dahildir. Ayrıca kayıt liste girdilerini ekleme ve güncelleştirme gibi hizmet tarafı değişiklikleri de işlemlere dahildir.
