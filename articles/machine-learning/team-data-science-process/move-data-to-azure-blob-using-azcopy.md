---
title: AzCopy kullanarak Azure Blob Storage gelen ve veri taşıma | Microsoft Docs
description: AzCopy kullanarak Azure Blob Depolamadan/Depolamaya Veri Taşıma
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: c309ceb2-0e83-4a07-b16d-c997dcd62d5c
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: a25a35bc05781c6a52e21d697233ba1187ebeccf
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34838424"
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>AzCopy kullanarak Azure Blob Storage gelen ve veri taşıma
AzCopy karşıya yükleme, indirme ve veri kopyalamayı için ve Microsoft Azure blob, dosya ve tablo depolama için tasarlanmış bir komut satırı yardımcı programıdır.

Azure platformu kullanarak üzerinde AzCopy ve ek bilgiler yükleme ile ilgili yönergeler için bkz: [AzCopy komut satırı yardımcı programı ile çalışmaya başlama](../../storage/common/storage-use-azcopy.md).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Tarafından sağlanan kodlarıyla ayarlanan VM kullanıyorsanız [veri bilimi sanal makinelerle](virtual-machines.md), sonra da AzCopy VM üzerinde zaten yüklü.
> 
> [!NOTE]
> Azure blob depolama tam bir giriş için bkz [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu belge, Azure aboneliği, bir depolama hesabı ve o hesap için karşılık gelen depolama anahtarı sahip olduğunuzu varsayar. Karşıya yükleme/veri yüklemeden önce Azure depolama hesabı adı ve hesap anahtarınızın bilmeniz gerekir.

* Bir Azure aboneliği ayarlamak için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Bir depolama hesabı oluşturma hakkında yönergeler ve hesabı ve anahtarı bilgilerini almak için bkz: [Azure storage hesapları hakkında](../../storage/common/storage-create-storage-account.md).

## <a name="run-azcopy-commands"></a>AzCopy komutlarını çalıştırın
AzCopy komutları çalıştırmak için bir komut penceresi açın ve yürütülebilir AzCopy.exe bulunduğu bilgisayarınızda AzCopy yükleme dizinine gidin. 

AzCopy komutları temel sözdizimi aşağıdaki gibidir:

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> AzCopy yükleme konumu, sistem yoluna ekleyin ve ardından herhangi bir dizinden komutları çalıştırın. AzCopy varsayılan olarak, yüklü *% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* veya *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.
> 
> 

## <a name="upload-files-to-an-azure-blob"></a>Dosyaları bir Azure blobuna yükle
Bir dosyayı karşıya yüklemek için aşağıdaki komutu kullanın:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Bir Azure blob üzerinden dosyaları indirme
Bir Azure blob üzerinden bir dosyayı indirmek için aşağıdaki komutu kullanın:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Azure kapsayıcı arasında aktarım BLOB'ları
BLOB'ları Azure kapsayıcıları arasında aktarmak için aşağıdaki komutu kullanın:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>AzCopy kullanma ipuçları
> [!TIP]
> 1. Zaman **karşıya** dosyaları */S* dosyaları yinelemeli olarak yükler. Bu parametre olmadan dizinlerindeki dosyaları karşıya değil.  
> 2. Zaman **indirme** dosyası */S* tüm dosyaları belirtilen dizinde ve alt dizinlerinde veya belirtilen dizinde ve alt dizinlerinde belirtilen desenle eşleşen, indirilen tüm dosyaları kadar kapsayıcısını özyinelemeli olarak arar.  
> 3. Belirtemeyeceğiniz bir **belirli blob dosya** kullanarak indirmek için */Source* parametresi. Belirli bir dosyayı indirmek için kullanarak indirmek için blob dosya adını belirtin. */desen* parametresi. **/S** parametresi için bir dosya adı deseni yinelemeli Ara AzCopy sağlamak için kullanılabilir. Desen parametre olmadan, AzCopy dizindeki tüm dosyaları indirir.
> 
> 

