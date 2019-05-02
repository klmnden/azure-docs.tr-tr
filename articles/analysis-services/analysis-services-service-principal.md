---
title: Hizmet sorumluları ile Azure Analysis Services görevleri otomatik hale getirin | Microsoft Docs
description: Azure Analysis Services görevlerini otomatikleştirmek için hizmet sorumlusu oluşturma konusunda bilgi edinin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: a440494b183d18c1d888b5d39836eb4317190d02
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64708324"
---
# <a name="automation-with-service-principals"></a>Hizmet sorumlularıyla otomasyon

Hizmet sorumluları, katılımsız kaynak ve hizmet düzeyinde işlemler gerçekleştirmek için kiracınızın içinde oluşturduğunuz bir Azure Active Directory uygulama kaynağıdır. Benzersiz türde oldukları *kullanıcı kimliğini* bir uygulama kimliği ve parola veya sertifika ile. Bir hizmet sorumlusu yalnızca kendisi için atanan izinleri ve rolleri tarafından tanımlanan görevleri gerçekleştirmek gerekli izinleri vardır. 

Analysis Services hizmet sorumluları Azure Otomasyonu, PowerShell katılımsız modda, özel istemci uygulamaları ve web uygulamaları ile ortak görevleri otomatik hale getirmek için kullanılır. Örneğin, sağlama sunucular, dağıtma, modelleri, veri yenileme, Ölçek artırma/azaltma ve VM'yi duraklatabilir/sürdürebilirsiniz tüm hizmet sorumlularını kullanma tarafından otomatik olarak yapılabilir. Normal bir Azure AD UPN'sini hesaplarına çok benzer, rol üyeliğini hizmet sorumlularının izinlerini atanır.

## <a name="create-service-principals"></a>Hizmet sorumlusu oluşturma
 
Hizmet sorumluları, Azure portalında veya PowerShell kullanarak oluşturulabilir. Daha fazla bilgi için bkz:

[Hizmet sorumlusu - Azure portal'ı oluşturma](../active-directory/develop/howto-create-service-principal-portal.md)   
[Hizmet sorumlusu oluşturma - PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="store-credential-and-certificate-assets-in-azure-automation"></a>Azure Automation'da kimlik bilgisi ve sertifika varlıklar Store

Hizmet sorumlusu kimlik bilgileri ve sertifikaları güvenli bir şekilde Azure Otomasyonu'nda runbook işlemleri için saklanır. Daha fazla bilgi için bkz:

[Azure automation'da kimlik bilgisi varlıkları](../automation/automation-credentials.md)   
[Azure Otomasyonu'ndaki sertifika varlıkları](../automation/automation-certificates.md)

## <a name="add-service-principals-to-server-admin-role"></a>Sunucu Yöneticisi rolüne hizmet sorumlularını Ekle

Analysis Services sunucusu yönetim işlemleri için bir hizmet sorumlusu kullanabilmeniz için önce sunucu Yöneticiler rolüne eklemeniz gerekir. Daha fazla bilgi için bkz. [sunucu yöneticisi rolüne hizmet sorumlusu ekleme](analysis-services-addservprinc-admins.md).

## <a name="service-principals-in-connection-strings"></a>Bağlantı dizeleri hizmet sorumluları

Hizmet sorumlusu uygulama kimliği ve parola veya sertifika kullanılabilir bağlantı dizeleri hemen hemen aynı UPN.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

#### <a name="a-nameazmodule-using-azanalysisservices-module"></a><a name="azmodule" />Az.AnalysisServices modülünü kullanma

İle kaynak yönetimi işlemleri için bir hizmet sorumlusunu kullanırken [Az.AnalysisServices](/powershell/module/az.analysisservices) modülü, kullanım `Connect-AzAccount` cmdlet'i. 

Aşağıdaki örnekte, AppID ve parola, / ölçeği ve salt okunur kopyaya eşitleme için Denetim düzlemi işlemleri gerçekleştirmek için kullanılır:

```powershell
Param (
        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force
$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

# Connect using Az module
Connect-AzAccount -Credential $Credential -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx"

# Syncronize a database for query scale out
Sync-AzAnalysisServicesInstance -Instance "asazure://westus.asazure.windows.net/testsvr" -Database "testdb"

# Scale up the server to an S1, set 2 read-only replicas, and remove the primary from the query pool. The new replicas will hydrate from the synchronized data.
Set-AzAnalysisServicesServer -Name "testsvr" -ResourceGroupName "testRG" -Sku "S1" -ReadonlyReplicaCount 2 -DefaultConnectionMode Readonly
```

#### <a name="using-sqlserver-module"></a>SQLServer modülündeki kullanma

Aşağıdaki örnekte, uygulama kimliği ve parola, bir model veritabanı yenileme işlemi gerçekleştirmek için kullanılır:

```powershell
Param (
        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force

$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

Invoke-ProcessTable -Server "asazure://westcentralus.asazure.windows.net/myserver" -TableName "MyTable" -Database "MyDb" -RefreshType "Full" -ServicePrincipal -ApplicationId $AppId -TenantId $TenantId -Credential $Credential
```

### <a name="amo-and-adomd"></a>AMO ve ADOMD 

İstemci uygulamaları ve web apps ile bağlanırken [AMO ve ADOMD istemci kitaplıkları](analysis-services-data-providers.md) 15.0.2 ve daha yüksek yüklenebilir NuGet paketleri bağlantı dizeleri aşağıdaki sözdizimini kullanarak hizmet sorumluları destekleyen bir sürüm: `app:AppID` ve parola veya `cert:thumbprint`. 

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
[Azure PowerShell ile oturum açma için oturum açın](https://docs.microsoft.com/powershell/azure/authenticate-azureps)   
[Sunucu Yöneticisi rolüne hizmet sorumlusu ekleme](analysis-services-addservprinc-admins.md)   
