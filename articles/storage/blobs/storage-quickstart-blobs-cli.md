---
title: "Azure hızlı başlangıç - aktarımı nesneleri/Azure CLI kullanarak Azure Blob depolama biriminden | Microsoft Docs"
description: "Azure CLI kullanarak Azure Blob storage/gruptan nesneleri aktarmak hızlı bir şekilde öğrenin"
services: storage
documentationcenter: na
author: mmacy
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
ms.author: marsma
ms.openlocfilehash: c9b7e7a1fbc6b67821183ce31bdf2527de490c92
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-the-azure-cli"></a>Aktarım nesneleri/Azure CLI kullanarak Azure Blob depolama biriminden

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Karşıya yükleme ve verileri için ve Azure Blob depolama biriminden indirmek için Azure CLI kullanarak bu hızlı başlangıç ayrıntıları.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

BLOB'ları bir kapsayıcıya her zaman yüklenir. Kapsayıcılar, bilgisayarınızda dosyaları dizinlerde düzenlemek gibi gruplar BLOB düzenlemek izin verir.

[az storage container create](/cli/azure/storage/container#create) komutunu kullanarak blobları depolamak için bir kapsayıcı oluşturun.

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Bir blob karşıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. BLOB storage'da depolanan dosyaların çoğu blok blobları depolanır. Ekleme blobları, varolan değiştirme içeriği olmadan, ayarlamayı günlük veri varolan bir blob eklenmelidir olduğunda kullanılır. Sayfa blobları IaaS sanal makinelerinin VHD dosyalarını yedekler.

Bu örnekte, biz blob ile son adımda oluşturduğumuz kapsayıcı karşıya [az depolama blob karşıya yükleme](/cli/azure/storage/blob#upload) komutu.

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, aksi takdirde üzerine yazılacaktır. Devam etmeden önce istediğiniz sayıda dosyaları karşıya yükleme.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[az storage blob list](/cli/azure/storage/blob#list) komutuyla kapsayıcıdaki blobları listeleyin.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Blob indirme

Kullanım [az depolama blob yükleme](/cli/azure/storage/blob#download) daha önce yüklenen bir blob indirmek için komutu.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>AzCopy ile veri aktarımı

[AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) programıdır Azure depolama için yüksek performanslı kodlanabilir veri aktarımı için başka bir seçenek. AzCopy depolamaya ve Blob, dosya ve tablo depolamadan veri aktarmak için kullanabilirsiniz.

Hızlı bir örnek olarak, bir dosyayı karşıya yüklemeyi adlı için AzCopy komutunu aşağıda verilmiştir *dosyam.txt* için *mystoragecontainer* kapsayıcı.

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://mystorageaccount.blob.core.windows.net/mystoragecontainer \
    --dest-key <storage-account-access-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık herhangi bir kaynağa, kaynak grubunda gerekiyorsa, kaynak grubuyla silmek Bu hızlı başlangıç oluşturulan depolama hesabı da dahil olmak üzere [az grubu Sil](/cli/azure/group#delete) komutu.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç yerel disk ve Azure Blob depolamada kapsayıcı arasında dosyaları aktarmak nasıl öğrendiniz. Azure Storage blobları ile çalışma hakkında daha fazla bilgi edinmek için Azure Blob storage ile çalışma Öğreticisi devam edin.

> [!div class="nextstepaction"]
> [Nasıl yapılır: Blob Depolama işlemleri Azure CLI ile](storage-how-to-use-blobs-cli.md)
