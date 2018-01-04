---
title: "Python: Azure Data Lake Store'daki hesap yönetimi işlemleri | Microsoft Docs"
description: "Data Lake Store hesap yönetim işlemleriyle çalışmak için Python SDK'yı nasıl kullanacağınızı öğrenin."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2017
ms.author: nitinme
ms.openlocfilehash: 76e84687815ca6f4b031e5f7143ba0079fb053db
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="account-management-operations-on-azure-data-lake-store-using-python"></a>Python kullanılarak gerçekleştirilen Azure Data Lake Store'daki hesap yönetimi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Azure Data Lake Store için Python SDK'yı Data Lake Store hesabı oluşturma, Data Lake Store hesaplarını listeleme gibi temel hesap yönetim işlemlerini gerçekleştirme amacıyla kullanmayı öğrenin. Data Lake Store'da dosya sistemi işlemlerini Python kullanarak gerçekleştirme talimatları için bkz. [Data Lake Store'da Python kullanılarak gerçekleştirilen dosya sistemi işlemleri](data-lake-store-data-operations-python.md).

## <a name="prerequisites"></a>Ön koşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.6.2 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure kaynak grubu**. Talimatlar için bkz. [Azure kaynak grubu oluşturma](../azure-resource-manager/resource-group-portal.md).

## <a name="install-the-modules"></a>Modülleri yükleme

Data Lake Store ile Python kullanarak çalışabilmeniz için üç modül yüklemeniz gerekir.

* `azure-mgmt-resource` modülü, Active Directory gibi şeyler için Azure modüllerini içerir.
* `azure-mgmt-datalake-store` modülü, Azure Data Lake Store hesap yönetim işlemlerini içerir. Bu modül hakkında daha fazla bilgi için bkz. [Azure Data Lake Store Yönetimi modül başvurusu](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).
* `azure-datalake-store` modülü, Azure Data Lake Store dosya sistemi işlemlerini içerir. Bu modül hakkında daha fazla bilgi için bkz. [Azure Data Lake Store Dosya Sistemi modül başvurusu](http://azure-datalake-store.readthedocs.io/en/latest/).

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

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. Değişiklikleri örneğim.py uygulamasına kaydedin.

## <a name="authentication"></a>Kimlik Doğrulaması

Bu bölümde Azure AD ile gerçekleştirilen farklı kimlik doğrulama yöntemlerinden bahsedeceğiz. Şu seçenekleri kullanabilirsiniz:

* Uygulamanızın son kullanıcı kimlik doğrulaması için bkz. [Python kullanarak Data Lake Store'da son kullanıcı kimlik doğrulaması gerçekleştirme](data-lake-store-end-user-authenticate-python.md).
* Uygulamanızın servisler arası kimlik doğrulaması için bkz. [Python kullanarak Data Lake Store'da servisler arası kimlik doğrulaması gerçekleştirme](data-lake-store-service-to-service-authenticate-python.md).

## <a name="create-client-and-data-lake-store-account"></a>İstemci ve Data Lake Store hesabı oluşturma

Aşağıdaki kod parçacığı ilk olarak Data Lake Store hesabı istemcisini oluşturur. İstemci nesnesini kullanarak bir Data Lake Store hesabı oluşturur. Kod parçacığı son olarak bir dosya sistemi istemci nesnesi oluşturur.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'

    ## Create data lake store account management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(armCreds, subscriptionId)

    ## Create a Data Lake Store account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    
## <a name="list-the-data-lake-store-accounts"></a>Data Lake Store hesaplarını listeleme

    ## List the existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="delete-the-data-lake-store-account"></a>Data Lake Store hesabını silme

    ## Delete the existing Data Lake Store accounts
    adlsAcctClient.account.delete(adlsAccountName)
    

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Store'da Python kullanılarak gerçekleştirilen dosya sistemi işlemleri](data-lake-store-data-operations-python.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Store Python (Hesap yönetimi) Başvurusu](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html)
* [Azure Data Lake Store Python (Dosya sistemi) Başvurusu](http://azure-datalake-store.readthedocs.io/en/latest)
* [Azure Data Lake Store ile uyumlu Açık Kaynak Büyük Veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)
