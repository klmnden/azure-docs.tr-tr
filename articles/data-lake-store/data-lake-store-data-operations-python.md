---
title: 'Python: Azure Data Lake depolama Gen1 gerçekleştirilen dosya sistemi işlemleri | Microsoft Docs'
description: Data Lake depolama Gen1 dosya sistemiyle çalışmak için Python SDK'sını kullanmayı öğrenin.
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 57efc718a51398b577a0078ba829d2f6209cab54
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58880755"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-python"></a>Dosya sistemi işlemlerini Python kullanarak Azure Data Lake depolama Gen1
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Bu makalede, Azure Data Lake depolama Gen1 dosya sistemi işlemlerini gerçekleştirmek için Python SDK'sını kullanmayı öğrenin. Data Lake depolama Gen1 hesap yönetim işlemlerini Python kullanarak gerçekleştirme talimatları için bkz. [hesap yönetim işlemlerini Python kullanarak Data Lake depolama Gen1](data-lake-store-get-started-python.md).

## <a name="prerequisites"></a>Önkoşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.6.2 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake depolama Gen1 hesabı**. Konumundaki yönergeleri [Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](data-lake-store-get-started-portal.md).

## <a name="install-the-modules"></a>Modülleri yükleme

Data Lake depolama Gen1 ile çalışmak için Python'ı kullanarak, üç modül yüklemeniz gerekir.

* `azure-mgmt-resource` modülü, Active Directory gibi şeyler için Azure modüllerini içerir.
* `azure-mgmt-datalake-store` Modülü Azure Data Lake depolama Gen1 hesap yönetim işlemlerini içerir. Bu modül hakkında daha fazla bilgi için bkz. [azure-mgmt-datalake-store modülü başvurusu](https://docs.microsoft.com/python/api/azure.mgmt.datalake.store?view=azure-python).
* `azure-datalake-store` Modülü Azure Data Lake depolama Gen1 dosya sistemi işlemlerini içerir. Bu modül hakkında daha fazla bilgi için bkz. [azure-datalake-store dosya sistemi modül başvurusu](https://azure-datalake-store.readthedocs.io/en/latest/).

Modülleri yüklemek için aşağıdaki komutları kullanın.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

1. Tercih ettiğiniz IDE’de yeni bir Python uygulaması (örneğin, **örneğim.py**) oluşturun.

2. Gerekli modülleri içeri aktarmak için aşağıdaki satırları ekleyin

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Storage Gen1 filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. Değişiklikleri örneğim.py uygulamasına kaydedin.

## <a name="authentication"></a>Authentication

Bu bölümde Azure AD ile gerçekleştirilen farklı kimlik doğrulama yöntemlerinden bahsedeceğiz. Şu seçenekleri kullanabilirsiniz:

* Uygulamanız için son kullanıcı kimlik doğrulaması için bkz. [Python kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1](data-lake-store-end-user-authenticate-python.md).
* Uygulamanız için hizmetten hizmete kimlik doğrulaması için bkz [hizmetten hizmete kimlik doğrulaması Python kullanarak Data Lake depolama Gen1 ile](data-lake-store-service-to-service-authenticate-python.md).

## <a name="create-filesystem-client"></a>Dosya sistemi istemcisini oluşturma

Aşağıdaki kod parçacığı ilk Data Lake depolama Gen1 hesabı istemcisini oluşturur. Bir Data Lake depolama Gen1 hesabı oluşturmak için istemci nesnesini kullanır. Kod parçacığı son olarak bir dosya sistemi istemci nesnesi oluşturur.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(adlCreds, store_name=adlsAccountName)

## <a name="create-a-directory"></a>Dizin oluşturma

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a>Dosya indirme

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a>Bir dizini silme

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="next-steps"></a>Sonraki adımlar
* [Hesap yönetim işlemlerini Python kullanarak Data Lake depolama Gen1](data-lake-store-get-started-python.md).

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Data Lake depolama Gen1 Python (dosya sistemi) başvurusu](https://azure-datalake-store.readthedocs.io/en/latest)
* [Azure Data Lake depolama Gen1 ile uyumlu açık kaynak büyük veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)
