---
title: "Karma yapılandırmak için Azure Active Directory'ye katılmış cihazlarda nasıl | Microsoft Docs"
description: "Karma Azure Active Directory'ye katılmış cihazları yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: c43d6bcd62690fe41599888b06ee9828c8e40fc0
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="how-to-configure-hybrid-azure-active-directory-joined-devices"></a>Karma Azure Active Directory'ye katılmış cihazları yapılandırma

Azure Active Directory'de (Azure AD) ile cihaz yönetimi, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak aygıtlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olabilirsiniz. Daha fazla ayrıntı için bkz: [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md).

Şirket içi Active Directory ortamında varsa ve Azure AD etki alanına katılmış cihazlarınızı katılmasını istediğiniz, bu karma Azure AD alanına katılmış cihazları yapılandırarak gerçekleştirebilirsiniz. Konu ile ilgili adımları sağlar. 


## <a name="before-you-begin"></a>Başlamadan önce

Karma Azure AD alanına katılmış aygıtlar ortamınızda yapılandırmaya başlamadan önce kendiniz desteklenen senaryolar ve kısıtlamalar ile kazanmalısınız.  

Açıklamaları okunabilirliğini artırmak için bu konuda aşağıdaki terim kullanır: 

- **Windows geçerli aygıtları** -Windows 10 veya Windows Server 2016 çalıştıran etki alanına katılmış cihazlar için bu terim başvuruyor.
- **Windows alt düzey aygıtları** -bu terim tümüne başvuruyor **desteklenen** çalışan Windows 10 ne Windows Server 2016 etki alanına katılmış Windows cihazları.  


### <a name="windows-current-devices"></a>Geçerli Windows cihazları

- Windows masaüstü işletim sistemi çalıştıran cihazlar için Windows 10 Anniversary güncelleştirme (sürüm 1607) kullanılmasını öneririz veya sonraki bir sürümü. 
- Geçerli Windows cihazlarının kaydı **olan** parola karma eşitlemesi yapılandırmaları gibi Federasyon olmayan ortamlarda desteklenir.  


### <a name="windows-down-level-devices"></a>Windows alt düzey aygıtları

- Aşağıdaki Windows alt düzey aygıtları desteklenir:
    - Windows 8.1
    - Windows 7
    - Windows Server 2012 R2
    - Windows Server 2012
    - Windows Server 2008 R2
- Windows alt düzey aygıtları kaydını **olan** sorunsuz çoklu oturum açma ile federe olmayan ortamlarda desteklenen [Azure Active Directory sorunsuz çoklu oturum açma](https://aka.ms/hybrid/sso).
- Windows alt düzey aygıtları kaydını **değil** dolaşım profilleri kullanarak cihazları için desteklenir. Dolaşım profilleri veya ayarlarını FQDN'yi kullanıyorsanız, Windows 10 kullanın.



## <a name="prerequisites"></a>Ön koşullar

Karma Azure AD alanına katılmış cihazları, kuruluşunuzda etkinleştirmeye başlamadan önce Azure AD güncel bir sürümünü çalıştırdığından emin olmanız gerekir bağlanın.

Azure AD Connect:

- Bilgisayar hesabında şirket içi Active Directory (AD) ve Azure AD cihaz nesne arasındaki ilişkiyi tutar. 
- Başka bir aygıt etkinleştirir ilgili özellikler gibi iş için Windows Hello.



## <a name="configuration-steps"></a>Yapılandırma adımları

Karma Azure AD alanına katılmış aygıtlar çeşitli Windows cihaz platformları için yapılandırabilirsiniz. Bu konu, tüm normal yapılandırma senaryoları için gerekli adımları içerir.  

Senaryonuz için gerekli olan adımları özetini almak için aşağıdaki tabloyu kullanın:  



| Adımlar                                      | Windows geçerli ve parola karma eşitlemesi | Windows geçerli ve Federasyon | Alt düzey Windows |
| :--                                        | :-:                                    | :-:                            | :-:                |
| 1. adım: hizmet bağlantı noktası yapılandırma | ![İşaretli][1]                            | ![İşaretli][1]                    | ![İşaretli][1]        |
| 2. adım: talep verme kurma           |                                        | ![İşaretli][1]                    | ![İşaretli][1]        |
| 3. adım: Windows 10 cihazları etkinleştirme      |                                        |                                | ![İşaretli][1]        |
| Adım 4: Denetimi dağıtımı ve dağıtım     | ![İşaretli][1]                            | ![İşaretli][1]                    | ![İşaretli][1]        |
| 5. adım: alanına katılmamış aygıtlar doğrulayın          | ![İşaretli][1]                            | ![İşaretli][1]                    | ![İşaretli][1]        |



## <a name="step-1-configure-service-connection-point"></a>1. adım: hizmet bağlantı noktası yapılandırma

Hizmet bağlantı noktası (SCP) nesnesi, cihazlar tarafından kayıt sırasında Azure AD Kiracı bilgileri bulmak için kullanılır. Şirket içi Active Directory (AD), SCP nesnesi karma Azure AD alanına katılmış cihazlar için adlandırma bağlamı bölüm bilgisayarın ormanın yapılandırmada mevcut olmalıdır. Her ormanda yalnızca bir yapılandırma adlandırma bağlamında yoktur. Bir Çoklu orman Active Directory yapılandırması, hizmet bağlantı noktası etki alanına katılmış bilgisayarları içeren tüm ormanlarda mevcut olması gerekir.

Kullanabileceğiniz [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) ormanınızın yapılandırma adlandırma bağlamında almak üzere.  

Active Directory etki alanı adına sahip bir orman için *fabrikam.com*, yapılandırma adlandırma bağlamında değil:

`CN=Configuration,DC=fabrikam,DC=com`

Ormanınıza etki alanına katılmış cihazların otomatik kayıt için SCP nesnesi şu konumdadır:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Nasıl Azure AD Connect dağıttığınız bağlı olarak, SCP nesnesi zaten yapılandırılmış olabilir.
Nesne varlığını doğrulayın ve aşağıdaki Windows PowerShell Betiği kullanılarak bulma değerleri alabilirsiniz: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

**$Scp. Anahtar sözcükler** çıktı Azure AD Kiracı bilgileri örneğin gösterir:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Hizmet bağlantı noktası mevcut değilse, bunu çalıştırarak oluşturabilirsiniz `Initialize-ADSyncDomainJoinedComputerSync` Azure AD Connect sunucunuzda cmdlet'i. Kuruluş yönetici kimlik bilgileri, bu cmdlet'i çalıştırmak için gereklidir.  
Cmdlet:

- Azure AD Connect bağlı Active Directory ormanındaki hizmet bağlantı noktası oluşturur. 
- Belirtmenizi gerektirir `AdConnectorAccount` parametresi. Active Directory Bağlayıcısı hesabı Azure ad connect yapılandırılan hesap budur. 


Aşağıdaki komut dosyası cmdlet'ini kullanarak bir örnek gösterilmektedir. Bu komut `$aadAdminCred = Get-Credential` bir kullanıcı adı yazın gerektirir. Kullanıcı asıl adı (UPN) biçiminde kullanıcı adı sağlamanız gerekir (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

`Initialize-ADSyncDomainJoinedComputerSync` Cmdlet:

- Active Directory PowerShell modülü ve bir etki alanı denetleyicisinde çalışan Active Directory Web Hizmetleri Bel AD DS araçları kullanır. Active Directory Web Hizmetleri, Windows Server 2008 R2 çalıştıran etki alanı denetleyicilerinde ve üzerinde desteklenir.
- Yalnızca tarafından desteklenen **MSOnline PowerShell modülü sürümü 1.1.166.0**. Bu modül indirmek için bunu kullanın [bağlantı](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).   
- AD DS araçları yüklü değilse, `Initialize-ADSyncDomainJoinedComputerSync` başarısız olur.  AD DS araçları özellikleri - uzak sunucu yönetim araçları - Rol Yönetim Araçları altında Sunucu Yöneticisi üzerinden yüklenebilir.

Windows Server 2008 veya önceki sürümlerini çalıştıran etki alanı denetleyicileri için hizmet bağlantı noktası oluşturmak için aşağıdaki komut dosyası kullanın.

Bir Çoklu orman yapılandırması, hizmet bağlantı noktası bilgisayarları var olduğu her ormanda oluşturmak için aşağıdaki komut dosyasını kullanmanız gerekir:
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your AD configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC

    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()


## <a name="step-2-setup-issuance-of-claims"></a>2. adım: talep verme kurma

Bir Federasyon Azure AD yapılandırma cihazlar Active Directory Federasyon Hizmetleri (AD FS) üzerinde kullanır veya 3. taraf bir Federasyon Hizmeti için Azure AD kimlik doğrulaması yapmak için şirket içi. Cihazların Azure Active Directory cihaz kayıt Hizmeti'ne karşı (Azure DRS) kaydetmek için bir erişim belirteci almak için kimlik doğrulaması.

Windows geçerli cihazlar şirket içi Federasyon Hizmeti tarafından barındırılan tümleşik Windows kimlik etkin bir WS-Trust uç (1.3 veya 2005 sürümleri) kullanarak kimlik doğrulaması.

> [!NOTE]
> AD FS, ya da kullanırken **adfs/services/güven/13/windowstransport** veya **adfs/services/güven/2005/windowstransport** etkinleştirilmesi gerekir. Ayrıca Web kimlik doğrulaması Proxy kullanıyorsanız, bu uç noktası proxy yayımlanır emin olun. Hangi uç noktalarının altında AD FS Yönetim Konsolu aracılığıyla özelliğinin etkinleştirilip etkinleştirilmediğini **hizmet > uç noktaları**.
>
>AD FS, şirket içi Federasyon Hizmeti olarak yoksa, WS-Trust 1.3 veya 2005 uç noktalarının ve bunlar meta veri değişimi dosyası (MEX) aracılığıyla yayımlanır destekledikleri emin olmak için satıcınızın yönergeleri izleyin.

Aşağıdaki talep tamamlanması, cihaz kaydı için Azure DRS'tarafından alınan belirteç bulunmalıdır. Azure DRS Azure AD ile sonra yeni oluşturulan cihaz nesnesi bilgisayar hesabı ile şirket içi ilişkilendirmek için Azure AD Connect tarafından kullanılan bu bilgilerin bazıları, bir cihaz nesnesi oluşturun.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Birden fazla doğrulanan etki alanı adı varsa, bilgisayarlar için aşağıdaki talep sağlamanız gerekir:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Zaten bir İmmutableıd talep (örn., alternatif oturum açma kimliği) dağıttığınız, bilgisayarlar için karşılık gelen bir talep sağlamanız gerekir:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

Aşağıdaki bölümlerde hakkında bilgiler yer:
 
- Değerlerin her talep olmalıdır
- Nasıl bir tanımı gibi AD FS'de görüneceği

Tanımı, değerleri mevcut olup olmadığını veya bunları oluşturmanız gerekiyorsa doğrulamak için yardımcı olur.

> [!NOTE]
> Şirket içi Federasyon sunucunuz için AD FS kullanmıyorsanız, bu talepleri vermek için uygun bir yapılandırma oluşturmak için satıcınızın yönergeleri izleyin.

### <a name="issue-account-type-claim"></a>Sorunu hesap türü talep

**`http://schemas.microsoft.com/ws/2012/01/accounttype`**-Bu talep değerini içermelidir **DJ**, cihaz etki alanına katılmış bir bilgisayar olarak tanımlar. AD FS'de şuna benzer bir verme dönüştürme kural ekleyebilirsiniz:

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a>Bilgisayar hesabı içi objectGUID sorun

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**-Bu talebi içermelidir **objectGUID** şirket içi bilgisayar hesabı değeri. AD FS'de şuna benzer bir verme dönüştürme kural ekleyebilirsiniz:

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a>Bilgisayar hesabı içi objectSID sorun

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**-Bu talebi içermelidir **objectSID** şirket içi bilgisayar hesabı değeri. AD FS'de şuna benzer bir verme dönüştürme kural ekleyebilirsiniz:

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>Birden çok etki alanı adlarını Azure AD'de belirlediğinizde issuerID bilgisayar için sorun

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**-Herhangi bir şirket içi Federasyon Hizmeti ile (AD FS veya 3. taraf) bağlanma doğrulanmış etki alanı adlarını Tekdüzen Kaynak Tanımlayıcısı (URI) Bu talebi içermelidir belirteç veren. ' De AD FS, yukarıdaki olanlardan sonra olanları belirli sırayla görüneceği verme dönüştürme kuralları ekleyebilirsiniz. Lütfen açıkça kullanıcılar için kuralı vermek için bir kural gerekli olduğuna dikkat edin. Aşağıdaki kuralları, kullanıcı ve bilgisayar kimlik doğrulaması tanımlama ilk bir kuralı eklenir.

    @RuleName = "Issue account type with the value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


Yukarıdaki talep

- `<verified-domain-name>`Azure AD'de doğrulanmış etki alanı adlarınızı biri ile değiştirmek için gereken bir yer tutucudur. Örneğin, değer "http://contoso.com/adfs/services/trust/" =



Doğrulanmış etki alanı adları hakkında daha fazla ayrıntı için bkz: [bir özel etki alanı adını Azure Active Directory'ye ekleme](active-directory-add-domain.md).  
Doğrulanmış şirket etki alanlarının bir listesini almak için kullanabileceğiniz [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet'i. 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>Kullanıcılar için bir tane bulunduğunda (kimliği ayarlanmadan örneğin alternatif oturum açma) İmmutableıd bilgisayar için sorun

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**-Bu talep bilgisayarlar için geçerli bir değer içermesi gerekir. AD FS içinde verme dönüştürme kural aşağıdaki gibi oluşturabilirsiniz:

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a>AD FS verme dönüştürme kuralları oluşturmak için yardımcı kod

Aşağıdaki komut dosyası, yukarıda açıklanan kuralları dönüştürme verme oluşturulmasını ile yardımcı olur.

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with the value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

### <a name="remarks"></a>Açıklamalar 

- Bu komut dosyası kuralları için varolan kuralları ekler. Komut dosyası iki kez çalıştırmayın kuralları iki kez eklenebilir olduğundan. Karşılık gelen hiçbir kural bu talepleri (karşılık gelen koşullarda) için komut dosyası yeniden çalıştırmadan önce var olduğundan emin olun.

- (Azure AD portalı veya Get-MsolDomains cmdlet'i aracılığıyla gösterildiği gibi) birden çok doğrulanmış etki alanı adı varsa, değerini ayarlamak **$multipleVerifiedDomainNames** betikteki **$true**. Ayrıca Azure AD Connect veya başka yöntemler aracılığıyla oluşturulan tüm mevcut issuerid talep kaldırdığınızdan emin olun. Bu kural için örnek aşağıda verilmiştir:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Zaten yayımlandı durumunda bir **İmmutableıd** kullanıcı hesapları için talep, değerini **$immutableIDAlreadyIssuedforUsers** betikteki **$true**.

## <a name="step-3-enable-windows-down-level-devices"></a>Adım 3: Windows alt düzey aygıtları etkinleştirin

Bazı etki alanına katılmış aygıtlarınız Windows cihazları sürümlerdeki, gerekir:

- Kullanıcıların aygıtlarını kaydetmesini sağlamak için Azure AD içinde bir ilke ayarlayın.
 
- Şirket içi Federasyon hizmetini desteklemek için bir talep yapılandırma **tümleşik Windows kimlik doğrulaması (IWA)** cihaz kaydı için.
 
- Azure AD cihaz kimlik doğrulama uç noktası cihaz doğrulanırken sertifika istemleri önlemek için yerel Intranet bölgesine ekleyin.

### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Kullanıcıların aygıtlarını kaydetmesini sağlamak için Azure AD'de ilkesini ayarlama

Windows alt düzey cihazları kaydetmek için kullanıcıların Azure AD'de cihazları kaydetmek izin vermek için ayarı ayarlandığından emin olmanız gerekir. Azure Portal'da, bu ayarı altında bulabilirsiniz:

`Azure Active Directory > Users and groups > Device settings`
    
Aşağıdaki ilke ayarlamak **tüm**: **kullanıcıları Azure AD ile cihazlarını kaydetme**

![Cihaz kaydetme](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a>Şirket içi Federasyon hizmetini yapılandır 

Şirket içi Federasyon hizmetinizi veren desteklemelidir **authenticationmehod** ve **wiaormultiauthn** aşağıda gösterildiği gibi kodlanmış bir değer resouce_params parametresiyle tutan Azure AD bağlı olan taraf için kimlik doğrulama isteği alırken talepleri:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Böyle bir istek geldiğinde, şirket içi Federasyon Hizmeti tümleşik Windows kimlik doğrulaması kullanarak kullanıcı kimlik doğrulaması yapmalıdır ve başarılı üzerinde aşağıdaki iki talep kesmeniz gerekir:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

AD FS'de kimlik doğrulama yöntemini geçişleri üzerinden bir verme dönüştürme kuralı eklemeniz gerekir.  

**Bu kural eklemek için:**

1. AD FS yönetim konsolunda Git `AD FS > Trust Relationships > Relying Party Trusts`.
2. Microsoft Office 365 kimlik Platformu'na bağlı taraf güven nesnesi sağ tıklayın ve ardından **talep kurallarını Düzenle**.
3. Üzerinde **verme dönüştürme kuralları** sekmesine **Kuralı Ekle**.
4. İçinde **talep kuralı** şablonu listesinden **talepleri özel kural kullanarak Gönder**.
5. Seçin **sonraki**.
6. İçinde **talep kuralı adı** kutusuna **kimlik doğrulama yöntemi talep kuralı**.
7. İçinde **talep kuralı** kutusunda, aşağıdaki kural yazın:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. Değiştirildikten sonra Federasyon sunucunuzda aşağıdaki PowerShell komutunu yazın  **\<RPObjectName\>**  adıyla bağlı olan taraf nesnesi, Azure AD bağlı olan taraf güven nesnesi. Bu nesne genellikle adlı **Microsoft Office 365 kimlik Platformu'na**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a>Azure AD cihaz kimlik doğrulama uç noktası yerel Intranet bölgesine ekleyin

Sertifika önlemek için kayıt aygıtları kullanıcılar yerel Intranet bölgesine Internet Explorer'da aşağıdaki URL'yi eklemek için etki alanına katılmış cihazlar için bir ilke itme Azure AD kimlik doğrulaması sırasında ister:

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a>Adım 4: Denetimi dağıtımı ve dağıtım

Gerekli adımları tamamladıktan sonra etki alanına katılmış cihazları otomatik olarak Azure AD katılım hazırsınız:

- Windows 10 Anniversary güncelleştirmesi ve Windows Server 2016 çalıştıran tüm etki alanına katılmış cihazlar otomatik olarak kayıt aygıt adresindeki Azure AD ile yeniden başlatın veya kullanıcı oturum açma. 

- Yeni cihazların etki alanına katılma işlemi tamamlandıktan sonra aygıt yeniden başlatıldığında Azure AD ile kaydedin.

- Daha önce Azure AD olan cihaz kayıtlı (örneğin, Intune için) için geçiş "*etki alanına katılmış, AAD kayıtlı*"; Ancak, etki alanının normal akıştaki nedeniyle tüm cihazlar arasında tamamlamak bu işlem biraz zaman alabilir ve kullanıcı etkinliği.

### <a name="remarks"></a>Açıklamalar

- Windows 10 ve Windows Server 2016 etki alanına katılmış bilgisayarlar otomatik kaydını piyasaya sürümü denetlemek için Grup İlkesi nesnesini kullanabilirsiniz.

- Windows 10 Kasım 2015 güncelleştirmesi otomatik olarak birleştiren Azure AD ile **yalnızca** sunum Grup İlkesi nesnesi ayarlarsanız.

- Kullanıcılarınıza Windows alt düzey bilgisayarların, dağıttığınız bir [Windows Installer paketi](#windows-installer-packages-for-non-windows-10-computers) seçtiğiniz bilgisayarlara.

- Windows 8.1 etki alanına katılmış cihazlar için Grup İlkesi nesnesini itme olursa, bir birleştirme denenir; Ancak, kullanmanız önerilir [Windows Installer paketi](#windows-installer-packages-for-non-windows-10-computers) tüm Windows alt düzey aygıtları katılmak için. 

### <a name="create-a-group-policy-object"></a>Bir Grup İlkesi nesnesi oluşturma 

Windows geçerli bilgisayarların piyasaya sürümü denetlemek için dağıtmanız **etki alanına katılmış bilgisayarları cihaz olarak kaydetme** kaydetmek istediğiniz aygıtları için Grup İlkesi nesnesi. Örneğin, bir kuruluş birimi veya bir güvenlik grubu için ilke dağıtabilirsiniz.

**İlke ayarlamak için:**

1. Açık **Sunucu Yöneticisi'ni**ve ardından `Tools > Group Policy Management`.
2. Otomatik kaydı Windows geçerli bilgisayarların etkinleştirmek istediğiniz etki alanı için karşılık gelen etki alanı düğümüne gidin.
3. Sağ **Grup İlkesi nesneleri**ve ardından **yeni**.
4. Grup İlkesi nesnesi için bir ad yazın. Örneğin, * karma Azure AD birleştirme. 
5. **Tamam** düğmesine tıklayın.
6. Yeni Grup İlkesi nesneniz sağ tıklayın ve ardından **Düzenle**.
7. Git **Bilgisayar Yapılandırması** > **ilkeleri** > **Yönetim Şablonları** > **Windows bileşenleri** > **aygıt kaydı**. 
8. Sağ **etki alanına katılmış bilgisayarları cihaz olarak kaydetme**ve ardından **Düzenle**.
   
   > [!NOTE]
   > Bu Grup İlkesi şablonu, Grup İlkesi Yönetimi konsolunun önceki sürümlerden adlandırıldı. Konsol önceki bir sürümünü kullanıyorsanız, Git `Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`. 

7. Seçin **etkin**ve ardından **Uygula**.
8. **Tamam** düğmesine tıklayın.
9. Grup İlkesi nesnesini, bir konumla bağlayın. Örneğin, belirli bir kuruluş birimine olarak bağlayabilirsiniz. Otomatik olarak Azure AD ile katılmak bilgisayarları belirli güvenlik grubuna da bağlayabilirsiniz. Bu ilke tüm etki alanına katılmış Windows 10 ve Windows Server 2016 kuruluşunuzdaki bilgisayarlara atamak için Grup İlkesi nesnesini etki alanına bağlayın.

### <a name="windows-installer-packages-for-non-windows-10-computers"></a>Windows 10 bilgisayarları için Windows Installer paketleri

Federasyon ortamında Windows alt düzey bilgisayarları etki alanına katılmış katılmak için indirin ve bu Windows Installer (.msi) paketi İndirme Merkezi'nden yüklemek [Windows 10 bilgisayarlar için Microsoft çalışma alanına katılma](https://www.microsoft.com/en-us/download/details.aspx?id=53554) Sayfa.

Paketi, System Center Configuration Manager gibi yazılım dağıtım sistemi kullanarak dağıtabilirsiniz. Paket ile standart sessiz yükleme seçeneklerini destekler *sessiz* parametresi. System Center Configuration Manager geçerli dalının tamamlanmış kayıtlar izleme yeteneği gibi önceki sürümlerinden ek avantajlar sunar. Daha fazla bilgi için bkz: [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).

Yükleyici sistemde kullanıcının bağlamında çalışan zamanlanmış bir görev oluşturur. Kullanıcı Windows açtığında görevi tetiklenir. Görev sessizce aygıt tümleşik Windows kimlik doğrulaması kullanarak kimlik doğrulama sonra Azure AD kullanıcı kimlik bilgileri ile birleştirir. Aygıtta zamanlanmış görev görmek için Git **Microsoft** > **çalışma alanına katılma**ve Görev Zamanlayıcı Kitaplığı'na gidin.

## <a name="step-5-verify-joined-devices"></a>5. adım: alanına katılmamış aygıtlar doğrulayın

Kullanarak, kuruluşunuzda başarılı alanına katılmamış aygıtlar denetleyebilirsiniz [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet'te [Azure Active Directory PowerShell Modülü](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

Bu cmdlet'in çıktısı, kayıtlı ve Azure AD ile birleştirilmiş cihazları gösterir. Tüm cihazlar almak için kullanın **-tüm** parametresi ve bunları filtre kullanarak **deviceTrustType** özelliği. Etki alanına katılmış aygıtlar değerine sahip **etki alanına katılmış**.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
