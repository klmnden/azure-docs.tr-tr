---
title: Azure Hızlı Başlangıç - Azure PowerShell kullanarak nesne depolamada blob oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta nesne (Blob) depolamada Azure PowerShell’i kullanırsınız. Sonra, Azure Depolama’ya blob yüklemek, blob indirmek ve bir kapsayıcıdaki blobları listelemek için PowerShell’i kullanırsınız.
services: storage
author: roygara
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 11/14/2018
ms.author: rogarana
ms.openlocfilehash: 13d28e43f9f712f5e597da8171ba9ebf4118bd49
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51712050"
---
# <a name="quickstart-upload-download-and-list-blobs-by-using-azure-powershell"></a>Hızlı Başlangıç: Azure PowerShell kullanarak blobları yükleme, indirme ve listeleme

Azure kaynaklarını oluşturmak ve yönetmek için Azure PowerShell modülünü kullanın. Azure kaynaklarını oluşturma ve yönetme işlemleri PowerShell komut satırından veya betiklerden gerçekleştirilebilir. Bu kılavuzda yerel disk ile Azure Blob depolama arasında dosyaları aktarmak için PowerShell kullanma hakkında bilgi sağlanır.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Bu hızlı başlangıç, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir. Bu, blob gruplarını bilgisayarınızdaki dosyaları klasörler halinde düzenlediğiniz gibi düzenleyebilmenizi sağlar.

Kapsayıcı adını ayarlayın ve ardından [New-AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer)’ı kullanarak kapsayıcıyı oluşturun. Dosyalara genel erişime izin vermek için izinleri `blob` olarak ayarlayın. Bu örnekteki kapsayıcı adı *quickstartblobs*’tur.

```powershell
$containerName = "quickstartblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. IaaS VM’lerini yedekleyen VHD dosyaları sayfa bloblarıdır. Ekleme bloblarını, bir dosyaya yazıp daha sonradan daha fazla bilgi eklemek istediğiniz durumlarda günlüğe kaydetmek için kullanın. Blob depolamada depolanan çoğu dosya blok blobudur. 

Bir dosyayı bir blok blobuna yüklemek için, bir kapsayıcı başvurusu alın ve bu kapsayıcıdaki blok blobuna bir başvuru alın. Blob başvurusunu aldıktan sonra, [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) kullanarak verileri karşıya yükleyebilirsiniz. Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, blob varsa blobun üzerine yazılır.

Aşağıdaki örneklerde yerel diskteki *D:\\_TestImages* klasöründen *Image001.jpg* ve *Image002.png* dosyaları az önce oluşturduğunuz kapsayıcıya yüklenmektedir.

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

Devam etmeden önce istediğiniz sayıda dosyayı karşıya yükleyin.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) kullanarak kapsayıcıdaki blobların listesini alın. Bu örnekte karşıya yüklenen blobların yalnızca adları gösterilmektedir.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Blob’ları indirme

Blobları yerel diskinize indirin. İndirmek istediğiniz her blobda, blobu indirmek için adı ayarlayın ve [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) çağrısı yapın.

Bu örnekte, bloblar yerel diskteki *D:\\_TestImages\Downloads* klasörüne indirilir. 

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

[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) yardımcı programı, Azure Depolama’ya yüksek performanslı betik oluşturulabilir veri aktarımı için diğer bir seçenektir. Blob, Dosya ve Tablo depolamaları arasında veri aktarmak için AzCopy kullanın.

Hızlı bir örnek olarak, bir PowerShell penceresinden *myfile.txt* adlı dosyayı *mystoragecontainer* kapsayıcısına yüklemek için AzCopy komutu aşağıda gösterilmiştir.

```PowerShell
./AzCopy `
    /Source:C:\myfolder `
    /Dest:https://mystorageaccount.blob.core.windows.net/mystoragecontainer `
    /DestKey:<storage-account-access-key> `
    /Pattern:"myfile.txt"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz tüm varlıkları kaldırın. Varlıkları kaldırmanın en kolay yolu kaynak grubunu silmektir. Kaynak grubunu kaldırdığınızda o grubun içindeki tüm kaynaklar da silinir. Aşağıdaki örnekte, kaynak grubu kaldırıldığında depolama hesabı ve kaynak grubunun kendisi de kaldırılır.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları yerel bir disk ile Azure Blob depolama arasında aktardınız. PowerShell'i kullanarak Blob depolama ile çalışma hakkında daha fazla bilgi edinmek için Azure PowerShell'i Azure Depolama'yla kullanma öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Depolama ile Azure PowerShell’i kullanma](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

### <a name="microsoft-azure-powershell-storage-cmdlets-reference"></a>Microsoft Azure PowerShell Depolama cmdlet’leri başvurusu

* [Depolama PowerShell cmdlet’leri](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini

* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
