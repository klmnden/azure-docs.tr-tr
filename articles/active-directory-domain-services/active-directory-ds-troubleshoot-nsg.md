---
title: 'Azure Active Directory etki alanı Hizmetleri: Sorun giderme ağ güvenlik grubu yapılandırma | Microsoft Docs'
description: Azure AD etki alanı Hizmetleri için NSG yapılandırma sorunlarını giderme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: ergreenl
ms.openlocfilehash: ce03ee0e0936cea4b96e48fbc949f40ee0fe83a0
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="troubleshoot-invalid-networking-configuration-for-your-managed-domain"></a>Geçersiz ağ yapılandırması, yönetilen etki alanınız için sorun giderme
Bu makale ve aşağıdaki uyarı iletisine neden ağ ile ilişkili yapılandırma hatalarını gidermek yardımcı olur:

## <a name="alert-aadds104-network-error"></a>Uyarı AADDS104: Ağ hatası
**Uyarı iletisi:** *Microsoft, bu yönetilen etki alanı için etki alanı denetleyicilerinin ulaşmasını oluşturamıyor. Bu sanal ağ blokları erişiminizi yönetilen etki alanı için yapılandırılmış bir ağ güvenlik grubu (NSG) durum. Başka bir olası neden, kullanıcı tanımlı bir yol yoksa, Internet'ten gelen trafiği engeller olmasıdır.*

Geçersiz NSG ağ hatalarının en yaygın nedeni Azure AD etki alanı Hizmetleri için bağlantılardır. Sanal ağınızı erişmesine izin vermek için yapılandırılmış ağ güvenlik grubu (NSG) [belirli bağlantı noktalarını](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services). Bu bağlantı noktaları engellenirse Microsoft izlemek veya yönetilen etki alanınızı güncelleştirin. Ayrıca, Azure AD dizininizi ve yönetilen etki alanınız arasında eşitleme etkilenmez. NSG oluşturulurken, bu bağlantı noktaları hizmetindeki kesintiye uğramaması için açık tutun.


## <a name="sample-nsg"></a>Örnek NSG
Aşağıdaki tabloda, yönetilen etki alanınız güvenli izlemek, yönetmek ve bilgileri güncelleştirmek Microsoft verirken engelleneceği NSG bir örnek gösterilmektedir.

![Örnek NSG](.\media\active-directory-domain-services-alerts\default-nsg.png)

>[!NOTE]
> Azure AD etki alanı Hizmetleri sanal ağdan sınırsız giden erişim gerektirir. Sanal ağ için giden erişimi kısıtlayan herhangi bir ek NSG kural oluşturmamayı öneririz.

## <a name="add-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir ağ güvenlik grubu kural ekleme
PowerShell kullanmak istemiyorsanız, Azure portalını kullanarak Nsg'ler tek kuralları el ile ekleyebilirsiniz. Ağ güvenlik grubundaki kuralları oluşturmak için aşağıdaki adımları tamamlayın:

1. Gidin [ağ güvenlik grupları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) Azure portalında sayfası
2. Tablodan, yönetilen etki alanınızı etkin olduğu alt ağ ile ilişkili NSG seçin.
3. Altında **ayarları** sol panelinde ya da tıklatın **gelen güvenlik kuralları** veya **giden güvenlik kurallarında**.
4. Tıklayarak kural oluşturma **Ekle** ve bilgileri doldurma. **Tamam**’a tıklayın.
5. Kuralınız kuralları tabloda bularak oluşturulup oluşturulmadığını denetleyin.


## <a name="create-an-nsg-for-azure-ad-domain-services-using-powershell"></a>PowerShell kullanarak Azure AD etki alanı Hizmetleri için bir NSG oluşturma
Bu NSG herhangi diğer istenmeyen gelen erişimi engelleme sırasında Azure AD etki alanı Hizmetleri tarafından gerekli bağlantı noktalarına gelen trafiğe izin verecek şekilde yapılandırılır.

**Önkoşul: Azure PowerShell'i yükleyip** yönergelerini izleyin [Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlanmak](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

>[!TIP]
> Azure PowerShell modülü en son sürümünü kullanmanızı öneririz. Yüklü Azure PowerShell modülünü daha eski bir sürümü zaten varsa, en son sürüme güncelleştirin.
>

PowerShell kullanarak yeni bir NSG oluşturmak için aşağıdaki adımları kullanın.
1. Azure aboneliğinizde oturum açın.

  ```PowerShell
  # Log in to your Azure subscription.
  Connect-AzureRmAccount
  ```

2. Bir NSG ile üç kurallar oluşturun. Aşağıdaki komut dosyası, Azure AD etki alanı hizmetleri çalıştırmak için gerekli bağlantı noktalarını erişmesine izin vermek NSG için üç kuralları tanımlar. Sonra bu kuralları içeren yeni bir NSG komut dosyasını oluşturur. Sanal ağda dağıtılmış iş yükleri için gerekirse diğer gelen trafiğe izin ek kurallar eklemek için aynı biçimi kullanır.

  ```PowerShell
  # Allow inbound HTTPS traffic to enable synchronization to your managed domain.
  $SyncRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowSyncWithAzureAD -Description "Allow synchronization with Azure AD" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 443

  # Allow management of your domain over port 5986 (PowerShell Remoting)
  $PSRemotingRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowPSRemoting -Description "Allow management of domain through port 5986" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
  -SourceAddressPrefix 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161 -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 5986

  #The following two rules are optional and needed only in certain situations.

  # Allow management of your domain over port 3389 (remote desktop).
  $RemoteDesktopRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowRD -Description "Allow management of domain through port 3389" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
  -SourceAddressPrefix 207.68.190.32/27, 13.106.78.32/27, 10.254.32.0/20, 10.97.136.0/22, 13.106.174.32/27, 13.106.4.96/27 -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389

  # Secure LDAP rule, it is recommended to change the source address prefix to include only the IP addresses
  $SecureLDAPRule = New-AzureRmNetworkSecurityRuleConfig -Name SecureLDAP -Description "Allow access through secure LDAP port" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 104 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 636

  # Create the NSG with the rules above (if you need the remote desktop rule and secure ldap rule, add it below)
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
  -Name "AADDomainServices-NSG" -SecurityRules $SyncRule, $PSRemotingRule
  ```

3. Son olarak, NSG vnet ve alt ağ tercih ile ilişkilendirin.

  ```PowerShell
  # Find vnet and subnet
  $Vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroup -Name $VnetName
  $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $Vnet -Name $SubnetName

  # Set the nsg to the subnet and save the changes
  $Subnet.NetworkSecurityGroup = $Nsg
  Set-AzureRmVirtualNetwork -VirtualNetwork $Vnet
  ```

## <a name="full-script-to-create-and-apply-an-nsg-for-azure-ad-domain-services"></a>Komut dosyası oluşturabilir ve Azure AD etki alanı Hizmetleri için bir NSG uygulayabilirsiniz tam
>[!TIP]
> Azure PowerShell modülü en son sürümünü kullanmanızı öneririz. Yüklü Azure PowerShell modülünü daha eski bir sürümü zaten varsa, en son sürüme güncelleştirin.
>

```PowerShell
# Change the following values to match your deployment
$ResourceGroup = "ResourceGroupName"
$Location = "westus"
$VnetName = "exampleVnet"
$SubnetName = "exampleSubnet"

# Log in to your Azure subscription.
Connect-AzureRmAccount

# Allow inbound HTTPS traffic to enable synchronization to your managed domain.
$SyncRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowSyncWithAzureAD -Description "Allow synchronization with Azure AD" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 443

# Allow management of your domain over port 5986 (PowerShell Remoting)
$PSRemotingRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowPSRemoting -Description "Allow management of domain through port 5986" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
-SourceAddressPrefix 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161 -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 5986

# The following two rules are optional and needed only in certain situations.

# Allow management of your domain over port 3389 (remote desktop).
$RemoteDesktopRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowRD -Description "Allow management of domain through port 3389" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
-SourceAddressPrefix 207.68.190.32/27, 13.106.78.32/27, 10.254.32.0/20, 10.97.136.0/22, 13.106.174.32/27, 13.106.4.96/27 -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389

# Secure LDAP rule, it is recommended to change the source address prefix to include only the IP addresses
$SecureLDAPRule = New-AzureRmNetworkSecurityRuleConfig -Name SecureLDAP -Description "Allow access through secure LDAP port" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 104 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 636

# Create the NSG with the rules above (if you need the remote desktop rule and secure ldap rule, add it below)
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
-Name "AADDomainServices-NSG" -SecurityRules $SyncRule, $PSRemotingRule

# Find vnet and subnet
$Vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroup -Name $VnetName
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $Vnet -Name $SubnetName

# Set the nsg to the subnet and save the changes
$Subnet.NetworkSecurityGroup = $Nsg
Set-AzureRmVirtualNetwork -VirtualNetwork $Vnet
```


## <a name="need-help"></a>Yardım mı gerekiyor?
Azure Active Directory etki alanı Hizmetleri ürün ekibine başvurun [paylaşmak geri bildirim veya destek](active-directory-ds-contact-us.md).
