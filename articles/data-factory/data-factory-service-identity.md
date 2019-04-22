---
title: Yönetilen kimliği için Data Factory | Microsoft Docs
description: Azure Data Factory için yönetilen kimlik hakkında öğrenin.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: jingwang
ms.openlocfilehash: 3c1bb38eb12ce77d172257706cd458cebda4bd8c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59260757"
---
# <a name="managed-identity-for-data-factory"></a>Data Factory için yönetilen kimlik

Bu makale, yönetilen kimlik ne olduğunu anlamanıza yardımcı olur (eski adıyla yönetilen hizmet kimliği/MSI olarak da bilinir) Data Factory ve nasıl çalışır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış

Veri Fabrikası oluştururken, yönetilen bir kimlik factory oluşturmayla birlikte oluşturulabilir. Yönetilen kimlik Azure Activity Directory için kayıtlı bir yönetilen uygulama ve bu belirli veri üretecini temsil eder.

Data Factory için yönetilen kimlik aşağıdaki özellikleri avantajları:

- [Azure anahtar Kasası'nda kimlik bilgisi Store](store-credentials-in-key-vault.md), bu durumda data factory yönetilen kimlik Azure Key Vault kimlik doğrulaması için kullanılır.
- Bağlayıcıyı [Azure Blob Depolama](connector-azure-blob-storage.md), [Azure Data Lake depolama Gen1](connector-azure-data-lake-store.md), [Azure Data Lake depolama Gen2](connector-azure-data-lake-storage.md), [Azure SQL veritabanı](connector-azure-sql-database.md), ve [Azure SQL veri ambarı](connector-azure-sql-data-warehouse.md).
- [Web etkinliği](control-flow-web-activity.md).

## <a name="generate-managed-identity"></a>Yönetilen kimlik oluşturma

Data Factory için yönetilen kimliği şu şekilde oluşturulur:

- Data factory sayesinde oluştururken **Azure portalı veya PowerShell**, yönetilen kimlik her zaman otomatik olarak oluşturulur.
- Data factory sayesinde oluştururken **SDK**, yönetilen kimlik oluşturulur, yalnızca belirttiğiniz "kimlik yeni FactoryIdentity() =" oluşturma için Fabrika nesnesinde. Örnekte bakın [.NET hızlı başlangıç - veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Data factory sayesinde oluştururken **REST API**, yönetilen kimlik, istek gövdesinde "kimlik" bölümünde belirtirseniz oluşturulur. Örnekte bakın [veri fabrikası oluşturma - Hızlı Başlangıç'ı REST](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Veri fabrikanızın aşağıdaki ilişkili yönetilen bir kimlik yok bulup bulamayacağınızı [yönetilen kimlik almak](#retrieve-managed-identity) yönergesi, açıkça oluşturun bir data factory kimlik Başlatıcı ile program aracılığıyla güncelleştirerek:

- [PowerShell kullanarak yönetilen kimlik oluşturma](#generate-managed-identity-using-powershell)
- [REST API kullanarak yönetilen kimlik oluşturma](#generate-managed-identity-using-rest-api)
- [Bir Azure Resource Manager şablonu kullanarak bir yönetilen kimlik oluşturma](#generate-managed-identity-using-an-azure-resource-manager-template)
- [Yönetilen kimlik kullanarak SDK oluşturma](#generate-managed-identity-using-sdk)

>[!NOTE]
>- Yönetilen kimlik değiştirilemez. Zaten bir yönetilen kimliğe sahip bir veri fabrikası güncelleştirme hiçbir etkisi olmaz, yönetilen kimlik değişmeden tutulur.
>- Zaten bir yönetilen kimlik üretecini "kimlik" parametresini belirtmeden veya "kimlik" bölümünde REST istek gövdesinde belirtmeden sahip bir veri fabrikası güncelleştirirseniz, bir hata alırsınız.
>- Bir veri fabrikasını silmek, ilişkili yönetilen kimlik birlikte silinecek.

### <a name="generate-managed-identity-using-powershell"></a>PowerShell kullanarak yönetilen kimlik oluşturma

Çağrı **kümesi AzDataFactoryV2** "Kimlik" alanları yeni oluşturulan gördüğünüz daha sonra yeniden komutu:

```powershell
PS C:\WINDOWS\system32> Set-AzDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName> -Location <region>

DataFactoryName   : ADFV2DemoFactory
DataFactoryId     : /subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory
ResourceGroupName : <resourceGroupName>
Location          : East US
Tags              : {}
Identity          : Microsoft.Azure.Management.DataFactory.Models.FactoryIdentity
ProvisioningState : Succeeded
```

### <a name="generate-managed-identity-using-rest-api"></a>REST API kullanarak yönetilen kimlik oluşturma

İstek gövdesindeki "kimlik" bölümünde API çağrısı:

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2018-06-01
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

**Yanıt**: yönetilen kimlik otomatik olarak oluşturulur ve "kimlik" bölümüne uygun şekilde doldurulur.

```json
{
    "name": "<dataFactoryName>",
    "tags": {},
    "properties": {
        "provisioningState": "Succeeded",
        "loggingStorageAccountKey": "**********",
        "createTime": "2017-09-26T04:10:01.1135678Z",
        "version": "2018-06-01"
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

### <a name="generate-managed-identity-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak bir yönetilen kimlik oluşturma

**Şablon**: "identity" ekleyin: {"type": "SystemAssigned"}.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

### <a name="generate-managed-identity-using-sdk"></a>Yönetilen kimlik kullanarak SDK oluşturma

Kimlik ile veri fabrikası create_or_update işlevi çağrısı yeni FactoryIdentity() =. .NET kullanarak örnek kod:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="retrieve-managed-identity"></a>Yönetilen kimlik alma

Yönetilen kimlik Azure portalından veya programlama yoluyla alabilirsiniz. Aşağıdaki bölümlerde bazı örnekler gösterilmektedir.

>[!TIP]
> Yönetilen kimlik görmüyorsanız [yönetilen kimlik oluşturmak](#generate-managed-identity) fabrikanızı güncelleştirerek.

### <a name="retrieve-managed-identity-using-azure-portal"></a>Azure portalını kullanarak yönetilen kimliğini alma

Bulabilirsiniz yönetilen kimlik bilgileri Azure portalından -> data factory'nizi -> Özellikler:

- Yönetilen Kimlik Nesne Tanımlayıcısı
- Yönetilen Kimlik Kiracısı
- **Identity Application kimliği yönetilen** > Bu değeri kopyalayın

![Yönetilen kimlik alma](media/data-factory-service-identity/retrieve-service-identity-portal.png)

### <a name="retrieve-managed-identity-using-powershell"></a>PowerShell kullanarak yönetilen kimliğini alma

Yönetilen kimlik sorumlu kimliği ve Kiracı kimliği, belirli bir veri fabrikası gibi aldığınızda döndürülür:

```powershell
PS C:\WINDOWS\system32> (Get-AzDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Sorumlu Kimliği kopyalayın, sonra aşağıdaki komutu Azure Active Directory ile asıl kimlik almak için parametre olarak çalıştırın. **ApplicationId**, hangi erişim vermek için kullanın:

```powershell
PS C:\WINDOWS\system32> Get-AzADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="next-steps"></a>Sonraki adımlar
Hangi yaparken ve yönetilen kimliği data factory'yi nasıl dağıtır aşağıdaki konulara bakın:

- [Azure anahtar Kasası'nda kimlik bilgisi Store](store-credentials-in-key-vault.md)
- [Azure Data Lake Store yönetilen kimliklerle Azure kaynaklarında kimlik doğrulaması için / için veri kopyalama](connector-azure-data-lake-store.md)

Bkz: [Azure kaynaklarına genel bakış için yönetilen kimlikleri](/azure/active-directory/managed-identities-azure-resources/overview) hangi veri fabrikası yönetilen kimliği, Azure kaynakları için yönetilen kimlikleri hakkında daha fazla arka plan temel alınır. 
