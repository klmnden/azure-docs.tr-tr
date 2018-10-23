---
title: Hibrit Azure Active Directory'ye katılmış cihazları elle yapılandır | Microsoft Docs
description: Hibrit Azure Active Directory'ye katılmış cihazları elle nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/25/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 4e3b7aff97cbcebe34e6af4755900e8888c5e57d
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49352812"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-joined-devices-manually"></a>Öğretici: Hibrit Azure Active Directory'ye katılmış cihazları elle yapılandırma 

Azure Active Directory’de (Azure AD) cihaz yönetimi ile, kullanıcılarınızın güvenlik ve uyumluluk açısından standartlarınızı karşılayan cihazlardan kaynaklarınıza eriştiğinden emin olabilirsiniz. Daha fazla bilgi için, bkz. [Azure Active Directory'de cihaz yönetimine giriş](overview.md).


> [!TIP]
> Azure AD Connect kullanma seçeneğine sahipseniz bkz. [Senaryonuzu seçin](hybrid-azuread-join-plan.md#select-your-scenario). Azure AD Connect özelliğini kullanarak Hibrit Azure AD'ye katılım yapılandırmasını önemli oranda basitleştirebilirsiniz.



Şirket içi Active Directory ortamınız varsa ve etki alanınıza katılmış cihazları Azure AD'ye katmak istiyorsanız hibrit Azure AD'ye katılmış cihazları yapılandırarak bunu gerçekleştirebilirsiniz. Bu öğreticide, cihazlarınız için hibrit Azure AD'ye katılımı elle nasıl yapılandıracağınızı öğrenirsiniz.

> [!div class="checklist"]
> * Ön koşullar
> * Yapılandırma adımları
> * Hizmet bağlantı noktasını yapılandırma
> * Talep verme Kurulumu
> * Windows alt düzey cihazlarını etkinleştirme
> * Katılmış cihazları doğrulama
> * Uygulamanızda sorun giderme
 




## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide, şu konularda bilgi sahibi olduğunuz varsayılır:
    
-  [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)
    
-  [Hibrit Azure Active Directory'ye katılma uygulamanızı planlama](hybrid-azuread-join-plan.md)

-  [Cihazlarınızın hibrit Azure AD'ye katılımını denetleme](hybrid-azuread-join-control.md)


Kuruluşunuzda hibrit Azure AD'ye katılmış cihazları etkinleştirmeye başlamadan önce mutlaka:

- Güncel Azure AD Connect sürümünün yüklü olduğundan emin olun.

- Azure AD Connect özelliğinin hibrit Azure AD'ye katılmasını istediğiniz cihazların bilgisayar nesnelerini Azure AD'ye eşitlediğinden emin olun. Bilgisayar nesneleri belirli kuruluş birimlerine (OU) aitse bu kuruluş birimlerinin Azure AD Connect'te eşitleme için de yapılandırılmış olması gerekir.

  

Azure AD Connect:

- Şirket içi Active Directory'nizdeki (AD) bilgisayar hesabı ile Azure AD'deki cihaz nesnesi arasındaki ilişkiyi korur. 
- İş için Windows Hello gibi diğer cihaz ile ilgili özellikleri etkinleştirir.

Azure AD'ye bilgisayarların kaydı için kuruluşunuzun ağındaki bilgisayarlardan aşağıdaki URL'lere erişilebildiğinden emin olun:

- https://enterpriseregistration.windows.net

- https://login.microsoftonline.com İzin Ver
- https://device.login.microsoftonline.com

- Kuruluşunuza ait STS (federasyon etki alanları)

Henüz yapılmadıysa kuruluşunuza ait STS'nin (federasyon etki alanları) kullanıcının yerel intranet ayarlarına dahil edilmesi gerekir.

Kuruluşunuz Sorunsuz SSO kullanmayı planlıyorsa aşağıdaki URL'lere kuruluşunuzdaki bilgisayarlardan erişilebilmelidir ve aynı zamanda kullanıcının yerel intranet bölgesine eklenmelidir:

- https://autologon.microsoftazuread-sso.com

- Ayrıca kullanıcının intranet bölgesinde aşağıdaki ayarların etkinleştirilebilmesi gereklidir: "Betikle durum çubuğu güncelleştirmelerine izin ver".

Kuruluşunuz şirket içi AD ile yönetilen (federasyon olmayan) kurulum kullanıyorsa ve Azure AD ile federasyon için ADFS kullanmıyorsa Windows 10'da hibrit Azure AD'ye katılım özelliği, AD'deki bilgisayar nesnelerinin Azure AD'ye eşitlenmesine dayanır. Hibrit Azure AD'ye katılması gereken bilgisayar nesnelerini içeren tüm Kuruluş Birimlerinin (OU) Azure Ad Connect eşitleme yapılandırmasında eşitlemeler için etkinleştirildiğinden emin olun.

1703 veya önceki sürümlereki Windows 10 cihazları için, kuruluşunuz giden bağlantı proxy'si aracılığıyla İnternete erişimi gerektiriyorsa Windows 10 bilgisayarlarını Azure AD'ye kayıt için etkinleştirmek üzere Web Proxy Otomatik Bulma (WPAD) uygulamanız gerekir. 

Windows 10 1803 sürümünden itibaren federasyon etki alanındaki bir cihaz tarafından AD FS kullanılarak gerçekleştirilen hibrit Azure AD katılma girişimi başarısız olsa dahi Azure AD Connect'in bilgisayar/cihaz nesnelerini Azure AD ile eşleştirecek şekilde yapılandırılmış olması durumunda cihaz eşitlenen bilgisayarı/cihazı kullanarak hibrit Azure AD katılımını tamamlamaya çalışacaktır.

## <a name="configuration-steps"></a>Yapılandırma adımları

Hibrit Azure AD'ye katılmış cihazları çeşitli türlerdeki Windows cihazı platformları için yapılandırabilirsiniz. Bu konu, tüm tipik yapılandırma senaryoları için gereken adımları içerir.  

Senaryonuz için gereken adımlara ilişkin genel bir bakış elde etmek için aşağıdaki tabloyu kullanın:  



| Adımlar                                      | Windows geçerli ve parola karması eşitleme | Windows geçerli ve federasyon | Windows alt düzey |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Hizmet bağlantı noktasını yapılandırma | ![İşaretli][1]                            | ![İşaretli][1]                    | ![İşaretli][1]        |
| Talep verme Kurulumu           |                                        | ![İşaretli][1]                    | ![İşaretli][1]        |
| Windows 10 olmayan cihazları etkinleştirme      |                                        |                                | ![İşaretli][1]        |
| Katılmış cihazları doğrulama          | ![İşaretli][1]                            | ![İşaretli][1]                    | ![İşaretli][1]        |



## <a name="configure-service-connection-point"></a>Hizmet bağlantı noktasını yapılandırma

Azure AD kiracı bilgilerini keşfetmek için kayıt sırasında cihazlarınız tarafından hizmet bağlantı noktası (SCP) nesnesi kullanılır. Şirket içi Active Directory'nizde (AD), bilgisayarın ormanındaki yapılandırma adlandırma bağlamı bölümünde hibrit Azure AD'ye katılmış cihazların SCP nesnesinin bulunması gerekir. Orman başına yalnızca bir yapılandırma adlandırma bağlamı bulunur. Çoklu ormanlı Active Directory yapılandırmasında, etki alanına katılmış bilgisayarları içeren tüm ormanlarda hizmet bağlantı noktası bulunmalıdır.

Ormanınızın yapılandırma adlandırma bağlamını almak için [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) cmdlet öğesini kullanabilirsiniz.  

Active Directory etki alanı adı *fabrikam.com* olan bir orman için yapılandırma adlandırma bağlamı:

`CN=Configuration,DC=fabrikam,DC=com`

Ormanınızda, etki alanına katılmış cihazların otomatik kaydına ilişkin SCP nesnesi şu konumdadır:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Azure AD Connect'i nasıl dağıttığınıza bağlı olarak SCP nesnesi yapılandırılmış olabilir.
Nesnenin varlığını doğrulayabilir ve aşağıdaki Windows PowerShell betiğini kullanan keşif değerlerini alabilirsiniz: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

**$scp.Keywords** çıktısı Azure AD kiracı bilgilerini gösterir, örneğin:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Hizmet bağlantı noktası mevcut değilse Azure AD Connect sunucunuzda `Initialize-ADSyncDomainJoinedComputerSync` cmdlet öğesini çalıştırarak oluşturabilirsiniz. Bu cmdlet öğesini çalıştırmak için kuruluş yöneticisi kimlik bilgileri gereklidir.  
cmdlet:

- Azure AD Connect'in bağlı olduğu Active Directory ormanında hizmet bağlantı noktasını oluşturur. 
- `AdConnectorAccount` parametresini belirtmenizi gerektirir. Bu, Azure AD Connect'teki Active Directory bağlayıcısı olarak yapılandırılan hesaptır. 


Aşağıdaki betikte, cmdlet kullanımına ilişkin bir örnek gösterilmektedir. Bu betikte, `$aadAdminCred = Get-Credential` bir kullanıcı adı yazmanızı gerektirir. Kullanıcı asıl adı (UPN) biçiminde kullanıcı adı sağlamanız gerekir (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

`Initialize-ADSyncDomainJoinedComputerSync` cmdlet:

- Bir etki alanı denetleyicisinde çalıştırılan Active Directory Web Hizmetlerine dayanan AD DS Araçlarını ve Active Directory PowerShell modülünü kullanır. Active Directory Web Hizmetleri Windows Server 2008 R2 ve sonraki sürümleri çalıştıran etki alanı denetleyicilerinde desteklenir.
- Yalnızca **MSOnline PowerShell modülü sürüm 1.1.166.0** ile desteklenir. Bu modülü indirmek için bu [bağlantıyı](https://msconfiggallery.cloudapp.net/packages/MSOnline/1.1.166.0/) kullanın.   
- AD DS araçları yüklü değilse `Initialize-ADSyncDomainJoinedComputerSync` başarısız olur.  AD DS araçları, Özellikler - Uzak Sunucu Yönetim Araçları - Rol Yönetim Araçları bölümünün altındaki Sunucu Yöneticisi aracılığıyla yüklenebilir.

Windows Server 2008 veya önceki sürümlerini çalıştıran etki alanı denetleyicileri için hizmet bağlantı noktası oluşturmak üzere aşağıdaki betiği kullanın.

Çoklu orman yapılandırmasında, bilgisayarların bulunduğu her ormanda hizmet bağlantı noktası oluşturmak için aşağıdaki betiği kullanmanız gerekir:
 
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

Yukarıdaki betikte

- `$verifiedDomain = "contoso.com"`, Azure AD'de doğrulanmış etki alanı adlarınızdan biri ile değiştirmeniz gereken yer tutucudur. Kullanabilmeniz için etki alanına sahip olmanız gerekecektir.

Doğrulanmış etki alanı adları hakkında daha fazla bilgi için, bkz. [Azure Active Directory'ye özel etki alanı adı ekleyin](../active-directory-domains-add-azure-portal.md).  
Doğrulanmış şirket etki alanlarınızın bir listesini edinmek için [Get-AzureADDomain](/powershell/module/Azuread/Get-AzureADDomain?view=azureadps-2.0) cmdlet öğesini kullanabilirsiniz. 

![Get-AzureADDomain](./media/hybrid-azuread-join-manual-steps/01.png)

## <a name="setup-issuance-of-claims"></a>Talep verme Kurulumu

Federasyon Azure AD yapılandırmasında, cihazlar Azure AD'ye kimlik doğrulaması yapmak için Active Directory Federasyon Hizmetleri (AD FS) veya 3. taraf şirket içi federasyon hizmetine güvenir. Cihazlar, Azure Active Directory Device Registration Service (Azure DRS) kullanarak kayıt için erişim belirteci almak üzere kimlik doğrulaması yapar.

Windows geçerli cihazları, Tümleşik Windows Kimlik Doğrulamasını kullanarak şirket içi federasyon hizmeti tarafından barındırılan aktif bir WS-Trust uç noktaya (1.3 veya 2005 sürümleri) kimlik doğrulaması yapar.

> [!NOTE]
> AD FS kullanırken **adfs/services/trust/13/windowstransport** veya **adfs/services/trust/2005/windowstransport** özelliğinin etkinleştirilmesi gerekir. Web Kimlik Doğrulaması Proxy'si kullanıyorsanız ayrıca bu uç noktanın proxy aracılığıyla yayımlandığından emin olun. **Hizmet > Uç Noktalar** bölümün altındaki AD FS yönetim konsolu aracılığıyla hangi uç noktaların etkinleştirildiğini görebilirsiniz.
>
>Şirket içi federasyon hizmetiniz olarak AD FS kullanmıyorsanız WS-Trust 1.3 veya 2005 uç noktalarını desteklediklerinden ve bunların Meta Veri Değişimi (MEX) dosyası aracılığıyla yayımlandıklarından emin olmak için satıcınızın talimatlarını uygulayın.

Cihaz kaydının tamamlanması için Azure DRS tarafından alınan belirteçte aşağıdaki taleplerin yer alması gerekir. Azure DRS bu bilgilerin bir kısmıyla Azure AD'de bir cihaz nesnesi oluşturur. Ardından Azure AD Connect, yeni oluşturulan cihaz nesnesi ile şirket içi bilgisayar hesabını ilişkilendirmek için bu bilgileri kullanır.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Birden fazla doğrulanmış etki alanı adınız varsa bilgisayarlar için aşağıdaki talebi sağlamanız gerekir:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Zaten bir ImmutableID talebi veriyorsanız (ör. alternatif oturum açma kimliği) bilgisayarlar için bir karşılık gelen talep sağlamanız gerekir:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

Aşağıdaki bölümlerde, şu konular hakkında bilgiler edinebilirsiniz:
 
- Her bir talebin içermesi gereken değerler
- AD FS'de bir tanım nasıl görünür?

Tanım, değerlerin mevcut olup olmadığını veya bunları oluşturmanızın gerekip gerekmediğini doğrulamanıza yardımcı olur.

> [!NOTE]
> Şirket içi federasyon sunucunuz için AD FS kullanmıyorsanız bu talepleri vermek üzere uygun yapılandırmayı oluşturmak için satıcınızın talimatlarını uygulayın.

### <a name="issue-account-type-claim"></a>Hesap türü talep verme

**`http://schemas.microsoft.com/ws/2012/01/accounttype`** - Bu talebin, cihazı etki alanına katılmış bilgisayar olarak tanımlayan bir **DJ** değeri içermesi gerekir. AD FS'de, aşağıdaki gibi görünen bir verme aktarım kuralı ekleyebilirsiniz:

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

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** - Bu talebin, şirket içi bilgisayar hesabına ilişkin **objectGUID** değerini içermesi gerekir. AD FS'de, aşağıdaki gibi görünen bir verme aktarım kuralı ekleyebilirsiniz:

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

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** - Bu talebin, şirket içi bilgisayar hesabına ilişkin **objectSid** değerini içermesi gerekir. AD FS'de, aşağıdaki gibi görünen bir verme aktarım kuralı ekleyebilirsiniz:

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

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>Azure AD'de birden çok doğrulanmış etki alanı adı olduğunda bilgisayarın issuerID değerini verme

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** - Bu talebin, belirteci veren şirket içi federasyon hizmeti (AD FS veya 3. taraf) ile bağlanan herhangi bir doğrulanmış etki alanı adına ait Tekdüzen Kaynak Tanımlayıcısı (URI) değerini içermesi gerekir. AD FS'de, yukarıdakilerden sonra aşağıda belirtilenlere benzer verme aktarım kuralları ekleyebilirsiniz. Kullanıcılar için kuralı açıkça vermek üzere bir kuralın gerekli olduğunu lütfen unutmayın. Aşağıdaki kurallarda, kullanıcıyı ve bilgisayar kimlik doğrulamasını tanımlayan bir ilk kural eklenmiştir.

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


Yukarıdaki talepte

- `<verified-domain-name>`, Azure AD'de doğrulanmış etki alanı adlarınızdan biri ile değiştirmeniz gereken yer tutucudur. Örneğin, Değer = "http://contoso.com/adfs/services/trust/"



Doğrulanmış etki alanı adları hakkında daha fazla bilgi için, bkz. [Azure Active Directory'ye özel etki alanı adı ekleyin](../active-directory-domains-add-azure-portal.md).  

Doğrulanmış şirket etki alanlarınızın bir listesini edinmek için [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet öğesini kullanabilirsiniz. 

![Get-MsolDomain](./media/hybrid-azuread-join-manual-steps/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>Kullanıcılar için bir adet mevcut olduğunda (ör. alternatif oturum açma kimliği ayarlıdır) bilgisayar için ImmutableID değerini verme

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** - Bu talebin, bilgisayarlar için geçerli bir değer içermesi gerekir. AD FS'de aşağıda şekilde verme aktarım kuralı oluşturabilirsiniz:

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

Aşağıdaki betik, yukarıda açıklanan verme aktarım kurallarını oluşturmanıza yardımcı olur.

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

- Bu betik, kuralları mevcut kurallara ekler. Kurallar kümesi iki kez ekleneceğinden betiği iki kez çalıştırmayın. Betiği yeniden çalıştırmadan önce bu talepler için hiçbir karşılık gelen kural olmadığından emin olun (karşılık gelen koşullar bölümünde).

- Çoklu doğrulanmış etki alanı adınız varsa (Azure AD portal veya Get-MsolDomains cmdlet aracılığıyla gösterildiği şekilde) betikteki **$multipleVerifiedDomainNames** değerini **$true** olarak ayarlayın. Ayrıca Azure AD Connect veya başka yollarla oluşturulmuş olabilecek tüm mevcut issuerıd taleplerini kaldırdığınızdan emin olun. Bu kural için bir örnek aşağıda verilmiştir:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Kullanıcı hesapları için zaten bir **ImmutableID** talebi verdiyseniz betikteki **$immutableIDAlreadyIssuedforUsers** değerini **$true** olarak ayarlayın.

## <a name="enable-windows-down-level-devices"></a>Windows alt düzey cihazlarını etkinleştirme

Bazı etki alanına katılmış cihazlar Windows alt düzey cihazlarıysa şunları gerçekleştirmeniz gerekir:

- Kullanıcıların cihazları kaydedebilmesini sağlamak için Azure AD'de bir ilke ayarlayın.
 
- Cihaz kaydı için **Tümleşik Windows Kimlik Doğrulaması (IWA)** özelliğini desteklemek üzere talepleri vermek için şirket içi federasyon hizmetinizi yapılandırın.
 
- Cihazın kimlik doğrulaması yapılırken sertifika istemlerinin engellenmesi için yerel İntranet bölgelerine Azure AD cihaz kimlik doğrulaması uç noktasını ekleyin.

### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Kullanıcıların cihazları kaydetmesini sağlamak için Azure AD'de ilke ayarlama

Windows alt düzey cihazlarını kaydetmek için, kullanıcıların Azure AD'de cihazları kaydedebilmesini sağlama ayarının ayarlandığından emin olmanız gerekir. Azure portal'da bu ayarları aşağıdaki bölümde bulabilirsiniz:

`Azure Active Directory > Users and groups > Device settings`
    
Aşağıdaki ilke **Tümü** olarak ayarlanmalıdır: **Kullanıcılar cihazlarını Azure AD ile kaydedebilir**

![Cihaz kaydetme](./media/hybrid-azuread-join-manual-steps/23.png)


### <a name="configure-on-premises-federation-service"></a>Şirket içi federasyon hizmetini yapılandırma 

Şirket içi federasyon hizmetinizin, aşağıda gösterildiği şekilde kodlanmış bir değer içeren resouce_params parametresini içeren Azure AD bağlı olan tarafa kimlik doğrulaması isteği alırken **authenticationmethod** ve **wiaormultiauthn** taleplerini vermeyi desteklemesi gerekir.

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Böyle bir istek geldiğinde şirket içi federasyon hizmetinin, Tümleşik Windows Kimlik Doğrulaması kullanarak kullanıcının kimliğini doğrulaması gerekir ve başarılı olduktan sonra aşağıdaki iki talebi vermelidir:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

AD FS'de kimlik doğrulama yöntemi ile geçen bir verme aktarım kuralı eklemeniz gerekir.  

**Bu kuralı eklemek için:**

1. AD FS yönetim konsolunda `AD FS > Trust Relationships > Relying Party Trusts` bölümüne gidin.
2. Microsoft Office 365 Identity Platform bağlı olan taraf güven nesnesine sağ tıklayın ve ardından **Talep Kurallarını Düzenle** seçeneğini belirleyin.
3. **Verme Aktarım Kuralları** sekmesinde, **Kural Ekle** seçeneğini belirleyin.
4. **Talep kuralı** şablon listesinde, **Talepleri Özel Kural Kullanarak Gönder** seçeneğini belirleyin.
5. **İleri**’yi seçin.
6. **Talep kuralı adı** kutusuna **Kimlik Doğrulama Yöntemi Talep Kuralı** yazın.
7. **Talep kuralı** kutusuna aşağıdaki kuralı yazın:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. Federasyon sunucunuzda, **\<RPObjectName\>** öğesini Azure AD bağlı olan taraf güven nesnenizin bağlı olan taraf nesne adı ile değiştirdikten sonra aşağıdaki PowerShell komutunu yazın. Bu nesne genellikle **Microsoft Office 365 Identity Platform** olarak adlandırılır.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a>Azure AD cihaz kimlik doğrulaması uç noktasını Yerel İntranet bölgelerine ekleyin

Kayıt cihazlarındaki kullanıcılar Azure AD'ye kimlik doğrulaması yaparken sertifika istemlerini engellemek için, Internet Explorer'da Yerel İntranet bölgesine aşağıdaki URL'yi eklemek üzere etki alanına katılmış cihazlarınıza bir ilke gönderebilirsiniz:

`https://device.login.microsoftonline.com`


## <a name="verify-joined-devices"></a>Katılmış cihazları doğrulama

[Azure Active Directory PowerShell modülünde](/powershell/azure/install-msonlinev1?view=azureadps-2.0) [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet öğesini kullanarak kuruluşunuzda başarıyla katılmış cihazları kontrol edebilirsiniz.

Bu cmdlet öğesinin çıktısı, Azure AD ile kaydedilmiş ve katılmış cihazları gösterir. Tüm cihazları almak için **-Tümü** parametresini kullanın ve ardından **deviceTrustType** özelliğini kullanarak filtreleyebilirsiniz. Etki alanına katılmış cihazlar **Etki Alanına Katıldı** değerini içerir.



## <a name="troubleshoot-your-implementation"></a>Uygulamanızda sorun giderme

Etki alanına katılmış Windows cihazları için hibrit Azure AD'ye katılımı tamamlama sırasında sorun yaşıyorsanız:

- [Windows geçerli cihazları için Hibrit Azure AD'ye katılım sorunlarını giderme](troubleshoot-hybrid-join-windows-current.md)
- [Windows alt düzey cihazları için Hibrit Azure AD'ye katılım sorunlarını giderme](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'de cihaz yönetimine giriş](overview.md)



<!--Image references-->
[1]: ./media/hybrid-azuread-join-manual-steps/12.png
