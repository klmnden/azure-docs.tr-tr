---
title: Azure cihaz sağlamayı aygıt kavramlar | Microsoft Docs
description: Cihaz kavramları aygıt hizmeti sağlama ve IOT Hub ile cihazları için belirli sağlama açıklar
services: iot-dps
keywords: ''
author: nberdy
ms.author: nberdy
ms.date: 09/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 5482801461e2afea33d65d559723116f37a35d1f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="iot-hub-device-provisioning-service-device-concepts"></a>IOT Hub cihaz sağlama hizmeti aygıt kavramları

IOT Hub cihaz hizmeti sağlama, IOT Hub'de belirtilen bir IOT hub'ına sağlama zero touch Cihazınızı yapılandırmak üzere kullanmak için bir yardımcı hizmetidir. Cihaz Sağlama Hizmeti ile güvenli ve ölçeklenebilir bir şekilde milyonlarca cihaz sağlayabilirsiniz.

Bu makalede genel bir fikir veren *aygıt* kavramları söz konusu aygıt sağlama. Bu makalede kişiler dahil edilen en uygun [üretim adım](about-iot-dps.md#manufacturing-step) bir aygıtı dağıtıma hazır alamazsınız.

## <a name="attestation-mechanism"></a>Kanıtlama mekanizması

Kanıtlama mekanizması bir cihazın kimliğini onaylamak için kullanılan yöntem budur. Kanıtlama mekanizması, sağlama hizmeti hangi yöntemi ile belirli bir cihazı kullanmak üzere kanıtlama bildiren kayıt listesine de ilgilidir.

> [!NOTE]
> IOT hub'ı "kimlik doğrulama düzeni" benzer bir kavram, hizmet olarak kullanır.

Cihaz sağlama hizmeti kanıtlama iki tür destekler:
* **X.509 sertifikaları** üzerinde standart X.509 sertifikası kimlik doğrulaması akışı tabanlı.
* **Güvenilir Platform Modülü (TPM)** imzalı bir paylaşılan erişim imzası (SAS) belirteci sunmak için anahtarlar TPM standart kullanarak bir nonce challenge üzerinde temel. Bu aygıttaki fiziksel TPM gerektirmez, ancak her onay anahtarını kullanarak şifreli olarak hizmet bekliyor [TPM spec](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).

## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Donanım güvenlik modülü ya da HSM, cihaz gizli güvenli, donanım tabanlı depolama için kullanılan ve en güvenli gizli depolama biçimidir. X.509 sertifikaları ve SAS belirteci HSM'de depolanabilir. HSM'ler hem de kanıtlama mekanizmalarıyla kullanılabilir sağlama hizmeti destekler.

> [!TIP]
> Gizli aygıtlarınızda güvenli bir şekilde depolamak için bir HSM aygıtlar kullanarak öneririz.

Cihaz gizlilikler de yazılım (bellek) depolanan, ancak bu daha az güvenli depolama HSM daha biçimidir.

## <a name="registration-id"></a>Kayıt kimliği

Kayıt kimliği, cihaz hizmeti sağlama bir cihazı benzersiz olarak tanımlamak için kullanılır. Cihaz kimliği sağlama hizmetinde benzersiz [kimliği kapsam](#id-scope). Her bir cihaz kayıt kimliği olması gerekir. Kayıt Kimliği alfasayısal, küçük harf ve kısa çizgi içerebilir.

* TPM söz konusu olduğunda, kayıt kimliği TPM tarafından sağlanır.
* X.509 tabanlı kanıtlama söz konusu olduğunda, kayıt kimliği, sertifika konu adı olarak sağlanır.

## <a name="device-id"></a>Cihaz Kimliği

IOT Hub ' göründüğü gibi cihaz kimliği kimliğidir. İstenen cihaz kimliği kayıt girişi ayarlanabilir, ancak ayarlanması için gerekli değildir. İstenen cihaz kimliği kayıt listesinde belirtilirse, kayıt kimliği cihaz kaydedilirken cihaz kimliği olarak kullanılır. Daha fazla bilgi edinmek [cihaz IOT Hub'ındaki kimliklerini](../iot-hub/iot-hub-devguide-identity-registry.md).

## <a name="id-scope"></a>Kapsam kimliği

Kullanıcı tarafından oluşturulan ve aygıt üzerinden kaydedeceksiniz sağlama hizmete benzersiz şekilde tanımlamak için kullanılan kimliği kapsam bir aygıt hizmeti sağlama için atanır. Kimliği kapsam hizmeti tarafından oluşturulan ve sabit, hangi benzersizliği garanti eder.

> [!NOTE]
> Benzersizlik uzun süre çalışan dağıtım işlemlerini ve birleşme ve alım senaryoları için önemlidir.
