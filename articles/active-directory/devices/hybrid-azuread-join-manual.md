---
title: Hibrit Azure Active Directory'ye katılmış cihazları elle yapılandır | Microsoft Docs
description: Hibrit Azure Active Directory'ye katılmış cihazlarda el ile yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: joflore
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: e3944031101b954c380b5db6503002dae57ea16d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60352430"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-joined-devices-manually"></a>Öğretici: Hibrit Azure Active Directory'ye katılmış cihazlarda el ile yapılandırma 

Azure Active Directory'de (Azure AD) ile cihaz yönetimi, kullanıcıların kaynaklarınıza güvenlik ve uyumluluğa yönelik standartlarınızı karşılayan cihazlardan eriştiklerini emin olabilirsiniz. Daha fazla bilgi için [Azure Active Directory'de cihaz yönetimine giriş](overview.md).


> [!TIP]
> Azure AD Connect kullanarak sizin için bir seçenek ise, ilgili öğretici için bkz. [yönetilen](hybrid-azuread-join-managed-domains.md) veya [Federasyon](hybrid-azuread-join-federated-domains.md) etki alanları. Azure AD Connect kullanarak hibrit Azure AD'ye katılma yapılandırmasını önemli ölçüde basitleştirebilir.

Şirket içi Active Directory ortamınız varsa ve etki alanınıza katılmış cihazları Azure AD'ye katmak istiyorsanız hibrit Azure AD'ye katılmış cihazları yapılandırarak bunu gerçekleştirebilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * El ile hibrit Azure AD'ye katılım'ı yapılandırma
> * Hizmet bağlantı noktasını yapılandırma
> * Talep verme ayarlayın
> * Windows alt düzey cihazlarını etkinleştirme
> * Katılmış cihazları doğrulama
> * Uygulamanızda sorun giderme
 




## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, alışık olduğunuz varsayılır:
    
-  [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)    
-  [Hibrit Azure Active Directory join uygulamanızı planlama](hybrid-azuread-join-plan.md)
-  [Cihazlarınızın hibrit Azure AD katılımını denetleme](hybrid-azuread-join-control.md)


Kuruluşunuzdaki hibrit Azure AD'ye katılmış cihazların etkinleştirme başlamadan önce emin olun:

- Azure AD Connect'in güncel bir sürümünü çalıştırıyorsunuz.
- Azure AD Connect cihazların hibrit Azure AD'ye Azure AD'ye katılmış olmasını istediğiniz bilgisayar nesnelerinin eşitlendi. Bilgisayar nesnelerinin belirli kuruluş birimine (OU) aitse, bu OU'lar için de Azure AD Connect eşitlemesi yapılandırılmış olması gerekir.

  

Azure AD Connect:

- Şirket içi Active Directory Örneğinizde bilgisayar hesabını ve Azure AD'de cihaz nesnesi arasındaki ilişkiyi tutar. 
- Gibi Windows iş için Hello diğer cihazlarla ilgili özellikleri etkinleştirir.

Aşağıdaki URL'ler Azure AD'ye bilgisayarların kayıt için kuruluşunuzun ağ içinde bilgisayarlardan erişilebilir olduğundan emin olun:

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- Kullanıcının yerel intranet ayarları dahil kuruluşunuzun STS (Federasyon etki alanları için),

Sorunsuz çoklu oturum açmayı kullanmak, kuruluşunuzun planlıyorsa, aşağıdaki URL kuruluşunuz içinde bilgisayarlardan erişilebilir olması gerekir. Ayrıca kullanıcının yerel intranet bölgesine eklenmelidir.

- https://autologon.microsoftazuread-sso.com

Ayrıca, kullanıcının intranet bölgesinde aşağıdaki ayarı etkinleştirilmelidir: "Durum çubuğu komut güncelleştirmelerine izin ver."

Kuruluşunuz kullanıyorsa yönetilen (Federasyon olmayan) kurulum ile şirket içi Active Directory ve Azure AD ile federasyona eklemek için Active Directory Federasyon Hizmetleri (AD FS) kullanmaz ve hibrit Azure AD'ye katılım'ı Windows 10 bilgisayar nesnesi Active kullanır Azure AD ile eşitlenmesi için dizin. Herhangi bir OU'da, hibrit Azure AD'ye katılmış olması gereken nesnelerin Azure AD Connect eşitleme yapılandırmasını eşitleme için etkin bilgisayar içerdiğinden emin olun.

Kuruluşunuzda bir giden proxy üzerinden internet erişimi gerekiyorsa, sürüm 1703 veya önceki Windows 10 cihazları için Web Proxy Otomatik Bulma (WPAD), Azure AD'ye kaydettirmek Windows 10 bilgisayarlarını etkinleştirmek için uygulamalıdır. 

Bir AD FS ile Federasyon etki alanındaki bir cihaz hibrit Azure AD'ye katılma denemede başarısız olsa bile ve Azure AD Connect için yapılandırılmışsa, Windows 10 1803'ile başlayan bilgisayar/cihaz nesneleri Azure AD'ye eşitleme, cihaz hibrit Azure AD'ye katılımı tarafımızca tamamlamak dener İng eşitlenmiş bilgisayar/cihaz.

## <a name="verify-configuration-steps"></a>Yapılandırma adımları doğrulayın

Hibrit Azure AD'ye katılmış cihazları çeşitli türlerdeki Windows cihazı platformları için yapılandırabilirsiniz. Bu konu, tüm tipik yapılandırma senaryoları için gereken adımları içerir.  

Senaryonuz için gereken adımlara ilişkin genel bir bakış elde etmek için aşağıdaki tabloyu kullanın:  



| Adımlar                                      | Windows geçerli ve parola karması eşitleme | Windows geçerli ve federasyon | Windows alt düzey |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Hizmet bağlantı noktasını yapılandırma | ![İşaretli][1]                            | ![İşaretli][1]                    | ![İşaretli][1]        |
| Talep verme ayarlayın           |                                        | ![İşaretli][1]                    | ![İşaretli][1]        |
| Windows 10 olmayan cihazları etkinleştirme      |                                        |                                | ![İşaretli][1]        |
| Katılmış cihazları doğrulama          | ![İşaretli][1]                            | ![İşaretli][1]                    | ![İşaretli][1]        |



## <a name="configure-a-service-connection-point"></a>Hizmet bağlantı noktasını yapılandırma

Cihazlarınızı, Azure AD Kiracı bilgileri bulmak için kayıt sırasında bir hizmet bağlantı noktası (SCP) nesnesini kullanın. Şirket içi Active Directory Örneğinizde SCP nesne hibrit Azure AD'ye katılmış cihazlar için bilgisayarın ormanın bağlam bölüm adlandırma yapılandırmada mevcut olmalıdır. Orman başına yalnızca bir yapılandırma adlandırma bağlamı bulunur. Bir çok ormanlı Active Directory yapılandırması, etki alanına katılmış bilgisayarları içeren tüm ormanlarda hizmet bağlantı noktası mevcut olmalıdır.

Ormanınızın yapılandırma adlandırma bağlamını almak için [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) cmdlet öğesini kullanabilirsiniz.  

Active Directory etki alanı adı *fabrikam.com* olan bir orman için yapılandırma adlandırma bağlamı:

`CN=Configuration,DC=fabrikam,DC=com`

Ormanınızda, etki alanına katılmış cihazların otomatik kaydına ilişkin SCP nesnesi şu konumdadır:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Nasıl Azure AD Connect dağıttığınız bağlı olarak, SCP nesnesi zaten yapılandırılmış olabilir.
Nesnesinin varlığını doğrulamak ve aşağıdaki Windows PowerShell betiğini kullanarak bulma değerlerini alma: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

**$Scp. Anahtar sözcükler** çıktı Azure AD Kiracı bilgileri gösterir. Bir örneği aşağıda verilmiştir:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Hizmet bağlantı noktası mevcut değilse Azure AD Connect sunucunuzda `Initialize-ADSyncDomainJoinedComputerSync` cmdlet öğesini çalıştırarak oluşturabilirsiniz. Kuruluş Yöneticisi kimlik bilgileri, bu cmdlet'i çalıştırmak için gereklidir.  

cmdlet:

- Azure AD Connect bağlı Active Directory ormanında hizmet bağlantı noktası oluşturur. 
- `AdConnectorAccount` parametresini belirtmenizi gerektirir. Bu hesap, Azure AD CONNECT'te Active Directory Bağlayıcısı hesabı olarak yapılandırılır. 


Aşağıdaki betikte, cmdlet kullanımına ilişkin bir örnek gösterilmektedir. Bu betikte, `$aadAdminCred = Get-Credential` bir kullanıcı adı yazmanızı gerektirir. Kullanıcı asıl adı (UPN) biçiminde kullanıcı adı sağlamanız gerekir (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

`Initialize-ADSyncDomainJoinedComputerSync` cmdlet:

- Active Directory PowerShell modülü ve Azure Active Directory etki alanı Hizmetleri (Azure AD DS) araçlarını kullanır. Bu araçlar, bir etki alanı denetleyicisinde çalışan Active Directory Web Hizmetleri dayanır. Active Directory Web Hizmetleri Windows Server 2008 R2 ve sonraki sürümleri çalıştıran etki alanı denetleyicilerinde desteklenir.
- Yalnızca MSOnline PowerShell modülü sürüm 1.1.166.0 tarafından desteklenir. Bu modül indirmek için kullanın [bu bağlantıyı](https://msconfiggallery.cloudapp.net/packages/MSOnline/1.1.166.0/).   
- Azure AD DS araçları yüklü değilse `Initialize-ADSyncDomainJoinedComputerSync` başarısız olur. Azure AD DS Araçları altında Sunucu Yöneticisi aracılığıyla yükleyebilirsiniz **özellikleri** > **Uzak Sunucu Yönetim Araçları** > **rol yönetim araçları** .

Windows Server 2008 veya önceki sürümlerini çalıştıran etki alanı denetleyicileri için hizmet bağlantı noktası oluşturmak için aşağıdaki betiği kullanın. Bir Çoklu orman yapılandırması, hizmet bağlantı noktası bilgisayarları bulunduğu her ormanda oluşturmak için aşağıdaki betiği kullanın.
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your Active Directory configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC
    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()

Önceki kodda `$verifiedDomain = "contoso.com"` bir yer tutucudur. Azure AD'de doğrulanmış etki alanı adlarınızı biri ile değiştirin. Kullanabilmeniz için önce etki alanına sahip olması.

Doğrulanmış etki alanı adları hakkında daha fazla bilgi için bkz. [Azure Active Directory'ye özel etki alanı adı ekleme](../active-directory-domains-add-azure-portal.md). 

Doğrulanmış şirket etki alanlarınızın bir listesini edinmek için [Get-AzureADDomain](/powershell/module/Azuread/Get-AzureADDomain?view=azureadps-2.0) cmdlet öğesini kullanabilirsiniz. 

![Şirket etki alanlarının listesi](./media/hybrid-azuread-join-manual/01.png)

## <a name="set-up-issuance-of-claims"></a>Talep verme ayarlayın

Federasyon Azure AD yapılandırması cihazları kullanan AD FS veya bir Microsoft iş ortağı, Azure AD'ye kimlik doğrulaması için bir şirket içi Federasyon Hizmeti'nden. Cihazlar, Azure Active Directory Device Registration Service (Azure DRS) kullanarak kayıt için erişim belirteci almak üzere kimlik doğrulaması yapar.

Windows cihazları, şirket içi Federasyon Hizmeti tarafından barındırılan bir etkin WS-Trust uç (1.3 veya 2005 sürümleri) tümleşik Windows kimlik doğrulamasını kullanarak kimlik doğrulaması.

> [!NOTE]
> Kullanırken AD FS ya da **adfs/services/güven/13/windowstransport** veya **adfs/services/güven/2005/windowstransport** etkinleştirilmesi gerekir. Ayrıca Web kimlik doğrulaması Ara sunucusu kullanıyorsanız, bu uç nokta proxy'si aracılığıyla yayımlandığından emin olun. Hangi uç noktaları altında AD FS Yönetim Konsolu aracılığıyla etkinleştirilir gördüğünüz **hizmet** > **uç noktaları**.
>
>Şirket içi Federasyon hizmetinizin AD FS yoksa, satıcınızdan WS-Trust 1.3 veya 2005 uç noktaları ve bunlar meta veri değişimi dosyası (MEX) aracılığıyla yayımlanan destekledikleri emin olmak için yönergeleri izleyin.

Cihaz kaydı tamamlamak aşağıdaki talep Azure DRS alır bir belirteç bulunmalıdır. Azure DRS, bu bilgilerin bazıları ile Azure AD'de bir cihaz nesnesi oluşturacak. Azure AD Connect, sonra yeni oluşturulan bir cihaz nesnesi bilgisayar hesabı şirket içi ile ilişkilendirmek için bu bilgileri kullanır.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Birden fazla doğrulanmış etki alanı adınız varsa bilgisayarlar için aşağıdaki talebi sağlamanız gerekir:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Zaten bir Immutableıd talebi (örneğin, alternatif bir oturum açma kimliği) veriyorsanız, bilgisayarlar için karşılık gelen bir talep vermeniz gerekir:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

Aşağıdaki bölümlerde, şu konular hakkında bilgiler edinebilirsiniz:
 
- Her talep olması gereken değerleri.
- Hangi bir tanımı, AD FS'de gibi görünür.

Tanım, değerlerin mevcut olup olmadığını veya bunları oluşturmanızın gerekip gerekmediğini doğrulamanıza yardımcı olur.

> [!NOTE]
> Şirket içi federasyon sunucunuz için AD FS kullanmıyorsanız bu talepleri vermek üzere uygun yapılandırmayı oluşturmak için satıcınızın talimatlarını uygulayın.

### <a name="issue-account-type-claim"></a>Hesap türü talep verme

`http://schemas.microsoft.com/ws/2012/01/accounttype` Talep değerini içermelidir **DJ**, cihaz etki alanına katılmış bir bilgisayar olarak tanımlar. AD FS'de, aşağıdaki gibi görünen bir verme aktarım kuralı ekleyebilirsiniz:

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

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a>Şirket içi bilgisayar hesabının objectGUID değerini verme

`http://schemas.microsoft.com/identity/claims/onpremobjectguid` Talep içermelidir **objectGUID** şirket içi bilgisayar hesabının değeri. AD FS'de, aşağıdaki gibi görünen bir verme aktarım kuralı ekleyebilirsiniz:

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
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a>Şirket içi bilgisayar hesabının objectSID değerini verme

`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid` Talep içermelidir **objectSID** şirket içi bilgisayar hesabının değeri. AD FS'de, aşağıdaki gibi görünen bir verme aktarım kuralı ekleyebilirsiniz:

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

### <a name="issue-issuerid-for-the-computer-when-multiple-verified-domain-names-are-in-azure-ad"></a>Birden çok doğrulanmış etki alanı adlarını Azure AD'de olduğunda bilgisayarın İssuerıd sorun

`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid` Talep Tekdüzen Kaynak Tanımlayıcısı (URI) herhangi bir şirket içi Federasyon Hizmeti (AD FS veya iş ortağı) ile bağlanan doğrulanmış etki alanı adlarını içermelidir belirteci verilirken. AD FS'de, önceki ayarlara sonra bu belirli bir sırayla aşağıdaki sorguyu görünmesi verme dönüştürme kuralları ekleyebilirsiniz. Kullanıcılar için kural açıkça vermek için bu kurallardan biri gerekli olduğunu unutmayın. Aşağıdaki kuralları'nda, bilgisayar kimlik doğrulaması ve kullanıcı tanımlayan bir ilk kuralı eklenir.

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


Önceki talep `<verified-domain-name>` bir yer tutucudur. Azure AD'de doğrulanmış etki alanı adlarınızı biri ile değiştirin. Örneğin, `Value = "http://contoso.com/adfs/services/trust/"`.



Doğrulanmış etki alanı adları hakkında daha fazla bilgi için bkz. [Azure Active Directory'ye özel etki alanı adı ekleme](../active-directory-domains-add-azure-portal.md).  

Doğrulanmış şirket etki alanlarınızın bir listesini edinmek için [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet öğesini kullanabilirsiniz. 

![Şirket etki alanlarının listesi](./media/hybrid-azuread-join-manual/01.png)

### <a name="issue-immutableid-for-the-computer-when-one-for-users-exists-for-example-an-alternate-login-id-is-set"></a>Kullanıcılar için birer (örneğin, alternatif bir oturum açma kimliği ayarlanır) mevcut olduğunda bilgisayar için Immutableıd sorunu

`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID` Talep bilgisayarlar için geçerli bir değer içermesi gerekir. AD FS'de aşağıda şekilde verme aktarım kuralı oluşturabilirsiniz:

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

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a>AD FS verme aktarım kuralları oluşturmak üzere yardımcı betik

Aşağıdaki betiği oluşturulmasıyla verme dönüştürme kuralları daha önce açıklanan yardımcı olur.

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

#### <a name="remarks"></a>Açıklamalar 

- Bu betik, kuralları mevcut kurallara ekler. Kural kümesinin iki kez eklenmesi çünkü betiği iki kez çalıştırmayın. Betiği yeniden çalıştırmadan önce bu talepler için hiçbir karşılık gelen kural olmadığından emin olun (karşılık gelen koşullar bölümünde).

- Birden çok doğrulanmış etki alanı adı varsa (Azure AD portalında veya aracılığıyla gösterildiği **Get-MsolDomain** cmdlet'i), değerini ayarlamak **$multipleVerifiedDomainNames** betikteki **$true** . Ayrıca var olan tüm kaldırdığınızdan emin olun **issuerıd** talep başka bir yolla aracılığıyla ya da Azure AD Connect tarafından oluşturulmuş olabilir. Bu kural için bir örnek aşağıda verilmiştir:

      c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Kullanıcı hesapları için zaten bir **ImmutableID** talebi verdiyseniz betikteki **$immutableIDAlreadyIssuedforUsers** değerini **$true** olarak ayarlayın.

## <a name="enable-windows-down-level-devices"></a>Windows alt düzey cihazlarını etkinleştirme

Bazı etki alanına katılmış cihazlar Windows alt düzey cihazlarıysa şunları gerçekleştirmeniz gerekir:

- Kullanıcıların cihazları kaydedebilmesini sağlamak için Azure AD'de bir ilke ayarlayın. 
- Tümleşik Windows kimlik doğrulaması (IWA) için cihaz kaydını desteklemek için talepleri vermek için şirket içi Federasyon hizmetini yapılandırın. 
- Azure AD cihaz kimlik doğrulama uç noktası, cihaz kimlik doğrulaması yapılırken sertifika istemleri önlemek için yerel intranet bölgesine ekleyin.
- Windows alt düzey cihazları denetler. 


### <a name="set-a-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Kullanıcıların aygıtlarını kaydetmesini sağlamak için Azure AD'de bir ilke ayarlama

Windows alt düzey cihazları kaydetmek için kullanıcıların cihazları Azure AD'ye kaydetme ayarı etkin olduğundan emin olun. Azure portalında bu ayarı altında bulabilirsiniz **Azure Active Directory** > **kullanıcılar ve gruplar** > **cihaz ayarları**.
    
Aşağıdaki ilke ayarlanmalıdır **tüm**: **Kullanıcıların cihazlarını Azure AD'ye kaydetme**.

![Kullanıcıların aygıtlarını kaydetmesini sağlayan tüm düğmesi](./media/hybrid-azuread-join-manual/23.png)


### <a name="configure-the-on-premises-federation-service"></a>Şirket içi Federasyon hizmetini yapılandırın 

Şirket içi Federasyon hizmetinizin verme desteklemelidir **authenticationmethod** ve **wiaormultiauthn** tutan Azure AD bağlı olan taraf için kimlik doğrulama isteği aldığında, talepleri bir Aşağıdaki kodlanmış değer parametresiyle resource_params:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Böyle bir istek geldiğinde, şirket içi Federasyon Hizmeti kullanıcı tümleşik Windows kimlik doğrulamasını kullanarak kimlik doğrulaması gerekir. Federasyon Hizmeti, kimlik doğrulaması başarılı olduğunda, aşağıdaki iki talep yayımlamanız gerekir:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

AD FS'de kimlik doğrulama yöntemiyle geçirmeden bir verme dönüştürme kuralı eklemeniz gerekir. Bu kural eklemek için:

1. AD FS yönetim konsolunda Git **AD FS** > **güven ilişkileri** > **bağlı olan taraf güvenleri**.
2. Microsoft Office 365 Identity Platform bağlı olan taraf güven nesnesine sağ tıklayın ve ardından **Talep Kurallarını Düzenle** seçeneğini belirleyin.
3. **Verme Aktarım Kuralları** sekmesinde, **Kural Ekle** seçeneğini belirleyin.
4. **Talep kuralı** şablon listesinde, **Talepleri Özel Kural Kullanarak Gönder** seçeneğini belirleyin.
5. **İleri**’yi seçin.
6. İçinde **talep kuralı adı** kutusuna **kimlik doğrulama yöntemi talep kuralı**.
7. İçinde **talep kuralı** kutusunda, aşağıdaki kural girin:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. Federasyon sunucunuzda aşağıdaki PowerShell komutunu girin. Değiştirin **\<RPObjectName\>** ile bağlı olan taraf nesne adı, Azure AD bağlı olan taraf güven nesnesi. Bu nesne genellikle **Microsoft Office 365 Identity Platform** olarak adlandırılır.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-endpoint-to-the-local-intranet-zones"></a>Azure AD cihaz kimlik doğrulama uç noktası yerel intranet bölgesine ekleyin.

Kayıtlı cihazların kullanıcıları Azure AD'ye kimlik doğrulaması sırasında sertifika istemleri önlemek için yerel intranet bölgesine Internet Explorer'da aşağıdaki URL'yi eklemek için etki alanına katılmış cihazlar için bir ilke gönderebilirsiniz:

`https://device.login.microsoftonline.com`


### <a name="control-windows-down-level-devices"></a>Windows alt düzey cihazlarını denetleme 

Windows alt düzey cihazlarını kaydetmek için İndirme Merkezi’nden bir Windows Installer paketi (.msi) indirip yüklemeniz gerekir. Daha fazla bilgi için [cihazlarınızı hibrit Azure AD'ye katılma denetim](hybrid-azuread-join-control.md#control-windows-down-level-devices). 



## <a name="verify-joined-devices"></a>Katılmış cihazları doğrulama

Kullanarak kuruluşunuzda başarıyla alanına katılmış cihazlar için denetleyebilirsiniz [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet'inde [Azure Active Directory PowerShell Modülü](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

Bu cmdlet öğesinin çıktısı, Azure AD ile kaydedilmiş ve katılmış cihazları gösterir. Tüm cihazlar almak için kullanın **-tüm** parametresi ve bunları kullanarak filtre **deviceTrustType** özelliği. Etki alanına katılmış cihazlar değerine sahip **etki alanına katılmış**.



## <a name="troubleshoot-your-implementation"></a>Uygulamanızda sorun giderme

Etki alanına katılmış Windows cihazlar için hibrit Azure AD'ye katılma tamamlama konusunda sorun yaşıyorsanız, bkz:

- [Windows geçerli cihazları için Hibrit Azure AD'ye katılım sorunlarını giderme](troubleshoot-hybrid-join-windows-current.md)
- [Windows alt düzey cihazları için Hibrit Azure AD'ye katılım sorunlarını giderme](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'de cihaz yönetimine giriş](overview.md)



<!--Image references-->
[1]: ./media/hybrid-azuread-join-manual/12.png
