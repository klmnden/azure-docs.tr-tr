---
title: U-SQL betiği - Azure'ı kullanarak verileri dönüştürme | Microsoft Docs
description: Azure Data Lake Analytics işlem hizmetini U-SQL betikleri çalıştırarak işleme veya dönüştürme veri öğrenin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: abnarain
ms.openlocfilehash: d5b074fcf182bcc9bf4dc17ba21215d27e13cbdd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60888444"
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Üzerinde Azure Data Lake Analytics U-SQL betikleri çalıştırarak verileri dönüştürme 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-usql-activity.md)
> * [Geçerli sürüm](transform-data-using-data-lake-analytics.md)

Bir Azure data factory'de bir işlem hattı, bağlantılı depolama Hizmetleri'ndeki veri bağlı işlem hizmetlerini kullanarak işler. Bu etkinliklerin nerede her etkinlik bir özel işleme işlemi gerçekleştirir dizisi içerir. Bu makalede **Data Lake Analytics U-SQL etkinliği** çalıştırılan bir **U-SQL** üzerinde komut bir **Azure Data Lake Analytics** işlem bağlı hizmeti. 

Bir Azure Data Lake Analytics hesabı bir Data Lake Analytics U-SQL etkinliği ile işlem hattı oluşturmadan önce oluşturun. Azure Data Lake Analytics hakkında bilgi edinmek için [Azure Data Lake Analytics ile çalışmaya başlama](../data-lake-analytics/data-lake-analytics-get-started-portal.md).


## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics bağlı hizmeti
Oluşturduğunuz bir **Azure Data Lake Analytics** bir Azure Data Lake Analytics bağlamak için bağlı hizmet bir Azure data factory hizmetine işlem. Data Lake Analytics U-SQL etkinliği işlem hattındaki bu bağlı hizmetini ifade eder. 

Aşağıdaki tabloda JSON tanımında kullanılan genel özellikleri için açıklamalar sağlar. 

| Özellik                 | Açıklama                              | Gerekli                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| **type**                 | Type özelliği ayarlanmalıdır: **AzureDataLakeAnalytics**. | Evet                                      |
| **accountName**          | Azure Data Lake Analytics hesap adı.  | Evet                                      |
| **dataLakeAnalyticsUri** | Azure Data Lake Analytics URI.           | Hayır                                       |
| **Subscriptionıd**       | Azure abonelik kimliği                    | Hayır                                       |
| **resourceGroupName**    | Azure kaynak grubu adı                | Hayır                                       |

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması
Azure Data Lake Analytics bağlı hizmeti, Azure Data Lake Analytics hizmetine bağlanmak için bir hizmet sorumlusu kimlik doğrulaması gerektirir. Hizmet sorumlusu kimlik doğrulaması kullanmak için Azure Active Directory (Azure AD) uygulama varlığı Kaydet ve Data Lake Analytics hem de kullandığı Data Lake Store erişimi verin. Ayrıntılı adımlar için bkz. [hizmetten hizmete kimlik doğrulaması](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

* Uygulama Kimliği
* Uygulama anahtarı 
* Kiracı Kimliği

Hizmet sorumlusu izni kullanarak Azure Data Lake Anatlyics [kullanıcı ekleme sihirbazını](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#add-a-new-user).

Hizmet sorumlusu kimlik doğrulaması, aşağıdaki özellikleri belirterek kullanın:

| Özellik                | Açıklama                              | Gerekli |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Uygulamanın istemci kimliği belirtin.     | Evet      |
| **serviceprincipalkey değerleri** | Uygulama anahtarını belirtin.           | Evet      |
| **Kiracı**              | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet      |

**Örnek: Hizmet sorumlusu kimlik doğrulaması**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "<azure data lake analytics URI>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "value": "<service principal key>",
                "type": "SecureString"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }       
    }
}
```

Bağlı hizmet hakkında daha fazla bilgi için bkz: [işlem bağlı Hizmetleri](compute-linked-services.md).

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL Etkinliği
Aşağıdaki JSON kod parçacığında, bir Data Lake Analytics U-SQL etkinliği ile işlem hattı tanımlar. Etkinlik tanımı, daha önce oluşturduğunuz Azure Data Lake Analytics bağlı hizmeti için bir başvuru içeriyor. Bir Data Lake Analytics U-SQL betiği yürütmek için Data Factory için Data Lake Analytics belirtilen betik gönderir ve gerekli giriş ve çıkışları getirmek ve çıkış Data Lake Analytics komut dosyasında tanımlanır. 

```json
{
    "name": "ADLA U-SQL Activity",
    "description": "description",
    "type": "DataLakeAnalyticsU-SQL",
    "linkedServiceName": {
        "referenceName": "<linked service name of Azure Data Lake Analytics>",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "<linked service name of Azure Data Lake Store or Azure Storage which contains the U-SQL script>",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
        "degreeOfParallelism": 3,
        "priority": 100,
        "parameters": {
            "in": "/datalake/input/SearchLog.tsv",
            "out": "/datalake/output/Result.tsv"
        }
    }   
}
```

Aşağıdaki tabloda, adları ve açıklamaları bu etkinliğe özgü olan özellikleri açıklanmaktadır. 

| Özellik            | Açıklama                              | Gerekli |
| :------------------ | :--------------------------------------- | :------- |
| ad                | İşlem hattındaki etkinliğin adı     | Evet      |
| açıklama         | Etkinliğin ne yaptığını açıklayan metin.  | Hayır       |
| type                | Data Lake Analytics U-SQL etkinliği için etkinlik türdür **DataLakeAnalyticsU SQL**. | Evet      |
| linkedServiceName   | Azure Data Lake analytics'e bağlı hizmeti. Bu bağlı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi.  |Evet       |
| ScriptPath          | U-SQL komut dosyasını içeren klasörün yolu. Dosyanın adı büyük/küçük harfe duyarlıdır. | Evet      |
| scriptLinkedService | Bağlı bağlantı hizmeti **Azure Data Lake Store** veya **Azure depolama** data factory'ye betiği içeren | Evet      |
| degreeOfParallelism | Aynı anda işi çalıştırmak için kullanılan düğümlerin sayısı. | Hayır       |
| öncelik            | Sıraya alınan tüm önce çalıştırılması gerektiğini belirler. Alt sayısı, öncelik o kadar yüksektir. | Hayır       |
| parametreler          | U-SQL betiğe geçirilecek parametreler.    | Hayır       |
| runtimeVersion      | Çalışma zamanı sürümünü kullanmak için U-SQL altyapısı. | Hayır       |
| CompilationMode     | <p>U-SQL derleme modu. Şu değerlerden biri olmalıdır: **Anlam:** Yalnızca anlam denetimleri ve gerekli sağlamlık denetimleri gerçekleştirmek **tam:** Sözdizimi denetimi, en iyi duruma getirme, kod oluşturma, vb. dahil olmak üzere tam derleme gerçekleştirmek **SingleBox:** TargetType ayarıyla SingleBox tam derleme gerçekleştirin. Bu özellik için bir değer belirtmezseniz, sunucunun en iyi derleme modu belirler. | Hayır |

Bkz: [SearchLogProcessing.txt](#sample-u-sql-script) betik tanımı. 

## <a name="sample-u-sql-script"></a>Örnek U-SQL betiği

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

Yukarıdaki örnek betik, girdi ve çıktı betik içinde tanımlanan  **\@içinde** ve  **\@kullanıma** parametreleri. Değerleri  **\@içinde** ve  **\@kullanıma** U-SQL betiği parametrelerinde geçirilir dinamik olarak Data Factory tarafından 'parameters' bölümünü kullanarak. 

Azure Data Lake Analytics hizmeti üzerinde çalıştırılan işler için işlem hattı Tanımınızda degreeOfParallelism ve öncelik gibi diğer özellikleri de belirtebilirsiniz.

## <a name="dynamic-parameters"></a>Dinamik parametreler
Örnek işlem hattı tanımında, giriş ve çıkış parametreleri ile sabit kodlu değer atanır. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Bunun yerine dinamik parametreleri kullanmak mümkündür. Örneğin: 

```json
"parameters": {
    "in": "/datalake/input/@{formatDateTime(pipeline().parameters.WindowStart,'yyyy/MM/dd')}/data.tsv",
    "out": "/datalake/output/@{formatDateTime(pipeline().parameters.WindowStart,'yyyy/MM/dd')}/result.tsv"
}
```

Bu durumda, giriş dosyaları hala /datalake/input klasöründen seçilir ve çıkış dosyalarının /datalake/output klasöründe oluşturulur. Dosya adları, işlem hattı tetiklendiğinde geçirilen penceresi başlangıç zamanı temel alınarak dinamiktir.  

## <a name="next-steps"></a>Sonraki adımlar
Anlatan farklı yollarla verileri dönüştürmek aşağıdaki makalelere bakın: 

* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliğinde](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
