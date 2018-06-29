---
title: Azure Data Lake Store gelen ve giden veri kopyalama | Microsoft Docs
description: Azure Data Factory kullanarak Data Lake Store gelen ve giden veri kopyalama öğrenin
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 8f86f43b4d8c474f338285abffb3c444f5ebc2d7
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054747"
---
# <a name="copy-data-to-and-from-data-lake-store-by-using-data-factory"></a>Veri Fabrikası kullanarak Data Lake Store gelen ve giden veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-azure-datalake-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-data-lake-store.md)

> [!NOTE]
> Bu makale, veri fabrikası 1 sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2 Azure Data Lake Store Bağlayıcısı](../connector-azure-data-lake-store.md).

Bu makalede kopya etkinliği Azure Data Factory'de ve Azure Data Lake Store gelen verileri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, veri taşıma kopyalama etkinliği ile bir genel bakış.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Veri kopyalama **Azure Data Lake Store gelen** aşağıdaki veri depolar:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarına verileri kopyalayabilirsiniz **Azure Data Lake Store'a**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Kopyalama etkinliği ile işlem hattı oluşturmadan önce bir Data Lake Store hesabı oluşturun. Daha fazla bilgi için bkz: [Azure Data Lake Store ile çalışmaya başlama](../../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="supported-authentication-types"></a>Desteklenen kimlik doğrulama türleri
Data Lake Store Bağlayıcısı'nı bu kimlik doğrulama türlerini destekler:
* Hizmet sorumlusu kimlik doğrulaması
* Kullanıcı kimlik bilgisi (OAuth) kimlik doğrulaması 

Hizmet asıl kimlik doğrulaması, özellikle bir zamanlanmış veri kopyalama için kullanmanızı öneririz. Belirteç süre sonu davranış kullanıcı kimlik bilgilerinin ile ortaya çıkabilir. Yapılandırma ayrıntıları için bkz: [bağlantılı hizmet özellikleri](#linked-service-properties) bölümü.

## <a name="get-started"></a>başlarken
Verileri farklı araçlar/API'lerini kullanarak Azure Data Lake Store denetleyicisinden taşır kopyalama etkinliği ile işlem hattı oluşturun.

Verileri kopyalamak için bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma, Öğretici için bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu** , **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma bir **veri fabrikası**. Veri Fabrikası bir veya daha fazla ardışık düzen içerebilir. 
2. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, bir Azure Data Lake Store bir Azure blob depolama alanından veri kopyalıyorsanız, Azure depolama hesabı ve Azure Data Lake deposu veri fabrikanıza bağlamak için iki bağlı hizmet oluşturun. Azure Data Lake Store için özel bağlantılı hizmet özellikleri için bkz: [bağlantılı hizmet özellikleri](#linked-service-properties) bölümü. 
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. Son adımda bahsedilen örnekte blob kapsayıcısı ve giriş verilerini içeren klasörü belirtmek için bir veri kümesi oluşturun. Ve blob depolama biriminden kopyalanan verileri tutar Data Lake deposu klasör ve dosya yolu belirtmek üzere başka bir veri kümesi oluşturun. Azure Data Lake Store için özel veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. Daha önce bahsedilen örnekte BlobSource bir kaynak ve AzureDataLakeStoreSink havuzu olarak kopya etkinliği için kullanırsınız. Azure Blob depolama alanına Azure Data Lake Deposu'ndan veri kopyalıyorsanız benzer şekilde, AzureDataLakeStoreSource ve BlobSink kopyalama etkinliği kullanırsınız. Azure Data Lake Store için belirli kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir veri deposu bir kaynak veya bir havuz nasıl kullanılacağı hakkında daha fazla bilgi için önceki bölümde, veri deposu için bağlantıya tıklayın.  

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  Bir Azure Data Lake Store/veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımlarıyla örnekleri için bkz: [JSON örnekler](#json-examples-for-copying-data-to-and-from-data-lake-store) bu makalenin.

Aşağıdaki bölümler, Data Lake Store'a Data Factory varlıklarını belirli tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Bağlı hizmet, veri fabrikası için bir veri deposu bağlar. Bağlı hizmet türü oluşturma **AzureDataLakeStore** Data Lake Store verilerinizi veri fabrikanıza bağlamak için. Aşağıdaki tabloda, bağlı Data Lake Store Services'e özel JSON öğelerini açıklar. Hizmet sorumlusu ve kullanıcı kimlik bilgileri doğrulaması arasında seçim yapabilirsiniz.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **type** | Type özelliği ayarlamak **AzureDataLakeStore**. | Evet |
| **dataLakeStoreUri** | Azure Data Lake Store hesabı hakkında bilgi. Bu bilgiler aşağıdaki biçimlerden birini alır: `https://[accountname].azuredatalakestore.net/webhdfs/v1` veya `adl://[accountname].azuredatalakestore.net/`. | Evet |
| **Subscriptionıd** | Data Lake Store hesabına ait olduğu azure abonelik kimliği. | Havuz için gerekli |
| **resourceGroupName** | Data Lake Store hesabına ait olduğu azure kaynak grubu adı. | Havuz için gerekli |

### <a name="service-principal-authentication-recommended"></a>(Önerilen) hizmet asıl kimlik doğrulaması
Hizmet asıl kimlik doğrulaması kullanmak için Azure Active Directory (Azure AD) bir uygulama varlığı kaydetmek ve Data Lake Store'a erişim izni. Ayrıntılı adımlar için bkz: [hizmeti için kimlik doğrulama](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlantılı hizmet tanımlamak için kullandığınız aşağıdaki değerleri not edin:
* Uygulama Kimliği
* Uygulama anahtarı 
* Kiracı Kimliği

> [!IMPORTANT]
> Hizmet asıl uygun Azure Data Lake Store'da izni olduğundan emin olun:
>- **Data Lake Store kaynağı olarak kullanmak için**, en az izni **okuma + yürütme** veri erişim izni listesi ve bir klasörün içeriğini kopyalayın veya **okuma** tek bir dosya kopyalama izni. Hesap düzeyinde erişim denetimi gereksinimi yoktur.
>- **Data Lake Store havuzu olarak kullanılacak**, en az izni **yazma + yürütme** veri erişim alt öğeleri klasöründe oluşturma izni. Kopya güçlendirmeniz Azure IR kullanıyorsanız (kaynak ve havuz olan buluta), veri fabrikası Data Lake Store'nın bölge algılamak izin için en az izni **okuyucu** hesap erişim denetimi (IAM) rolü. Bu IAM rol önlemek istiyorsanız [executionLocation belirtin](data-factory-data-movement-activities.md#global) Data Lake Store kopyalama etkinliğinde konumu ile.
>- Varsa, **ardışık düzen yazmak için kopyalama Sihirbazı'nı kullanın**, en az izni **okuyucu** hesap erişim denetimi (IAM) rolü. Ayrıca, en az izni **okuma + yürütme** ("/"), Data Lake Store kök ve alt öğelerini izni. Aksi takdirde, "sağlanan kimlik bilgileri geçersiz." iletisi görebilirsiniz

Hizmet asıl kimlik doğrulaması, aşağıdaki özellikleri belirterek kullanın:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **servicePrincipalId** | Uygulamanın istemci kimliği belirtin. | Evet |
| **servicePrincipalKey** | Uygulamanın anahtarını belirtin. | Evet |
| **Kiracı** | Uygulamanızın bulunduğu altında Kiracı bilgileri (etki alanı adı veya Kiracı kimliği) belirtin. Azure portalının sağ üst köşedeki fare gelerek alabilir. | Evet |

**Örnek: Hizmet asıl kimlik doğrulaması**
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
Alternatif olarak, aşağıdaki özellikleri belirterek veya bu Data Lake Store kopyalamak için kullanıcı kimlik bilgilerinin kullanabilirsiniz:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **Yetkilendirme** | Tıklatın **Authorize** Data Factory Düzenleyici'düğmesine tıklayın ve bu özelliği otomatik olarak oluşturulur yetkilendirme URL'si atar kimlik bilgilerinizi girin. | Evet |
| **sessionId** | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersiz olup yalnızca bir kez kullanılabilir. Data Factory Düzenleyici kullandığınızda bu ayarı otomatik olarak oluşturulur. | Evet |

> [!IMPORTANT]
> Azure Data Lake Store'da kullanıcı uygun izni vermek emin olun:
>- **Data Lake Store kaynağı olarak kullanmak için**, en az izni **okuma + yürütme** veri erişim izni listesi ve bir klasörün içeriğini kopyalayın veya **okuma** tek bir dosya kopyalama izni. Hesap düzeyinde erişim denetimi gereksinimi yoktur.
>- **Data Lake Store havuzu olarak kullanılacak**, en az izni **yazma + yürütme** veri erişim alt öğeleri klasöründe oluşturma izni. Kopya güçlendirmeniz Azure IR kullanıyorsanız (kaynak ve havuz olan buluta), veri fabrikası Data Lake Store'nın bölge algılamak izin için en az izni **okuyucu** hesap erişim denetimi (IAM) rolü. Bu IAM rol önlemek istiyorsanız [executionLocation belirtin](data-factory-data-movement-activities.md#global) Data Lake Store kopyalama etkinliğinde konumu ile.
>- Varsa, **ardışık düzen yazmak için kopyalama Sihirbazı'nı kullanın**, en az izni **okuyucu** hesap erişim denetimi (IAM) rolü. Ayrıca, en az izni **okuma + yürütme** ("/"), Data Lake Store kök ve alt öğelerini izni. Aksi takdirde, "sağlanan kimlik bilgileri geçersiz." iletisi görebilirsiniz

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
Kullanarak oluşturmak yetkilendirme kodu **Authorize** düğmesi belirli bir miktar süre sonra süresi dolar. Aşağıdaki ileti kimlik doğrulama belirteci süresinin dolduğunu anlamına gelir:

Kimlik bilgisi işlemi hatası: invalid_grant - AADSTS70002: Kimlik doğrulanırken hata oluştu. AADSTS70008: Sağlanan erişim izninin süresi doldu veya iptal edildi. İzleme kimliği: d18629e8-af88-43c5-88e3-d8419eb1fca1 bağıntı kimliği: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 zaman damgası: 2015-12-15 21 09 31Z.

Aşağıdaki tabloda, farklı türlerdeki kullanıcı hesapları zaman aşımı süresinin gösterilmektedir:

| Kullanıcı türü | Kullanım süresi sonu |
|:--- |:--- |
| Kullanıcı hesaplarını *değil* Azure Active Directory tarafından yönetilen (örneğin, @hotmail.com veya @live.com) |12 saat |
| Azure Active Directory tarafından yönetilen kullanıcılar hesapları |14 gün sonra en son dilim çalıştırın <br/><br/>90 OAuth tabanlı bir bağlantılı hizmette dayanan bir dilim 14 günde bir en az bir kez çalıştırıyorsa gün |

Belirteci süre sonu önce parolanızı değiştirirseniz, belirteç hemen süresi dolar. Bu bölümde daha önce bahsedilen iletisi görürsünüz.

Kullanarak hesabı yeniden yetkilendirmek **Authorize** düğmesini bağlantılı hizmet dağıtmak için belirtecin süresi dolduğunda. Değerleri de oluşturabilirsiniz **SessionID** ve **yetkilendirme** aşağıdaki kodu kullanarak program aracılığıyla özellikleri:


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
Kod içinde kullanılan veri fabrikası sınıfları hakkında daha fazla ayrıntı için bkz: [AzureDataLakeStoreLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), ve [ AuthorizationSessionGetResponse sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) Konular. Sürümü bir başvuru ekleyin `2.9.10826.1824` , `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` için `WindowsFormsWebAuthenticationDialog` kod içinde kullanılan bir sınıftır.

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

**Belirti:** veri kopyalama işlemi sırasında **içine** kopyalama etkinliği şu hata ile başarısız olursa Azure Data Lake Store,:

  ```
  Failed to detect the region for Azure Data Lake account {your account name}. Please make sure that the Resource Group name: {resource group name} and subscription ID: {subscription ID} of this Azure Data Lake Store resource are correct.
  ```

**Kök neden:** 2 olası nedeni vardır:

1. `resourceGroupName` Ve/veya `subscriptionId` Azure Data Lake bağlantılı Store'a içinde hizmet yanlış; için belirtilen
2. Kullanıcı veya hizmet sorumlusu gerekli izne sahip değil.

**Çözüm:**

1. Emin olun `subscriptionId` ve `resourceGroupName` bağlantılı hizmeti belirtin `typeProperties` gerçekten veri gölü hesabına ait olanlardır.

2. En az izni olduğundan emin olun "**okuyucu**" kullanıcı veya hizmet asıl veri gölü hesabı üzerinde rol. Bunu yapmak nasıl şöyledir:

    1. Azure Portal -> Data Lake Store hesabınız
    2. Data Lake Store dikey "erişim denetimi (IAM)"'i tıklatın
    3. Dikey penceresinde "erişim denetimi (IAM)", "Ekle"'yi tıklatın
    4. "Rol" "Okuyucu" olarak ayarlayın ve kullanıcı veya kopya için erişim vermek için kullandığınız hizmet sorumlusu seçin

3. Kullanıcı veya hizmet sorumlusu "Okuyucu" rolü vermek istemiyorsanız, alernative kullanmaktır [açıkça bir yürütme konumu belirtmek](data-factory-data-movement-activities.md#global) kopyalama activitywith Data Lake Store konumunu içinde. Örnek:

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
Bir Data Lake Store giriş verilerini göstermek için bir veri kümesi belirtmek için ayarladığınız **türü** dataset özelliğinin **AzureDataLakeStore**. Ayarlama **linkedServiceName** Data Lake Store adını dataset özelliğinin bağlı hizmeti. JSON bölümler ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. JSON, bir veri kümesini bölümlerini gibi **yapısı**, **kullanılabilirlik**, ve **İlkesi**, tüm veri kümesi türleri için benzerdir (Azure SQL database, Azure blob ve Azure tablo için Örnek). **TypeProperties** bölüm veri kümesi her tür için farklıdır ve konum ve verilerin veri deposunda biçimi gibi bilgiler sağlar. 

**TypeProperties** bir veri kümesi için bir bölüm türü **AzureDataLakeStore** aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **folderPath** |Kapsayıcı ve Data Lake Store klasörü yolu. |Evet |
| **Dosya adı** |Azure Data Lake Store'da dosyasının adı. **FileName** özelliği isteğe bağlıdır ve büyük küçük harfe duyarlı. <br/><br/>Belirtirseniz **fileName**, belirli bir dosya üzerinde (kopyalama dahil) etkinliği çalışır.<br/><br/>Zaman **fileName** belirtilmezse, kopyalama içeren tüm dosyaları **folderPath** girdi veri kümesi içinde.<br/><br/>Zaman **fileName** bir çıkış veri kümesi için belirtilmemiş ve **preserveHierarchy** belirtilmedi etkinlik havuzunda oluşturulan dosyanın adıdır veri biçiminde. _GUID_.txt'. Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt. |Hayır |
| **partitionedBy** |**PartitionedBy** özelliği isteğe bağlıdır. Dinamik yolu ve dosya adını time series verilerini belirtmek için kullanabilirsiniz. Örneğin, **folderPath** için verileri saatte parametreli olabilir. Ayrıntılı bilgi ve örnekler için bkz: [partitionedBy özelliği](#using-partitionedby-property). |Hayır |
| **Biçimi** | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve  **ParquetFormat**. Ayarlama **türü** altında özellik **biçimi** şu değerlerden biri için. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [JSON biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi ](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler [Azure Data Factory tarafından desteklenen dosya ve sıkıştırma biçimleri](data-factory-supported-file-and-compression-formats.md) makalesi. <br><br> Dosyaları kopyalamak istiyorsanız, "olarak-olan" dosya tabanlı depoları arasında (ikili kopya), atla `format` iki girdi ve çıktı veri kümesi tanımları bölümünde. |Hayır |
| **Sıkıştırma** | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory tarafından desteklenen dosya ve sıkıştırma biçimleri](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır |

### <a name="the-partitionedby-property"></a>PartitionedBy özelliği
Dinamik belirtebilirsiniz **folderPath** ve **fileName** zaman serisi verilerle özelliklerini **partitionedBy** özelliği, veri fabrikası işlevleri ve sistem değişkenleri. Ayrıntılar için bkz [Azure Data Factory - işlevler ve sistem değişkenleri](data-factory-functions-variables.md) makalesi.


Aşağıdaki örnekte, `{Slice}` Data Factory sistem değişkenin değeriyle değiştirilir `SliceStart` belirtilen biçimde (`yyyyMMddHH`). Adı `SliceStart` dilimin başlangıç zamanı gösterir. `folderPath` Özelliktir giriş olarak her dilim için farklı `wikidatagateway/wikisampledataout/2014100103` veya `wikidatagateway/wikisampledataout/2014100104`.

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Aşağıdaki örnek, yıl, ay, gün ve saati de `SliceStart` tarafından kullanılan ayrı değişkenleri içine ayıklanan `folderPath` ve `fileName` özellikleri:
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
Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında daha fazla bilgi için bkz: [Azure Data Factory'deki veri kümelerini](data-factory-create-datasets.md) ve [Data Factory zamanlama ve yürütme](data-factory-scheduling-and-execution.md) makaleleri. 


## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilke gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

Kullanılabilir özellikler **typeProperties** bölüm etkinliğin her etkinlik türü ile değişir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

**AzureDataLakeStoreSource** aşağıdaki özelliğinde destekleyen **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| **özyinelemeli** |Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. |(Varsayılan değer) false değerini true |Hayır |


**AzureDataLakeStoreSink** aşağıdaki özellikleri destekler **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| **copyBehavior** |Kopyalama davranışını belirtir. |<b>PreserveHierarchy</b>: Dosya hiyerarşisi hedef klasördeki korur. Kaynak dosyanın kaynak klasöre göreli yol, hedef dosya hedef klasöre göreli yolunu aynıdır.<br/><br/><b>FlattenHierarchy</b>: tüm dosyaları kaynak klasörden hedef klasörün ilk düzeyi oluşturulur. Hedef dosyalar otomatik olarak oluşturulur adlarıyla oluşturulur.<br/><br/><b>MergeFiles</b>: bir dosya için kaynak klasöründeki tüm dosyaları birleştirir. Dosya veya blob adı belirtilirse, birleştirilmiş dosya adı belirtilen addır. Aksi takdirde, dosya adı oluşturulur. |Hayır |

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri
Bu bölümde, sonuçta elde edilen davranışını özyinelemeli ve copyBehavior değerleri farklı birleşimlerini kopyalama işlemi açıklanmaktadır.

| özyinelemeli | copyBehavior | Bunun sonucunda oluşan davranışı |
| --- | --- | --- |
| true |preserveHierarchy |Bir kaynak klasörü için Klasör1 şu yapıda: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 kaynak aynı yapısını oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy |Bir kaynak klasörü için Klasör1 şu yapıda: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya1 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan adı |
| true |mergeFiles |Bir kaynak klasörü için Klasör1 şu yapıda: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef Klasör1 aşağıdaki yapısıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1 + dosya2 + dosya3 + File4 + 5 dosyası içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir |
| false |preserveHierarchy |Bir kaynak klasörü için Klasör1 şu yapıda: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/><br/>Dosya3, File4 ve File5 Subfolder1 değil toplanma. |
| false |flattenHierarchy |Bir kaynak klasörü için Klasör1 şu yapıda:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya1 için otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan adı<br/><br/><br/>Dosya3, File4 ve File5 Subfolder1 değil toplanma. |
| false |mergeFiles |Bir kaynak klasörü için Klasör1 şu yapıda:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Hedef klasör Klasör1 aşağıdaki yapısıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1 + dosya2 içeriği otomatik olarak oluşturulan dosya adında bir dosya halinde birleştirilir. dosya1 için otomatik olarak oluşturulan adı<br/><br/>Dosya3, File4 ve File5 Subfolder1 değil toplanma. |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Ayrıntılar için bkz [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md) makalesi.

## <a name="json-examples-for-copying-data-to-and-from-data-lake-store"></a>Data Lake Store gelen ve giden veri kopyalamak için JSON örnekleri
Aşağıdaki örnekler örnek JSON tanımları sağlar. Bu örnek tanımları kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Örnekler ve Data Lake Store ve Azure Blob depolama biriminden veri kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir _doğrudan_ herhangi birinden herhangi birine desteklenen kaynakları iç havuzlar. Daha fazla bilgi için "desteklenen veri depoları ve biçimleri" bölümüne bakın [Kopyala etkinliğini kullanarak veri taşıma](data-factory-data-movement-activities.md) makalesi.  

### <a name="example-copy-data-from-azure-blob-storage-to-azure-data-lake-store"></a>Örnek: verileri Azure Blob depolama alanından Azure Data Lake Store için kopyalama
Bu bölümdeki örnek kodu göstermektedir:

* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bağlı hizmet türü [AzureDataLakeStore](#linked-service-properties).
* Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureDataLakeStore](#dataset-properties).
* A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve [AzureDataLakeStoreSink](#copy-activity-properties).

Ne zaman serisi verileri Azure Blob depolama biriminden olan örnekler için Data Lake Store saatte kopyalanır. 

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

**Azure Data Lake Store hizmeti bağlı**

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
> Yapılandırma ayrıntıları için bkz: [bağlantılı hizmet özellikleri](#linked-service-properties) bölümü.
>

**Azure Blob girdi veri kümesi**

Aşağıdaki örnekte, verileri yeni bir blob üzerinden saatte kayıt (`"frequency": "Hour", "interval": 1`). Blob klasör yolu ve dosya adı dinamik olarak değerlendirilir işleniyor dilim başlangıç zamanı temel alınarak. Klasör yolu yıl, ay ve gün başlangıç saati bölümünü kullanır. Dosya adı, başlangıç zamanı saat bölümünü kullanır. `"external": true` Ayarı tablo data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil Data Factory hizmetinin sizi bilgilendirir.

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

**Azure Data Lake Store çıkış dataset**

Aşağıdaki örnek, Data Lake Store'a veri kopyalar. Yeni veriler her saat için Data Lake Store kopyalanır.

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


**Bir blob kaynağı ve Data Lake Store havuz sahip işlem hattı kopyalama etkinliği**

Aşağıdaki örnekte, ardışık düzen giriş ve çıktı veri kümeleri için yapılandırılmış bir kopyalama etkinliği içerir. Kopyalama etkinliği saatte çalışacak şekilde zamanlanır. JSON tanımını düzenindeki `source` türü ayarlanmış `BlobSource`ve `sink` türü ayarlanmış `AzureDataLakeStoreSink`.

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

### <a name="example-copy-data-from-azure-data-lake-store-to-an-azure-blob"></a>Örnek: Verilerini Azure Data Lake Store bir Azure blob
Bu bölümdeki örnek kodu göstermektedir:

* Bağlı hizmet türü [AzureDataLakeStore](#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureDataLakeStore](#dataset-properties).
* Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [AzureDataLakeStoreSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Kodu her saat bir Azure blob veri Gölü Deposu'ndan veri time series verilerini kopyalar. 

**Azure Data Lake Store hizmeti bağlı**

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
> Yapılandırma ayrıntıları için bkz: [bağlantılı hizmet özellikleri](#linked-service-properties) bölümü.
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
**Azure Data Lake girdi veri kümesi**

Bu örnekte, ayarı `"external"` için `true` Data Factory hizmetinin tablo data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil bildirir.

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

Aşağıdaki örnekte, veriler her saat yeni bir bloba yazılır (`"frequency": "Hour", "interval": 1`). Blob klasör yolu dinamik işlenmekte olan dilim başlangıç zamanı temel alınarak değerlendirilir. Klasör yolu yıl, ay, gün ve saat bölümünü başlangıç saatini kullanır.

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

**Bir Azure Data Lake Store kaynak ve blob havuz sahip işlem hattı kopyalama etkinliği**

Aşağıdaki örnekte, ardışık düzen giriş ve çıktı veri kümeleri için yapılandırılmış bir kopyalama etkinliği içerir. Kopyalama etkinliği saatte çalışacak şekilde zamanlanır. JSON tanımını düzenindeki `source` türü ayarlanmış `AzureDataLakeStoreSource`ve `sink` türü ayarlanmış `BlobSink`.

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

Kopyalama etkinliği tanımında sütunları havuz kümesindeki kaynak kümesinden sütunları eşleyebilirsiniz. Ayrıntılar için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayar
Kopyalama etkinliği performans ve onu en iyi duruma getirme etkileyen faktörler hakkında bilgi edinmek için [kopyalama etkinliği performans ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) makalesi.
