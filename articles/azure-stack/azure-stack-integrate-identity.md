---
title: "Azure yığın datacenter tümleştirmesi - kimliği"
description: "Azure yığın AD FS ile veri merkezinizi AD FS tümleştirmek öğrenin"
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: article
ms.date: 12/12/2017
ms.author: mabrigg
keywords: 
<<<<<<< HEAD
ms.openlocfilehash: e43b9c7a854bc7150247a2b92d2d37ad6d74c705
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
=======
ms.openlocfilehash: 642ed3298eec0bab5515df117c0310786358e417
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="azure-stack-datacenter-integration---identity"></a>Azure yığın datacenter tümleştirmesi - kimliği

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kullanarak Azure yığın kimlik sağlayıcıları olarak dağıtabilirsiniz. Azure yığın dağıtmadan önce seçim yapmanız gerekir. AD FS kullanarak dağıtımı da bağlantısı kesilmiş modunda Azure yığın dağıtma olarak adlandırılır.

Aşağıdaki tabloda iki kimlik seçenekleri arasındaki farklar gösterilmektedir:

||Fiziksel olarak bağlantısı kesildi|Fiziksel olarak bağlı|
|---------|---------|---------|
|Faturalandırma|Kapasite olması gerekir<br> Yalnızca Kurumsal Anlaşma (EA)|Kapasite veya ödeme olarak-size-kullanımı<br>EA veya Bulut çözümü sağlayıcısı (CSP)|
|Kimlik|AD FS olmalıdır|Azure AD veya AD FS|
|Market dağıtım|Şu anda kullanılamıyor|Destekleniyor<br>KLG lisanslama|
|Kayıt|Önerilen, çıkarılabilir medya gerektirir<br> ve ayrı bağlı bir aygıt.|Otomatik|
|Düzeltme eki ve güncelleştirme|Gerekli, çıkarılabilir medya gerektirir<br> ve ayrı bağlı bir aygıt.|Güncelleştirme paketini doğrudan indirilebilir<br> Internet'ten Azure yığını.|

> [!IMPORTANT]
> Tüm Azure yığın çözümü yeniden dağıtmadan kimlik sağlayıcısı geçemezsiniz.

## <a name="active-directory-federation-services-and-graph"></a>Active Directory Federasyon Hizmetleri ve grafik

AD FS ile dağıtma kaynakları Azure yığınında kimlik doğrulaması yapmak için var olan bir Active Directory ormanında kimlikleri sağlar. Bu var olan Active Directory ormanı, bir AD FS federasyon güveni oluşturulmasına izin vermek için AD FS dağıtımını gerektirir.

Kimlik doğrulama kimliğinin bir parçasıdır. Rol tabanlı erişim denetimi (RBAC) Azure yığınında yönetmek için grafik bileşeninde yapılandırılmış olması gerekir. Bir kaynağa erişim yetkisi aktarıldığında LDAP protokolünü kullanarak var olan Active Directory ormanındaki kullanıcı hesabı grafik bileşeni arar.

![Azure AD FS yığın mimarisi](media/azure-stack-integrate-identity/Azure-Stack-ADFS-architecture.png)

Var olan AD FS talep Azure yığın AD FS'ye (Kaynak STS) gönderdiği hesabının güvenlik belirteci hizmeti (STS) değil. Azure yığınında Otomasyon Talep sağlayıcı güveni var olan AD FS için meta veri uç noktası oluşturur.

Var olan AD FS bağlı olan taraf güveni yapılandırılması gerekir. Bu adım tarafından Otomasyon belirtilmez ve operatör tarafından yapılandırılması gerekir. Azure yığın meta veri uç noktasının AzureStackStampDeploymentInfo.JSON dosyasında ya da ayrıcalıklı uç noktası aracılığıyla komutunu çalıştırarak belgelenen `Get-AzureStackInfo`.

Bağlı olan taraf güven yapılandırması Microsoft tarafından sağlanan talep dönüştürme kuralları yapılandırmanızı gerektirir.

Grafik yapılandırma için okuma izni olan Active Directory içinde olması koşuluyla, bir hizmet hesabı olması gerekir. Bu hesap gereklidir RBAC senaryoları etkinleştirmek Otomasyon için giriş olarak.

Son adım için yeni bir sahibi varsayılan sağlayıcı abonelik için yapılandırılır. Bu hesap Azure yığın Yönetici portalında oturum açıp tüm kaynaklara tam erişimi vardır.

Gereksinimleri:


|Bileşen|Gereksinim|
|---------|---------|
|Graf|Microsoft Active Directory 2012/2012 R2/2016|
|AD FS|Windows Server 2012/2012 R2/2016|

## <a name="setting-up-graph-integration"></a>Grafik tümleştirme ayarlama

Otomasyon parametre için girdi olarak aşağıdaki bilgiler gereklidir:


|Parametre|Açıklama|Örnek|
|---------|---------|---------|
|CustomADGlobalCatalog|Active Directory ormanı hedef FQDN'si<br>ile tümleştirmek istediğiniz|contoso.com|
|CustomADAdminCredentials|LDAP okuma izni olan bir kullanıcı|YOURDOMAIN\graphservice|

### <a name="create-user-account-in-the-existing-active-directory-optional"></a>Kullanıcı hesabı var olan Active Directory (isteğe bağlı) oluşturun

İsteğe bağlı olarak, var olan Active Directory Graph hizmeti için bir hesap oluşturabilirsiniz. Kullanmak istediğiniz bir hesap zaten yoksa bu adımı gerçekleştirin.

1. Var olan Active Directory içinde aşağıdaki kullanıcı hesabını (öneri) oluşturun:
   - **Kullanıcı adı**: graphservice
   - **Parola**: güçlü bir parola kullanın<br>Süresi dolmayacak parolayı yapılandırın.

   Hiçbir özel izinler veya üyelik gereklidir.

#### <a name="trigger-automation-to-configure-graph"></a>Tetikleyici Otomasyon grafik yapılandırmak için

Bu yordam için Azure yığınında ayrıcalıklı uç noktası ile iletişim kurabilir veri merkezi ağınızı bir bilgisayar kullanın.

2. (Yönetici olarak çalıştır) yükseltilmiş bir Windows PowerShell oturumu açın ve ayrıcalıklı uç noktası IP adresine bağlanın. Kimlik bilgilerini kullanmak **CloudAdmin** kimliğini doğrulamak için.

   ```powershell
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

3. Ayrıcalıklı uç noktasına bağlı değilseniz, aşağıdaki komutu çalıştırın: 

   ```powershell
   Register-DirectoryService -CustomADGlobalCatalog contoso.com
   ```

   İstendiğinde, Grafik Hizmeti (örneğin, graphservice) için kullanmak istediğiniz kullanıcı hesabının kimlik bilgilerini belirtin.

   > [!IMPORTANT]
   > Kimlik bilgileri için açılır bekleyin (Get-Credential ayrıcalıklı uç desteklenmez) ve grafik hizmet hesabı kimlik bilgilerini girin.

#### <a name="graph-protocols-and-ports"></a>Grafik protokoller ve bağlantı noktaları

Azure yığın grafik hizmetinde hedef Active Directory ile iletişim kurmak için aşağıdaki protokolleri ve bağlantı noktalarını kullanır:

|Tür|Bağlantı noktası|Protokol|
|---------|---------|---------|
|LDAP|389|TCP VE UDP|
|LDAP SSL|636|TCP|
|LDAP GC|3268|TCP|
|LDAP GC SSL|3269|TCP|

## <a name="setting-up-ad-fs-integration-by-downloading-federation-metadata"></a>Federasyon meta verileri yükleyerek AD FS tümleştirme ayarlama

Aşağıdaki bilgiler gereklidir Otomasyon parametreleri için giriş olarak:

|Parametre|Açıklama|Örnek|
|---------|---------|---------|
|CustomAdfsName|Talep sağlayıcı adı. <cr>AD FS giriş sayfasında bu şekilde görünür.|Contoso|
|CustomAD<br>FSFederationMetadataEndpointUri|Federasyon meta veri bağlantısı|https://ad01.contoso.com/federationmetadata/2007-06/federationmetadata.XML|


### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Talep sağlayıcı güveni Azure yığınında yapılandırmak için tetikleyici Otomasyon

Bu yordam için Azure yığınında ayrıcalıklı uç noktası ile iletişim kurabilen bir bilgisayar kullanın. Hesap tarafından kullanılan sertifikanın beklenir **STS AD FS** Azure yığını tarafından güvenilir.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve ayrıcalıklı uç noktasına bağlanın.

   ```powershell
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ayrıcalıklı uç noktasına bağlı değilseniz, ortamınız için uygun parametreleri kullanarak aşağıdaki komutu çalıştırın:

   ```powershell
   Register-CustomAdfs -CustomAdfsName Contoso -CustomADFSFederationMetadataEndpointUri https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml
   ```

3. Ortamınız için uygun parametrelerle varsayılan sağlayıcı aboneliğin sahibi güncelleştirmek için aşağıdaki komutu çalıştırın:

   ```powershell
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

## <a name="setting-up-ad-fs-integration-by-providing-federation-metadata-file"></a>Federasyon meta veri dosyası sağlayarak AD FS tümleştirme ayarlama

Aşağıdaki durumlardan herhangi biri doğruysa, bu yöntemi kullanın:

- Sertifika zinciri, tüm diğer uç noktalardan Azure yığın karşılaştırılan AD FS için farklıdır.
- Azure yığın AD FS örneğinden var olan AD FS sunucusuna ağ bağlantısı yok.

Aşağıdaki bilgiler gereklidir Otomasyon parametreleri için giriş olarak:


|Parametre|Açıklama|Örnek|
|---------|---------|---------|
|CustomAdfsName|Talep sağlayıcı adı. AD FS giriş sayfasında bu şekilde görünür.|Contoso|
|CustomADFSFederationMetadataFile|Federasyon meta veri dosyası|https://ad01.contoso.com/federationmetadata/2007-06/federationmetadata.XML|

### <a name="create-federation-metadata-file"></a>Federasyon meta veri dosyası oluşturma

Aşağıdaki yordam için hesap STS olur var olan AD FS dağıtımı ile ağ bağlantısına sahip bir bilgisayar kullanmanız gerekir. Ayrıca, gerekli sertifikaları yüklenmelidir.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve ortamınız için uygun parametreleri kullanarak aşağıdaki komutu çalıştırın:

   ```powershell
   [XML]$Metadata = Invoke-WebRequest -URI https://win-SQOOJN70SGL.contoso.com/federationmetadata/2007-06/federationmetadata.xml -UseBasicParsing

   $Metadata.outerxml|out-file c:\metadata.xml
   ```

2. Meta veri dosyası ayrıcalıklı uç noktasından erişilebilen bir paylaşıma kopyalayın.


### <a name="trigger-automation-to-configure-claims-provider-trust-in-azure-stack"></a>Talep sağlayıcı güveni Azure yığınında yapılandırmak için tetikleyici Otomasyon

Bu yordam için Azure yığınında ayrıcalıklı uç noktası ile iletişim kurabilen bir bilgisayar kullanın.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve ayrıcalıklı uç noktasına bağlanın.

   ```powershell
   $creds=Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ayrıcalıklı uç noktasına bağlı değilseniz, ortamınız için uygun parametreleri kullanarak aşağıdaki komutu çalıştırın:

   ```powershell
   Register-CustomAdfs -CustomAdfsName Contoso – CustomADFSFederationMetadataFile \\share\metadataexample.xml
   ```

3. Ortamınız için uygun parametrelerle varsayılan sağlayıcı aboneliğin sahibi güncelleştirmek için aşağıdaki komutu çalıştırın:

   ```powershell
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "administrator@contoso.com"
   ```

## <a name="configure-relying-party-on-existing-ad-fs-deployment-account-sts"></a>Var olan AD FS dağıtımı (hesap STS) bağlı olan taraf yapılandırmanız

Microsoft, talep dönüştürme kuralları dahil bağlı olan taraf güveni yapılandıran bir komut dosyası sağlar. Komut dosyası kullanarak komutları el ile çalışırken isteğe bağlıdır.

Yardımcı betikten indirebilirsiniz [Azure yığın Araçları](https://github.com/Azure/AzureStack-Tools/tree/vnext/DatacenterIntegration/Identity) github'da.

El ile komutları çalıştırmak karar verirseniz, aşağıdaki adımları izleyin:

1. Aşağıdaki içerik, merkezinin AD FS örneği veya grubu üye üzerinde (örneğin, c:\ClaimRules.txt kaydedilen) bir .txt dosyasına kopyalayın:

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
   c:[Type == http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname]
   => issue(claim = c);
   ```

2. Windows Forms tabanlı kimlik doğrulamasını etkinleştirmek için yükseltilmiş bir kullanıcı olarak bir Windows PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

   ```powershell
   Set-AdfsProperties -WIASupportedUserAgents @("MSAuthHost/1.0/In-Domain","MSIPC","Windows Rights Management Client","Kloud")
   ```

3. Bağlı olan taraf güveni eklemek için AD FS örneğini veya bir grup üyesi aşağıdaki Windows PowerShell komutunu çalıştırın. 1. adımda oluşturduğunuz dosya üzerine gelin ve AD FS uç noktasına güncelleştirdiğinizden emin olun.

   **AD FS 2016**

   ```powershell
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true -AccessControlPolicyName "Permit everyone"
   ```

   **AD FS 2012/2012 R2 için**

   ```powershell
   Add-ADFSRelyingPartyTrust -Name AzureStack -MetadataUrl "https://YourAzureStackADFSEndpoint/FederationMetadata/2007-06/FederationMetadata.xml" -IssuanceTransformRulesFile "C:\ClaimIssuanceRules.txt" -AutoUpdateEnabled:$true -MonitoringEnabled:$true -enabled:$true
   ```

   > [!IMPORTANT]
   > AD FS MMC ek bileşenini Windows Server 2012 veya 2012 R2 AD FS kullanırken verme yetkilendirme kurallarını yapılandırmak için kullanmanız gerekir.

4. Azure yığın erişmek için Internet Explorer veya Edge tarayıcısı kullandığınızda, belirteç bağlamaları yoksay gerekir. Aksi takdirde, oturum açma denemeleri başarısız. AD FS örneğini veya bir grup üyesi, aşağıdaki komutu çalıştırın:

   ```powershell
   Set-AdfsProperties -IgnoreTokenBinding $true
   ```

5. Yenileme belirteçleri etkinleştirmek için yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

   ```powershell
   Set-ADFSRelyingPartyTrust -TargetName AzureStack -TokenLifeTime 1440
   ```

## <a name="spn-creation"></a>SPN oluşturma

Kimlik doğrulaması için bir hizmet asıl adı (SPN) kullanılmasını gerektirir birçok senaryo vardır. Bazı örnekler şunlardır:

- AD FS dağıtımı Azure yığınının CLI kullanımı
- Azure AD FS ile dağıtıldığında yığını için System Center Yönetim Paketi
- Azure AD FS ile dağıtıldığında yığınında kaynak sağlayıcıları
- Çeşitli uygulamalar
- Etkileşimli olmayan oturum açma gerektirir

SPN oluşturma hakkında daha fazla bilgi için bkz: [AD FS için hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#create-service-principal-for-ad-fs).


## <a name="troubleshooting"></a>Sorun giderme

### <a name="configuration-rollback"></a>Yapılandırma geri alma

Ortamı, artık burada doğrulanabilir bir durumda bırakır bir hata oluşursa, bir geri alma seçeneği kullanılabilir.

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki komutları çalıştırın:

   ```powershell
   $creds = Get-Credential
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $creds
   ```

2. Ardından aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Reset-DatacenterIntegationConfiguration
   ```

   Geri alma eylemi çalıştırdıktan sonra tüm yapılandırma değişikliklerini geri alınır. Yalnızca yerleşik ile kimlik doğrulaması **CloudAdmin** kullanıcı mümkündür.

   > [!IMPORTANT]
   > Varsayılan sağlayıcı abonelik özgün sahibinin yapılandırmanız gerekir

   ```powershell
   Set-ServiceAdminOwner -ServiceAdminOwnerUpn "azurestackadmin@[Internal Domain]"
   ```

### <a name="collecting-additional-logs"></a>Ek günlükleri toplama

Cmdlet'lerinden herhangi birini başarısız olursa, kullanarak ek günlüklerini toplayabilir `Get-Azurestacklogs` cmdlet'i.

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

[Azure veri merkezi tümleştirme yığın - uç noktalarını yayımlama](azure-stack-integrate-endpoints.md)
