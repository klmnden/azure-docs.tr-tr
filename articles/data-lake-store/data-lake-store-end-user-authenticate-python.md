---
title: "Son kullanıcı kimlik doğrulaması: Azure Active Directory'yi kullanarak Python Data Lake Store ile | Microsoft Docs"
description: "Python ile Azure Active Directory kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması elde öğrenin"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2017
ms.author: nitinme
ms.openlocfilehash: 48990c57fb10127733623000a105507b5a48d900
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-python"></a>Python kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’yı kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
> 

Bu makalede, Python SDK'sı Azure Data Lake Store ile son kullanıcı kimlik doğrulaması yapmak için nasıl kullanılacağı hakkında bilgi edinin. Son kullanıcı kimlik doğrulaması ek iki kategoriye bölünebilir:

* Çok faktörlü kimlik doğrulaması olmadan son kullanıcı kimlik doğrulaması
* Çok faktörlü kimlik doğrulaması ile son kullanıcı kimlik doğrulaması

Her iki bu seçenek, bu makalede ele alınmıştır. Python kullanarak Data Lake Store ile hizmet kimlik doğrulaması için bkz: [hizmeti için kimlik doğrulaması Python kullanarak Data Lake Store ile](data-lake-store-service-to-service-authenticate-python.md).

## <a name="prerequisites"></a>Önkoşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.6.2 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Yerel" uygulama oluşturma**. ' Ndaki adımları tamamlanmış gerekir [son kullanıcı kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](data-lake-store-end-user-authenticate-using-active-directory.md).

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

1. Örneğin, tercih ettiğiniz IDE içinde yeni bir Python uygulaması oluşturma **mysample.py**.

2. Gerekli modülleri içeri aktarmak için aşağıdaki kod parçacığını ekleyin

    ```
    ## Use this for Azure AD authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    import adal
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, pprint, uuid, time
    ```

3. Değişiklikleri örneğim.py uygulamasına kaydedin.

## <a name="end-user-authentication-with-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulaması ile son kullanıcı kimlik doğrulaması

### <a name="for-account-management"></a>Hesap Yönetimi

Aşağıdaki kod parçacığında, bir Data Lake Store hesabındaki hesap yönetimi işlemleri için Azure AD ile kimlik doğrulaması yapmak için kullanın. Aşağıdaki kod parçacığını uygulamanızda multi-factor authentication ile kimlik doğrulaması gerçekleştirmek için kullanabilirsiniz. Var olan Azure AD için aşağıdaki değerleri girin **yerel** uygulama.

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

Bir Data Lake Store hesabına dosya sistemi işlemleri için Azure AD ile kimlik doğrulaması yapmak için bunu kullanın. Aşağıdaki kod parçacığını uygulamanızda multi-factor authentication ile kimlik doğrulaması gerçekleştirmek için kullanabilirsiniz. Var olan Azure AD için aşağıdaki değerleri girin **yerel** uygulama.

    adlCreds = lib.auth(tenant_id='FILL-IN-HERE', resource = 'https://datalake.azure.net/')

## <a name="end-user-authentication-without-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulaması olmadan son kullanıcı kimlik doğrulaması

Bu kullanım dışıdır. Daha fazla bilgi için bkz: [Azure Python SDK'sını kullanarak kimlik doğrulaması](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python#mgmt-auth-token).
   
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Python kullanarak Azure Data Lake Store ile kimlik doğrulaması için son kullanıcı kimlik doğrulaması kullanmayı öğrendiniz. Şimdi, Azure Data Lake Store ile çalışmaya Python kullanma hakkında konuşun aşağıdaki makaleleri da bakabilirsiniz.

* [Python kullanarak Data Lake Store üzerinde hesap yönetimi işlemleri](data-lake-store-get-started-python.md)
* [Python kullanarak Data Lake Store üzerinde veri işlemleri](data-lake-store-data-operations-python.md)

