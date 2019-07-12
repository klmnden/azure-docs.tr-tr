---
title: Azure Data Lake depolama Gen1 gelen ve giden veri kopyalama | Microsoft Docs
description: Azure Data factory'yi kullanarak Data Lake Store gelen ve giden veri kopyalama hakkında bilgi edinin
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d8637a2711c0301d9e9f409e169ed04fb3d65783
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839539"
---
# <a name="copy-data-to-and-from-data-lake-storage-gen1-by-using-data-factory"></a>Data factory'yi kullanarak Data Lake depolama Gen1 gelen ve giden veri kopyalama
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-azure-datalake-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-data-lake-store.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de Azure Data Lake depolama Gen1 bağlayıcı](../connector-azure-data-lake-store.md).

Bu makalede, Azure Data Lake depolama Gen1 (daha önce Azure Data Lake Store da bilinir) gelen ve giden veri taşımak için Azure Data Factory'de kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, veri taşıma, kopyalama etkinliği ile bir genel bakış.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Veri kopyalayabilirsiniz **Azure Data Lake Store'dan** aşağıdaki verilere depolar:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından veri kopyalayabilirsiniz **Azure Data Lake Store için**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Kopyalama etkinliği ile işlem hattı oluşturmadan önce bir Data Lake Store hesabı oluşturun. Daha fazla bilgi için [Azure Data Lake Store ile çalışmaya başlama](../../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="supported-authentication-types"></a>Kimlik doğrulaması türleri desteklenir
Data Lake Store bağlayıcı, bu kimlik doğrulama türlerini destekler:
* Hizmet sorumlusu kimlik doğrulaması
* Kullanıcı kimlik bilgisi (OAuth) kimlik doğrulaması

Özellikle bir zamanlanmış veri kopyalama için hizmet sorumlusu kimlik doğrulaması kullanmanızı öneririz. Belirteç sona erme davranış, kullanıcı kimlik bilgilerinin ile ortaya çıkabilir. Yapılandırma ayrıntıları için bkz. [bağlı hizmeti özellikleri](#linked-service-properties) bölümü.

## <a name="get-started"></a>başlarken
Farklı araçlar/API'lerini kullanarak bir Azure Data Lake Store gönderip buralardan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Verileri kopyalamak için bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturmaya ilişkin bir öğretici için bkz. [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).

Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma bir **veri fabrikası**. Veri fabrikası, bir veya daha fazla işlem hattı içerebilir.
2. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, bir Azure Data Lake Store için bir Azure blob depolamadan veri kopyalıyorsanız, Azure depolama hesabınızı ve Azure Data Lake store, veri fabrikanıza bağlamak için iki bağlı hizmet oluşturursunuz. Azure Data Lake Store için özel bağlı hizmeti özellikleri için bkz: [bağlı hizmeti özellikleri](#linked-service-properties) bölümü.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için. Son adımda bahsedilen örnekte, bir veri kümesi blob kapsayıcıyı ve girdi verilerini içeren klasörü belirtin oluşturun. Ayrıca, blob depolama alanından kopyalanan verileri tutan bir veri Gölü deposu klasör ve dosya yolu belirtmek için başka bir veri kümesi oluşturursunuz. Azure Data Lake Store için özel veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile. Daha önce bahsedilen örnekte BlobSource bir kaynak ve AzureDataLakeStoreSink havuz olarak kopyalama etkinliği için kullanırsınız. Azure Data Lake Store ' Azure Blob depolama alanına kopyalanıyorsa, benzer şekilde, kümesinin kullanılması gerekir ve BlobSink kopyalama etkinliği kullanırsınız. Azure Data Lake Store için özel kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir kaynak veya havuz bir veri deposunu kullanma hakkında daha fazla ayrıntı için önceki bölümde veri deponuz için bağlantıya tıklayın.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın. Veri gönderip buralardan bir Azure Data Lake Store kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile örnekleri için bkz [JSON örnekler](#json-examples-for-copying-data-to-and-from-data-lake-store) bu makalenin.

Aşağıdaki bölümler, Data Lake Store için belirli Data Factory varlıkları tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Bağlı hizmet, bir veri deposuna bir veri fabrikasına bağlar. Bağlı hizmet türü oluşturma **birlikte AzureDataLakeStore** Data Lake Store verilerinizi veri fabrikanıza bağlamak için. Aşağıdaki tabloda Data Lake Store bağlı hizmetler için özel JSON öğeleri açıklar. Hizmet sorumlusu ve kullanıcı kimlik bilgileri doğrulaması arasında seçim yapabilirsiniz.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **type** | Type özelliği ayarlanmalıdır **birlikte AzureDataLakeStore**. | Evet |
| **dataLakeStoreUri** | Azure Data Lake Store hesabı hakkında bilgi. Bu bilgiler aşağıdaki biçimlerden birini alır: `https://[accountname].azuredatalakestore.net/webhdfs/v1` veya `adl://[accountname].azuredatalakestore.net/`. | Evet |
| **Subscriptionıd** | Data Lake Store hesabına ait olduğu azure abonelik kimliği. | Havuz için gerekli |
| **resourceGroupName** | Data Lake Store hesabına ait olduğu azure kaynak grubu adı. | Havuz için gerekli |

### <a name="service-principal-authentication-recommended"></a>(Önerilen) hizmet sorumlusu kimlik doğrulaması
Hizmet sorumlusu kimlik doğrulaması kullanmak için Azure Active Directory (Azure AD) uygulama varlığı Kaydet ve Data Lake Store erişimi verin. Ayrıntılı adımlar için bkz. [hizmetten hizmete kimlik doğrulaması](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:
* Uygulama Kimliği
* Uygulama anahtarı
* Kiracı Kimliği

> [!IMPORTANT]
> Hizmet sorumlusu uygun Azure Data Lake Store içinde izni olduğundan emin olun:
>- **Data Lake Store kaynağı olarak kullanmak için**, en az izni **okuma + yürütme** veri erişim izni listeler ve bir klasörün içeriğini kopyalayın veya **okuma** tek bir dosyayı kopyalama izni. Hesap düzeyinde erişim denetimi gereksinimi yoktur.
>- **Havuz olarak Data Lake Store kullanmayı**, en az izni **yazma + yürütme** veri erişim izni klasörde alt öğeler oluşturmak için. Ve kopyalama olanağı Azure IR kullanıyorsanız (hem kaynak hem de bulutta) Data Factory'ye Data Lake Store'nın bölgesi algılamak izin vermek için en az izni **okuyucu** hesap erişim denetimi (IAM) rolü. Bu IAM rol kaçınmak istiyorsanız [executionLocation belirtin](data-factory-data-movement-activities.md#global) kopyalama etkinliğinde, Data Lake Store konumu ile.
>- Varsa, **işlem hatlarını yazmak için kopyalama Sihirbazı'nı kullanmak**, en az izni **okuyucu** hesap erişim denetimi (IAM) rolü. Ayrıca, en az izni **okuma + yürütme** ("/"), Data Lake Store kök ve alt izni. Aksi takdirde, "sağlanan kimlik bilgileri geçersiz." iletisini görebilirsiniz

Hizmet sorumlusu kimlik doğrulaması, aşağıdaki özellikleri belirterek kullanın:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **servicePrincipalId** | Uygulamanın istemci kimliği belirtin. | Evet |
| **serviceprincipalkey değerleri** | Uygulama anahtarını belirtin. | Evet |
| **Kiracı** | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet |

**Örnek: Hizmet sorumlusu kimlik doğrulaması**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Kullanıcı kimlik bilgileri doğrulaması
Alternatif olarak, aşağıdaki özellikleri belirterek ya da Data Lake Store için kopyalamak için kullanıcı kimlik bilgilerinin kullanabilirsiniz:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **Yetkilendirme** | Tıklayın **Authorize** düğmesini Data Factory Düzenleyicisi'nde ve bu özelliği otomatik olarak oluşturulan yetkilendirme URL'si atar kimlik bilgilerinizi girin. | Evet |
| **sessionId** | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersiz olup yalnızca bir kez kullanılabilir. Bu ayar, Data Factory Düzenleyicisi'ni kullandığınızda otomatik olarak oluşturulur. | Evet |

> [!IMPORTANT]
> Azure Data Lake Store kullanıcı uygun izin verme emin olun:
>- **Data Lake Store kaynağı olarak kullanmak için**, en az izni **okuma + yürütme** veri erişim izni listeler ve bir klasörün içeriğini kopyalayın veya **okuma** tek bir dosyayı kopyalama izni. Hesap düzeyinde erişim denetimi gereksinimi yoktur.
>- **Havuz olarak Data Lake Store kullanmayı**, en az izni **yazma + yürütme** veri erişim izni klasörde alt öğeler oluşturmak için. Ve kopyalama olanağı Azure IR kullanıyorsanız (hem kaynak hem de bulutta) Data Factory'ye Data Lake Store'nın bölgesi algılamak izin vermek için en az izni **okuyucu** hesap erişim denetimi (IAM) rolü. Bu IAM rol kaçınmak istiyorsanız [executionLocation belirtin](data-factory-data-movement-activities.md#global) kopyalama etkinliğinde, Data Lake Store konumu ile.
>- Varsa, **işlem hatlarını yazmak için kopyalama Sihirbazı'nı kullanmak**, en az izni **okuyucu** hesap erişim denetimi (IAM) rolü. Ayrıca, en az izni **okuma + yürütme** ("/"), Data Lake Store kök ve alt izni. Aksi takdirde, "sağlanan kimlik bilgileri geçersiz." iletisini görebilirsiniz

**Örnek: Kullanıcı kimlik bilgileri doğrulaması**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

#### <a name="token-expiration"></a>Belirteç süre sonu
Yetkilendirme kodunu kullanarak oluşturduğunuz **Authorize** düğmesine bir belirli bir süre sonra süresi dolar. Aşağıdaki ileti kimlik doğrulama belirtecinin süresi doldu anlamına gelir:

Kimlik bilgileri işlemi hatası: invalid_grant - AADSTS70002: Kimlik bilgileri doğrulanırken hata. AADSTS70008: Sağlanan erişim izni süresi doldu veya iptal edildi. İzleme kimliği: d18629e8-af88-43c5-88e3-d8419eb1fca1 bağıntı kimliği: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 zaman damgası: 2015-12-15 21-09-31Z.

Aşağıdaki tabloda, farklı türlerdeki kullanıcı hesapları, geçerlilik sonu süreleri gösterilmektedir:

| Kullanıcı türü | Sürenin dolacağı tarih |
|:--- |:--- |
| Kullanıcı hesaplarını *değil* Azure Active Directory tarafından yönetilen (örneğin, @hotmail.com veya @live.com) |12 saat |
| Kullanıcı hesaplarını Azure Active Directory tarafından yönetilen |14 gün sonra en son dilim çalıştırın <br/><br/>90 bir OAuth tabanlı bağlı hizmetini temel alan bir dilimi 14 günde en az bir kez çalıştırılıyorsa, gün |

Belirteç süre önce parolanızı değiştirirseniz, hemen belirtecin süresi dolar. Bu bölümde daha önce bahsedilen iletisini görürsünüz.

Kullanarak hesabı yeniden yetkilendirin **Authorize** bağlı hizmeti yeniden dağıtmak için belirtecinin süresi olduğunda düğme. Değerleri için de oluşturabilirsiniz **SessionID** ve **yetkilendirme** aşağıdaki kodu kullanarak program aracılığıyla özellikleri:


```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```
Kod içinde kullanılan Data Factory sınıfları hakkında daha fazla bilgi için bkz. [AzureDataLakeStoreLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), ve [ AuthorizationSessionGetResponse sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) konuları. Sürüm bir başvuru ekleyin `2.9.10826.1824` , `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` için `WindowsFormsWebAuthenticationDialog` kod içinde kullanılan sınıf.

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

**Belirti:** Veri kopyalama işlemi sırasında **içine** , kopyalama etkinliği şu hatayla başarısız olursa Azure Data Lake Store,:

  ```
  Failed to detect the region for Azure Data Lake account {your account name}. Please make sure that the Resource Group name: {resource group name} and subscription ID: {subscription ID} of this Azure Data Lake Store resource are correct.
  ```

**Kök neden:** 2 olası nedeni vardır:

1. `resourceGroupName` Ve/veya `subscriptionId` hizmetidir yanlış; Azure Data Lake bağlı Store içinde belirtilen
2. Kullanıcı veya hizmet sorumlusu gerekli izne sahip değil.

**Çözüm:**

1. Emin `subscriptionId` ve `resourceGroupName` bağlı hizmeti belirtin `typeProperties` gerçekten data lake hesabınıza ait olanlardır.

2. En az izni olduğundan emin olun **okuyucu** rolüne kullanıcı veya hizmet sorumlusu data lake hesabı. Bunu yapmak nasıl aşağıda verilmiştir:

    1. Azure portalına gidin -> Data Lake Store hesabınız
    2. Tıklayın **erişim denetimi (IAM)** Data Lake Store dikey
    3. Tıklayın **rol ataması Ekle**
    4. Ayarlama **rol** olarak **okuyucu**, kullanıcı veya erişim vermek için kopya için kullandığınız hizmet sorumlusu seçin

3. Vermek istemiyorsanız **okuyucu** kullanıcı veya hizmet sorumlusu, alternatif rolüdür için [yürütme konumu açıkça belirtmeniz](data-factory-data-movement-activities.md#global) kopyalama etkinliği, Data Lake Store konumu ile içinde. Örnek:

    ```json
    {
      "name": "CopyToADLS",
      "type": "Copy",
      ......
      "typeProperties": {
        "source": {
          "type": "<source type>"
        },
        "sink": {
          "type": "AzureDataLakeStoreSink"
        },
        "exeuctionLocation": "West US"
      }
    }
    ```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bir Data Lake Store, girdi verilerini temsil eden bir veri kümesi belirtmek için ayarladığınız **türü** veri kümesine özelliği **birlikte AzureDataLakeStore**. Ayarlama **linkedServiceName** özellik adı olarak Data Lake Store veri kümesinin bağlı hizmeti. JSON bölümler ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bir veri kümesi, json'da bölümlerini gibi **yapısı**, **kullanılabilirlik**, ve **ilke**, tüm veri kümesi türleri için benzer (Azure SQL veritabanı, Azure blob ve Azure tablo için Örnek). **TypeProperties** bölümünde her veri kümesi türü için farklıdır ve konum ve verilerin veri deposundaki biçimi gibi bilgiler sağlar.

**TypeProperties** türü için bir veri kümesi bölümünü **birlikte AzureDataLakeStore** aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **folderPath** |Kapsayıcı ve Data Lake Store klasörü yolu. |Evet |
| **Dosya adı** |Azure Data Lake Store dosya adı. **FileName** özelliği isteğe bağlıdır ve büyük küçük harfe duyarlı. <br/><br/>Belirtirseniz **fileName**, etkinlik (kopyalama dahil) belirli bir dosya üzerinde çalışır.<br/><br/>Zaman **fileName** belirtilmezse, tüm dosyalarda kopyalama içerir **folderPath** giriş veri kümesinde.<br/><br/>Zaman **fileName** bir çıktı veri kümesi için belirtilmemiş ve **preserveHierarchy** belirtilmezse etkinlik havuzunda oluşturulan dosya adıdır biçiminde `Data._Guid_.txt`. Örneğin: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt. |Hayır |
| **partitionedBy** |**PartitionedBy** özelliği, isteğe bağlıdır. Bir dinamik yol ve dosya adı için zaman serisi verilerini belirtmek için kullanabilirsiniz. Örneğin, **folderPath** veri her saat için parametreli olabilir. Ayrıntılar ve örnekler için partitionedBy özelliğine bakın. |Hayır |
| **Biçim** | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParquetFormat**. Ayarlama **türü** özelliği altında **biçimi** şu değerlerden biri olarak. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [JSON biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi ](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine [Azure Data Factory tarafından desteklenen dosya ve sıkıştırma biçimleri](data-factory-supported-file-and-compression-formats.md) makalesi. <br><br> Dosyaları kopyalamak istiyorsanız "olarak-olan" dosya tabanlı depoları arasında (ikili kopya) atlamak `format` hem girdi ve çıktı veri kümesi tanımları bölümünde. |Hayır |
| **Sıkıştırma** | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyleri **Optimal** ve **en hızlı**. Daha fazla bilgi için [Azure Data Factory tarafından desteklenen dosya ve sıkıştırma biçimleri](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

### <a name="the-partitionedby-property"></a>PartitionedBy özelliği
Dinamik belirtebilirsiniz **folderPath** ve **fileName** ile zaman serisi verilerinin özelliklerini **partitionedBy** özelliği, Data Factory işlevleri ve sistem değişkenleri. Ayrıntılar için bkz [Azure Data Factory - işlevler ve sistem değişkenleri](data-factory-functions-variables.md) makalesi.


Aşağıdaki örnekte, `{Slice}` Data Factory sistem değişkenin değeriyle değiştirilir `SliceStart` belirtilen biçimde (`yyyyMMddHH`). Adı `SliceStart` dilim başlangıç saati gösterir. `folderPath` Özelliktir görüldüğü her dilim için farklı `wikidatagateway/wikisampledataout/2014100103` veya `wikidatagateway/wikisampledataout/2014100104`.

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Aşağıdaki örnekte, yıl, ay, gün ve saati de `SliceStart` tarafından kullanılan ayrı değişkenleri ayıklanan `folderPath` ve `fileName` özellikleri:
```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
[
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında daha fazla bilgi için bkz. [Azure Data factory'deki veri kümelerini](data-factory-create-datasets.md) ve [Data Factory zamanlama ve yürütme](data-factory-scheduling-and-execution.md) makaleler.


## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [komut zincirleri oluşturma](data-factory-create-pipelines.md) makalesi. İlke adı ve açıklaması, girdi ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için kullanılabilir.

Bulunan özelliklerin **typeProperties** etkinlik bölümünü her etkinlik türü ile farklı. Bir kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

**Kümesinin kullanılması gerekir** aşağıdaki özellik destekler **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| **özyinelemeli** |Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. |(Varsayılan değer) true, False |Hayır |

**AzureDataLakeStoreSink** şu özelliklerde destekler **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| **copyBehavior** |Kopyalama davranışını belirtir. |<b>PreserveHierarchy</b>: Hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yoluna aynıdır.<br/><br/><b>FlattenHierarchy</b>: Tüm dosyaları kaynak klasörden hedef klasörün ilk düzeyinde oluşturulur. Hedef dosyalar otomatik olarak oluşturulan adları ile oluşturulur.<br/><br/><b>MergeFiles</b>: Tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya ya da blob adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, dosya otomatik olarak oluşturulan addır. |Hayır |

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri
Bu bölümde, elde edilen davranışını özyinelemeli ve copyBehavior değer farklı birleşimleri kopyalama işlemi açıklanmaktadır.

| recursive | copyBehavior | Sonuç davranış |
| --- | --- | --- |
| true |preserveHierarchy |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 kaynak aynı yapıda ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de dosya2 + dosya3 + File4 + 5 dosyası içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir |
| false |preserveHierarchy |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |
| false |flattenHierarchy |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |
| false |mergeFiles |Bir kaynak klasörü Klasör1 aşağıdaki yapıya sahip:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de + dosya2 içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir. Fıle1'de otomatik olarak oluşturulan adı<br/><br/>Subfolder1 dosya3 File4 ve File5 ile değil teslim alındı. |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Ayrıntılar için bkz [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md) makalesi.

## <a name="json-examples-for-copying-data-to-and-from-data-lake-store"></a>Data Lake Store gelen ve giden veri kopyalamak için JSON örnekleri
Aşağıdaki örneklerde, örnek JSON tanımları sağlanır. Bu örnek tanımlarını kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Örnekler ve Data Lake Store ve Azure Blob depolama alanından verileri kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir _doğrudan_ herhangi birinden herhangi birine desteklenen kaynakları başlatır. Daha fazla bilgi için bkz: "desteklenen veri depoları ve biçimler" bölümündeki [kopyalama etkinliğiyle veri taşıma](data-factory-data-movement-activities.md) makalesi.

### <a name="example-copy-data-from-azure-blob-storage-to-azure-data-lake-store"></a>Örnek: Azure Data Lake Store için Azure Blob depolamadan veri kopyalama
Bu bölümdeki örnek kodu gösterir:

* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bağlı hizmet türü [birlikte AzureDataLakeStore](#linked-service-properties).
* Girdi [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [birlikte AzureDataLakeStore](#dataset-properties).
* A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinlikli [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve [AzureDataLakeStoreSink](#copy-activity-properties).

Zaman serisi verileri Azure Blob Depolama'dan nasıl olduğunu örnekler her saat için Data Lake Store kopyalanır.

**Azure Storage bağlı hizmeti**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**Azure Data Lake Store bağlı hizmeti**

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

> [!NOTE]
> Yapılandırma ayrıntıları için bkz. [bağlı hizmeti özellikleri](#linked-service-properties) bölümü.
>

**Azure Blob girdi veri kümesi**

Aşağıdaki örnekte, verileri yeni bir blobun saatte seçilir (`"frequency": "Hour", "interval": 1`). Blob klasörü yolu ve dosya adı dinamik olarak değerlendirilir işlenmekte olan dilimin başlangıç zamanı temel alınarak. Klasör yolu, yıl, ay ve gün kısmını başlangıç saatini kullanır. Dosya adı, başlangıç zamanı saat bölümünü kullanır. `"external": true` Ayar, Data Factory hizmetinin tablo harici veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ]
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**Azure Data Lake Store çıktı veri kümesi**

Aşağıdaki örnek, Data Lake Store için veri kopyalar. Yeni veriler her saat için Data Lake Store kopyalanır.

```JSON
{
    "name": "AzureDataLakeStoreOutput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Blob kaynağı ve havuz Data Lake Store ile bir işlem hattındaki kopyalama etkinliği**

Aşağıdaki örnekte, işlem hattının giriş ve çıkış veri kümesi için yapılandırılmış bir kopyalama etkinliği içeriyor. Kopyalama etkinliği, saatte çalışacak şekilde zamanlanır. JSON tanımı, işlem hattındaki `source` türü ayarlandığında `BlobSource`ve `sink` türü ayarlandığında `AzureDataLakeStoreSink`.

```json
{
    "name":"SamplePipeline",
    "properties":
    {
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":
        [
            {
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureDataLakeStoreOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

### <a name="example-copy-data-from-azure-data-lake-store-to-an-azure-blob"></a>Örnek: Bir Azure blob'a veri Azure Data Lake Store'dan kopyalama
Bu bölümdeki örnek kodu gösterir:

* Bağlı hizmet türü [birlikte AzureDataLakeStore](#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Girdi [veri kümesi](data-factory-create-datasets.md) türü [birlikte AzureDataLakeStore](#dataset-properties).
* Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinlikli [kümesinin kullanılması gerekir](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Kod zaman serisi verileri Data Lake Store ' Azure blobuna saatte kopyalar.

**Azure Data Lake Store bağlı hizmeti**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        }
    }
}
```

> [!NOTE]
> Yapılandırma ayrıntıları için bkz. [bağlı hizmeti özellikleri](#linked-service-properties) bölümü.
>

**Azure Storage bağlı hizmeti**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure Data Lake giriş veri kümesi**

Bu örnekte, ayarı `"external"` için `true` tablo harici veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil Data Factory hizmetinin bildirir.

```json
{
    "name": "AzureDataLakeStoreInput",
    "properties":
    {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
**Azure Blob çıktı veri kümesi**

Aşağıdaki örnekte, veriler her saat yeni bir bloba yazılır (`"frequency": "Hour", "interval": 1`). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat kısmı, başlangıç zamanı klasör yolu kullanır.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Bir Azure Data Lake Store kaynak ve havuz blob ile bir işlem hattındaki kopyalama etkinliği**

Aşağıdaki örnekte, işlem hattının giriş ve çıkış veri kümesi için yapılandırılmış bir kopyalama etkinliği içeriyor. Kopyalama etkinliği, saatte çalışacak şekilde zamanlanır. JSON tanımı, işlem hattındaki `source` türü ayarlandığında `AzureDataLakeStoreSource`ve `sink` türü ayarlandığında `BlobSink`.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[
            {
                "name": "AzureDakeLaketoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureDataLakeStoreInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureDataLakeStoreSource",
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

Kopyalama etkinliği tanımında, kaynak veri kümesindeki sütunları havuz veri kümesi sütunlara eşleyebilirsiniz. Ayrıntılar için bkz [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayar
Kopyalama etkinliği performansı ve bunu en iyi duruma getirme etkileyen faktörler hakkında bilgi edinmek için [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) makalesi.
