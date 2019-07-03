---
title: Azure Blockchain hizmet consortium üyeleri, Azure PowerShell kullanarak yönetme
description: Azure PowerShell kullanarak Azure blok zinciri hizmeti consortium üyeleri yönetmeyi öğrenin.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/10/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 4bb72bc3fe8b85a8d2aed88e02f5f3150abb6899
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66493653"
---
# <a name="manage-consortium-members-in-azure-blockchain-service-by-using-powershell"></a>PowerShell kullanarak Azure blok zinciri hizmetinde Consortium üyelerini Yönet

Azure Blockchain hizmetiniz için blok zinciri consortium üyelerini yönetmek için PowerShell kullanabilirsiniz. Yönetici ayrıcalıklarına sahip üyeler davet, ekleyin, kaldırın ve blok zinciri consortium tüm katılımcılar için rolleri değiştirin. Kullanıcı ayrıcalıkları üyeleri blok zinciri consortium tüm katılımcıları görüntüleyebilir ve kullanıcıların üye görünen adı değiştirin.

## <a name="prerequisites"></a>Önkoşullar

* Blok zinciri üye kullanarak oluşturmak [Azure portalında](create-member.md).
* Consortia, üyeleri ve düğümleri hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmet consortium](consortium.md).

## <a name="open-azure-cloud-shell"></a>Azure Cloud Shell’i açma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Giderek bir ayrı bir tarayıcı sekmesinde Cloud Shell açabilirsiniz [shell.azure.com/powershell](https://shell.azure.com/powershell). Seçin **kopyalama** kod bloklarını kopyalamak için kopyalayıp Cloud shell'e yapıştırın ve seçin **Enter** çalıştırmak için.

## <a name="install-the-powershell-module"></a>PowerShell modülünü yükleme

PowerShell Galerisi'nden Microsoft.AzureBlockchainService.ConsortiumManagement.PS paketi yükleyin.

```powershell-interactive
Install-Module -Name Microsoft.AzureBlockchainService.ConsortiumManagement.PS -Scope CurrentUser
Import-Module Microsoft.AzureBlockchainService.ConsortiumManagement.PS
```

## <a name="set-the-information-preference"></a>Bilgi tercihini Ayarla

Bilgi tercih değişkeni ayarlayarak cmdlet'leri çalıştırılırken daha fazla bilgi edinebilirsiniz. Varsayılan olarak, *$InformationPreference* ayarlanır *SilentlyContinue*.

Cmdlet'lerinden daha ayrıntılı bilgi için tercih PowerShell'de şu şekilde ayarlayın:

```powershell-interactive
$InformationPreference = 'Continue'
```

## <a name="establish-a-web3-connection"></a>Web3 bağlantı

Consortium üyelerini yönetmek için blok zinciri hizmeti üye uç noktanızı Web3 bağlantı kurun. Bu betik, consortium Yönetimi cmdlet'leri çağırmak için genel değişkenleri ayarlamak için kullanabilirsiniz.

```powershell-interactive
$Connection = New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
$MemberAccount = Import-Web3Account -ManagedAccountAddress '<Member account address>' -ManagedAccountPassword '<Member account password>'
$ContractConnection = Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>' -Web3Client $Connection
```

Değiştirin *\<üye hesap parolası\>* üye oluştururken kullandığınız üye hesap parolası ile.

Diğer değerleri, Azure portalında bulun:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Varsayılan Blok zinciri Service üyelik Git **genel bakış** sayfası.

    ![Üye genel bakış](./media/manage-consortium-powershell/member-overview.png)

    Değiştirin *\<üye hesabı\>* ve *\<RootContract adresi\>* portalından değerlerle.

1. Uç nokta adresini seçin **işlem düğümleri**ve ardından **varsayılan işlem düğümü**. Varsayılan düğüm blockchain üyeyle aynı ada sahip.
1. Seçin **bağlantı dizeleri**.

    ![Bağlantı dizeleri](./media/manage-consortium-powershell/connection-strings.png)

    Değiştirin *\<uç nokta adresi\>* değeriyle **HTTPS (erişim anahtarı: 1)** veya **HTTPS (2 erişim anahtarı)** .

## <a name="manage-the-network-and-smart-contracts"></a>Ağ ve smart contracts yönetme

Akıllı sözleşme cmdlet'leri ve ağın blok zinciri uç noktanın nitelikli akıllı anlaşmalar consortium yönetiminden sorumlu bir bağlantı kurmak için kullanın.

### <a name="import-consortiummanagementcontracts"></a>İçeri aktarma ConsortiumManagementContracts

Consortium yönetim takımının nitelikli akıllı anlaşmalar için bağlanmak için bu cmdlet'i kullanın. Bu sözleşmeler, yönetebilir ve zorunlu consortium üyeler için kullanılır.

`Import-ConsortiumManagementContracts -RootContractAddress <String> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| RootContractAddress | Kök sözleşme adresini consortium yönetim akıllı anlaşmalar | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>'  -Web3Client $Connection
```

### <a name="import-web3account"></a>İçeri aktarma Web3Account

Uzak düğümün yönetim hesabı bilgileri tutmak için bir nesne oluşturmak için bu cmdlet'i kullanın.

`Import-Web3Account -ManagedAccountAddress <String> -ManagedAccountPassword <String>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| ManagedAccountAddress | Blok zinciri üye hesap adresi | Evet |
| ManagedAccountPassword | Hesap adresi parolası | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
Import-Web3Account -ManagedAccountAddress '<Member account address>'  -ManagedAccountPassword '<Member account password>'
```

### <a name="new-web3connection"></a>Yeni Web3Connection

Bir işlem düğümünün RPC uç nokta bağlantı kurmak için bu cmdlet'i kullanın.

`New-Web3Connection [-RemoteRPCEndpoint <String>]`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| RemoteRPCEndpoint | Blok zinciri üye uç nokta adresi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
```

## <a name="manage-the-consortium-members"></a>Consortium üyelerini Yönet

Consortium üye yönetim cmdlet'leri consortium içinde üyelerini yönetmek için kullanın. Kullanılabilir eylemler consortium rolünüze bağlıdır.

### <a name="get-blockchainmember"></a>Get-BlockchainMember

Üye bilgilerini alma veya consortium üyelerini listelemek için bu cmdlet'i kullanın.

`Get-BlockchainMember [[-Name] <String>] -Members <IContract> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Name | Hakkında ayrıntıları almak istediğiniz Blockchain hizmet üyesinin adı. Bir adı girildiğinde, üyenin ayrıntılarını döndürür. Bir ad atlandığında, tüm consortium üyelerin listesi döndürür. | Hayır |
| Members | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
$ContractConnection | Get-BlockchainMember -Name <Member Name>
```

#### <a name="example-output"></a>Örnek çıktı

```
Name           : myblockchainmember
CorrelationId  : 0
DisplayName    : myCompany
SubscriptionId : <Azure subscription ID>
AccountAddress : 0x85b911c9e103d6405573151258d668479e9ebeef
Role           : ADMIN
```

### <a name="remove-blockchainmember"></a>Remove-BlockchainMember

Blok zinciri üye kaldırmak için bu cmdlet'i kullanın.

`Remove-BlockchainMember -Name <String> -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Name | Üye adı kaldırmak için | Evet |
| Members | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Account | İçeri aktarma-Web3Account alınan Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
$ContractConnection | Remove-BlockchainMember -Name <Member Name> -Web3Account $MemberAccount
```

### <a name="set-blockchainmember"></a>Set-BlockchainMember

Blok zinciri görünen adını ve consortium rolü dahil olmak üzere, üye öznitelikleri ayarlamak için bu cmdlet'i kullanın.

Consortium Yöneticiler ayarlayabilir **DisplayName** ve **rol** tüm üyeleri için. Consortium üyesi olan kullanıcı rolüyle yalnızca kendi üyenin görünen adını değiştirebilirsiniz.

`Set-BlockchainMember -Name <String> [-DisplayName <String>] [-AccountAddress <String>] [-Role <String>]
 -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| Name | Blok zinciri üyenin adı | Evet |
| DisplayName | Yeni görünen adı | Hayır |
| AccountAddress | Hesap adresi | Hayır |
| Members | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Account | İçeri aktarma-Web3Account alınan Web3Account nesnesi | Evet |
| Web3Client |  New-Web3Connection alınan Web3Client nesnesi| Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
$ContractConnection | Set-BlockchainMember -Name <Member Name> -DisplayName <Display name> -Web3Account $MemberAccount
```

## <a name="manage-the-consortium-members-invitations"></a>Consortium üyelerini davet yönetme

Consortium üyesi davet Yönetimi cmdlet'leri consortium üyelerini davet yönetmek için kullanın. Kullanılabilir eylemler consortium rolünüze bağlıdır.

### <a name="new-blockchainmemberinvitation"></a>Yeni BlockchainMemberInvitation

Consortium üyeler davet etmek için bu cmdlet'i kullanın.

`New-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Üye davet etmek için Azure abonelik kimliği | Evet |
| Role | Consortium rol. Değerler, yönetici veya kullanıcı olabilir. Yönetici consortium Yöneticisi rolüdür. Kullanıcı consortium üye rolüdür. | Evet |
| Members | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Account | İçeri aktarma-Web3Account alınan Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
$ContractConnection | New-BlockchainMemberInvitation -SubscriptionId <Azure Subscription ID> -Role USER -Web3Account $MemberAccount
```

### <a name="get-blockchainmemberinvitation"></a>Get-BlockchainMemberInvitation

Consortium üye davet durumu listelemek veya almak için bu cmdlet'i kullanın.

`Get-BlockchainMemberInvitation [[-SubscriptionId] <String>] -Members <IContract> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Üye davet etmek için Azure abonelik kimliği. Abonelik kimliği: Abonelik döndürür sağlanan davet ayrıntıları kimliğin durumunda. Abonelik kimliği atlanırsa, tüm üye davet listesini döndürür. | Hayır |
| Members | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
$ContractConnection | Get-BlockchainMemberInvitation – SubscriptionId <Azure subscription ID>
```

#### <a name="example-output"></a>Örnek çıktı

```
SubscriptionId                       Role CorrelationId
--------------                       ---- -------------
<Azure subscription ID>              USER             2
```

### <a name="remove-blockchainmemberinvitation"></a>Remove-BlockchainMemberInvitation

Consortium üye davet iptal etmek için bu cmdlet'i kullanın.

`Remove-BlockchainMemberInvitation -SubscriptionId <String> -Members <IContract> -Web3Account <IAccount>
 -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Üyenin iptal etmek için Azure abonelik kimliği | Evet |
| Members | İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Account | İçeri aktarma-Web3Account alınan Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
$ContractConnection | Remove-BlockchainMemberInvitation -SubscriptionId <Subscription ID> -Web3Account $MemberAccount
```

### <a name="set-blockchainmemberinvitation"></a>Set-BlockchainMemberInvitation

Ayarlamak için bu cmdlet'i kullanmak **rol** var olan bir davet için. Yalnızca Yöneticiler consortium davetleri değiştirebilirsiniz.

`Set-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parametre | Açıklama | Gerekli |
|-----------|-------------|:--------:|
| SubscriptionId | Üye davet etmek için Azure abonelik kimliği | Evet |
| Role | Davet için yeni consortium rolü. Değerleri **kullanıcı** veya **yönetici**. | Evet |
| Members |  İçeri aktarma-ConsortiumManagementContracts alınan üye nesnesi | Evet |
| Web3Account | İçeri aktarma-Web3Account alınan Web3Account nesnesi | Evet |
| Web3Client | New-Web3Connection alınan Web3Client nesnesi | Evet |

#### <a name="example"></a>Örnek

```powershell-interactive
$ContractConnection | Set-BlockchainMemberInvitation -SubscriptionId <Azure subscription ID> -Role USER -Web3Account $MemberAccount
```

## <a name="next-steps"></a>Sonraki adımlar

Consortia, üyeleri ve düğümleri hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Azure Blockchain hizmet consortium](consortium.md)
