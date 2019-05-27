---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 1f8f8d314a8bb37a08b3696f597b395a8a4beb8e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170633"
---
## <a name="sign-in-to-azure"></a>Oturum açın: Azure

`Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Aşağıdaki kod örneğini kullanarak bölgelerin listesini görüntüleyin ve kullanmak istediğinizi bulun. Bu örnekte **eastus** kullanılmıştır. Konumu bir değişkende depolayın ve tek bir yerde değiştirebilmek için değişkeni kullanın.

```powershell
Get-AzLocation | select Location
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Bir Azure kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
$resourceGroup = "myResourceGroup"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Kullanarak LRS çoğaltma ile standart, genel amaçlı depolama hesabı oluşturma [yeni AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount). Ardından kullanmak istediğiniz depolama hesabını tanımlayan depolama hesabı bağlamını alın. Depolama hesabında bir işlem gerçekleştirirken, kimlik bilgilerini tekrar tekrar geçirmek yerine bağlama başvurun. Yerel olarak yedekli depolama (LRS) ve blob şifrelemesi (varsayılan olarak etkindir) ile *mystorageaccount* adlı bir depolama hesabı oluşturmak için aşağıdaki örneği kullanın.

```powershell
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -SkuName Standard_LRS `
  -Location $location `

$ctx = $storageAccount.Context
```
