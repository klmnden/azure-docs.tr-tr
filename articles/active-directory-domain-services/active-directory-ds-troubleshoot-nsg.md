---
title: "Azure Active Directory etki alanı Hizmetleri: Sorun giderme ağ güvenlik grubu yapılandırma | Microsoft Docs"
description: "Azure AD etki alanı Hizmetleri için NSG yapılandırma sorunlarını giderme"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: ergreenl
ms.openlocfilehash: 447f9119ea379e278be77d8699c7dcb751ea3085
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-network-security-group-configuration"></a>Azure AD etki alanı Hizmetleri - sorun giderme ağ güvenlik grubu yapılandırma



## <a name="aadds104-network-error"></a>AADDS104: Ağ hatası

**Uyarı iletisi:** *Microsoft, bu yönetilen etki alanı için etki alanı denetleyicilerinin ulaşmasını oluşturamıyor. Bu sanal ağ blokları erişiminizi yönetilen etki alanı için yapılandırılmış bir ağ güvenlik grubu (NSG) durum. Başka bir olası neden, bu blokları gelen trafiğin internet'ten bir kullanıcı tanımlı yol olup olmadığını olmasıdır.*

Ağ hatalarının en yaygın nedeni Azure AD etki alanı Hizmetleri için yanlış NSG yapılandırmaları öznitelikli. Microsoft hizmet ve etki alanı yönetilen korumak emin olmak için ağ güvenlik grubu (NSG) erişmesine izin vermek için kullanmalısınız [belirli bağlantı noktalarını](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services) alt ağınız içinde. Bu bağlantı noktaları engellenirse Microsoft hizmetinize detrimental olabilecek, gereken, kaynaklara erişemez. NSG oluşturulurken, bu bağlantı noktaları herhangi bir kesinti emin olmak için açık tutun.

## <a name="sample-nsg"></a>Örnek NSG
Aşağıdaki tabloda, yönetilen etki alanınız güvenli izlemek, yönetmek ve bilgileri güncelleştirmek Microsoft verirken engelleneceği NSG bir örnek gösterilmektedir.

![Örnek NSG](.\media\active-directory-domain-services-alerts\default-nsg.png)

>[!NOTE]
> Azure AD etki alanı Hizmetleri sınırsız giden erişim gerektirir. Nsg'lerinizi herhangi bir ek outboud kurala oluşturmamayı öneririz.

## <a name="adding-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir ağ güvenlik grubu kural ekleme
PowerShell kullanmak istemiyorsanız, Azure portalını kullanarak Nsg'ler tek kuralları el ile ekleyebilirsiniz. Ağ güvenlik grubundaki kuralları oluşturmak için aşağıdaki adımları izleyin.

1. Gidin [ağ güvenlik grupları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) Azure portalında sayfası
2. Tablodan, etki alanı ile ilişkili NSG seçin.
3. Sol taraftaki gezinti ayarlarında altında tıklayın **gelen güvenlik kuralları** veya **giden güvenlik kurallarında**.
4. Tıklayarak kural oluşturma **Ekle** ve bilgileri doldurma. **Tamam**’a tıklayın.
5. Kuralınız kuralları tabloda bularak oluşturulup oluşturulmadığını denetleyin.


## <a name="create-a-default-nsg-using-powershell"></a>Varsayılan oluşturma NSG PowerShell'i kullanma

Önceki tüm bağlantı noktaları Azure AD etki alanı çalıştırmak için Hizmetleri herhangi bir istenmeyen erişimi engelleme sırasında açın tutar PowerShell kullanarak yeni bir NSG oluşturmak için kullanabileceğiniz adımlar verilmektedir.


Bu çözüm yüklenmesi ve çalıştırılması gerektirir [Azure AD Powershell](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?toc=%2Fazure%2Factive-directory-domain-services%2Ftoc.json&view=azureadps-2.0).

1. Azure AD dizininizi bağlayın.

  ```PowerShell
  # Connect to your Azure AD directory.
  Connect-AzureAD
  ```
2. Azure aboneliğinizde oturum açın.

  ```PowerShell
  # Log in to your Azure subscription.
  Login-AzureRmAccount
  ```

3. Bir NSG ile üç kurallar oluşturun. Aşağıdaki komut dosyası, Azure AD etki alanı hizmetleri çalıştırmak için gerekli bağlantı noktalarını erişmesine izin vermek NSG için üç kuralları tanımlar. Sonra bu kuralları içeren yeni bir NSG komut dosyasını oluşturur. Aynı biçimini kullanarak uygun gördüğünüz şekilde ek kurallar eklemek Hoş Geldiniz.

  ```PowerShell
  # Create the rules needed
  $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 443
  $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389
  $rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
  -SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 5986
  # Create the NSG with the 3 rules above
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
  -Name "AADDS-NSG" -SecurityRules $rule1,$rule2,$rule3
  ```

4. Son olarak, komut dosyası vnet ve alt ağ tercih NSG ilişkilendirir.

  ```PowerShell
  # Find vnet and subnet
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
  # Set the nsg to the subnet and save the changes
  $subnet.NetworkSecurityGroup = $nsg
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

### <a name="full-script"></a>Tam betik

```PowerShell
#Change the following values to match your deployment
$resourceGroup = "ResourceGroupName"
$vnetName = "exampleVnet"
$subnetName = "exampleSubnet"

$serviceIPs = "52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161"

# Create the rules needed
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 443

$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389

$rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
-SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389


# Connect to your Azure AD directory.
Connect-AzureAD

# Log in to your Azure subscription.
Login-AzureRmAccount

# Create the NSG with the 3 rules above
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
-Name "NSG-Default" -SecurityRules $rule1,$rule2,$rule3

# Find vnet and subnet
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName

# Set the nsg to the subnet and save the changes
$subnet.NetworkSecurityGroup = $nsg
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

> [!NOTE]
>Bu varsayılan NSG güvenli LDAP için kullanılan bağlantı noktasına erişim kilitleme değil. Bu bağlantı noktası kuralının nasıl oluşturulacağını öğrenmek için ziyaret [bu makalede](active-directory-ds-troubleshoot-ldaps.md).
>

## <a name="contact-us"></a>Bizimle iletişim kurun
Azure Active Directory etki alanı Hizmetleri ürün ekibine başvurun [paylaşmak geri bildirim veya destek](active-directory-ds-contact-us.md).
