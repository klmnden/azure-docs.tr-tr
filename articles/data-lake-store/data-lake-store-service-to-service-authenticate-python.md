---
title: "Hizmetten hizmete kimlik doğrulaması: Azure Active Directory'yi kullanarak Python ile Azure Data Lake depolama Gen1 | Microsoft Docs"
description: Python kullanarak Azure Active Directory kullanarak hizmetten hizmete kimlik doğrulaması ile Azure Data Lake depolama Gen1 elde öğrenin
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
ms.openlocfilehash: 84b7fac10374c1c8f23d17ad775d522b4cb261e8
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877763"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-python"></a>Azure Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması Python kullanma
> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’sını kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

Bu makalede, Azure Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması yapmak için Python SDK'sını kullanma hakkında bilgi edinin. Python kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1 bkz [Python kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1](data-lake-store-end-user-authenticate-python.md).


## <a name="prerequisites"></a>Önkoşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.6.2 kullanılmıştır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Bir Azure Active Directory "Web" uygulaması oluşturma**. Adımları tamamlamış olmanız gerekir [hizmetten hizmete kimlik doğrulaması Azure Active Directory kullanarak Data Lake depolama Gen1 ile](data-lake-store-service-to-service-authenticate-using-active-directory.md).

## <a name="install-the-modules"></a>Modülleri yükleme

Data Lake depolama Gen1 ile çalışmak için Python'ı kullanarak, üç modül yüklemeniz gerekir.

* `azure-mgmt-resource` modülü, Active Directory gibi şeyler için Azure modüllerini içerir.
* `azure-mgmt-datalake-store` Modülü Data Lake depolama Gen1 hesap yönetim işlemlerini içerir. Bu modül hakkında daha fazla bilgi için bkz. [Azure Data Lake depolama Gen1 Yönetimi modül başvurusu](https://docs.microsoft.com/python/api/azure.mgmt.datalake.store?view=azure-python).
* `azure-datalake-store` Modülü Data Lake depolama Gen1 dosya sistemi işlemlerini içerir. Bu modül hakkında daha fazla bilgi için bkz. [azure-datalake-store dosya sistemi modül başvurusu](https://azure-datalake-store.readthedocs.io/en/latest/).

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

    ## Required for Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Data Lake Storage Gen1 filesystem management
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

Bu kod parçacığı Data Lake depolama Gen1 hesabı, bir Data Lake depolama Gen1 silme hesabı, vb. gibi hesap yönetim işlemlerini Data Lake depolama Gen1 oluşturmak için Azure AD ile kimlik doğrulaması yapmak için kullanın. Aşağıdaki kod parçacığı uygulamanızın etkileşimli olmayan kimlik doğrulaması için mevcut bir Azure AD "Web App" için bir uygulama / hizmet sorumlusu istemci gizli anahtarı kullanarak kullanılabilir uygulama.

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

Aşağıdaki kod parçacığı Data Lake depolama Gen1 gerçekleştirilen dosya sistemi işlemleri için Azure AD ile kimlik doğrulaması yapmak gibi için kullanın, klasör, karşıya dosya yükleme vb. oluşturun. Gizli anahtar / hizmet sorumlusu kullanılarak aşağıdaki kod parçacığı uygulamanızın etkileşimli olmayan kimlik doğrulaması için kullanılabilir. Bunu mevcut Azure AD "Web App" uygulaması ile birlikte kullanın.

    tenant = '<TENANT>'
    RESOURCE = 'https://datalake.azure.net/'
    client_id = '<CLIENT_ID>'
    client_secret = '<CLIENT_SECRET>'
    
    adlCreds = lib.auth(tenant_id = tenant,
                    client_secret = client_secret,
                    client_id = client_id,
                    resource = RESOURCE)

<!-- ## Service-to-service authentication with certificate for account management

Use this snippet to authenticate with Azure AD for account management operations on Data Lake Storage Gen1 such as create a Data Lake Storage Gen1 account, delete a Data Lake Storage Gen1 account, etc. The following snippet can be used to authenticate your application non-interactively, using the certificate of an existing Azure AD "Web App" application. For instructions on how to create an Azure AD application, see [Create service principal with certificates](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

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
Bu makalede, Data Lake depolama Gen1 ile kimlik doğrulaması için hizmetten hizmete kimlik doğrulaması kullanmayı öğrendiniz Python kullanarak. Data Lake depolama Gen1 ile çalışmak için Python'ı kullanma hakkında konuşmak Aşağıdaki makaleler artık göz atabilirsiniz.

* [Data Lake depolama Gen1 hesap yönetim işlemlerini Python kullanarak](data-lake-store-get-started-python.md)
* [Data Lake depolama Gen1 üzerinde Python kullanarak veri işlemleri](data-lake-store-data-operations-python.md)


