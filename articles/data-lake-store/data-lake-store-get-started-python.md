---
title: "Python SDK’sını kullanarak Azure Data Lake Store ile çalışmaya başlama | Microsoft Docs"
description: "Data Lake Store hesapları ve dosya sistemiyle çalışmak için Python SDK’yı nasıl kullanacağınızı öğrenin."
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
ms.date: 06/29/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 375a603360ac249fc1b08923a94c85652390a3fc
ms.contentlocale: tr-tr
ms.lasthandoff: 07/01/2017


---

<a id="get-started-with-azure-data-lake-store-using-python" class="xliff"></a>

# Python’u kullanarak Azure Data Lake Store ile çalışmaya başlama

> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme gibi temel işlemleri gerçekleştirmek için Azure’a yönelik Python SDK’yı ve Azure Data Lake Store’u kullanma hakkında bilgi edinin. Data Lake hakkında daha fazla bilgi için bkz. [Azure Data Lake Store](data-lake-store-overview.md).

<a id="prerequisites" class="xliff"></a>

## Ön koşullar

* **Python**. Python’u [buradan](https://www.python.org/downloads/) indirebilirsiniz. Bu makalede Python 3.5.2 kullanılmıştır.

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory Uygulaması oluşturma**. Data Lake Store uygulamasında Azure AD ile kimlik doğrulaması yapmak için Azure AD uygulamasını kullanın. Azure AD kimlik doğrulaması için **son kullanıcı kimlik doğrulaması** veya **hizmetten hizmete kimlik doğrulama** gibi farklı yaklaşımlar bulunmaktadır. Kimlik doğrulaması gerçekleştirmeyle ilgili yönergeler ve daha fazla bilgi için [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md) veya [Hizmetten hizmete kimlik doğrulaması](data-lake-store-authenticate-using-active-directory.md) bölümlerine göz atın.

<a id="install-the-modules" class="xliff"></a>

## Modülleri yükleme

Data Lake Store ile Python kullanarak çalışabilmeniz için üç modül yüklemeniz gerekir.

* `azure-mgmt-resource` modülü. Bu, Active Directory gibi şeyler için Azure modüllerini içerir
* `azure-mgmt-datalake-store` modülü. Bu modül, Azure Data Lake Store hesap yönetim işlemlerini içerir. Bu modül hakkında daha fazla bilgi için bkz. [Azure Data Lake Store Yönetimi modül başvurusu](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).
* `azure-datalake-store` modülü. Bu modül, Azure Data Lake Store dosya sistemi işlemlerini içerir. Bu modül hakkında daha fazla bilgi için bkz. [Azure Data Lake Store Dosya Sistemi modül başvurusu](http://azure-datalake-store.readthedocs.io/en/latest/).

Modülleri yüklemek için aşağıdaki komutları kullanın.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

<a id="create-a-new-python-application" class="xliff"></a>

## Yeni Python uygulaması oluşturma

1. Tercih ettiğiniz IDE’de yeni bir Python uygulaması (örneğin, **örneğim.py**) oluşturun.

2. Gerekli modülleri içeri aktarmak için aşağıdaki satırları ekleyin

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

<a id="authentication" class="xliff"></a>

## Kimlik Doğrulaması

Bu bölümde Azure AD ile gerçekleştirilen farklı kimlik doğrulama yöntemlerinden bahsedeceğiz. Şu seçenekleri kullanabilirsiniz:

* Son kullanıcı kimlik doğrulaması
* Hizmetten hizmete kimlik doğrulaması
* Multi-factor authentication

Bu kimlik doğrulama seçeneklerini hem hesap yönetimi hem de dosya sistemi yönetim modülleri için kullanmanız gerekir.

<a id="end-user-authentication-for-account-management" class="xliff"></a>

### Hesap yönetimi için son kullanıcı kimlik doğrulaması

Hesap yönetimi işlemleri (Data Lake Store hesabı oluşturma/silme, vb.) gerçekleştirmek üzere Azure AD kimlik doğrulaması gerçekleştirmek için bunu kullanın. Bir Azure AD kullanıcısının kullanıcı adını ve parolasını sağlamalısınız. Kullanıcının, çok faktörlü kimlik doğrulaması kullanacak şekilde yapılandırılmaması gerektiğini unutmayın.

    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

<a id="end-user-authentication-for-filesystem-operations" class="xliff"></a>

### Dosya sistemi işlemleri için son kullanıcı kimlik doğrulaması

Dosya sistemi işlemlerine (klasör oluşturma, karşıya dosya yükleme vb.) yönelik Azure AD kimlik doğrulaması gerçekleştirmek için bunu kullanın. Bunu mevcut bir Azure AD **yerel istemci** uygulaması ile kullanın. Kimlik bilgilerini sağladığınız Azure AD kullanıcısı, çok faktörlü kimlik doğrulaması kullanacak şekilde yapılandırılmamalıdır.

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

<a id="service-to-service-authentication-with-client-secret-for-account-management" class="xliff"></a>

### Hesap yönetimi için gizli anahtarla hizmetten hizmete kimlik doğrulaması

Hesap yönetimi işlemleri (Data Lake Store hesabı oluşturma/silme, vb.) gerçekleştirmek üzere Azure AD kimlik doğrulaması gerçekleştirmek için bunu kullanın. Gizli anahtar / hizmet sorumlusu kullanılarak aşağıdaki kod parçacığı uygulamanızın etkileşimli olmayan kimlik doğrulaması için kullanılabilir. Bunu mevcut Azure AD "Web App" uygulaması ile birlikte kullanın.

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

<a id="service-to-service-authentication-with-client-secret-for-filesystem-operations" class="xliff"></a>

### Dosya sistemi işlemleri için gizli anahtarla hizmetten hizmete kimlik doğrulaması

Dosya sistemi işlemlerine (klasör oluşturma, karşıya dosya yükleme vb.) yönelik Azure AD kimlik doğrulaması gerçekleştirmek için bunu kullanın. Gizli anahtar / hizmet sorumlusu kullanılarak aşağıdaki kod parçacığı uygulamanızın etkileşimli olmayan kimlik doğrulaması için kullanılabilir. Bunu mevcut Azure AD "Web App" uygulaması ile birlikte kullanın.

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

<a id="multi-factor-authentication-for-account-management" class="xliff"></a>

### Hesap yönetimi için multi-factor authentication

Hesap yönetimi işlemleri (Data Lake Store hesabı oluşturma/silme, vb.) gerçekleştirmek üzere Azure AD kimlik doğrulaması gerçekleştirmek için bunu kullanın. Aşağıdaki kod parçacığını uygulamanızda multi-factor authentication ile kimlik doğrulaması gerçekleştirmek için kullanabilirsiniz. Bunu mevcut Azure AD "Web App" uygulaması ile birlikte kullanın.

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
    credentials = AADTokenCredentials(mgmt_token, client_id)

<a id="multi-factor-authentication-for-filesystem-management" class="xliff"></a>

### Dosya sistemi yönetimi için multi-factor authentication

Dosya sistemi işlemlerine (klasör oluşturma, karşıya dosya yükleme vb.) yönelik Azure AD kimlik doğrulaması gerçekleştirmek için bunu kullanın. Aşağıdaki kod parçacığını uygulamanızda multi-factor authentication ile kimlik doğrulaması gerçekleştirmek için kullanabilirsiniz. Bunu mevcut Azure AD "Web App" uygulaması ile birlikte kullanın.

    token = lib.auth(tenant_id='FILL-IN-HERE')

<a id="create-an-azure-resource-group" class="xliff"></a>

## Azure Kaynak Grubu oluşturma

Aşağıdaki kod parçacığını kullanarak bir Azure Kaynak Grubu oluşturun:

    ## Declare variables
    subscriptionId= 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'
    
    ## Create management client object
    resourceClient = ResourceManagementClient(
        credentials,
        subscriptionId
    )
    
    ## Create an Azure Resource Group
    resourceClient.resource_groups.create_or_update(
        resourceGroup,
        ResourceGroup(
            location=location
        )
    )

<a id="create-clients-and-data-lake-store-account" class="xliff"></a>

## İstemciler ve Data Lake Store hesabı oluşturma

Aşağıdaki kod parçacığı ilk olarak Data Lake Store hesabı istemcisini oluşturur. İstemci nesnesini kullanarak bir Data Lake Store hesabı oluşturur. Kod parçacığı son olarak bir dosya sistemi istemci nesnesi oluşturur.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(credentials, subscriptionId)

    ## Create a Data Lake Store account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(token, store_name=adlsAccountName)

<a id="list-the-data-lake-store-accounts" class="xliff"></a>

## Data Lake Store hesaplarını listeleme

    ## List the existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

<a id="create-a-directory" class="xliff"></a>

## Dizin oluşturma

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

<a id="upload-a-file" class="xliff"></a>

## Dosyayı karşıya yükleme


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


<a id="download-a-file" class="xliff"></a>

## Dosya indirme

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

<a id="delete-a-directory" class="xliff"></a>

## Bir dizini silme

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

<a id="see-also" class="xliff"></a>

## Ayrıca bkz.

- [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
- [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Data Lake Store .NET SDK Başvurusu](https://msdn.microsoft.com/library/mt581387.aspx)
- [Data Lake Store REST Başvurusu](https://msdn.microsoft.com/library/mt693424.aspx)

