---
title: "S2S VPN bağlantıları için VPN cihazı yapılandırma komut dosyaları indirin: Azure Resource Manager | Microsoft Docs"
description: "Bu makalede Azure Resource Manager kullanarak Azure VPN Gateway'ler ile S2S VPN bağlantıları için VPN cihazı yapılandırma komut dosyaları indirme aracılığıyla anlatılmaktadır."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2018
ms.author: yushwang
ms.openlocfilehash: ebff881cdaa7dd3e14fa1687588408cd9a911553
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="download-vpn-device-configuration-scripts-for-s2s-vpn-connections"></a>S2S VPN bağlantıları için VPN cihazı yapılandırma komut dosyaları indirme

Bu makalede Azure Resource Manager kullanarak Azure VPN Gateway'ler ile S2S VPN bağlantıları için VPN cihazı yapılandırma komut dosyaları indirme aracılığıyla anlatılmaktadır. Aşağıdaki diyagramda, üst düzey iş akışı gösterilmektedir.

![yükleme komut dosyası](./media/vpn-gateway-download-vpndevicescript/downloaddevicescript.png)

## <a name="about"></a>VPN cihazı yapılandırma komut dosyaları hakkında

Bir Azure VPN ağ geçidi, bir şirket içi VPN cihazı ve IPSec S2S VPN tüneli iki bağlanan şirketler arası VPN bağlantısı oluşur. Genel iş akışını aşağıdaki adımları içerir:

1. Oluşturma ve bir Azure VPN ağ geçidi (sanal ağ geçidi) yapılandırma
2. Oluşturma ve şirket içi ağ ve VPN cihazı temsil eden bir Azure yerel ağ geçidi yapılandırma
3. Oluşturma ve Azure VPN ağ geçidi ve yerel ağ geçidi arasında bir Azure VPN bağlantısı yapılandırma
4. Azure VPN ağ geçidi ile gerçek S2S VPN tüneli oluşturmak için yerel ağ geçidi tarafından temsil edilen şirket içi VPN cihazı yapılandırma

1-3 Azure kullanarak adımları tamamlayabilirsiniz [portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md), veya [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md). Son adım, şirket içi VPN aygıtları Azure dışında yapılandırmayı içerir. Bu özellik, karşılık gelen değerler Azure VPN ağ geçidi, sanal ağ ve şirket içi ağ adres öneklerini ve VPN bağlantı özelliklerini, zaten doldurulmuş vb. ile VPN cihazınız için bir yapılandırma betiğini indir olanak sağlar. Bir başlangıç noktası olarak komut dosyası kullanarak veya komut dosyasını doğrudan şirket içi VPN cihazlarınızı yapılandırma konsolu aracılığıyla uygulanır.

> [!IMPORTANT]
> * Her VPN cihazı yapılandırma betiği sözdizimi, farklı ve yoğun olarak bağımlı modelleri ve bellenim sürümleri. Özel cihaz modeli ve sürüm bilgileri kullanılabilir şablonlar karşı dikkat edin.
> * Bazı parametre değerlerini cihazda benzersiz olması gerekir ve cihaz erişmeden belirlenemiyor. Azure tarafından oluşturulan yapılandırma komut dosyaları bu değerler önceden doldurun, ancak sağlanan değerler aygıtınızda geçerli olduğundan emin olmak gerekir. Örnekler için:
>    * Arabirim numaralarını
>    * Erişim denetim listesi numaraları
>    * İlke adları veya numaraları, vb.
> * For anahtar sözcüğü, Ara "**Değiştir**", katıştırılmış betik uygulamadan önce doğrulamak için gereken parametreleri bulmak için komut.
> * Bazı şablonlar eklemek bir "**Temizleme**" bölümü yapılandırmaları kaldırmak için geçerli olabilir. Temizleme bölümlerde varsayılan olarak, varsayılan dışı bırakılır.

## <a name="download-the-configuration-script-from-azure-portal"></a>Azure Portalı'ndan yapılandırma betiğini indir

Bir Azure VPN ağ geçidi, yerel ağ geçidi ve iki bağlanan bağlantı kaynağı oluşturun. Aşağıdaki sayfayı adımlarında size yol gösterir:

* [Azure portalında bir siteden siteye bağlantı oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)

Bağlantı kaynağı oluşturulduktan sonra VPN cihazı yapılandırma komut dosyaları indirmek için aşağıdaki yönergeleri izleyin:

1. Tarayıcıdan gidin [Azure portal](http://portal.azure.com) ve gerekiyorsa Azure hesabınızla oturum açın
2. Oluşturduğunuz bağlantı kaynağa gidin. "Tüm hizmetleri" tıklatarak sonra "Ağ" ve "Bağlantılar." tarafından tüm bağlantı kaynakları listesini bulabilirsiniz

    ![bağlantı listesi](./media/vpn-gateway-download-vpndevicescript/connectionlist.png)

3. Yapılandırmak istediğiniz bağlantıyı tıklatın.

    ![bağlantı genel bakış](./media/vpn-gateway-download-vpndevicescript/connectionoverview.png)

4. Vurgulanan kırmızı bağlantı genel bakış sayfasında "yapılandırma indirme" bağlantısını tıklatın; Bu "Yükleme Yapılandırması" sayfasını açar.

    ![yükleme komut dosyası 1](./media/vpn-gateway-download-vpndevicescript/downloadscript-1.png)

5. VPN cihazınız için model ailesi ve bellenim sürümü seçin ve "Yükleme Yapılandırması" düğmesine tıklayın.

    ![download66 betik 2](./media/vpn-gateway-download-vpndevicescript/downloadscript-2.PNG)

6. İndirilen komut dosyası (bir metin dosyası) tarayıcınızdan kaydetmeniz istenir.
7. Yapılandırma komut dosyası yüklendikten sonra tanımlamak ve değiştirilmesi gerekebilir parametreleri incelemek için bir metin Düzenleyicisi'ni ve anahtar sözcüğü "Değiştir" için arama açın.

    ![komut dosyası Düzenle](./media/vpn-gateway-download-vpndevicescript/editscript.png)

## <a name="download-the-configuration-script-using-azure-powershell"></a>Azure PowerShell kullanarak yapılandırma betiğini indir

Ayrıca, aşağıdaki örnekte gösterildiği gibi Azure PowerShell kullanarak yapılandırma komut dosyası karşıdan yükleyebilirsiniz:

```powershell
$Sub         = "<YourSubscriptionName>"
$RG          = "TestRG1"
$GWName      = "VNet1GW"
$Connection  = "VNet1toSite5"

Login-AzureRmAccount
Set-AzureRmContext -Subscription $Sub

# List the available VPN device models and versions
Get-AzureRmVirtualNetworkGatewaySupportedVpnDevice -Name $GWName -ResourceGroupName $RG

# Download the configuration script for the connection
Get-AzureRmVirtualNetworkGatewayConnectionVpnDeviceConfigScript -Name $Connection -ResourceGroupName $RG -DeviceVendor Juniper -DeviceFamily Juniper_SRX_GA -FirmwareVersion Juniper_SRX_12.x_GA
```

## <a name="apply-the-configuration-script-to-your-vpn-device"></a>VPN Cihazınızı yapılandırma komut dosyası Uygula

İndirilir ve yapılandırma komut dosyası doğrulanmış sonra sonraki adım komut dosyasını VPN Cihazınızı uygulamaktır. Gerçek yordamın VPN cihaz marka ve model göre değişir. İşlemi kılavuzları veya yönerge sayfalarını VPN cihazlarınızı başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
