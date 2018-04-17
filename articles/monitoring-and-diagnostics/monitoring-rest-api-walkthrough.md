---
title: Azure REST API izleme gözden geçirme | Microsoft Docs
description: Nasıl isteklerinin kimlik doğrulaması ve kullanılabilir ölçüm tanımlarını ve ölçüm değerleri almak için Azure İzleyici REST API'sini kullanın.
author: mcollier
manager: ''
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 565e6a88-3131-4a48-8b82-3effc9a3d5c6
ms.service: monitoring-and-diagnostics
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.search.region: ''
ms.search.scope: ''
ms.search.validFrom: ''
ms.dyn365.ops.version: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: mcollier
ms.openlocfilehash: a87f60b04806fb337a9b4558a67ffa11da661ad5
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure REST API izleme gözden geçirme
Bu makalede, kodunuzu kullanabilmeniz için kimlik doğrulaması yapmak gösterilmiştir [Microsoft Azure İzleyici REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

Azure İzleyici API program aracılığıyla kullanılabilir varsayılan ölçüm tanımlarını, ayrıntı düzeyi ve ölçüm değerleri almak mümkün kılar. Azure SQL Database, Azure Cosmos DB ya da Azure Data Lake gibi ayrı veri deposundaki verileri kaydedilebilir. Buradan, gerektiği gibi ek çözümleme gerçekleştirilebilir.

Çeşitli ölçüm veri noktaları ile çalışma yanı sıra, monitör API'si Ayrıca, uyarı kuralları, etkinlik günlükleri görüntüle ve daha fazlasını listelemek mümkün kılar. Kullanılabilir işlemleri tam bir listesi için bkz: [Microsoft Azure İzleyici REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Kimlik doğrulama Azure İzleyici istekleri
İlk istek kimliğini doğrulamak için bir adımdır.

Azure İzleyici API karşı yürütülen tüm görevler Azure Resource Manager kimlik doğrulama modeli kullanır. Bu nedenle, tüm istekleri Azure Active Directory (Azure AD) ile kimlik doğrulaması gerekir. İstemci uygulaması kimliğini doğrulamak için bir Azure AD hizmet sorumlusu oluşturmak ve kimlik doğrulama (JWT) belirteci almak için bir yaklaşımdır. Aşağıdaki örnek betik, bir Azure AD hizmeti PowerShell aracılığıyla asıl oluşturmayı gösterir. Üzerinde daha ayrıntılı bir kılavuz için belgelere bakın [kaynaklara erişmek için bir hizmet sorumlusu oluşturmak için Azure PowerShell kullanarak](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps). Ayrıca mümkün [Azure Portalı aracılığıyla hizmet sorumlusu oluşturmak](../azure-resource-manager/resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"
$secureStringPassword = ConvertTo-SecureString -String $pwd -AsPlainText -Force

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $secureStringPassword

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Azure İzleyici API sorgulamak için istemci uygulamasının daha önce oluşturulan hizmet asıl kimlik doğrulaması için kullanmanız gerekir. Aşağıdaki örnek PowerShell komut dosyasını bir gösterir yaklaşımını kullanarak [Active Directory kimlik doğrulama Kitaplığı](../active-directory/active-directory-authentication-libraries.md) JWT kimlik doğrulama belirteci almak için (ADAL). JWT belirteci Azure İzleyici REST API istekleri HTTP yetkilendirme parametresinde bir parçası olarak geçirilir.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.microsoftonline.com/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $secureStringPassword)

$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Doğrulandıktan sonra sorguları, ardından karşı Azure İzleyici REST API çalıştırılabilir. İki yararlı sorgular şunlardır:

1. Bir kaynak için ölçüm açıklamalarının listesi
2. Ölçü değerlerini alma

## <a name="retrieve-metric-definitions-multi-dimensional-api"></a>Ölçüm tanımlarını (çok boyutlu API) alma

Kullanım [Azure İzleyici ölçüm tanımlarını REST API](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) hizmeti için kullanılabilir ölçümleri listesi erişmek için.

**Yöntem**: Al

**İstek URI'si**: https://management.azure.com/subscriptions/ *{Subscriptionıd}*/resourceGroups/*{resourceGroupName}*/providers/*{resourceProviderNamespace}* / *{resourceType}*/*{resourceName}*/providers/microsoft.insights/metricDefinitions?api-version=*{apiVersion}*

Örneğin, bir Azure depolama hesabı için ölçüm tanımlarını almak için isteği şu şekilde görünür:

```PowerShell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricDefinitions?api-version=2018-01-01"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contosostorage-metricdef-results.json" `
                  -Verbose

```
> [!NOTE]
> Çok boyutlu Azure İzleyici ölçümleri REST API kullanarak ölçüm tanımlarını almak için "2018-01-01" API sürümü kullanın.
>
>

Sonuçta elde edilen JSON yanıt gövdesine aşağıdaki örneğe benzer olmalıdır: (ikinci ölçüm boyutları sahip olduğuna dikkat edin)

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

## <a name="retrieve-dimension-values-multi-dimensional-api"></a>Boyut değerlerini (çok boyutlu API) alma
Kullanılabilir ölçüm tanımlarını bilinen sonra boyutlarda bazı ölçümleri olabilir. Hangi değerleri aralığı bulmak istediğiniz ölçümü için sorgulama önce bir boyutu vardır. Ardından filtre uygulamak için seçebileceğiniz bu boyut değerleri veya ölçümleri sorgularken ölçümleri boyut değerlerine göre segment göre.  Kullanım [Azure İzleyici ölçümleri REST API](https://docs.microsoft.com/rest/api/monitor/metrics) Bunu başarmak için.

Ölçüm kişinin adı 'value' (değil ' localizedValue') filtreleme tüm istekleri için kullanır. Hiçbir filtre belirtilmezse, varsayılan ölçü döndürülür. Bu API'ın kullanımını, yalnızca bir joker karakter filtresi bir boyut izin verir.

> [!NOTE]
> Azure İzleyici REST API'sini kullanarak boyut değerleri almak için "2018-01-01" API sürümü kullanın.
>
>

**Yöntem**: Al

**İstek URI'si**: https://management.azure.com/subscriptions/ *{subscrıptıon-ID}*/resourceGroups/*{kaynak grubu adı}*/providers/*{kaynak-sağlayıcısı-namespace}* / *{kaynak türü}*/*{kaynak-adı}*/providers/microsoft.insights/metrics?metric=*{ölçüm}* & timespan =*{starttime/endtime}*& $filter =*{filtre}*& resultType = meta veri & api-version =*{apiVersion}*

Örneğin, 'API adı boyutu için' 'İşlemleri' ölçümü için burada gösterilen boyut değerlerinin listesini almak için GeoType boyut = 'Primary' belirtilen zaman aralığı içinde istek aşağıdaki gibi olur:

```PowerShell
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

## <a name="retrieve-metric-values-multi-dimensional-api"></a>Ölçü değerlerini (çok boyutlu API) alma
Kullanılabilir ölçüm tanımlarını ve olası boyut değerlerini bilinen sonra ardından ilgili ölçüm değerleri almak mümkündür.  Kullanım [Azure İzleyici ölçümleri REST API](https://docs.microsoft.com/rest/api/monitor/metrics) Bunu başarmak için.

Ölçüm kişinin adı 'value' (değil ' localizedValue') filtreleme tüm istekleri için kullanır. Hiçbir boyut filtreleri belirtilirse, toplanan toplanmış ölçüm döndürülür. Ölçüm sorguda birden çok serisi döndürürse, serisi sınırlı sıralı bir listesi dönmek için 'Top' ve 'OrderBy' sorgu parametreleri kullanabilirsiniz.

> [!NOTE]
> Azure İzleyici REST API'sini kullanarak çok boyutlu ölçüm değerleri almak için "2018-01-01" API sürümü kullanın.
>
>

**Yöntem**: Al

**İstek URI'si**: https://management.azure.com/subscriptions/ *{subscrıptıon-ID}*/resourceGroups/*{kaynak grubu adı}*/providers/*{kaynak-sağlayıcısı-namespace}* / *{kaynak türü}*/*{kaynak-adı}*/providers/microsoft.insights/metrics?metric=*{ölçüm}* & timespan =*{starttime/endtime}*& $filter =*{filtre}*& aralığı =*{timeGrain}*& toplama =*{ aggreation}*& api-version =*{apiVersion}*

Örneğin, en üst 3 almak için API'ler, azalan sırada, 5 dk. aralığı sırasında 'işlem' sayısı tarafından GeotType 'Birincil' olduğu değer isteği aşağıdaki gibi olur:

```PowerShell
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
Kullanım [Azure İzleyici ölçüm tanımlarını REST API](https://msdn.microsoft.com/library/mt743621.aspx) hizmeti için kullanılabilir ölçümleri listesi erişmek için.

**Yöntem**: Al

**İstek URI'si**: https://management.azure.com/subscriptions/ *{Subscriptionıd}*/resourceGroups/*{resourceGroupName}*/providers/*{resourceProviderNamespace}* / *{resourceType}*/*{resourceName}*/providers/microsoft.insights/metricDefinitions?api-version=*{apiVersion}*

Örneğin, bir Azure mantıksal uygulama ölçüm tanımlarını almak için isteği şu şekilde görünür:

```PowerShell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contostweets-metricdef-results.json" `
                  -Verbose
```
> [!NOTE]
> Azure İzleyici REST API'sini kullanarak ölçüm tanımlarını almak için "2016-03-01" API sürümü kullanın.
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

Daha fazla bilgi için bkz: [Azure İzleyici REST API kaynak ölçüm tanımlarını listesinde](https://msdn.microsoft.com/library/azure/mt743621.aspx) belgeleri.

## <a name="retrieve-metric-values"></a>Ölçü değerlerini alma
Kullanılabilir ölçüm tanımlarını bilinen sonra ardından ilgili ölçüm değerleri almak mümkündür. Filtreleme tüm istekler için ölçüm kişinin adı 'value' (değil ' localizedValue') kullanın (örneğin, 'CPU zamanı' ve 'İsteklerini' Ölçüm veri noktalarını almaya). Hiçbir filtre belirtilmezse, varsayılan ölçü döndürülür.

> [!NOTE]
> Azure İzleyici REST API'sini kullanarak ölçüm değerleri almak için "2016-09-01" API sürümü kullanın.
>
>

**Yöntem**: Al

**İstek URI'si**: https://management.azure.com/subscriptions/ *{subscrıptıon-ID}*/resourceGroups/*{kaynak grubu adı}*/providers/*{kaynak-sağlayıcısı-namespace}* / *{kaynak türü}*/*{kaynak-adı}*/providers/microsoft.insights/metrics?$filter=*{filtre}*& api-version =*{apiVersion}*

Örneğin, 1 saatlik bir zaman çizgisi ve belirli bir zaman aralığı için RunsSucceeded ölçüm veri noktalarını almak için isteği aşağıdaki gibi olur:

```PowerShell
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2017-08-18T19:00:00 and endTime eq 2017-08-18T23:00:00 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contostweets-metrics-results.json" `
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

Birden çok veri veya toplama noktası almak için ölçüm tanımı adları ve toplama türleri filtre için aşağıdaki örnekte görüldüğü gibi ekleyin:

```PowerShell
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2017-08-18T21:00:00 and endTime eq 2017-08-18T21:30:00 and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contostweets-metrics-multiple-results.json" `
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
Ek bir yaklaşım kullanmaktır [ARMClient](https://github.com/projectkudu/armclient) Windows makinenizde. ARMClient Azure AD kimlik doğrulaması (ve sonuçta elde edilen JWT belirteci) otomatik olarak yönetir. ARMClient kullanımını ölçüm verileri almak için aşağıdaki adımları özetlemektedir:

1. Yükleme [Chocolatey](https://chocolatey.org/) ve [ARMClient](https://github.com/projectkudu/armclient).
2. Bir terminal penceresi yazın *armclient.exe oturum açma*. Bunun yapılması Azure'da oturum açma ister.
3. Tür *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*
4. Tür *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-09-01*

Örneğin, belirli bir mantıksal uygulama ölçüm tanımlarını almak için aşağıdaki komutu yürütün:
```
armclient GET /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01
```


## <a name="retrieve-the-resource-id"></a>Kaynak Kimliği alma
REST API kullanarak gerçekten kullanılabilir ölçüm tanımlarını, ayrıntı düzeyi ve ilgili değerleri anlamanıza yardımcı olabilir. Bilgi kullanıldığında yararlı olduğunu [Azure Yönetimi Kitaplığı](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Önceki kod için kullanılacak kaynak kimliği istenen Azure kaynak tam yoludur. Örneğin, bir Azure Web uygulaması karşı sorgulamak için kaynak kimliği olur:

*/Subscriptions/{Subscription-id}/resourceGroups/{Resource-Group-Name}/providers/Microsoft.Web/Sites/{Site-Name}/*

Aşağıdaki listede kaynak kimliği biçimlerinden çeşitli Azure kaynakları için birkaç örnek içerir:

* **IOT hub'ı** -/subscriptions/*{subscrıptıon-ID}*/resourceGroups/*{kaynak grubu adı}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-adı}*
* **SQL esnek havuzu** -/subscriptions/*{subscrıptıon-ID}*/resourceGroups/*{kaynak grubu adı}*/providers/Microsoft.Sql/servers/*{havuzu-db}*/elasticpools/*{sql-havuzu-adı}*
* **SQL veritabanı (v12)** -/subscriptions/*{subscrıptıon-ID}*/resourceGroups/*{kaynak grubu adı}*/providers/Microsoft.Sql/servers/*{sunucu-adı}*/databases/*{veritabanı-adı}*
* **Hizmet veri yolu** -/subscriptions/*{subscrıptıon-ID}*/resourceGroups/*{kaynak grubu adı}*/providers/Microsoft.ServiceBus/*{ad}* / *{servicebus-adı}*
* **Sanal makine ölçek kümeleri** -/subscriptions/*{subscrıptıon-ID}*/resourceGroups/*{kaynak grubu adı}*/providers/Microsoft.Compute/virtualMachineScaleSets/ *{vm-adı}*
* **Sanal makineleri** -/subscriptions/*{subscrıptıon-ID}*/resourceGroups/*{kaynak grubu adı}*/providers/Microsoft.Compute/virtualMachines/*{vm-adı}*
* **Olay hub'ları** -/subscriptions/*{subscrıptıon-ID}*/resourceGroups/*{kaynak grubu adı}*/providers/Microsoft.EventHub/namespaces/*{ eventhub-namespace}*

Azure kaynak Gezgini, istenen kaynak Azure portalında ve PowerShell veya Azure CLI aracılığıyla görüntüleme kullanımı dahil olmak üzere kaynak kimliği alma için alternatif bir yaklaşım vardır.

### <a name="azure-resource-explorer"></a>Azure Resource Manager
İstenen kaynak için kaynak Kimliğini bulmak için kullanmak için yararlı bir yaklaşım ise [Azure kaynak Gezgini](https://resources.azure.com) aracı. İstenen kaynağa gidin ve ardından, olduğu gibi aşağıdaki ekran görüntüsünde gösterilen tanıtıcı bakın:

![Alt "Azure kaynak Gezgini"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure portalına
Kaynak Kimliği de Azure portalından elde edilebilir. Bunu yapmak için istenen kaynağa gidin ve ardından Özellikler'i seçin. Kaynak Kimliği, aşağıdaki ekran görüntüsünde görüldüğü gibi özellikleri bölümünde görüntülenir:

!["Alt kaynak kimliği özellikleri dikey penceresinde Azure portalında görüntülenen"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
Kaynak Kimliği, Azure PowerShell cmdlet'leri kullanılarak alınabilir. Örneğin, bir Azure mantıksal uygulama için kaynak Kimliğini almak için aşağıdaki örnekte olduğu gibi Get-AzureLogicApp cmdlet'ini yürütün:

```PowerShell
Get-AzureRmLogicApp -ResourceGroupName azmon-rest-api-walkthrough -Name contosotweets
```

Sonuç aşağıdaki örneğe benzer olmalıdır:
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
Azure CLI kullanarak bir Azure depolama hesabı için kaynak kimliği almak için aşağıdaki örnekte gösterildiği gibi 'az depolama hesabı Göster' komutunu yürütün:

```
az storage account show -g azmon-rest-api-walkthrough -n contosotweets2017
```

Sonuç aşağıdaki örneğe benzer olmalıdır:
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
> Azure mantıksal uygulamaları olmayan henüz Azure CLI aracılığıyla kullanılabilir, böylece bir Azure Storage hesabı önceki örnekte gösterilir.
>
>

## <a name="retrieve-activity-log-data"></a>Etkinlik günlüğü verilerini alma
Ölçüm tanımlarını ve ilgili değerleri ek olarak, ayrıca Azure kaynaklarla ilgili ek ilginç Öngörüler almak için Azure İzleyici REST API kullanmak da mümkündür. Örnek olarak, sorguyu mümkündür [etkinlik günlüğü](https://msdn.microsoft.com/library/azure/dn931934.aspx) veri. Aşağıdaki örnek, Azure aboneliği için sorgu etkinlik günlüğü verileri belirli bir tarih aralığı içinde Azure İzleyici REST API'sini kullanarak gösterir:

```PowerShell
$apiVersion = "2015-04-01"
$filter = "eventTimestamp ge '2017-08-18' and eventTimestamp le '2017-08-19'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -Verbose
```

## <a name="next-steps"></a>Sonraki adımlar
* Gözden geçirme [izleme genel bakış](monitoring-overview.md).
* Görünüm [desteklenen Azure İzleyicisi ile ölçümleri](monitoring-supported-metrics.md).
* Gözden geçirme [Microsoft Azure izlemek REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Gözden geçirme [Azure Yönetimi Kitaplığı](https://msdn.microsoft.com/library/azure/mt417623.aspx).
