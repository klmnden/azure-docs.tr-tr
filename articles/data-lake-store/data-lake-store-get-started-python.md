---
title: 'Python: Hesap yönetimi işlemleri Azure Data Lake depolama Gen1 | Microsoft Docs'
description: Azure Data Lake depolama Gen1 hesap yönetim işlemleriyle çalışmak için Python SDK'sını kullanmayı öğrenin.
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: b6ef5a5c12bb766fb7106d5c7a8189c4b92980d2
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58880211"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-python"></a>Azure Data Lake depolama Gen1 hesap yönetim işlemlerini Python kullanarak
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Temel hesap yönetim işlemlerini gibi Oluştur bir Data Lake depolama Gen1 hesabı, hesapları listeleme Data Lake depolama Gen1 gerçekleştirmek için Azure Data Lake depolama Gen1 için Python SDK'sını kullanmayı öğrenin. Dosya sistemi işlemlerini Python kullanarak Data Lake depolama Gen1 gerçekleştirmek yönergeler için bkz: [dosya sistemi işlemlerini Python kullanarak Data Lake depolama Gen1](data-lake-store-data-operations-python.md).

## <a name="prerequisites"></a>Önkoşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.6.2 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure kaynak grubu**. Talimatlar için bkz. [Azure kaynak grubu oluşturma](../azure-resource-manager/manage-resource-groups-portal.md).

## <a name="install-the-modules"></a>Modülleri yükleme

Data Lake depolama Gen1 ile çalışmak için Python'ı kullanarak, üç modül yüklemeniz gerekir.

* `azure-mgmt-resource` modülü, Active Directory gibi şeyler için Azure modüllerini içerir.
* `azure-mgmt-datalake-store` Modülü Azure Data Lake depolama Gen1 hesap yönetim işlemlerini içerir. Bu modül hakkında daha fazla bilgi için bkz. [Azure Data Lake depolama Gen1 Yönetimi modül başvurusu](https://docs.microsoft.com/python/api/azure.mgmt.datalake.store?view=azure-python).
* `azure-datalake-store` Modülü Azure Data Lake depolama Gen1 dosya sistemi işlemlerini içerir. Bu modül hakkında daha fazla bilgi için bkz. [azure-datalake-store dosya sistemi modül başvurusu](https://azure-datalake-store.readthedocs.io/en/latest/).

Modülleri yüklemek için aşağıdaki komutları kullanın.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

1. Tercih ettiğiniz IDE’de yeni bir Python uygulaması (örneğin, **örneğim.py**) oluşturun.

2. Gerekli modülleri içeri aktarmak için aşağıdaki kod parçacığını ekleyin

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Data Lake Storage Gen1 filesystem management
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

## <a name="create-client-and-data-lake-storage-gen1-account"></a>İstemci ve Data Lake depolama Gen1 hesabı oluşturma

Aşağıdaki kod parçacığı ilk Data Lake depolama Gen1 hesabı istemcisini oluşturur. Bir Data Lake depolama Gen1 hesabı oluşturmak için istemci nesnesini kullanır. Kod parçacığı son olarak bir dosya sistemi istemci nesnesi oluşturur.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'

    ## Create Data Lake Storage Gen1 account management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(armCreds, subscriptionId)

    ## Create a Data Lake Storage Gen1 account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    
## <a name="list-the-data-lake-storage-gen1-accounts"></a>Data Lake depolama Gen1 hesaplarını listeleme

    ## List the existing Data Lake Storage Gen1 accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="delete-the-data-lake-storage-gen1-account"></a>Data Lake depolama Gen1 hesabını Sil

    ## Delete an existing Data Lake Storage Gen1 account
    adlsAcctClient.account.delete(adlsAccountName)
    

## <a name="next-steps"></a>Sonraki adımlar
* [Dosya sistemi işlemlerini Python kullanarak Data Lake depolama Gen1](data-lake-store-data-operations-python.md).

## <a name="see-also"></a>Ayrıca bkz.

* [Azure-datalake-store Python (dosya sistemi) başvurusu](https://azure-datalake-store.readthedocs.io/en/latest)
* [Azure Data Lake depolama Gen1 ile uyumlu açık kaynak büyük veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)
