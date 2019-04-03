---
title: "Son kullanıcı kimlik doğrulaması: Azure Active Directory'yi kullanarak Python ile Azure Data Lake depolama Gen1 | Microsoft Docs"
description: Python ile Azure Active Directory kullanarak son kullanıcı kimlik doğrulaması ile Azure Data Lake depolama Gen1 elde öğrenin
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
ms.openlocfilehash: 8b72604d7e736230911d0a0987b88d372be4ddf3
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58881288"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-python"></a>Python kullanarak son kullanıcı kimlik doğrulaması ile Azure Data Lake depolama Gen1
> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’sını kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
> 

Bu makalede, Azure Data Lake depolama Gen1 son kullanıcı kimlik doğrulaması yapmak için Python SDK'sını kullanma hakkında bilgi edinin. Son kullanıcı kimlik doğrulaması başka iki kategoriye ayrılabilir:

* Multi-Factor authentication olmaksızın son kullanıcı kimlik doğrulaması
* Multi-Factor authentication ile son kullanıcı kimlik doğrulaması

Bu seçeneklerin, bu makalede ele alınmıştır. Python kullanarak hizmetten hizmete kimlik doğrulaması ile Data Lake depolama Gen1 bkz [hizmetten hizmete kimlik doğrulaması Python kullanarak Data Lake depolama Gen1 ile](data-lake-store-service-to-service-authenticate-python.md).

## <a name="prerequisites"></a>Önkoşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.6.2 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Yerel" uygulama oluşturma**. Adımları tamamlamış olmanız gerekir [Azure Active Directory kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1](data-lake-store-end-user-authenticate-using-active-directory.md).

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

1. Tercih ettiğiniz IDE'de, örneğin, yeni bir Python uygulaması oluşturma **Örneğim.PY**.

2. Gerekli modülleri içeri aktarmak için aşağıdaki kod parçacığını ekleyin

    ```
    ## Use this for Azure AD authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Storage Gen1 filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    import adal
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, pprint, uuid, time
    ```

3. Değişiklikleri örneğim.py uygulamasına kaydedin.

## <a name="end-user-authentication-with-multi-factor-authentication"></a>Multi-Factor authentication ile son kullanıcı kimlik doğrulaması

### <a name="for-account-management"></a>Hesap Yönetimi

Aşağıdaki kod parçacığı Data Lake depolama Gen1 hesabında hesap yönetimi işlemleri için Azure AD kimlik doğrulaması gerçekleştirmek için kullanın. Aşağıdaki kod parçacığını uygulamanızda multi-factor authentication ile kimlik doğrulaması gerçekleştirmek için kullanabilirsiniz. Mevcut bir Azure AD için aşağıdaki değerleri sağlayın **yerel** uygulama.

    authority_host_url = "https://login.microsoftonline.com"
    tenant = "FILL-IN-HERE"
    authority_url = authority_host_url + '/' + tenant
    client_id = 'FILL-IN-HERE'
    redirect = 'urn:ietf:wg:oauth:2.0:oob'
    RESOURCE = 'https://management.core.windows.net/'
    
    context = adal.AuthenticationContext(authority_url)
    code = context.acquire_user_code(RESOURCE, client_id)
    print(code['message'])
    mgmt_token = context.acquire_token_with_device_code(RESOURCE, code, client_id)
    armCreds = AADTokenCredentials(mgmt_token, client_id, resource = RESOURCE)

### <a name="for-filesystem-operations"></a>Dosya sistemi işlemleri için

Dosya sistemi işlemleri Data Lake depolama Gen1 hesabı için Azure AD kimlik doğrulaması gerçekleştirmek için bunu kullanın. Aşağıdaki kod parçacığını uygulamanızda multi-factor authentication ile kimlik doğrulaması gerçekleştirmek için kullanabilirsiniz. Mevcut bir Azure AD için aşağıdaki değerleri sağlayın **yerel** uygulama.

    adlCreds = lib.auth(tenant_id='FILL-IN-HERE', resource = 'https://datalake.azure.net/')

## <a name="end-user-authentication-without-multi-factor-authentication"></a>Multi-Factor authentication olmaksızın son kullanıcı kimlik doğrulaması

Bu kullanım dışıdır. Daha fazla bilgi için [Azure Python SDK'sını kullanarak kimlik doğrulaması](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python#mgmt-auth-token).
   
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Data Lake depolama Gen1 ile kimlik doğrulaması için son kullanıcı kimlik doğrulaması kullanmayı öğrendiniz Python kullanarak. Şimdi, Azure Data Lake depolama Gen1 ile çalışmak için Python'ı kullanma hakkında konuşmak aşağıdaki makaleleri da bakabilirsiniz.

* [Data Lake depolama Gen1 hesap yönetim işlemlerini Python kullanarak](data-lake-store-get-started-python.md)
* [Data Lake depolama Gen1 üzerinde Python kullanarak veri işlemleri](data-lake-store-data-operations-python.md)

