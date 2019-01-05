---
title: Azure Hızlı Başlangıç - Azure PowerShell kullanarak nesne depolamada blob oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta nesne (Blob) depolamada Azure PowerShell’i kullanırsınız. Sonra, Azure Depolama’ya blob yüklemek, blob indirmek ve bir kapsayıcıdaki blobları listelemek için PowerShell’i kullanırsınız.
services: storage
author: roygara
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: rogarana
ms.openlocfilehash: f85d404df37d34f7363114fbbf34ceec3bbe7c0f
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54042810"
---
# <a name="quickstart-upload-download-and-list-blobs-by-using-azure-powershell"></a>Hızlı Başlangıç: Karşıya yükleme, indirme ve Azure PowerShell kullanarak blobları Listele

Azure kaynaklarını oluşturmak ve yönetmek için Azure PowerShell modülünü kullanın. Azure kaynaklarını oluşturma ve yönetme işlemleri PowerShell komut satırından veya betiklerden gerçekleştirilebilir. Bu kılavuzda yerel disk ile Azure Blob depolama arasında dosyaları aktarmak için PowerShell kullanma hakkında bilgi sağlanır.

## <a name="prerequisites"></a>Önkoşullar

Azure depolamaya erişmek için bir Azure aboneliği gerekir. Bir aboneliğiniz zaten yoksa, oluşturup bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu hızlı başlangıçta, Azure PowerShell modülü Az 0.7 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir. Bu, blob gruplarını bilgisayarınızdaki dosyaları klasörler halinde düzenlediğiniz gibi düzenleyebilmenizi sağlar.

Kapsayıcı adını ayarlayın ve ardından [New-AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer)’ı kullanarak kapsayıcıyı oluşturun. Dosyalara genel erişime izin vermek için izinleri `blob` olarak ayarlayın. Bu örnekteki kapsayıcı adı *quickstartblobs*’tur.

```powershell
$containerName = "quickstartblobs"
new-azurestoragecontainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. IaaS VM’lerini yedekleyen VHD dosyaları sayfa bloblarıdır. Ekleme bloblarını, bir dosyaya yazıp daha sonradan daha fazla bilgi eklemek istediğiniz durumlarda günlüğe kaydetmek için kullanın. Blob depolamada depolanan çoğu dosya blok blobudur. 

Bir dosyayı bir blok blobuna yüklemek için, bir kapsayıcı başvurusu alın ve bu kapsayıcıdaki blok blobuna bir başvuru alın. Blob başvurusunu aldıktan sonra verileri için kullanarak karşıya yükleyebilirsiniz [set-azurestorageblobcontent](/powershell/module/azure.storage/set-azurestorageblobcontent). Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, blob varsa blobun üzerine yazılır.

Aşağıdaki örneklerde yerel diskteki *D:\\_TestImages* klasöründen *Image001.jpg* ve *Image002.png* dosyaları az önce oluşturduğunuz kapsayıcıya yüklenmektedir.

```powershell
# upload a file
set-azurestorageblobcontent -File "D:\_TestImages\Image001.jpg" `
  -Container $containerName `
  -Blob "Image001.jpg" `
  -Context $ctx 

# upload another file
set-azurestorageblobcontent -File "D:\_TestImages\Image002.png" `
  -Container $containerName `
  -Blob "Image002.png" `
  -Context $ctx
```

Devam etmeden önce istediğiniz sayıda dosyayı karşıya yükleyin.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

Kullanarak kapsayıcıda BLOB listesini alma [get-azurestorageblob](/powershell/module/azure.storage/get-azurestorageblob). Bu örnekte karşıya yüklenen blobların yalnızca adları gösterilmektedir.

```powershell
get-azurestorageblob -Container $ContainerName -Context $ctx | select Name
```

## <a name="download-blobs"></a>Blob’ları indirme

Blobları yerel diskinize indirin. İndirmek istediğiniz her bir blob kümesi için ad ve çağrı [get-azurestorageblobcontent](/powershell/module/azure.storage/get-azurestorageblobcontent) blobu indirmek için.

Bu örnekte, bloblar yerel diskteki *D:\\_TestImages\Downloads* klasörüne indirilir. 

```powershell
# download first blob
get-azurestorageblobcontent -Blob "Image001.jpg" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 

# download another blob
get-azurestorageblobcontent -Blob "Image002.png" `
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

* [Depolama PowerShell cmdlet’leri](/powershell/module/az.storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini

* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
