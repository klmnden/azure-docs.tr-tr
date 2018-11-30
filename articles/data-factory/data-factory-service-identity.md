---
title: Azure veri fabrikası hizmet kimliği | Microsoft Docs
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
ms.date: 11/28/2018
ms.author: jingwang
ms.openlocfilehash: 892fa32f73cec86e5d10a0d67da3d80bedd539aa
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619870"
---
# <a name="azure-data-factory-service-identity"></a>Azure veri fabrikası hizmet kimliği

Bu makale, veri fabrikası hizmet Kimliği nedir ve nasıl çalıştığını anlamanıza yardımcı olur.

## <a name="overview"></a>Genel Bakış

Veri Fabrikası oluştururken, bir hizmet kimliği factory oluşturmayla birlikte oluşturulabilir. Hizmet kimliği için Azure Activity Directory kayıtlı bir yönetilen uygulama ve bu belirli veri üretecini temsil eder.

Veri Fabrikası hizmet kimliği aşağıdaki özellikler avantajları:

- [Azure anahtar Kasası'nda kimlik bilgisi Store](store-credentials-in-key-vault.md), bu durumda, veri fabrikası hizmet kimliği Azure Key Vault kimlik doğrulaması için kullanılır.
- Bağlayıcıyı [Azure Blob Depolama](connector-azure-blob-storage.md), [Azure Data Lake depolama Gen1](connector-azure-data-lake-store.md), [Azure Data Lake depolama Gen2](connector-azure-data-lake-storage.md), [Azure SQL veritabanı](connector-azure-sql-database.md), ve [Azure SQL veri ambarı](connector-azure-sql-data-warehouse.md).
- [Web etkinliği](control-flow-web-activity.md).

## <a name="generate-service-identity"></a>Hizmet kimliği oluşturma

Veri Fabrikası hizmet kimliği şu şekilde oluşturulur:

- Data factory sayesinde oluştururken **Azure portalı veya PowerShell**, hizmet kimliği her zaman otomatik olarak oluşturulur.
- Data factory sayesinde oluştururken **SDK**, yalnızca belirttiğiniz, hizmet kimliği oluşturulacak "kimlik yeni FactoryIdentity() =" oluşturma için Fabrika nesnesinde. Örnekte bakın [.NET hızlı başlangıç - veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Data factory sayesinde oluştururken **REST API**, hizmet kimliği, istek gövdesinde "kimlik" bölümünde belirtirseniz oluşturulur. Örnekte bakın [veri fabrikası oluşturma - Hızlı Başlangıç'ı REST](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Veri fabrikanızın aşağıdaki ilişkili hizmet kimliği yoksa bulup bulamayacağınızı [hizmet kimliği almak](#retrieve-service-identity) yönergesi, açıkça oluşturun bir data factory kimlik Başlatıcı ile program aracılığıyla güncelleştirerek:

- [PowerShell kullanarak hizmet kimliği oluşturma](#generate-service-identity-using-powershell)
- [REST API kullanarak hizmet kimliği oluşturma](#generate-service-identity-using-rest-api)
- [Bir Azure Resource Manager şablonu kullanarak bir hizmet kimliği oluşturma](#generate-service-identity-using-resource-management-template)
- [Hizmet kimliği kullanarak SDK oluşturma](#generate-service-identity-using-sdk)

>[!NOTE]
>- Hizmet kimliği değiştirilemez. Bir hizmet kimliği zaten olan bir veri fabrikası güncelleştirme hiçbir etkisi olmaz, hizmet kimliği değiştirilmemiş tutulur.
>- Zaten bir hizmet kimliği üretecini "kimlik" parametresini belirtmeden veya "kimlik" bölümünde REST istek gövdesinde belirtmeden sahip bir veri fabrikası güncelleştirirseniz, bir hata alırsınız.
>- Bir veri fabrikasını silmek, ilişkili hizmet kimliği birlikte silinecek.

### <a name="generate-service-identity-using-powershell"></a>PowerShell kullanarak hizmet kimliği oluşturma

Çağrı **Set-AzureRmDataFactoryV2** "Kimlik" alanları yeni oluşturulan gördüğünüz daha sonra yeniden komutu:

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

**İstek gövdesi**: "identity" ekleyin: {"type": "SystemAssigned"}.

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

**Yanıt**: hizmet kimliği otomatik olarak oluşturulur ve "kimlik" bölümüne uygun şekilde doldurulur.

```json
{
    "name": "<dataFactoryName>",
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
    "location": "<region>"
}
```

### <a name="generate-service-identity-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak bir hizmet kimliği oluşturma

**Şablon**: "identity" ekleyin: {"type": "SystemAssigned"}.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "resources": [{
        "name": "<dataFactoryName>",
        "apiVersion": "2018-06-01",
        "type": "Microsoft.DataFactory/factories",
        "location": "<region>",
        "identity": {
            "type": "SystemAssigned"
        }
    }]
}
```

### <a name="generate-service-identity-using-sdk"></a>Hizmet kimliği kullanarak SDK oluşturma

Kimlik ile veri fabrikası create_or_update işlevi çağrısı yeni FactoryIdentity() =. .NET kullanarak örnek kod:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="retrieve-service-identity"></a>Hizmet kimliği alınamıyor

Azure portalından veya programlama yoluyla hizmet kimliği alabilir. Aşağıdaki bölümlerde bazı örnekler gösterilmektedir.

>[!TIP]
> Hizmet kimliği görmüyorsanız [hizmet kimliği oluşturma](#generate-service-identity) fabrikanızı güncelleştirerek.

### <a name="retrieve-service-identity-using-azure-portal"></a>Azure portalını kullanarak hizmet kimliği alınamıyor

Bulabilirsiniz hizmet kimlik bilgilerini Azure portalından -> data factory'nizi -> Ayarlar -> Özellikler:

- HİZMET KİMLİĞİ TANIMLAYICISI
- HİZMET KİMLİĞİ KİRACISI
- **Hizmet kimliği uygulama kimliği** > Bu değeri kopyalayın

![Hizmet kimliği alınamıyor](media/data-factory-service-identity/retrieve-service-identity-portal.png)

### <a name="retrieve-service-identity-using-powershell"></a>PowerShell kullanarak hizmet kimliği alınamıyor

Hizmet kimliği asıl kimliği ve Kiracı kimliği gibi belirli veri fabrikası aldığınızda döndürülür:

```powershell
PS C:\WINDOWS\system32> (Get-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Sorumlu Kimliği kopyalayın, sonra aşağıdaki komutu Azure Active Directory ile asıl kimlik almak için parametre olarak çalıştırın. **ApplicationId**, hangi erişim vermek için kullanın:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="next-steps"></a>Sonraki adımlar
Ne zaman ve nasıl veri fabrikası hizmet kimliği kullanmak aşağıdaki konulara bakın:

- [Azure anahtar Kasası'nda kimlik bilgisi Store](store-credentials-in-key-vault.md)
- [Azure Data Lake Store yönetilen kimliklerle Azure kaynaklarında kimlik doğrulaması için / için veri kopyalama](connector-azure-data-lake-store.md)

Bkz: [Azure kaynaklarına genel bakış için yönetilen kimlikleri](~/articles/active-directory/msi-overview.md) hangi veri fabrikası hizmet kimliği Azure kaynakları için yönetilen kimlikleri hakkında daha fazla arka plan için temel aldığı. 
