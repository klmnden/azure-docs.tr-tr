---
title: Azure Stack veri merkezi tümleştirmesi - kimlik
description: Azure Stack AD FS, veri merkezinizi AD FS ile tümleştirmeyi öğrenin
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 03/04/2019
ms.author: patricka
ms.reviewer: thoroet
ms.lastreviewed: 03/04/2019
ms.openlocfilehash: 14095d4ffbd23a57ef769aa702b6e7c3c8af9994
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485994"
---
# <a name="azure-stack-datacenter-integration---identity"></a>Azure Stack veri merkezi tümleştirmesi - kimlik
Kimlik sağlayıcısı Azure Stack, Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kullanarak dağıtabilirsiniz. Azure Stack dağıtmadan önce seçim yapmanız gerekir. Azure seçebileceğiniz bağlantılı bir senaryoda, AD veya AD FS. Bağlantısı kesilmiş bir senaryo için yalnızca AD FS desteklenir.

> [!IMPORTANT]
> Azure Stack çözümün tamamını yeniden dağıtmaya gerek kalmadan kimlik sağlayıcısı geçiş yapamazsınız.

## <a name="active-directory-federation-services-and-graph"></a>Active Directory Federasyon Hizmetleri ve grafik

AD FS ile dağıtma, Azure Stack kaynakları ile kimlik doğrulaması yapmak için var olan bir Active Directory ormanında kimlikleri sağlar. Bu mevcut Active Directory ormanı, bir AD FS federasyon güveni oluşturulmasına izin vermek için AD FS dağıtımını gerektirir.

Kimlik doğrulaması, kimlik, bir parçasıdır. Rol tabanlı erişim denetimi (RBAC) Azure Stack'te yönetmek için grafik bileşeni yapılandırılması gerekir. Bir kaynağa erişim yetkisi aktarıldığında LDAP protokolünü kullanarak mevcut Active Directory ormanındaki kullanıcı hesabını grafik bileşeni arar.

![Azure Stack AD FS mimarisi](media/azure-stack-integrate-identity/Azure-Stack-ADFS-architecture.png)

Var olan AD FS Azure yığını (Kaynak STS) AD FS talep gönderen hesabı güvenlik belirteci hizmeti (STS) ' dir. Otomasyon Azure Stack'te için var olan AD FS Talep sağlayıcı güveni olan meta veri uç noktası oluşturur.

Var olan AD FS bağlı olan taraf güveni yapılandırılması gerekir. Bu adım tarafından Otomasyon yapılmaz ve operatör tarafından yapılandırılması gerekir. AD FS için Azure Stack VIP uç nokta deseni kullanılarak oluşturulabilir. `https://adfs.<Region>.<ExternalFQDN>/`.

Bağlı olan taraf güveni yapılandırması Microsoft tarafından sağlanan talep dönüştürme kuralları yapılandırmanızı gerektirir.

Koşuluyla okuma izni var olan Active Directory Graph yapılandırma için bir hizmet hesabı olması gerekir. Bu hesap gerekli, RBAC senaryoları etkinleştirmek, otomasyon için giriş olarak.

Son adım için yeni bir sahip için varsayılan sağlayıcı aboneliği yapılandırılır. Bu hesap Azure Stack Yönetici portalında oturum açarken tüm kaynakların tam erişimi vardır.

Gereksinimler:

|Bileşen|Gereksinim|
|---------|---------|
|Graf|Microsoft Active Directory 2012/2012 R2/2016|
|AD FS|Windows Server 2012/2012 R2/2016|

## <a name="setting-up-graph-integration"></a>Graph entegrasyonuna ayarlama

Graf, yalnızca tek bir Active Directory ormanı ile tümleştirmeyi destekler. Birden çok ormanınız varsa, yalnızca yapılandırmada belirtilen orman kullanıcılar ve gruplar getirmek için kullanılır.

Otomasyon parametreleri için girdi olarak aşağıdaki bilgiler gereklidir:

|Parametre|Açıklama|Örnek|
|---------|---------|---------|
|CustomADGlobalCatalog|' % S'hedef Active Directory orman FQDN'si<br>ile tümleştirmek istediğiniz|contoso.com|
|CustomADAdminCredentials|LDAP okuma iznine sahip bir kullanıcı|YOURDOMAIN\graphservice|

### <a name="configure-active-directory-sites"></a>Active Directory sitelerini yapılandırma

Birden çok siteye sahip Active Directory dağıtımları için Azure Stack dağıtımınıza yakın Active Directory sitesi yapılandırın. Yapılandırma, Azure Stack Graph hizmeti uzak bir siteden bir genel katalog sunucusu kullanarak sorguları gidermek olan önler.

Azure Stack ekleme [genel VIP ağı](azure-stack-network.md#public-vip-network) alt ağ ile Azure Stack'e en yakın Active Directory sitesi. Örneğin, Seattle ve Redmond Seattle sitesinde dağıtılan Azure Stack ile iki site Active Directory'niz varsa Active Directory sitesine Seattle için Azure Stack genel VIP alt ağı eklersiniz.

Active Directory siteleri hakkında daha fazla bilgi için bkz. [site topolojisini tasarlama](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/designing-the-site-topology).

> [!Note]  
> Tek bir Site Active Directory'niz varsa bu adımı atlayabilirsiniz. Yapılandırılmış bir catch tüm alt ağı olması durumunda Azure Stack genel VIP alt ağı parçası olmadığını doğrulayın.

### <a name="create-user-account-in-the-existing-active-directory-optional"></a>Var olan Active Directory kullanıcı hesabı oluşturma (isteğe bağlı)

İsteğe bağlı olarak, mevcut Active Directory Graph hizmeti için bir hesap oluşturabilirsiniz. Kullanmak istediğiniz bir hesap zaten yoksa, bu adımı gerçekleştirin.

1. Var olan Active Directory'de (öneri) aşağıdaki kullanıcı hesabı oluşturun:
   - **Kullanıcı adı**: graphservice
   - **Parola**: güçlü bir parola kullanın<br>Parola süresi dolmayacak şekilde yapılandırın.

   Hiçbir özel izinler veya üyelik gereklidir.

#### <a name="trigger-automation-to-configure-graph"></a>Tetikleyici Otomasyon grafiği yapılandırmak için

Bu yordam için Azure Stack'te ayrıcalıklı uç noktası ile iletişim kurabilen veri merkezi ağınızı bir bilgisayar kullanın.

1. (Yönetici olarak çalıştır) yükseltilmiş Windows PowerShell oturumu açın ve ayrıcalıklı uç noktanın IP adresine bağlanın. Kimlik bilgilerini kullanan **CloudAdmin** kimliğini doğrulamak için.

   ```powershell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ayrıcalıklı uç noktasına bağlı olduğunuzdan, aşağıdaki komutu çalıştırın: 

   ```powershell  
   Register-DirectoryService -CustomADGlobalCatalog contoso.com
   ```

   İstendiğinde, Graph hizmeti (örneğin, graphservice) için kullanmak istediğiniz kullanıcı hesabının kimlik bilgilerini belirtin. Register-DirectoryService cmdlet'i için giriş, orman adı / ormandaki etki alanı yerine orman içindeki başka bir etki alanı kökü.

   > [!IMPORTANT]
   > Açılır kimlik için bekleyin (Get-Credential ayrıcalıklı uç noktasında desteklenmez) ve graf hizmet hesabı kimlik bilgilerini girin.

#### <a name="graph-protocols-and-ports"></a>Graf protokoller ve bağlantı noktaları

Graf hizmeti Azure Stack'te yazılabilir genel katalog sunucusu (GC) ve Active Directory ormanı hedefte oturum açma istekleri işleyebilmesi için Anahtar Dağıtım Merkezi (KDC) ile iletişim kurmak için aşağıdaki bağlantı noktaları ve protokolleri kullanır.

Azure Stack'te Graph hizmeti, Active Directory hedefi ile iletişim kurmak için aşağıdaki protokoller ve bağlantı noktaları kullanır:

|Type|Bağlantı noktası|Protokol|
|---------|---------|---------|
|LDAP|389|TCP VE UDP|
|LDAP SSL|636|TCP|
|LDAP GC|3268|TCP|
|LDAP GC SSL|3269|TCP|

## <a name="setting-up-ad-fs-integration-by-downloading-federation-metadata"></a>Federasyon meta verileri yükleyerek AD FS tümleştirmesini ayarlama

Aşağıdaki bilgiler gereklidir Otomasyon parametreler için giriş olarak:

|Parametre|Açıklama|Örnek|
|---------|---------|---------|
|CustomAdfsName|Talep sağlayıcı adı.<br>AD FS giriş sayfasında bu şekilde görünür.|Contoso|
|CustomAD<br>FSFederationMetadataEndpointUri|Federasyon meta veri bağlantısı| https:\//ad01.contoso.com/federationmetadata/2007-06/federationmetadata.xml |


### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Azure stack'teki Talep sağlayıcı güveni yapılandırmak için tetikleyici Otomasyon

Bu yordam için Azure Stack'te ayrıcalıklı uç noktası ile iletişim kurabilen bir bilgisayarı kullanın. Beklenen sertifika hesap tarafından kullanılan **AD STS FS** Azure yığını tarafından güvenilir.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve ayrıcalıklı uç noktasına bağlanın.

   ```powershell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ayrıcalıklı uç noktasına bağlı olduğunuzdan, ortamınız için uygun parametreleri kullanarak aşağıdaki komutu çalıştırın:

   ```powershell  
   Register-CustomAdfs -CustomAdfsName Contoso -CustomADFSFederationMetadataEndpointUri https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml
   ```

3. Ortamınız için uygun parametreleri kullanarak varsayılan sağlayıcı aboneliği sahibini güncelleştirmek için aşağıdaki komutu çalıştırın:

   ```powershell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

## <a name="setting-up-ad-fs-integration-by-providing-federation-metadata-file"></a>Federasyon meta veri dosyası sağlayarak AD FS tümleştirmesini ayarlama

Aşağıdaki koşullardan biri doğru olduğunda 1807 sürümünden başlayarak, bu yöntemi kullanın:

- Sertifika zinciri, tüm diğer uç noktalardan Azure Stack ile karşılaştırıldığında, AD FS için farklıdır.
- Azure yığını'nın AD FS örneğinden var olan AD FS sunucusuna hiçbir ağ bağlantısı yoktur.

Aşağıdaki bilgiler gereklidir Otomasyon parametreler için giriş olarak:


|Parametre|Açıklama|Örnek|
|---------|---------|---------|
|CustomAdfsName|Talep sağlayıcı adı. AD FS giriş sayfasında bu şekilde görünür.|Contoso|
|CustomADFSFederationMetadataFileContent|Meta veri içeriği|$using:federationMetadataFileContent|

### <a name="create-federation-metadata-file"></a>Federasyon meta veri dosyası oluşturma

Aşağıdaki yordam için hesap STS olur var olan AD FS dağıtımı, ağ bağlantısı olan bir bilgisayar kullanmanız gerekir. Ayrıca, gerekli sertifikaları yüklenmelidir.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve ortamınız için uygun parametreleri kullanarak aşağıdaki komutu çalıştırın:

   ```powershell  
    $url = "https://win-SQOOJN70SGL.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml"
    $webclient = New-Object System.Net.WebClient
    $webclient.Encoding = [System.Text.Encoding]::UTF8
    $metadataAsString = $webclient.DownloadString($url)
    Set-Content -Path c:\metadata.xml -Encoding UTF8 -Value $metadataAsString
   ```

2. Meta veri dosyası ayrıcalıklı uç noktası ile iletişim kurabilen bilgisayara kopyalayın.

### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Azure stack'teki Talep sağlayıcı güveni yapılandırmak için tetikleyici Otomasyon

Bu yordam için Azure Stack'te ayrıcalıklı uç noktasıyla iletişim kurabilir ve önceki adımda oluşturduğunuz meta veri dosyası için erişime sahip bir bilgisayar kullanın.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve ayrıcalıklı uç noktasına bağlanın.

   ```powershell  
   $federationMetadataFileContent = get-content c:\metadata.xml
   $creds=Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ayrıcalıklı uç noktasına bağlı olduğunuzdan, ortamınız için uygun parametreleri kullanarak aşağıdaki komutu çalıştırın:

    ```powershell
    Register-CustomAdfs -CustomAdfsName Contoso -CustomADFSFederationMetadataFileContent $using:federationMetadataFileContent
    ```

3. Ortamınız için uygun parametreleri kullanarak varsayılan sağlayıcı aboneliği sahibini güncelleştirmek için aşağıdaki komutu çalıştırın:

   ```powershell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

   > [!Note]  
   > Var olan AD FS (hesap STS) sertifikayı döndürdüğünüzde AD FS tümleştirmenin yeniden ayarlamanız gerekir. Meta veri uç noktasının erişilebilir olduğundan veya meta veri dosyası sağlayarak yapılandırılmış olsa bile, tümleştirmenin ayarlamanız gerekir.

## <a name="configure-relying-party-on-existing-ad-fs-deployment-account-sts"></a>Var olan AD FS dağıtımı (hesabı STS) üzerinde bağlı olan taraf yapılandırma

Microsoft, talep dönüştürme kuralları dahil olmak üzere bağlı olan taraf güveni yapılandıran bir betik sağlar. Betik kullanarak komutları el ile çalıştırırken isteğe bağlıdır.

Yardımcısı betikten indirebileceğiniz [Azure Stack Araçları](https://github.com/Azure/AzureStack-Tools/tree/vnext/DatacenterIntegration/Identity) GitHub üzerinde.

El ile komutları çalıştırmak karar verirseniz, aşağıdaki adımları izleyin:

1. Üzerinde veri merkezinizin AD FS örneğini veya grup üyesi (örneğin, c:\ClaimRules.txt kaydedilen) bir .txt dosyasına aşağıdaki içeriği kopyalayın:

   ```text
   @RuleTemplate = "LdapClaims"
   @RuleName = "Name claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"), query = ";userPrincipalName;{0}", param = c.Value);

   @RuleTemplate = "LdapClaims"
   @RuleName = "UPN claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"), query = ";userPrincipalName;{0}", param = c.Value);

   @RuleTemplate = "LdapClaims"
   @RuleName = "ObjectID claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"]
   => issue(Type = "http://schemas.microsoft.com/identity/claims/objectidentifier", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, Value = c.Value, ValueType = c.ValueType);

   @RuleName = "Family Name and Given claim"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
   => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname", "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"), query = ";sn,givenName;{0}", param = c.Value);

   @RuleTemplate = "PassThroughClaims"
   @RuleName = "Pass through all Group SID claims"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
   => issue(claim = c);

   @RuleTemplate = "PassThroughClaims"
   @RuleName = "Pass through all windows account name claims"
   c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
   => issue(claim = c);
   ```

2. Bu Windows Forms tabanlı kimlik doğrulaması için doğrulama extranet ve intranet etkinleştirilir. Önce doğrulama, aşağıdaki cmdlet'i çalıştırarak zaten etkin:

   ```powershell  
   Get-AdfsAuthenticationProvider | where-object { $_.name -eq "FormsAuthentication" } | select Name, AllowedForPrimaryExtranet, AllowedForPrimaryIntranet
   ```

    > [!Note]  
    > Windows tümleşik kimlik doğrulaması (desteklenen kullanıcı aracısı dizeleri olabilir, AD FS dağıtımı için eski WIA) en son istemcileri desteklemek üzere güncelleştirilmesi gerekebilir. Makalesinde WIA güncelleştirme hakkında daha fazla desteklenen kullanıcı aracısı dizeleri okuma [intranet form tabanlı kimlik doğrulamayı yapılandırma WIA desteklemeyen cihazlar için](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-intranet-forms-based-authentication-for-devices-that-do-not-support-wia).<br>Form tabanlı kimlik doğrulama İlkesi etkinleştirme adımları makalesinde belirtilmiştir [kimlik doğrulama ilkelerini yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-authentication-policies).

3. Bağlı olan taraf güveni eklemek için AD FS örneğini veya bir grup üyesi üzerinde aşağıdaki Windows PowerShell komutunu çalıştırın. AD FS uç noktasına güncelleştirdiğinizden emin olun ve 1. adımda oluşturduğunuz dosyanın üzerine gelin.

   **AD FS 2016 için**

   ```powershell  
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true -AccessControlPolicyName "Permit everyone" -TokenLifeTime 1440
   ```

   **AD FS 2012/2012 R2 için**

   ```powershell  
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true -TokenLifeTime 1440
   ```

   > [!IMPORTANT]  
   > AD FS MMC ek bileşenini Windows Server 2012 veya 2012 R2 AD FS kullanırken verme yetkilendirme kurallarını yapılandırmak için kullanmanız gerekir.

4. Azure Stack erişmek için Internet Explorer veya Microsoft Edge Tarayıcısı'ı kullandığınızda, belirteç bağlamaları yoksay gerekir. Aksi takdirde, oturum açma denemesi başarısız. AD FS örneğinizin veya bir grup üyesi, aşağıdaki komutu çalıştırın:

   > [!note]  
   > Bu adım, Windows Server 2012 veya 2012 R2 AD FS kullanırken geçerli değildir. Bu komut atla ve devam et ile tümleştirme güvenlidir.

   ```powershell  
   Set-AdfsProperties -IgnoreTokenBinding $true
   ```

## <a name="spn-creation"></a>SPN oluşturma

Kimlik doğrulaması için bir hizmet asıl adı (SPN) kullanımını zorunlu birçok senaryo vardır. Bazı örnekler şunlardır:

- Azure Stack AD FS dağıtımı ile CLI kullanımı
- Dağıtılan AD FS ile Azure Stack için System Center Yönetim Paketi
- Azure stack'teki AD FS ile dağıtırken kaynak sağlayıcıları
- Çeşitli uygulamalar
- Etkileşimli olmayan bir oturum açma gerektirir

> [!Important]  
> AD FS yalnızca etkileşimli oturum açma oturumları destekler. Otomatik bir senaryo için etkileşimli olmayan oturum açma ihtiyacınız varsa, bir SPN kullanmanız gerekir.

SPN oluşturma hakkında daha fazla bilgi için bkz. [AD FS için hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).


## <a name="troubleshooting"></a>Sorun giderme

### <a name="configuration-rollback"></a>Yapılandırma geri alma

Ortam, artık burada doğrulanabilir bir durumda bırakır bir hata oluşursa, bir geri alma seçeneği kullanılabilir.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki komutları çalıştırın:

   ```powershell  
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ardından, aşağıdaki cmdlet'i çalıştırın:

   ```powershell  
   Reset-DatacenterIntegrationConfiguration
   ```

   Geri alma eylemi çalıştırdıktan sonra tüm yapılandırma değişiklikleri geri alınır. Yalnızca kimlik doğrulaması ile yerleşik **CloudAdmin** kullanıcı mümkündür.

   > [!IMPORTANT]
   > Varsayılan sağlayıcı aboneliği özgün sahibi yapılandırmanız gerekir

   ```powershell  
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "azurestackadmin@[Internal Domain]"
   ```

### <a name="collecting-additional-logs"></a>Ek günlükleri toplama

Cmdlet'lerinden herhangi birini başarısız olursa kullanarak ek günlük toplayabilirsiniz `Get-Azurestacklogs` cmdlet'i.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki komutları çalıştırın:

   ```powershell  
   $creds = Get-Credential
   Enter-pssession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ardından, aşağıdaki cmdlet'i çalıştırın:

   ```powershell  
   Get-AzureStackLog -OutputPath \\myworstation\AzureStackLogs -FilterByRole ECE
   ```


## <a name="next-steps"></a>Sonraki adımlar

[Dış izleme çözümlerini tümleştirme](azure-stack-integrate-monitor.md)
