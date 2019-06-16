---
title: Azure cihaz sağlama cihaz kavramlar | Microsoft Docs
description: Cihaz sağlama kavramları belirli cihazlara cihaz sağlama hizmeti ve IOT Hub ile açıklar
author: nberdy
ms.author: nberdy
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 2904da863707c5f653d774b0a480cc48c95c8d1c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60745987"
---
# <a name="iot-hub-device-provisioning-service-device-concepts"></a>IOT Hub cihazı sağlama hizmeti cihaz kavramları

IOT Hub cihazı sağlama hizmeti, sıfır dokunma cihaz sağlama belirtilen bir IOT hub'ı yapılandırmak için kullandığınız bir IOT Hub için bir yardımcı hizmettir. Cihaz Sağlama Hizmeti ile güvenli ve ölçeklenebilir bir şekilde milyonlarca cihaz sağlayabilirsiniz.

Bu makalede, genel bir fikir veren *cihaz* kavramları söz konusu cihaz sağlama. Bu makalede katılan kişilikler için en uygun [üretim adım](about-iot-dps.md#manufacturing-step) dağıtım için hazır bir aygıt alma.

## <a name="attestation-mechanism"></a>Kanıtlama mekanizması

Kanıtlama mekanizması bir cihazın kimliğini onaylamak için kullanılan bir yöntemdir. Kanıtlama mekanizması, sağlama hizmeti kanıtlama belirli bir cihaz ile kullanmak için hangi yöntemin belirten bir kayıt listesi için de geçerlidir.

> [!NOTE]
> IOT Hub için benzer bir kavram bu hizmet, "kimlik doğrulama şeması" kullanır.

Cihaz sağlama hizmeti, aşağıdaki biçimlerden kanıtlama destekler:
* **X.509 sertifikaları** temel standart X.509 sertifika kimlik doğrulaması akış.
* **Güvenilir Platform Modülü (TPM)** imzalı bir paylaşılan erişim imzası (SAS) belirteç sunmak için anahtarları için TPM standart kullanarak bir nonce challenge üzerinde temel. Bu cihazda bir fiziksel TPM'nin gerektirmez, ancak her onay anahtarını kullanarak gerçekleştiğini doğrulamak hizmet bekliyor [TPM spec](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).
* **Simetrik anahtar** paylaşılan erişim imzası (SAS) tabanlı [güvenlik belirteçleri](../iot-hub/iot-hub-devguide-security.md#security-tokens), karma bir imza ve katıştırılmış bir sona erme içerir. Daha fazla bilgi için [simetrik anahtar kanıtı](concepts-symmetric-key-attestation.md).

## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Donanım güvenlik modülüne veya HSM, cihaz gizli dizileri güvenli, donanım tabanlı depolama için kullanılır ve en güvenli gizli dizi depolama biçimindedir. X.509 sertifikaları hem de SAS belirteçlerini HSM'de depolanabilir. HSM'ler hem de kanıtlama mekanizmaları ile kullanılabilir sağlama hizmetini destekler.

> [!TIP]
> Güvenli bir şekilde cihazlarınızda gizli dizileri depolamak için cihazlarla bir HSM kullanıldığında öneririz.

Cihaz gizli dizileri, yazılım (bellek) depolanabilir, ancak bu daha az güvenli bir depolama biçiminin bir HSM daha.

## <a name="registration-id"></a>Kayıt Kimliği

Kayıt kimliği, bir cihaz sağlama Hizmeti'ne cihazı benzersiz şekilde tanımlamak için kullanılır. Cihaz kimliği sağlama hizmetine benzersiz [kimlik kapsamı](#id-scope). Her cihaz bir kayıt kimliği olması gerekir. Kayıt Kimliği alfasayısal, küçük harf ve kısa çizgi içerebilir.

* TPM söz konusu olduğunda, kayıt Kimliğini TPM tarafından sağlanır.
* X.509 tabanlı kanıtlama söz konusu olduğunda, kayıt Kimliğini sertifikanın konu adı olarak sağlanır.

## <a name="device-id"></a>Cihaz Kimliği

IOT Hub'ında göründüğü gibi cihaz kimliği kimliğidir. İstenen cihaz kimliği kayıt girişi ayarlanabilir, ancak ayarlamak için gerekli değildir. İstenen cihaz kimliği kayıt listesinde belirtilirse, kayıt kimliği cihaz kaydederken bir cihaz kimliği kullanılır. Daha fazla bilgi edinin [cihaz IOT hub kimliklerini](../iot-hub/iot-hub-devguide-identity-registry.md).

## <a name="id-scope"></a>Kimlik kapsamı

Kimlik kapsamı, kullanıcı tarafından oluşturulan ve belirli sağlama hizmeti aracılığıyla cihazı kaydedeceksiniz benzersiz şekilde tanımlamak için kullanılan bir cihaz sağlama Hizmeti'ne atanır. Kimlik kapsamı hizmeti tarafından oluşturulan ve değişmezse, hangi benzersizliği garanti eder.

> [!NOTE]
> Benzersizlik, uzun süre çalışan dağıtım işlemlerini ve birleşme ve alım senaryolar için önemlidir.
