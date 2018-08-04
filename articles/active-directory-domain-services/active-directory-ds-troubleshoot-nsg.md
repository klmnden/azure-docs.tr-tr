---
title: 'Azure Active Directory Domain Services: Sorun giderme ağ güvenlik grubu yapılandırması | Microsoft Docs'
description: Azure AD Domain Services için NSG yapılandırmasıyla ilgili sorunları giderme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: ergreenl
ms.openlocfilehash: a2acbed81e323718c7d294d87ebf699c35664d02
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502654"
---
# <a name="troubleshoot-invalid-networking-configuration-for-your-managed-domain"></a>Geçersiz ağ yapılandırması, yönetilen etki alanınız için sorun giderme
Bu makale ve aşağıdaki uyarı iletisinde neden ağla ilgili yapılandırma hatalarını gidermek yardımcı olur:

## <a name="alert-aadds104-network-error"></a>Uyarı AADDS104: Ağ hatası
**Uyarı iletisi:** *Microsoft bu yönetilen etki alanı için etki alanı denetleyicilerine ulaşamıyoruz. Bu, sanal ağ yönetilen etki alanı erişimi engeller üzerinde yapılandırılan bir ağ güvenlik grubu (NSG) durum meydana. Başka bir olası neden, kullanıcı tanımlı bir yol varsa, internet'ten gelen trafiği engeller. ' dir.*

Geçersiz NSG ağ hatalarının en yaygın nedeni Azure AD Domain Services için yapılandırmalardır. Sanal ağınızı erişmesine izin vermek için yapılandırılan ağ güvenlik grubu (NSG) [belirli bağlantı noktalarını](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services). Bu bağlantı noktaları engellenirse, Microsoft izleme veya yönetilen etki alanınıza güncelleştirin. Ayrıca, Azure AD dizininizi ve yönetilen etki alanınıza eşitlemesi etkilenir. NSG oluşturulurken bu bağlantı noktaları hizmet kesintisini önlemek için açık tutun.

### <a name="checking-your-nsg-for-compliance"></a>NSG uyumluluğu denetleniyor

1. Gidin [ağ güvenlik grupları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) Azure portalında sayfası
2. Tablodan, yönetilen etki alanınıza etkinleştirildiği alt ağ ile ilişkilendirilmiş NSG seçin.
3. Altında **ayarları** sol panelde tıklayın **gelen güvenlik kuralları**
4. Yerinde kuralları gözden geçirin ve erişimi engelleme hangi kuralları tanımlamak [Bu bağlantı noktaları](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services)
5. NSG kuralı siliniyor, bir kural ekleme ya da tamamen yeni bir NSG oluşturmayı uyumluluk sağlamak için düzenleyin. Adımları [alınabilecek](#add-a-rule-to-a-network-security-group-using-the-azure-portal) veya [yeni, uyumlu bir NSG oluşturmak](#create-a-nsg-for-azure-ad-domain-services-using-powershell) olan aşağıdaki

## <a name="sample-nsg"></a>Örnek NSG
Aşağıdaki tabloda, yönetilen etki alanınıza güvenli izleme, yönetme ve güncelleştirme bilgileri Microsoft'a verirken engelleneceği NSG bir örnek gösterilmektedir.

![Örnek NSG](.\media\active-directory-domain-services-alerts\default-nsg.png)

>[!NOTE]
> Azure AD Domain Services, sanal ağdan giden sınırsız erişim gerektirir. Sanal ağ için giden erişimi kısıtlayan herhangi ek bir NSG kuralı oluşturmamayı öneririz.

## <a name="add-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Azure portalını kullanarak bir ağ güvenlik grubu Kuralı Ekle
PowerShell kullanmak istemiyorsanız, Azure portalı kullanarak Nsg'ler için tek kuralları el ile ekleyebilirsiniz. Ağ güvenlik grubunuzu kuralları oluşturmak için aşağıdaki adımları tamamlayın:

1. Gidin [ağ güvenlik grupları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) Azure portalında sayfası.
2. Tablodan, yönetilen etki alanınıza etkinleştirildiği alt ağ ile ilişkilendirilmiş NSG seçin.
3. Altında **ayarları** sol panelde tıklayın **gelen güvenlik kuralları** veya **giden güvenlik kuralları**.
4. Tıklayarak kural oluşturma **Ekle** ve bilgiler. **Tamam** düğmesine tıklayın.
5. Kurallar tablodaki yerleştirerek, kuralı oluşturuldu doğrulayın.


## <a name="create-a-nsg-for-azure-ad-domain-services-using-powershell"></a>PowerShell kullanarak Azure AD Domain Services için bir NSG oluşturma
Bu NSG, tüm diğer istenmeyen gelen erişimi engelleme sırasında Azure AD Domain Services tarafından gerekli bağlantı noktalarına gelen trafiğe izin verecek şekilde yapılandırılır.

**Önkoşul: PowerShell'i yükleme ve Azure yapılandırma** yönergelerini izleyin [Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlayın](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

>[!TIP]
> Azure PowerShell modülünün en son sürümünü kullanmanızı öneririz. Azure PowerShell Modülü yüklü daha eski bir sürümü zaten varsa, en son sürüme güncelleştirin.
>

PowerShell kullanarak yeni bir NSG oluşturmak için aşağıdaki adımları kullanın.
1. Azure aboneliğinizde oturum açın.

  ```PowerShell
  # Log in to your Azure subscription.
  Connect-AzureRmAccount
  ```

2. Bir NSG ile üç kurallar oluşturun. Aşağıdaki betik, Azure AD Domain Services'ı çalıştırmak için gereken bağlantı noktalarına erişime izin veren NSG için üç kuralları tanımlar. Ardından, bu kurallar içeren yeni bir NSG betik oluşturur. Sanal ağda dağıtılan iş yükleri için gerekirse diğer gelen trafiğe izin veren ek kuralları eklemek için aynı biçimini kullanın.

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

3. Son olarak, NSG vnet ve tercih ettiğiniz alt ağı ile ilişkilendirin.

  ```PowerShell
  # Find vnet and subnet
  $Vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroup -Name $VnetName
  $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $Vnet -Name $SubnetName

  # Set the nsg to the subnet and save the changes
  $Subnet.NetworkSecurityGroup = $Nsg
  Set-AzureRmVirtualNetwork -VirtualNetwork $Vnet
  ```

## <a name="full-script-to-create-and-apply-an-nsg-for-azure-ad-domain-services"></a>Tam komut dosyası oluşturabilir ve Azure AD Domain Services için bir NSG uygulayabilirsiniz
>[!TIP]
> Azure PowerShell modülünün en son sürümünü kullanmanızı öneririz. Azure PowerShell Modülü yüklü daha eski bir sürümü zaten varsa, en son sürüme güncelleştirin.
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
Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](active-directory-ds-contact-us.md).
