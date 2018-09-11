---
ms.assetid: ''
title: Azure anahtar kasası güvenlik duvarları ve sanal ağları yapılandırma
description: Key Vault güvenlik duvarları ve sanal ağları yapılandırmak için adım adım yönergeler
services: key-vault
author: amitbapat
manager: mbaldwin
ms.service: key-vault
ms.topic: conceptual
ms.workload: identity
ms.date: 08/31/2018
ms.author: ambapat
ms.openlocfilehash: 6315434c1e8acc82e02f5c9e5ae8ab2d1cacc887
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302062"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Azure anahtar kasası güvenlik duvarları ve sanal ağları yapılandırma

Bu kılavuzda, Key Vault güvenlik duvarları ve anahtar kasanız için erişimi kısıtlamak için sanal ağları yapılandırmak için adım adım yönergeler açıklanmaktadır. [Key Vault için sanal ağ hizmet uç noktaları](key-vault-overview-vnet-service-endpoints.md) belirtilen sanal ağ ve/veya bir dizi IPv4 (Internet Protokolü sürüm 4) adres aralıkları için anahtar kasası erişimi sınırlamak sağlar.

> [!IMPORTANT]
> Güvenlik Duvarı ve sanal ağ kuralları aslında tüm anahtar kasası sonra [veri düzlemi](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) işlemleri, yalnızca izin verilen sanal ağlara ya da IPv4 adres aralıklarını çağıranın isteği oluşturulduğunda gerçekleştirilebilir. Bu ayrıca, Azure Portalı'ndan anahtar kasasına erişmek için geçerlidir. Bir kullanıcının tarayıcı bir anahtar kasasına Azure portalından, ancak kendi istemci makine izin verilenler listesinde değilse, bunlar liste anahtarlar/gizli dizileri/sertifikaları mümkün olmayabilir. Bu ayrıca diğer Azure Hizmetleri tarafından 'Anahtar kasası Seçici' etkiler. Kullanıcılar anahtar kasalarının listesi bakın, ancak güvenlik duvarı kurallarını istemci makine engelliyorsa anahtarları listesinde değil.

## <a name="azure-portal"></a>Azure portal

1. Korumak istediğiniz bir anahtar kasasına gidin.
2. Tıklayarak **güvenlik duvarları ve sanal ağlar**.
3. Tıklayarak **seçili ağlar** altında **erişime izin verecek**.
4. Var olan sanal ağları güvenlik duvarları ve sanal ağ kuralları eklemek için tıklatın **+ var olan sanal ağları Ekle**.
5. Açılan yeni dikey pencerede, abonelik, sanal ağlar ve bu anahtar kasasına erişim vermek istediğiniz alt ağı, başarılı seçin. Sanal ağlar ve alt ağı, başarılı seçeneğini belirlerseniz, sahip değilse hizmet uç noktaları etkinleştirilmiş iletisini görürsünüz, "aşağıdaki ağ hizmet uç noktaları... enavled yok". Tıklayın **etkinleştirme** listelenen için hizmet uç noktalarını etkinleştirmek istediğinizi onayladıktan sonra sanal ağlar ve alt ağı, başarılı. Bu, etkili olması 15 dakika kadar sürebilir.
6. Ayrıca, yeni sanal ağlar ve alt ağı, başarılı ekleyin ve ardından tıklayarak alt ağı, başarılı ve yeni oluşturulan sanal ağlar için hizmet uç noktaları etkinleştirin **+ yeni sanal ağ Ekle** ve istemleri izleyerek.
7. Altında **IP ağları**, IPv4 adres aralıklarını yazarak IPv4 adres aralıklarını ekleyebilirsiniz [(sınıfsız etki alanları arası yönlendirme) CIDR gösteriminde](https://tools.ietf.org/html/rfc4632) veya tek IP adresleri.
8. **Kaydet**’e tıklayın.

## <a name="azure-cli-20"></a>Azure CLI 2.0

1. [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) ve [oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

2. Bu anahtar kasası için herhangi bir kuralın ayarlamadıysanız, listesinde kullanılabilir bir sanal ağ kuralları, listesi boş olur.
```azurecli
az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
```

3. Key Vault var olan sanal ağı ve alt ağ için hizmet uç noktasını girin
```azurecli
az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
```

4. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
```

5. Gelen trafiğe izin vermek için IP adres aralığı Ekle
```azurecli
az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
```

6. Bu anahtar kasası güvenilir hizmetlerin tarafından erişilebilir olması gerekiyorsa 'atlama için AzureServices ayarlayın
```azurecli
az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
```

7. Artık son ve en önemli adım, ağ üzerinde 'Reddet' varsayılan eylemini ayarlayarak kurallarını etkinleştirin
```azurecli
az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
```

## <a name="azure-powershell"></a>Azure PowerShell

1. Son yükleme [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) ve [oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

2. Bu anahtar kasası için herhangi bir kuralın ayarlamadıysanız, listesinde kullanılabilir bir sanal ağ kuralları, listesi boş olur.
```PowerShell
(Get-AzureRmKeyVault -VaultName "mykeyvault").NetworkAcls
```

3. Key Vault var olan sanal ağı ve alt ağ için hizmet uç noktasını girin
```PowerShell
Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzureRmVirtualNetwork
```

4. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
```

5. Gelen trafiğe izin vermek için IP adres aralığı Ekle
```PowerShell
Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
```

6. Bu anahtar kasası güvenilir hizmetlerin tarafından erişilebilir olması gerekiyorsa 'atlama için AzureServices ayarlayın
```PowerShell
Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
```

7. Artık son ve en önemli adım, ağ üzerinde 'Reddet' varsayılan eylemini ayarlayarak kurallarını etkinleştirin
```PowerShell
Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
```

## <a name="references"></a>Başvurular

* Azure CLI 2.0 komutlarını - [az keyvault ağ-rule](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Azure PowerShell cmdlet'leri - [Get-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurermkeyvault), [Ekle AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureRmKeyVaultNetworkRule), [Remove-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureRmKeyVaultNetworkRule), [ Güncelleştirme AzureRmKeyVaultNetworkRuleSet](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureRmKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Sonraki adımlar

* [Sanal ağ hizmet uç noktaları için anahtar kasası](key-vault-overview-vnet-service-endpoints.md)
* [Anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md)