---
title: Azure veri kutusu ağ geçidi güvenlik | Microsoft Docs
description: Koruma verileri Azure veri kutusu ağ geçidi sanal cihaz ve hizmet şirket içi güvenlik ve gizlilik özellikleri açıklar ve bulut.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 230d1a28ba15a8736e46c02cb08217a28fc18599
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60754357"
---
# <a name="azure-data-box-gateway-security-and-data-protection"></a>Azure veri kutusu ağ geçidi güvenlik ve veri koruması

Yeni bir Teknoloji benimseme özellikle teknolojisi ile gizli veya özel verileri kullandıysanız güvenlik büyük kaygısı andır. Varlıkları yalnızca yetkili emin olmanız azure veri kutusu ağ geçidi yardımcı görüntüleme, değiştirme veya verilerinizi silin.

Bu makalede her çözüm bileşenlerini ve bunlarda depolanan verileri koruyan Azure veri kutusu ağ geçidi güvenlik özellikleri açıklar.

Veri kutusu ağ geçidi çözüm birbiriyle etkileşim dört ana bileşen oluşur:

- **Veri kutusu ağ geçidi hizmeti, Azure üzerinde barındırılan**. Cihaz sipariş oluşturmak için kullandığınız yönetim kaynak cihazı yapılandırma ve sonra siparişin tamamlanana kadar izleyin.
- **Veri kutusu ağ geçidi cihazı**. Sağladığınız sistemi hiper yönetici içine sağlama sanal cihaz. Bu sanal cihazı şirket içi verilerinizi Azure'a aktarmak için kullanılır.
- **İstemciler/ana bilgisayarları bağlı cihaza**. Veri kutusu ağ geçidi cihazı için bağlayın ve korunması gereken verileri içeren istemcilerin altyapınızdaki.
- **Bulut depolama**. Azure bulut platformunda verilerin depolandığı konumu. Bu konum normalde, oluşturduğunuz veri kutusu ağ geçidi kaynağa bağlı depolama hesabıdır.


## <a name="data-box-gateway-service-protection"></a>Veri kutusu ağ geçidi hizmeti koruma

Azure'da barındırılan bir yönetim hizmeti veri kutusu ağ geçidi hizmetidir. Hizmet, yapılandırmak ve cihazı yönetmek için kullanılır.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-gateway-device-protection"></a>Veri kutusu ağ geçidi cihaz koruma

Veri kutusu ağ geçidi cihazı sağlayan bir şirket içi sisteminin hiper yöneticide sağlanan sanal bir cihazdır. Cihaz verileri Azure'a göndermeniz yardımcı olur. Cihazınız:

- Veri kutusu Edge/veri kutusu ağ geçidi hizmetine erişmek için bir etkinleştirme anahtarı gerekir.
- Her zaman bir cihaz parola korumalı.
<!---  secure boot enabled.
- Runs Windows Defender Device Guard. Device Guard allows you to run only trusted applications that you define in your code integrity policies.-->

### <a name="protect-the-device-via-activation-key"></a>Cihaz etkinleştirme anahtarı aracılığıyla koruma

Yetkili bir veri kutusu ağ geçidi cihaz yalnızca Azure aboneliğinizde oluşturduğunuz veri kutusu Ağ Geçidi Hizmeti'ne Katıl izin verilmez. Bir cihaz yetkilendirmek için veri kutusu ağ geçidi hizmeti cihazla etkinleştirmek için etkinleştirme anahtarı kullanmanız gerekir.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Daha fazla bilgi için [etkinleştirme anahtarı alma](data-box-gateway-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Cihaz parola aracılığıyla koruma

Parolalar, verilerinizi yalnızca yetkili kullanıcıların erişebildiğinden emin olun. Veri kutusu ağ geçidi cihaz önyüklemesi kilitli bir durumda.

Şunları yapabilirsiniz:

- Yerel web kullanıcı Arabirimi cihazın bir tarayıcı aracılığıyla bağlanın ve ardından cihaza oturum açmak için bir parola sağlayın.
- Uzaktan HTTP üzerinden cihazın PowerShell arabirimine bağlanın. Uzaktan Yönetim varsayılan olarak etkinleştirilir. Sonra cihaza oturum açmak için cihaz parolasının sunabilir. Daha fazla bilgi için [veri kutusu ağ geçidi Cihazınızı uzaktan bağlanma](data-box-gateway-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- Yerel web kullanıcı Arabirimine kullanım [parolayı değiştirmek](data-box-gateway-manage-access-power-connectivity-mode.md#manage-device-access). Oturum açmada sorun zorunda kalmazsınız tüm uzaktan erişim kullanıcılara bildirmek parolayı değiştirirseniz, unutmayın.


## <a name="protect-your-data"></a>Verilerinizi koruyun

Bu bölümde, aktarım sırasında ve depolanan verileri korumak veri kutusu ağ geçidi güvenlik özellikleri açıklanmaktadır.

### <a name="protect-data-at-rest"></a>Bekleyen verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]

### <a name="protect-data-in-flight"></a>Uçuş verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Depolama hesapları ile verileri koruma

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Döndürme ve ardından [depolama hesap anahtarlarınızı eşitleme](data-box-gateway-manage-shares.md#sync-storage-keys) düzenli olarak, depolama hesabınıza yetkisiz kullanıcılara karşı korumak için.

## <a name="manage-personal-information"></a>Kişisel bilgilerini yönetme

Veri kutusu ağ geçidi hizmeti, aşağıdaki senaryolarda kişisel bilgilerinizi toplar:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Kimin erişebileceğini veya bir paylaşımı silmek kullanıcıların listesini görüntülemek için adımları izleyin. [yönetme veri kutusu ağ geçidi paylaşımlarında](data-box-gateway-manage-shares.md).

Daha fazla bilgi için Microsoft gizlilik ilkesi gözden [Güven Merkezi](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Sonraki adımlar

[Veri kutusu ağ geçidi Cihazınızı dağıtma](data-box-gateway-deploy-prep.md)
