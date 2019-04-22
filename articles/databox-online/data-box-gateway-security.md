---
title: Azure veri kutusu ağ geçidi güvenlik | Microsoft Docs
description: Azure veri kutusu ağ geçidi sanal cihaz, hizmet ve şirket içindeki ve buluttaki verileri korumak güvenlik ve gizlilik özellikleri açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: b27b712128ddfbb07a7a7f68f616c20ec3fb53d3
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59685984"
---
# <a name="azure-data-box-gateway-security-and-data-protection"></a>Azure veri kutusu ağ geçidi güvenlik ve veri koruması

Teknoloji gizli veya özel verilerle özellikle kullanılıyorsa, yeni bir Teknoloji benimseme zaman güvenlik önemli bir konudur. Microsoft Azure veri kutusu ağ geçidi çözümü, yalnızca yetkili varlıklar görüntüleme, değiştirme veya verilerinizi silme emin olun yardımcı olur.

Bu makalede, tüm çözüm bileşenlerini ve bunlar üzerinde depolanan verileri korumaya yardımcı Azure veri kutusu ağ geçidi güvenlik özellikleri açıklar.

Veri kutusu ağ geçidi çözüm birbiriyle etkileşim dört ana bileşen oluşur:

- **Azure'da barındırılan veri kutusu ağ geçidi hizmeti** – cihaz sırasını oluşturmak, cihazı yapılandırma ve ardından sırayla tamamlanması izlemek için kullandığınız yönetim kaynak.
- **Veri kutusu ağ geçidi cihazı** – sağladığınız sistemi hiper yönetici içine sağlanan sanal cihaz. Bu sanal cihazı şirket içi verilerinizi Azure'a aktarmak için kullanılır.
- **İstemciler/ana bilgisayarları bağlı cihaza** – veri kutusu ağ geçidi cihazı için bağlayın ve korunması gereken verileri içeren istemcilerin altyapınızdaki.
- **Bulut depolama** – Azure bulutunda verilerin depolandığı konum. Bu konum normalde, oluşturduğunuz veri kutusu ağ geçidi kaynağa bağlı depolama hesabıdır.


## <a name="data-box-gateway-service-protection"></a>Veri kutusu ağ geçidi hizmeti koruma

Veri kutusu ağ geçidi hizmeti, Microsoft Azure'da barındırılan bir yönetim hizmetidir. Hizmet, yapılandırmak ve cihazı yönetmek için kullanılır.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-gateway-device-protection"></a>Veri kutusu ağ geçidi cihaz koruma

Veri kutusu ağ geçidi sağlanan sağlayan bir şirket içi sisteminin hiper yöneticide sanal cihazı cihazdır. Cihaz verileri Azure'a göndermeniz yardımcı olur. Cihazınız:

- Veri kutusu Edge/veri kutusu ağ geçidi hizmetine erişmek için bir etkinleştirme anahtarı gerekir.
- Her zaman bir cihaz parola korumalı.
<!---  secure boot enabled.
- Runs Windows Defender Device Guard. Device Guard allows you to run only trusted applications that you define in your code integrity policies.-->

### <a name="protect-the-device-via-activation-key"></a>Cihaz etkinleştirme anahtarı aracılığıyla koruma

Yetkili bir veri kutusu ağ geçidi cihaz yalnızca Azure aboneliğinizde oluşturduğunuz veri kutusu Ağ Geçidi Hizmeti'ne Katıl izin verilmez. Bir cihaz yetkilendirmek için veri kutusu ağ geçidi hizmeti cihazla etkinleştirmek için etkinleştirme anahtarı kullanmanız gerekir.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Daha fazla bilgi için Git [etkinleştirme anahtarı alma](data-box-gateway-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Cihaz parola aracılığıyla koruma

Parolalar, verilerinizi yalnızca yetkili kullanıcılar için erişilebilir olduğundan emin olun. Veri kutusu ağ geçidi cihaz önyüklemesi kilitli bir durumda.

Şunları yapabilirsiniz:

- Yerel web kullanıcı Arabirimi cihazın bir tarayıcı aracılığıyla bağlanın ve ardından cihazda oturum için bir parola sağlayın.
- Uzaktan HTTP üzerinden cihaz PowerShell arabirimine bağlanın. Uzaktan Yönetim varsayılan olarak etkinleştirilir. Sonra cihazda oturum açmasına cihaz parolasının sunabilir. Daha fazla bilgi için Git [veri kutusu ağ geçidi Cihazınızı uzaktan bağlanma](data-box-gateway-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- Yerel web kullanıcı Arabirimine kullanım [parolayı değiştirmek](data-box-gateway-manage-access-power-connectivity-mode.md#manage-device-access). Parolayı değiştirirseniz, böylece bir oturum açma hatası yaşamamasını tüm uzaktan erişim kullanıcıları bilgilendir emin olun.


## <a name="protect-the-data"></a>Verileri koruma

Bu bölümde, Taşınmakta olan veriler ve depolanan verileri korumaya veri kutusu ağ geçidi güvenlik özellikleri açıklanmaktadır.

### <a name="protect-data-at-rest"></a>Bekleyen verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]

### <a name="protect-data-in-flight"></a>Uçuş verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Depolama hesapları ile verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Döndürme ve ardından [depolama hesap anahtarlarınızı eşitleme](data-box-gateway-manage-shares.md#sync-storage-keys) düzenli olarak, depolama hesabınıza yetkisiz kullanıcılar tarafından erişmediğinden emin olun yardımcı olmak için.

## <a name="manage-personal-information"></a>Kişisel bilgilerini yönetme

Veri kutusu ağ geçidi hizmeti, aşağıdaki anahtar örneklerinde kişisel bilgilerinizi toplar:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Kimin erişebileceğini veya bir paylaşımı silmek kullanıcıların listesini görüntülemek için adımları izleyin. [yönetme veri kutusu ağ geçidi paylaşımlarında](data-box-gateway-manage-shares.md).

Daha fazla bilgi için, [Güven Merkezi](https://www.microsoft.com/trustcenter)’nde Microsoft Gizlilik ilkesini gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

[Veri kutusu ağ geçidi Cihazınızı dağıtma](data-box-gateway-deploy-prep.md).
