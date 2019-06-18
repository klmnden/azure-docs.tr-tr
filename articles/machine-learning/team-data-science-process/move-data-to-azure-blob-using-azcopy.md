---
title: AzCopy - Team Data Science Process ile BLOB Depolama veri kopyalama
description: AzCopy kullanarak Azure Blob Depolama ve veri kopyalamak
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 6c0951eb6ad3b7651da97e1a49c5edf5ab55a199
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61044371"
---
# <a name="copy-data-to-and-from-azure-blob-storage-using-azcopy"></a>AzCopy kullanarak Azure Blob Depolama ve veri kopyalamak
AzCopy, karşıya yükleme, indirme ve veri kopyalamak için ve Microsoft Azure blob, dosya ve tablo depolama için tasarlanan bir komut satırı yardımcı programıdır.

AzCopy ve ek bilgiler Azure platformu üzerinden yükleme ile ilgili yönergeler için bkz: [AzCopy komut satırı yardımcı programı ile çalışmaya başlama](../../storage/common/storage-use-azcopy.md).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Tarafından sağlanan betikleri ile oluşturulan VM kullanıyorsanız [azure'da veri bilimi sanal makineleri](virtual-machines.md), daha sonra AzCopy VM üzerinde zaten yüklü.
> 
> [!NOTE]
> Azure blob depolama için bir tam giriş için başvurmak [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu belge, bir Azure aboneliği, bir depolama hesabı ve karşılık gelen depolama anahtarını ilgili hesabın sahibi olduğunuzu varsayar. Karşıya yükleme/veri indirmeden önce Azure depolama hesabı adını ve hesap anahtarınızı bilmeniz gerekir.

* Bir Azure aboneliğini ayarlama hakkında bilgi için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Bir depolama hesabı oluşturma hakkında yönergeler için ve hesap ve anahtar bilgilerini almak için bkz: [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md).

## <a name="run-azcopy-commands"></a>AzCopy komutları çalıştırın
AzCopy komutları çalıştırmak için bir komut penceresi açın ve yürütülebilir AzCopy.exe bulunduğu bilgisayarınız üzerinde AzCopy yükleme dizinine gidin. 

AzCopy komutları için temel sözdizimi aşağıdaki gibidir:

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> AzCopy yükleme konumu, sistem yoluna ekleyin ve ardından herhangi bir dizinden komutları çalıştırın. AzCopy varsayılan olarak, yüklü *% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* veya *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.
> 
> 

## <a name="upload-files-to-an-azure-blob"></a>Bir Azure blobuna karşıya dosya yükleme
Bir dosyayı karşıya yüklemek için aşağıdaki komutu kullanın:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Azure blobundan dosyaları indirme
Azure blobundan bir dosyayı indirmek için aşağıdaki komutu kullanın:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="copy-blobs-between-azure-containers"></a>Azure kapsayıcılar arasında blobları kopyalama
Azure kapsayıcılar arasında BLOB'ları kopyalamak için aşağıdaki komutu kullanın:

    # Copying blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be copied. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>AzCopy kullanarak ipuçları
> [!TIP]
> 1. Zaman **karşıya** dosyaları */S* dosyalarını yinelemeli olarak yükler. Bu parametre olmadan dizinlerindeki dosyaları karşıya yüklenmemiş.  
> 2. Zaman **indirme** dosyası */S* belirtilen dizinde ve alt dizinlerinde tüm dosyaları veya belirtilen dizinde belirtilen desenle eşleşen tüm dosyaları kadar kapsayıcı yinelemeli olarak arar. ve alt dizinlerinde indirilir.  
> 3. Belirtemezsiniz bir **bloba dosya** kullanarak indirmek için */Source* parametresi. Belirli bir dosyayı indirmek için kullanarak indirmek için blob dosya adı belirtin. */desen* parametresi. **/S** parametresi, bir dosya adı deseni için öz yinelemeli olarak ara AzCopy sağlamak için kullanılabilir. Düzen parametresi olmadan, AzCopy Bu dizindeki tüm dosyaları indirir.
> 
> 

