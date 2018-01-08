---
title: "Azure Hızlı Başlangıç - PowerShell kullanarak depolama hesabı oluşturma | Microsoft Docs"
description: "PowerShell ile hızlı bir şekilde yeni bir depolama hesabı oluşturmayı öğrenin"
services: storage
documentationcenter: 
author: tamram
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
ms.author: tamram
ms.openlocfilehash: 87f179d2cf3802af3eca58e0adf57b07069bfcc6
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="create-a-storage-account-using-powershell"></a>PowerShell kullanarak depolama hesabı oluşturma

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu kılavuzda bir Azure Depolama hesabı oluşturmak için PowerShell kullanma ayrıntılı bir şekilde açıklanmaktadır. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu hızlı başlangıç, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Liste görüntülendikten sonra, kullanmak istediğiniz öğeyi bulun. Bu örnekte **eastus** kullanılmıştır. Bunu bir değişkende depolayın ve tek bir yerde değiştirebilmek için değişkeni kullanın.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

```powershell
# put resource group in a variable so you can use the same group name going forward
#   without hardcoding it repeatedly
$resourceGroup = "contoso-storage-accounts"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-general-purpose-storage-account"></a>Genel amaçlı depolama hesabı oluşturma

Nasıl ve hangi hizmet için (blob, dosya, tablo veya kuyruk) kullanılacağına bağlı olarak birkaç depolama hesabı türü bulunur. Bu tabloda olasılıklar gösterilmektedir.

|**Depolama hesabı türü**|**Genel amaçlı Standart**|**Genel amaçlı Premium**|**Blob depolama, sık erişimli ve seyrek erişimli erişim katmanları**|
|-----|-----|-----|-----|
|**Desteklenen hizmetler**| Blob, Dosya, Tablo, Kuyruk hizmetleri | Blob hizmeti | Blob hizmeti|
|**Desteklenen blob türleri**|Blok blobları, sayfa blobları, ek bloblar | Sayfa blobları | Blok blobları ve ek blobları|

Dört hizmetin tümü için kullanabileceğiniz bir genel amaçlı depolama hesabı oluşturmak için [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) komutunu kullanın. Depolama hesabını *contosomvcstandard* olarak adlandırın ve Yerel Olarak Yedekli Depolama ve blob şifrelemesi etkin olarak yapılandırın.

```powershell
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "contosomvcstandard" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz. Bu durumda, oluşturduğunuz depolama hesabı kaldırılır.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, genel amaçlı bir standart depolama hesabı oluşturdunuz. Depolama hesabınız ile blobları karşıya yüklemeyi ve indirmeyi öğrenmek için, Blob depolama hızlı başlangıcı ile devam edin.
> [!div class="nextstepaction"]
> [PowerShell kullanarak nesneleri Azure Blob depolama içine/dışına aktarma](../blobs/storage-quickstart-blobs-powershell.md)