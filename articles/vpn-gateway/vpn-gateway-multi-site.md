---
title: 'Bir sanal ağ VPN Gateway ve PowerShell kullanarak birden çok siteye Bağlan: Klasik | Microsoft Docs'
description: Birden çok yerel şirket içi siteye bir VPN ağ geçidi kullanarak bir Klasik sanal ağa bağlayın.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-service-management
ms.assetid: b043df6e-f1e8-4a4d-8467-c06079e2c093
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: yushwang
ms.openlocfilehash: 77f8b7094c96e507eef1d360a26240627bc0e350
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60836095"
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection-classic"></a>Bir sanal ağa bir VPN ağ geçidi bağlantısı var (Klasik) ile siteden siteye bağlantı ekleme

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klasik)](vpn-gateway-multi-site.md)
>
>

Bu makalede PowerShell kullanarak siteden siteye (S2S) bağlantı var olan bir bağlantısı olan bir VPN ağ geçidi ekleme adımları gösterilmektedir. Bu bağlantı türü genellikle "çok siteli" yapılandırmayı olarak adlandırılır. Bu makaledeki adımlarda Klasik dağıtım modelini (Hizmet Yönetimi olarak da bilinir) kullanılarak oluşturulan sanal ağlar için geçerlidir. Bu adımlar, ExpressRoute/için siteden siteye bağlantı yapılandırmaları uygulanmaz.

### <a name="deployment-models-and-methods"></a>Dağıtım modelleri ve yöntemleri

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Bu tabloda yeni makaleler güncelleştiriyoruz ve bu yapılandırma için ek araçlar kullanılabilir. Bir makale kullanılabilir olduğunda, doğrudan bu tablodan bağlantısı verilir.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a>Bağlama hakkında

Birden çok şirket içi siteler tek bir sanal ağa bağlanabilir. Bu özellikle karma bulut çözümleri oluşturmak için idealdir. Azure sanal ağ geçidinizin çok siteli bağlantı oluşturma, diğer siteden siteye bağlantı oluşturma işlemiyle benzerdir. Aslında, ağ geçidini dinamik olduğu sürece, var olan bir Azure VPN ağ geçidi kullanabilirsiniz (rota tabanlı).

Sanal ağınıza bağlı statik ağ geçidi zaten varsa, ağ geçidi türünü dinamik olarak çok siteli uyum sağlamak için sanal ağ'ı yeniden dağıtmaya gerek kalmadan değiştirebilirsiniz. Yönlendirme türü değiştirmeden önce şirket içi VPN ağ geçidi rota tabanlı VPN yapılandırmaları desteklediğinden emin olun.

![çok siteli diyagramı](./media/vpn-gateway-multi-site/multisite.png "çok siteli")

## <a name="points-to-consider"></a>Dikkat edilmesi gereken noktalar

**Bu sanal ağa değişiklik yapmak için portalın kullanmanız mümkün olmayacaktır.** Portalı yerine ağ yapılandırma dosyasını değişiklikler yapmanız gerekir. Portalda değişiklik yaparsanız, bu sanal ağ için çok siteli başvuru ayarlarınızı şunun üzerine yazacağız.

Çok siteli yordamı tamamladınız zamanında ağ yapılandırma dosyasını kullanarak hissedene. Ancak, ağ yapılandırmanız üzerinde çalışan birden çok kişinin varsa, herkes bu sınırlama hakkında bilmesi emin olmanız gerekir. Bu, portal hiç kullanamazsınız gelmez. Bu belirli sanal ağa yapılandırma değişikliklerinden dışındaki diğer her şey için kullanabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdakilere sahip olduğunu doğrulayın:

* Her uyumlu VPN donanımına konumu şirket içi. Denetleme [sanal ağ bağlantısı için VPN cihazları hakkında](vpn-gateway-about-vpn-devices.md) kullanmak istiyorsanız cihazın uyumlu olarak bilinen bir sorun olup olmadığını doğrulayın.
* Dışarıya dönük genel IPv4 için bir IP adresi her VPN cihazı. IP adresi bir NAT'nin arkasında olamaz Bu gereksinimdir.
* Azure PowerShell cmdlet’lerinin en yeni sürümünü yüklemeniz gerekir. Resource Manager sürümü yanı sıra Hizmet Yönetimi (SM) sürümünü yüklediğinizden emin olun. Bkz: [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview) daha fazla bilgi için.
* Öğrenciyseniz VPN donanımınızın yapılandırma sırasında yeterli. VPN Cihazınızı yapılandırmak ya da vermez kişi ile çalışma konusunda güçlü bir anlayışa sahip olmanız.
* (Zaten yüklemediyseniz oluşturduysanız) sanal ağınız için kullanmak istediğiniz IP adresi aralığı.
* IP adresi aralıklarını her yerel ağ siteleri için bağlanmayı. IP adresi her çakışmadığından bağlanmak istediğiniz yerel ağ siteleri için aralıklarını emin olmanız gerekir. Aksi takdirde, portalı veya REST API'yi karşıya yüklenen yapılandırma reddeder.<br>Örneğin, IP adresi aralığı 10.2.3.0/24 içeren ve bir hedef adresi 10.2.3.3 paketiyle sahip iki yerel ağ siteleri varsa, Azure adres aralıklarını çakışan çünkü paketi göndermek istediğiniz hangi site bilmeniz mıydı. Yönlendirme sorunlarını önlemek için Azure, çakışan aralıklarını içeren bir yapılandırma dosyası karşıya izin vermez.

## <a name="1-create-a-site-to-site-vpn"></a>1. Siteden Siteye VPN oluşturma
Zaten bir dinamik yönlendirme ağ geçidi ile siteden siteye VPN harika varsa! Geçebilirsiniz [sanal ağ yapılandırma ayarlarını dışarı aktar](#export). Aksi durumda, aşağıdakileri yapın:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Zaten sahip olduğunuz bir siteden siteye sanal ağ, ancak bu statik (ilke tabanlı) yönlendirme ağ geçidi varsa:
1. Dinamik yönlendirme ağ geçidi türüne değiştirin. Çok siteli VPN dinamik (diğer adıyla rota tabanlı) yönlendirme ağ geçidi gerektirir. Ağ geçidi türünü değiştirmek için önce mevcut ağ geçidini silin ve ardından yeni bir tane oluşturmak gerekir.
2. Yeni ağ geçidi yapılandırması ve, VPN tüneli oluşturun. Yönergeler, yönergeler için bkz. [SKU ve VPN türünü belirtme](vpn-gateway-howto-site-to-site-classic-portal.md#sku). Yönlendirme türü 'Dynamic' olarak belirttiğinizden emin olun.

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Siteden siteye sanal ağ yoksa:
1. Bu yönergeleri kullanarak, siteden siteye sanal ağ oluşturun: [Sanal ağ ile bir siteden siteye VPN bağlantısı oluşturma](vpn-gateway-site-to-site-create.md).  
2. Bu yönergeleri kullanarak dinamik yönlendirme ağ geçidi yapılandırın: [Bir VPN ağ geçidi yapılandırma](vpn-gateway-configure-vpn-gateway-mp.md). Seçtiğinizden emin olun **dinamik yönlendirme** , ağ geçidi türü.

## <a name="export"></a>2. Ağ yapılandırma dosyasını dışarı aktar
Azure ağ yapılandırma dosyanız, aşağıdaki komutu çalıştırarak dışarı aktarın. Farklı bir konuma gerekirse dışa aktarılacak dosyanın konumunu değiştirebilirsiniz.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-the-network-configuration-file"></a>3. Ağ yapılandırma dosyasını açın
Son adımda yüklediğiniz ağ yapılandırma dosyasını açın. İstediğiniz herhangi bir xml düzenleyici kullanın. Dosya, aşağıdakine benzer görünmelidir:

        <NetworkConfiguration xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. Birden çok site başvurular ekleme
Site başvuru bilgilerini ekleyip, yapılandırma ConnectionsToLocalNetwork/LocalNetworkSiteRef değişiklik yapacaksınız. Yeni bir yerel site başvuru Tetikleyicileri yeni bir tünel oluşturmak için Azure ekleniyor. Aşağıdaki örnekte, tek sitede bağlantısı için ağ yapılandırmadır. Değişikliklerinizi yaptıktan sonra dosyayı kaydedin.

```xml
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

(Çok siteli bir yapılandırma oluşturun) ek site başvuruları eklemek için aşağıdaki örnekte gösterildiği gibi ek "LocalNetworkSiteRef" satırı eklemeniz yeterlidir:

```xml
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-the-network-configuration-file"></a>5. Ağ yapılandırma dosyasını içe aktarın
Ağ yapılandırma dosyasını içeri aktarın. Bu dosya değişikliği içeri aktardığınızda, yeni tüneller eklenir. Tüneller, daha önce oluşturduğunuz dinamik ağ geçidi kullanır. Dosyayı içe aktarmak için PowerShell kullanabilirsiniz.

## <a name="6-download-keys"></a>6. Anahtarlarını indir
Yeni, tünel ekledikten sonra her tünel için IPSec/IKE önceden paylaşılan anahtarları almak için 'Get-AzureVNetGatewayKey' PowerShell cmdlet'ini kullanın.

Örneğin:

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

Dilerseniz de kullanabilirsiniz *sanal ağ geçidi paylaşılan anahtar Al* önceden paylaşılan anahtarları almak için REST API.

## <a name="7-verify-your-connections"></a>7. Bağlantılarınızı doğrulayın
Çok siteli tünel durumunu kontrol edin. Her tünel anahtarlar'ı indirdikten sonra bağlantıları doğrulamak isteyebilirsiniz. Aşağıdaki örnekte gösterildiği gibi sanal ağ tüneller, listesini almak için 'Get-AzureVnetConnection' kullanın. Vnet1'e VNet adıdır.

```powershell
Get-AzureVnetConnection -VNetName VNET1
```

Örnek dönüş:

```
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
```

## <a name="next-steps"></a>Sonraki adımlar

VPN ağ geçitleri hakkında daha fazla bilgi için bkz. [VPN Gateways hakkında](vpn-gateway-about-vpngateways.md).
