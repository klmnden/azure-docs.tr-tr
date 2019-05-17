---
title: PowerShell kullanarak azure blok zinciri hizmet consortium Yönetimi
description: PowerShell kullanarak Azure blok zinciri hizmeti consortium üyeleri yönetme
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/10/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: f15fa3b4972a2ac54d1d9bce916fdd42c2951d2f
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550873"
---
# <a name="manage-consortium-members-in-azure-blockchain-service-using-powershell"></a>PowerShell kullanarak Azure blok zinciri hizmetinde Consortium üyelerini Yönet

Azure Blockchain hizmetiniz için blok zinciri consortium üyelerini yönetmek için PowerShell kullanabilirsiniz. Üyeleri bir yönetici ayrıcalıklarıyla davet, Ekle, Kaldır rolleri için blok zinciri consortium tüm katılımcıları değiştirir. Üyeleri kullanıcı ayrıcalıklarıyla blok zinciri consortium tüm katılımcıları görüntüleyebilirsiniz ve kullanıcıların üye görünen adı değiştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* [Azure portalını kullanarak bir blok zinciri üye oluştur](create-member.md)
* Consortia, üyeleri ve düğümleri hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmet consortium](consortium.md)

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="install-powershell-module"></a>PowerShell modülünü yükleme

PowerShell Galerisi'nden Microsoft.AzureBlockchainService.ConsortiumManagement.PS paketi yükleyin.

```powershell-interactive
Install-Module -Name Microsoft.AzureBlockchainService.ConsortiumManagement.PS -Scope CurrentUser
Import-Module Microsoft.AzureBlockchainService.ConsortiumManagement.PS
```

## <a name="set-information-preference"></a>Kümesi bilgi tercihi

Cmdlet'ler tarafından bilgi tercih değişkeni ayarlama yürütülürken daha fazla bilgi edinebilirsiniz. Varsayılan olarak, *$InformationPreference* ayarlanır *SilentlyContinue*.

Cmdlet'lerinden daha ayrıntılı bilgi için tercih PowerShell'de şu şekilde ayarlayın:

```powershell-interactive
$InformationPreference = 'Continue'
```

## <a name="establish-a-web3-connection"></a>Web3 bağlantı

Consortium üyelerini yönetmek için Azure blok zinciri hizmeti üye uç noktanızı Web3 bağlantı yapmanız gerekir. Bu betik, consortium Yönetimi cmdlet'leri çağrılırken kullanılacak genel değişkenleri ayarlamak için kullanabilirsiniz.

```powershell-interactive
$Connection = New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
$MemberAccount = Import-Web3Account -ManagedAccountAddress '<Member account address>' -ManagedAccountPassword '<Member account password>'
$ContractConnection = Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>' -Web3Client $Connection
```

Değiştirin \<üye hesap parolası\> üye oluştururken kullanılan üye hesap parolası ile.

Diğer değerleri, Azure portalında bulun:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Varsayılan Azure blok zinciri hizmet üyesine gidin **genel bakış** sayfası.

    ![Üye genel bakış](./media/manage-consortium-powershell/member-overview.png)

    Değiştirin \<üye hesabı\>, ve \<RootContract adresi\> portalından değerlerle.

1. Uç nokta adresini seçin **işlem düğümleri** seçip **varsayılan** işlem düğümü. Varsayılan işlem düğümü blok zinciri üyeyle aynı ada sahip.
1. Seçin **bağlantı dizeleri**.

    ![Bağlantı dizeleri](./media/manage-consortium-powershell/connection-strings.png)

    Değiştirin \<uç nokta adresi\> değeriyle **HTTPS (erişim anahtarı: 1)** veya **HTTPS (2 erişim anahtarı)**.

## <a name="network-and-smart-contract-management"></a>Ağ ve akıllı sözleşme Yönetimi

Akıllı sözleşme cmdlet'leri ve ağın blok zinciri uç nokta akıllı sözleşmelerinizin consortium yönetiminden sorumlu bir bağlantı kurmak için kullanın.

### <a name="import-consortiummanagementcontracts"></a>İçeri aktarma ConsortiumManagementContracts

Yönetebilir ve zorunlu üyeleri consortium içinde kullanılan consortium yönetim akıllı anlaşmalar bağlanır.

`Import-ConsortiumManagementContracts -RootContractAddress <String> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| RootContractAddress | Kök sözleşme adresini consortium yönetim akıllı anlaşmalar | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

**Örnek**

```powershell-interactive
Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>'  -Web3Client $Connection
```

### <a name="import-web3account"></a>İçeri aktarma Web3Account

Uzak düğüm yönetim hesabı bilgileri tutmak için bir nesne oluşturmak için bu cmdlet'i kullanın.

`Import-Web3Account -ManagedAccountAddress <String> -ManagedAccountPassword <String>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| ManagedAccountAddress | Blok zinciri üye hesap adresi | Evet |
| ManagedAccountPassword | Hesap adresi parolası | Evet |

**Örnek**

```powershell-interactive
Import-Web3Account -ManagedAccountAddress '<Member account address>'  -ManagedAccountPassword '<Member account password>'
```

### <a name="new-web3connection"></a>Yeni Web3Connection

Bir işlem düğümünün RPC uç nokta için bir bağlantı kurar.

`New-Web3Connection [-RemoteRPCEndpoint <String>]`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| RemoteRPCEndpoint | Blok zinciri üye uç nokta adresi | Evet |

**Örnek**

```powershell-interactive
New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
```

## <a name="consortium-member-management"></a>Consortium üye yönetim

Consortium üye yönetim cmdlet'leri consortium içinde üyelerini yönetmek için kullanın. Kullanılabilir eylemler consortium rolünüze bağlıdır.

### <a name="get-blockchainmember"></a>Get-BlockchainMember

Üye bilgilerini veya consortium üyelerinin listesini alır.

`Get-BlockchainMember [[-Name] <String>] -Members <IContract> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Ad | Ayrıntıları almak istediğiniz Azure blok zinciri hizmet üyesinin adı. Üye adı sağlarsanız, üye ayrıntılarını döndürülür. Ad belirtilmezse, tüm consortium üyelerin listesi döndürülür. | Hayır |
| Üyeler | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

**Örnek**

```powershell-interactive
$ContractConnection | Get-BlockchainMember -Name <Member Name>
```

**Örnek çıktı**

```
Name           : myblockchainmember
CorrelationId  : 0
DisplayName    : myCompany
SubscriptionId : <Azure subscription ID>
AccountAddress : 0x85b911c9e103d6405573151258d668479e9ebeef
Role           : ADMIN
```

### <a name="remove-blockchainmember"></a>Remove-BlockchainMember

Blok zinciri üyesi kaldırır.

`Remove-BlockchainMember -Name <String> -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Ad | Üye adı kaldırmak için | Evet |
| Üyeler | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Account | İçeri aktarma-Web3Account alınan Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

**Örnek**

```powershell-interactive
$ContractConnection | Remove-BlockchainMember -Name <Member Name> -Web3Account $MemberAccount
```

### <a name="set-blockchainmember"></a>Set-BlockchainMember

Blok zinciri görünen adı ve consortium rolü dahil olmak üzere üye öznitelikleri ayarlar.

Consortium Yöneticiler ayarlayabilir **DisplayName** ve **rol** tüm üyeleri için. Kullanıcı rolünün üyesiyle Consortium yalnızca kendi üyenin görünen adını değiştirebilirsiniz.

`Set-BlockchainMember -Name <String> [-DisplayName <String>] [-AccountAddress <String>] [-Role <String>]
 -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Ad | Blok zinciri üyenin adı | Evet |
| DisplayName | Yeni görünen adı | Hayır |
| AccountAddress | Hesap adresi | Hayır |
| Üyeler | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Account | İçeri aktarma-Web3Account alınan Web3Account nesnesi | Evet |
| Web3Client |  New-Web3Connection alınan Web3Client nesnesi| Evet |

**Örnek**

```powershell-interactive
$ContractConnection | Set-BlockchainMember -Name <Member Name> -DisplayName <Display name> -Web3Account $MemberAccount
```

## <a name="consortium-member-invitation-management"></a>Consortium üye Davetiyesi Yönetimi

Üye davet consortium yönetmek için Consortium üyesi davet Yönetimi cmdlet'leri kullanın. Kullanılabilir eylemler consortium rolünüze bağlıdır.

### <a name="new-blockchainmemberinvitation"></a>Yeni BlockchainMemberInvitation

Consortium üyeler davet edin.

`New-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Davet edilen üye Azure abonelik kimliği | Evet |
| Rol | Consortium rolü. Değerler, yönetici veya kullanıcı olabilir. Yönetici consortium Yöneticisi rolüdür. Kullanıcı consortium üye rolüdür. | Evet |
| Üyeler | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Account | İçeri aktarma-Web3Account alınan Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

**Örnek**

```powershell-interactive
$ContractConnection | New-BlockchainMemberInvitation -SubscriptionId <Azure Subscription ID> -Role USER -Web3Account $MemberAccount
```

### <a name="get-blockchainmemberinvitation"></a>Get-BlockchainMemberInvitation

Alır veya consortium üye davet durumunu listeler.

`Get-BlockchainMemberInvitation [[-SubscriptionId] <String>] -Members <IContract> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Davet edilen üye Azure abonelik kimliği. Subscriptionıd sağlanırsa, abonelik kimliği davet ayrıntılarını döndürülür. Subscriptionıd atlanırsa, tüm üye davet listesi döndürülür. | Hayır |
| Üyeler | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

**Örnek**

```powershell-interactive
$ContractConnection | Get-BlockchainMemberInvitation – SubscriptionId <Azure subscription ID>
```

**Örnek çıktı**

```
SubscriptionId                       Role CorrelationId
--------------                       ---- -------------
<Azure subscription ID>              USER             2
```

### <a name="remove-blockchainmemberinvitation"></a>Remove-BlockchainMemberInvitation

Consortium üyesi davet iptal eder.

`Remove-BlockchainMemberInvitation -SubscriptionId <String> -Members <IContract> -Web3Account <IAccount>
 -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Üyenin iptal etmek için Azure abonelik kimliği | Evet |
| Üyeler | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Account | İçeri aktarma-Web3Account alınan Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

**Örnek**

```powershell-interactive
$ContractConnection | Remove-BlockchainMemberInvitation -SubscriptionId <Subscription ID> -Web3Account $MemberAccount
```

### <a name="set-blockchainmemberinvitation"></a>Set-BlockchainMemberInvitation

Kümeleri **rol** var olan bir davet için. Yalnızca Yöneticiler consortium davetleri değiştirebilirsiniz.

`Set-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Davet edilen üye Azure abonelik kimliği | Evet |
| Rol | Davet için yeni consortium rolü. Değerleri **kullanıcı** veya **yönetici** | Evet |
| Üyeler |  İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Account | İçeri aktarma-Web3Account alınan Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

**Örnek**

```powershell-interactive
$ContractConnection | Set-BlockchainMemberInvitation -SubscriptionId <Azure subscription ID> -Role USER -Web3Account $MemberAccount
```

## <a name="next-steps"></a>Sonraki adımlar

Consortia, üyeleri ve düğümleri hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Azure Blockchain hizmet consortium](consortium.md)