---
title: "Öğretici: Tanılama ve etkinlik günlüğü verileri Azure veri Gezgini'nde bir kod satırı olmadan alma"
description: Bu öğreticide, verileri Azure Veri Gezgini veri olmadan tek satırlık bir kod ve sorgu alma konusunda bilgi edinin.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: tutorial
ms.date: 2/5/2019
ms.openlocfilehash: 39019c4b11d055aa8f550928bd677e4ce33d6252
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885603"
---
# <a name="tutorial-ingest-data-in-azure-data-explorer-without-one-line-of-code"></a>Öğretici: Azure veri Gezgini'nde verileri tek satırlık bir kod olmadan alma

Bu öğreticide nasıl Tanılama alma ve etkinlik günlüğü verileri bir Azure Veri Gezgini kümesine bir kod satırı olmadan sağlanır. Bu basit alma yöntemi, veri analizi için Azure Veri Gezgini sorgulama hızlıca başlamak sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir Azure Veri Gezgini veritabanında tablolar ve alım eşlemesi oluşturun.
> * Güncelleştirme İlkesi'ni kullanarak içe alınan veri biçimi.
> * Oluşturma bir [olay hub'ı](/azure/event-hubs/event-hubs-about) ve Azure veri Gezgini'ne bağlanın.
> * Veri bir olay Hub'ından Stream [Azure İzleyici tanılama günlükleri](/azure/azure-monitor/platform/diagnostic-logs-overview) ve [Azure İzleyici etkinlik günlüklerini](/azure/azure-monitor/platform/activity-logs-overview).
> * Azure Veri Gezgini'ni kullanarak alınan verileri sorgulayın.

> [!NOTE]
> Tüm kaynaklar aynı Azure konumuna/bölgede oluşturun. Bu, Azure İzleyici tanılama günlükleri için bir zorunluluktur.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Bir Azure Veri Gezgini kümesi ile veritabanı](create-cluster-database-portal.md). Bu öğreticide, veritabanı adı. *AzureMonitoring*.

## <a name="azure-monitoring-data-provider---diagnostic-and-activity-logs"></a>Azure veri sağlayıcısı - Tanılama izleme ve etkinlik günlükleri

Görüntüleyebilir ve Azure izleme tanılama ve etkinlik günlükleri tarafından sağlanan verileri anlayın. Bu veri şemalarını temel alan bir alma işlem hattı oluşturacağız.

### <a name="diagnostic-logs-example"></a>Tanılama günlüklerini örnek

Azure tanılama günlükleri, hizmetin çalışması hakkında veriler sağlayan bir Azure hizmeti tarafından yayılan ölçümleridir. Veriler ile 1 dakikalık zaman dilimi toplanır. Her bir tanılama günlükleri Olay, bir kayıt içerir. Bir Azure Veri Gezgini ölçüm olayı şeması örneği üzerinde sorgu süresi aşağıdadır:

```json
{
    "count": 14,
    "total": 0,
    "minimum": 0,
    "maximum": 0,
    "average": 0,
    "resourceId": "/SUBSCRIPTIONS/F3101802-8C4F-4E6E-819C-A3B5794D33DD/RESOURCEGROUPS/KEDAMARI/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/KEREN",
    "time": "2018-12-20T17:00:00.0000000Z",
    "metricName": "QueryDuration",
    "timeGrain": "PT1M"
}
```

### <a name="activity-logs-example"></a>Etkinlik günlükleri örneği

Azure etkinlik günlükleri, kayıtların bir koleksiyonunu içeren abonelik düzeyi günlüklerdir. Günlükleri, aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Tanılama günlükleri, bir etkinlik günlükleri Olay kayıtlarını bir dizi sahiptir. Öğreticide daha sonra bu kayıtları dizisi bölmek gerekir. Erişim denetimi için bir etkinlik günlüğü olayında'nın bir örneği verilmiştir:

```json
{
    "records": [
    {
        "time": "2018-12-26T16:23:06.1090193Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Start",
        "resultSignature": "Started.",
        "durationMs": 0,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            ...
        }
    },
    {
        "time": "2018-12-26T16:23:06.3040244Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Success",
        "resultSignature": "Succeeded.OK",
        "durationMs": 194,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            "statusCode": "OK",
            "serviceRequestId": "87acdebc-945f-4c0c-b931-03050e085626"
        }
    }]
}
```

## <a name="set-up-ingestion-pipeline-in-azure-data-explorer"></a>Azure veri Gezgini'nde alma işlem hattı ayarlama 

Azure Veri Gezgini işlem hattı Kurulum dahil çeşitli adımları içerir [tablo oluşturma ve veri alımı](/azure/data-explorer/ingest-sample-data#ingest-data). Düzenleme, eşleyin ve verileri güncelleştirin.

### <a name="connect-to-azure-data-explorer-web-ui"></a>Azure Veri Gezgini Web kullanıcı Arabirimine bağlanma

1. Azure veri Gezgini'nde *AzureMonitoring* veritabanı **sorgu**, Azure Veri Gezgini web kullanıcı arabirimini açar.

    ![Sorgu](media/ingest-data-no-code/query-database.png)

### <a name="create-target-tables"></a>Hedef Tablo oluşturma

Azure Veri Gezgini veritabanında hedef tablolar oluşturmak için Azure Veri Gezgini web kullanıcı arabirimini kullanın.

#### <a name="diagnostic-logs-table"></a>Tanılama günlükleri tablo

1. Tablo oluşturma *DiagnosticLogsRecords* içinde *AzureMonitoring* Tanılama Günlüğü alacak veritabanı kayıtlarını kullanarak `.create table` denetim komutu:

    ```kusto
    .create table DiagnosticLogsRecords (Timestamp:datetime, ResourceId:string, MetricName:string, Count:int, Total:double, Minimum:double, Maximum:double, Average:double, TimeGrain:string)
    ```

1. Seçin **çalıştırma** tablo oluşturun.

    ![Sorguyu Çalıştır](media/ingest-data-no-code/run-query.png)

#### <a name="activity-logs-tables"></a>Tabloları etkinlik günlükleri

Etkinlik günlükleri yapısı tablosal değil olduğundan, verileri işlemek ve her olay için bir veya daha fazla kayıt genişletin gerekir. Ham verileri Ara bir tabloya alınan *ActivityLogsRawRecords*. O anda veri yönetilebilir ve genişletilmiş. Genişletilmiş veriler daha sonra içine alınan *ActivityLogsRecords* bir güncelleştirme ilkesini kullanarak tablo. Bu nedenle, etkinlik günlükleri alımı için iki ayrı tablolar oluşturmak gerekir.

1. Tablo oluşturma *ActivityLogsRecords* içinde *AzureMonitoring* etkinlik günlüğü kayıtlarını alacak veritabanı. Tablo oluşturmak için aşağıdaki Azure Veri Gezgini sorguyu çalıştırın:

    ```kusto
    .create table ActivityLogsRecords (Timestamp:datetime, ResourceId:string, OperationName:string, Category:string, ResultType:string, ResultSignature:string, DurationMs:int, IdentityAuthorization:dynamic, IdentityClaims:dynamic, Location:string, Level:string)
    ```

1. Ara veri tablosu oluşturma *ActivityLogsRawRecords* içinde *AzureMonitoring* veritabanı veri işleme için:

    ```kusto
    .create table ActivityLogsRawRecords (Records:dynamic)
    ```

<!--
     ```kusto
     .alter-merge table ActivityLogsRawRecords policy retention softdelete = 0d
    <[Retention](/azure/kusto/management/retention-policy) for an intermediate data table is set at zero retention policy.
-->

### <a name="create-table-mappings"></a>Tablo eşlemeleri oluşturma

 Veri biçimi `json`, bu nedenle, veri eşlemesi gereklidir. `json` Eşleme her json yolu bir tablo sütunu adıyla eşleşir.

#### <a name="diagnostic-logs-table-mapping"></a>Tanılama günlükleri Tablo eşleme

Veri tablosuna eşlemek için aşağıdaki sorguyu kullanın:

```kusto
.create table DiagnosticLogsRecords ingestion json mapping 'DiagnosticLogsRecordsMapping' '[
{"column":"Timestamp","path":"$.time"},
{"column":"ResourceId","path":"$.resourceId"},
{"column":"MetricName","path":"$.metricName"},
{"column":"Count","path":"$.count"},
{"column":"Total","path":"$.total"},
{"column":"Minimum","path":"$.minimum"},
{"column":"Maximum","path":"$.maximum"},
{"column":"Average","path":"$.average"},
{"column":"TimeGrain","path":"$.timeGrain"}]'
```

#### <a name="activity-logs-table-mapping"></a>Etkinlik günlükleri Tablo eşleme

Veri tablosuna eşlemek için aşağıdaki sorguyu kullanın:

```kusto
.create table ActivityLogsRawRecords ingestion json mapping 'ActivityLogsRawRecordsMapping' '[
{"column":"Records","path":"$.records"}]'
```

### <a name="create-update-policy"></a>Güncelleştirme İlkesi Oluştur

1. Oluşturma bir [işlevi](/azure/kusto/management/functions) , genişleyen sahip kayıtların koleksiyonudur ve böylece koleksiyondaki her değer ayrı bir satır alır. Kullanım [ `mvexpand` ](/azure/kusto/query/mvexpandoperator) işleci:

    ```kusto
    .create function ActivityLogRecordsExpand() {
        ActivityLogsRawRecords
        | mvexpand events = Records
        | project
            Timestamp = todatetime(events["time"]),
            ResourceId = tostring(events["resourceId"]),
            OperationName = tostring(events["operationName"]),
            Category = tostring(events["category"]),
            ResultType = tostring(events["resultType"]),
            ResultSignature = tostring(events["resultSignature"]),
            DurationMs = toint(events["durationMs"]),
            IdentityAuthorization = events["identity.authorization"],
            IdentityClaims = events["identity.claims"],
            Location = tostring(events["location"]),
            Level = tostring(events["level"])
    }
    ```

2. Ekleme bir [güncelleştirme ilkesi](/azure/kusto/concepts/updatepolicy) hedef tablosu için. Yeni içe alınan tüm veriler üzerinde sorgu otomatik olarak çalıştırılacak *ActivityLogsRawRecords* ara veri tablosu ve sonuçları içine alma *ActivityLogsRecords* tablosu:

    ```kusto
    .alter table ActivityLogRecords policy update @'[{"Source": "ActivityLogsRawRecords", "Query": "ActivityLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

## <a name="create-an-event-hub-namespace"></a>Bir olay hub'ı Namespace oluşturma

Azure tanılama günlükleri dışarı aktarma ölçümler bir depolama hesabı veya bir olay hub'ı etkinleştirin. Bu öğreticide, biz bir olay hub'ı aracılığıyla ölçümleri yol. Aşağıdaki adımlarda, tanılama günlükleri için bir olay hub'ları Namespace ve olay Hub'ı oluşturacaksınız. Azure izleme, olay hub'ı oluşturacaktır *insights-operational-logs* etkinlik günlükleri için.

1. Azure portalında bir Azure Resource Manager şablonu kullanarak bir olay hub'ı oluşturun. Dağıtımı başlatmak için aşağıdaki düğmeyi kullanın. Sağ tıklayıp **yeni pencerede aç**, geri kalanını bu makaledeki adımları izleyebilirsiniz. **Azure'a Dağıt** düğmesi Azure portalına yönlendirilirsiniz.

    [![Azure’a dağıtma](media/ingest-data-no-code/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

1. Bir olay hub'ı Namespace ve tanılama günlükleri için bir olay Hub'ı oluşturun.

    ![Olay Hub'ı oluşturma](media/ingest-data-no-code/event-hub.png)

    Formu aşağıdaki bilgilerle doldurun. Aşağıdaki tabloda yer almayan ayarlar için varsayılan değerleri kullanın.

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Abonelik | Aboneliğiniz | Olay hub'ınız için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Kaynak grubu | *test-resource-group* | Yeni bir kaynak grubu oluşturun. |
    | Konum | İhtiyaçlarınıza en uygun bölgeyi seçin. | Olay hub'ı ad alanı, diğer kaynaklar aynı konumda oluşturun.
    | Ad alanı adı | *AzureMonitoringData* | Ad alanınızı tanımlayan benzersiz bir ad seçin.
    | Olay hub'ı adı | *DiagnosticLogsData* | Olay hub'ı benzersiz bir kapsayıcı kapsamı sunan ad alanında bulunur. |
    | Tüketici grubu adı | *adxpipeline* | Bir tüketici grubu adı oluşturun. Sağlayan birden çok tüketen uygulamayı her olay akışının ayrı bir görünümüne sahip. |
    | | |

## <a name="connect-azure-monitoring-logs-to-event-hub"></a>Azure izleme günlükleri Olay Hub'ına bağlama

### <a name="diagnostic-logs-connection-to-event-hub"></a>Olay Hub'ına bağlantı tanılama günlükleri

Ölçümleri dışarı aktarılacağı bir kaynak seçin. Olay hub'ı Namespace, anahtar kasası, IOT Hub ve Azure Veri Gezgini küme dahil olmak üzere dışarı aktarılan tanılama günlüklerini etkinleştirme çeşitli kaynak türleri vardır. Bu öğreticide, bizim kaynağı olarak Azure Veri Gezgini küme kullanırız.

1. Kusto kümeniz, Azure portalında seçin.

    ![Tanılama ayarları](media/ingest-data-no-code/diagnostic-settings.png)

1. Seçin **tanılama ayarları** sol menüsünde.
1. Tıklayın **tanılamayı Aç** bağlantı. **Tanılama ayarları** penceresi açılır.

    ![Tanılama Ayarları penceresi](media/ingest-data-no-code/diagnostic-settings-window.png)

1. İçinde **tanılama ayarları** bölmesi:
    1. Tanılama günlük verilerini adlandırın: *ADXExportedData*
    1. Seçin **AllMetrics** onay kutusunu (isteğe bağlı).
    1. Seçin **Stream olay hub'ına** onay kutusu.
    1. Tıklayın **yapılandırın**

1. İçinde **Select olay hub'ı** bölmesinde yapılandırmak, oluşturduğunuz olay hub'ına dışarı aktarma:
    1. **Olay hub'ı ad alanını seçin** *AzureMonitoringData* açılır listesinden.
    1. **Select olay hub'ı adı** *diagnosticlogsdata* açılır listesinden.
    1. **Select olay hub'ı ilke adı** açılır listesinden.
    1. **Tamam** düğmesine tıklayın.

1. **Kaydet**’e tıklayın. Olay hub'ı ad alanı adı ve ilke adı penceresinde görünür.

    ![Tanılama ayarları kaydedin](media/ingest-data-no-code/save-diagnostic-settings.png)

### <a name="activity-logs-connection-to-event-hub"></a>Olay Hub'ına bağlantı etkinlik günlükleri

1. Azure portalında sol taraftaki menüde seçin **etkinlik günlüğü**
1. **Etkinlik günlüğü** penceresi açılır. **Olay hub'ına dışarı Aktar'a tıklayın**

    ![Etkinlik günlüğü](media/ingest-data-no-code/activity-log.png)

1. İçinde **Etkinlik günlüğünü dışarı aktar** penceresi:
 
    ![Etkinlik günlüğünü dışarı aktarın](media/ingest-data-no-code/export-activity-log.png)

    1. Aboneliğinizi seçin.
    1. İçinde **bölgeleri** açılır listesinde, seçin **Tümünü Seç**
    1. Seçin **dışarı aktarma, olay hub'ına** onay kutusu.
    1. Tıklayarak **bir service bus ad alanı seçin** açmak için **Select olay hub'ı** bölmesi.
    1. İçinde **Select olay hub'ı** bölmesinde seçin'açılır menülerden: aboneliğinizi, olay ad alanınızı *AzureMonitoringData*ve varsayılan olay hub'ı ilke adı.
    1. **Tamam** düğmesine tıklayın.
    1. Tıklayın **Kaydet** penceresinin sağ üst tarafındaki. Bir olay hub'ı adı ile *insights-operational-logs* oluşturulur.

### <a name="see-data-flowing-to-your-event-hubs"></a>Verilerin, Event Hubs'a aktığını

1. Bağlantı tanımlanır ve Etkinlik günlüğünü dışarı aktarma olay hub'ına tamamlanana kadar birkaç dakika bekleyin. Oluşturduğunuz olay hub'ları görmek için olay hub'ı ad alanınıza gidin.

    ![Oluşturulan olay hub'ları](media/ingest-data-no-code/event-hubs-created.png)

1. Olay Hub'ınıza akan verileri görürsünüz:

    ![Event Hubs verilerini](media/ingest-data-no-code/event-hubs-data.png)

## <a name="connect-event-hub-to-azure-data-explorer"></a>Olay hub'ı Azure veri Gezgini'ne bağlanma

### <a name="diagnostic-logs-data-connection"></a>Tanılama günlükleri veri bağlantısı

1. Azure Veri Gezgini kümenizdeki *kustodocs*seçin **veritabanları** soldaki menüde.
1. İçinde **veritabanları** penceresinde, veritabanı adını seçin *AzureMonitoring*
1. Sol menüde **veri alımı**
1. İçinde **veri alımı** penceresinde tıklayın **+ veri bağlantısı ekleme**
1. İçinde **veri bağlantısı** penceresinde aşağıdaki bilgileri girin:

    ![Olay Hub'ı bağlantısı](media/ingest-data-no-code/event-hub-data-connection.png)

    Veri kaynağı:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Veri bağlantısı adı | *DiagnosticsLogsConnection* | Azure Veri Gezgini'nde oluşturmak istediğiniz bağlantının adı.|
    | Olay hub’ı ad alanı | *AzureMonitoringData* | Önceden seçtiğiniz ve ad alanınızı tanımlayan ad. |
    | Olay hub'ı | *diagnosticlogsdata* | Oluşturduğunuz olay hub'ı. |
    | Tüketici grubu | *adxpipeline* | Oluşturduğunuz olay hub'ında tanımlanan tüketici grubu. |
    | | |

    Hedef Tablo:

    İki yönlendirme seçeneği vardır: *statik* ve *dinamik*. Bu öğreticide tablo adı, dosya biçimi ve eşleme belirttiğiniz (varsayılan), yönlendirme statik kullanın. Bu nedenle, bırakın **verilerimi yönlendirme bilgilerini içeren** seçilmemiş.

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Tablo | *DiagnosticLogsRecords* | Oluşturduğunuz tabloyu *AzureMonitoring* veritabanı. |
    | Veri biçimi | *JSON* | Tablo biçimlendirin. |
    | Sütun eşleme | *DiagnosticLogsRecordsMapping* | Oluşturduğunuz eşleme *AzureMonitoring* sütun adları ve veri türleri için gelen JSON verilerini eşleştiren veritabanı *DiagnosticLogsRecords*.|
    | | |

1. **Oluştur**'a tıklayın  

### <a name="activity-logs-data-connection"></a>Etkinlik günlükleri veri bağlantısı

Adımları yineleyin [tanılama günlüklerine yönelik veri bağlantısı](#diagnostic-logs-data-connection) etkinlik oluşturmak için bölüm veri bağlantısı günlüğe kaydeder.

1. Aşağıdaki ayarları Ekle **veri bağlantısı** penceresi:

    Veri kaynağı:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Veri bağlantısı adı | *ActivityLogsConnection* | Azure Veri Gezgini'nde oluşturmak istediğiniz bağlantının adı.|
    | Olay hub’ı ad alanı | *AzureMonitoringData* | Önceden seçtiğiniz ve ad alanınızı tanımlayan ad. |
    | Olay hub'ı | *insights-operational-logs* | Oluşturduğunuz olay hub'ı. |
    | Tüketici grubu | *$Default* | Varsayılan bir tüketici grubu. Gerekirse, farklı bir tüketici grubu oluşturabilirsiniz. |
    | | |

    Hedef Tablo:

    İki yönlendirme seçeneği vardır: *statik* ve *dinamik*. Bu öğreticide tablo adı, dosya biçimi ve eşleme belirttiğiniz (varsayılan), yönlendirme statik kullanın. Bu nedenle, bırakın **verilerimi yönlendirme bilgilerini içeren** seçilmemiş.

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Tablo | *ActivityLogsRawRecords* | Oluşturduğunuz tabloyu *AzureMonitoring* veritabanı. |
    | Veri biçimi | *JSON* | Tablo biçimlendirin. |
    | Sütun eşleme | *ActivityLogsRawRecordsMapping* | Oluşturduğunuz eşleme *AzureMonitoring* sütun adları ve veri türleri için gelen JSON verilerini eşleştiren veritabanı *ActivityLogsRawRecords*.|
    | | |

1. **Oluştur**'a tıklayın  

## <a name="query-the-new-tables"></a>Yeni Tablo sorgulama

Bir işlem hattı ile veri akışının sizde. Küme aracılığıyla alımı 5 dakika, varsayılan olarak alır, böylece sorgu başına önce birkaç dakika veri akışı izin.

### <a name="diagnostic-logs-table-query-example"></a>Tanılama günlükleri Tablo sorgusu örneği

Aşağıdaki sorgu, Azure Veri Gezgini tanılama günlük kayıtlarından sorgu süresi verileri analiz eder:

```kusto
DiagnosticLogsRecords
| where Timestamp > ago(15m) and MetricName == 'QueryDuration'
| summarize avg(Average)
```

Sorgu sonuçları:

|   |   |
| --- | --- |
|   |  avg_Average |
|   | 00:06.156 |
| | |

### <a name="activity-logs-table-query-example"></a>Etkinlik günlükleri Tablo sorgusu örneği

Aşağıdaki sorguyu Azure Veri Gezgini etkinlik günlüğü kaynaklarından gelen verileri analiz eder:

```kusto
ActivityLogsRecords
| where OperationName == 'MICROSOFT.EVENTHUB/NAMESPACES/AUTHORIZATIONRULES/LISTKEYS/ACTION'
| where ResultType == 'Success'
| summarize avg(DurationMs)
```

Sorgu sonuçları:
|   |   |
| --- | --- |
|   |  AVG(DurationMs) |
|   | 768.333 |
| | |

## <a name="next-steps"></a>Sonraki adımlar

Azure veri şu makaleden yararlanarak Gezgini'nden ayıklanan verilerin çok daha fazla sorguları yazma öğrenin:

> [!div class="nextstepaction"]
> [Azure Veri Gezgini için sorguları yazma](write-queries.md)
