---
title: Azure Key Vault güvenlik duvarları ve sanal ağları - Azure Key Vault yapılandırma
description: Key Vault güvenlik duvarları ve sanal ağları yapılandırmak için adım adım yönergeler
services: key-vault
author: amitbapat
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: ambapat
ms.openlocfilehash: a6f2e899e8be39abdefaf9d4f524eae457673c1a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64694415"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Azure Key Vault güvenlik duvarları ve sanal ağları yapılandırma

Bu makalede, Azure Key Vault güvenlik duvarları ve anahtar kasanız için erişimi kısıtlamak için sanal ağları yapılandırmak için adım adım yönergeler sağlar. [Sanal ağ hizmet uç noktaları için Key Vault](key-vault-overview-vnet-service-endpoints.md) , belirtilen sanal ağ ve IPv4 (internet Protokolü sürüm 4) adres aralıkları kümesini erişimini sınırlamanıza olanak tanır.

> [!IMPORTANT]
> Güvenlik duvarı kuralları etkin olduktan sonra kullanıcılar yalnızca Key Vault gerçekleştirebilir [veri düzlemi](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) isteklerinde kaynaklanan olduğunda işlemlerine izin sanal ağlara veya IPv4 adres aralıklarını. Bu, aynı zamanda Key Vault Azure portalından erişmek için geçerlidir. Kullanıcılar, Azure portalından bir anahtar Kasası'na göz atabilir olsa da, kendi istemci makine izin verilenler listesinde değilse, bunlar liste anahtarlarını, gizli dizileri veya sertifikaları mümkün olmayabilir. Bu anahtar kasası Seçici tarafından diğer Azure hizmetleri de etkiler. Kullanıcılar anahtar kasalarının listesi bakın, ancak güvenlik duvarı kurallarını istemci makine engelliyorsa anahtarları listesi değil mümkün olabilir.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Azure portalını kullanarak Key Vault güvenlik duvarları ve sanal ağları yapılandırma aşağıda verilmiştir:

1. Korumak istediğiniz bir anahtar kasasına göz atın.
2. Seçin **güvenlik duvarları ve sanal ağlar**.
3. Altında **erişime izin verecek**seçin **seçili ağlar**.
4. Var olan sanal ağları güvenlik duvarları ve sanal ağ kuralları eklemek için seçin **+ var olan sanal ağları Ekle**.
5. Açılan yeni dikey pencerede, abonelik, sanal ağlar ve bu anahtar kasasına erişim vermek istediğiniz alt ağları seçin. Seçtiğiniz alt ağlar ve sanal ağ hizmet uç noktaları etkinleştirilmiş yoksa, hizmet uç noktalarının etkinleştirilmesi ve istediğinizi onaylayın **etkinleştirme**. İşlem etkili olması 15 dakika kadar sürebilir.
6. Altında **IP ağları**, IPv4 adres aralıklarını yazarak IPv4 adres aralıklarını ekleyin [(sınıfsız etki alanları arası yönlendirme) CIDR gösteriminde](https://tools.ietf.org/html/rfc4632) veya tek IP adresleri.
7. **Kaydet**’i seçin.

Ayrıca yeni sanal ağlar ve alt ağlar ekleyin ve ardından yeni oluşturulan sanal ağlar ve alt ağlar için hizmet uç noktası seçerek etkinleştirin **+ yeni sanal ağ Ekle**. Ardından yönergeleri izleyin.

## <a name="use-the-azure-cli"></a>Azure CLI kullanma 

İşte Azure CLI'yi kullanarak Key Vault güvenlik duvarları ve sanal ağları yapılandırma

1. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) ve [oturum](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

2. Kullanılabilir sanal ağ kuralları listesi. Bu anahtar kasası için herhangi bir kuralın ayarlamadıysanız listesi boş olur.
   ```azurecli
   az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
   ```

3. Hizmet uç noktası, bir var olan sanal ağı ve alt ağ üzerinde anahtar kasası için etkinleştirin.
   ```azurecli
   az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
   ```

4. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin.
   ```azurecli
   subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
   az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
   ```

5. Bir IP adresi aralığı, trafiğe izin verecek şekilde ekleyin.
   ```azurecli
   az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
   ```

6. Bu anahtar kasası güvenilir hizmetlerin tarafından erişilebilir olması gereken verilirse `bypass` için `AzureServices`.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
   ```

7. Varsayılan eylem ayarlayarak ağ kurallarını açma `Deny`.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
   ```

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell kullanarak Key Vault güvenlik duvarları ve sanal ağları yapılandırma aşağıda verilmiştir:

1. Son yükleme [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps), ve [oturum](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

2. Kullanılabilir sanal ağ kuralları listesi. Bu anahtar kasası için herhangi bir kuralın ayarlamadıysanız listesi boş olur.
   ```powershell
   (Get-AzKeyVault -VaultName "mykeyvault").NetworkAcls
   ```

3. Hizmet uç noktası, bir var olan sanal ağı ve alt ağ üzerinde anahtar kasası için etkinleştirin.
   ```powershell
   Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzVirtualNetwork
   ```

4. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin.
   ```powershell
   $subnet = Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"
   Add-AzKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
   ```

5. Bir IP adresi aralığı, trafiğe izin verecek şekilde ekleyin.
   ```powershell
   Add-AzKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
   ```

6. Bu anahtar kasası güvenilir hizmetlerin tarafından erişilebilir olması gereken verilirse `bypass` için `AzureServices`.
   ```powershell
   Update-AzKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
   ```

7. Varsayılan eylem ayarlayarak ağ kurallarını açma `Deny`.
   ```powershell
   Update-AzKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
   ```

## <a name="references"></a>Başvurular

* Azure CLI komutları: [az keyvault ağ-rule](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Azure PowerShell cmdlet'leri: [Get-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/get-azkeyvault), [Add-AzKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/az.KeyVault/Add-azKeyVaultNetworkRule), [Remove-AzKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/az.KeyVault/Remove-azKeyVaultNetworkRule), [Update-AzKeyVaultNetworkRuleSet](https://docs.microsoft.com/powershell/module/az.KeyVault/Update-azKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Sonraki adımlar

* [Sanal ağ hizmet uç noktaları için Key Vault](key-vault-overview-vnet-service-endpoints.md)
* [Anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md)
