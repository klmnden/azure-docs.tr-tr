---
title: "Azure hızlı başlangıç - PowerShell kullanarak depolama hesabı oluşturma | Microsoft Docs"
description: "Hızlı bir şekilde PowerShell ile yeni bir depolama hesabı oluşturmayı öğrenin"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/29/2017
ms.author: robinsh
ms.openlocfilehash: c9175cce0cb93e73009fb8d751e54f631603d482
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-using-powershell"></a>PowerShell kullanarak depolama hesabı oluşturma

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. PowerShell kullanarak bir Azure Storage hesabı oluşturmak için bu kılavuzu ayrıntıları. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu hızlı başlangıç, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```

Kullanmak istediğiniz konumu bilmiyorsanız, kullanılabilir konumlarını listeleyebilirsiniz. Liste görüntülendikten sonra kullanmak istediğiniz bir bulun. Bu örnekte **eastus**. Bu bir değişkende saklayın ve tek bir yerde değiştirebilmeniz için değişkeni kullanın.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

```powershell
# put resource group in a variable so you can use the same group name going forward
#   without hardcoding it repeatedly
$resourceGroup = "contoso-storage-accounts"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Genel amaçlı standart depolama hesabı oluşturma

Çeşitli türlerde depolama hesapları, hangi hizmet (BLOB'lar, dosyalar, tabloların veya kuyrukların) ve nasıl kullanılacak gittiği bağlı olarak vardır. Bu tablo olanakları gösterir.

|**Depolama hesabı türü**|**Genel amaçlı Standart**|**Genel amaçlı Premium**|**Blob depolama, sık erişimli ve seyrek erişimli erişim katmanları**|
|-----|-----|-----|-----|
|**Desteklenen hizmetler**| BLOB, dosya, tablo, kuyruk Hizmetleri | Blob Hizmeti | Blob Hizmeti|
|**Desteklenen blob türleri**|Blok blobları, sayfa blobları, ekleme blobları | Sayfa blobları | Blok blobları ve ek blobları|

Kullanmak [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) tüm dört hizmetler için kullanabileceğiniz bir genel amaçlı standart depolama hesabı oluşturmak için. Depolama hesabı adı *contosomvcstandard*ve yerel olarak yedekli depolama varsa ve şifreleme etkin blob şekilde yapılandırın.

```powershell
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "contosomvcstandard" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubunu kaldırmak için komut ve ilişkili tüm kaynakları. Bu durumda, oluşturduğunuz depolama hesabını kaldırır.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç, genel amaçlı standart depolama hesabı oluşturduğunuzu düşünün. Karşıya yükleme ve depolama hesabınızla BLOB'ları indirme öğrenmek için Blob Depolama Hızlı Başlangıç için devam edin.
> [!div class="nextstepaction"]
> [Aktarım nesneleri/PowerShell kullanarak Azure Blob depolama biriminden](../blobs/storage-quickstart-blobs-powershell.md)