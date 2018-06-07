---
title: U-SQL betiği - Azure kullanarak veri dönüştürme | Microsoft Docs
description: Azure Data Lake Analytics işlem hizmette U-SQL betiklerini çalıştırarak nasıl işleneceğini veya dönüştürme veri öğrenin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: abnarain
ms.openlocfilehash: 1bf030d7eaba5c8aa608c504f65c5ebf291eab3d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34619703"
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Üzerinde Azure Data Lake Analytics U-SQL betiklerini çalıştırarak veri dönüştürme 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-usql-activity.md)
> * [Sürüm 2 - Önizleme](transform-data-using-data-lake-analytics.md)

Bir Azure data factory işlem hattı bağlantılı işlem hizmetlerini kullanarak bağlantılı depolama Hizmetleri'nde verilerini işler. Burada her etkinlik özel işleme işlemi gerçekleştiren etkinlikler dizisini içerir. Bu makalede **Data Lake Analytics U-SQL etkinliği** çalıştıran bir **U-SQL** üzerinde komut dosyası bir **Azure Data Lake Analytics** bağlantılı hizmet işlem. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 USQL etkinliğinde](v1/data-factory-usql-activity.md).

Bir Azure Data Lake Analytics hesabı, bir veri Lake Analytics U-SQL etkinliği ile işlem hattı oluşturmadan önce oluşturun. Azure Data Lake Analytics hakkında bilgi edinmek için [Azure Data Lake Analytics ile çalışmaya başlama](../data-lake-analytics/data-lake-analytics-get-started-portal.md).


## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics bağlı hizmeti
Oluşturduğunuz bir **Azure Data Lake Analytics** bir Azure Data Lake Analytics bağlamak için bağlantılı hizmeti bir Azure data factory hizmetine işlem. Data Lake Analytics U-SQL etkinliği ardışık düzeninde bu bağlı hizmetin başvuruyor. 

Aşağıdaki tabloda JSON tanımında kullanılan genel özellikleri için açıklamalar sağlanır. 

| Özellik                 | Açıklama                              | Gerekli                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| **type**                 | Type özelliği ayarlanmalıdır: **AzureDataLakeAnalytics**. | Evet                                      |
| **accountName**          | Azure Data Lake Analytics hesap adı.  | Evet                                      |
| **dataLakeAnalyticsUri** | Azure Data Lake Analytics URI.           | Hayır                                       |
| **Subscriptionıd**       | Azure abonelik kimliği                    | Hayır (belirtilmezse, data Factory abonelik kullanılır). |
| **resourceGroupName**    | Azure kaynak grubu adı                | Hayır (belirtilmezse, kaynak grubu data Factory kullanılır). |

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması
Azure Data Lake Analytics bağlı hizmeti Azure Data Lake Analytics Hizmeti'ne bağlanmak için bir hizmet asıl kimlik doğrulaması gerektirir. Hizmet asıl kimlik doğrulaması kullanmak için Azure Active Directory (Azure AD) bir uygulama varlığı kaydetmek ve Data Lake Analytics ve Data Lake Store kullandığı erişim. Ayrıntılı adımlar için bkz: [hizmeti için kimlik doğrulama](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlantılı hizmet tanımlamak için kullandığınız aşağıdaki değerleri not edin:

* Uygulama Kimliği
* Uygulama anahtarı 
* Kiracı Kimliği

Azure Data Lake Anatlyics kullanarak hizmet asıl iznini [Kullanıcı Ekleme Sihirbazı](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#add-a-new-user).

Hizmet asıl kimlik doğrulaması, aşağıdaki özellikleri belirterek kullanın:

| Özellik                | Açıklama                              | Gerekli |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Uygulamanın istemci kimliği belirtin.     | Evet      |
| **servicePrincipalKey** | Uygulamanın anahtarını belirtin.           | Evet      |
| **Kiracı**              | Uygulamanızın bulunduğu altında Kiracı bilgileri (etki alanı adı veya Kiracı kimliği) belirtin. Azure portalının sağ üst köşedeki fare gelerek alabilir. | Evet      |

**Örnek: Hizmet asıl kimlik doğrulaması**
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
            "tenant": "<tenant ID>",
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

Bağlantılı hizmeti hakkında daha fazla bilgi için bkz: [işlem bağlı Hizmetleri](compute-linked-services.md).

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL Etkinliği
Aşağıdaki JSON parçacığında, bir Data Lake Analytics U-SQL etkinliği ile işlem hattı tanımlar. Etkinlik tanımı daha önce oluşturduğunuz Azure Data Lake Analytics bağlantılı hizmeti bir başvuru içeriyor. Bir Data Lake Analytics U-SQL betiği çalıştırmak için veri fabrikası için Data Lake Analytics belirtilen komut dosyası gönderir ve getirebilir ve çıktı Data Lake Analytics komut dosyasında gerekli girişleri ve çıkışları tanımlanır. 

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

Aşağıdaki tabloda, adları ve açıklamaları bu etkinliğe özgü özellikleri açıklanmaktadır. 

| Özellik            | Açıklama                              | Gerekli |
| :------------------ | :--------------------------------------- | :------- |
| ad                | İşlem hattında etkinlik adı     | Evet      |
| açıklama         | Etkinlik yaptığı açıklayan metin.  | Hayır       |
| type                | Data Lake Analytics U-SQL etkinliği için etkinlik türüdür **DataLakeAnalyticsU SQL**. | Evet      |
| linkedServiceName   | Azure Data Lake Analytics bağlantılı hizmetine. Bu bağlantılı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi.  |Evet       |
| scriptPath          | U-SQL komut dosyasını içeren klasörün yolu. Dosyanın adı büyük/küçük harf duyarlıdır. | Evet      |
| scriptLinkedService | Bağlantılı bağlantılar hizmeti **Azure Data Lake Store** veya **Azure Storage** data factory betiğe içeren | Evet      |
| degreeOfParallelism | Aynı anda işi çalıştırmak için kullanılan düğümlerin sayısı. | Hayır       |
| öncelik            | İlk çalıştırmak için sıraya alınan tüm işlerden seçili belirler. Alt sayısı, öncelik o kadar yüksektir. | Hayır       |
| parametreler          | U-SQL komut dosyasına geçirilecek parametreler.    | Hayır       |
| runtimeVersion      | Çalışma zamanı sürümü kullanmak için U-SQL altyapısı. | Hayır       |
| compilationMode     | <p>U-SQL derleme modu. Şu değerlerden biri olmalıdır: **Semantic:** yalnızca anlamsal denetler ve gerekli sağlamlık denetimleri gerçekleştirmek **tam:** sözdizimi denetimi, en iyi duruma getirme, kod oluşturma, vb. dahil olmak üzere tam derleme gerçekleştirin., **SingleBox:** SingleBox için TargetType ayarıyla tam derleme gerçekleştirin. Bu özellik için bir değer belirtmezseniz, sunucu en iyi bir derleme moduna belirler. | Hayır |

Veri Fabrikası gönderir bakın [SearchLogProcessing.txt betik tanımı](#sample-u-sql-script) betik tanımı. 

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

İçinde Yukarıdaki örnek komut dosyası, giriş ve çıkış komut tanımlanmış **@in** ve **@out** parametreleri. Değerleri **@in** ve **@out** U-SQL betiği parametrelerinde geçirilir dinamik olarak tarafından veri fabrikası 'parameters' bölümünü kullanarak. 

DegreeOfParallelism ve öncelik gibi diğer özellikleri ardışık düzen tanımınızı Azure Data Lake Analytics hizmeti Çalıştır işleri için de belirtebilirsiniz.

## <a name="dynamic-parameters"></a>Dinamik parametreler
Örnek ardışık düzen tanımı'nda giriş ve çıkış parametreleri sabit kodlanmış değerlerle atandı. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Dinamik parametreleri bunun yerine kullanmak da mümkündür. Örneğin: 

```json
"parameters": {
    "in": "/datalake/input/@{formatDateTime(pipeline().parameters.WindowStart,'yyyy/MM/dd')}/data.tsv",
    "out": "/datalake/output/@{formatDateTime(pipeline().parameters.WindowStart,'yyyy/MM/dd')}/result.tsv"
}
```

Bu durumda, giriş dosyaları hala /datalake/input klasöründen toplanmış ve çıktı dosyaları /datalake/output klasöründe oluşturulur. Dosya adları, ardışık düzen tetiklendiğinde geçirilen penceresi başlangıç zamanı temel alınarak dinamik.  

## <a name="next-steps"></a>Sonraki adımlar
Diğer yollarla verileri dönüştürmek açıklanmaktadır aşağıdaki makalelere bakın: 

* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning toplu iş yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
