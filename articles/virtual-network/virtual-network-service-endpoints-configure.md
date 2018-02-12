---
title: "Azure sanal ağ hizmet uç noktalarını yapılandırma | Microsoft Docs"
description: "Hizmet uç noktalarını sanal ağınızdan etkinleştirmeyi ve devre dışı bırakmayı öğrenin"
services: virtual-network
documentationcenter: na
author: anithaa
manager: narayan
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2018
ms.author: anithaa
ms.custom: 
ms.openlocfilehash: e2242851d51dee56679231b9f34c8b474ba6578d
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="configure-virtual-network-service-endpoints"></a>Sanal Ağ Hizmet Uç Noktalarını Yapılandırma

Sanal Ağ hizmet uç noktaları, Azure hizmet kaynaklarını Azure Sanal Ağınızla sınırlandırmanızı ve bu kaynaklara İnternet erişimini tamamen kaldırmanızı sağlar. Hizmet uç noktaları sanal ağınızdan bir Azure hizmetine doğrudan bağlantı sunarak sanal ağınızın özel adres alanı aracılığıyla Azure hizmetlerine erişmenize imkan tanır. Hizmet uç noktaları aracılığıyla Azure hizmetlerine gönderilen trafik her zaman Microsoft Azure omurga ağı üzerinde kalır. ["Sanal ağ hizmeti uç noktaları"](virtual-network-service-endpoints-overview.md) hakkında daha fazla bilgi edinin

Bu makalede hizmet uç noktalarını etkinleştirme ve devre dışı bırakma adımları anlatılmaktadır. Bir Azure hizmetine giden alt ağda uç noktalar etkinleştirildikten sonra hizmet kaynaklarını bir sanal ağ ile sınırlandırabilirsiniz.

Hizmet uç noktaları [Azure portalı](#azure-portal), [Azure PowerShell](#azure-powershell), [Azure komut satırı arabirimi](#azure-cli) veya Azure Resource Manager [şablonuyla](#resource-manager-template) yapılandırılabilir.

>[!NOTE]
Önizleme sırasında sanal ağ hizmet uç noktaları özelliği belirli bölgelerde desteklenir. Desteklenen bölgelerin listesi için [Azure Sanal Ağ güncelleştirmeleri](https://azure.microsoft.com/updates/?product=virtual-network) sayfasına bakın.

## <a name="service-endpoint-configuration-overview"></a>Hizmet uç noktası yapılandırmasına genel bakış

- Hizmet uç noktaları yalnızca Azure Resource Manager dağıtım modeliyle dağıtılan sanal ağlarda yapılandırılabilir.

- Hizmet uç noktaları bir sanal ağın tüm alt ağlarında ayarlanır.

- Bir alt ağda bir hizmet için yalnızca bir hizmet uç noktası yapılandırabilirsiniz. Farklı hizmetler (Azure Storage, Azure SQL gibi) için birden fazla hizmet uç noktası yapılandırabilirsiniz.

- Uç noktaları yeni veya var olan bir alt ağ üzerinde etkinleştirebilirsiniz.

- Uç noktanın konumu otomatik olarak yapılandırılır. Hizmet uç noktalarının yapılandırmasında varsayılan olarak alt ağın bölgesi kullanılır. Azure Depolama'da bölgesel yük devretme senaryolarını desteklemek amacıyla uç noktalar otomatik olarak [Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions#what-are-paired-regions) ile yapılandırılır.

  >[!NOTE]
  Sanal ağın/alt ağın boyutuna bağlı olarak hizmet uç noktasının etkinleştirilme süresi uzun olabilir. Hizmet uç noktalarını etkinleştirirken devam eden kritik görev olmadığından emin olun. Hizmet uç noktaları alt ağınızdaki her NIC üzerinde rota değiştirir ve açık TCP bağlantılarını sonlandırabilir. 

- Alt ağdaki tüm NIC'lere giden trafik akışı sanal ağ özel IP adreslerini kullanmaya geçtiğinde hizmet uç noktası çağrısı "başarılı" sonucunu döndürür.

- Uç nokta yapılandırmasını doğrulamak için Geçerli Rotalar:

   Hizmet uç noktasının doğru şekilde yapılandırılıp yapılandırılmadığını doğrulamak için alt ağda bulunan herhangi bir NIC üzerindeki "geçerli rotalar" hizmet ve bölge başına nextHopType: VirtualNetworkServiceEndpoint ile yeni bir "varsayılan" rota gösterir. [Geçerli rotalarla sorun giderme](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal#using-effective-routes-to-troubleshoot-vm-traffic-flow) hakkında daha fazla bilgi edinin

   >[!NOTE]
   Geçerli rotalar yalnızca alt ağda yapılandırılmış ve çalışan bir sanal makineyle ilişkilendirilmiş bir veya daha fazla ağ arabirimi (NIC) varsa görüntülenebilir.

## <a name="azure-portal"></a>Azure Portal

### <a name="setting-up-service-endpoint-on-a-subnet-during-vnet-create"></a>Sanal ağ oluşturma sırasında alt ağ üzerinde hizmet uç noktası ayarlama

1. [Azure portalını](https://portal.azure.com/) açın.
Azure hesabınızı kullanarak Azure'da oturum açın. Azure hesabınız yoksa ücretsiz denemeye kaydolabilirsiniz. Hesap, sanal ağ ve hizmet uç noktası oluşturma [izinlerine](#provisioning) sahip olmalıdır.
2. +Yeni > Ağ > Sanal ağ > +Ekle'ye tıklayın.
3. "Sanal ağ oluştur" sayfasına aşağıdaki değerleri girin ve Oluştur'a tıklayın:

Ayar | Değer
------- | -----
Adı    | myVnet
Adres alanı | 10.0.0.0/16
Alt ağ adı|mySubnet
Alt ağ adres aralığı|10.0.0.0/24
Kaynak grubu|Yeni oluştur'u seçili bırakın ve bir ad girin.
Konum|Avustralya Doğu gibi desteklenen bir bölge seçin
Abonelik|Aboneliğinizi seçin.
__ServiceEndpoints__|Etkin
__Hizmetler__ | Kullanılabilir hizmetlerin birini veya tümünü seçin. Desteklenen hizmetler: __"Microsoft.Storage", "Microsoft.Sql"__.

Uç noktalar için hizmetleri seçin: ![Hizmet Uç Noktası Hizmetlerini Seçin](media/virtual-network-service-endpoints-portal/vnet-create-flow-services.png)

4. Tüm ayarların doğru olduğundan emin olun ve "Oluştur"a tıklayın.

![Hizmet uç noktası ayarlama](media/virtual-network-service-endpoints-portal/create-vnet-flow.png)

5. Azure hizmet kaynaklarını sanal ağınızla sınırlandırmayı tamamlamak için [Sonraki adımlar](#Next%20Steps) bölümünde yer alan hizmet belgelerine tıklayın.


### <a name="validating-service-endpoint-configuration"></a>Hizmet uç noktası yapılandırmasını doğrulama

Hizmet uç noktalarının aşağıdaki adımlarla yapılandırıldığını doğrulayın:

- Kaynaklar içinde "Sanal Ağlar"a tıklayın. Sanal ağı arayın.
- Sanal ağ adına tıklayın ve "Hizmet Uç Noktaları" bölümüne gidin
- Yapılandırılmış uç noktaları "Başarılı" şeklinde görünür. Otomatik olarak yapılandırılmış konumlar da görülebilir

![Hizmet Uç Noktası Yapılandırmasını Doğrulama](media/virtual-network-service-endpoints-portal/vnet-validate-settings.png
)

### <a name="effective-routes-to-validate-endpoint-configuration"></a>Uç nokta yapılandırmasını doğrulamak için geçerli rotalar

Geçerli rotayı alt ağ üzerindeki bir ağ arabiriminde (NIC) görüntülemek için alt ağdaki NIC'lerden birine tıklayın. "Destek + Sorun Giderme" bölümünde "Geçerli rotalar"a tıklayın. Uç noktası yapılandırıldıysa hedef olarak hizmetin adres ön eklerine sahip yeni bir "varsayılan" rota ve nextHopType için "VirtualNetworkServiceEndpoint" değerini görürsünüz.

![Hizmet uç noktaları için geçerli rotalar](media/virtual-network-service-endpoints-portal/effective-routes.png)

### <a name="setting-up-service-endpoints-for-existing-subnets-in-a-vnet"></a>Bir sanal ağdaki mevcut alt ağlar için hizmet uç noktalarını ayarlama

1. Kaynaklar sayfasında "Sanal ağlar"a tıklayın ve var olan sanal ağlardan birini arayın
2. Sanal ağ adına tıklayın ve "Hizmet uç noktaları" bölümüne gidin
3. "Ekle"ye tıklayın. "Hizmet"i seçin. Bir seferde yalnızca bir hizmet için uç nokta oluşturabilirsiniz. 
4. Uç noktayı uygulamak istediğiniz tüm alt ağları seçin. "Ekle"ye tıklayın

![Alt Ağ Hizmet Uç Noktası Yapılandırması](media/virtual-network-service-endpoints-portal/existing-subnet.png)

### <a name="deleting-service-endpoints"></a>Hizmet uç noktalarını silme
1. Kaynaklar içinde "Sanal Ağlar"a tıklayın. Sanal ağ adına göre filtreleme yaparak var olan sanal ağlardan birini arayın.
2. Sanal ağ adına tıklayın ve "Hizmet Uç Noktaları" bölümüne gidin
3. Hizmet adına tıklayın ve hizmet uç noktası girişine sağ tıklayın
4. "Sil"i seçin

![Hizmet Uç Noktasını Silme](media/virtual-network-service-endpoints-portal/delete-endpoint.png)

## <a name="azure-powershell"></a>Azure PowerShell

Kurulum ön koşulları:

- PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modülünün en son sürümünü yükleyin. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
- Bir PowerShell oturumu başlatmak için **Başlat**'a gidin, **powershell** yazın ve **PowerShell**'e tıklayın.
- PowerShell'de `login-azurermaccount` komutunu girerek Azure oturumu açın. Hesap, sanal ağ ve hizmet uç noktası oluşturma [izinlerine](#provisioning) sahip olmalıdır.

### <a name="get-available-service-endpoints-for-azure-region"></a>Azure bölgesi için kullanılabilir hizmet uç noktalarını alma

Bir Azure bölgesinde uç noktalar için desteklenen hizmet listesini almak üzere aşağıdaki komutu kullanın.
 ```powershell
Get-AzureRmVirtualNetworkAvailableEndpointService -location eastus
```

Çıktı: 
Adı | Kimlik | Tür
-----|----|-------
Microsoft.Storage|/subscriptions/xxxx-xxx-xxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Storage|Microsoft.Network/virtualNetworkEndpointServices
Microsoft.Sql|/subscriptions/xxxx-xxx-xxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Sql|Microsoft.Network/virtualNetworkEndpointServices

### <a name="add-azure-storage-service-endpoint-to-a-subnet-mysubnet-while-creating-the-virtual-network-myvnet"></a>Azure Depolama hizmet uç noktasını *mySubnet* adlı alt ağa ekleyin ve *myVNet* adlı sanal ağı oluşturun

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix "10.0.1.0/24" -ServiceEndpoint “Microsoft.Storage”

New-AzureRmVirtualNetwork -Name "myVNet" -AddressPrefix "10.0.0.0/16" -Subnet $subnet -ResourceGroupName "myRG" -Location "WestUS"
```
Virgülle ayrılmış hizmet adları listesini kullanarak birden fazla hizmeti etkinleştirebilirsiniz.
Örnek: "Microsoft.Storage", "Microsoft.Sql"

Beklenen Çıktı:
```
Subnets : [
            {
            "Name": "mySubnet",
             ...
            "ServiceEndpoints": [
              {
                   "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Storage",
                    "Locations": [
                        "westus",
                        "eastus"
                                 ]
               }
                                ],
            "ProvisioningState": "Succeeded"
            }
          ]
```

Azure hizmet kaynaklarını sanal ağınızla sınırlandırmayı tamamlamak için [Sonraki adımlar](#Next%20Steps) bölümünde yer alan hizmet belgelerine tıklayın.

### <a name="add-multiple-service-endpoints-to-an-existing-subnet"></a>Var olan alt ağa birden fazla hizmet uç noktası ekleme

```powershell
Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"  -AddressPrefix "10.0.1.0/24" -ServiceEndpoint "Microsoft.Storage", "Microsoft.Sql" | Set-AzureRmVirtualNetwork
```

Beklenen Çıktı: 
```
Subnets : [
            {
                "Name": "mySubnet",
                 ...
                "ServiceEndpoints": [
                {
                    "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Storage",
                    "Locations": [
                        "eastus",
                        "westus"
                                 ]
                },
                {
                    "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Sql",
                    "Locations": [
                        "eastus"
                                 ]
                }
                ],
               "ProvisioningState": "Succeeded"
            }
         ]
```

### <a name="view-service-endpoints-configured-on-a-subnet"></a>Bir alt ağ üzerinde yapılandırılmış hizmet uç noktalarını görüntüleme

```powershell
$subnet=Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"
$subnet.ServiceEndpoints
```
Çıktı:
```
ProvisioningState Service           Locations
----------------- -------           ---------
Succeeded         Microsoft.Storage {eastus, westus}
Succeeded         Microsoft.Sql     {eastus}
```

### <a name="delete-service-endpoints-on-a-subnet"></a>Bir alt ağ üzerindeki hizmet uç noktalarını silme
```powershell
Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"  -AddressPrefix "10.0.1.0/24" -ServiceEndpoint $null | Set-AzureRmVirtualNetwork
```

## <a name="azure-cli"></a>Azure CLI

Kurulum ön koşulları:
- [az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin. Oturum açma hakkında daha fazla bilgi için bkz. [Azure CLI 2.0'ı kullanmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).
 - Hesap, sanal ağ ve hizmet uç noktası oluşturma [izinlerine](#provisioning) sahip olmalıdır.

 Sanal ağlarla ilgili komutların tam listesi için bkz. [Azure CLI Sanal Ağ komutları](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest)

### <a name="get-available-service-endpoints-for-azure-region"></a>Azure bölgesi için kullanılabilir hizmet uç noktalarını alma

Bir Azure bölgesinde (örneğin "EastUS") uç noktalar için desteklenen hizmet listesini almak üzere aşağıdaki komutu kullanın.
```azure-cli
az network vnet list-endpoint-services -l eastus
```
Çıktı:
```
    {
    "id": "/subscriptions/xxxx-xxxx-xxxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Storage",
    "name": "Microsoft.Storage",
    "type": "Microsoft.Network/virtualNetworkEndpointServices"
     },
     {
     "id": "/subscriptions/xxxx-xxxx-xxxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Sql",
     "name": "Microsoft.Sql",
     "type":   "Microsoft.Network/virtualNetworkEndpointServices"
     }
```

### <a name="add-azure-storage-service-endpoint-to-a-subnet-mysubnet-while-creating-the-virtual-network-myvnet"></a>Azure Depolama hizmet uç noktasını *mySubnet* adlı alt ağa ekleyin ve *myVNet* adlı sanal ağı oluşturun

```azure-cli
az network vnet create -g myRG -n myVNet --address-prefixes 10.0.0.0/16 -l eastUS

az network vnet subnet create -g myRG -n mySubnet --vnet-name myVNet --address-prefix 10.0.1.0/24 --service-endpoints Microsoft.Storage
```

Birden fazla uç nokta eklemek için: --service-endpoints Microsoft.Storage Microsoft.Sql

Çıktı:
```
{
  "addressPrefix": "10.0.1.0/24",
  ...
  "name": "mySubnet",
  "networkSecurityGroup": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myRG",
  "resourceNavigationLinks": null,
  "routeTable": null,
  "serviceEndpoints": [
    {
      "locations": [
        "eastus",
        "westus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Storage"
    }
  ]
}
```

Azure hizmet kaynaklarını sanal ağınızla sınırlandırmayı tamamlamak için [Sonraki adımlar](#Next%20Steps) bölümünde yer alan hizmet belgelerine tıklayın.

### <a name="add-multiple-service-endpoints-to-an-existing-subnet"></a>Var olan alt ağa birden fazla hizmet uç noktası ekleme

```azure-cli
az network vnet subnet update -g myRG -n mySubnet2 --vnet-name myVNet --service-endpoints Microsoft.Storage Microsoft.Sql
```

Beklenen Çıktı:
```
{
  "addressPrefix": "10.0.2.0/24",
  ...
  "name": "mySubnet2",
  ...
  "serviceEndpoints": [
    {
      "locations": [
        "eastus",
        "westus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Storage"
    },
    {
      "locations": [
        "eastus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Sql"
    }
  ]
}
```

### <a name="view-service-endpoints-configured-on-a-subnet"></a>Bir alt ağ üzerinde yapılandırılmış hizmet uç noktalarını görüntüleme

```azure-cli
az network vnet subnet show -g myRG -n mySubnet --vnet-name myVNet
```

### <a name="delete-service-endpoints-on-a-subnet"></a>Bir alt ağ üzerindeki hizmet uç noktalarını silme
```azure-cli
az network vnet subnet update -g myRG -n mySubnet --vnet-name myVNet --service-endpoints ""
```

Çıktı: 
```
{
  "addressPrefix": "10.0.1.0/24",
  ...
  "name": "mySubnet",
  "networkSecurityGroup": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myRG",
  "resourceNavigationLinks": null,
  "routeTable": null,
  "serviceEndpoints": null
}
```

## <a name="resource-manager-template"></a>Resource Manager Şablonu

### <a name="securing-azure-service-resources-to-vnets"></a>Azure hizmet kaynaklarını sanal ağlar ile sınırlama

Belirli Azure kaynaklarını hizmet uç noktaları üzerinden sanal ağınızla sınırlayabilirsiniz.

Bir depolama hesabını bir sanal ağ içindeki alt ağ ile sınırlamak için örnek [Resource Manager şablonunu](https://azure.microsoft.com/resources/templates/201-vnet-2subnets-service-endpoints-storage-integration) indirin.

Şablon her birinde bir NIC'e sahip bir sanal makine bulunan 2 alt ağa sahip bir sanal ağ oluşturur. Bir alt ağdaki uç noktayı etkinleştirir ve bir depolama hesabını ilgili alt ağ ile sınırlar.

Şablonu indirebilir ve belirli bölümlerini değiştirerek senaryonuza uygun hale getirebilirsiniz.

Şablonla birlikte verilen talimatları kullanarak şablonu Azure portalı, PowerShell veya Azure CLI aracılığıyla dağıtabilirsiniz. Uç noktayı kurmak ve hesabı güvenli hale getirmek için gerekli [izinlere](#provisioning) sahip olduğunuzdan emin olun.

Azure kaynaklarını bir alt ağ ile sınırlamak için:

- İlgili alt ağda yapılandırılmış bir hizmet uç noktası bulunmalıdır.
- Kaynağın bir sanal ağ kuralı eklenerek sanal ağ ile sınırlanması gerekir.

### <a name="deleting-service-endpoints-with-resources-secured-to-the-subnet"></a>Alt ağ ile sınırlanmış kaynaklara sahip hizmet uç noktalarını silme
Azure hizmet kaynakları alt ağ ile sınırlanmış durumdaysa ve hizmet uç noktası silinirse kaynağa artık alt ağdan erişemezsiniz.
Uç noktanın tekrar etkinleştirilmesi tek başına önceden alt ağ ile sınırlanmış kaynağa erişimi geri getirmez.

Hizmet kaynağını bu alt ağ ile yeniden sınırlamak için şunları yapmanız gerekir:

- Uç noktayı yeniden etkinleştirin
- Kaynak üzerindeki eski sanal ağ kuralını kaldırın
- Kaynağı alt ağ ile sınırlayan yeni bir kural ekleyin

## <a name="provisioning"></a>Sağlama

Hizmet uç noktaları sanal ağda yazma erişimine sahip bir kullanıcı tarafından sanal ağlarda birbirinden bağımsız olarak yapılandırılabilir.

Azure hizmet kaynaklarını bir sanal ağ ile sınırlamak için kullanıcının eklenen alt ağlarda "Microsoft.Network/JoinServicetoaSubnet" iznine sahip olması gerekir. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.

[Yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ve [özel rollere](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) belirli izinlerin atanması hakkında daha fazla bilgi edinin.

Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. Bunların farklı aboneliklerde olması halinde kaynakların aynı Active Directory (AD) kiracısı altında bulunması gerekir.

## <a name="next-steps"></a>Sonraki Adımlar

Hizmet kaynağını alt ağlar ile sınırlama hakkında daha fazla talimat için aşağıdaki bağlantıları inceleyin:

[Azure Depolama hesaplarını sanal ağlarla sınırlama](https://docs.microsoft.com/azure/storage/common/storage-network-security)

[Azure SQL kaynaklarını sanal ağlarla sınırlama](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview)
