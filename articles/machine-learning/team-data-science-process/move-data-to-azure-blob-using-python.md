---
title: "Python kullanarak Azure Blob Storage gelen ve veri taşıma | Microsoft Docs"
description: "Python kullanarak Azure Blob Depolamadan/Depolamaya Veri Taşıma"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 24276252-b3dd-4edf-9e5d-f6803f8ccccc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: d7847f695a77ad469f56a20518cb979c41384d1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Python kullanarak Azure Blob Storage gelen ve veri taşıma
Bu konu, listelemek, karşıya yükleme ve indirme Python API'yi kullanarak BLOB'ları açıklar. Python Azure SDK'SINDA sağlanan API'si ile şunları yapabilirsiniz:

* Bir kapsayıcı oluşturma
* Bir kapsayıcıya bir blob yükleme
* Blob’ları indirme
* Blob’ları bir kapsayıcıda listeleme
* Blob silme

Python API kullanımı hakkında daha fazla bilgi için bkz: [python'dan Blob Depolama hizmetini kullanmayı](../../storage/blobs/storage-python-how-to-use-blob-storage.md).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Tarafından sağlanan kodlarıyla ayarlanan VM kullanıyorsanız [veri bilimi sanal makinelerle](virtual-machines.md), sonra da AzCopy VM üzerinde zaten yüklü.
> 
> [!NOTE]
> Azure blob depolama tam bir giriş için bkz [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu belge, Azure aboneliği, bir depolama hesabı ve o hesap için karşılık gelen depolama anahtarı sahip olduğunuzu varsayar. Karşıya yükleme/veri yüklemeden önce Azure depolama hesabı adı ve hesap anahtarınızın bilmeniz gerekir.

* Bir Azure aboneliği ayarlamak için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Bir depolama hesabı oluşturma hakkında yönergeler ve hesabı ve anahtarı bilgilerini almak için bkz: [Azure storage hesapları hakkında](../../storage/common/storage-create-storage-account.md).

## <a name="upload-data-to-blob"></a>Veri Blobuna yükle
İlk Azure Storage programlı olarak erişmek istediğiniz tüm Python kodu, aşağıdaki kod parçacığını ekleyin:

    from azure.storage.blob import BlobService

**BlobService** nesne kapsayıcıları ve blob'larla çalışmanıza olanak sağlar. Aşağıdaki kod depolama hesabı adı ve hesap anahtarı kullanarak bir BlobService nesnesi oluşturur. Hesap adı ve hesap anahtarı gerçek hesabı ve anahtarı ile değiştirin.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Bir blob verileri yüklemek için aşağıdaki yöntemleri kullanın:

1. PUT\_blok\_blob\_gelen\_(yükler, belirtilen yol bir dosyanın içeriğini) yolu
2. PUT\_block_blob\_gelen\_dosyası (zaten açılmış bir dosya/akışı içeriğini yükler)
3. PUT\_blok\_blob\_gelen\_bayt (karşıya bir bayt dizisi)
4. PUT\_blok\_blob\_gelen\_metin (belirtilen kodlama kullanılarak belirtilen metin değeri yükler)

Aşağıdaki örnek kod bir kapsayıcıya yerel bir dosya yükler:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Aşağıdaki örnek kod, blob depolama için yerel bir dizinde (dizinleri hariç) tüm dosyaları yükler:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Veri Blobundan indirin
Bir blob üzerinden veri indirmek için aşağıdaki yöntemleri kullanın:

1. Alma\_blob\_için\_yolu
2. Alma\_blob\_için\_dosyası
3. Alma\_blob\_için\_bayt
4. Alma\_blob\_için\_metin

Veri boyutu 64 MB aştığında gerekli Öbekleme gerçekleştirmek bu yöntemleri.

Aşağıdaki örnek kod, yerel bir dosyaya bir blob bir kapsayıcıda içeriğini indirir:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Aşağıdaki örnek kod tüm BLOB bir kapsayıcı yükler. Liste kullanan\_kapsayıcısında kullanılabilir BLOB'lar listesini almak için BLOB'ların ve yerel bir dizine indirir.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
