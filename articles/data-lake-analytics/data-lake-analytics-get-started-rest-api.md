---
title: "REST API&quot;yi kullanarak Data Lake Analytics ile çalışmaya başlama| Microsoft Belgeleri"
description: "Data Lake Analytics üzerinde işlem yapmak için WebHDFS REST API&quot;lerini kullanma"
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 5e133d92-baaa-44c9-890c-ab2d85c91122
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 2fea3686b1484406d31c5447c7d3d7e2451b827e
ms.openlocfilehash: 1898b3d6aa1a9ccbc9f4427cf994c02f9fa35abd


---
# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>REST API’lerini kullanarak Azure Data Lake Analytics ile çalışmaya başlama | Azure
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Data Lake Analytics hesaplarını, işlerini ve kataloğunu yönetmek için WebHDFS REST API’lerini ve Data Lake Analytics REST API’lerini nasıl kullanacağınızı öğrenin. 

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Active Directory Uygulaması oluşturma**. Data Lake Analytics uygulamasında Azure AD ile kimlik doğrulaması yapmak için Azure AD uygulamasını kullanın. Azure AD kimlik doğrulaması için **son kullanıcı kimlik doğrulaması** veya **hizmetten hizmete kimlik doğrulama** gibi farklı yaklaşımlar bulunmaktadır. Kimlik doğrulaması hakkında yönergeler ve daha fazla bilgi için bkz. [Azure Active Directory kullanarak Data Lake Analytics kimlik doğrulaması yapma](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).
* [cURL](http://curl.haxx.se/). Bu makalede, bir Data Lake Analytics hesabına yönelik olarak REST API çağrılarının nasıl yapılacağını göstermek üzere cURL kullanılmıştır.

## <a name="authenticate-with-azure-active-directory"></a>Azure Active Directory ile kimlik doğrulama
Azure Active Directory ile kimlik doğrulama gerçekleştirmek için kullanılabilecek iki yöntem vardır.

### <a name="end-user-authentication-interactive"></a>Son kullanıcı kimlik doğrulaması (etkileşimli)
Bu yöntemi kullanarak, uygulama kullanıcıdan oturum açmasını ister ve tüm işlemler, kullanıcı bağlamında gerçekleştirilir. 

Etkileşimli kimlik doğrulaması için aşağıdaki adımları izleyin:

1. Uygulamanızı kullanarak kullanıcıyı şu URL'ye yönlendirin:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<REDIRECT-URI>, bir URL içinde kullanılmak üzere kodlanmalıdır. Bu nedenle, https://localhost için `https%3A%2F%2Flocalhost` kullanılır)
   > 
   > 
   
    Bu öğreticinin amaçları doğrultusunda, yukarıdaki URL'deki yer tutucu değerlerini değiştirebilir ve bir web tarayıcısının adres çubuğuna yapıştırabilirsiniz. Azure oturum açma bilgilerinizi kullanarak kimlik doğrulaması gerçekleştirmeye yönlendirileceksiniz. Başarıyla oturum açmanızın ardından yanıt, tarayıcının adres çubuğunda görüntülenir. Yanıt şu biçimde olacaktır:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. Yanıttaki yetki kodunu alın. Bu öğretici için, web tarayıcısının adres çubuğundan yetki kodunu kopyalayabilir ve belirteç uç noktasını istemek için aşağıda gösterildiği gibi POST isteğinde geçirebilirsiniz:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > Bu durumda, \<REDIRECT-URI> öğesinin kodlanması gerekmez.
   > 
   > 
3. Yanıt, bir erişim belirteci içeren bir JSON nesnesi (ör. `"access_token": "<ACCESS_TOKEN>"`) ve bir yenileme belirtecidir (ör. `"refresh_token": "<REFRESH_TOKEN>"`). Uygulamanız Azure Data Lake Store'a erişmek için erişim belirtecini ve bir erişim belirtecinin süresi olduğunda başka bir erişim belirteci almak için yenileme belirtecini kullanır.
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. Erişim belirtecinin süresi dolduğunda, aşağıda gösterildiği gibi yenileme belirtecini kullanarak yeni bir erişim belirteci isteyebilirsiniz:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<CLIENT-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Etkileşimli kullanıcı kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Yetki kodu izin akışı](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Hizmetten hizmete kimlik doğrulaması (etkileşimli olmayan)
Bu yöntemi kullanarak uygulama, işlemleri gerçekleştirmek için kendi kimlik bilgilerini sağlar. Bunun için aşağıda gösterilene benzer bir POST isteği yayımlamanız gerekir: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Bu isteğin çıktısı, sonrasında REST API çağrılarınızla geçireceğiniz bir yetki belirteci (aşağıdaki çıktıda `access-token` ile belirtilmiştir) içerir. Bu kimlik doğrulama belirtecini bir metin dosyasına kaydedin; belirteç, bu makalenin ilerleyen bölümlerinde gerekli olacaktır.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Bu makalede, **etkileşimli olmayan** yaklaşım kullanılmıştır. Etkileşimli olmayan seçeneği (hizmet-hizmet çağrıları) hakkında daha fazla bilgi için bkz. [Kimlik bilgilerini kullanarak gerçekleştirilen hizmet-hizmet çağrıları](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma
Data Lake Analytics hesabı oluşturabilmek için Azure kaynak grubu ve Data Lake Store hesabı oluşturmanız gerekir.  Bkz. [Data Lake Store hesabı oluşturma](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Aşağıdaki Curl komutu nasıl hesap oluşturulacağını göstermektedir:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

\<`REDACTED`\> yerine yetkilendirme belirtecini, \<`AzureSubscriptionID`\> yerine abonelik kimliğinizi, \<`AzureResourceGroupName`\> yerine var olan Azure kaynak grubu adını ve \<`NewAzureDataLakeAnalyticsAccountName`\> yerine yeni Data Lake Analytics hesabı adını yazın. Bu komuta yönelik istek yükü, yukarıdaki `-d` parametresi için sağlanan **CreateDatalakeAnalyticsAccountRequest.json** dosyasına dahildir. Söz konusu input.json dosyasının içeriği şuna benzer:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Bir abonelikteki Data Lake Analytics hesaplarını listeleme
Aşağıdaki Curl komutu bir abonelikteki hesapların nasıl listeleneceğini göstermektedir:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

\<`REDACTED`\> yerine yetkilendirme belirtecini, \<`AzureSubscriptionID`\> yerine de abonelik kimliğinizi yazın. Çıktı şuna benzer olacaktır:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Bir Data Lake Analytics hesabı hakkında bilgi edinme
Aşağıdaki Curl komutu hesap bilgilerinin nasıl alınacağını göstermektedir:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

\<`REDACTED`\> yerine yetkilendirme belirtecini, \<`AzureSubscriptionID`\> yerine abonelik kimliğinizi, \<`AzureResourceGroupName`\> yerine var olan Azure kaynak grubu adını ve \<`DataLakeAnalyticsAccountName`\> yerine var olan Data Lake Analytics hesabını yazın. Çıktı şuna benzer olacaktır:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Bir Data Lake Analytics hesabının Data Lake Store bilgilerini listeleme
Aşağıdaki Curl komutu bir hesaptaki Data Lake Store bilgilerinin nasıl listeleneceğini göstermektedir:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

\<`REDACTED`\> yerine yetkilendirme belirtecini, \<`AzureSubscriptionID`\> yerine abonelik kimliğinizi, \<`AzureResourceGroupName`\> yerine var olan Azure kaynak grubu adını ve \<`DataLakeAnalyticsAccountName`\> yerine var olan Data Lake Analytics hesabını yazın. Çıktı şuna benzer olacaktır:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>U-SQL işlerini gönderme
Aşağıdaki Curl komutu bir U-SQL işinin nasıl gönderileceğini göstermektedir:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

\<`REDACTED`\> yerine yetkilendirme belirtecini, \<`DataLakeAnalyticsAccountName`\> yerine de var olan Data Lake Analytics hesabının adını yazın. Bu komuta yönelik istek yükü, yukarıdaki `-d` parametresi için sağlanan **SubmitADLAJob.json** dosyasına dahildir. Söz konusu input.json dosyasının içeriği şuna benzer:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

Çıktı şuna benzer olacaktır:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>U-SQL işlerini listeleme
Aşağıdaki Curl komutu bir U-SQL işinin nasıl listeleneceğini göstermektedir:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

\<`REDACTED`\> yerine yetkilendirme belirtecini, \<`DataLakeAnalyticsAccountName`\> yerine de var olan Data Lake Analytics hesabının adını yazın. 

Çıktı şuna benzer olacaktır:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Katalog öğelerini alma
Aşağıdaki Curl komutu katalogdan veritabanlarının nasıl alınacağını göstermektedir:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

Çıktı şuna benzer olacaktır:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Ayrıca bkz.
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
* Yönetim görevleri için bkz. [Azure portalı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).
* Data Lake Analytics'e yönelik bir genel bakış için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).
* Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.
* Tanılama bilgilerini günlüğe kaydetmek için bkz. [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)




<!--HONumber=Feb17_HO1-->


