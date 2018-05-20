---
title: Hizmet asıl adı ile Azure Analysis Services görevleri otomatik hale getirme | Microsoft Docs
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 2b49b85d39f55052e112fd9f4f0e28bdc6c91637
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="automation-with-service-principals"></a>Hizmet asıl adı ile Otomasyon

Hizmet asıl adı olan bir Azure Active Directory Uygulama kaynağı katılımsız kaynak ve hizmet düzeyi işlemleri gerçekleştirmek için Kiracı içinde oluşturun. Benzersiz bir tür olup olmadıklarını *kullanıcı kimliği* bir uygulama kimliği ve parola veya sertifika ile. Bir hizmet sorumlusu yalnızca, için atandığı izinleri ve rolleri tarafından tanımlanan görevleri gerçekleştirmek için gereken bu izinleri vardır. 

Analysis Services içinde hizmet asıl adı Azure Otomasyonu, PowerShell katılımsız modda, özel istemci uygulamaları ve web uygulamaları ile ortak görevleri otomatikleştirmek için kullanılır. Örneğin, sağlama sunucular, modelleri, veri yenileme, Ölçek dağıtma yukarı/aşağı ve Duraklat/Sürdür tüm hizmet asıl adı kullanarak otomatik olarak yapılabilir. İzinleri normal Azure AD UPN hesapları benzediğini rolü üyeliği aracılığıyla hizmet asıl adı atanmış.

## <a name="create-service-principals"></a>Hizmet sorumlusu oluşturma
 
Hizmet sorumluları, Azure portalında veya PowerShell kullanarak oluşturulabilir. Daha fazla bilgi için bkz:

[Hizmet sorumlusu - Azure portal oluşturma](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Hizmet sorumlusu oluşturma - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="store-credential-and-certificate-assets-in-azure-automation"></a>Azure Otomasyonu kimlik bilgisi ve sertifika varlıkları depolamak

Güvenli bir şekilde hizmet asıl kimlik bilgileri ve sertifikaları Azure Otomasyonu runbook işlemleri için depolanabilir. Daha fazla bilgi için bkz:

[Azure Otomasyonu kimlik bilgisi varlıkları](../automation/automation-credentials.md)   
[Azure Otomasyonu sertifika varlıkları](../automation/automation-certificates.md)

## <a name="add-service-principals-to-server-admin-role"></a>Sunucu Yönetici rolü için hizmet asıl adı ekleyin

Analysis Services sunucu yönetimi işlemleri için bir hizmet sorumlusu kullanmadan önce sunucu yöneticileri rolüne eklemeniz gerekir. Daha fazla bilgi için bkz: [için sunucu yöneticisi rolünün bir hizmet sorumlusu ekleme](analysis-services-addservprinc-admins.md).

## <a name="service-principals-in-connection-strings"></a>Bağlantı dizeleri, hizmet asıl adı

Hizmet asıl AppID ve parola veya sertifika kullanılabilir bağlantı dizeleri kadar aynı UPN.

### <a name="powershell"></a>PowerShell

Bir hizmet sorumlusu kaynak yönetim işlemleri ile kullanırken [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) modülü, kullanım `Login-AzureRmAccount` cmdlet'i. Bir hizmet asıl sunucu işlemleri ile kullanırken [SQLServer](https://www.powershellgallery.com/packages/SqlServer) modülü, kullanım `Add-AzureAnalysisServicesAccount` cmdlet'i. 

Aşağıdaki örnekte, AppID ve parola, bir model veritabanı yenileme işlemi gerçekleştirmek için kullanılır:

```PowerShell
Param (

        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force

$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

Add-AzureAnalysisServicesAccount -Credential $Credential -ServicePrincipal -TenantId $TenantId -RolloutEnvironment "westcentralus.asazure.windows.net"

Invoke-ProcessTable -Server "asazure://westcentralus.asazure.windows.net/myserver" -TableName "MyTable" -Database "MyDb" -RefreshType "Full"
```

### <a name="amo-and-adomd"></a>AMO ve ADOMD 

İstemci uygulamaları ve web uygulamaları ile bağlanırken [AMO ve ADOMD istemci kitaplıkları](analysis-services-data-providers.md) 15.0.2 ve daha yüksek yüklenebilir NuGet paketleri aşağıdaki sözdizimini kullanarak bağlantı dizeleri hizmet asıl adı desteği sürüm: `app:AppID` ve parola veya `cert:thumbprint`. 

Aşağıdaki örnekte, `appID` ve `password` bir model veritabanı yenileme işlemi gerçekleştirmek için kullanılır:

```C#
string appId = "xxx";
string authKey = "yyy";
string connString = $"Provider=MSOLAP;Data Source=asazure://westus.asazure.windows.net/<servername>;User ID=app:{appId};Password={authKey};";
Server server = new Server();
server.Connect(connString);
Database db = server.Databases.FindByName("adventureworks");
Table tbl = db.Model.Tables.Find("DimDate");
tbl.RequestRefresh(RefreshType.Full);
db.Model.SaveChanges();
```

## <a name="next-steps"></a>Sonraki adımlar
[Azure PowerShell ile oturum açın](https://docs.microsoft.com/powershell/azure/authenticate-azureps)   
[Sunucu Yöneticisi rolünün bir hizmet sorumlusu ekleme](analysis-services-addservprinc-admins.md)   