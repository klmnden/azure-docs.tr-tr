---
title: Azure'da Office 365 yönetim çözümü | Microsoft Docs
description: Bu makalede, yapılandırma ve azure'da Office 365 çözüm kullanımı hakkında ayrıntılı bilgi sağlar.  Azure İzleyici'de oluşturulan Office 365 kayıtları ayrıntılı açıklamasını içerir.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/24/2019
ms.author: bwren
ms.openlocfilehash: da9e322f74433df7066ec574db7a49123f96d76b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66130741"
---
# <a name="office-365-management-solution-in-azure-preview"></a>Office 365 Yönetim çözümüne (Önizleme)

![Office 365 logosu](media/solution-office-365/icon.png)

Office 365 yönetim çözümü, Azure İzleyici'de, Office 365 ortamı izlemenize olanak sağlar.

- Kullanıcı etkinlikleri Office 365 hesaplarınızın yanı sıra kullanım düzenlerini çözümleme için davranış eğilimlerini izleyin. Örneğin, kuruluşunuz ya da en popüler SharePoint siteleri dışında paylaşılan dosyalar gibi belirli kullanım senaryoları ayıklayabilirsiniz.
- Yapılandırma değişiklikleri veya yüksek ayrıcalıklı işlemleri izlemek için yönetici etkinliklerini izler.
- Algılama ve kuruluş gereksinimlerinize özelleştirilebilen istenmeyen kullanıcı davranışı araştırın.
- Denetim ve uyumluluk gösterir. Örneğin, dosya erişim işlemleri ve denetim ve uyumluluk işlemiyle size gibi gizli bilgiler içeren dosyaları üzerinde izleyebilirsiniz.
- Kullanarak işletimsel sorun giderme işlemleri uygulayabilirsiniz [oturum sorguları](../log-query/log-query-overview.md) kuruluşunuzun Office 365 etkinlik verileri üzerinde.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Yüklenmiş ve yapılandırılmış bu çözüm olan önce gerekli verilmiştir.

- Kuruluş Office 365 aboneliği.
- Genel yönetici olan bir kullanıcı hesabı için kimlik bilgileri.
- Denetim verileri almak için şunları yapmanız gerekir [denetimi yapılandırma](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&rs=en-US&ad=US#PickTab=Before_you_begin) Office 365 aboneliğinize.  Unutmayın [posta kutusu denetimini](https://technet.microsoft.com/library/dn879651.aspx) ayrı olarak yapılandırılır.  Yine de çözümü yüklemek ve denetim yapılandırılmamışsa, diğer veri toplayın.
 

## <a name="management-packs"></a>Yönetim paketleri

Bu çözüm, tüm yönetim paketlerinde yüklemez [bağlı Yönetim grupları](../platform/om-agents.md).
  

## <a name="install-and-configure"></a>Yükleme ve yapılandırma

Başlangıç ekleyerek [aboneliğinizi Office 365 çözüme](solutions.md#install-a-monitoring-solution). Eklendikten sonra Office 365 aboneliğinize erişimi vermek için bu bölümdeki yapılandırma adımları gerçekleştirmeniz gerekir.

### <a name="required-information"></a>Gerekli bilgileri

Bu yordama başlamadan önce aşağıdaki bilgileri toplayın.

Log Analytics çalışma alanınızdan:

- Çalışma alanı adı: Office 365 verilerine nerede toplanacağını çalışma alanı.
- Kaynak grubu adı: Çalışma alanını içeren kaynak grubu.
- Azure abonelik kimliği: Çalışma alanını içeren aboneliği.

Office 365 aboneliğinize:

- Kullanıcı Adı: Bir yönetici hesabı e-posta adresi.
- Kiracı Kimliği: Office 365 aboneliğiniz için benzersiz kimlik.
- İstemci kimliği: Office 365 istemci temsil eden 16-karakter dizesi.
- İstemci gizli anahtarı: Kimlik doğrulaması için gereken şifreli dize.

### <a name="create-an-office-365-application-in-azure-active-directory"></a>Azure Active Directory'de bir Office 365 uygulaması oluşturma

İlk adım, Azure Active Directory yönetim çözümü, Office 365 çözümünüzü erişmek için kullanacağı bir uygulama oluşturmaktır.

1. [https://portal.azure.com](https://portal.azure.com/) adresinden Azure portalında oturum açın.
1. Seçin **Azure Active Directory** ardından **uygulama kayıtları**.
1. **Yeni uygulama kaydı**’na tıklayın.

    ![Uygulama kaydı ekleme](media/solution-office-365/add-app-registration.png)
1. Bir uygulama girin **adı** ve **oturum açma URL'si**.  Adı açıklayıcı olmalıdır.  Kullanım `http://localhost` URL'si ve canlı _Web uygulaması / API_ için **uygulama türü**
    
    ![Uygulama oluştur](media/solution-office-365/create-application.png)
1. Tıklayın **Oluştur** ve uygulama bilgilerini doğrulayın.

    ![Kayıtlı uygulama](media/solution-office-365/registered-app.png)

### <a name="configure-application-for-office-365"></a>Office 365 için uygulamayı yapılandırma

1. Tıklayın **ayarları** açmak için **ayarları** menüsü.
1. Seçin **özellikleri**. Değişiklik **çok kiracılı** için _Evet_.

    ![Ayarları multitenant](media/solution-office-365/settings-multitenant.png)

1. Seçin **gerekli izinler** içinde **ayarları** menüsünü seçin ve ardından **Ekle**.
1. Tıklayın **bir API seçin** ardından **Office 365 Yönetim API'leri**. tıklayın **Office 365 Yönetim API'leri**. **Seç**'e tıklayın.

    ![API Seçin](media/solution-office-365/select-api.png)

1. Altında **izinleri seçin** hem de aşağıdaki seçenekleri belirleyin **uygulama izinleri** ve **temsilci izinleri**:
   - Kuruluşunuza ilişkin hizmet durumu bilgilerini okur
   - Kuruluşunuz için etkinlik verilerini okuyun
   - Kuruluşunuza ilişkin etkinlik raporlarını okur

     ![API Seçin](media/solution-office-365/select-permissions.png)

1. Tıklayın **seçin** ardından **Bitti**.
1. Tıklayın **izinleri verin** ve ardından **Evet** doğrulama için sorulduğunda.

    ![İzin ver](media/solution-office-365/grant-permissions.png)

### <a name="add-a-key-for-the-application"></a>Uygulama için bir anahtar ekleyin

1. Seçin **anahtarları** içinde **ayarları** menüsü.
1. Yazın bir **açıklama** ve **süresi** yeni anahtar için.
1. Tıklayın **Kaydet** kopyalayın **değer** , oluşturulur.

    ![Anahtarlar](media/solution-office-365/keys.png)

### <a name="add-admin-consent"></a>Yönetici onayı ekleme

İlk kez yönetim hesabı etkinleştirmek için uygulama için yönetici onayı sağlamanız gerekir. Bir PowerShell Betiği ile bunu yapabilirsiniz. 

1. Aşağıdaki betik olarak Kaydet *office365_consent.ps1*.

    ```powershell
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,     
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId
    )
    
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    
    IF ($Subscription -eq $null)
        {Login-AzAccount -ErrorAction Stop}
    $Subscription = (Select-AzSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $Workspace = (Set-AzOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $WorkspaceLocation= $Workspace.Location
    $WorkspaceLocation
    
    Function AdminConsent{
    
    $domain='login.microsoftonline.com'
    switch ($WorkspaceLocation.Replace(" ","").ToLower()) {
           "eastus"   {$OfficeAppClientId="d7eb65b0-8167-4b5d-b371-719a2e5e30cc"; break}
           "westeurope"   {$OfficeAppClientId="c9005da2-023d-40f1-a17a-2b7d91af4ede"; break}
           "southeastasia"   {$OfficeAppClientId="09c5b521-648d-4e29-81ff-7f3a71b27270"; break}
           "australiasoutheast"  {$OfficeAppClientId="f553e464-612b-480f-adb9-14fd8b6cbff8"; break}   
           "westcentralus"  {$OfficeAppClientId="98a2a546-84b4-49c0-88b8-11b011dc8c4e"; break}
           "japaneast"   {$OfficeAppClientId="b07d97d3-731b-4247-93d1-755b5dae91cb"; break}
           "uksouth"   {$OfficeAppClientId="f232cf9b-e7a9-4ebb-a143-be00850cd22a"; break}
           "centralindia"   {$OfficeAppClientId="ffbd6cf4-cba8-4bea-8b08-4fb5ee2a60bd"; break}
           "canadacentral"  {$OfficeAppClientId="c2d686db-f759-43c9-ade5-9d7aeec19455"; break}
           "eastus2"  {$OfficeAppClientId="7eb65b0-8167-4b5d-b371-719a2e5e30cc"; break}
           "westus2"  {$OfficeAppClientId="98a2a546-84b4-49c0-88b8-11b011dc8c4e"; break} #Need to check
           "usgovvirginia" {$OfficeAppClientId="c8b41a87-f8c5-4d10-98a4-f8c11c3933fe"; 
                             $domain='login.microsoftonline.us'; break}
           default {$OfficeAppClientId="55b65fb5-b825-43b5-8972-c8b6875867c1";
                    $domain='login.windows-ppe.net'; break} #Int
        }
    
        $domain
        Start-Process -FilePath  "https://$($domain)/common/adminconsent?client_id=$($OfficeAppClientId)&state=12345"
    }
    
    AdminConsent -ErrorAction Stop
    ```

2. Aşağıdaki komutu kullanarak betiği çalıştırın. İki kez kimlik bilgileri istenir. Log Analytics çalışma alanınız için önce kimlik bilgilerini sağlayın ve ardından Office 365 genel yönetici kimlik bilgilerini Kiracı.

    ```
    .\office365_consent.ps1 -WorkspaceName <Workspace name> -ResourceGroupName <Resource group name> -SubscriptionId <Subscription ID>
    ```

    Örnek:

    ```
    .\office365_consent.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631- yyyyyyyyyyyy'
    ```

1. Aşağıdakine benzer bir pencere ile sunulur. Tıklayın **kabul**.
    
    ![Yönetici onayı](media/solution-office-365/admin-consent.png)

### <a name="subscribe-to-log-analytics-workspace"></a>Log Analytics çalışma alanına abone olun

Son adım Log Analytics çalışma alanınıza uygulama abone olmaktır. Ayrıca bir PowerShell Betiği ile bunu.

1. Aşağıdaki betik olarak Kaydet *office365_subscription.ps1*.

    ```powershell
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId,
        [Parameter(Mandatory=$True)][string]$OfficeUsername,
        [Parameter(Mandatory=$True)][string]$OfficeTennantId,
        [Parameter(Mandatory=$True)][string]$OfficeClientId,
        [Parameter(Mandatory=$True)][string]$OfficeClientSecret
    )
    $line='#-------------------------------------------------------------------------------------------------------------------------------------------------------------------------'
    $line
    IF ($Subscription -eq $null)
        {Login-AzAccount -ErrorAction Stop}
    $Subscription = (Select-AzSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    $Workspace = (Set-AzOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $Workspace
    $WorkspaceLocation= $Workspace.Location
    $OfficeClientSecret =[uri]::EscapeDataString($OfficeClientSecret)
    
    # Client ID for Azure PowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2"
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    $domain='login.microsoftonline.com'
    $adTenant = $Subscription[0].Tenant.Id
    $authority = "https://login.windows.net/$adTenant";
    $ARMResource ="https://management.azure.com/";
    $xms_client_tenant_Id ='55b65fb5-b825-43b5-8972-c8b6875867c1'
    
    switch ($WorkspaceLocation) {
           "USGov Virginia" { 
                             $domain='login.microsoftonline.us';
                              $authority = "https://login.microsoftonline.us/$adTenant";
                              $ARMResource ="https://management.usgovcloudapi.net/"; break} # US Gov Virginia
           default {
                    $domain='login.microsoftonline.com'; 
                    $authority = "https://login.windows.net/$adTenant";
                    $ARMResource ="https://management.azure.com/";break} 
                    }
    
    Function RESTAPI-Auth { 
    
    $global:SubscriptionID = $Subscription.SubscriptionId
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURIARM=$ARMResource;
    # Authenticate and Acquire Token 
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $global:authResultARM = $authContext.AcquireToken($resourceAppIdURIARM, $clientId, $redirectUri, "Auto")
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $authHeader
    }
    
    Function Failure {
    $line
    $formatstring = "{0} : {1}`n{2}`n" +
                    "    + CategoryInfo          : {3}`n" +
                    "    + FullyQualifiedErrorId : {4}`n"
    $fields = $_.InvocationInfo.MyCommand.Name,
              $_.ErrorDetails.Message,
              $_.InvocationInfo.PositionMessage,
              $_.CategoryInfo.ToString(),
              $_.FullyQualifiedErrorId
    
    $formatstring -f $fields
    $_.Exception.Response
    
    $line
    break
    }
    
    Function Connection-API
    {
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $ResourceName = "https://manage.office.com"
    $SubscriptionId   =  $Subscription[0].Subscription.Id
    
    $line
    $connectionAPIUrl = $ARMResource + 'subscriptions/' + $SubscriptionId + '/resourceGroups/' + $ResourceGroupName + '/providers/Microsoft.OperationalInsights/workspaces/' + $WorkspaceName + '/connections/office365connection_' + $SubscriptionId + $OfficeTennantId + '?api-version=2017-04-26-preview'
    $connectionAPIUrl
    $line
    
    $xms_client_tenant_Id ='1da8f770-27f4-4351-8cb3-43ee54f14759'
    
    $BodyString = "{
                    'properties': {
                                    'AuthProvider':'Office365',
                                    'clientId': '" + $OfficeClientId + "',
                                    'clientSecret': '" + $OfficeClientSecret + "',
                                    'Username': '" + $OfficeUsername   + "',
                                    'Url': 'https://$($domain)/" + $OfficeTennantId + "/oauth2/token',
                                  },
                    'etag': '*',
                    'kind': 'Connection',
                    'solution': 'Connection',
                   }"
    
    $params = @{
        ContentType = 'application/json'
        Headers = @{
        'Authorization'="$($authHeader)"
        'x-ms-client-tenant-id'=$xms_client_tenant_Id #Prod-'1da8f770-27f4-4351-8cb3-43ee54f14759'
        'Content-Type' = 'application/json'
        }
        Body = $BodyString
        Method = 'Put'
        URI = $connectionAPIUrl
    }
    $response = Invoke-WebRequest @params 
    $response
    $line
    
    }
    
    Function Office-Subscribe-Call{
    try{
    #----------------------------------------------------------------------------------------------------------------------------------------------
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $SubscriptionId   =  $Subscription[0].Subscription.Id
    $OfficeAPIUrl = $ARMResource + 'subscriptions/' + $SubscriptionId + '/resourceGroups/' + $ResourceGroupName + '/providers/Microsoft.OperationalInsights/workspaces/' + $WorkspaceName + '/datasources/office365datasources_' + $SubscriptionId + $OfficeTennantId + '?api-version=2015-11-01-preview'
    
    $OfficeBodyString = "{
                    'properties': {
                                    'AuthProvider':'Office365',
                                    'office365TenantID': '" + $OfficeTennantId + "',
                                    'connectionID': 'office365connection_" + $SubscriptionId + $OfficeTennantId + "',
                                    'office365AdminUsername': '" + $OfficeUsername + "',
                                    'contentTypes':'Audit.Exchange,Audit.AzureActiveDirectory,Audit.SharePoint'
                                  },
                    'etag': '*',
                    'kind': 'Office365',
                    'solution': 'Office365',
                   }"
    
    $Officeparams = @{
        ContentType = 'application/json'
        Headers = @{
        'Authorization'="$($authHeader)"
        'x-ms-client-tenant-id'=$xms_client_tenant_Id
        'Content-Type' = 'application/json'
        }
        Body = $OfficeBodyString
        Method = 'Put'
        URI = $OfficeAPIUrl
      }
    
    $officeresponse = Invoke-WebRequest @Officeparams 
    $officeresponse
    }
    catch{ Failure }
    }
    
    #GetDetails 
    RESTAPI-Auth -ErrorAction Stop
    Connection-API -ErrorAction Stop
    Office-Subscribe-Call -ErrorAction Stop
    ```

2. Betiği ile aşağıdaki komutu çalıştırın:

    ```
    .\office365_subscription.ps1 -WorkspaceName <Log Analytics workspace name> -ResourceGroupName <Resource Group name> -SubscriptionId <Subscription ID> -OfficeUsername <OfficeUsername> -OfficeTennantID <Tenant ID> -OfficeClientId <Client ID> -OfficeClientSecret <Client secret>
    ```

    Örnek:

    ```powershell
    .\office365_subscription.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631-yyyyyyyyyyyy' -OfficeUsername 'admin@contoso.com' -OfficeTennantID 'ce4464f8-a172-4dcf-b675-xxxxxxxxxxxx' -OfficeClientId 'f8f14c50-5438-4c51-8956-zzzzzzzzzzzz' -OfficeClientSecret 'y5Lrwthu6n5QgLOWlqhvKqtVUZXX0exrA2KRHmtHgQb='
    ```

### <a name="troubleshooting"></a>Sorun giderme

Uygulamanız bu çalışma alanına zaten abone olunursa veya başka bir çalışma alanına bu Kiracı abone olunursa şu hatayı görebilirsiniz.

```Output
Invoke-WebRequest : {"Message":"An error has occurred."}
At C:\Users\v-tanmah\Desktop\ps scripts\office365_subscription.ps1:161 char:19
+ $officeresponse = Invoke-WebRequest @Officeparams
+                   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebException
    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand 
```

Geçersiz parametre değerleri sağlanırsa, aşağıdaki hatayı görebilirsiniz.

```Output
Select-AzSubscription : Please provide a valid tenant or a valid subscription.
At line:12 char:18
+ ... cription = (Select-AzSubscription -SubscriptionId $($Subscriptio ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzContext], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.SetAzContextCommand

```

## <a name="uninstall"></a>Kaldır

Bağlantısındaki işlemi kullanarak Office 365 yönetim çözümü kaldırabilirsiniz [bir yönetim çözümünü Kaldır](solutions.md#remove-a-monitoring-solution). Bu, Office 365'ten Azure İzleyici ile ancak toplanan verilerin durdurmaz. Office 365'ten aboneliği ve veri toplamayı durdurmak için aşağıdaki yordamı izleyin.

1. Aşağıdaki betik olarak Kaydet *office365_unsubscribe.ps1*.

    ```powershell
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId,
        [Parameter(Mandatory=$True)][string]$OfficeTennantId
    )
    $line='#-------------------------------------------------------------------------------------------------------------------------------------------------------------------------'
    
    $line
    IF ($Subscription -eq $null)
        {Login-AzAccount -ErrorAction Stop}
    $Subscription = (Select-AzSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    $Workspace = (Set-AzOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $Workspace
    $WorkspaceLocation= $Workspace.Location
    
    # Client ID for Azure PowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2"
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    $domain='login.microsoftonline.com'
    $adTenant =  $Subscription[0].Tenant.Id
    $authority = "https://login.windows.net/$adTenant";
    $ARMResource ="https://management.azure.com/";
    $xms_client_tenant_Id ='55b65fb5-b825-43b5-8972-c8b6875867c1'
    
    switch ($WorkspaceLocation) {
           "USGov Virginia" { 
                             $domain='login.microsoftonline.us';
                              $authority = "https://login.microsoftonline.us/$adTenant";
                              $ARMResource ="https://management.usgovcloudapi.net/"; break} # US Gov Virginia
           default {
                    $domain='login.microsoftonline.com'; 
                    $authority = "https://login.windows.net/$adTenant";
                    $ARMResource ="https://management.azure.com/";break} 
                    }
    
    Function RESTAPI-Auth { 
    
    $global:SubscriptionID = $Subscription.SubscriptionId
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURIARM=$ARMResource;
    # Authenticate and Acquire Token 
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $global:authResultARM = $authContext.AcquireToken($resourceAppIdURIARM, $clientId, $redirectUri, "Auto")
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $authHeader
    }
    
    Function Office-UnSubscribe-Call{
    
    #----------------------------------------------------------------------------------------------------------------------------------------------
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $ResourceName = "https://manage.office.com"
    $SubscriptionId   = $Subscription[0].Subscription.Id
    $OfficeAPIUrl = $ARMResource + 'subscriptions/' + $SubscriptionId + '/resourceGroups/' + $ResourceGroupName + '/providers/Microsoft.OperationalInsights/workspaces/' + $WorkspaceName + '/datasources/office365datasources_'  + $SubscriptionId + $OfficeTennantId + '?api-version=2015-11-01-preview'
    
    $Officeparams = @{
        ContentType = 'application/json'
        Headers = @{
        'Authorization'="$($authHeader)"
        'x-ms-client-tenant-id'=$xms_client_tenant_Id
        'Content-Type' = 'application/json'
        }
        Method = 'Delete'
        URI = $OfficeAPIUrl
      }
    
    $officeresponse = Invoke-WebRequest @Officeparams 
    $officeresponse
    
    }
    
    #GetDetails 
    RESTAPI-Auth -ErrorAction Stop
    Office-UnSubscribe-Call -ErrorAction Stop
    ```

2. Betiği ile aşağıdaki komutu çalıştırın:

    ```
    .\office365_unsubscribe.ps1 -WorkspaceName <Log Analytics workspace name> -ResourceGroupName <Resource Group name> -SubscriptionId <Subscription ID> -OfficeTennantID <Tenant ID> 
    ```

    Örnek:

    ```powershell
    .\office365_unsubscribe.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631-yyyyyyyyyyyy' -OfficeTennantID 'ce4464f8-a172-4dcf-b675-xxxxxxxxxxxx'
    ```

## <a name="data-collection"></a>Veri toplama

### <a name="supported-agents"></a>Desteklenen aracılar

Office 365 çözüm herhangi bir veri almıyorsa [Log Analytics aracılarını](../platform/agent-data-sources.md).  Office 365'ten doğrudan verileri alır.

### <a name="collection-frequency"></a>Toplama sıklığı

Bu, başlangıçta Toplanacak veriler için birkaç saat sürebilir. Toplama başladığında, Office 365 gönderen bir [Web kancası bildirim](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) ayrıntılı veriler ile Azure İzleyici için her bir kayıt oluşturulur. Bu kayıt alınan sonra birkaç dakika içinde Azure İzleyici'de kullanılabilir.

## <a name="using-the-solution"></a>Çözümü kullanma

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

Log Analytics çalışma alanınıza, Office 365 çözümü eklediğinizde **Office 365** kutucuk, panonuza eklenir. Bu kutucukta, ortamınızdaki bilgisayarların sayısına ve güncelleştirme uyumluluğuna ilişkin bir sayı ve grafik gösterimi görüntülenir.<br><br>
![Office 365 Özet kutucuğu](media/solution-office-365/tile.png)  

Tıklayarak **Office 365** açmak için kutucuğa **Office 365** Pano.

![Office 365 Panosu](media/solution-office-365/dashboard.png)  

Pano aşağıdaki tabloda gösterilen sütunları içerir. Her bir sütunun Belirtilen kapsam ve zaman aralığı için söz konusu sütunun ölçütlerle eşleşen sayısına göre en çok kullanılan on uyarılar listeler. Bkz: sütununun altındaki tüm tıklayarak ya da sütun başlığına tıklayarak listenin tamamını sağlayan bir günlük araması çalıştırabilirsiniz.

| Sütun | Açıklama |
|:--|:--|
| İşlemler | İzlenen tüm Office 365 aboneliklerinizi ilişkin etkin kullanıcılar hakkında bilgi sağlar. Zaman içinde gerçekleşen etkinlikler sayısını görmek mümkün olacaktır.
| Exchange | Exchange Server posta kutusu Ekle izni veya Set-posta kutusu eylemlerinde dökümünü gösterir. |
| SharePoint | Kullanıcılar SharePoint belgelerindeki gerçekleştirmek üst etkinlikleri gösterir. Bu kutucuklardan birini detaya arama sayfasında hedef belge ve Bu etkinliğin konumu gibi bu etkinlikler ile ilgili ayrıntıları gösterir. Örneğin, bir dosyaya erişim olayında, erişiliyor, belgeyi görmek mümkün olmayacak ilişkili hesabın adı ve IP adresi. |
| Azure Active Directory | Kullanıcı parola sıfırlama ve oturum açma denemesi gibi üst kullanıcı etkinlikleri içerir. Detaya, bu etkinliklerin sonuç durumu gibi ayrıntıları görmek mümkün olacaktır. Bu genellikle, Azure Active Directory şüpheli etkinlikleri izlemek istiyorsanız yararlıdır. |




## <a name="azure-monitor-log-records"></a>Azure İzleyici kayıtlarını günlüğe kaydet

Azure İzleyici'de Log Analytics çalışma alanında Office 365 çözüm tarafından oluşturulan tüm kayıtları bir **türü** , **OfficeActivity**.  **OfficeWorkload** özelliği, Exchange, AzureActiveDirectory, SharePoint veya OneDrive için - kayıt başvuruyor hangi Office 365 hizmet belirler.  **RecordType** özelliği, işlem türünü belirtir.  Özellikler, her işlem türü için farklılık gösterir ve aşağıdaki tablolarda gösterilmiştir.

### <a name="common-properties"></a>Ortak Özellikler

Aşağıdaki özellikler, tüm Office 365 kayıtlarına yaygındır.

| Özellik | Description |
|:--- |:--- |
| Tür | *OfficeActivity* |
| Clientıp | Etkinlik günlüğe kaydedildiğinde kullanılan cihazın IP adresi. IP adresi IPv4 veya IPv6 adresi biçiminde görüntülenir. |
| OfficeWorkload | Kayıt başvurduğu office 365 hizmeti.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| İşlem | Kullanıcı veya yönetici etkinliğinin adı.  |
| OrganizationId | Kuruluşunuzun Office 365 kiracısı için GUID. Bu değer her zaman içinde gerçekleştiği Office 365 hizmet ne olursa olsun, kuruluşunuz için aynı olacaktır. |
| RecordType | Gerçekleştirilen işlem türü. |
| ResultStatus | Eylemin (Operation özelliğinde belirtilen) başarılı olup olmadığını gösterir. Olası değerler şunlardır: başarılı oldu, kısmen başarılı veya başarısız oldu. Exchange yönetim etkinliği için ya da True değeridir ya da yanlış. |
| UserId | UPN'sini (kullanıcı asıl adı) günlüğe kaydedilmesini kaydında sonuçlanan eylemi gerçekleştiren kullanıcının; Örneğin, my_name@my_domain_name. Sistem hesapları (sharepoınt\system veya gibi ntauthority\system adlı) tarafından gerçekleştirilen etkinlik kayıtları da dahil edilir. | 
| UserKey | Kullanıcı Kimliği özelliğinde belirtilen kullanıcı için alternatif bir kimliği.  Örneğin, bu özellik, kullanıcıların SharePoint, OneDrive iş ve Exchange için gerçekleştirilen olayları için passport benzersiz Tanımlayıcısı (PUID) ile doldurulur. Bu özellik, diğer hizmetler ve sistem hesapları tarafından gerçekleştirilen olayları gerçekleşen olayları için kullanıcı kimliği özelliği olarak aynı değeri de belirtebilirsiniz|
| UserType | İşlemi gerçekleştiren kullanıcının türü.<br><br>Yönetim Bölgesi<br>Uygulama<br>DcAdmin<br>Normal<br>Ayrılmış<br>ServicePrincipal<br>Sistem |


### <a name="azure-active-directory-base"></a>Azure Active Directory temel

Aşağıdaki özellikler, tüm Azure Active Directory kayıtlarına yaygındır.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | Azure AD olay türü. |
| ExtendedProperties | Azure AD olay genişletilmiş özellikleri. |


### <a name="azure-active-directory-account-logon"></a>Azure Active Directory hesabı oturum açma

Bir Active Directory kullanıcı oturum açmayı denediğinde bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectoryAccountLogon |
| Uygulama | Office 15 gibi hesap oturum açma olayı tetikleyen uygulama. |
| İstemci | İstemci ayrıntılarını cihaz, cihaz işletim sistemi ve kullanıldı cihaz tarayıcısı hesabı oturum açma olayı. |
| LoginStatus | Bu özellik, OrgIdLogon.LoginStatus doğrudan kullanılır. Çeşitli ilgi çekici oturum açma hataları eşleme algoritmaları uyarı tarafından yapılabilir. |
| UserDomain | Kiracı kimlik bilgilerini (TII). | 


### <a name="azure-active-directory"></a>Azure Active Directory

Azure Active Directory nesneleri değiştirme veya ekleme yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AADTarget | Kullanıcı eylemi (Operation özelliği tarafından tanımlanır) üzerinde gerçekleştirildi. |
| Aktör | Gerçekleştirilen eylem hizmet sorumlusu veya kullanıcı. |
| ActorContextId | Aktör ait olduğu kuruluş GUİD'si. |
| ActorIpAddress | Aktörün IP adresi IPv4 veya IPv6 adresi biçiminde. |
| InterSystemsId | Office 365 hizmet içinde bileşenleri arasında eylemleri izlemek GUID. |
| IntraSystemId |   Azure izleme eylemi için Active Directory'yi tarafından oluşturulan GUID. |
| SupportTicketId | Müşteri durumlarda "act-on-behalf-of" eylemi için bilet kimliği destekler. |
| TargetContextId | Hedeflenen kullanıcının ait olduğu kuruluş GUİD'si. |


### <a name="data-center-security"></a>Veri Merkezi güvenliği

Bu kayıtlar, veri merkezi güvenlik denetim verilerden oluşturulur.  

| Özellik | Açıklama |
|:--- |:--- |
| EffectiveOrganization | Yükseltme/cmdlet hedeflenmiş Kiracı adı. |
| ElevationApprovedTime | Ne zaman yükseltme onaylandığı için zaman damgası. |
| ElevationApprover | Bir Microsoft Yöneticisi adı. |
| ElevationDuration | Ayrıcalık etkin olduğu süre. |
| ElevationRequestId |  Yükseltme isteği için benzersiz bir tanımlayıcı. |
| ElevationRole | Ayrıcalık rolü için istendi. |
| ElevationTime | Ayrıcalık başlangıç zamanı. |
| Start_Time | Cmdlet'ini yürütme başlangıç saati. |


### <a name="exchange-admin"></a>Exchange Yöneticisi

Exchange yapılandırmasını değişiklik yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeAdmin |
| ExternalAccess |  Cmdlet, kuruluşunuz, Microsoft Veri merkezinde personeli tarafından veya bir veri merkezinde hizmet hesabı bir kullanıcı veya yönetici temsilcisi tarafından çalıştırılıp çalıştırılmadığını belirler. ' % S'değeri False, cmdlet, kuruluşunuzdaki bir kişi tarafından çalıştırıldığı gösterir. Cmdlet'i, veri merkezi personelinin, bir veri merkezinde hizmet hesabı veya yönetici temsilcisi tarafından çalıştırıldı True değerini gösterir. |
| ModifiedObjectResolvedName |  Bu cmdlet tarafından değiştirilmiş olan nesne kolay adıdır. Bu cmdlet nesne değiştirirse günlüğe kaydedilir. |
| OrganizationName | Kiracı adı. |
| OriginatingServer | Cmdlet yürütüldüğü sunucunun adı. |
| Parametreler | Ad ve işlemleri özelliğinde tanımlanır cmdlet ile kullanılan tüm parametreler için değer. |


### <a name="exchange-mailbox"></a>Exchange posta kutusu

Değişiklikler ve eklemeler Exchange posta kutularına yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| ClientInfoString | Tarayıcı sürümü, Outlook sürüm ve mobil cihaz bilgileri gibi işlemi gerçekleştirmek için kullanılan e-posta istemcisi hakkında bilgi. |
| Client_IPAddress | İşlem günlüğe kaydedildiğinde kullanılan cihazın IP adresi. IP adresi IPv4 veya IPv6 adresi biçiminde görüntülenir. |
| ClientMachineName | Outlook istemcisi barındıran makine adı. |
| ClientProcessName | Posta kutusu erişmek için kullanılan e-posta istemcisi. |
| ClientVersion | E-posta istemcisi sürümü. |
| InternalLogonType | İç kullanım için ayrılmıştır. |
| Logon_Type | Posta kutusu erişilen ve günlüğe kaydedildi işlemin gerçekleştirilmesinden kullanıcı türünü belirtir. |
| LogonUserDisplayName |    İşlemi gerçekleştiren kullanıcının kolay adı. |
| LogonUserSid | İşlemi gerçekleştiren kullanıcının SID'si. |
| MailboxGuid | Erişilmiş olan posta kutusunun Exchange GUID. |
| MailboxOwnerMasterAccountSid | Posta kutusu sahibi hesabın asıl hesap SID'si. |
| MailboxOwnerSid | Posta kutusu sahibi SID'si. |
| MailboxOwnerUPN | Erişilmiş olan posta kutusu sahibi olan kişinin e-posta adresi. |


### <a name="exchange-mailbox-audit"></a>Exchange posta kutusu denetimi

Bir posta kutusu denetim girişi oluşturulduğunda bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| Öğe | Üzerinde işlemin gerçekleştirildiği öğesini temsil eder | 
| SendAsUserMailboxGuid | E-posta olarak gönderilecek erişildi posta kutusunun Exchange GUID. |
| SendAsUserSmtp | Kimliğine bürünülen kullanıcının SMTP adresi. |
| SendonBehalfOfUserMailboxGuid | Adına posta gönderme erişildi posta kutusunun Exchange GUID. |
| SendOnBehalfOfUserSmtp | SMTP adresi kullanıcının adına e-posta gönderilir. |


### <a name="exchange-mailbox-audit-group"></a>Exchange posta kutusu denetim grubu

Değişiklikler ve eklemeler Exchange gruplarına yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | Gruptaki her öğe hakkında bilgi. |
| CrossMailboxOperations | İşlem birden fazla posta kutusu dahil olmadığını gösterir. |
| DestMailboxId | Yalnızca CrossMailboxOperations parametre True ise ayarlayın. Hedef posta kutusu GUID belirtir. |
| DestMailboxOwnerMasterAccountSid | Yalnızca CrossMailboxOperations parametre True ise ayarlayın. Ana hedef posta kutusu sahibi SID'si hesap için SID belirtir. |
| DestMailboxOwnerSid | Yalnızca CrossMailboxOperations parametre True ise ayarlayın. Hedef posta kutusu SID'ini belirtir. |
| DestMailboxOwnerUPN | Yalnızca CrossMailboxOperations parametre True ise ayarlayın. Hedef posta kutusu sahibi UPN'sini belirtir. |
| DestFolder | Taşıma gibi işlemler için hedef klasör. |
| Klasör | Bir öğe grubunu bulunduğu klasör. |
| Klasörler |     Bir işlemde yer alan kaynak klasörleri hakkında bilgiler; Örneğin, klasörleri seçtiyseniz ve ardından silinir. |


### <a name="sharepoint-base"></a>SharePoint temel

Bu özellikler, tüm SharePoint kayıtlara yaygındır.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| EventSource | SharePoint'te bir olayın oluştuğunu belirtir. Olası değerler şunlardır: SharePoint veya ObjectModel. |
| Itemtype | Erişilen veya değiştirilen nesnenin türü. Ayrıntılar için Itemtype tabloya nesne türlerine bakın. |
| MachineDomainInfo | Cihaz eşitleme işlemleri hakkındaki bilgiler. Bu bilgiler, yalnızca istekteki ise bildirilir. |
| MachineId |   Cihaz eşitleme işlemleri hakkındaki bilgiler. Bu bilgiler, yalnızca istekteki ise bildirilir. |
| Site_ | Dosya veya klasör kullanıcı tarafından erişilen bulunduğu sitenin GUID. |
| Source_Name | Denetlenen işlemi tetikleyen varlık. Olası değerler şunlardır: SharePoint veya ObjectModel. |
| UserAgent | Kullanıcının istemci veya tarayıcı ilgili bilgiler. Bu bilgiler istemci veya tarayıcı tarafından sağlanır. |


### <a name="sharepoint-schema"></a>SharePoint şeması

SharePoint için yapılan yapılandırma değişiklikleri bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | Özel olaylar için isteğe bağlı dize. |
| Event_Data |  Özel olaylar için isteğe bağlı yükü. |
| ModifiedProperties | Özellik için bir site veya bir site koleksiyonu yöneticisi grubunun bir üyesi kullanıcı ekleme gibi yönetici olayları dahil edilir. Özelliği değiştirilmiş (site yöneticisi olarak eklendikten sonra bu kullanıcının) özelliğinin yeni değeri ve değiştirilmiş nesne önceki değerini (örneğin, Site Yönetici grubu), değiştirilen özelliğin adını içerir. |


### <a name="sharepoint-file-operations"></a>SharePoint dosya işlemleri

Bu kayıtlar, SharePoint'te dosya işlemleri için yanıt oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | Kopyalanamaz veya taşınamaz bir dosyanın dosya uzantısı. Bu özellik yalnızca FileCopied ve FileMoved olayları görüntülenir. |
| DestinationFileName öğesinin | Kopyalanan taşındığında veya dosyanın adı. Bu özellik yalnızca FileCopied ve FileMoved olayları görüntülenir. |
| DestinationRelativeUrl | Hedef klasöre bir dosya nerede kopyalanamaz veya taşınamaz URL'si. SiteURL DestinationRelativeURL ve destinationFileName öğesinin parametrelerin değerleri birleşimi kopyalanmıştır dosyanın tam yolu adı objectID özelliğinin değeri ile aynıdır. Bu özellik yalnızca FileCopied ve FileMoved olayları görüntülenir. |
| SharingType | Kaynak ile paylaşıldı kullanıcıya atanmış izinler paylaşım türü. Bu kullanıcı UserSharedWith parametresi tarafından tanımlanır. |
| Site_Url | Dosya veya klasör kullanıcı tarafından erişilen bulunduğu site URL'si. |
| SourceFileExtension | Kullanıcı tarafından erişilen dosyanın dosya uzantısı. Erişilmiş olan nesnedeki bir klasör ise, bu özellik boştur. |
| SourceFileName |  Dosya veya kullanıcı tarafından erişilen klasörün adı. |
| SourceRelativeUrl | Kullanıcı tarafından erişilen dosyayı içeren klasörü URL'si. SiteURL SourceRelativeURL ve SourceFileName parametrelerin değerleri birleşimi kullanıcı tarafından erişilen dosyanın tam yolunu unvanıdır objectID özelliğinin değeri ile aynıdır. |
| UserSharedWith |  Bir kaynak ile paylaşılan kullanıcı. |




## <a name="sample-log-searches"></a>Örnek günlük aramaları

Aşağıdaki tabloda, bu çözüm tarafından toplanan güncelleştirme kayıtlarına ilişkin örnek günlük aramaları sunulmaktadır.

| Sorgu | Açıklama |
| --- | --- |
|Office 365 aboneliğinizde tüm işlemlerin sayısı |OfficeActivity &#124; Count() işlevi işlemi tarafından özetleme |
|SharePoint siteleri kullanımı|OfficeActivity &#124; burada OfficeWorkload = ~ "sharepoint" &#124; count() by SiteUrl özetlemek \| sayısı asc göre sırala|
|Dosya erişimi işlemlerini kullanıcı türüne göre|Arama (OfficeActivity) OfficeWorkload = ~ "azureactivedirectory" ve "MyTest"|
|Belirli bir anahtar sözcükle arama yapın|Tür OfficeActivity OfficeWorkload = "MyTest" azureactivedirectory =|
|Exchange şirket dış eylemlerini izleme|OfficeActivity &#124; burada OfficeWorkload = ~ "exchange" ve ExternalAccess == true|



## <a name="next-steps"></a>Sonraki adımlar

* Kullanım [sorgular Azure İzleyici'de oturum](../log-query/log-query-overview.md) ayrıntılı güncelleştirme verilerini görüntülemek için.
* [Kendi panolarınızı oluşturun](../learn/tutorial-logs-dashboards.md) , sık kullanılan Office 365 arama sorgularını görüntülemek için.
* [Uyarı oluşturma](../platform/alerts-overview.md) önemli Office 365 etkinliklerini proaktif olarak gönderilecek.  