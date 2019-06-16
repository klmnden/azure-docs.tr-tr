---
title: Azure REST API izleme Kılavuzu
description: Nasıl isteklerinin kimliğini doğrulamak ve kullanılabilir ölçüm tanımlarını ve ölçüm değerleri almak için Azure İzleyici REST API'sini kullanın.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: robb
ms.subservice: ''
ms.openlocfilehash: bbc5aaf02f4ab4388e816faaf8df536770f3302a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65205626"
---
# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure REST API izleme Kılavuzu

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede kodunuzu kullanabilmeniz için kimlik doğrulaması yapma gösterilmektedir [Microsoft Azure İzleyici REST API Başvurusu](https://docs.microsoft.com/rest/api/monitor/).

Azure İzleyici API program aracılığıyla kullanılabilir varsayılan ölçüm tanımları, ayrıntı düzeyi ve ölçüm değerleri almak mümkün kılar. Verileri Azure SQL veritabanı, Azure Cosmos DB veya Azure Data Lake gibi ayrı bir veri deposu olarak kaydedilebilir. Buradan, gerektiği gibi ek çözümleme gerçekleştirilebilir.

Çeşitli ölçüm veri noktalarıyla çalışma yanı sıra, monitör API'si de, uyarı kuralları, etkinlik günlüklerini görüntüleme ve daha fazlasını listesinde mümkün kılar. Kullanılabilir işlemleri tam bir listesi için bkz. [Microsoft Azure İzleyici REST API Başvurusu](https://docs.microsoft.com/rest/api/monitor/).

## <a name="authenticating-azure-monitor-requests"></a>Azure İzleyici kimlik doğrulama istekleri

İlk adım, isteğin kimliğini sağlamaktır.

Azure İzleyici API'sine karşı yürütülen tüm görevler, Azure Resource Manager kimlik doğrulama modeli kullanın. Bu nedenle, tüm istekleri Azure Active Directory (Azure AD) ile kimlik doğrulaması gerekir. İstemci uygulaması kimlik doğrulaması için bir yaklaşım, Azure AD hizmet sorumlusu oluşturma ve (JWT) kimlik doğrulama belirtecini alma oluşturmaktır. Aşağıdaki örnek betik, Azure AD hizmet sorumlusu PowerShell aracılığıyla oluşturma gösterilmektedir. Daha ayrıntılı bir Rehber için belgelerine başvurun [kaynaklara erişmek için bir hizmet sorumlusu oluşturmak için Azure PowerShell kullanarak](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps). Ayrıca filtrelenebilir [Azure portal aracılığıyla hizmet sorumlusu oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md).

```powershell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"

# Authenticate to a specific Azure subscription.
Connect-AzAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"
$secureStringPassword = ConvertTo-SecureString -String $pwd -AsPlainText -Force

# Create a new Azure AD application
$azureAdApplication = New-AzADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $secureStringPassword

# Create a new service principal associated with the designated application
New-AzADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Azure İzleyici API sorgulamak için istemci uygulaması daha önce oluşturulan hizmet sorumlusuyla kimlik doğrulaması yapmak için kullanmanız gerekir. Aşağıdaki örnek PowerShell Betiği bir gösterir kullanarak yaklaşımını [Active Directory Authentication Library](../../active-directory/develop/active-directory-authentication-libraries.md) JWT kimlik doğrulama belirteci almak için (ADAL). JWT belirteci varsayılan olarak, Azure İzleyici REST API için bir HTTP yetkilendirme parametresi isteklerinde bir parçası olarak geçirilir.

```powershell
$azureAdApplication = Get-AzADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.microsoftonline.com/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)

$result = $AuthContext.AcquireTokenAsync("https://management.core.windows.net/", $cred).GetAwaiter().GetResult()

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Kimlik doğrulandıktan sonra sorgular, ardından Azure İzleyici REST API'sine karşı gerçekleştirilebilir. İki yararlı sorgular şunlardır:

1. Bir kaynak için ölçüm tanımlarını Listele
2. Ölçüm değerlerini alma

> [!NOTE]
> Azure REST API ile kimlik doğrulaması hakkında ek bilgi için bkz [Azure REST API Başvurusu](https://docs.microsoft.com/rest/api/azure/).
>
>

## <a name="retrieve-metric-definitions-multi-dimensional-api"></a>(Çok boyutlu API) ölçüm tanımlarını Al

Kullanım [Azure İzleyici ölçüm tanımlarını REST API](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) hizmeti için kullanılabilir ölçüm listesine erişmek için.

**Yöntemi**: GET

**İstek URI'si**: https:\/\/management.azure.com/subscriptions/ *{Subscriptionıd}* /resourceGroups/ *{resourceGroupName}* /providers/ *{resourceProviderNamespace}* / *{resourceType}* / *{resourceName}* /providers/microsoft.insights/metricDefinitions?api-version= *{ apiVersion}*

Örneğin, bir Azure depolama hesabı için ölçüm tanımları almak için isteği şu şekilde görünür:

```powershell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricDefinitions?api-version=2018-01-01"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contosostorage-metricdef-results.json" `
                  -Verbose

```

> [!NOTE]
> Çok boyutlu Azure İzleyici ölçümleri REST API kullanarak ölçüm tanımları almak için "2018-01-01" API sürümü kullanın.
>
>

Sonuçta elde edilen JSON yanıt gövdesine aşağıdaki örneğe benzer olacaktır: (İkinci ölçüm boyutları olmadığını unutmayın)

```JSON
{
    "value": [
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/UsedCapacity",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage",
            "namespace": "Microsoft.Storage/storageAccounts",
            "category": "Capacity",
            "name": {
                "value": "UsedCapacity",
                "localizedValue": "Used capacity"
            },
            "isDimensionRequired": false,
            "unit": "Bytes",
            "primaryAggregationType": "Average",
            "supportedAggregationTypes": [
                "Total",
                "Average",
                "Minimum",
                "Maximum"
            ],
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT6H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT12H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "P1D",
                    "retention": "P93D"
                }
            ]
        },
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/Transactions",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage",
            "namespace": "Microsoft.Storage/storageAccounts",
            "category": "Transaction",
            "name": {
                "value": "Transactions",
                "localizedValue": "Transactions"
            },
            "isDimensionRequired": false,
            "unit": "Count",
            "primaryAggregationType": "Total",
            "supportedAggregationTypes": [
                "Total"
            ],
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT5M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT15M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT30M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT1H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT6H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT12H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "P1D",
                    "retention": "P93D"
                }
            ],
            "dimensions": [
                {
                    "value": "ResponseType",
                    "localizedValue": "Response type"
                },
                {
                    "value": "GeoType",
                    "localizedValue": "Geo type"
                },
                {
                    "value": "ApiName",
                    "localizedValue": "API name"
                }
            ]
        },
        ...
    ]
}
```

## <a name="retrieve-dimension-values-multi-dimensional-api"></a>Boyut değerleri (çok boyutlu API) alma

Kullanılabilir ölçüm tanımlarını bilinen sonra boyutlara sahip bazı ölçümler olabilir. Hangi değerleri aralığı bulmak isteyebilirsiniz ölçüm için sorgulama önce bir boyuta sahiptir. Bu boyut değerleri filtrelemek için daha sonra seçebilirsiniz veya segment ölçümleri sorgulanırken ölçümler için boyut değerlerini temel alan temel.  Kullanım [Azure İzleyici ölçümleri REST API](https://docs.microsoft.com/rest/api/monitor/metrics) Bunu başarmak için.

Ölçüm kişinin adı 'value' (değil ' localizedValue'), filtre tüm istekler için kullanın. Filtre belirtilmezse, varsayılan ölçümü döndürülür. Bu API kullanımı, yalnızca bir joker karakter filtresi bir boyut sağlar.

> [!NOTE]
> Azure İzleyici REST API'sini kullanarak boyut değerleri almak için "2018-01-01" API sürümü kullanın.
>
>

**Yöntemi**: GET

**İstek URI'si**: https\://management.azure.com/subscriptions/ *{abonelik-kimliği}* /resourceGroups/ *{kaynak-grup-adı}* /providers/ *{ Kaynak-sağlayıcısı-namespace}* / *{kaynak-türü}* / *{kaynak-adı}* /providers/microsoft.insights/metrics? metricnames = *{ölçümü}* & zaman aralığı = *{starttime/endtime}* & $filter = *{filter}* & resulttype'ı meta verileri & api sürümü == *{apiVersion}*

Örneğin, 'İşlemleri' ölçümü için 'API adı boyutu' için nereden yayılan boyut değerlerinin listesini almak için GeoType boyut 'Birincil' = belirtilen zaman aralığı içinde İstek şu şekilde olacaktır:

```powershell
$filter = "APIName eq '*' and GeoType eq 'Primary'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metrics?metricnames=Transactions&timespan=2018-03-01T00:00:00Z/2018-03-02T00:00:00Z&resultType=metadata&`$filter=${filter}&api-version=2018-01-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosostorage-dimension-values.json" `
    -Verbose
```

Sonuçta elde edilen JSON yanıt gövdesine aşağıdaki örneğe benzer olacaktır:

```JSON
{
  "timespan": "2018-03-01T00:00:00Z/2018-03-02T00:00:00Z",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Transactions",
        "localizedValue": "Transactions"
      },
      "unit": "Count",
      "timeseries": [
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "DeleteBlob"
            }
          ]
        },
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "SetBlobProperties"
            }
          ]
        },
        ...
      ]
    }
  ],
  "namespace": "Microsoft.Storage/storageAccounts",
  "resourceregion": "eastus"
}
```

## <a name="retrieve-metric-values-multi-dimensional-api"></a>Ölçüm değerleri (çok boyutlu API) alma

Kullanılabilir ölçüm tanımlarını ve olası boyut değerleri bilinen sonra ardından ilgili ölçüm değerleri almak mümkündür.  Kullanım [Azure İzleyici ölçümleri REST API](https://docs.microsoft.com/rest/api/monitor/metrics) Bunu başarmak için.

Ölçüm kişinin adı 'value' (değil ' localizedValue'), filtre tüm istekler için kullanın. Boyut filtre belirtilmezse, toplanan toplanmış ölçüm döndürülür. Ölçüm sorgusunu birden çok zaman serisi döndürürse, zaman serisi sınırlı sıralanmış bir listesini döndürmek için 'Üst' ve 'OrderBy' sorgu parametreleri kullanabilirsiniz.

> [!NOTE]
> Azure İzleyici REST API'sini kullanarak çok boyutlu ölçüm değerleri almak için "2018-01-01" API sürümü kullanın.
>
>

**Yöntemi**: GET

**İstek URI'si**: https://management.azure.com/subscriptions/ *{abonelik-kimliği}* /resourceGroups/ *{kaynak-grup-adı}* /providers/ *{kaynak-sağlayıcısı-namespace}* / *{kaynak-türü}* / *{kaynak-adı}* /providers/microsoft.insights/metrics?metricnames= *{} ölçümü*& zaman aralığı = *{starttime/endtime}* & $filter = *{filter}* & aralığı = *{timeGrain}* & toplama = *{ aggreation}* & api sürümü = *{apiVersion}*

Örneğin, ilk 3 almak için API'leri, azalan sırada bir 5 dakika aralık sırasında 'işlemleri' sayısına göre GeotType 'Birincil' olduğu değerini isteği şu şekilde olacaktır:

```powershell
$filter = "APIName eq '*' and GeoType eq 'Primary'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metrics?metricnames=Transactions&timespan=2018-03-01T02:00:00Z/2018-03-01T02:05:00Z&`$filter=${filter}&interval=PT1M&aggregation=Total&top=3&orderby=Total desc&api-version=2018-01-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosostorage-metric-values.json" `
    -Verbose
```

Sonuçta elde edilen JSON yanıt gövdesine aşağıdaki örneğe benzer olacaktır:

```JSON
{
  "cost": 0,
  "timespan": "2018-03-01T02:00:00Z/2018-03-01T02:05:00Z",
  "interval": "PT1M",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Transactions",
        "localizedValue": "Transactions"
      },
      "unit": "Count",
      "timeseries": [
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "GetBlobProperties"
            }
          ],
          "data": [
            {
              "timeStamp": "2017-09-19T02:00:00Z",
              "total": 2
            },
            {
              "timeStamp": "2017-09-19T02:01:00Z",
              "total": 1
            },
            {
              "timeStamp": "2017-09-19T02:02:00Z",
              "total": 3
            },
            {
              "timeStamp": "2017-09-19T02:03:00Z",
              "total": 7
            },
            {
              "timeStamp": "2017-09-19T02:04:00Z",
              "total": 2
            }
          ]
        },
        ...
      ]
    }
  ],
  "namespace": "Microsoft.Storage/storageAccounts",
  "resourceregion": "eastus"
}
```

## <a name="retrieve-metric-definitions"></a>Ölçüm tanımlarını Al

Kullanım [Azure İzleyici ölçüm tanımlarını REST API](https://msdn.microsoft.com/library/mt743621.aspx) hizmeti için kullanılabilir ölçüm listesine erişmek için.

**Yöntemi**: GET

**İstek URI'si**: https:\/\/management.azure.com/subscriptions/ *{Subscriptionıd}* /resourceGroups/ *{resourceGroupName}* /providers/ *{resourceProviderNamespace}* / *{resourceType}* / *{resourceName}* /providers/microsoft.insights/metricDefinitions?api-version= *{ apiVersion}*

Örneğin, bir Azure mantıksal uygulaması için ölçüm tanımları almak için isteği şu şekilde görünür:

```powershell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contosotweets-metricdef-results.json" `
                  -Verbose
```

> [!NOTE]
> Azure İzleyici REST API'sini kullanarak ölçüm tanımları almak için "2016-03-01" API sürümü kullanın.
>
>

Sonuçta elde edilen JSON yanıt gövdesine aşağıdaki örneğe benzer olacaktır:

```JSON
{
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricdefinitions",
  "value": [
    {
      "name": {
        "value": "RunsStarted",
        "localizedValue": "Runs Started"
      },
      "category": "AllMetrics",
      "startTime": "0001-01-01T00:00:00Z",
      "endTime": "0001-01-01T00:00:00Z",
      "unit": "Count",
      "primaryAggregationType": "Total",
      "resourceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets",
      "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets",
      "metricAvailabilities": [
        {
          "timeGrain": "PT1M",
          "retention": "P30D",
          "location": null,
          "blobLocation": null
        },
        {
          "timeGrain": "PT1H",
          "retention": "P30D",
          "location": null,
          "blobLocation": null
        }
      ],
      "properties": null,
      "dimensions": null,
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricdefinitions/RunsStarted",
      "supportedAggregationTypes": [ "None", "Average", "Minimum", "Maximum", "Total", "Count" ]
    }
  ]
}
```

Daha fazla bilgi için [Azure İzleyici REST API'si, bir kaynak için ölçüm tanımları listesi](https://msdn.microsoft.com/library/azure/mt743621.aspx) belgeleri.

## <a name="retrieve-metric-values"></a>Ölçüm değerleri alma

Kullanılabilir ölçüm tanımlarını bilinen sonra ardından ilgili ölçüm değerleri almak mümkündür. Filtreleme tüm istekler için ölçüm kişinin adı 'değeri' (değil ' localizedValue') kullanın (örneğin, 'CPU zamanı' ve 'İster' Ölçüm veri noktalarını alma). Filtre belirtilmezse, varsayılan ölçümü döndürülür.

> [!NOTE]
> Azure İzleyici REST API'sini kullanarak ölçüm değerleri almak için "2016-09-01" API sürümü kullanın.
>
>

**Yöntemi**: GET

**İstek URI'si**: https://management.azure.com/subscriptions/ *{abonelik-kimliği}* /resourceGroups/ *{kaynak-grup-adı}* /providers/ *{kaynak-sağlayıcısı-namespace}* / *{kaynak-türü}* / *{kaynak-adı}* /providers/microsoft.insights/metrics?$filter= *{filter}* & api sürümü = *{apiVersion}*

Örneğin, 1 saatlik bir zaman dilimi ve belirtilen zaman aralığı için RunsSucceeded ölçüm veri noktalarını almaya yönelik istek şu şekilde olacaktır:

```powershell
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2017-08-18T19:00:00 and endTime eq 2017-08-18T23:00:00 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosotweets-metrics-results.json" `
    -Verbose
```

Sonuçta elde edilen JSON yanıt gövdesine aşağıdaki örneğe benzer olacaktır:

```JSON
{
  "value": [
    {
      "data": [
        {
          "timeStamp": "2017-08-18T19:00:00Z",
          "total": 0.0
        },
        {
          "timeStamp": "2017-08-18T20:00:00Z",
          "total": 159.0
        },
        {
          "timeStamp": "2017-08-18T21:00:00Z",
          "total": 174.0
        },
        {
          "timeStamp": "2017-08-18T22:00:00Z",
          "total": 97.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/RunsSucceeded",
      "name": {
        "value": "RunsSucceeded",
        "localizedValue": "Runs Succeeded"
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```

Birden çok veri veya toplama noktalarını almak için ölçüm tanımı adlarını ve toplama türlerini filtresi için aşağıdaki örnekte görüldüğü gibi ekleyin:

```powershell
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2017-08-18T21:00:00 and endTime eq 2017-08-18T21:30:00 and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosotweets-metrics-multiple-results.json" `
    -Verbose
```

Sonuçta elde edilen JSON yanıt gövdesine aşağıdaki örneğe benzer olacaktır:

```JSON
{
  "value": [
    {
      "data": [
        {
          "timeStamp": "2017-08-18T21:03:00Z",
          "total": 5.0,
          "average": 1.0
        },
        {
          "timeStamp": "2017-08-18T21:04:00Z",
          "total": 7.0,
          "average": 1.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/ActionsCompleted",
      "name": {
        "value": "ActionsCompleted",
        "localizedValue": "Actions Completed "
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    },
    {
      "data": [
        {
          "timeStamp": "2017-08-18T21:03:00Z",
          "total": 5.0,
          "average": 1.0
        },
        {
          "timeStamp": "2017-08-18T21:04:00Z",
          "total": 7.0,
          "average": 1.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/RunsSucceeded",
      "name": {
        "value": "RunsSucceeded",
        "localizedValue": "Runs Succeeded"
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```

### <a name="use-armclient"></a>ARMClient kullanın

Ek bir yaklaşım kullanmaktır [ARMClient](https://github.com/projectkudu/armclient) Windows makinenizde. ARMClient Azure AD kimlik doğrulaması (ve sonuçta elde edilen JWT belirteci) otomatik olarak işler. Aşağıdaki adımlar, ölçüm verilerini almak için ARMClient kullanımını özetlemektedir:

1. Yükleme [Chocolatey](https://chocolatey.org/) ve [ARMClient](https://github.com/projectkudu/armclient).
2. Bir terminal penceresinde şunu yazın *armclient.exe oturum açma*. Bunun yapılması Azure'da oturum açmanızı ister.
3. Tür *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*
4. Tür *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-09-01*

Örneğin, belirli bir mantıksal uygulama için ölçüm tanımları almak için aşağıdaki komutu yürütün:

```
armclient GET /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01
```

## <a name="retrieve-the-resource-id"></a>Kaynak Kimliğini alma

REST API kullanarak gerçekten kullanılabilir ölçüm tanımları, ayrıntı düzeyi ve ilgili değerleri anlamanıza yardımcı olabilir. Bilgi kullanırken faydalı olduğunu [Azure Yönetimi Kitaplığı](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Yukarıdaki kod için kullanılacak kaynak kimliği istenen Azure kaynağına tam yoludur. Örneğin, bir Azure Web uygulaması karşı sorgulamak için kaynak Kimliğini olacaktır:

*/Subscriptions/{Subscription-id}/resourceGroups/{Resource-Group-Name}/providers/Microsoft.Web/Sites/{Site-Name}/*

Aşağıdaki liste, çeşitli Azure kaynakları için kaynak kodu biçimlerini birkaç örnekleri içerir:

* **IOT hub'ı** -/subscriptions/ *{abonelik-kimliği}* /resourceGroups/ *{kaynak-grup-adı}* /providers/Microsoft.Devices/IotHubs/ *{IOT-hub-adı}*
* **SQL esnek havuzu** -/subscriptions/ *{abonelik-kimliği}* /resourceGroups/ *{kaynak-grup-adı}* /providers/Microsoft.Sql/servers/ *{havuzu-db}* /elasticpools/ *{sql-havuzu-adı}*
* **SQL veritabanı (v12)** -/subscriptions/ *{abonelik-kimliği}* /resourceGroups/ *{kaynak-grup-adı}* /providers/Microsoft.Sql/servers/ *{sunucu-adı}* /databases/ *{veritabanı-adı}*
* **Service Bus** -/subscriptions/ *{abonelik-kimliği}* /resourceGroups/ *{kaynak-grup-adı}* /providers/Microsoft.ServiceBus/ *{namespace}* / *{servicebus-adı}*
* **Sanal makine ölçek kümeleri** -/subscriptions/ *{abonelik-kimliği}* /resourceGroups/ *{kaynak-grup-adı}* /providers/Microsoft.Compute/virtualMachineScaleSets/ *{vm-adı}*
* **Vm'leri** -/subscriptions/ *{abonelik-kimliği}* /resourceGroups/ *{kaynak-grup-adı}* /providers/Microsoft.Compute/virtualMachines/ *{vm-adı}*
* **Olay hub'ları** -/subscriptions/ *{abonelik-kimliği}* /resourceGroups/ *{kaynak-grup-adı}* /providers/Microsoft.EventHub/namespaces/ *{ eventhub namespace}*

Azure kaynak Gezgini, Azure portalında ve PowerShell veya Azure CLI aracılığıyla istenen kaynak görüntüleme kullanma dahil olmak üzere kaynak Kimliğini almak için alternatif bir yaklaşım vardır.

### <a name="azure-resource-explorer"></a>Azure Resource Manager

İstenen kaynak için kaynak Kimliğini bulmak için bir yardımcı yaklaşımdır [Azure kaynak Gezgini](https://resources.azure.com) aracı. İstenen kaynağa gidin ve ardından, aşağıdaki ekran görüntüsünde gösterildiği gibi gösterilen tanıtıcı bakın:

!["Azure kaynak Gezgini" alt](./media/rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure portal

Kaynak Kimliği, Azure portalından da alınabilir. Bunu yapmak için istenen kaynağa gidin ve ardından Özellikler'i seçin. Kaynak Kimliği, aşağıdaki ekran görüntüsünde görüldüğü gibi özellikleri bölümünde görüntülenir:

!["Azure portalında özellikler dikey penceresinde görüntülenen alt kaynak kimliği"](./media/rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell

Kaynak Kimliği, Azure PowerShell cmdlet'leri kullanılarak alınabilir. Örneğin, bir Azure mantıksal uygulaması için kaynak Kimliğini almak için aşağıdaki örnekte olduğu gibi Get-AzureLogicApp cmdlet'ini yürütün:

```powershell
Get-AzLogicApp -ResourceGroupName azmon-rest-api-walkthrough -Name contosotweets
```

Sonuç, aşağıdaki örneğe benzer olmalıdır:

```
Id             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets
Name           : ContosoTweets
Type           : Microsoft.Logic/workflows
Location       : centralus
ChangedTime    : 8/21/2017 6:58:57 PM
CreatedTime    : 8/18/2017 7:54:21 PM
AccessEndpoint : https://prod-08.centralus.logic.azure.com:443/workflows/f3a91b352fcc47e6bff989b85446c5db
State          : Enabled
Definition     : {$schema, contentVersion, parameters, triggers...}
Parameters     : {[$connections, Microsoft.Azure.Management.Logic.Models.WorkflowParameter]}
SkuName        :
AppServicePlan :
PlanType       :
PlanId         :
Version        : 08586982649483762729
```

### <a name="azure-cli"></a>Azure CLI

Azure CLI kullanarak bir Azure depolama hesabı kaynak kimliği almak için yürütme `az storage account show` aşağıdaki örnekte gösterildiği gibi komut:

```
az storage account show -g azmon-rest-api-walkthrough -n contosotweets2017
```

Sonuç, aşağıdaki örneğe benzer olmalıdır:

```JSON
{
  "accessTier": null,
  "creationTime": "2017-08-18T19:58:41.840552+00:00",
  "customDomain": null,
  "enableHttpsTrafficOnly": false,
  "encryption": null,
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/contosotweets2017",
  "identity": null,
  "kind": "Storage",
  "lastGeoFailoverTime": null,
  "location": "centralus",
  "name": "contosotweets2017",
  "networkAcls": null,
  "primaryEndpoints": {
    "blob": "https://contosotweets2017.blob.core.windows.net/",
    "file": "https://contosotweets2017.file.core.windows.net/",
    "queue": "https://contosotweets2017.queue.core.windows.net/",
    "table": "https://contosotweets2017.table.core.windows.net/"
  },
  "primaryLocation": "centralus",
  "provisioningState": "Succeeded",
  "resourceGroup": "azmon-rest-api-walkthrough",
  "secondaryEndpoints": null,
  "secondaryLocation": "eastus2",
  "sku": {
    "name": "Standard_GRS",
    "tier": "Standard"
  },
  "statusOfPrimary": "available",
  "statusOfSecondary": "available",
  "tags": {},
  "type": "Microsoft.Storage/storageAccounts"
}
```

> [!NOTE]
> Azure Logic Apps olmayan henüz Azure CLI aracılığıyla kullanılabilir, böylece bir Azure depolama hesabı önceki örnekte gösterilir.
>
>

## <a name="retrieve-activity-log-data"></a>Etkinlik günlüğü verileri alma

Ölçüm tanımlarını ve ilgili değerleri yanı sıra, ayrıca Azure kaynaklarla ilgili ek ilgi çekici bilgiler almak için Azure İzleyici REST API'sini kullanmak mümkündür. Örneğin, sorgu için olası [etkinlik günlüğü](https://msdn.microsoft.com/library/azure/dn931934.aspx) veri. Aşağıdaki örnek, bir Azure aboneliği için belirli bir tarih aralığı içinde sorgu etkinlik günlüğü verileri için Azure İzleyici REST API kullanarak göstermektedir:

```powershell
$apiVersion = "2015-04-01"
$filter = "eventTimestamp ge '2017-08-18' and eventTimestamp le '2017-08-19'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -Verbose
```

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [izlemeye genel bakış](../../azure-monitor/overview.md).
* Görünüm [Azure İzleyici ile desteklenen ölçümler](metrics-supported.md).
* Gözden geçirme [Microsoft Azure REST API Başvurusu izleme](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Gözden geçirme [Azure Yönetimi Kitaplığı](https://msdn.microsoft.com/library/azure/mt417623.aspx).