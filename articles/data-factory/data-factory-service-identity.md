---
title: Azure Data Factory hizmeti kimliği | Microsoft Docs
description: Azure Data factory'de veri fabrikası hizmet kimliği hakkında bilgi edinin.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: jingwang
ms.openlocfilehash: f4ce76385897c24bd5259d5a39aa1756769fe2aa
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284489"
---
# <a name="azure-data-factory-service-identity"></a>Azure Data Factory hizmet kimliği

Bu makalede, veri fabrikası hizmet Kimliği nedir ve nasıl çalıştığını anlamanıza yardımcı olur.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory version1 belgelerine](v1/data-factory-introduction.md).

## <a name="overview"></a>Genel Bakış

Veri Fabrikası oluştururken, bir hizmet kimliği factory oluşturmayla birlikte oluşturulabilir. Hizmet kimliği Azure etkinlik dizinine kayıtlı yönetilen bir uygulama ve bu belirli veri üretecini temsil eder.

Veri Fabrikası hizmet kimliği aşağıdaki iki özellik avantajlar:

- [Azure anahtar kasası kimlik bilgisi depolamak](store-credentials-in-key-vault.md), veri fabrikası hizmet kimliği; bu durumda Azure anahtar kasası kimlik doğrulaması için kullanılır.
- [/ Azure Data Lake Store'a veri kopyalamak](connector-azure-data-lake-store.md), bu durumda veri fabrikası hizmet kimliği desteklenen Data Lake Store kimlik doğrulama türlerinden biri olarak kullanılabilir.

## <a name="generate-service-identity"></a>Hizmet kimliği oluşturma

Veri Fabrikası hizmet kimliği şu şekilde oluşturulur:

- Veri Fabrikası aracılığıyla oluştururken **Azure portal veya PowerShell**, hizmet kimliği her zaman otomatik olarak oluşturulur ADF V2 genel Önizleme itibaren.
- Veri Fabrikası aracılığıyla oluştururken **SDK**, yalnızca belirtirseniz, hizmet kimliği oluşturulacak "kimlik yeni FactoryIdentity() =" oluşturma için Fabrika nesnesinde. Örnekte bkz [.NET hızlı başlangıç - veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Veri Fabrikası aracılığıyla oluştururken **REST API**, istek gövdesinde "kimlik" bölümü belirtirseniz, hizmet kimliği oluşturulur. Örnekte bkz [REST hızlı başlangıç - veri fabrikası oluşturma](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Veri fabrikanızın aşağıdaki ilişkili hizmet kimliği yok bulursanız [hizmet kimliği alma](#retrieve-service-identity) yönerge, siz açıkça üretebilir bir data factory ile kimlik Başlatıcı program aracılığıyla güncelleştirme:

- [PowerShell kullanarak hizmet kimliği oluşturma](#generate-service-identity-using-powershell)
- [REST API kullanarak hizmet kimliği oluşturma](#generate-service-identity-using-rest-api)
- [SDK'yı kullanarak hizmet kimliği oluşturma](#generate-service-identity-using-sdk)

>[!NOTE]
>- Hizmet kimliği değiştirilemez. Bir hizmet kimliği zaten olan bir veri fabrikası güncelleştirme hiçbir etkisi olmaz, hizmet kimliği değiştirilmemiş tutulur.
>- Zaten bir hizmet kimliği üretecini "kimlik" parametresinde belirtmeden veya REST istek gövdesinde "kimlik" bölümü belirtmeden sahip bir veri fabrikası güncelleştirirseniz, bir hata alırsınız.
>- Veri Fabrikası sildiğinizde, ilişkili hizmet kimliği boyunca silinir.

### <a name="generate-service-identity-using-powershell"></a>PowerShell kullanarak hizmet kimliği oluşturma

Çağrı **kümesi AzureRmDataFactoryV2** "Kimlik" alanları yeni oluşturulan gördüğünüz daha sonra yeniden, komut:

```powershell
PS C:\WINDOWS\system32> Set-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName> -Location <region>

DataFactoryName   : ADFV2DemoFactory
DataFactoryId     : /subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory
ResourceGroupName : <resourceGroupName>
Location          : East US
Tags              : {}
Identity          : Microsoft.Azure.Management.DataFactory.Models.FactoryIdentity
ProvisioningState : Succeeded
```

### <a name="generate-service-identity-using-rest-api"></a>REST API kullanarak hizmet kimliği oluşturma

İstek gövdesindeki "kimlik" bölümünde API çağrısı:

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2017-09-01-preview
```

**İstek gövdesindeki**: "kimlik" ekleyin: {"tür": "SystemAssigned"}.

```json
{
    "name": "<dataFactoryName>",
    "location": "<region>",
    "properties": {},
    "identity": {
        "type": "SystemAssigned"
    }
}
```

**Yanıt**: hizmet kimliği otomatik olarak oluşturulur ve "kimlik" bölümü buna uygun olarak doldurulur.

```json
{
    "name": "ADFV2DemoFactory",
    "tags": {},
    "properties": {
        "provisioningState": "Succeeded",
        "loggingStorageAccountKey": "**********",
        "createTime": "2017-09-26T04:10:01.1135678Z",
        "version": "2017-09-01-preview"
    },
    "identity": {
        "type": "SystemAssigned",
        "principalId": "765ad4ab-XXXX-XXXX-XXXX-51ed985819dc",
        "tenantId": "72f988bf-XXXX-XXXX-XXXX-2d7cd011db47"
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory",
    "type": "Microsoft.DataFactory/factories",
    "location": "EastUS"
}
```

### <a name="generate-service-identity-using-sdk"></a>SDK'yı kullanarak hizmet kimliği oluşturma

Kimliğine sahip veri fabrikası create_or_update işlevini çağırın yeni FactoryIdentity() =. .NET kullanarak örnek kod:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="retrieve-service-identity"></a>Hizmet kimliği alma

Azure portalından veya program aracılığıyla hizmet kimliği alabilir. Aşağıdaki bölümlerde bazı örnekler göstermektedir.

>[!TIP]
> Hizmet kimliği görmüyorsanız, [hizmet kimliği oluşturma](#generate-service-identity) Fabrikanızda güncelleştirerek.

### <a name="retrieve-service-identity-using-azure-portal"></a>Azure portalını kullanarak hizmet kimliği alma

Bulabileceğiniz Azure portalından hizmeti kimlik bilgileri -> veri fabrikanızın Ayarları -> Özellikler ->:

- HİZMET KİMLİK KİMLİĞİ
- HİZMET KİMLİK KİRACI
- **HİZMETİ kimliği uygulama kimliği** > Bu değeri kopyalayın

![Hizmet kimliği alma](media/data-factory-service-identity/retrieve-service-identity-portal.png)

### <a name="retrieve-service-identity-using-powershell"></a>PowerShell kullanarak hizmet kimliği alma

Belirli veri fabrikası gibi aldığınızda, hizmet kimliği asıl kimliği ve Kiracı kimliği döndürülür:

```powershell
PS C:\WINDOWS\system32> (Get-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Sorumlu Kimliği kopyalayın ve sonra aşağıdaki Azure Active Directory komutunu asıl kimliği ile almak için parametre olarak çalıştır **ApplicationId**, hangi erişim vermek için kullanın:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası hizmet kimlik kullanmak üzere nasıl ve ne zaman tanıtmak için aşağıdaki konulara bakın:

- [Azure anahtar kasası kimlik bilgisi deposu](store-credentials-in-key-vault.md)
- [Yönetilen hizmet kimlik doğrulama kullanarak Azure Data Lake Store başlangıç/bitiş veri kopyalama](connector-azure-data-lake-store.md)

Bkz: [MSI genel bakış](~/articles/active-directory/msi-overview.md) hangi veri fabrikası hizmet kimliği yönetilen hizmet kimliği hakkında daha fazla arka plan için temel aldığı. 