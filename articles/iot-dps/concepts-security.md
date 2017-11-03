---
title: "Azure IOT Hub cihaz sağlama hizmetinde güvenlik kavramları | Microsoft Docs"
description: "Güvenlik kavramları aygıt hizmeti sağlama ve IOT Hub ile cihazları için belirli sağlama açıklar"
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 09/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 3ccbaaf55d2bdfedffcdb5ca069798328e2d75fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="iot-hub-device-provisioning-service-security-concepts"></a>IOT Hub cihaz sağlama hizmeti güvenlik kavramları 

IOT Hub cihaz hizmeti sağlama, IOT Hub'de belirtilen bir IOT hub'ına sağlama zero touch Cihazınızı yapılandırmak üzere kullanmak için bir yardımcı hizmetidir. Cihaz sağlama hizmeti ile milyonlarca cihaza güvenli ve ölçeklenebilir bir şekilde sağlayabilirsiniz. Bu makalede genel bir fikir veren *güvenlik* kavramları söz konusu aygıt sağlama. Bu makalede, bir cihaz dağıtım için hazır hale katılan tüm kişiler için geçerlidir.

## <a name="attestation-mechanism"></a>Kanıtlama mekanizması

Kanıtlama mekanizması bir cihazın kimliğini onaylamak için kullanılan yöntem budur. Kanıtlama mekanizması, sağlama hizmeti hangi yöntemi ile belirli bir cihazı kullanmak üzere kanıtlama bildiren kayıt listesine de ilgilidir.

> [!NOTE]
> IOT hub'ı "kimlik doğrulama düzeni" benzer bir kavram, hizmet olarak kullanır.

Cihaz sağlama hizmeti kanıtlama iki tür destekler:
* **X.509 sertifikaları** üzerinde standart X.509 sertifikası kimlik doğrulaması akışı tabanlı.
* **SAS belirteci** tuşları TPM standart kullanarak bir nonce challenge göre. Bu aygıttaki fiziksel TPM gerektirmez, ancak her onay anahtarını kullanarak şifreli olarak hizmet bekliyor [TPM spec](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).

## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Donanım güvenlik modülü ya da HSM, cihaz gizli güvenli, donanım tabanlı depolama için kullanılan ve en güvenli gizli depolama biçimidir. X.509 sertifikaları ve SAS belirteci HSM'de depolanabilir. HSM'ler olabilir hem de kanıtlama mekanizmaları ile kullanılan sağlama destekler.

> [!TIP]
> Gizli aygıtlarınızda güvenli bir şekilde depolamak için bir HSM aygıtlar kullanarak öneririz.

Cihaz gizlilikler de yazılım (bellek) depolanan, ancak bu daha az güvenli depolama HSM daha biçimidir.

## <a name="trusted-platform-module-tpm"></a>Güvenilir Platform Modülü (TPM)

TPM platform kimliğini doğrulamak için kullanılan anahtarları güvenli bir şekilde depolamak için bir standart başvurabilir veya standart uygulama modülleri ile etkileşim kurmak için kullanılan g/ç arabirimi başvurabilirsiniz. TPM'ler ayrık donanım, tümleşik donanım, bellenim veya yazılım tabanlı bulunabilir. Daha fazla bilgi edinmek [TPM'ler ve TPM kanıtlama](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation). Cihaz sağlama hizmeti yalnızca TPM 2.0 destekler.

## <a name="endorsement-key"></a>Onay anahtarı

Onay anahtarı zaman üretim sırasında eklendi ve her TPM için benzersiz olan TPM içinde yer alan bir asimetrik anahtar ' dir. Onay anahtarı kaldırılamaz veya değiştirilemez. Onay anahtarı ortak kısmını orijinal TPM tanımak için kullanılırken onay anahtarını özel bölümünü TPM dışında hiçbir zaman yayımlanır. Daha fazla bilgi edinmek [onay anahtarını](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx).

## <a name="storage-root-key"></a>Depolama kök anahtarı

Depolama kök anahtarı TPM'de depolanan ve böylece bu anahtarlar TPM olmadan kullanılamaz uygulamaları tarafından oluşturulan TPM anahtarlarını korumak için kullanılır. TPM sahipliğini depolama kök anahtarı oluşturulur; Yeni bir kullanıcı sahipliği olabilmesi için TPM temizlediğinizde, yeni bir depolama kök anahtarı oluşturulur. Daha fazla bilgi edinmek [depolama kök anahtarı](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx).

## <a name="root-certificate"></a>Kök sertifikası

Bir kök sertifika bir X.509 sertifikası bir sertifika yetkilisi temsil eden tür ve otomatik olarak imzalanır. Sertifika zinciri terminus olur.

## <a name="intermediate-certificate"></a>Ara Sertifika

Bir ara sertifika, kök sertifika (veya başka bir sertifika, zincirde kök sertifikası ile) imzalanmış ve yaprak sertifikasını imzalamak için kullanılan bir X.509 sertifikası yok.

## <a name="leaf-certificate"></a>Yaprak sertifikası

Bir yaprak sertifikası veya son varlık sertifikası, sertifika sahibinin tanımlamak için kullanılır ve kök sertifikası sertifika zincirinde vardır. Yaprak sertifikası diğer sertifikaları imzalamak için kullanılmaz.
