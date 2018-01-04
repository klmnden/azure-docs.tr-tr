---
title: "Bir sanal ağ geçidini silmek: PowerShell: Azure Klasik | Microsoft Docs"
description: "Klasik dağıtım modelinde PowerShell kullanarak bir sanal ağ geçidini silin."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: cherylmc
ms.openlocfilehash: b1bc18307227a728e2bc8fd95e30fdc1cbdb8c59
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="delete-a-virtual-network-gateway-using-powershell-classic"></a>PowerShell (Klasik) kullanarak bir sanal ağ geçidini Sil
> [!div class="op_single_selector"]
> * [Resource Manager - Azure portalı](vpn-gateway-delete-vnet-gateway-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [Klasik - PowerShell](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

Bu makalede PowerShell kullanarak bir VPN ağ geçidi Klasik dağıtım modelinde silmenize yardımcı olur. Sanal ağ geçidi silindikten sonra artık kullanmadığınız öğeleri kaldırmak için ağ yapılandırma dosyasını değiştirin.

##<a name="connect"></a>1. adım: Azure'a bağlanma

### <a name="1-install-the-latest-powershell-cmdlets"></a>1. En son PowerShell cmdlet'lerini yükleyin.

Azure Hizmet Yönetimi (SM) PowerShell cmdlet'lerinin en son sürümünü yükleyip yeniden açın. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

### <a name="2-connect-to-your-azure-account"></a>2. Azure hesabınıza bağlanın. 

PowerShell konsolunuzu yükseltilmiş haklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Add-AzureAccount
```

## <a name="export"></a>2. adım: Dışarı aktarma ve ağ yapılandırma dosyasını görüntüleyin

Bilgisayarınızda bir dizin oluşturun ve sonra ağ yapılandırma dosyasını dizine aktarın. Bu dosya geçerli yapılandırma bilgilerini hem görünümüne ve ağ yapılandırmasını değiştirmek için kullanın.

Bu örnekte, ağ yapılandırma dosyası C:\AzureNet dizinine aktarılır.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Dosyayı bir metin düzenleyicisiyle açın ve klasik sanal ağınızı adını görüntüleyin. Azure portalında VNet oluşturduğunuzda, Azure kullandığı tam adı portalda görünür değil. Örneğin, 'ClassicVNet1' Azure portalında adlandırılması görünür bir VNet olabilir kadar uzun ad ağ yapılandırma dosyasında. Adı şöyle görünebilir: 'Grup ClassicRG1 ClassicVNet1'. Sanal ağ adları olarak listelenen **' VirtualNetworkSite adı ='**. Adları PowerShell cmdlet'lerini çalıştırırken ağ yapılandırma dosyası kullanın.

## <a name="delete"></a>3. adım: sanal ağ geçidini Sil

Bir sanal ağ geçidini sildiğinizde, ağ geçidi üzerinden vnet'e tüm bağlantılar kesilir. Vnet'e bağlı P2S istemcileriniz varsa, uyarı olmadan kesilir.

Bu örnek, sanal ağ geçidini siler. Ağ yapılandırma dosyasından sanal ağın tam adı kullandığınızdan emin olun.

```powershell
Remove-AzureVNetGateway -VNetName "Group ClassicRG1 ClassicVNet1"
```

Başarılı olursa, dönüş gösterir:

```
Status : Successful
```

## <a name="modify"></a>4. adım: ağ yapılandırma dosyasını değiştirme

Bir sanal ağ geçidini sildiğinizde, cmdlet ağ yapılandırma dosyası değiştirmez. Artık kullanılmayan öğeleri kaldırmak için dosyasını değiştirmeniz gerekir. Aşağıdaki bölümlerde indirdiğiniz ağ yapılandırma dosyasını değiştirme yardımcı olur.

### <a name="lnsref"></a>Yerel ağ sitesi başvuruları

Site başvuru bilgileri kaldırmak için yapılandırma değişiklikleri yapmanız **ConnectionsToLocalNetwork/LocalNetworkSiteRef**. Bir yerel site başvuru Tetikleyicileri tünel silmek için Azure kaldırılıyor. Oluşturduğunuz yapılandırmasına bağlı olarak, olmayabilir bir **LocalNetworkSiteRef** listelenir.

```
<Gateway>
   <ConnectionsToLocalNetwork>
     <LocalNetworkSiteRef name="D1BFC9CB_Site2">
       <Connection type="IPsec" />
     </LocalNetworkSiteRef>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

Örnek:

```
<Gateway>
   <ConnectionsToLocalNetwork>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

###<a name="lns"></a>Yerel ağ sitesi

Artık kullanmadığınız yerel siteleri kaldırın. Oluşturduğunuz yapılandırmasına bağlı olarak, yoksa olası bir **LocalNetworkSite** listelenir.

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
  <LocalNetworkSite name="Site3">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>57.179.18.164</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

Bu örnekte, yalnızca Site3 kaldırıldı.

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

### <a name="clientaddresss"></a>İstemci adres havuzu

P2S bağlantısı Vnet'iniz olsaydı olacaktır bir **VPNClientAddressPool**. Sildiğiniz sanal ağ geçidi karşılık gelen istemci adres havuzları kaldırın.

```
<Gateway>
    <VPNClientAddressPool>
      <AddressPrefix>10.1.0.0/24</AddressPrefix>
    </VPNClientAddressPool>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

Örnek:

```
<Gateway>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

### <a name="gwsub"></a>GatewaySubnet

Silme **GatewaySubnet** Vnet'e karşılık gelir.

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
   <Subnet name="GatewaySubnet">
     <AddressPrefix>10.11.1.0/29</AddressPrefix>
   </Subnet>
 </Subnets>
```

Örnek:

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
 </Subnets>
```

## <a name="upload"></a>5. adım: ağ yapılandırma dosyasını karşıya yükle

Yaptığınız değişiklikleri kaydedin ve ağ yapılandırma dosyasını Azure'a yükleyin. Ortamınız için gerektiği gibi dosya yolu değiştirdiğinizden emin olun.

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Başarılı olursa, dönüş bu örneğe benzer bir şey gösterir:

```
OperationDescription        OperationId                      OperationStatus                                                
--------------------        -----------                      ---------------                                           
Set-AzureVNetConfig         e0ee6e66-9167-cfa7-a746-7casb9   Succeeded