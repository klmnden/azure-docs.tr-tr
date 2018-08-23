---
title: Azure Stack için hizmet sorumlusu oluşturma | Microsoft Docs
description: Azure Kaynak Yöneticisi'nde rol tabanlı erişim denetimi ile kaynaklara erişimi yönetmek için kullanılabilir olan yeni bir hizmet sorumlusu oluşturmayı açıklar.
services: azure-resource-manager
documentationcenter: na
author: sethmanheim
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2018
ms.author: sethm
ms.openlocfilehash: f7233d6a27b9ec3d58f33f7032bbec7a646d24f7
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42366128"
---
# <a name="provide-applications-access-to-azure-stack"></a>Uygulamalara Azure Stack erişimi sağlama

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bir uygulama dağıtmak veya Azure Stack'te Azure Resource Manager üzerinden kaynaklarını yapılandırmak için erişim gerektiğinde, uygulamanız için bir kimlik bilgisi olan bir hizmet sorumlusu oluşturun.  Ardından, yalnızca bu hizmet sorumlusuna gerekli izinleri devredebilirsiniz.  

Örneğin, Azure kaynaklarını envantere almak üzere Azure Resource Manager'ı kullanan bir yapılandırma yönetim aracı olabilir.  Bu senaryoda, hizmet sorumlusu oluşturma, bu hizmet sorumlusu okuyucu rolüne verin ve yapılandırma Yönetim Aracı'na salt okunur erişimi sınırlayın. 

Hizmet sorumluları olmadığından uygulama kendi kimlik bilgileriniz altında çalışan için tercih edilir:

* Hizmet sorumlusu kendi hesap izinleri farklı olan izinler atayabilirsiniz. Normalde, bu izinler tam olarak uygulamaya gereken izinlerle sınırlı olur.
* Sizin Sorumluluklarınız değiştirirseniz uygulamanın kimlik bilgilerini değiştirmeniz gerekmez.
* Katılımsız betik yürütülürken kimlik doğrulaması otomatikleştirmek için bir sertifika kullanabilirsiniz.  

## <a name="getting-started"></a>Başlarken

Nasıl Azure Stack dağıttığınız bağlı olarak, bir hizmet sorumlusu oluşturma işlemiyle başlayın.  Bu belge, her ikisi için de bir hizmet sorumlusu oluşturma işleminde size kılavuzluk eder [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad) ve [Active Directory Federasyon Services(AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).  Hizmet sorumlusu oluşturduktan sonra AD FS ve Azure Active Directory genel adımları alışkın olduğunuz [temsilci izinleri](azure-stack-create-service-principals.md#assign-role-to-service-principal) rolü.     

## <a name="create-service-principal-for-azure-ad"></a>Azure AD hizmet sorumlusu oluşturma

Azure Stack kimlik deposu olarak Azure AD'yi kullanarak dağıttıysanız, Azure için gibi hizmet sorumluları oluşturabilirsiniz.  Bu bölümde Portalı aracılığıyla adımların nasıl gerçekleştirileceğini gösterir.  Sahip olduğunuz denetimi [Azure AD izinleri gerekli](../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) başlamadan önce.

### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma
Bu bölümde, Azure AD'de uygulamanızı temsil eden bir uygulama (hizmet sorumlusu) oluşturma.

1. Üzerinden Azure hesabınızla oturum açın [Azure portalında](https://portal.azure.com).
2. Seçin **Azure Active Directory** > **uygulama kayıtları** > **Ekle**   
3. Uygulama için bir ad ve URL sağlayın. Şunlardan birini seçin **Web uygulaması / API** veya **yerel** oluşturmak istediğiniz uygulama türü. Değerleri ayarladıktan sonra seçin **Oluştur**.

Uygulamanız için bir hizmet sorumlusu oluşturdunuz.

### <a name="get-credentials"></a>Kimlik bilgilerini al
Programlamayla oturum açılırken, kimlik, uygulamanız için ve bir Web uygulaması için kullandığınız / API, bir kimlik doğrulama anahtarı. Bu değerleri almak için aşağıdaki adımları kullanın:

1. Gelen **uygulama kayıtları** Active Directory'de, uygulamanızı seçin.

2. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın. Uygulamalarda [örnek uygulamalar](#sample-applications) istemci kimliği olarak bu değere bölümüne bakın

     ![istemci kimliği](./media/azure-stack-create-service-principal/image12.png)
3. Bir Web uygulaması için bir kimlik doğrulama anahtarını oluşturmak için / API, select **ayarları** > **anahtarları**. 

4. Anahtar için bir açıklama ve süre sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.

Anahtar kaydedildikten sonra, anahtarın değeri görüntülenir. Bu değeri kopyalayın çünkü daha sonra anahtarı alamazsınız. Uygulama Kimliği da uygulamayı imzalamak için anahtar değerini sağlayın. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

![kaydedilen anahtar](./media/azure-stack-create-service-principal/image15.png)


İşlem tamamlandıktan sonra devam [uygulamanızı rol atama](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>AD FS için hizmet sorumlusu oluşturma
AD FS ile Azure Stack dağıttıysanız, hizmet sorumlusu oluşturma, erişim için rol atama ve o kimlik kullanarak PowerShell üzerinden oturum için PowerShell kullanabilirsiniz.

Betik ayrıcalıklı uç noktasından bir ERCS sanal makinede çalıştırılır.


Gereksinimler:
- Bir sertifika gereklidir.

**Parametreler**

Aşağıdaki bilgiler gereklidir Otomasyon parametreler için giriş olarak:


|Parametre|Açıklama|Örnek|
|---------|---------|---------|
|Ad|SPN hesabının adı|Uygulamam|
|ClientCertificates|Sertifika nesneler dizisi|X509 sertifika|
|ClientRedirectUris<br>(İsteğe bağlı)|Uygulama yeniden yönlendirme URI'si|         |

**Örnek**

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki komutları çalıştırın:

   > [!NOTE]
   > Bu örnekte otomatik olarak imzalanan bir sertifika oluşturur. Bu komutları bir üretim dağıtımında çalıştırdığınızda, kullanmak istediğiniz sertifika için sertifika nesnesini almak için Get-sertifika'yı kullanın.

   ```PowerShell  
    # Credential for accessing the ERCS PrivilegedEndpoint typically domain\cloudadmin
    $creds = Get-Credential

    # Creating a PSSession to the ERCS PrivilegedEndpoint
    $session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $creds

    # This produces a self signed cert for testing purposes.  It is prefered to use a managed certificate for this.
    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<yourappname>" -KeySpec KeyExchange

    $ServicePrincipal = Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name '<yourappname>' -ClientCertificates $using:cert}
    $AzureStackInfo = Invoke-Command -Session $session -ScriptBlock { get-azurestackstampinformation }
    $session|remove-pssession

    # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

    # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

    # TenantID for the stamp. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $TenantID = $AzureStackInfo.AADTenantID

    # Register an AzureRM environment that targets your Azure Stack instance
    Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

    # Set the GraphEndpointResourceId value
    Set-AzureRmEnvironment `
    -Name "AzureStackUser" `
    -GraphAudience $GraphAudience `
    -EnableAdfsAuthentication:$true

    Add-AzureRmAccount -EnvironmentName "azurestackuser" `
    -ServicePrincipal `
    -CertificateThumbprint $ServicePrincipal.Thumbprint `
    -ApplicationId $ServicePrincipal.ClientId `
    -TenantId $TenantID

    # Output the SPN details
    $ServicePrincipal

   ```

2. Otomasyon tamamlandıktan sonra SPN kullanmak için gerekli ayrıntıları görüntüler. 

   Örneğin:

   ```
   ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
   ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
   Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
   ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
   PSComputerName        : azs-ercs01
   RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
   ```

### <a name="assign-a-role"></a>Rol atama
Hizmet sorumlusu oluşturulduktan sonra şunları yapmalısınız [rol atama](azure-stack-create-service-principals.md#assign-role-to-service-principal)

### <a name="sign-in-through-powershell"></a>PowerShell aracılığıyla oturum açın
Bir rol atadıktan sonra Azure Stack aşağıdaki komutu ile hizmet sorumlusunu kullanarak oturum:

```powershell
Add-AzureRmAccount -EnvironmentName "<AzureStackEnvironmentName>" `
 -ServicePrincipal `
 -CertificateThumbprint $servicePrincipal.Thumbprint `
 -ApplicationId $servicePrincipal.ClientId ` 
 -TenantId $directoryTenantId
```

## <a name="assign-role-to-service-principal"></a>Hizmet sorumlusuna rolü atama
Aboneliğinizdeki kaynaklara erişmek için uygulamaya bir rol atamanız gerekir. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir roller hakkında bilgi edinmek için [RBAC: yerleşik roller](../role-based-access-control/built-in-roles.md).

Abonelik, kaynak grubu veya kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam için izinler devralınmıştır. Örneğin, bir kaynak grubu için okuyucu rolüne uygulamaya ekleme kaynak grubunu ve içerdiği tüm kaynakları okuyun anlamına gelir.

1. Azure Stack Portalı'nda, uygulamayı atamak istediğiniz kapsam düzeyini gidin. Örneğin abonelik kapsamında bir rol atamak için seçin **abonelikleri**. Bunun yerine, bir kaynak grubu veya kaynak seçebilirsiniz.

2. Belirli bir abonelikte (kaynak grubu veya kaynak) uygulama atamak için seçin.

     ![Abonelik atama için seçin](./media/azure-stack-create-service-principal/image16.png)

3. Seçin **erişim denetimi (IAM)**.

     ![erişim seçin](./media/azure-stack-create-service-principal/image17.png)

4. **Add (Ekle)** seçeneğini belirleyin.

5. Uygulamayı atamak istediğiniz rolü seçin.

6. Uygulamanız için arama yapın ve seçin.

7. Seçin **Tamam** rol atama tamamlanması. Bu kapsam için bir role atanmış kullanıcı listesinde uygulamanızı görürsünüz.

Bir hizmet sorumlusu oluşturuldu ve atanan role göre bu Azure Stack kaynaklara erişmek için uygulamanızı içinde kullanmaya başlayabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

[ADFS için kullanıcı ekleme](azure-stack-add-users-adfs.md)
[kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)
