---
title: Azure'da Office 365 yönetim çözümü | Microsoft Docs
description: Bu makalede, yapılandırması ve Azure Office 365 çözümde kullanımı hakkında ayrıntılar sağlar.  Günlük analizi oluşturulan Office 365 kayıtları ayrıntılı açıklamasını içerir.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: bwren
ms.openlocfilehash: e93860328ed6a31b7a06cc3420fb50ab4af9a00c
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236112"
---
# <a name="office-365-management-solution-in-azure-preview"></a>Office 365 yönetim çözümü Azure (Önizleme)

![Office 365 logosu](media/oms-solution-office-365/icon.png)

Office 365 yönetim çözümü, Office 365 ortamınızda günlük analizi izlemenize olanak tanır.

- Office 365 hesaplarınızın yanı sıra kullanım desenlerini analiz davranış eğilimleri belirlemek için kullanıcı etkinliklerini izler. Örneğin, kuruluşunuzun veya en popüler SharePoint siteleri dışında paylaşılan dosyaları gibi belirli kullanım senaryoları ayıklayabilirsiniz.
- Yapılandırma değişikliklerini veya yüksek ayrıcalıklı işlemleri izlemek için yönetici etkinliklerini izler.
- Algılama ve kuruluş gereksinimlerinize göre özelleştirilmiş istenmeyen kullanıcı davranışı araştırın.
- Denetim ve uyumluluk gösterir. Örneğin, Denetim ve uyumluluk işlemiyle Yardım gizli dosyalar üzerinde dosya erişim işlemleri izleyebilirsiniz.
- Kullanarak işlemsel sorun giderme gerçekleştirir [oturum aramaları](../log-analytics/log-analytics-log-search.md) , kuruluşunuzun Office 365 etkinlik verileri üstünde.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki yüklenmiş ve yapılandırılmış bu çözümü olan önce gereklidir.

- Kuruluş Office 365 aboneliği.
- Genel yönetici olan bir kullanıcı hesabı için kimlik bilgileri.
- Denetim verileri almak için şunları yapmalısınız [denetimini yapılandırmak](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&rs=en-US&ad=US#PickTab=Before_you_begin) Office 365 aboneliğinizde.  Unutmayın [posta kutusu denetimi](https://technet.microsoft.com/library/dn879651.aspx) ayrı olarak yapılandırılır.  Hala çözümü yüklemek ve denetim yapılandırılmamışsa diğer verileri toplar.
 

## <a name="management-packs"></a>Yönetim paketleri
Bu çözüm içinde herhangi bir Yönetim Paketi yüklemez [bağlı Yönetim grupları](../log-analytics/log-analytics-om-agents.md).
  
## <a name="install-and-configure"></a>Yükleme ve yapılandırma
Başlangıç ekleyerek [Office 365 aboneliğiniz çözüme](/monitoring/monitoring-solutions.md#install-a-management-solution). Eklendikten sonra Office 365 aboneliğinize erişim vermek için bu bölümdeki yapılandırma adımlarını gerçekleştirmeniz gerekir.

### <a name="required-information"></a>Gerekli bilgiler
Bu yordama başlamadan önce aşağıdaki bilgileri toplayın.

Günlük analizi çalışma alanından:

- Çalışma alanı adı: Burada Office 365 veri toplanacak çalışma.
- Kaynak grubu adı: çalışma alanını içeren kaynak grubu.
- Azure abonelik kimliği: çalışma alanı içeriyor abonelik.

Office 365 aboneliğinizden:

- Kullanıcı adı: Bir yönetici hesabı e-posta adresi.
- Kiracı kimliği: Office 365 aboneliği için benzersiz kimlik.
- İstemci kimliği: Office 365 istemci temsil eden 16 karakter dizesi.
- İstemci gizliliği: Şifreli bir dize için kimlik doğrulaması gerekli.

### <a name="create-an-office-365-application-in-azure-active-directory"></a>Azure Active Directory'de bir Office 365 uygulama oluşturma
İlk adım, Azure Active Directory yönetim çözümü, Office 365 çözümünüzün erişmek için kullanacağı bir uygulaması oluşturmaktır.

1. [https://portal.azure.com](https://portal.azure.com/) adresinden Azure portalında oturum açın.
1. Seçin **Azure Active Directory** ve ardından **uygulama kayıtlar**.
1. **Yeni uygulama kaydı**’na tıklayın.

    ![Uygulama kayıt ekleme](media/oms-solution-office-365/add-app-registration.png)
1. Bir uygulama girin **adı** ve **oturum açma URL'si**.  Ad açıklayıcı olmalıdır.  Kullanım _http://localhost_ URL ve canlı _Web uygulaması / API_ için **uygulama türü**
    
    ![Uygulama oluştur](media/oms-solution-office-365/create-application.png)
1. Tıklatın **oluşturma** ve uygulama bilgilerini doğrulayın.

    ![Kayıtlı uygulama](media/oms-solution-office-365/registered-app.png)

### <a name="configure-application-for-office-365"></a>Office 365 için uygulamayı yapılandırma

1. Tıklatın **ayarları** açmak için **ayarları** menüsü.
1. Seçin **özellikleri**. Değişiklik **çoklu kiralanan** için _Evet_.

    ![Ayarları çok kullanıcılı](media/oms-solution-office-365/settings-multitenant.png)

1. Seçin **gerekli izinleri** içinde **ayarları** menüsünü seçin ve ardından **Ekle**.
1. Tıklatın **bir API seçin** ve ardından **Office 365 Yönetim API'leri**. tıklatın **Office 365 Yönetim API'leri**. **Seç**'e tıklayın.

    ![API seçin](media/oms-solution-office-365/select-api.png)

1. Altında **seçin izinleri** her ikisi için aşağıdaki seçenekleri belirleyin **uygulama izinleri** ve **izinleri atanmış**:
    - Kuruluşunuza ilişkin hizmet durumu bilgilerini okur
    - Kuruluşunuz için etkinlik verilerini okuyun
    - Kuruluşunuza ilişkin etkinlik raporlarını okur

    ![API seçin](media/oms-solution-office-365/select-permissions.png)

1. Tıklatın **seçin** ve ardından **Bitti**.
1. Tıklatın **izinleri verin** ve ardından **Evet** doğrulama için sorulduğunda.

    ![İzinleri verme](media/oms-solution-office-365/grant-permissions.png)

### <a name="add-a-key-for-the-application"></a>Uygulama için bir anahtar ekleyin

1. Seçin **anahtarları** içinde **ayarları** menüsü.
1. Yazın bir **açıklama** ve **süresi** yeni anahtar için.
1. Tıklatın **kaydetmek** ve ardından kopyalama **değeri** , oluşturulur.

    ![Anahtarlar](media/oms-solution-office-365/keys.png)

### <a name="add-admin-consent"></a>Yönetici onayı ekleme
İlk kez yönetici hesabını etkinleştirmek için uygulama için yönetici izni sağlamanız gerekir. Bir PowerShell Betiği ile bunu yapabilirsiniz. 

1. Aşağıdaki komut dosyası olarak kaydetmek *office365_consent.ps1*.

    ```
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,     
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId
    )
    
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
        
    IF ($Subscription -eq $null)
        {Login-AzureRmAccount -ErrorAction Stop}
    $Subscription = (Select-AzureRmSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $Workspace = (Set-AzureRMOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $WorkspaceLocation= $Workspace.Location
    $WorkspaceLocationShort= $Workspace.PortalUrl.Split("/",$option)[1].Split(".",$option)[0]
    $WorkspaceLocation
    
    Function AdminConsent{
    
    $domain='login.microsoftonline.com'
    $mmsDomain = 'login.mms.microsoft.com'
    $WorkspaceLocationShort= $Workspace.PortalUrl.Split("/",$option)[1].Split(".",$option)[0]
    $WorkspaceLocationShort
    $WorkspaceLocation
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
                             $domain='login.microsoftonline.us';
                             $mmsDomain = 'usbn1.login.oms.microsoft.us'; break} # US Gov Virginia
           default {$OfficeAppClientId="55b65fb5-b825-43b5-8972-c8b6875867c1";
                    $domain='login.windows-ppe.net'; break} #Int
        }
    
        $OfficeAppRedirectUrl="https://$($WorkspaceLocationShort).$($mmsDomain)/Office365/Authorize"
        $OfficeAppRedirectUrl
        $domain
        Start-Process -FilePath  "https://$($domain)/common/adminconsent?client_id=$($OfficeAppClientId)&state=12345&redirect_uri=$($OfficeAppRedirectUrl)"
    }
    
    AdminConsent -ErrorAction Stop
    ```

2. Komut dosyası ile aşağıdaki komutu çalıştırın.
    ```
    .\office365_consent.ps1 -WorkspaceName <Workspace name> -ResourceGroupName <Resource group name> -SubscriptionId <Subscription ID>
    ```
    Örnek:

    ```
    .\office365_consent.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631- yyyyyyyyyyyy'
    ```

1. Birine benzer bir pencere ile sunulur. Tıklatın **kabul**.
    
    ![Yönetici onayı](media/oms-solution-office-365/admin-consent.png)

### <a name="subscribe-to-log-analytics-workspace"></a>Günlük analizi çalışma alanına abone olma
Günlük analizi çalışma alanınız uygulamaya abone olmak için son adım olacaktır. Ayrıca bir PowerShell Betiği ile bunu.

1. Aşağıdaki komut dosyası olarak kaydetmek *office365_subscription.ps1*.

    ```
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
        {Login-AzureRmAccount -ErrorAction Stop}
    $Subscription = (Select-AzureRmSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    $Workspace = (Set-AzureRMOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
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
    
    # Load ADAL Azure AD Authentication Library Assemblies
    $adal = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
    $adalforms = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll"
    $null = [System.Reflection.Assembly]::LoadFrom($adal)
    $null = [System.Reflection.Assembly]::LoadFrom($adalforms)
     
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
                                    'contentTypes':'Audit.Exchange,Audit.AzureActiveDirectory'
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

2. Komut dosyası ile aşağıdaki komutu çalıştırın:
    ```
    .\office365_subscription.ps1 -WorkspaceName <Log Analytics workspace name> -ResourceGroupName <Resource Group name> -SubscriptionId <Subscription ID> -OfficeUsername <OfficeUsername> -OfficeTennantID <Tenant ID> -OfficeClientId <Client ID> -OfficeClientSecret <Client secret>
    ```
    Örnek:

    ```
    .\office365_subscription.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631-yyyyyyyyyyyy' -OfficeUsername 'admin@contoso.com' -OfficeTennantID 'ce4464f8-a172-4dcf-b675-xxxxxxxxxxxx' -OfficeClientId 'f8f14c50-5438-4c51-8956-zzzzzzzzzzzz' -OfficeClientSecret 'y5Lrwthu6n5QgLOWlqhvKqtVUZXX0exrA2KRHmtHgQb='
    ```

### <a name="troublshooting"></a>Troublshooting

Abonelik zaten var. sonra bir abonelik oluşturmayı denerseniz, aşağıdaki hatayı görebilirsiniz.

```
Invoke-WebRequest : {"Message":"An error has occurred."}
At C:\Users\v-tanmah\Desktop\ps scripts\office365_subscription.ps1:161 char:19
+ $officeresponse = Invoke-WebRequest @Officeparams
+                   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebException
    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand 
```

Geçersiz parametre değerlerini sağladıysanız aşağıdaki hatayı görebilirsiniz.

```
Select-AzureRmSubscription : Please provide a valid tenant or a valid subscription.
At line:12 char:18
+ ... cription = (Select-AzureRmSubscription -SubscriptionId $($Subscriptio ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureRmContext], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.SetAzureRMContextCommand

```

## <a name="uninstall"></a>Kaldırma
İşlemi kullanarak Office 365 yönetim çözümü kaldırabilirsiniz [bir yönetim çözümü kaldırmak](../monitoring/monitoring-solutions.md#remove-a-management-solution). Bu, Office 365'ten günlük analizi ancak toplanmakta olan veri durdurmaz. Office 365'ten aboneliği ve veri toplamayı durdurmak için aşağıdaki yordamı izleyin.

1. Aşağıdaki komut dosyası olarak kaydetmek *office365_unsubscribe.ps1*.

    ```
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId,
        [Parameter(Mandatory=$True)][string]$OfficeTennantId
    )
    $line='#-------------------------------------------------------------------------------------------------------------------------------------------------------------------------'
    
    $line
    IF ($Subscription -eq $null)
        {Login-AzureRmAccount -ErrorAction Stop}
    $Subscription = (Select-AzureRmSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    $Workspace = (Set-AzureRMOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
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
    
    # Load ADAL Azure AD Authentication Library Assemblies
    $adal = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
    $adalforms = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll"
    $null = [System.Reflection.Assembly]::LoadFrom($adal)
    $null = [System.Reflection.Assembly]::LoadFrom($adalforms)
     
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

2. Komut dosyası ile aşağıdaki komutu çalıştırın:

    ```
    .\office365_unsubscribe.ps1 -WorkspaceName <Log Analytics workspace name> -ResourceGroupName <Resource Group name> -SubscriptionId <Subscription ID> -OfficeTennantID <Tenant ID> 
    ```

    Örnek:

    ```
    .\office365_unsubscribe.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631-yyyyyyyyyyyy' -OfficeTennantID 'ce4464f8-a172-4dcf-b675-xxxxxxxxxxxx'
    ```

## <a name="data-collection"></a>Veri toplama
### <a name="supported-agents"></a>Desteklenen aracılar
Office 365 çözümü veri birinden almıyorsa [OMS Aracısı](../log-analytics/log-analytics-data-sources.md).  Office 365'ten doğrudan verileri alır.

### <a name="collection-frequency"></a>Toplama sıklığı
Başlangıçta Toplanacak veriler için birkaç saat sürebilir. Toplama başladıktan sonra Office 365 gönderir bir [Web kancası bildirim](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) ayrıntılı verilerle günlük analizi için her zaman bir kayıt oluşturulur. Bu kayıt alınma sonra birkaç dakika içinde günlük analizi içinde kullanılabilir.

## <a name="using-the-solution"></a>Çözümü kullanma
Office 365 çözümü, günlük analizi çalışma alanına eklediğinizde **Office 365** döşeme panonuza eklenir. Bu kutucukta, ortamınızdaki bilgisayarların sayısına ve güncelleştirme uyumluluğuna ilişkin bir sayı ve grafik gösterimi görüntülenir.<br><br>
![Office 365 Özet kutucuğu](media/oms-solution-office-365/tile.png)  

Tıklayın **Office 365** açmak için kutucuğa **Office 365** Pano.

![Office 365 Panosu](media/oms-solution-office-365/dashboard.png)  

Pano aşağıdaki tabloda gösterilen sütunları içerir. Her bir sütunun üst on uyarıları Belirtilen kapsam ve zaman aralığı için bu sütunun ölçütlerle eşleşen sayısına göre listeler. Bkz: tüm sütun altındaki tıklatarak veya sütun başlığını tıklatarak listenin tamamını sağlayan bir günlük arama çalıştırabilirsiniz.

| Sütun | Açıklama |
|:--|:--|
| İşlemler | İzlenen tüm Office 365 abonelik etkin kullanıcılar hakkında bilgi sağlar. Zaman içinde gerçekleşen etkinliklerin sayısını görmeye olacaktır.
| Exchange | Posta Kutusu Ekle izni veya Set-Mailbox gibi Exchange Server etkinlikleri dökümünü gösterir. |
| SharePoint | Kullanıcılar SharePoint belgelerindeki gerçekleştirmek üst etkinlikleri gösterir. Bu kutucuğunda detaya, arama sayfası hedef belge ve bu etkinliği konumunu gibi bu etkinlikler ayrıntılarını gösterir. Örneğin, dosyaya erişim bir olay için erişiliyor, belge görmeye siz olacaksınız ilişkili hesap adı ve IP adresi. |
| Azure Active Directory | Kullanıcı parola sıfırlama ve oturum açma denemesi gibi en çok kullanan kullanıcı etkinlikleri içerir. Ayrıntıya olduğunda, bu etkinlikler sonuç durumu gibi ayrıntılarını görmeye olacaktır. Bu genellikle, Azure Active Directory kuşkulu etkinlikleri izlemek istiyorsanız yararlıdır. |




## <a name="log-analytics-records"></a>Log Analytics kayıtları

Günlük analizi çalışma alanında Office 365 çözümü tarafından oluşturulan tüm kayıtlarına sahip bir **türü** , **OfficeActivity**.  **OfficeWorkload** özelliği, Exchange, AzureActiveDirectory, SharePoint veya OneDrive için - kayıt başvuruyor hangi Office 365 hizmet belirler.  **RecordType** özelliği işlemi türünü belirtir.  Özellikler her işlem türü için değişir ve aşağıdaki tablolarda gösterilmiştir.

### <a name="common-properties"></a>Ortak Özellikler
Aşağıdaki özellikleri için tüm Office 365 kayıtları yaygındır.

| Özellik | Açıklama |
|:--- |:--- |
| Tür | *OfficeActivity* |
| ClientIP | Etkinlik oturum açıldığında kullanılan aygıtın IP adresi. IP adresi, bir IPv4 veya IPv6 adresi biçiminde görüntülenir. |
| OfficeWorkload | Kayıt başvurduğu office 365 hizmeti.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| İşlem | Kullanıcı veya yönetici etkinliği adı.  |
| OrganizationId | Kuruluşunuzun Office 365 Kiracı için GUID. Bu değer her zaman içinde oluştuğu Office 365 hizmeti bağımsız olarak, kuruluşunuz için aynı olacaktır. |
| RecordType | Gerçekleştirilen işlem türü. |
| ResultStatus | (İşlemi özelliğinde belirtilen) eylem başarılı olup olmadığını gösterir. Olası değerler, başarılı, PartiallySucceded veya başarısız olur. Exchange yönetici etkinliği için değer her iki true'dur ya da yanlış. |
| UserId | (Kullanıcı asıl adı) günlüğe kaydedilmesini kaydında sonuçlandı eylemi gerçekleştiren kullanıcının UPN'sini; Örneğin, my_name@my_domain_name. Sistem hesapları (örneğin, SHAREPOINT\system veya ntauthority\system adlı) tarafından gerçekleştirilen etkinlik kayıtlarını da dahil olduğunu unutmayın. | 
| UserKey | UserId özelliğinde belirtilen kullanıcı için alternatif bir kimliği.  Örneğin, bu özellik, iş ve Exchange için SharePoint, OneDrive bulunan kullanıcılar tarafından gerçekleştirilen olaylar için passport benzersiz kimliği (PUID) ile doldurulur. Bu özellik, diğer hizmetleri ve sistem hesapları tarafından gerçekleştirilen etkinlikleri içinde gerçekleşen olayların UserId özelliği olarak aynı değeri de belirtebilir|
| UserType | İşlem gerçekleştirilirken kullanıcı türü.<br><br>Yönetim Bölgesi<br>Uygulama<br>DcAdmin<br>Normal<br>Ayrılmış<br>ServicePrincipal<br>Sistem |


### <a name="azure-active-directory-base"></a>Azure Active Directory temel
Aşağıdaki özellikleri için tüm Azure Active Directory kayıtları yaygındır.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | Azure AD olay türü. |
| extendedProperties | Azure AD olay genişletilmiş özellikler. |


### <a name="azure-active-directory-account-logon"></a>Azure Active Directory hesabı oturum açma
Bir Active Directory kullanıcı oturum açma girişiminde Bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectoryAccountLogon |
| Uygulama | Office 15 gibi hesap oturum açma olayı tetikler uygulama. |
| İstemci | İstemci ayrıntılarını cihaz, cihaz işletim sistemine ve için kullanılan aygıt tarayıcı hesap oturum açma olay. |
| loginStatus | Bu özellik, doğrudan OrgIdLogon.LoginStatus olur. Çeşitli ilginç oturum açma hataları eşleme algoritmaları uyarı tarafından yapılabilir. |
| UserDomain | Kiracı kimlik bilgileri (TII). | 


### <a name="azure-active-directory"></a>Azure Active Directory
Azure Active Directory nesneleri değiştirme veya ekleme yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AADTarget | (İşlemi özelliği tarafından tanımlanan) eylem gerçekleştirilip gerçekleştirilmediğine kullanıcı. |
| Aktör | Kullanıcı veya gerçekleştirilen eylem hizmet sorumlusu. |
| ActorContextId | Aktör ait kuruluş GUID. |
| ActorIpAddress | Aktör'ın IP adresi IPv4 veya IPv6 adresi biçiminde. |
| InterSystemsId | Office 365 hizmet içinde bileşenleri arasında eylemleri izlemek GUID. |
| IntraSystemId |   Azure eylemi izlemek için Active Directory tarafından üretilen GUID. |
| SupportTicketId | Müşteri, "act-üzerinde-adına-of" durumlarda eylemi için bilet kimliği destekler. |
| TargetContextId | Hedeflenen kullanıcının ait olduğu kuruluş GUID. |


### <a name="data-center-security"></a>Veri Merkezi güvenlik
Bu kayıtları veri merkezi güvenlik denetim verilerden oluşturulur.  

| Özellik | Açıklama |
|:--- |:--- |
| EffectiveOrganization | Yükseltme/cmdlet hedeflenmiş Kiracı adı. |
| ElevationApprovedTime | Yükseltme onaylandığı zaman damgası. |
| ElevationApprover | Bir Microsoft yöneticisinin adı. |
| ElevationDuration | Yükseltme etkin olduğu süre. |
| ElevationRequestId |  Yükseltme isteği için benzersiz bir tanımlayıcı. |
| ElevationRole | Yükseltme rol için istendi. |
| ElevationTime | Yükseltme başlangıç saati. |
| Start_Time | Cmdlet yürütmesini başlangıç saati. |


### <a name="exchange-admin"></a>Exchange yönetici
Exchange yapılandırması için değişiklik yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeAdmin |
| ExternalAccess |  Cmdlet, kuruluşunuz Microsoft datacenter personeli tarafından veya bir veri merkezi hizmet hesabı bir kullanıcı tarafından veya yönetici temsilcisi tarafından çalıştırılıp çalıştırılmadığını belirtir. Cmdlet, kuruluşunuzdaki bir kişi tarafından çalıştırıldığı False değerini gösterir. Cmdlet datacenter personel, bir veri merkezi hizmet hesabı veya yönetici temsilcisi tarafından çalıştırıldığı True değerini gösterir. |
| ModifiedObjectResolvedName |  Bu cmdlet tarafından değiştirilen nesne kullanıcı dostu adıdır. Bu cmdlet nesne değiştiriyorsa günlüğe kaydedilir. |
| Kuruluş adı | Kiracı adı. |
| OriginatingServer | Cmdlet yürütülen sunucunun adıdır. |
| Parametreler | Ad ve işlemleri özelliğinde tanımlanan cmdlet ile kullanılan tüm parametreler için değer. |


### <a name="exchange-mailbox"></a>Exchange posta kutusu
Exchange posta kutularına değişiklikler ve eklemeler yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| ClientInfoString | Bir tarayıcı sürümü, Outlook sürümü ve mobil aygıt bilgileri gibi işlemi gerçekleştirmek için kullanılan e-posta istemcisi hakkında bilgi. |
| Client_IPAddress | İşlemi oturum açıldığında kullanılan aygıtın IP adresi. IP adresi, bir IPv4 veya IPv6 adresi biçiminde görüntülenir. |
| ClientMachineName | Outlook istemcisi barındıran makine adı. |
| ClientProcessName | Posta kutularına erişmek için kullanılan e-posta istemcisi. |
| ClientVersion | E-posta istemcisi sürümü. |
| InternalLogonType | İç kullanım için ayrılmıştır. |
| Logon_Type | Posta kutusu erişilen ve günlüğe işlemi gerçekleştiren kullanıcı türünü belirtir. |
| LogonUserDisplayName |    İşlemi gerçekleştiren kullanıcı kolay adı. |
| LogonUserSid | İşlemi gerçekleştiren kullanıcının SID'si. |
| MailboxGuid | Erişilen posta kutusunun Exchange GUID. |
| MailboxOwnerMasterAccountSid | Posta kutusu sahibi hesabının yönetici hesabı SID'si. |
| MailboxOwnerSid | Posta kutusu sahibi SID'si. |
| MailboxOwnerUPN | Erişilen posta kutusu sahibi olan kişinin e-posta adresi. |


### <a name="exchange-mailbox-audit"></a>Exchange posta kutusu denetimi
Bir posta kutusu denetim girdisi oluşturulduğunda bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| Öğe | Bağlı işlem gerçekleştirildi öğeyi temsil eder | 
| SendAsUserMailboxGuid | E-posta olarak gönderilecek erişilmiş olan posta kutusunun Exchange GUID. |
| SendAsUserSmtp | Kimliğine bürünülen kullanıcı SMTP adresi. |
| SendonBehalfOfUserMailboxGuid | Adına posta göndermek için erişilen posta kutusunun Exchange GUID. |
| SendOnBehalfOfUserSmtp | SMTP adresi kullanıcının adına e-posta gönderilir. |


### <a name="exchange-mailbox-audit-group"></a>Exchange posta kutusu denetim grubu
Değişiklikler ve eklemeler Exchange gruplarına yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | Gruptaki her öğe hakkında bilgiler. |
| CrossMailboxOperations | İşlem birden fazla posta kutusu dahil olmadığını gösterir. |
| DestMailboxId | Yalnızca CrossMailboxOperations parametresi True ise ayarlayın. Hedef posta kutusu GUID belirtir. |
| DestMailboxOwnerMasterAccountSid | Yalnızca CrossMailboxOperations parametresi True ise ayarlayın. Hedef posta kutusu sahibi SID'si yönetici hesabı için SID belirtir. |
| DestMailboxOwnerSid | Yalnızca CrossMailboxOperations parametresi True ise ayarlayın. Hedef posta kutusu SID'si belirtir. |
| DestMailboxOwnerUPN | Yalnızca CrossMailboxOperations parametresi True ise ayarlayın. Hedef posta kutusu sahibinin UPN belirtir. |
| DestFolder | Taşıma gibi işlemleri için hedef klasör. |
| Klasör | Bir öğe grubunu bulunduğu klasör. |
| Klasörler |     Bir işlemde yer alan kaynak klasörleri hakkında bilgi; Örneğin, klasörleri seçtiyseniz ve ardından silinir. |


### <a name="sharepoint-base"></a>SharePoint tabanı
Bu özellikler, tüm SharePoint kayıtları yaygındır.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| EventSource | Bir olay SharePoint'te oluştuğunu tanımlar. Olası değerler, SharePoint veya ObjectModel olur. |
| itemType | Erişilen veya değiştirilmiş nesnenin türü. Ayrıntılar için ItemType tabloya nesne türlerini bakın. |
| MachineDomainInfo | Aygıt eşitleme işlemleri hakkında bilgi. Yalnızca istek varsa, bu bilgileri bildirilir. |
| ResourceId |   Aygıt eşitleme işlemleri hakkında bilgi. Yalnızca istek varsa, bu bilgileri bildirilir. |
| Site_ | Dosya veya klasör kullanıcı tarafından erişilen bulunduğu site GUID. |
| Source_Name | Denetlenen işlemi tetiklenen varlık. Olası değerler, SharePoint veya ObjectModel olur. |
| UserAgent | Kullanıcının istemci veya tarayıcı ilgili bilgiler. Bu bilgiler istemci veya tarayıcı tarafından sağlanır. |


### <a name="sharepoint-schema"></a>SharePoint şeması
SharePoint için yapılandırma değişiklik yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | Özel olaylar için isteğe bağlı dize. |
| Event_Data |  Özel olaylar için isteğe bağlı yükü. |
| ModifiedProperties | Özelliği bir site veya bir site koleksiyonu yönetici grubunun bir üyesi olarak kullanıcı ekleme gibi yönetim olaylar için dahil edilmiştir. Özelliği (örneğin, Site Yönetici grubu), (bir site yöneticisi olarak eklenmiş gibi kullanıcının) değiştirilmiş özelliğin yeni değeri ve değiştirilmiş nesne önceki değeri değiştirildiği özelliğinin adını içerir. |


### <a name="sharepoint-file-operations"></a>SharePoint dosya işlemleri
Bu kayıtları yanıt SharePoint'teki dosya işlemleri olarak oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | Kopyalanan veya taşınan bir dosyanın dosya uzantısını. Bu özellik yalnızca FileCopied ve FileMoved olaylar için görüntülenir. |
| DestinationFileName öğesinin | Kopyalanan veya taşınan dosyasının adı. Bu özellik yalnızca FileCopied ve FileMoved olaylar için görüntülenir. |
| DestinationRelativeUrl | Burada bir dosya kopyalanan taşınmış veya hedef klasör URL'si. SiteURL, DestinationRelativeURL ve destinationFileName öğesinin parametreleri ait değerlerin birleşimi kopyalanmıştır dosyasının tam yolu adı olduğundan objectID özelliğinin değeri ile aynıdır. Bu özellik yalnızca FileCopied ve FileMoved olaylar için görüntülenir. |
| SharingType | Paylaşım kaynağı ile paylaşıldı kullanıcıya atanmış izinleri türü. Bu kullanıcı UserSharedWith parametresi tarafından tanımlanır. |
| Site_Url | Dosya veya klasör kullanıcı tarafından erişilen bulunduğu site URL'si. |
| SourceFileExtension | Kullanıcı tarafından erişilen dosyanın dosya uzantısı. Bu özellik, erişilen nesne bir klasör ise boştur. |
| SourceFileName |  Dosya veya kullanıcı tarafından erişilen klasörün adı. |
| SourceRelativeUrl | Kullanıcı tarafından erişilen dosyanın bulunduğu klasörü URL'si. Birleşimi SiteURL, SourceRelativeURL ve SourceFileName parametreleri için değerleri, kullanıcı tarafından erişilen dosyanın tam yolunu adı objectID özelliğinin değeri ile aynıdır. |
| UserSharedWith |  Bir kaynak ile paylaşıldı kullanıcı. |




## <a name="sample-log-searches"></a>Örnek günlük aramaları
Aşağıdaki tabloda, bu çözüm tarafından toplanan güncelleştirme kayıtlarına ilişkin örnek günlük aramaları sunulmaktadır.

| Sorgu | Açıklama |
| --- | --- |
|Office 365 aboneliğinizin tüm işlemlerinin sayısı |OfficeActivity &#124; işlemi tarafından count() özetler |
|SharePoint siteleri kullanımı|OfficeActivity &#124; burada OfficeWorkload = ~ "sharepoint" &#124; SiteUrl tarafından count() özetler | Count asc göre sıralama|
|Kullanıcı türüne göre dosya erişim işlemleri|(OfficeActivity) OfficeWorkload aramada = ~ "azureactivedirectory" ve "MyTest"|
|Belirli bir anahtar sözcük ile arama|Tür OfficeActivity OfficeWorkload = azureactivedirectory "MyTest" =|
|Exchange dış eylemlerini izleme|OfficeActivity &#124; burada OfficeWorkload = ~ "exchange" ve ExternalAccess gerekmektedir|



## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılı güncelleştirme verilerini görüntülemek için [Log Analytics](../log-analytics/log-analytics-log-searches.md)’te Günlük Aramalarını kullanın.
* [Kendi panolar oluşturun](../log-analytics/log-analytics-dashboards.md) , sık kullanılan Office 365 arama sorgularını görüntülemek için.
* [Uyarı oluşturma](../log-analytics/log-analytics-alerts.md) önemli Office 365 etkinliklerini proaktif olarak bildirilmesi.  
