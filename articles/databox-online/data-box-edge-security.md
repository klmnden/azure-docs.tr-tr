---
title: Azure veri kutusu kenar güvenlik | Microsoft Docs
description: Azure veri kutusu Edge cihaz, hizmet ve veri şirket içi koruma güvenlik ve gizlilik özellikleri açıklar ve bulut.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 8823aebe17a5446b3c507878833c2525c338dde1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64718011"
---
# <a name="azure-data-box-edge-security-and-data-protection"></a>Azure veri kutusu kenar güvenlik ve veri koruması

Yeni bir Teknoloji benimseme özellikle teknolojisi ile gizli veya özel verileri kullandıysanız güvenlik büyük kaygısı andır. Varlıkları yalnızca yetkili emin olmanız azure veri kutusu Edge yardımcı görüntüleme, değiştirme veya verilerinizi silin.

Bu makalede, her çözüm bileşenlerini ve bunlarda depolanan verileri korumaya yardımcı olmak veri kutusu kenar güvenlik özellikleri açıklanır.

Azure veri kutusu Edge birbiriyle etkileşim dört ana bileşenden oluşur:

- **Azure'da barındırılan veri kutusu Edge hizmetine**. Cihaz sipariş oluşturmak için kullandığınız yönetim kaynak cihazı yapılandırma ve sonra siparişin tamamlanana kadar izleyin.
- **Veri kutusu Edge cihazı**. Şirket içi verilerinizi Azure'a alabilmeniz için sevk aktarım cihazı.
- **İstemciler/ana bilgisayarları bağlı cihaza**. Veri kutusu Edge cihazına bağlanmak ve korunması gereken verileri içeren istemcilerin altyapınızdaki.
- **Bulut depolama**. Azure bulut platformunda verilerin depolandığı konumu. Bu konum normalde, oluşturduğunuz veri kutusu Edge kaynağa bağlı depolama hesabıdır.

## <a name="data-box-edge-service-protection"></a>Veri kutusu Edge hizmet koruma

Azure'da barındırılan bir yönetim hizmeti veri kutusu uç hizmetidir. Hizmet, yapılandırmak ve cihazı yönetmek için kullanılır.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-edge-device-protection"></a>Veri kutusu Edge cihaz koruma

Veri kutusu sınır cihazı, verilerinizi yerel olarak işleme ve daha sonra Azure'a gönderme dönüştürme yardımcı olan bir şirket içi cihazdır. Cihazınız:

- Veri kutusu Edge hizmetine erişmek için bir etkinleştirme anahtarı gerekir.
- Her zaman bir cihaz parola korumalı.
- Kilitli aygıttır. Cihaz BMC ve BIOS parola korumalı. BIOS sınırlı kullanıcı erişimi tarafından korunur.
- Güvenli Önyükleme etkin.
- Windows Defender'ı cihaz koruyucusu çalıştırır. Kod bütünlüğü ilkelerinizi tanımladığınız güvenilen uygulamaları çalıştıracak cihaz koruma sağlar.

### <a name="protect-the-device-via-activation-key"></a>Cihaz etkinleştirme anahtarı aracılığıyla koruma

Yetkili bir veri kutusu Edge cihazı yalnızca Azure aboneliğinizde oluşturduğunuz veri kutusu Edge Hizmeti'ne Katıl izin verilmez. Bir cihaz yetkilendirmek için veri kutusu Edge hizmetiyle cihaz etkinleştirmek için etkinleştirme anahtarı kullanmanız gerekir.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Daha fazla bilgi için [etkinleştirme anahtarı alma](data-box-edge-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Cihaz parola aracılığıyla koruma

Parolalar, verilerinizi yalnızca yetkili kullanıcıların erişebildiğinden emin olun. Veri kutusu Edge cihazları önyükleme kilitli bir durumda.

Şunları yapabilirsiniz:

- Yerel web kullanıcı Arabirimi cihazın bir tarayıcı aracılığıyla bağlanın ve ardından cihaza oturum açmak için bir parola sağlayın.
- Uzaktan HTTP üzerinden cihaz PowerShell arabirimine bağlanın. Uzaktan Yönetim varsayılan olarak etkinleştirilir. Sonra cihaza oturum açmak için cihaz parolasının sunabilir. Daha fazla bilgi için [veri kutusu Edge cihazınıza uzaktan bağlanma](data-box-edge-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- Yerel web kullanıcı Arabirimine kullanım [parolayı değiştirmek](data-box-edge-manage-access-power-connectivity-mode.md#manage-device-access). Oturum açmada sorun zorunluluğunu tüm uzaktan erişim kullanıcılara bildirmek parolayı değiştirirseniz, unutmayın.

## <a name="protect-your-data"></a>Verilerinizi koruyun

Bu bölümde, aktarım sırasında ve depolanan verileri korumak veri kutusu kenar güvenlik özellikleri açıklanmaktadır.

### <a name="protect-data-at-rest"></a>Bekleyen verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]

### <a name="protect-data-in-flight"></a>Uçuş verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Depolama hesapları ile verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Döndürme ve ardından [depolama hesap anahtarlarınızı eşitleme](data-box-edge-manage-shares.md#sync-storage-keys) düzenli olarak, depolama hesabınıza yetkisiz kullanıcılara karşı korumak için.

## <a name="manage-personal-information"></a>Kişisel bilgilerini yönetme

Veri kutusu Edge hizmet aşağıdaki senaryolarda kişisel bilgilerinizi toplar:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Kimin erişebileceğini veya bir paylaşımı silmek kullanıcıların listesini görüntülemek için adımları izleyin. [yönetme veri kutusu uçta paylaşımları](data-box-edge-manage-shares.md).

Daha fazla bilgi için Microsoft gizlilik ilkesi gözden [Güven Merkezi](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Sonraki adımlar

[Veri kutusu Edge cihazınıza dağıtma](data-box-edge-deploy-prep.md)
