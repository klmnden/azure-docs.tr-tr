---
title: Azure Hızlı Başlangıç - Azure CLI kullanarak nesne depolamada blob oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta nesne (Blob) depolamada Azure CLI’yi kullanırsınız. Sonra, Azure Depolama’ya blob yüklemek, blob indirmek ve bir kapsayıcıdaki blobları listelemek için CLI’yi kullanırsınız.
services: storage
author: roygara
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 11/14/2018
ms.author: rogarana
ms.openlocfilehash: 11bd639d86c6ad9a9f373ac26dc271817bc46b08
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60392810"
---
# <a name="quickstart-upload-download-and-list-blobs-using-the-azure-cli"></a>Hızlı Başlangıç: Karşıya yükleme, indirme ve Azure CLI kullanarak blobları Listele

Azure CLI, Azure kaynaklarını yönetmek için Azure tarafından sunulan komut satırı deneyimidir. Azure Cloud Shell ile kendi tarayıcınızda da kullanabilirsiniz. Dilerseniz macOS, Linux veya Windows’a yükleyebilir ve komut satırından çalıştırabilirsiniz. Bu hızlı başlangıçta Azure Blob depolamaya veri yüklemek ve verileri indirmek için Azure CLI kullanma hakkında ayrıntılı bilgiler öğrenirsiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümünüzü belirlemek için `az --version` çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir. Bu, blob gruplarını bilgisayarınızdaki dosyaları klasörler halinde düzenlediğiniz gibi düzenleyebilmenizi sağlar.

[az storage container create](/cli/azure/storage/container) komutunu kullanarak blobları depolamak için bir kapsayıcı oluşturun.

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Blobu karşıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blob depolamada depolanan çoğu dosya blok blobu olarak depolanır. Ekleme blobları, verilerin mevcut içeriği değiştirmeden mevcut bir bloba eklenmesi gerektiğinde (örneğin günlüğe kaydetme için) kullanılır. Sayfa blobları IaaS sanal makinelerinin VHD dosyalarını yedekler.

İlk olarak blob yüklemek için bir dosya oluşturun.
Azure Cloud Shell kullanıyorsanız dosya oluşturmak için aşağıdaki adımları uygulayın: `vi helloworld` dosya açıldığında **ekle**'ye basın, "Hello world" yazın ve **Esc**'ye basıp `:x` girin ve **Enter**'a basın.

Bu örnekte, son adımda [az storage blob upload](/cli/azure/storage/blob) komutuyla oluşturduğunuz kapsayıcıya bir blob yükleyeceksiniz.

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
    --name helloworld \
    --file helloworld
```

Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, aksi takdirde üzerine yazılacaktır. Devam etmeden önce istediğiniz sayıda dosyayı karşıya yükleyin.

Aynı anda birden fazla dosya yüklemek için [az storage blob upload-batch](/cli/azure/storage/blob) komutunu kullanabilirsiniz.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[az storage blob list](/cli/azure/storage/blob) komutuyla kapsayıcıdaki blobları listeleyin.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Blob indirme

Önceden karşıya yüklediğiniz bir blobu indirmek için [az storage blob download](/cli/azure/storage/blob) komutunu kullanın.

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

Bu Hızlı Başlangıç öğreticisinde oluşturduğunuz depolama hesabı dahil, kaynak grubunuzdaki hiçbir kaynağa artık ihtiyacınız yoksa [az group delete](/cli/azure/group) komutuyla kaynak grubunu silin.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç’ta, dosyaları yerel bir disk ve Azure Blob depolamadaki bir kapsayıcı arasında aktarmayı öğrendiniz. Azure Depolama’da bloblarla çalışma hakkında daha fazla bilgi edinmek için, Azure Blob depolamayla çalışma hakkındaki öğretici ile devam edin.

> [!div class="nextstepaction"]
> [Nasıl yapılır: Azure CLI ile BLOB Depolama işlemleri](storage-how-to-use-blobs-cli.md)
