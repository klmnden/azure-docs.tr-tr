---
title: "Azure hızlı başlangıç - aktarımı nesneleri/PowerShell kullanarak Azure Blob depolama biriminden | Microsoft Docs"
description: "Hızlı bir şekilde nesneleri için/PowerShell kullanarak Azure Blob storage aktarım öğrenin"
services: storage
documentationcenter: storage
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
ms.date: 07/19/2017
ms.author: robinsh
ms.openlocfilehash: 1a9941b21b92c70dd0a46ce2e4c75142e1786650
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-azure-powershell"></a>Aktarım nesneleri/Azure PowerShell kullanarak Azure Blob depolama biriminden

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Yerel disk ve Azure Blob Depolama arasında dosyaları aktarmak için PowerShell kullanarak bu kılavuzu ayrıntıları.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu hızlı başlangıç, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

BLOB'ları bir kapsayıcıya her zaman yüklenir. Bu dosyalarınızı bilgisayarınızda klasörlerde düzenleme gibi grupları BLOB düzenlemenizi sağlar.

Kapsayıcı adı ayarlayın, sonra kapsayıcı kullanarak oluşturduğunuz [yeni AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer), 'dosyaların genel erişime izin vermek için blob' izinleri ayarlama. Bu örnek kapsayıcı adı *quickstartblobs*.

```powershell
$containerName = "quickstartblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-to-the-container"></a>BLOB kapsayıcıya karşıya yükle

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Sayfa blobları Iaas sanal makineleri yedeklemek için kullanılan VHD dosyalarını var. Ekleme blobları gibi bir dosyaya yazmak ve daha fazla bilgi ekleme tutmak istediğiniz günlük için kullanılır. BLOB storage'da depolanan çoğu blok blobları dosyalarıdır. 

Bir dosyayı bir blok blobuna yüklemek için bir kapsayıcı başvurusu alın ve sonra bu kapsayıcıda blok blob başvurusu alın. Blob başvurusu edindiğinizde, veri kendisine kullanarak karşıya yükleyebilirsiniz [kümesi AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent). Bu işlem zaten mevcut değil veya zaten mevcut değilse bu raporun üzerine blob oluşturur.

Aşağıdaki örnekler Image001.jpg ve D: gelen Image002.png karşıya\\oluşturduğunuz kapsayıcısı yerel diskteki _TestImages klasör.

```powershell
# upload a file
Set-AzureStorageBlobContent -File "D:\_TestImages\Image001.jpg" `
  -Container $containerName `
  -Blob "Image001.jpg" `
  -Context $ctx 

# upload another file
Set-AzureStorageBlobContent -File "D:\_TestImages\Image002.png" `
  -Container $containerName `
  -Blob "Image002.png" `
  -Context $ctx
```

Devam etmeden önce istediğiniz sayıda dosyaları karşıya yükleme.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

BLOB'ları kullanarak kapsayıcı listesini almak [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob). Bu örnek yalnızca karşıya BLOB'ları adlarını gösterir.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Blob’ları indirme

BLOB'ları yerel diskinize indirin. Her blob indirilmesi çağrısı ve adını ayarlayın [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) blob indirmek için.

Bu örnek BLOB D: indirmeleri\\yerel diskteki _TestImages\Downloads. 

```powershell
# download first blob
Get-AzureStorageBlobContent -Blob "Image001.jpg" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 

# download another blob
Get-AzureStorageBlobContent -Blob "Image002.png" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 
```

## <a name="data-transfer-with-azcopy"></a>AzCopy ile veri aktarımı

[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) programıdır Azure depolama için yüksek performanslı kodlanabilir veri aktarımı için başka bir seçenek. AzCopy depolamaya ve Blob, dosya ve tablo depolamadan veri aktarmak için kullanabilirsiniz.

Hızlı bir örnek olarak, bir dosyayı karşıya yüklemeyi adlı için AzCopy komutunu aşağıda verilmiştir *dosyam.txt* için *mystoragecontainer* kapsayıcıdan bir PowerShell penceresi içinde.

```PowerShell
./AzCopy `
    /Source:C:\myfolder `
    /Dest:https://mystorageaccount.blob.core.windows.net/mystoragecontainer `
    /DestKey:<storage-account-access-key> `
    /Pattern:"myfile.txt"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tüm oluşturduğunuz ve varlıkları kaldırın. Bunu yapmanın en kolay yolu, kaynak grubu silmektir. Bu grup içinde bulunan tüm kaynaklar da siler. Bu durumda, depolama hesabı ve kaynak grubu kaldırır.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç, bir yerel disk ve Azure Blob Depolama arasında dosyaları aktarmak nasıl öğrendiniz. Blob storage ile çalışma hakkında daha fazla bilgi için nasıl yapılır Blob depolama alanına devam edin.

> [!div class="nextstepaction"]
> [BLOB Depolama işlemleri nasıl yapılır konuları](storage-how-to-use-blobs-powershell.md)

### <a name="microsoft-azure-powershell-storage-cmdlets-reference"></a>Microsoft Azure PowerShell depolama cmdlet'leri başvurusu
* [Depolama PowerShell cmdlet'leri](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini
* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.