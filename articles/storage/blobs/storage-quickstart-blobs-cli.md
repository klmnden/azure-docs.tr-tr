---
title: "Azure Hızlı Başlangıç - Azure CLI kullanarak nesneleri Azure Blob depolama içine/dışına aktarma | Microsoft Docs"
description: "Hızlı bir şekilde Azure CLI kullanarak nesneleri Azure Blob depolama içine/dışına aktarmayı öğrenin"
services: storage
documentationcenter: na
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
ms.openlocfilehash: 7313df35baadf7aa6d476f44b113dc60e6845f4b
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-the-azure-cli"></a>Azure CLI kullanarak nesneleri Azure Blob depolama içine/dışına aktarma

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıç Azure Blob depolamaya veri yüklemek ve verileri indirmek için Azure CLI kullanma hakkında ayrıntılı bilgiler sağlar.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir. Bu, blob gruplarını bilgisayarınızdaki dosyaları klasörler halinde düzenlediğiniz gibi düzenleyebilmenizi sağlar.

[az storage container create](/cli/azure/storage/container#create) komutunu kullanarak blobları depolamak için bir kapsayıcı oluşturun.

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Blobu karşıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blob depolamada depolanan çoğu dosya blok blobu olarak depolanır. Ekleme blobları, verilerin mevcut içeriği değiştirmeden mevcut bir bloba eklenmesi gerektiğinde (örneğin günlüğe kaydetme için) kullanılır. Sayfa blobları IaaS sanal makinelerinin VHD dosyalarını yedekler.

İlk olarak blob yüklemek için bir dosya oluşturun.
Azure Cloud Shell kullanıyorsanız dosya oluşturmak için aşağıdaki adımları uygulayın: `vi helloworld` dosya açıldığında **ekle**'ye basın, "Hello world" yazın ve **Esc**'ye basıp `:x` girin ve **Enter**'a basın.

Bu örnekte, son adımda [az storage blob upload](/cli/azure/storage/blob#upload) komutuyla oluşturduğunuz kapsayıcıya bir blob yükleyeceksiniz.

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Azure Cloud Shell'de dosya oluşturmak için yukarıda anlatılan yöntemi kullandıysanız bu CLI komutunu kullanabilirsiniz (dosya temel dizinde oluşturulduğu için yol belirtmeniz gerekmediğine dikkat edin, normalde yol belirtmeniz gerekir):

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name helloworld
    --file helloworld
```

Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, aksi takdirde üzerine yazılacaktır. Devam etmeden önce istediğiniz sayıda dosyayı karşıya yükleyin.

Aynı anda birden fazla dosya yüklemek için [az storage blob upload-batch](/cli/azure/storage/blob#upload-batch) komutunu kullanabilirsiniz.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[az storage blob list](/cli/azure/storage/blob#list) komutuyla kapsayıcıdaki blobları listeleyin.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Blob indirme

Önceden karşıya yüklediğiniz bir blobu indirmek için [az storage blob download](/cli/azure/storage/blob#download) komutunu kullanın.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>AzCopy ile veri aktarımı

[AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) yardımcı programı, Azure Depolama’ya yüksek performanslı betik oluşturulabilir veri aktarımı için diğer bir seçenektir. Blob, Dosya ve Tablo depolamaları arasında veri aktarmak için AzCopy kullanabilirsiniz.

Hızlı bir örnek olarak, *myfile.txt* adlı dosyayı *mystoragecontainer* kapsayıcısına yüklemek için AzCopy komutu aşağıdadır.

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://mystorageaccount.blob.core.windows.net/mystoragecontainer \
    --dest-key <storage-account-access-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu Hızlı Başlangıç öğreticisinde oluşturduğunuz depolama hesabı dahil, kaynak grubunuzdaki hiçbir kaynağa artık ihtiyacınız yoksa [az group delete](/cli/azure/group#delete) komutuyla kaynak grubunu silin.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç’ta, dosyaları yerel bir disk ve Azure Blob depolamadaki bir kapsayıcı arasında aktarmayı öğrendiniz. Azure Depolama’da bloblarla çalışma hakkında daha fazla bilgi edinmek için, Azure Blob depolamayla çalışma hakkındaki öğretici ile devam edin.

> [!div class="nextstepaction"]
> [Nasıl yapılır: Azure CLI ile blob depolama işlemleri](storage-how-to-use-blobs-cli.md)
