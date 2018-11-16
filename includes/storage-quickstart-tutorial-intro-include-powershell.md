---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 027b370d2497822dcbd6f3958556357957f9e8f5
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51817884"
---
## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
```

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Aşağıdaki kod örneğini kullanarak bölgelerin listesini görüntüleyin ve kullanmak istediğinizi bulun. Bu örnekte **eastus** kullanılmıştır. Konumu bir değişkende depolayın ve tek bir yerde değiştirebilmek için değişkeni kullanın.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) kullanarak LRS ile standart bir genel amaçlı depolama hesabı oluşturun. Ardından kullanmak istediğiniz depolama hesabını tanımlayan depolama hesabı bağlamını alın. Depolama hesabında bir işlem gerçekleştirirken, kimlik bilgilerini tekrar tekrar geçirmek yerine bağlama başvurun. Yerel olarak yedekli depolama (LRS) ve blob şifrelemesi (varsayılan olarak etkindir) ile *mystorageaccount* adlı bir depolama hesabı oluşturmak için aşağıdaki örneği kullanın.

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -SkuName Standard_LRS `
  -Location $location `
  -Kind Storage

$ctx = $storageAccount.Context
```
