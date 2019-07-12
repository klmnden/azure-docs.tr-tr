---
title: Azure portalında Eylem grupları oluşturma ve yönetme
description: Azure portalında Eylem grupları oluşturma ve yönetme hakkında bilgi edinin.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 7/08/2019
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 842965aa49ae4cd546fe9c107107d2a2ceebebbb
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705248"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Azure portalında Eylem grupları oluşturma ve yönetme
Bir eylem grubu, Azure aboneliğinin sahibi tarafından tanımlanan bildirim tercihleri koleksiyonudur. Azure İzleyici ve hizmet Sistem Durumu Uyarıları Eylem grupları uyarı tetiklendi kullanıcılara bildirmek için kullanın. Çeşitli uyarılar aynı eylem grubu veya kullanıcının gereksinimlerine bağlı olarak farklı eylem grupları kullanabilir. Bir abonelikte en fazla 2.000 Eylem grupları yapılandırabilirsiniz.

Bir kişinin e-posta veya SMS, eylem grubuna eklenmiş olan belirten bir onay aldıkları bildirmek için bir eylem yapılandırırsınız.

Bu makalede, Azure portalında Eylem grupları oluşturma ve yönetme işlemini göstermektedir.

Her eylem aşağıdaki özelliklerinden oluşur:

* **Ad**: Eylem grubu içinde benzersiz bir tanımlayıcı.  
* **Eylem türü**: Gerçekleştirilen eylem. Bir ses araması, SMS, e-posta gönderme verilebilir; veya otomatik eylemler çeşitli türlerde tetikleniyor. Bu makalenin devamındaki türleri bakın.
* **Ayrıntılar**: Göre farklılık gösteren ilgili ayrıntıları *eylem türü*.

Eylem grupları yapılandırmak için Azure Resource Manager şablonlarını kullanma hakkında daha fazla bilgi için bkz: [eylem grubu Resource Manager şablonları](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak bir eylem grubu oluşturma

1. İçinde [Azure portalında](https://portal.azure.com)seçin **İzleyici**. **İzleyici** bölmesinde, tüm izleme ayarlarınızı ve tek bir görünümde verileri birleştirir.

    !["İzleme" hizmeti](./media/action-groups/home-monitor.png)
    
1. Seçin **uyarılar** seçip **işlemleri yönetmenizi**.

    ![Düğme eylemleri yönetme](./media/action-groups/manage-action-groups.png)
    
1. Seçin **eylem grubu Ekle**ve alanları doldurun.

    !["Eylem Grup Ekle" komutu](./media/action-groups/add-action-group.png)
    
1. Bir ad girin **eylem grubu adı** kutu ve bir ad girin **kısa ad** kutusu. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

      ![Eylem grubu Ekle"iletişim kutusu](./media/action-groups/action-group-define.png)

1. **Abonelik** kutusunda autofills geçerli aboneliğiniz ile. Eylem grubu kaydedildiği bir aboneliktir.

1. Seçin **kaynak grubu** eylem grubu kaydedildiği içinde.

1. Eylemlerin bir listesini tanımlar. Her eylem için aşağıdakileri sağlar:

    1. **Ad**: Bu eylem için benzersiz bir tanımlayıcı girin.

    1. **Eylem türü**: E-posta/SMS/anında iletme/ses, mantıksal uygulama, Web kancası, ITSM veya Otomasyon Runbook'u seçin.

    1. **Ayrıntılar**: Eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi, Web kancası URI'si, Azure uygulaması, ITSM bağlantısı veya Otomasyon runbook'u girin. ITSM eylemleri için ayrıca belirtin **iş öğesi** ve ITSM aracınız için gereken diğer alanları.
    
    1. **Ortak uyarı şeması**: Etkinleştirmeyi seçebilirsiniz [ortak uyarı şeması](https://aka.ms/commonAlertSchemaDocs), Genişletilebilir tek bir avantajı sağlar ve birleşik uyarı yük boyunca tüm uyarı Hizmetleri Azure İzleyici'de.

1. Seçin **Tamam** eylem grubunu oluşturmak için.

## <a name="manage-your-action-groups"></a>Eylem grupları yönetme

Bir eylem grubu oluşturduktan sonra görünür **Eylem grupları** bölümünü **İzleyici** bölmesi. Yönetmek istediğiniz eylem grubu seçin:

* Ekleme, düzenleme veya eylemleri kaldırın.
* Eylem grubunu silin.

## <a name="action-specific-information"></a>Özel eylem bilgileri

> [!NOTE]
> Bkz: [izleme için abonelik hizmeti limitleri](https://docs.microsoft.com/azure/azure-subscription-service-limits#azure-monitor-limits) sayısal sınırlar aşağıdaki öğelerin her biri için.  

### <a name="azure-app-push-notifications"></a>Azure uygulaması anında iletme bildirimleri
Bir eylem grubu içinde sınırlı sayıda Azure uygulaması eylemler olabilir.

### <a name="email"></a>Email
Aşağıdaki e-posta adreslerinden e-postalar gönderilir. E-posta filtreleme uygun şekilde yapılandırıldığından emin olun
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

E-posta eylemleri sınırlı sayıda bir eylem grubu içinde olabilir. Bkz: [bilgileri sınırlama oranı](./../../azure-monitor/platform/alerts-rate-limiting.md) makalesi.

### <a name="itsm"></a>ITSM
ITSM eylemi bir ITSM bağlantısı gerektirir. Oluşturmayı bir [ITSM bağlantısı](../../azure-monitor/platform/itsmc-overview.md).

ITSM eylemleri sınırlı sayıda bir eylem grubu içinde olabilir. 

### <a name="logic-app"></a>Logic App
Mantıksal uygulama eylemleri sınırlı sayıda bir eylem grubu içinde olabilir.

### <a name="function"></a>İşlev
İşlev tuşlarını işlevi eylemleri yapılandırılan uygulamalar için şu anda v2 işlev uygulamalarını'uygulama "files" için "AzureWebJobsSecretStorageType" ayarı yapılandırmak için gerektiren işlevleri API aracılığıyla okuyun. Daha fazla bilgi için [işlevler V2'de anahtar yönetimi değişiklikleri]( https://aka.ms/funcsecrets).

Sınırlı sayıda işlevi eylemleri bir eylem grubu içinde olabilir.

### <a name="automation-runbook"></a>Otomasyon Runbook'u
Başvurmak [Azure abonelik hizmeti limitleri](../../azure-subscription-service-limits.md) sınırları üzerinde Runbook yükler.

Runbook eylemleri sınırlı sayıda bir eylem grubu içinde olabilir. 

### <a name="sms"></a>SMS
Bkz: [bilgileri sınırlama oranı](./../../azure-monitor/platform/alerts-rate-limiting.md) ve [SMS uyarısı davranışı](../../azure-monitor/platform/alerts-sms-behavior.md) diğer önemli bilgiler için.

Bir eylem grubu içinde sınırlı sayıda SMS eylemler olabilir.  

### <a name="voice"></a>Ses
Bkz: [bilgileri sınırlama oranı](./../../azure-monitor/platform/alerts-rate-limiting.md) makalesi.

Bir eylem grubu içinde sınırlı sayıda ses eylemler olabilir.

### <a name="webhook"></a>Web Kancası
Web kancaları, aşağıdaki kurallar kullanılarak yeniden denenir. Web kancası çağrısı denenir, 2 katı şu HTTP durum kodları, döndürülen en fazla: 408, 429, 503, 504 veya HTTP uç noktası yanıt vermez. İlk yeniden deneme 10 saniye sonra yapılır. İkinci yeniden 100 saniye sonra gerçekleşir. İki hatasından sonra herhangi bir eylem grubu uç noktası 30 dakikalığına çağırır. 

Kaynak IP adresi aralıkları
 - 13.72.19.232
 - 13.106.57.181
 - 13.106.54.3
 - 13.106.54.19
 - 13.106.38.142
 - 13.106.38.148
 - 13.106.57.196
 - 52.244.68.117
 - 52.244.65.137
 - 52.183.31.0
 - 52.184.145.166
 - 51.4.138.199
 - 51.5.148.86
 - 51.5.149.19

Bu IP adresleri değişiklikler hakkındaki güncelleştirmeleri almak için Eylem grupları hizmeti hakkında bilgi veren bildirimleri için izleyen bir hizmet durumu uyarısı yapılandırma öneririz.

Web kancası eylemleri sınırlı sayıda bir eylem grubu içinde olabilir.

#### <a name="secure-webhook"></a>Güvenli Web kancası
**Güvenli Web kancası işlevi şu anda Önizleme aşamasındadır.**

Eylem grupları Web kancası eylemi, eylem grubu ve korumalı web API (Web kancası uç noktası) arasındaki bağlantıyı güvenli hale getirmek için Azure Active Directory yararlanmanızı sağlar. Bu işlev yararlanarak için genel iş akışı aşağıda açıklanmaktadır. Azure AD uygulama ve hizmet sorumluları genel bakış için bkz: [Microsoft kimlik Platformu (v2.0) genel bakış](https://docs.microsoft.com/azure/active-directory/develop/v2-overview).

1. Korumalı web API'niz için bir Azure AD uygulaması oluşturun. Bkz. https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview.
    - Korumalı API'NİZİN arka plan programı uygulama tarafından çağrılacak yapılandırın.
    
1. Eylem grupları, Azure AD uygulamanızı kullanmak etkinleştirin.

    > [!NOTE]
    > Bir üyesi olmanız gerekir [Azure AD uygulama yöneticisi rolü](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#available-roles) bu betiği yürütülemedi.
    
    - Azure AD Kiracı kimliğinizi kullanmak için PowerShell betik Connect-AzureAD çağrı değiştirme
    - Nesne Kimliğini Azure AD uygulamanızı kullanmak için PowerShell komut dosyanızın değişken $myAzureADApplicationObjectId değiştirme
    - Değiştirilmiş betiği çalıştırın.
    
1. Eylem grubu Web kancası eylemi yapılandırın.
    - Değer $myApp.ObjectId komut dosyasından kopyalayın ve Web kancası eylemi tanımı uygulama nesnesi Kimliği alanına girin.
    
    ![Güvenli Web kancası eylemi](./media/action-groups/action-groups-secure-webhook.png)

##### <a name="secure-webhook-powershell-script"></a>Web kancası PowerShell Betiği güvenliğini sağlama

```PowerShell
Connect-AzureAD -TenantId "<provide your Azure AD tenant ID here>"
    
# This is your Azure AD Application's ObjectId. 
$myAzureADApplicationObjectId = "<the Object Id of your Azure AD Application>"
    
# This is the Action Groups Azure AD AppId
$actionGroupsAppId = "461e8683-5575-4561-ac7f-899cc907d62a"
    
# This is the name of the new role we will add to your Azure AD Application
$actionGroupRoleName = "ActionGroupsSecureWebhook"
    
# Create an application role of given name and description
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}
    
# Get my Azure AD Application, it's roles and service principal
$myApp = Get-AzureADApplication -ObjectId $myAzureADApplicationObjectId
$myAppRoles = $myApp.AppRoles
$actionGroupsSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $actionGroupsAppId + "'")

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
    
# Create the role if it doesn't exist
if ($myAppRoles -match "ActionGroupsSecureWebhook")
{
    Write-Host "The Action Groups role is already defined.`n"
}
else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Add our new role to the Azure AD Application
    $newRole = CreateAppRole -Name $actionGroupRoleName -Description "This is a role for Action Groups to join"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}
    
# Create the service principal if it doesn't exist
if ($actionGroupsSP -match "AzNS AAD Webhook")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the Action Groups Azure AD Application and add it to the role
    $actionGroupsSP = New-AzureADServicePrincipal -AppId $actionGroupsAppId
}
    
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $actionGroupsSP.ObjectId -PrincipalId $actionGroupsSP.ObjectId
    
Write-Host "My Azure AD Application ($myApp.ObjectId): " + $myApp.ObjectId
Write-Host "My Azure AD Application's Roles"
Write-Host $myApp.AppRoles
```


## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [SMS uyarısı davranışı](../../azure-monitor/platform/alerts-sms-behavior.md).  
* Geçirmesine bir [etkinlik günlüğü uyarısı Web kancası şeması anlama](../../azure-monitor/platform/activity-log-alerts-webhook.md).  
* Daha fazla bilgi edinin [ITSM Bağlayıcısı](../../azure-monitor/platform/itsmc-overview.md)
* Daha fazla bilgi edinin [hız sınırlaması](../../azure-monitor/platform/alerts-rate-limiting.md) Uyarılardaki.
* Alma bir [etkinlik günlüğü uyarılarına genel bakış](../../azure-monitor/platform/alerts-overview.md)ve uyarıları alma hakkında bilgi edinin.  
* Bilgi edinmek için nasıl [hizmet durumu bildirimi gönderilen her uyarıları yapılandırma](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).
