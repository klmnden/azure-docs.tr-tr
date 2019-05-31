---
title: SAP Business Warehouse açık bir Azure Data Factory kullanarak hub'ı aracılığıyla veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına açık hub'ı aracılığıyla SAP Business Warehouse (BW) gelen bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: jingwang
ms.openlocfilehash: 6fb989632d3165ac5e54e540aae4385fc2258c85
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66256903"
---
# <a name="copy-data-from-sap-business-warehouse-via-open-hub-using-azure-data-factory"></a>SAP Business Warehouse açık bir Azure Data Factory kullanarak hub'ı aracılığıyla veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir SAP Business Warehouse (BW) öğesinden açık hub'ı aracılığıyla veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna SAP Business Warehouse açık hub'ı üzerinden gelen verileri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SAP Business Warehouse açık Hub bağlayıcı'yı destekler:

- SAP Business Warehouse **sürüm 7.01 veya üzeri (yığındaki son SAP destek paketi 2015 yıl sonra yayımlanan)** .
- Olabilen altında DSO, Infocube, MultiProvider, veri kaynağı, vb. açık Hub hedef yerel tablo aracılığıyla veri kopyalama.
- Temel kimlik doğrulaması kullanarak veri kopyalama.
- Uygulama sunucusuna bağlanılıyor.

## <a name="sap-bw-open-hub-integration"></a>SAP BW Open Hub tümleştirmesi 

[SAP BW Open Hub Service](https://wiki.scn.sap.com/wiki/display/BI/Overview+of+Open+Hub+Service) SAP BW verileri ayıklamak için etkili bir yoludur. Aşağıdaki diyagramda imajlarını kendi SAP sistemde tipik akışlar birini gösterir, SAP ECC hangi büyük veri akışları PSA -> DSO -> Küp ->.

SAP BW Open Hub hedef (OHD) için SAP veri geçirilen hedef tanımlar. SAP veri aktarım işlemi (DTP) tarafından desteklenen herhangi bir nesne açık hub'ı veri kaynakları, örneğin DSO, Infocube, veri kaynağı, vb. olarak kullanılabilir. Veritabanı tabloları (yerel veya uzak) - geçirilen verilerin depolandığı - açık Hub hedef türü olabilir ve düz dosyaları. BW OHD yerel tabloda veri kopyalama bu SAP BW Open Hub Bağlayıcısı desteği. Durumunda diğer türleri kullanıyorsanız, veritabanı veya dosya sistemine diğer bağlayıcıları kullanarak doğrudan bağlantı kurabilir.

![SAP BW Open Hub](./media/connector-sap-business-warehouse-open-hub/sap-bw-open-hub.png)

## <a name="delta-extraction-flow"></a>Delta ayıklama akışı

ADF SAP BW Open Hub Bağlayıcısı iki isteğe bağlı özellikleri sunar: `excludeLastRequest` ve `baseRequestId` açık Hub'ından delta yükü işlemek için kullanılabilir. 

- **excludeLastRequestId**: Son istek kayıtlarını hariç verilmeyeceğini belirtir. Varsayılan değer True'dur. 
- **baseRequestId**: Delta yükleme isteği kimliği. Ayarlandıktan sonra yalnızca veri RequestId bu özelliğin değerinden daha büyük olan alınır. 

Genel olarak, Azure Data Factory (ADF) için ayıklama SAP InfoProviders gelen 2 adımlardan oluşur: 

1. **SAP BW veri aktarım işlemi (DTP)** Bu adım bir SAP BW InfoProvider veriler, bir SAP BW Open Hub tabloya kopyalar. 

1. **ADF veri kopyalama** Bu adımda, ADF bağlayıcı tarafından okunan açık Hub tablosu 

![Delta ayıklama akışı](media/connector-sap-business-warehouse-open-hub/delta-extraction-flow.png)

İlk adımda bir DTP yürütülür. Her yürütme yeni bir SAP talep kimliği oluşturur. İstek Kimliği açık Hub tablosunda depolanır ve sonra belirlemek delta için ADF bağlayıcı tarafından kullanılır. İki adımı zaman uyumsuz olarak çalışır: DTP SAP tarafından tetiklenir ve ADF veri kopyalama ADF tetiklenir. 

Varsayılan olarak, ADF son delta açık Hub tablosundan okuyor değil ("hariç tutma son isteği" seçeneğini true). İşbu sözleşme ile ADF verileri % 100 (son delta eksik) açık Hub tablodaki verileri güncel değil. Hiçbir satır zaman uyumsuz ayıklama nedeni kayıp, buna karşılık, bu yordamı sağlar. DTP hala aynı tabloya yazma olsa bile ADF açık Hub tablosu okunurken düzgün çalışır. 

Genellikle son çalıştırılmasındaki hazırlama veri deposu (örneğin, yukarıdaki diyagramda Azure Blob üzerinde), ADF tarafından en fazla kopyalanan istek kimliği saklayın. Bu nedenle, aynı istekte ikinci kez ADF tarafından sonraki çalıştırmada okunmadı. Bu arada, verileri otomatik olarak açık Hub tablosundan silinmez unutmayın.

İçin uygun delta, işleme istek farklı DTPs kimlikleri aynı açık Hub tabloya sahip izin verilmiyor. Bu nedenle, her açık Hub hedef (OHD) için birden fazla DTP oluşturmamalıdır. Aynı InfoProvider tam ve değişim ayıklama ihtiyaç duyulduğunda için aynı InfoProvider iki OHDs oluşturmanız gerekir. 

## <a name="prerequisites"></a>Önkoşullar

SAP Business Warehouse açık Hub bu bağlayıcıyı kullanmak için gerekir:

- Bir şirket içinde barındırılan tümleştirme çalışma zamanı sürümü 3.13 veya üzeri olarak ayarlayın. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.

- İndirme **64-bit [SAP .NET bağlayıcı 3.0](https://support.sap.com/en/product/connectors/msnet.html)**  SAP'nin sitesinden ve şirket içinde barındırılan IR makineye yükleyin. Ne zaman yükleme, isteğe bağlı kurulum adımlarını penceresinde seçtiğinizden emin olun **yükleme derlemeleri GAC'ye** seçenek aşağıdaki görüntüde gösterildiği gibi. 

    ![SAP .NET bağlayıcısını yükleme](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- Data Factory BW Bağlayıcısı kullanılan SAP kullanıcının aşağıdaki izinleri olmalıdır: 

    - RFC ve SAP BW için yetkilendirme. 
    - "Yürütme" etkinlik "S_SDSAUTH" yetkilendirme nesnesinin izinleri.

- SAP açık Hub hedef türünde oluşturma **veritabanı tablosu** "Teknik anahtarı" seçeneği işaretli.  Ayrıca, gerekli olmamasına rağmen silme veri tablosundan olarak işaretlemeden bırakın için önerilir. DTP yararlanın (doğrudan yürütün veya var olan bir işlem zincirine tümleştirme) verileri (örneğin, küp) kaynak nesneden yerleşmesi açık hub hedef tabloya seçtiniz.

## <a name="getting-started"></a>Başlarken

> [!TIP]
>
> SAP BW Open Hub Bağlayıcısı'nı kullanarak bir kılavuz için bkz. [veri yükleme SAP Business Warehouse (BW) Azure Data Factory kullanarak](load-sap-bw-data.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, SAP Business Warehouse açık Hub bağlayıcıya belirli Data Factory varlıkları tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

SAP Business Warehouse açık bağlı Hub hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapOpenHub** | Evet |
| server | SAP BW örneği yer aldığı sunucunun adı. | Evet |
| systemNumber | SAP BW sisteminin sistem numarası.<br/>İzin verilen değer: bir dize olarak temsil edilen iki basamaklı ondalık sayı. | Evet |
| clientId | SAP W sisteminde istemcinin istemci kimliği.<br/>İzin verilen değer: bir dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet |
| language | SAP sisteminin kullandığı dil. | Hayır (varsayılan değer **tr**)|
| userName | SAP sunucusuna erişimi olan kullanıcı adı. | Evet |
| password | Kullanıcının parolası. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtildiği gibi bir şirket içinde barındırılan tümleştirme çalışma zamanı gereklidir [önkoşulları](#prerequisites). |Evet |

**Örnek:**

```json
{
    "name": "SapBwOpenHubLinkedService",
    "properties": {
        "type": "SapOpenHub",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, SAP BW Open Hub veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Gelen ve SAP BW Open Hub'ına veri kopyalamak için dataset öğesinin type özelliği ayarlamak **SapOpenHubTable**. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **SapOpenHubTable**.  | Evet |
| openHubDestinationName | Verileri kopyalamak için açık Hub hedef adı. | Evet |
| excludeLastRequest | Son istek kayıtlarını hariç verilmeyeceğini belirtir. | Hayır (varsayılan değer **true**) |
| baseRequestId | Delta yükleme isteği kimliği. Ayarladıktan sonra yalnızca RequestId verilerle **büyük** bu özelliğin değeri alınır.  | Hayır |

>[!TIP]
>Açık Hub tablonuzda yalnızca tek bir istek kimliği tarafından oluşturulan veriler içerir, örneğin, her zaman yük tam ve tablodaki mevcut verilerin üzerine veya yalnızca DTP testi için bir kez çalıştırdığınız unutmayın d kopyalamak için "excludeLastRequest" seçeneğinin işaretini kaldırın Ata'yı uğradı.

**Örnek:**

```json
{
    "name": "SAPBWOpenHubDataset",
    "properties": {
        "type": "SapOpenHubTable",
        "linkedServiceName": {
            "referenceName": "<SAP BW Open Hub linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "openHubDestinationName": "<open hub destination name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, SAP BW Open Hub kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-bw-open-hub-as-source"></a>SAP BW Open Hub kaynağı olarak

SAP BW Open Hub'ından veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SapOpenHubSource**. Kopyalama etkinliği gereken ek türe özgü özellikler yoktur **kaynak** bölümü.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPBWOpenHub",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP BW Open Hub input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SapOpenHubSource"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-bw-open-hub"></a>SAP BW Open merkezi için veri türü eşlemesi

SAP BW Open Hub'ından veri kopyalama işlemi sırasında aşağıdaki eşlemeler SAP BW veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| SAP ABAP türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| C (dize) | String |
| Ben (tamsayı) | Int32 |
| F (kayan nokta) | Double |
| D (tarih) | String |
| T (saat) | String |
| P (BCD paketlenmiş, para birimi, Decimal, miktar) | Decimal |
| N (Numc) | String |
| (İkili ve ham) X | String |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
