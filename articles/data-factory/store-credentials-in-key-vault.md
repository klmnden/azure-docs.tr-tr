---
title: "Azure anahtar kasası kimlik bilgilerini saklamak | Microsoft Docs"
description: "Veri depoları Azure Data Factory çalışma zamanında otomatik olarak almak bir Azure anahtar kasası kullanılan kimlik bilgilerini depolamak öğrenin."
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: jingwang
ms.openlocfilehash: 193d7c77f01384106b3e0d932d02ba6cdff9e750
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="store-credential-in-azure-key-vault"></a>Azure anahtar kasası kimlik bilgisi deposu

Veri depolarında kimlik bilgilerini depolamak bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md). Azure Data Factory veri deposunu kullanan bir etkinliği yürütülürken kimlik bilgilerini alır. Şu anda yalnızca [Dynamics Bağlayıcısı](connector-dynamics-crm-office-365.md) bu özelliği desteklemez.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory version1 documenttion](v1/data-factory-introduction.md).

## <a name="steps"></a>Adımlar

Veri Fabrikası oluştururken, bir hizmet kimliği factory oluşturmayla birlikte oluşturulabilir. Hizmet kimliği Azure etkinlik dizinine kayıtlı yönetilen bir uygulama ve bu belirli veri üretecini temsil eder.

- Veri Fabrikası aracılığıyla oluştururken **Azure portal veya PowerShell**, hizmet kimliği her zaman otomatik olarak oluşturulur itibaren genel Önizleme.
- Veri Fabrikası aracılığıyla oluştururken **SDK**, yalnızca belirtirseniz, hizmet kimliği oluşturulacak "kimlik yeni FactoryIdentity() =" oluşturma için Fabrika nesnesinde. Örnekte [.NET hızlı başlangıç - veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Veri Fabrikası aracılığıyla oluştururken **REST API**, istek gövdesinde "kimlik" bölümü belirtirseniz, hizmet kimliği oluşturulur. Örnekte [REST hızlı başlangıç - veri fabrikası oluşturma](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Azure anahtar kasasında depolanan bir kimlik bilgisi başvurmak için aktarmanız gerekir:

1. "Hizmet kimlik uygulama yanı sıra, veri fabrikası oluşturulan kimliği" kopyalayın. Başvurmak [hizmet kimliği alma](#retrieve-service-identity).
2. Hizmet kimliği Azure anahtar Kasası'na erişim. Anahtar kasasına erişim denetimi -> -> Ekle -> okuyucu izin eklemek için bu hizmeti kimliği uygulama kimliği arayın. Gizli anahtar kasasında erişmek bu belirlenen üreteci sağlar.
3. Azure anahtar Kasası'na işaret eden bir bağlantılı hizmet oluşturun. Başvurmak [Azure anahtar kasası bağlantılı hizmeti](#azure-key-vault-linked-service).
4. Veri deposu bağlı hizmetini, hangi başvuru içinde karşılık gelen gizli depolanan anahtar kasası oluşturun. Başvurmak [anahtar kasasında depolanan kimlik bilgileri başvuru](#reference-credential-stored-in-key-vault).

## <a name="retrieve-service-identity"></a>Hizmet kimliği alma

Azure portalından veya program aracılığıyla hizmet kimliği alabilir. Aşağıdaki bölümlerde bazı örnekler göstermektedir.

>[!TIP]
> Hizmet kimliği görmüyorsanız, [hizmet kimliği oluşturma](#generate-service-identity) Fabrikanızda güncelleştirerek.

### <a name="using-azure-portal"></a>Azure portalını kullanma

Bulabileceğiniz Azure portalından hizmeti kimlik bilgileri -> veri fabrikanızın Ayarları -> Özellikler ->:

- HİZMET KİMLİK KİMLİĞİ
- HİZMET KİMLİK KİRACI
- **HİZMETİ kimliği uygulama kimliği** > anahtar kasasına erişim vermek için bu değeri kopyalayın

![Hizmet kimliği alma](media/store-credentials-in-key-vault/retrieve-service-identity-portal.png)

### <a name="using-powershell"></a>PowerShell’i kullanma

Belirli veri fabrikası gibi aldığınızda, hizmet kimliği asıl kimliği ve Kiracı kimliği döndürülür:

```powershell
PS C:\WINDOWS\system32> (Get-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Sorumlu Kimliği kopyalayın ve sonra aşağıdaki Azure Active Directory komutunu asıl kimliği ile almak için parametre olarak çalıştır **ApplicationId**, hangi anahtar kasasına erişim vermek için kullanın:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="azure-key-vault-linked-service"></a>Azure anahtar Kasası'bağlı hizmeti

Aşağıdaki özellikler, Azure anahtar kasası bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureKeyVault**. | Evet |
| BaseUrl | Azure anahtar kasası URL'si belirtin. | Evet |

**Örnek:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
        "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-credential-stored-in-key-vault"></a>Anahtar kasasında depolanan başvuru kimlik bilgisi

Bir anahtar kasası gizlilik başvuran bağlantılı hizmetteki bir alan yapılandırırken aşağıdaki özellikleri desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Alanın türü özelliğini ayarlamak: **AzureKeyVaultSecret**. | Evet |
| secretName | Azure anahtar kasası gizliliği adı. | Evet |
| secretVersion | Azure anahtar kasası gizliliği sürümü.<br/>Belirtilmezse, her zaman en son sürümünü gizli anahtarı kullanır.<br/>Ardından belirtilirse, belirtilen sürüme sticks.| Hayır |
| Depolama | Kimlik bilgilerini saklamak için kullanacağınız bir Azure anahtar kasası bağlantılı hizmeti ifade eder. | Evet |

**Örnek: ("parola" bölümüne bakın)**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "mySecret",
                "store":{
                    "linkedServiceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="generate-service-identity"></a>Hizmet kimliği oluşturma

Veri fabrikanızın aşağıdaki ilişkili hizmet kimliği yok bulursanız [hizmet kimliği alma](#retrieve-service-identity) yönerge, üretebilir bir data factory ile kimlik Başlatıcı programlı olarak güncelleştiriliyor.

> [!NOTE]
> - **Hizmet kimliği değiştirilemez**. Bir hizmet kimliği zaten olan bir veri fabrikası güncelleştirme hiçbir etkisi olmaz, hizmet kimliği değiştirilmemiş tutulacak.
> - **Hizmet kimliği silinemiyor**. Zaten bir hizmet kimliği üretecini "kimlik" parametresinde belirtmeden veya REST istek gövdesinde "kimlik" bölümü belirtmeden sahip bir veri fabrikası güncelleştirirseniz, bir hata alırsınız.

Aşağıdaki bölümlerde, henüz yoksa, fabrikası için hizmet kimliği oluşturma bazı örnekler göstermektedir.

### <a name="using-powershell"></a>PowerShell’i kullanma

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

### <a name="using-rest-api"></a>REST API kullanma

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

### <a name="using-sdk"></a>SDK'sını kullanarak

Kimliğine sahip veri fabrikası create_or_update işlevini çağırın yeni FactoryIdentity() =. .NET kullanarak örnek kod:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).