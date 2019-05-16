---
title: Windows sanal masaüstü Kiracı ve ana makine havuzu oluşturma - Azure
description: Windows sanal masaüstü kiracılı bir ortam kurulumu sırasında sorunları ve Kiracı ve konak havuzu gidermek nasıl.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 04/08/2019
ms.author: v-chjenk
ms.openlocfilehash: 88e843c410a750387ecf58497dec79586e2a59d8
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523324"
---
# <a name="tenant-and-host-pool-creation"></a>Kiracı ve ana bilgisayar havuzu oluşturma

Bu makalede, Windows sanal masaüstü Kiracı ve ilgili oturumu konak havuzu altyapısının ilk kurulum sırasında sorunları ele alır.

## <a name="provide-feedback"></a>Geri bildirim gönder

Windows sanal masaüstü Önizleme aşamasındayken biz şu anda destek alma değildir. Ziyaret [Windows sanal masaüstü teknoloji topluluğuna](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) etkin topluluk üyeleri ve ürün ekibine Windows sanal masaüstü hizmetiyle tartışmak için.

## <a name="acquiring-the-windows-10-enterprise-multi-session-image"></a>Windows 10 Enterprise çok oturumu görüntü alınıyor

Windows 10 Enterprise çok oturumu görüntüyü kullanmak için Azure Marketi seçin Git **Başlarken** > **Microsoft Windows 10** > ve [için Windows 10 Enterprise Sanal Masaüstlerini Önizleme, sürüm 1809](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsdesktop.windows-10?tab=PlansAndPrice).

![Windows 10 Enterprise sürümü 1809 sanal masaüstlerini Önizleme için seçimi bir ekran görüntüsü.](media/AzureMarketPlace.png)

## <a name="creating-windows-virtual-desktop-tenant"></a>Windows sanal masaüstü Kiracı oluşturma

Bu bölüm, Windows sanal masaüstü Kiracı oluşturulurken olası sorunları kapsar.

### <a name="error-the-user-isnt-authorized-to-query-the-management-service"></a>Hata: Kullanıcı Yönetimi hizmetini sorgulama yetkisine sahip değil

![Ekran görüntüsü, PowerShell penceresinde bir kullanıcı, yönetim hizmeti sorgulamak için yetkili değil.](media/UserNotAuthorizedNewTenant.png)

Ham hata örneği:

```Error
   New-RdsTenant : User isn't authorized to query the management service.
   ActivityId: ad604c3a-85c6-4b41-9b81-5138162e5559
   Powershell commands to diagnose the failure:
   Get-RdsDiagnosticActivities -ActivityId ad604c3a-85c6-4b41-9b81-5138162e5559
   At line:1 char:1
   + New-RdsTenant -Name "testDesktopTenant" -AadTenantId "01234567-89ab-c ...
   + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
       + CategoryInfo          : FromStdErr: (Microsoft.RDInf...nt.NewRdsTenant:NewRdsTenant) [New-RdsTenant], RdsPowerSh
      ellException
       + FullyQualifiedErrorId : UnauthorizedAccess,Microsoft.RDInfra.RDPowershell.Tenant.NewRdsTenant
```

**Neden:** Oturum açmış olan kullanıcının, Azure Active Directory'de TenantCreator rol atanmamış.

**Düzeltme:** Bölümündeki yönergeleri [bir kullanıcı Azure Active Directory kiracınızdaki TenantCreator uygulama rolü atayın](https://docs.microsoft.com/azure/virtual-desktop/tenant-setup-azure-active-directory#assign-the-tenantcreator-application-role-to-a-user-in-your-azure-active-directory-tenant). Yönergeleri uyguladıktan sonra TenantCreator role atanmış kullanıcı gerekir.

![Ekran görüntüsü, TenantCreator rolü atanır.](media/TenantCreatorRoleAssigned.png)

## <a name="creating-windows-virtual-desktop-session-host-vms"></a>Windows Sanal Masaüstü oturumu konağı VM oluşturma

Uzak Masaüstü Hizmetleri/Windows sanal masaüstü takımlar yalnızca VM sağlama için Azure Resource Manager şablonu ilgili sorunlarını destek ancak oturumu ana bilgisayarı VM'ler çeşitli yollarla oluşturulabilir. Azure Resource Manager şablonu kullanılabilir [Azure Marketi](https://azuremarketplace.microsoft.com/) ve [GitHub](https://github.com/).

## <a name="issues-using-windows-virtual-desktop--provision-a-host-pool-azure-marketplace-offering"></a>Windows sanal masaüstü – sağlama konak havuzu Azure Market teklifi kullanarak sorunları

Windows sanal masaüstü – Azure Marketi'nden sağlama konak havuzu şablonu kullanılabilir.

### <a name="error-when-using-the-link-from-github-the-message-create-a-free-account-appears"></a>Hata: GitHub bağlantıyı kullanarak ileti "oluşturduğunuzda ücretsiz bir hesap" görüntülenir

![Ücretsiz bir hesap oluşturmak için ekran görüntüsü.](media/be615904ace9832754f0669de28abd94.png)

**1. neden:** Azure'da oturum açmak için kullanılan hesap active aboneliklerinde yok veya kullanılan hesap abonelikleri görmek için izinlere sahip değil.

**1 düzeltin:** Abonelik (en azından) katkıda bulunan erişimi oturumu ana bilgisayarı Vm'leri nerede bulunacağını dağıtılacak olan bir hesapla oturum açın.

**2. neden:** Kullanılmakta olan aboneliğin bir Microsoft bulut hizmeti sağlayıcısı (CSP) Kiracı'nın bir parçasıdır.

**2 düzeltin:** Git için GitHub konumu **oluşturma ve sağlama yeni sanal Windows Masaüstü konak havuzu** ve bu yönergeleri izleyin:

1. Sağ **azure'a Dağıt** seçip **kopya bağlantı adresi**.
2. Açık **not defteri** ve bağlantıyı yapıştırın.
3. # Karakterinden önce CSP son müşterinin Kiracı adı ekleyin.
4. Açık bir tarayıcı hem de Azure portalında yeni bağlantıya şablonu yükleyin.

    ```Example
    Example: https://portal.azure.com/<CSP end customer tenant name>
    #create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%
    2FRDS-Templates%2Fmaster%2Fwvd-templates%2FCreate%20and%20provision%20WVD%20host%20pool%2FmainTemplate.json
    ```

## <a name="azure-resource-manager-template-and-powershell-desired-state-configuration-dsc-errors"></a>Azure Resource Manager şablonu ve PowerShell Desired State Configuration (DSC) hataları

Azure Resource Manager şablonları ve PowerShell DSC başarısız dağıtımlardaki sorunları çözmek için bu yönergeleri izleyin.

1. Dağıtım kullanarak hataları gözden geçirin [Azure Resource Manager ile dağıtım işlemlerini görüntüleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).
2. Dağıtımdaki herhangi bir hata varsa, etkinlik günlüğü kullanarak hataları gözden geçirin. [kaynaklara uygulanan eylemleri denetlemek için etkinlik günlüklerini görüntüleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
3. Hata tanımlandıktan sonra hata iletisi ve kaynakları kullanın [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-common-deployment-errors) sorunu gidermek için.
4. Önceki dağıtım ve şablonu yeniden dağıtma yeniden deneme sırasında oluşturulan tüm kaynakları silin.

### <a name="error-your-deployment-failedhostnamejoindomain"></a>Hata: Dağıtım başarısız oldu... <hostname> /JOINDOMAIN

![Dağıtım başarısız oldu, ekran.](media/e72df4d5c05d390620e07f0d7328d50f.png)

Ham hata örneği:

```Error
 {"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. 
 Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\": \"Failed\",\r\n \"error\":
 {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.
 \",\r\n \"details\": [\r\n {\r\n \"code\": \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has reported a failure when processing
 extension 'joindomain'. Error message: \\\"Exception(s) occurred while joining Domain 'diamondsg.onmicrosoft.com'\\\".\"\r\n }\r\n ]\r\n }\r\n}"}]}
```

**1. neden:** VM'ler için etki alanına katılmak için sağlanan kimlik bilgileri hatalıdır.

**1 düzeltin:** VM'lerin etki alanına katılmamış olan için "Yanlış kimlik bilgileri" hatayı [oturum konak VM Yapılandırması](troubleshoot-vm-configuration.md).

**2. neden:** Etki alanı adı sorunu çözmezse.

**2 düzeltin:** Sanal makineleri etki alanına katılmamış olan "etki alanı adı sorunu çözmezse" hata bakın [oturum konak VM Yapılandırması](troubleshoot-vm-configuration.md).

### <a name="error-vmextensionprovisioningerror"></a>Hata: VMExtensionProvisioningError

![Ekran görüntüsü, uygulamanızın dağıtım terminal sağlama durumuyla başarısız.](media/7aaf15615309c18a984673be73ac969a.png)

**1. neden:** Windows sanal masaüstü ortamında ile geçici hata oluştu.

**2. neden:** Bağlantı ile geçici hata oluştu.

**Düzeltme:** PowerShell kullanarak oturum açarak Windows sanal masaüstü ortamın sağlıklı olup olmadığını onaylayın. El ile VM kaydı tamamlamak [PowerShell ile bir konak havuz oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).

### <a name="error-the-admin-username-specified-isnt-allowed"></a>Hata: Belirtilen yönetici kullanıcı adı izin verilmiyor

![Belirtilen yönetici izin verilmiyor ekran görüntüsü, dağıtım başarısız.](media/f2b3d3700e9517463ef88fa41875bac9.png)

Ham hata örneği:

```Error
 { "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostDesktop/providers/Microsoft.
  Resources/deployments/vmCreation-linkedTemplate/operations/EXAMPLE", "operationId": "EXAMPLE", "properties": { "provisioningOperation":
 "Create", "provisioningState": "Failed", "timestamp": "2019-01-29T20:53:18.904917Z", "duration": "PT3.0574505S", "trackingId":
 "1f460af8-34dd-4c03-9359-9ab249a1a005", "statusCode": "BadRequest", "statusMessage": { "error": { "code": "InvalidParameter", "message":
 "The Admin Username specified is not allowed.", "target": "adminUsername" } }, "targetResource": { "id": "/subscriptions/EXAMPLE
 /resourceGroups/demoHostDesktop/providers/Microsoft.Compute/virtualMachines/demo", "resourceType": "Microsoft.Compute/virtualMachines", "resourceName": "demo" } }}
```

**Neden:** Sağlanan parola (admin, administrator, kök) Yasak alt dizeler içeriyor.

**Düzeltme:** Kullanıcı adını güncelleştirin veya farklı kullanıcılar kullanın.

### <a name="error-vm-has-reported-a-failure-when-processing-extension"></a>Hata: VM uzantısı işlenirken bir hata bildirdi

![Uygulamanızın dağıtımı başarısız oldu, terminal sağlama durumuyla tamamlandı kaynak işleminin ekran görüntüsü.](media/49c4a1836a55d91cd65125cf227f411f.png)

Ham hata örneği:

```Error
{ "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostD/providers/Microsoft.Resources/deployments/
 rds.wvd-hostpool4-preview-20190129132410/operations/5A0757AC9E7205D2", "operationId": "5A0757AC9E7205D2", "properties":
 { "provisioningOperation": "Create", "provisioningState": "Failed", "timestamp": "2019-01-29T21:43:05.1416423Z",
 "duration": "PT7M56.8150879S", "trackingId": "43c4f71f-557c-4abd-80c3-01f545375455", "statusCode": "Conflict",
 "statusMessage": { "status": "Failed", "error": { "code": "ResourceDeploymentFailure", "message":
 "The resource operation completed with terminal provisioning state 'Failed'.", "details": [ { "code":
 "VMExtensionProvisioningError", "message": "VM has reported a failure when processing extension 'dscextension'. 
 Error message: \"DSC Configuration 'SessionHost' completed with error(s). Following are the first few: 
 PowerShell DSC resource MSFT_ScriptResource failed to execute Set-TargetResource functionality with error message: 
 One or more errors occurred. The SendConfigurationApply function did not succeed.\"." } ] } }, "targetResource": 
 { "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostD/providers/Microsoft. 
 Compute/virtualMachines/desktop-1/extensions/dscextension",
 "resourceType": "Microsoft.Compute/virtualMachines/extensions", "resourceName": "desktop-1/dscextension" } }}
```

**Neden:** PowerShell DSC uzantısı VM'de yönetici erişmek mümkün değildi.

**Düzeltme:** Kullanıcı adı ve parola, sanal makinede yönetici erişiminiz ve Azure Resource Manager şablonunu yeniden çalıştırmanız onaylayın.

### <a name="error-deploymentfailed--powershell-dsc-configuration-firstsessionhost-completed-with-errors"></a>Hata: DeploymentFailed – PowerShell DSC Yapılandırması 'FirstSessionHost' hatalarla tamamlandı

![PowerShell DSC Yapılandırması 'FirstSessionHost hatalarla tamamlandı' ekran görüntüsü, dağıtım başarısız.](media/64870370bcbe1286906f34cf0a8646ab.png)

Ham hata örneği:

```Error
{
    "code": "DeploymentFailed",
   "message": "At least one resource deployment operation failed. Please list 
 deployment operations for details. 4 Please see https://aka.ms/arm-debug for usage details.",
 "details": [
         { "code": "Conflict",  
         "message": "{\r\n \"status\": \"Failed\",\r\n \"error\": {\r\n \"code\":
         \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource
         operation completed with terminal provisioning state 'Failed'.\",\r\n
         \"details\": [\r\n {\r\n \"code\":
        \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has
              reported a failure when processing extension 'dscextension'.
              Error message: \\\"DSC Configuration 'FirstSessionHost'
              completed with error(s). Following are the first few:
              PowerShell DSC resource MSFT ScriptResource failed to
              execute Set-TargetResource functionality with error message:
              One or more errors occurred. The SendConfigurationApply
              function did not succeed.\\\".\"\r\n }\r\n ]\r\n }\r\n}"  }

```

**Neden:** PowerShell DSC uzantısı VM'de yönetici erişmek mümkün değildi.

**Düzeltme:** Kullanıcı adı ve parolası, sanal makinede yönetici erişimi olduğunu onaylayın ve Azure Resource Manager şablonunu yeniden çalıştırın.

### <a name="error-deploymentfailed--invalidresourcereference"></a>Hata: DeploymentFailed – InvalidResourceReference

Ham hata örneği:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/networkInterfaces/erd. Please make sure that
the referenced resource exists, and that both resources are in the same
region.\\\",\\r\\n\\\"details\\\": []\\r\\n }\\r\\n}\"\r\n }\r\n ]\r\n }\r\n ]\r\n }\r\n}"}]}
```

**Neden:** Kaynak grubu adının bir parçası, şablon tarafından oluşturulan bazı kaynaklar için kullanılır. Var olan kaynakları eşleşen adı nedeniyle, farklı bir gruptan mevcut bir kaynak şablonu seçebilirsiniz.

**Düzeltme:** Oturumu ana bilgisayarı Vm'leri dağıtmak için Azure Resource Manager şablonu çalıştırırken, ilk iki karakter Aboneliğinizin kaynak grubu adı için benzersiz hale getirin.

### <a name="error-deploymentfailed--invalidresourcereference"></a>Hata: DeploymentFailed – InvalidResourceReference

Ham hata örneği:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/DEMO/providers/Microsoft.Network/networkInterfaces
/EXAMPLE was not found. Please make sure that the referenced resource exists, and that both
resources are in the same region.\\\",\\r\\n \\\"details\\\": []\\r\\n }\\r\\n}\"\r\n
}\r\n ]\r\n }\r\n ]\r\n }\r\n\
```

**Neden:** Bu hata, Azure Resource Manager şablonu ile oluşturulan NIC aynı adı taşıyan başka bir NIC VNET içinde zaten sahip olmasıdır.

**Düzeltme:** Farklı bir ana bilgisayar ön ekini kullanın.

### <a name="error-deploymentfailed--error-downloading"></a>Hata: DeploymentFailed – indirilirken hata

Ham hata örneği:

```Error
\\\"The DSC Extension failed to execute: Error downloading
https://catalogartifact.azureedge.net/publicartifacts/rds.wvd-hostpool-3-preview-
2dec7a4d-006c-4cc0-965a-02bbe438d6ff-private-preview-
1/Artifacts/DSC/Configuration.zip after 29 attempts: The remote name could not be
resolved: 'catalogartifact.azureedge.net'.\\nMore information about the failure can
be found in the logs located under
'C:\\\\WindowsAzure\\\\Logs\\\\Plugins\\\\Microsoft.Powershell.DSC\\\\2.77.0.0' on
the VM.\\\"
```

**Neden:** Bu hata bir statik rota, güvenlik duvarı kuralı veya Azure Resource Manager şablonuna bağlı ZIP dosyasının indirme engelleyen NSG kaynaklanır.

**Düzeltme:** Statik yönlendirme, güvenlik duvarı kuralı veya NSG engelleme kaldırın. İsteğe bağlı olarak, Azure Resource Manager şablon json dosyasını bir metin düzenleyicisinde açın, bağlantıyı zip dosyasını alın ve kaynak izin verilen bir konuma indirin.

### <a name="error-the-user-isnt-authorized-to-query-the-management-service"></a>Hata: Kullanıcı Yönetimi hizmetini sorgulama yetkisine sahip değil

Ham hata örneği:

```Error
"response": { "content": { "startTime": "2019-04-01T17:45:33.3454563+00:00", "endTime": "2019-04-01T17:48:52.4392099+00:00", 
"status": "Failed", "error": { "code": "VMExtensionProvisioningError", "message": "VM has reported a failure when processing 
extension 'dscextension'. Error message: \"DSC Configuration 'FirstSessionHost' completed with error(s). 
Following are the first few: PowerShell DSC resource MSFT_ScriptResource failed to execute Set-TargetResource
 functionality with error message: User is not authorized to query the management service.
\nActivityId: 1b4f2b37-59e9-411e-9d95-4f7ccd481233\nPowershell commands to diagnose the failure:
\nGet-RdsDiagnosticActivities -ActivityId 1b4f2b37-59e9-411e-9d95-4f7ccd481233\n 
The SendConfigurationApply function did not succeed.\"." }, "name": "2c3272ec-d25b-47e5-8d70-a7493e9dc473" } } }}
```

**Neden:** Belirtilen Windows sanal masaüstü Kiracı Yöneticisi geçerli rol ataması yok.

**Düzeltme:** Windows sanal masaüstü PowerShell oturum açın ve bir rol ataması denenen kullanıcı atamak Windows sanal masaüstü Kiracı oluşturan kullanıcının olmalıdır. GitHub Azure Resource Manager şablon parametreleri çalıştırıyorsanız, PowerShell komutlarını kullanarak bu yönergeleri izleyin:

```PowerShell
Add-RdsAccount -DeploymentUrl “https://rdbroker.wvd.microsoft.com”
New-RdsRoleAssignment -TenantName <Windows Virtual Desktop tenant name> -RoleDefinitionName “RDS Contributor” -SignInName <UPN>
```

### <a name="error-user-requires-azure-multi-factor-authentication-mfa"></a>Hata: Azure multi-Factor Authentication (MFA) kullanıcı gerektirir

![Dağıtımınızın nedeniyle başarısız oldu, multi-Factor Authentication (MFA) yetersiz olduğu için ekran görüntüsü](media/MFARequiredError.png)

Ham hata örneği:

```Error
"message": "{\r\n  \"status\": \"Failed\",\r\n  \"error\": {\r\n    \"code\": \"ResourceDeploymentFailure\",\r\n    \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.\",\r\n    \"details\": [\r\n      {\r\n        \"code\": \"VMExtensionProvisioningError\",\r\n        \"message\": \"VM has reported a failure when processing extension 'dscextension'. Error message: \\\"DSC Configuration 'FirstSessionHost' completed with error(s). Following are the first few: PowerShell DSC resource MSFT_ScriptResource  failed to execute Set-TargetResource functionality with error message: One or more errors occurred.  The SendConfigurationApply function did not succeed.\\\".\"\r\n      }\r\n    ]\r\n  }\r\n}"
```

**Neden:** Belirtilen Windows sanal masaüstü Kiracı yönetici oturum açmak için Azure multi-Factor Authentication (MFA) gerektirir.

**Düzeltme:** Hizmet sorumlusu oluşturma ve Windows sanal masaüstü kiracınız için bir rolü, içindeki adımları izleyerek Ata [Öğreticisi: PowerShell ile hizmet sorumluları ve rol atamalarını oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-service-principal-role-powershell). Windows sanal masaüstüne hizmet sorumlusu ile oturum açabildiğinizi olduğunu doğruladıktan sonra Azure Marketi'nde teklif ya da hangi yöntemine bağlı olarak, kullanmakta olduğunuz GitHub Azure Resource Manager şablonu yeniden çalıştırın. Yönteminiz için doğru parametreleri girmek için aşağıdaki yönergeleri izleyin.

Azure Marketi'nde teklif çalıştırıyorsanız, düzgün bir şekilde sanal masaüstü Windows kimlik doğrulaması yapmak aşağıdaki parametreler için değerleri sağlayın:

- Windows sanal masaüstü Kiracı RDS sahibi: Hizmet sorumlusu
- Uygulama Kimliği: Oluşturduğunuz yeni bir hizmet sorumlusu uygulama kimliği
- Parola, parola ve onaylayın: Hizmet sorumlusu için oluşturulan parola gizliliği
- Azure AD Kiracı kimliği: Oluşturduğunuz hizmet sorumlusunun bir Azure AD Kiracı kimliği

GitHub Azure Resource Manager şablonu çalıştırıyorsanız, Windows sanal masaüstüne düzgün bir şekilde kimlik doğrulaması aşağıdaki parametreler için değerleri sağlayın:

- Kiracı yönetici kullanıcı asıl adı (UPN) veya uygulama kimliği: Oluşturduğunuz yeni bir hizmet sorumlusu uygulama kimliği
- Kiracı yönetici parolası: Hizmet sorumlusu için oluşturulan parola gizliliği
- IsServicePrincipal: **true**
- AadTenantId: Oluşturduğunuz hizmet sorumlusunun bir Azure AD Kiracı kimliği

## <a name="next-steps"></a>Sonraki adımlar

- Windows sanal masaüstü ve yükseltme parçaları sorun giderme hakkında genel bir bakış için bkz: [genel bakış, geri bildirim ve destek sorunlarını giderme](troubleshoot-set-up-overview.md).
- Bir sanal makine (VM) Windows sanal masaüstü yapılandırılırken sorunlarını gidermek için bkz: [oturumu ana bilgisayar Sanal Makine Yapılandırması](troubleshoot-vm-configuration.md).
- Windows sanal masaüstü istemci bağlantıları ile ilgili sorunları gidermek için bkz: [Uzak Masaüstü istemci bağlantıları](troubleshoot-client-connection.md).
- PowerShell ile Windows sanal masaüstü kullanırken sorunlarını gidermek için bkz: [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md).
- Önizleme hizmeti hakkında daha fazla bilgi için bkz: [Windows Desktop Önizleme ortamı](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Bir sorun giderme öğreticiyi incelemek için bkz: [Öğreticisi: Resource Manager şablonu dağıtımlardaki sorunları çözmek](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Eylemler denetleme hakkında bilgi edinmek için [Resource Manager denetim işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Dağıtım sırasında hataları belirlemek için Eylemler hakkında bilgi edinmek için bkz. [dağıtım işlemlerini görüntüleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).