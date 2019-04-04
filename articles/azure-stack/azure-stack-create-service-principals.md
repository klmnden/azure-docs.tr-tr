---
title: Azure Stack için hizmet sorumlusu yönetme | Microsoft Docs
description: Azure Kaynak Yöneticisi'nde rol tabanlı erişim denetimi ile kaynaklara erişimi yönetmek için kullanılabilir olan yeni bir hizmet sorumlusu yönetme işlemi açıklanır.
services: azure-resource-manager
documentationcenter: na
author: sethmanheim
manager: femila
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2018
ms.author: sethm
ms.lastreviewed: 12/18/2018
ms.openlocfilehash: 0f5a4dc76830740d69547a01ce40b5e10cf4a74b
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499417"
---
# <a name="provide-applications-access-to-azure-stack"></a>Uygulamalara Azure Stack erişimi sağlama

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bir uygulama dağıtmak veya Azure Stack'te Azure Resource Manager üzerinden kaynaklarını yapılandırmak için erişim gerektiğinde, uygulamanız için bir kimlik bilgisi olan bir hizmet sorumlusu oluşturun. Ardından, yalnızca bu hizmet sorumlusuna gerekli izinleri devredebilirsiniz.  

Örneğin, Azure kaynaklarını envantere almak üzere Azure Resource Manager'ı kullanan bir yapılandırma yönetim aracı olabilir. Bu senaryoda, hizmet sorumlusu oluşturma, bu hizmet sorumlusu okuyucu rolüne verin ve yapılandırma Yönetim Aracı'na salt okunur erişimi sınırlayın. 

Hizmet sorumluları olmadığından uygulama kendi kimlik bilgileriniz altında çalışan için tercih edilir:

 - Hizmet sorumlusu kendi hesap izinleri farklı olan izinler atayabilirsiniz. Normalde, bu izinler tam olarak uygulamaya gereken izinlerle sınırlı olur.
 - Sizin Sorumluluklarınız değiştirirseniz uygulamanın kimlik bilgilerini değiştirmeniz gerekmez.
 - Katılımsız betik yürütülürken kimlik doğrulaması otomatikleştirmek için bir sertifika kullanabilirsiniz.  

## <a name="getting-started"></a>Başlarken

Nasıl Azure Stack dağıttığınız bağlı olarak, bir hizmet sorumlusu oluşturma işlemiyle başlayın. Bu belgede, hizmet sorumlusu için oluşturma açıklanmaktadır:

- Azure Active Directory (Azure AD). Azure AD sağlayan bir çok kiracılı, bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Azure AD bağlı bir Azure Stack ile kullanabilirsiniz.
- Active Directory Federasyon Hizmetleri (AD FS). AD FS Basitleştirilmiş, güvenli Kimlik Federasyonu ve Web'de çoklu oturum açma (SSO) özellikleri sağlar. AD FS ile Azure Stack örnekleri bağlı ve bağlantısı kesilmiş kullanabilirsiniz.

Hizmet sorumlusu oluşturduktan sonra AD FS ve Azure Active Directory genel adımları rol izinleri devretmek için kullanılır.

## <a name="manage-service-principal-for-azure-ad"></a>Azure AD hizmet sorumlusunu Yönet

Azure Active Directory (Azure AD) ile Azure Stack, kimlik yönetimi hizmeti olarak dağıttıysanız, Azure için gibi hizmet sorumluları oluşturabilirsiniz. Bu bölümde Portalı aracılığıyla adımların nasıl gerçekleştirileceğini gösterir. Sahip olduğunuz denetimi [Azure AD izinleri gerekli](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions) başlamadan önce.

### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

Bu bölümde, Azure AD'de uygulamanızı temsil eden bir uygulama (hizmet sorumlusu) oluşturma.

1. Üzerinden Azure hesabınızla oturum açın [Azure portalında](https://portal.azure.com).
2. Seçin **Azure Active Directory** > **uygulama kayıtları** > **yeni uygulama kaydı**
3. Uygulama için bir ad ve URL sağlayın. Şunlardan birini seçin **Web uygulaması / API** veya **yerel** oluşturmak istediğiniz uygulama türü. Değerleri ayarladıktan sonra seçin **Oluştur**.

Uygulamanız için bir hizmet sorumlusu oluşturdunuz.

### <a name="get-credentials"></a>Kimlik bilgilerini al

Programlamayla oturum açılırken, kimlik, uygulamanız için ve bir Web uygulaması için kullandığınız / API, bir kimlik doğrulama anahtarı. Bu değerleri almak için aşağıdaki adımları kullanın:

1. Gelen **uygulama kayıtları** Active Directory'de, uygulamanızı seçin.

2. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın. Örnek uygulamalar bölümü uygulamalarında istemci kimliği olarak bu değere bakın.

     ![İstemci kimliği](./media/azure-stack-create-service-principal/image12.png)
3. Bir Web uygulaması için bir kimlik doğrulama anahtarını oluşturmak için / API, select **ayarları** > **anahtarları**. 

4. Anahtar için bir açıklama ve süre sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.

Anahtar kaydedildikten sonra, anahtarın değeri görüntülenir. Daha sonra anahtarı alınamıyor çünkü bu değeri not defteri veya başka bir geçici konuma, kopyalayın. Uygulama Kimliği da uygulamayı imzalamak için anahtar değerini sağlayın. Anahtar değeri, uygulamanızın onu alabildiği bir yerde Store.

![kaydedilen anahtar](./media/azure-stack-create-service-principal/image15.png)

İşlem tamamlandıktan sonra uygulamanızı bir rol atayabilirsiniz.

## <a name="manage-service-principal-for-ad-fs"></a>AD FS için hizmet sorumlusunu Yönet

Active Directory Federasyon Hizmetleri (AD FS) ile Azure Stack, kimlik yönetimi hizmeti olarak dağıttıysanız, hizmet sorumlusu oluşturma, erişim için bir rol atayın ve bu kimliklerini kullanarak oturum için PowerShell kullanın.

AD FS ile hizmet sorumlusu oluşturmak için iki yöntemden birini kullanabilirsiniz. Şunları yapabilirsiniz:
 - [Bir sertifika kullanarak bir hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md#create-a-service-principal-using-a-certificate)
 - [İstemci gizli anahtarı kullanarak bir hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md#create-a-service-principal-using-a-client-secret)

AD FS yönetimi ile ilgili görevler sorumluları hizmeti.

| Type | Eylem |
| --- | --- |
| AD FS sertifikası | [Oluşturma](azure-stack-create-service-principals.md#create-a-service-principal-using-a-certificate) |
| AD FS sertifikası | [Güncelleştirme](azure-stack-create-service-principals.md#update-certificate-for-service-principal-for-ad-fs) |
| AD FS sertifikası | [Kaldır](azure-stack-create-service-principals.md#remove-a-service-principal-for-ad-fs) |
| AD FS istemci gizli anahtarı | [Oluşturma](azure-stack-create-service-principals.md#create-a-service-principal-using-a-client-secret) |
| AD FS istemci gizli anahtarı | [Güncelleştirme](azure-stack-create-service-principals.md#create-a-service-principal-using-a-client-secret) |
| AD FS istemci gizli anahtarı | [Kaldır](azure-stack-create-service-principals.md#remove-a-service-principal-for-ad-fs) |

### <a name="create-a-service-principal-using-a-certificate"></a>Bir sertifika kullanarak bir hizmet sorumlusu oluşturma

Bir hizmet sorumlusu kimliği için AD FS kullanırken oluştururken, bir sertifika kullanabilirsiniz.

#### <a name="certificate"></a>Sertifika

Bir sertifika gereklidir.

**Sertifika gereksinimleri**

 - Şifreleme hizmeti sağlayıcısı (CSP), eski anahtar sağlayıcısı olmalıdır.
 - Ortak ve özel anahtarlar gerektiğinde PFX dosyasını sertifika biçimi olmalıdır. Windows sunucuları, ortak anahtar dosyasını (SSL sertifika dosyası) içeren .pfx dosyaları ve ilişkili özel anahtar dosyasını kullanın.
 - Üretim için sertifika bir iç sertifika yetkilisi veya bir ortak sertifika yetkilisi verilmiş olması gerekir. Bir ortak sertifika yetkilisi kullanmanız durumunda, içermesi yetkilisi temel işletim sistemi görüntüsüne Microsoft güvenilir kök yetkilisi programının bir parçası olarak. Tam listesini bulabilirsiniz [Microsoft güvenilen kök sertifika programı: Katılımcıların](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).
 - Azure Stack altyapınızı, sertifika yetkilisinin sertifika iptal listesi (CRL) konumuna sertifikada yayımlanan ağ erişimi olması gerekir. Bu CRL bir HTTP uç noktası olmalıdır.

#### <a name="parameters"></a>Parametreler

Aşağıdaki bilgiler gereklidir Otomasyon parametreler için giriş olarak:

|Parametre|Açıklama|Örnek|
|---------|---------|---------|
|Ad|SPN hesabının adı|Uygulamam|
|ClientCertificates|Sertifika nesneler dizisi|X509 sertifika|
|ClientRedirectUris<br>(İsteğe bağlı)|Uygulama yeniden yönlendirme URI'si|-|

#### <a name="use-powershell-to-create-a-service-principal"></a>Bir hizmet sorumlusu oluşturmak için PowerShell kullanma

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki cmdlet'leri çalıştırın:

   ```powershell  
    # Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
    $Creds = Get-Credential

    # Creating a PSSession to the ERCS PrivilegedEndpoint
    $Session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $Creds

    # If you have a managed certificate use the Get-Item command to retrieve your certificate from your certificate location.
    # If you don't want to use a managed certificate, you can produce a self signed cert for testing purposes: 
    # $Cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<YourAppName>" -KeySpec KeyExchange
    $Cert = Get-Item "<YourCertificateLocation>"
    
    $ServicePrincipal = Invoke-Command -Session $Session -ScriptBlock {New-GraphApplication -Name '<YourAppName>' -ClientCertificates $using:cert}
    $AzureStackInfo = Invoke-Command -Session $Session -ScriptBlock {Get-AzureStackStampInformation}
    $Session | Remove-PSSession

    # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. This is read from the AzureStackStampInformation output of the ERCS VM.
    $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

    # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. This is read from the AzureStackStampInformation output of the ERCS VM.
    $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

    # TenantID for the stamp. This is read from the AzureStackStampInformation output of the ERCS VM.
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

    Add-AzureRmAccount -EnvironmentName "AzureStackUser" `
    -ServicePrincipal `
    -CertificateThumbprint $ServicePrincipal.Thumbprint `
    -ApplicationId $ServicePrincipal.ClientId `
    -TenantId $TenantID

    # Output the SPN details
    $ServicePrincipal

   ```
   > [!Note]  
   > Kendinden imzalı bir sertifika kullanarak oluşturulabilir doğrulama amacıyla aşağıdaki örnekteki:

   ```powershell  
   $Cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<yourappname>" -KeySpec KeyExchange
   ```


2. Otomasyon tamamlandıktan sonra SPN kullanmak için gerekli ayrıntıları görüntüler. Daha sonra kullanmak için çıktıyı depolamak için önerilir.

   Örneğin:

   ```shell
   ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
   ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
   Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
   ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
   PSComputerName        : azs-ercs01
   RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
   ```

### <a name="update-certificate-for-service-principal-for-ad-fs"></a>AD FS için hizmet sorumlusu sertifikasını güncelleştir

AD FS ile Azure Stack dağıttıysanız, bir hizmet sorumlusu için parolayı güncelleştirmek için PowerShell kullanabilirsiniz.

Betik ayrıcalıklı uç noktasından bir ERCS sanal makinede çalıştırılır.

#### <a name="parameters"></a>Parametreler

Aşağıdaki bilgiler gereklidir Otomasyon parametreler için giriş olarak:

|Parametre|Açıklama|Örnek|
|---------|---------|---------|
|Ad|SPN hesabının adı|Uygulamam|
|ApplicationIdentifier|Benzersiz tanımlayıcı|S-1-5-21-1634563105-1224503876-2692824315-2119|
|ClientCertificate|Sertifika nesneler dizisi|X509 sertifika|

#### <a name="example-of-updating-service-principal-for-ad-fs"></a>AD FS için hizmet sorumlusu güncelleştirme örneği

Bu örnek, otomatik olarak imzalanan bir sertifika oluşturur. Bir üretim dağıtımında cmdlet'lerini çalıştırdığınızda kullanmanız [Get-Item](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Management/Get-Item) kullanmak istediğiniz sertifika için sertifika nesnesini almak için.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki cmdlet'leri çalıştırın:

     ```powershell
          # Creating a PSSession to the ERCS PrivilegedEndpoint
          $Session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $Creds

          # This produces a self signed cert for testing purposes. It is preferred to use a managed certificate for this.
          $NewCert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<YourAppName>" -KeySpec KeyExchange

          $RemoveServicePrincipal = Invoke-Command -Session $Session -ScriptBlock {Set-GraphApplication -ApplicationIdentifier  S-1-5-21-1634563105-1224503876-2692824315-2120 -ClientCertificates $NewCert}

          $Session | Remove-PSSession
     ```

2. Otomasyon tamamlandıktan sonra SPN kimlik doğrulaması için gereken güncelleştirilmiş parmak izi değerini görüntüler.

     ```Shell  
          ClientId              : 
          Thumbprint            : AF22EE716909041055A01FE6C6F5C5CDE78948E9
          ApplicationName       : Azurestack-ThomasAPP-3e5dc4d2-d286-481c-89ba-57aa290a4818
          ClientSecret          : 
          RunspaceId            : a580f894-8f9b-40ee-aa10-77d4d142b4e5
     ```

### <a name="create-a-service-principal-using-a-client-secret"></a>İstemci gizli anahtarı kullanarak bir hizmet sorumlusu oluşturma

Bir hizmet sorumlusu kimliği için AD FS kullanırken oluştururken, bir sertifika kullanabilirsiniz. Ayrıcalıklı uç noktasına cmdlet'leri çalıştırmak için kullanın.

Bu betikler ayrıcalıklı uç noktasından bir ERCS sanal makinede çalıştırılır. Ayrıcalıklı uç noktası hakkında daha fazla bilgi için bkz: [Azure Stack'te ayrıcalıklı uç noktayı kullanarak](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).

#### <a name="parameters"></a>Parametreler

Aşağıdaki bilgiler gereklidir Otomasyon parametreler için giriş olarak:

| Parametre | Açıklama | Örnek |
|----------------------|--------------------------|---------|
| Ad | SPN hesabının adı | Uygulamam |
| GenerateClientSecret | Gizli dizi oluşturma |  |

#### <a name="use-the-ercs-privilegedendpoint-to-create-the-service-principal"></a>Hizmet sorumlusu oluşturmak için ERCS PrivilegedEndpoint kullanın

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki cmdlet'leri çalıştırın:

     ```powershell  
      # Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
     $Creds = Get-Credential

     # Creating a PSSession to the ERCS PrivilegedEndpoint
     $Session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $Creds

     # Creating a SPN with a secre
     $ServicePrincipal = Invoke-Command -Session $Session -ScriptBlock {New-GraphApplication -Name '<YourAppName>' -GenerateClientSecret}
     $AzureStackInfo = Invoke-Command -Session $Session -ScriptBlock {Get-AzureStackStampInformation}
     $Session | Remove-PSSession

     # Output the SPN details
     $ServicePrincipal
     ```

2. Cmdlet'leri çalıştırdıktan sonra Kabuk SPN kullanmak için gerekli ayrıntıları görüntüler. İstemci gizli anahtarı sakladığınızdan emin olun.

     ```powershell  
     ApplicationIdentifier : S-1-5-21-1634563105-1224503876-2692824315-2623
     ClientId              : 8e0ffd12-26c8-4178-a74b-f26bd28db601
     Thumbprint            : 
     ApplicationName       : Azurestack-YourApp-6967581b-497e-4f5a-87b5-0c8d01a9f146
     ClientSecret          : 6RUZLRoBw3EebMDgaWGiowCkoko5_j_ujIPjA8dS
     PSComputerName        : 192.168.200.224
     RunspaceId            : 286daaa1-c9a6-4176-a1a8-03f543f90998
     ```

#### <a name="update-client-secret-for-a-service-principal-for-ad-fs"></a>AD FS için bir hizmet sorumlusunun istemci gizli anahtarı güncelleştirme

Yeni bir gizli PowerShell cmdlet tarafından oluşturulan otomatik olarak.

Betik ayrıcalıklı uç noktasından bir ERCS sanal makinede çalıştırılır.

##### <a name="parameters"></a>Parametreler

Aşağıdaki bilgiler gereklidir Otomasyon parametreler için giriş olarak:

| Parametre | Açıklama | Örnek |
|-----------------------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------|
| ApplicationIdentifier | Benzersiz tanımlayıcısı. | S-1-5-21-1634563105-1224503876-2692824315-2119 |
| ChangeClientSecret | Gizli bir geçiş süresi 2880 eski parolayı hala geçerli olduğu dakika ile değiştirir. |  |
| ResetClientSecret | İstemci gizli anahtarı hemen değiştirme |  |

##### <a name="example-of-updating-a-client-secret-for-ad-fs"></a>Örneğin, AD FS için bir istemci gizli anahtarı güncelleştirme

Örnekte **ResetClientSecret** parametresini hemen gizli değiştirir.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki cmdlet'leri çalıştırın:

     ```powershell  
          # Creating a PSSession to the ERCS PrivilegedEndpoint
          $Session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $Creds

          # This produces a self signed cert for testing purposes. It is preferred to use a managed certificate for this.
          $NewCert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<YourAppName>" -KeySpec KeyExchange

          $UpdateServicePrincipal = Invoke-Command -Session $Session -ScriptBlock {Set-GraphApplication -ApplicationIdentifier  S-1-5-21-1634563105-1224503876-2692824315-2120 -ResetClientSecret}

          $Session | Remove-PSSession
     ```

2. Otomasyon tamamlandıktan sonra yeni oluşturulan gizli dizi SPN kimlik doğrulaması için gerekli görüntüler. Yeni gizli sakladığınızdan emin olun.

     ```powershell  
          ApplicationIdentifier : S-1-5-21-1634563105-1224503876-2692824315-2120
          ClientId              :  
          Thumbprint            : 
          ApplicationName       : Azurestack-Yourapp-6967581b-497e-4f5a-87b5-0c8d01a9f146
          ClientSecret          : MKUNzeL6PwmlhWdHB59c25WDDZlJ1A6IWzwgv_Kn
          RunspaceId            : 6ed9f903-f1be-44e3-9fef-e7e0e3f48564
     ```

### <a name="remove-a-service-principal-for-ad-fs"></a>AD FS için bir hizmet sorumlusunu kaldırma

AD FS ile Azure Stack dağıttıysanız, hizmet sorumlusu silmek için PowerShell kullanabilirsiniz.

Betik ayrıcalıklı uç noktasından bir ERCS sanal makinede çalıştırılır.

#### <a name="parameters"></a>Parametreler

Aşağıdaki bilgiler gereklidir Otomasyon parametreler için giriş olarak:

|Parametre|Açıklama|Örnek|
|---------|---------|---------|
| Parametre | Açıklama | Örnek |
| ApplicationIdentifier | Benzersiz tanımlayıcı | S-1-5-21-1634563105-1224503876-2692824315-2119 |

> [!Note]  
> Var olan tüm hizmet sorumlularını ve bunların uygulama tanımlayıcısı listesini görüntülemek için get-graphapplication komutu kullanılabilir.

#### <a name="example-of-removing-the-service-principal-for-ad-fs"></a>AD FS için hizmet sorumlusu kaldırma örneği

```powershell  
     Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
     $Creds = Get-Credential

     # Creating a PSSession to the ERCS PrivilegedEndpoint
     $Session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $Creds

     $UpdateServicePrincipal = Invoke-Command -Session $Session -ScriptBlock {Remove-GraphApplication -ApplicationIdentifier S-1-5-21-1634563105-1224503876-2692824315-2119}

     $Session | Remove-PSSession
```

## <a name="assign-a-role"></a>Rol atama

Aboneliğinizdeki kaynaklara erişmek için uygulamaya bir rol atamanız gerekir. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir roller hakkında bilgi edinmek için [RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md).

Abonelik, kaynak grubu veya kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam için izinler devralınmıştır. Örneğin, bir kaynak grubu için okuyucu rolüne uygulamaya ekleme kaynak grubunu ve içerdiği tüm kaynakları okuyun anlamına gelir.

1. Azure Stack Portalı'nda, uygulamayı atamak istediğiniz kapsam düzeyini gidin. Örneğin abonelik kapsamında bir rol atamak için seçin **abonelikleri**. Bunun yerine, bir kaynak grubu veya kaynak seçebilirsiniz.

2. Belirli bir abonelikte (kaynak grubu veya kaynak) uygulama atamak için seçin.

     ![Abonelik atama için seçin](./media/azure-stack-create-service-principal/image16.png)

3. Seçin **erişim denetimi (IAM)**.

     ![erişim seçin](./media/azure-stack-create-service-principal/image17.png)

4. Seçin **rol ataması Ekle**.

5. Uygulamayı atamak istediğiniz rolü seçin.

6. Uygulamanız için arama yapın ve seçin.

7. Seçin **Tamam** rol atama tamamlanması. Bu kapsam için bir role atanmış kullanıcı listesinde uygulamanızı görürsünüz.

Bir hizmet sorumlusu oluşturuldu ve atanan role göre bu Azure Stack kaynaklara erişmek için uygulamanızı içinde kullanmaya başlayabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcıları AD FS’ye ekleme](azure-stack-add-users-adfs.md)  
[Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)  
[Azure Active Directory Belgeleri](https://docs.microsoft.com/azure/active-directory)  
[Active Directory Federasyon Hizmetleri](https://docs.microsoft.com/windows-server/identity/active-directory-federation-services)
