---
title: "Hizmetten hizmete kimlik doğrulaması: Azure Active Directory'yi kullanarak Python Data Lake Store ile | Microsoft Docs"
description: "Hizmetten hizmete kimlik doğrulaması Python kullanarak Azure Active Directory kullanarak Data Lake Store ile elde öğrenin"
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
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: c04b870e72c5d29df95d16b96cc423441af6fd85
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="service-to-service-authentication-with-data-lake-store-using-python"></a>Python kullanarak Data Lake Store ile hizmet kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’yı kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

Bu makalede, Python SDK'sı Azure Data Lake Store ile hizmet kimlik doğrulaması yapmak için nasıl kullanılacağı hakkında bilgi edinin. Python kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması için bkz: [Python kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-python.md).


## <a name="prerequisites"></a>Önkoşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.6.2 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Web" uygulama oluşturma**. ' Ndaki adımları tamamlanmış gerekir [hizmeti için kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](data-lake-store-service-to-service-authenticate-using-active-directory.md).

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
    import logging, getpass, pprint, uuid, time
    ```

3. Değişiklikleri örneğim.py uygulamasına kaydedin.

## <a name="service-to-service-authentication-with-client-secret-for-account-management"></a>Hesap yönetimi için gizli anahtarla hizmetten hizmete kimlik doğrulaması

Bu kod parçacığında, Data Lake hesabı yönetim işlemlerini Data Lake Store hesabı oluşturma, silme Data Lake Store hesabı, vb. gibi depolamak için Azure AD ile kimlik doğrulaması yapmak için kullanın. Aşağıdaki kod parçacığında etkileşimsiz, uygulamanız için kimlik doğrulaması için istemci gizli anahtarı mevcut Azure AD "Web uygulaması" için bir uygulama / hizmet sorumlusunu kullanarak kullanılabilir uygulama.

    authority_host_uri = 'https://login.microsoftonline.com'
    tenant = '<TENANT>'
    authority_uri = authority_host_uri + '/' + tenant
    RESOURCE = 'https://management.core.windows.net/'
    client_id = '<CLIENT_ID>'
    client_secret = '<CLIENT_SECRET>'
    
    context = adal.AuthenticationContext(authority_uri, api_version=None)
    mgmt_token = context.acquire_token_with_client_credentials(RESOURCE, client_id, client_secret)
    armCreds = AADTokenCredentials(mgmt_token, client_id, resource=RESOURCE)

## <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a>Dosya sistemi işlemleri için gizli anahtarla hizmetten hizmete kimlik doğrulaması

Aşağıdaki kod parçacığında, Data Lake Store üzerinde dosya sistemi işlemleri gibi oluşturmak için klasör, karşıya yükleme dosyasını vb. Azure AD ile kimlik doğrulaması yapmak için kullanın. Gizli anahtar / hizmet sorumlusu kullanılarak aşağıdaki kod parçacığı uygulamanızın etkileşimli olmayan kimlik doğrulaması için kullanılabilir. Bunu mevcut Azure AD "Web App" uygulaması ile birlikte kullanın.

    adlCreds = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE', resource = 'https://datalake.azure.net/')

<!-- ## Service-to-service authentication with certificate for account management

Use this snippet to authenticate with Azure AD for account management operations on Data Lake Store such as create Data Lake Store account, delete Data Lake Store account, etc. The following snippet can be used to authenticate your application non-interactively, using the certificate of an existing Azure AD "Web App" application. For instructions on how to create an Azure AD application, see [Create service principal with certificates](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).

    authority_host_uri = 'https://login.microsoftonline.com'
    tenant = '<TENANT>'
    authority_uri = authority_host_uri + '/' + tenant
    resource_uri = 'https://management.core.windows.net/'
    client_id = '<CLIENT_ID>'
    client_cert = '<CLIENT_CERT>'
    client_cert_thumbprint = '<CLIENT_CERT_THUMBPRINT>'

    context = adal.AuthenticationContext(authority_uri, api_version=None)
    mgmt_token = context.acquire_token_with_client_certificate(resource_uri, client_id, client_cert, client_cert_thumbprint)
    credentials = AADTokenCredentials(mgmt_token, client_id) -->

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Python kullanarak Azure Data Lake Store ile kimlik doğrulaması için hizmetten hizmete kimlik doğrulaması kullanmayı öğrendiniz. Şimdi, Azure Data Lake Store ile çalışmaya Python kullanma hakkında konuşun aşağıdaki makaleleri da bakabilirsiniz.

* [Python kullanarak Data Lake Store üzerinde hesap yönetimi işlemleri](data-lake-store-get-started-python.md)
* [Python kullanarak Data Lake Store üzerinde veri işlemleri](data-lake-store-data-operations-python.md)


