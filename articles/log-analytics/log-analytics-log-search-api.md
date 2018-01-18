---
title: "Günlük analizi oturum arama REST API | Microsoft Docs"
description: "Bu kılavuz, nasıl komutlarının nasıl kullanılacağını gösteren örnekler sağlar ve Operations Management Suite (OMS) günlük analizi arama REST API'sini kullanabilirsiniz tanımlayan temel bir öğretici sağlar."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: b4e9ebe8-80f0-418e-a855-de7954668df7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: bwren
ms.openlocfilehash: 0ca80408f8e8b2dae7ff35d50b3d2c41ae54d3d3
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="log-analytics-log-search-rest-api"></a>Günlük analizi günlük arama REST API'si

> [!IMPORTANT]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), için başvurmalıdır sonra [günlük arama API yeni sürümü için belge](https://dev.loganalytics.io/).  Bu eski API yine de yükseltilmiş bir çalışma alanı ile çalışabilir, ancak depracated yakında sunulacaktır.  Yeni API kullanmak için varolan çözümleri değiştirmeniz gerekir.

Bu kılavuz, günlük analizi Search REST API'sini nasıl kullanabileceğinize dair örnekler dahil olmak üzere temel bir öğretici sağlar. Günlük analizi Operations Management Suite (OMS) bir parçasıdır.


## <a name="overview-of-the-log-search-rest-api"></a>REST API günlük arama genel bakış
Günlük analizi arama REST API RESTful ve Azure Resource Manager API üzerinden erişilebilir. Bu makalede, API aracılığıyla erişme örnekler [ARMClient](https://github.com/projectkudu/ARMClient), Azure Kaynak Yöneticisi API'si çağırma basitleştiren bir açık kaynak komut satırı aracı. ARMClient kullanımını günlük analizi arama API erişmek için birçok seçenek biridir. Başka bir seçenek arama erişmek için cmdlet'leri içerir OperationalInsights için Azure PowerShell modülü kullanmaktır. Bu araçların, OMS çalışma alanları çağrı yapmak ve bunların içindeki arama komutları gerçekleştirmek için Azure Kaynak Yöneticisi API'si kullanabilir. API, arama sonuçlarını birçok farklı yolla programlı olarak kullanmanıza olanak sağlayan arama sonuçları JSON biçiminde çıkarır.

Azure Resource Manager aracılığıyla kullanılabilir bir [.NET kitaplığı](https://msdn.microsoft.com/library/azure/dn910477.aspx) ve [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx). Daha fazla bilgi için bağlantılı web sayfalarını gözden geçirin.

> [!NOTE]
> Gibi bir toplama komutunu kullanırsanız `|measure count()` veya `distinct`, aramak üzere yapılan her çağrı fazla 500.000 kayıt geri dönebilirsiniz. Bir toplama komutu dahil etmeyin aramaları fazla 5.000 kayıtları döndürür.
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Temel günlük analizi Search REST API'sini Öğreticisi
### <a name="to-use-armclient"></a>ARMClient kullanmak için
1. Yükleme [Chocolatey](https://chocolatey.org/), Windows için bir açık kaynak Paket Yöneticisi olduğu. Yönetici olarak bir komut istemi penceresi açın ve aşağıdaki komutu çalıştırın:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. Aşağıdaki komutu çalıştırarak ARMClient yükleyin:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-search-using-armclient"></a>ARMClient kullanarak bir arama yapmak için
1. Microsoft hesabınızı veya iş veya Okul hesabınızı kullanarak oturum açın:

    ```
    armclient login
    ```

    Başarılı oturum açma için belirli bir hesaba bağlı tüm abonelikleri listeler:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. Operations Management Suite çalışma alanları alın:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Başarılı bir Get çağrısı aboneliği ile bağlantılı tüm çalışma alanları çıkış:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Arama değişkeni oluşturun:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Yeni arama değişkenini kullanarak ara:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Günlük analizi arama REST API başvuru örnekleri
Aşağıdaki örnekler arama API nasıl kullanabileceğinizi gösterir.

### <a name="search---actionread"></a>Arama - eylem/okuma
**Örnek URL'si:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**İsteği:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
Aşağıdaki tabloda kullanılabilir olan özellikleri açıklanmaktadır.

| **Özellik** | **Açıklama** |
| --- | --- |
| Sayfanın Üstü |Döndürülecek sonuç sayısı. |
| vurgula |Eşleşen alanları vurgulamak için genellikle kullanılan öncesi ve sonrası parametreleri içerir |
| pre |Eşleşen alanlarınızı verilen dizeye ekler. |
| Yayınla |Verilen dize eşleşen alanlarınızı ekler. |
| sorgu |Arama sorgusu toplamak ve sonuçları döndürmek için kullanılır. |
| start |Gelen bulunacak sonuçlar istediğiniz zaman penceresi başlangıcı. |
| end |Gelen bulunacak sonuçlar istediğiniz zaman penceresi sonu. |

**Yanıtı:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Arama / {kimliği} - eylem/okuma
**Kayıtlı arama içeriğini isteyin:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> Güncelleştirilmiş sonuçları yoklama arama 'Bekleyen' durumuyla döndürürse, bu API aracılığıyla yapılabilir. 6 min sonra arama sonucu önbellekten bırakılacak ve HTTP gitti döndürülür. İlk arama isteği 'Başarılı' durumunu hemen döndürürse, sonuçları HTTP gitti sorgularsa döndürmek bu API neden önbellek eklenmez. Bir HTTP 200 sonuç içeriğini aynı güncelleştirilmiş değerlerle yalnızca ilk arama isteği olarak biçimindedir.
>
>

### <a name="saved-searches"></a>Kaydedilen aramalar
**Kaydedilmiş aramaları listesini isteyin:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Algoritmalardan: GET, PUT Sil

Koleksiyon yöntemleri desteklenir: Al

Aşağıdaki tabloda kullanılabilir olan özellikleri açıklanmaktadır.

| Özellik | Açıklama |
| --- | --- |
| Kimlik |Benzersiz tanımlayıcı. |
| ETag |**Düzeltme eki için gerekli**. Sunucu tarafından her yazma güncelleştirildi. Değer geçerli saklı değerine eşit olmalı veya ' *' güncelleştirilemedi. 409 eski/geçersiz değerler için döndürdü. |
| properties.query |**Gerekli**. Arama sorgusu. |
| properties.displayName |**Gerekli**. Sorgu kullanıcı tanımlı görünen adı. |
| properties.category |**Gerekli**. Kullanıcı tanımlı kategori sorgunun. |

> [!NOTE]
> Günlük analizi arama API şu anda kayıtlı bir çalışma alanında Kaydedilmiş aramaları için sorgulanan zaman aramalar kullanıcı tarafından oluşturulan döndürür. API çözümler tarafından sağlanan Kaydedilmiş aramaları döndürmez.
>
>

### <a name="create-saved-searches"></a>Kaydedilen Aramalar oluşturun
**İsteği:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> Tüm kayıtlı aramaları, çizelgeler ve günlük analizi API ile oluşturulan eylemler için ad, küçük olması gerekir.

### <a name="delete-saved-searches"></a>Kaydedilmiş aramaları silin
**İsteği:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Kaydedilmiş aramaları güncelleştirme
 **İsteği:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Meta veriler - yalnızca JSON
Burada tüm günlük türleri çalışma alanınızda toplanan veriler için alanları görmek için bir yoldur. Örneğin, olay türünü bilgisayar adında bir alan olup olmadığını bilmek istiyorsanız, bu sorgu kontrol etmenin bir yolu değil.

**İsteği alanları için:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Yanıtı:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

Aşağıdaki tabloda kullanılabilir olan özellikleri açıklanmaktadır.

| **Özellik** | **Açıklama** |
| --- | --- |
| ad |Alan adı. |
| displayName |Alan görünen adı. |
| type |Alan değeri türü. |
| modellenebilir |'Dizine', geçerli birleşimi ' depolanan ' ve 'model' özellikleri. |
| Görüntüleme |Geçerli 'display' özelliği. Alan aramada görünür ise true. |
| ownerType |Yalnızca edildi IP's ait türlerinde azalır. |

## <a name="optional-parameters"></a>İsteğe bağlı parametreler
Aşağıdaki bilgiler kullanılabilen isteğe bağlı parametreleri açıklar.

### <a name="highlighting"></a>Vurgulama
"Vurgulama" parametresi arama alt sistemi istemek için kullanmak isteğe bağlı bir parametre yanıt olarak bir dizi işaretçileri dahil edilir.

Bu işaretçileri başlangıç belirtmek ve arama sorgunuzu sağlanan koşulları eşleşen vurgulanan metin bitmelidir.
Arama tarafından vurgulanan terim sarmalamak için kullanılan başlangıç ve bitiş işaretçilerini belirtebilir.

**Örnek arama sorgusu**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Örnek sonuçları:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Önceki sonuç öneki ve eklenmiş olan bir hata iletisi içerdiğine dikkat edin.

## <a name="computer-groups"></a>Bilgisayar grupları
Bilgisayar, bir bilgisayar kümesi döndüren özel Kaydedilmiş aramaları gruplarıdır.  Diğer sorgular bir bilgisayar grubu gruptaki bilgisayarların sonuçları sınırlamak için kullanabilirsiniz.  Bir bilgisayar grubu, bilgisayar değerini içeren bir Grup etiketi bir kayıtlı arama olarak uygulanır.

Bir bilgisayar grubu için bir örnek yanıt aşağıdadır.

```
    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
          }],
    "Version": 1
    }
```

### <a name="retrieving-computer-groups"></a>Bilgisayar grupları alınıyor
Bir bilgisayar grubu almak için Grup kimliğiyle Get yöntemini kullanın

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Oluştururken veya güncelleştirirken bir bilgisayar grubu
Bir bilgisayar grubu oluşturmak için bir kayıtlı arama benzersiz kimliğe sahip Put yöntemini kullanın Var olan bir bilgisayar grubu kimliği kullanırsanız, bir değiştirilir. Günlük analizi portalında bir bilgisayar grubu oluşturduğunuzda, kimlik grup ve adını oluşturulur.

Grup tanımı için kullanılan sorgu düzgün çalışması grubu için bir bilgisayarlar kümesi döndürmelidir.  Sorgunuz ile sona önerilir `| Distinct Computer` doğru veri döndürüldüğünden emin olmak için.

Kayıtlı aramanın tanımı bir bilgisayar grubu olarak sınıflandırılabilir bilgisayara arama değeriyle Grup adlı bir etiketi eklemeniz gerekir.

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a>Bilgisayar gruplarını silme
Bir bilgisayar grubu silmek için Grup kimliğiyle Delete yöntemini kullanın

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) özel alanlar için ölçüt kullanarak sorguları oluşturmak için.
