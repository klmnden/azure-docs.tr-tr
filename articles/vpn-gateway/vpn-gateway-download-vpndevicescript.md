---
title: 'S2S VPN bağlantıları için VPN cihazı yapılandırma betiklerini indirme: Azure Resource Manager | Microsoft Docs'
description: Bu makalede Azure Resource Manager kullanarak Azure VPN Gateways ile S2S VPN bağlantıları için VPN cihazı yapılandırma betiklerini indirirken size yol gösterir.
services: vpn-gateway
author: yushwang
manager: rossort
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/09/2019
ms.author: yushwang
ms.openlocfilehash: f7ee53c10c6597dbf98f8f85fc31fe789137471e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66157512"
---
# <a name="download-vpn-device-configuration-scripts-for-s2s-vpn-connections"></a>S2S VPN bağlantıları için VPN cihazı yapılandırma betiklerini indirme

Bu makalede Azure Resource Manager kullanarak Azure VPN Gateways ile S2S VPN bağlantıları için VPN cihazı yapılandırma betiklerini indirirken size yol gösterir. Üst düzey iş akışı aşağıdaki diyagramda gösterilmiştir.

![Yükleme betiği](./media/vpn-gateway-download-vpndevicescript/downloaddevicescript.png)

Aşağıdaki cihazlar kullanılabilir komut dosyalarınız varsa:

[!INCLUDE [scripts](../../includes/vpn-gateway-device-configuration-scripts.md)]

## <a name="about"></a>VPN cihazı yapılandırma betiklerini hakkında

Şirketler arası VPN bağlantısı, bir Azure VPN ağ geçidi, bir şirket içi VPN cihazı ve iki bağlanma IPSec S2S VPN tüneli oluşur. Tipik iş akışı, aşağıdaki adımları içerir:

1. Oluşturma ve bir Azure VPN ağ geçidi (sanal ağ geçidi) yapılandırma
2. Oluşturma ve şirket içi ağ ve VPN cihazını temsil eden bir Azure yerel ağ geçidi yapılandırma
3. Azure VPN ağ geçidi ve yerel ağ geçidi arasında bir Azure VPN bağlantısı oluşturup yapılandırabilirsiniz
4. Yerel ağ geçidi tarafından temsil edilen Azure VPN ağ geçidi ile gerçek S2S VPN tüneli oluşturmak için şirket içi VPN cihazı yapılandırma

1-3 Azure kullanarak adımları tamamlayabilirsiniz [portalı](vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md), veya [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md). Son adım, Azure dışındaki şirket içi VPN cihazları yapılandırma gerektirir. Bu özellik, karşılık gelen değerler Azure VPN ağ geçidi, sanal ağ ve şirket içi ağ adresi ön ekleri ve VPN bağlantı özelliklerini, zaten doldurulmuş vb. ile VPN cihazınız için bir yapılandırma betiğini indir izin verir. Betik bir başlangıç noktası olarak kullanın veya yapılandırma konsolu aracılığıyla şirket içi VPN cihazlarınız için doğrudan betiği uygulayın.

> [!IMPORTANT]
> * Farklı ve modelleri ve bellenim sürümleri bir bağımlılığa her VPN cihazı yapılandırma betiğini sözdizimi aşağıdaki gibidir. Özel cihaz model ve sürüm bilgilerini kullanılabilir şablonların karşı dikkat edin.
> * Bazı parametre değerleri cihazda benzersiz olması gerekir ve cihazın erişmeden belirlenemiyor. Azure tarafından oluşturulan yapılandırma komut dosyaları bu değerler önceden doldurun, ancak sağlanan değerlerle Cihazınızda geçerli olduğundan emin olmak gerekir. Örnekler için:
>    * Arabirim numaralarını
>    * Erişim denetim numaraları listesi
>    * İlke adları veya sayılar, vb.
> * For anahtar sözcüğü, Ara "**değiştirin**", katıştırılmış betik uygulamadan önce doğrulamak için gereken parametreler bulmak için komut.
> * Bazı şablonlar içeren bir "**Temizleme**" bölümünde yapılandırmaları kaldırmanızı uygulayabilirsiniz. Temizleme bölümlerde varsayılan olarak, varsayılan dışı bırakılır.

## <a name="download-the-configuration-script-from-azure-portal"></a>Azure Portalı'ndan yapılandırma betiğini indir

Bir Azure VPN ağ geçidi, yerel ağ geçidi ve iki bağlanan bir bağlantı kaynağı oluşturun. Şu sayfaya adımlarında size kılavuzluk eder:

* [Azure portalını kullanarak siteden siteye bağlantı oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)

Bağlantı kaynağı oluşturulduktan sonra VPN cihazı yapılandırma betiklerini indirmek için aşağıdaki yönergeleri izleyin:

1. Bir tarayıcıdan gidin [Azure portalında](https://portal.azure.com) ve gerekiyorsa Azure hesabınızla oturum açın
2. Oluşturduğunuz bağlantı kaynağa gidin. Tüm bağlantı kaynakların listesini tıklayarak "Tüm hizmetler", ardından "Ağ" ve "Bağlantıları" tarafından bulabilirsiniz

    ![bağlantı listesi](./media/vpn-gateway-download-vpndevicescript/connectionlist.png)

3. Yapılandırmak istediğiniz bağlantısında'e tıklayın.

    ![bağlantı-genel bakış](./media/vpn-gateway-download-vpndevicescript/connectionoverview.png)

4. Kırmızı bağlantı genel bakış sayfasında vurgulandığı gibi "yapılandırma indir" bağlantısına tıklayın; Bu, "Yükleme Yapılandırması" sayfası açılır.

    ![Yükleme betiği 1](./media/vpn-gateway-download-vpndevicescript/downloadscript-1.png)

5. VPN cihazınız için model ailesi ve üretici yazılımı sürümü seçin ve ardından "İndirme yapılandırması" düğmesine tıklayın.

    ![download66 komut 2](./media/vpn-gateway-download-vpndevicescript/downloadscript-2.PNG)

6. İndirilen komut dosyası (bir metin dosyası) tarayıcınızdan kaydetmeniz istenir.
7. Yapılandırma betiği indirdikten sonra değiştirilmesi gerekebilir parametreleri incelemek üzere bir metin düzenleyicisi ve "REPLACE" anahtar sözcüğü arama açın.

    ![betiği Düzenle](./media/vpn-gateway-download-vpndevicescript/editscript.png)

## <a name="download-the-configuration-script-using-azure-powershell"></a>Azure PowerShell kullanarak yapılandırma betiğini indir

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Ayrıca, aşağıdaki örnekte gösterildiği gibi Azure PowerShell kullanarak yapılandırma betiğini indirebilirsiniz:

```azurepowershell-interactive
$RG          = "TestRG1"
$GWName      = "VNet1GW"
$Connection  = "VNet1toSite1"

# List the available VPN device models and versions
Get-AzVirtualNetworkGatewaySupportedVpnDevice -Name $GWName -ResourceGroupName $RG

# Download the configuration script for the connection
Get-AzVirtualNetworkGatewayConnectionVpnDeviceConfigScript -Name $Connection -ResourceGroupName $RG -DeviceVendor Juniper -DeviceFamily Juniper_SRX_GA -FirmwareVersion Juniper_SRX_12.x_GA
```

## <a name="apply-the-configuration-script-to-your-vpn-device"></a>VPN Cihazınızı yapılandırma betiği Uygula

İndirilen ve doğrulanan yapılandırma betiğini sonra sonraki adım, VPN cihazınıza betik uygulamaktır. Gerçek yordamı, VPN cihaz marka ve model göre değişir. VPN cihazlarınız için işlem kılavuzlarına veya yönerge sayfaları başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırmaya devam etmek, [siteden siteye bağlantı](vpn-gateway-howto-site-to-site-resource-manager-portal.md).
