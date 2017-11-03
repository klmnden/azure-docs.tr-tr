---
title: "Bir sanal ağ VPN ağ geçidi ve PowerShell kullanarak birden çok siteye Bağlan: Klasik | Microsoft Docs"
description: "Bu makalede Klasik dağıtım modeli için bir VPN ağ geçidi kullanarak bir sanal ağ için birden çok yerel şirket içi siteler konusunda size yol gösterir."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-service-management
ms.assetid: b043df6e-f1e8-4a4d-8467-c06079e2c093
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: yushwang
ms.openlocfilehash: 434f84dc6244eddce9b172a617722b218360ffc2
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection-classic"></a>Mevcut bir VPN ağ geçidi bağlantısı olan (Klasik) bir sanal ağa bir siteden siteye bağlantı Ekle

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klasik)](vpn-gateway-multi-site.md)
>
>

Bu makalede PowerShell kullanarak siteden siteye (S2S) bağlantıları varolan bir bağlantıyı sahip bir VPN ağ geçidi eklemek aracılığıyla anlatılmaktadır. Bu bağlantı türü genellikle "çok siteli" yapılandırma olarak adlandırılır. Bu makaledeki adımları (Hizmet Yönetimi olarak da bilinir) Klasik dağıtım modeli kullanılarak oluşturulmuş sanal ağlar için geçerlidir. Bu adımları ExpressRoute /-siteye eşzamanlı bağlantı yapılandırmaları için geçerli değildir.

### <a name="deployment-models-and-methods"></a>Dağıtım modelleri ve yöntemleri

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Bu tabloda yeni makaleler olarak güncelleştiriyoruz ve ek araçlar bu yapılandırma için kullanılabilir hale gelir. Bir makale kullanılabilir olduğunda sizi doğrudan bu tablodan bağlayın.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a>Bağlama hakkında

Birden çok şirket içi siteler tek bir sanal ağa bağlanabilir. Karma bulut çözümleri oluşturmak için özellikle çekici budur. Azure sanal ağ geçidiniz için çok siteli bağlantı oluşturma, diğer siteden siteye bağlantılar oluşturmak için benzer. Aslında, ağ geçidini dinamik olduğu sürece, mevcut bir Azure VPN ağ geçidi kullanabilirsiniz (rota tabanlı).

Sanal ağınıza bağlı bir statik ağ geçidi zaten varsa, ağ geçidi türü için dinamik çok siteli karşılamak üzere sanal ağ yeniden oluşturmak zorunda kalmadan değiştirebilirsiniz. Yönlendirme türü değiştirmeden önce şirket içi VPN ağ geçidinizi rota tabanlı VPN yapılandırmaları desteklediğinden emin olun.

![çok siteli diyagramı](./media/vpn-gateway-multi-site/multisite.png "çok siteli")

## <a name="points-to-consider"></a>Dikkate alınacak noktalar

**Bu sanal ağa değişiklik yapmak için portal kullanmanız mümkün olmayacaktır.** Portal kullanmak yerine ağ yapılandırma dosyasının değişiklikler yapmanız gerekir. Portalda değişiklik yaparsanız, bu sanal ağ için çok siteli başvuru ayarlarınızı üzerine.

Çok siteli yordamı tamamladıktan zamanına göre ağ yapılandırma dosyası kullanarak rahat geliyor olmalıdır. Ancak, ağ yapılandırmanızı üzerinde çalışan birden çok kişi varsa, herkes bu sınırlama hakkında bilir emin olmak gerekir. Bu, portal hiç kullanamazsınız anlamına gelmez. Bu belirli sanal ağa yapılandırma değişikliklerinden dışında başka her şey için kullanabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdakilere sahip olduğunu doğrulayın:

* Uyumlu VPN donanım her konum şirket içi. Denetleme [sanal ağ bağlantısı için VPN aygıtları hakkında](vpn-gateway-about-vpn-devices.md) kullanmak istediğiniz cihaz uyumlu olması için bilinen bir şey olup olmadığını doğrulayın.
* Bir dışarıya dönük IPv4 için genel IP adresi her VPN cihazı. IP adresi bir NAT'nin arkasında olamaz Bu gerekli değildir.
* Azure PowerShell cmdlet’lerinin en yeni sürümünü yüklemeniz gerekir. Resource Manager sürüm yanı sıra Hizmet Yönetimi (SM) sürümü yüklediğinizden emin olun. Bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) daha fazla bilgi için.
* Birisinden VPN donanımınızın yapılandırma sırasında yeterli. Mu birisiyle çalışmak veya VPN Cihazınızı yapılandırma güçlü bir anlamış olmanız gerekir.
* (, Zaten bir oluşturmadıysanız) sanal ağınız için kullanmak istediğiniz IP adresi aralığı.
* IP adresi aralıkları için bağlanma yerel ağ sitelerinin her biri için. IP adresi her çakışmaması bağlanmak istediğiniz yerel ağ sitesi için aralıkları emin olmak gerekir. Aksi takdirde, portalı veya REST API'yi karşıya yüklenen yapılandırma reddeder.<br>Örneğin, her ikisi de IP adresi aralığı 10.2.3.0/24 içerir ve hedef adres 10.2.3.3 ile bir pakete sahip iki yerel ağ sitesi varsa, Azure adres aralıklarını çakışan çünkü paketi göndermek istediğiniz hangi site bildiğiniz olmayacaktır. Yönlendirme sorunlarını önlemek için Azure örtüşen aralıklarına sahip bir yapılandırma dosyası karşıya izin vermiyor.

## <a name="1-create-a-site-to-site-vpn"></a>1. Siteden Siteye VPN oluşturma
Zaten bir dinamik yönlendirme ağ geçidi ile siteden siteye VPN harika varsa! Geçebilirsiniz [sanal ağ yapılandırma ayarlarını dışarı aktar](#export). Aksi halde, aşağıdakileri yapın:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Siteden siteye sanal ağ zaten var, ancak statik (ilke tabanlı) yönlendirme ağ geçidi varsa:
1. Dinamik yönlendirme için ağ geçidi türünü değiştirin. Çok siteli VPN dinamik (olarak da bilinen rota tabanlı) yönlendirme ağ geçidi gerektirir. Ağ geçidi türünü değiştirmek için önce mevcut ağ geçidini silin, sonra yeni bir tane oluşturmak gerekir.
2. Yeni ağ geçidiniz yapılandırın ve VPN tüneli oluşturur. Yönergeler için yönergeler için bkz: [SKU ve VPN türünü belirtin](vpn-gateway-howto-site-to-site-classic-portal.md#sku). Yönlendirme türü 'Dinamik' olarak belirttiğinizden emin olun.

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Siteden siteye sanal ağınız yoksa:
1. Bu yönergeleri kullanarak, siteden siteye sanal ağ oluşturma: [bir sanal ağ ile bir siteden siteye VPN bağlantısı oluşturma](vpn-gateway-site-to-site-create.md).  
2. Bu yönergeleri kullanarak dinamik yönlendirme ağ geçidi yapılandırma: [bir VPN ağ geçidi yapılandırma](vpn-gateway-configure-vpn-gateway-mp.md). Seçtiğinizden emin olun **dinamik yönlendirme** , ağ geçidi türü.

## <a name="export"></a>2. Ağ yapılandırma dosyasını dışarı aktarma
Azure ağı yapılandırma dosyanızda aşağıdaki komutu çalıştırarak dışa aktarın. Farklı bir konum gerekirse dışa aktarılacak dosya konumunu değiştirebilirsiniz.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-the-network-configuration-file"></a>3. Ağ yapılandırma dosyasını açın
Son adımda indirdiğiniz ağ yapılandırma dosyasını açın. İstediğiniz herhangi bir xml düzenleyicisi kullanın. Dosya aşağıdakine benzer görünmelidir:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
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

## <a name="4-add-multiple-site-references"></a>4. Birden çok sitesi başvuruları ekleme
Site başvuru bilgileri ekleyip, yapılandırma değişikliklerini ConnectionsToLocalNetwork/LocalNetworkSiteRef hale getireceğiz. Yeni bir yerel site başvuru Tetikleyiciler yeni bir tünel oluşturmak için Azure ekleniyor. Aşağıdaki örnekte, tek siteli bağlantı için ağ yapılandırmadır. Yaptığınız değişiklikleri yaptıktan sonra dosyayı kaydedin.

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

Ek sitesi başvuruları (çok siteli yapılandırma Oluştur) eklemek için aşağıdaki örnekte gösterildiği gibi ek "LocalNetworkSiteRef" satırı eklemeniz yeterlidir:

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-the-network-configuration-file"></a>5. Ağ yapılandırma dosyasını içeri aktar
Ağ yapılandırma dosyasını içeri aktarın. Bu dosya değişiklikleri içe aktardığınızda, yeni tüneller eklenir. Tünelleri, daha önce oluşturduğunuz dinamik ağ geçidi kullanır. Dosyayı içe aktarmak için PowerShell kullanın.

## <a name="6-download-keys"></a>6. Anahtarları indirin
Yeni tüneller eklendikten sonra her tüneli için IPSec/IKE önceden paylaşılan anahtarı almak için 'Get-AzureVNetGatewayKey' PowerShell cmdlet'ini kullanın.

Örneğin:

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

İsterseniz, ayrıca kullanabileceğiniz *alma sanal ağ geçidi paylaşılan anahtarı* önceden paylaşılan anahtarı almak için REST API.

## <a name="7-verify-your-connections"></a>7. Bağlantılarınızı doğrulayın
Çok siteli tünel durumunu kontrol edin. Her tünel tuşları indirdikten sonra bağlantıları doğrulamak istersiniz. Aşağıdaki örnekte gösterildiği gibi sanal ağ tüneller listesini almak için 'Get-AzureVnetConnection' kullanın. VNet1 VNet adıdır.

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

VPN ağ geçitleri hakkında daha fazla bilgi için bkz: [VPN ağ geçitleri hakkında](vpn-gateway-about-vpngateways.md).
