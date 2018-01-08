---
title: "Azure Hızlı Başlangıç - PowerShell kullanarak nesneleri Azure Blob depolama içine/dışına aktarma | Microsoft Docs"
description: "Hızlı bir şekilde PowerShell kullanarak nesneleri Azure Blob depolama içine/dışına aktarmayı öğrenin"
services: storage
documentationcenter: storage
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
ms.date: 07/19/2017
ms.author: tamram
ms.openlocfilehash: bbc0de7b3a63e8b541a6425e0c7fef9a72dfdffc
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-azure-powershell"></a>Azure PowerShell kullanarak nesneleri Azure Blob depolama içine/dışına aktarma

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu kılavuzda yerel disk ile Azure Blob depolama arasında dosyaları aktarmak için PowerShell kullanma hakkında ayrıntılı bilgiler sağlanmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu hızlı başlangıç, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir. Bu, blob gruplarını bilgisayarınızdaki dosyaları klasörler halinde düzenlediğiniz gibi düzenleyebilmenizi sağlar.

Kapsayıcı adını ayarlayın, daha sonra [New-AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer) kullanarak kapsayıcıyı oluşturun ve dosyalara genel erişime izin vermek için izinleri 'blob' olarak ayarlayın. Bu örnekteki kapsayıcı adı *quickstartblobs*’tur.

```powershell
$containerName = "quickstartblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. IaaS VM’lerini yedeklemek için kullanılan VHD dosyaları sayfa bloblarıdır. Ekleme blobları, bir dosyaya yazıp daha sonradan daha fazla bilgi eklemek istediğiniz durumlarda günlüğe kaydetme için kullanılır. Blob depolamada depolanan çoğu dosya blok blobudur. 

Bir dosyayı bir blok blobuna yüklemek için, bir kapsayıcı başvurusu alın ve bu kapsayıcıdaki blok blobuna bir başvuru alın. Blob başvurusunu aldıktan sonra, [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) kullanarak verileri karşıya yükleyebilirsiniz. Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, blob zaten varsa blobun üzerine yazılır.

Aşağıdaki örneklerde yerel diskteki D:\\_TestImages klasöründen Image001.jpg ve Image002.png dosyaları az önce oluşturduğunuz kapsayıcıya yüklenmektedir.

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

[Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) kullanarak kapsayıcıdaki blobların bir listesini alın. Bu örnekte karşıya yüklenen blobların yalnızca adları gösterilmektedir.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Blob’ları indirme

Blobları yerel diskinize indirin. İndirilecek her blob için, blobu indirmek için adı ayarlayın ve [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) öğesini çağırın.

Bu örnekte, bloblar yerel diskteki D:\\_TestImages\Downloads klasörüne indirilmektedir. 

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

[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) yardımcı programı, Azure Depolama’ya yüksek performanslı betik oluşturulabilir veri aktarımı için diğer bir seçenektir. Blob, Dosya ve Tablo depolamaları arasında veri aktarmak için AzCopy kullanabilirsiniz.

Hızlı bir örnek olarak, bir PowerShell penceresinden *myfile.txt* adlı dosyayı *mystoragecontainer* kapsayıcısına yüklemek için AzCopy komutu aşağıdadır.

```PowerShell
./AzCopy `
    /Source:C:\myfolder `
    /Dest:https://mystorageaccount.blob.core.windows.net/mystoragecontainer `
    /DestKey:<storage-account-access-key> `
    /Pattern:"myfile.txt"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz tüm varlıkları kaldırın. Bunu yapmanın en kolay yolu kaynak grubunu silmektir. Bu, ayrıca grubun içerdiği tüm kaynakları da siler. Bu durumda, depolama hesabı ve kaynak grubu kaldırılır.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları yerel bir disk ve Azure Blob depolama arasında aktarmayı öğrendiniz. Blob depolamayla çalışma hakkında daha fazla bilgi edinmek için, Blob depolama nasıl yapılır öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Blob Depolama İşlemleri Nasıl Yapılır](storage-how-to-use-blobs-powershell.md)

### <a name="microsoft-azure-powershell-storage-cmdlets-reference"></a>Microsoft Azure PowerShell Depolama cmdlet’leri başvurusu
* [Depolama PowerShell cmdlet’leri](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini
* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.