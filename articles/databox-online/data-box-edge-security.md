---
title: Azure veri kutusu kenar güvenlik | Microsoft Docs
description: Azure veri kutusu Edge cihaz, hizmet ve şirket içindeki ve buluttaki verileri korumaya güvenlik ve gizlilik özellikleri açıklar.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 5316ddf9d456731f2789241434926366f732993a
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682109"
---
# <a name="azure-data-box-edge-security-and-data-protection"></a>Azure veri kutusu kenar güvenlik ve veri koruması

Teknoloji gizli veya özel verilerle özellikle kullanılıyorsa, yeni bir Teknoloji benimseme zaman güvenlik önemli bir konudur. Microsoft Azure veri kutusu Edge çözümü, yalnızca yetkili varlıklar görüntüleme, değiştirme veya verilerinizi silme emin olun yardımcı olur.

Bu makalede, tüm çözüm bileşenlerini ve bunlar üzerinde depolanan verileri korumaya yardımcı olmak veri kutusu kenar güvenlik özellikleri açıklanır.

Azure veri kutusu Edge çözüm birbiriyle etkileşim dört ana bileşenden oluşur:

- **Azure'da barındırılan bir veri kutusu Edge hizmete** – cihaz sırasını oluşturmak, cihazı yapılandırma ve ardından sırayla tamamlanması izlemek için kullandığınız yönetim kaynak.
- **Veri kutusu Edge cihazı** – şirket içi verilerinizi Azure'a aktarmanız kadar size sevk aktarım cihazı.
- **İstemciler/ana bilgisayarları bağlı cihaza** – veri kutusu Edge cihazına bağlanmak ve korunması gereken verileri içeren istemcilerin altyapınızdaki.
- **Bulut depolama** – Azure bulutunda verilerin depolandığı konum. Bu konum normalde, oluşturduğunuz veri kutusu Edge kaynağa bağlı depolama hesabıdır.

## <a name="data-box-edge-service-protection"></a>Veri kutusu Edge hizmet koruma

Microsoft Azure'da barındırılan bir yönetim hizmeti veri kutusu uç hizmetidir. Hizmet, yapılandırmak ve cihazı yönetmek için kullanılır.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-edge-device-protection"></a>Veri kutusu Edge cihaz koruma

Veri kutusu sınır cihazı, yerel olarak işleme ve sonra bunu Azure'a göndererek verileri dönüştürme yardımcı olan bir şirket içi cihazdır. Cihazınız:

- Veri kutusu Edge hizmetine erişmek için bir etkinleştirme anahtarı gerekir.
- Her zaman bir cihaz parola korumalı.
- Kilitli aygıttır. Cihaz BMC ve BIOS BIOS sınırlı kullanıcı erişimi ile parola korumalı.
- Güvenli Önyükleme etkin.
- Windows Defender'ı cihaz koruyucusu çalıştırır. Device Guard, yalnızca kod bütünlüğü ilkelerinizde tanımladığınız güvenilen uygulamaları çalıştıracak olanak tanır.

### <a name="protect-the-device-via-activation-key"></a>Cihaz etkinleştirme anahtarı aracılığıyla koruma

Yetkili bir veri kutusu Edge cihazı yalnızca Azure aboneliğinizde oluşturduğunuz veri kutusu Edge Hizmeti'ne Katıl izin verilmez. Bir cihaz yetkilendirmek için veri kutusu Edge hizmetiyle cihaz etkinleştirmek için etkinleştirme anahtarı kullanmanız gerekir.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Daha fazla bilgi için Git [etkinleştirme anahtarı alma](data-box-edge-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Cihaz parola aracılığıyla koruma

Parolalar, verilerinizi yalnızca yetkili kullanıcılar için erişilebilir olduğundan emin olun. Veri kutusu Edge cihazları önyükleme kilitli bir durumda.

Şunları yapabilirsiniz:

- Yerel web kullanıcı Arabirimi cihazın bir tarayıcı aracılığıyla bağlanın ve ardından cihazda oturum için bir parola sağlayın.
- Uzaktan HTTP üzerinden cihaz PowerShell arabirimine bağlanın. Uzaktan Yönetim varsayılan olarak etkinleştirilir. Sonra cihazda oturum açmasına cihaz parolasının sunabilir. Daha fazla bilgi için Git [veri kutusu Edge cihazınıza uzaktan bağlanma](data-box-edge-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- Yerel web kullanıcı Arabirimine kullanım [parolayı değiştirmek](data-box-edge-manage-access-power-connectivity-mode.md#manage-device-access). Parolayı değiştirirseniz, böylece bir oturum açma hatası yaşamamasını tüm uzaktan erişim kullanıcıları bilgilendir emin olun.

## <a name="protect-the-data"></a>Verileri koruma

Bu bölümde, Taşınmakta olan veriler ve depolanan verileri korumaya veri kutusu kenar güvenlik özellikleri açıklanmaktadır.

### <a name="protect-data-at-rest"></a>Bekleyen verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]

### <a name="protect-data-in-flight"></a>Uçuş verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Depolama hesapları ile verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Döndürme ve ardından [depolama hesap anahtarlarınızı eşitleme](data-box-edge-manage-shares.md#sync-storage-keys) düzenli olarak, depolama hesabınıza yetkisiz kullanıcılar tarafından erişmediğinden emin olun yardımcı olmak için.

## <a name="manage-personal-information"></a>Kişisel bilgilerini yönetme

Veri kutusu uç hizmeti, aşağıdaki anahtar örneklerinde kişisel bilgilerini toplar:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Kimin erişebileceğini veya bir paylaşımı silmek kullanıcıların listesini görüntülemek için adımları izleyin. [yönetme veri kutusu uçta paylaşımları](data-box-edge-manage-shares.md).

Daha fazla bilgi için, [Güven Merkezi](https://www.microsoft.com/trustcenter)’nde Microsoft Gizlilik ilkesini gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

[Veri kutusu Edge cihazınıza dağıtma](data-box-edge-deploy-prep.md).
