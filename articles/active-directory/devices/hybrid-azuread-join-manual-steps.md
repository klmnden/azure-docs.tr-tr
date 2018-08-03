---
title: Hibrit Azure Active Directory'ye katılmış cihazlarda el ile yapılandırmanız | Microsoft Docs
description: Hibrit Azure Active Directory'ye katılmış cihazlarda el ile yapılandırmayı öğrenin.
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
ms.topic: article
ms.date: 08/02/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 2ee54ca3d6e787267010736343a570e614c4204d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427559"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-joined-devices-manually"></a>Öğretici: Azure Active Directory'ye katılmış cihazlarda karma el ile yapılandırma 

İle cihaz Yönetimi Azure Active Directory'de (Azure AD) güvenlik ve uyumluluğa yönelik standartlarınızı karşılayan cihazlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olabilirsiniz. Daha fazla ayrıntı için [Azure Active Directory'de cihaz yönetimine giriş](overview.md).

Bir şirket içi Active Directory ortamınız varsa ve etki alanına katılan cihazları Azure AD'ye istiyorsanız, bunu hibrit Azure AD'ye katılmış cihazları yapılandırarak gerçekleştirebilirsiniz. Bu makalede ile ilgili adımları sağlar. 



> [!TIP]
> Azure AD Connect kullanarak sizin için bir seçenek olup olmadığını, [senaryonuzu seçin](hybrid-azuread-join-plan.md#select-your-scenario). Azure AD Connect kullanarak hibrit Azure AD'ye katılma yapılandırmasını önemli ölçüde basitleştirebilir.


## <a name="before-you-begin"></a>Başlamadan önce

Hibrit Azure AD'ye katılmış cihazları yapılandırma ortamınıza başlamadan önce desteklenen senaryolar ve kısıtlamalar ile planladığınızdan.  

Bağlı, [Sistem Hazırlama Aracı'nı (Sysprep)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc721940(v=ws.10)), bir Azure AD ile kaydedilmemiş Windows yüklemesinden görüntüleri oluşturduğunuz emin olun.

Tüm etki alanına katılmış cihazlar Azure AD'ye cihaz yeniden başlatma veya kullanıcı ile çalışan Windows 10 Yıldönümü güncelleştirmesi ve Windows Server 2016'ın otomatik olarak kaydedilecek aşağıda belirtilen yapılandırma adımları tamamlandıktan sonra oturum. **Bu otomatik yazmaç davranışı tercih edilen değilse ya da denetimli bir şekilde kademeli isteniyorsa**, Lütfen önce seçerek etkinleştirin veya önce otomatik dağıtımı devre dışı bırakmak için aşağıdaki "Adım 4: denetim dağıtım ve piyasaya çıkma" bölümündeki yönergeleri izleyin başka yapılandırma adımları izleyerek.  

Açıklamaları okunabilirliğini geliştirmek için bu makalede aşağıdaki terimi kullanılmaktadır: 

- **Windows cihazları** -Windows 10 veya Windows Server 2016 çalıştıran etki alanına katılmış cihazlar için bu terimi anlamına gelir.
- **Windows alt düzey cihazları** -bu terim tümüne başvuruyor **desteklenen** çalışan Windows 10 ne Windows Server 2016 etki alanına katılmış Windows cihazlar.  

### <a name="windows-current-devices"></a>Windows cihazları

- Windows masaüstü işletim sistemini çalıştıran cihazlar için desteklenen sürüm Windows 10 Yıldönümü Güncelleştirmesi (sürüm 1607) olan veya üzeri. 
- Geçerli Windows cihazların kaydını **olduğu** parola karması eşitleme yapılandırması gibi Federasyon olmayan ortamlarda desteklenir.  


### <a name="windows-down-level-devices"></a>Windows alt düzey cihazları

- Aşağıdaki Windows alt düzey cihazlar desteklenir:
    - Windows 8.1
    - Windows 7
    - Windows Server 2012 R2
    - Windows Server 2012
    - Windows Server 2008 R2
- Windows alt düzey cihazların kaydını **olduğu** sorunsuz çoklu oturum açma aracılığıyla Federasyon olmayan ortamlarda desteklenen [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start). 
- Windows alt düzey cihazların kaydını **değil** sorunsuz çoklu oturum açma olmadan Azure AD geçişli kimlik doğrulaması kullanılırken desteklenir.
- Windows alt düzey cihazların kaydını **değil** dolaşım profilleri kullanan cihazlar için desteklenir. Gezici profilleri veya ayarlarını FQDN'yi kullanıyorsanız, Windows 10 kullanın.



## <a name="prerequisites"></a>Önkoşullar

Kuruluşunuzdaki hibrit Azure AD'ye katılmış cihazların etkinleştirme başlamadan önce emin olmanız gerekir:

- Azure AD güncel bir sürümünü çalıştırdığınızı bağlanın.

- Azure AD connect cihazların hibrit Azure AD'ye Azure AD'ye katılmış olmasını istediğiniz bilgisayar nesnelerinin eşitlendi. Bilgisayar nesnelerinin belirli kuruluş birimine (OU) aitse bu OU'ları Azure AD'de eşitleme için yapılandırılması gereken sonra da bağlanın.

  

Azure AD Connect:

- Şirket içi Active Directory (AD) ve'ndaki bilgisayar hesabının Azure AD'de cihaz nesnesi arasındaki ilişkiyi tutar. 
- Diğer cihaz sağlar ilgili özellikler gibi Windows iş için Hello.

Aşağıdaki URL'ler Azure AD'ye kuruluş ağınızdaki bilgisayarların kayıt bilgisayarlardan erişilebilir olduğundan emin olun:

- https://enterpriseregistration.windows.net

- https://login.microsoftonline.com İzin ver
- https://device.login.microsoftonline.com

- Kuruluşunuzun STS (Federasyon etki alanları)

Yapmadıysanız, kuruluşunuzun STS (için Federasyon etki alanları) kullanıcının yerel intranet ayarları eklenmelidir.

Kuruluşunuz sorunsuz çoklu oturum açma kullanmayı planlıyorsanız, aşağıdaki URL'ler kuruluşunuz içinde bilgisayarlardan erişilebilmesi gereken ve kullanıcının yerel intranet bölgesine de eklenmelidir:

- https://autologon.microsoftazuread-sso.com

- Ayrıca, kullanıcının Intranet Bölgesi'nde aşağıdaki ayarı etkinleştirilmelidir: "durum çubuğu komut güncelleştirmelerine izin ver."

Kuruluşunuz yönetilen Kurulum (Federasyon olmayan) kullanıyorsa, şirket içi AD ve ADFS Azure AD ile federasyona eklemek için kullanmaz hibrit Azure AD'ye katılma Windows 10 Azure AD'ye sync'ed olması için ad bilgisayar nesneler üzerinde kullanır ardından. Tüm kurumsal birimlerde (hibrit Azure AD'ye katılmış olması gereken bilgisayar nesnelerini içeren OU) için etkin olması, Azure AD Connect eşitleme yapılandırmasını eşitleme emin olun.

Kuruluşunuzda bir giden proxy üzerinden Internet erişimi gerekiyorsa, sürüm 1703 veya önceki Windows 10 cihazları için Web Proxy Otomatik Bulma (WPAD), Azure AD'ye kaydettirmek Windows 10 bilgisayarlarını etkinleştirmek için uygulamalıdır. 

## <a name="configuration-steps"></a>Yapılandırma adımları

Hibrit Azure AD'ye katılmış cihazlar çeşitli türlerdeki Windows cihaz platformları için yapılandırabilirsiniz. Bu konu, tüm tipik yapılandırma senaryoları için gerekli olan adımları içerir.  

Senaryonuz için gerekli olan adımları genel bakışını almak için aşağıdaki tabloyu kullanın:  



| Adımlar                                      | Windows geçerli ve parola karması eşitleme | Windows geçerli ve Federasyon | Windows alt düzey |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Hizmet bağlantı noktasını yapılandırma | ![İşaretli][1]                            | ![İşaretli][1]                    | ![İşaretli][1]        |
| Talep verme Kurulumu           |                                        | ![İşaretli][1]                    | ![İşaretli][1]        |
| Windows 10 cihazları etkinleştirme      |                                        |                                | ![İşaretli][1]        |
| Denetim dağıtım ve sunum     | ![İşaretli][1]                            | ![İşaretli][1]                    | ![İşaretli][1]        |
| Katılan cihazlar doğrulayın          | ![İşaretli][1]                            | ![İşaretli][1]                    | ![İşaretli][1]        |



## <a name="configure-service-connection-point"></a>Hizmet bağlantı noktasını yapılandırma

Hizmet bağlantı noktası (SCP) nesnesine, cihazlarınız tarafından Azure AD Kiracı bilgileri bulmak için kayıt sırasında kullanılır. Şirket içi Active Directory'nizde (AD), SCP nesne hibrit Azure AD'ye katılmış cihazlar için bilgisayarın ormanın bağlam bölüm adlandırma yapılandırmada bulunmalıdır. Her ormanda yalnızca bir yapılandırma adlandırma bağlamında yoktur. Bir çok ormanlı Active Directory yapılandırması, etki alanına katılmış bilgisayarları içeren tüm ormanlardaki hizmet bağlantı noktası mevcut olmalıdır.

Kullanabileceğiniz [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) , ormanının yapılandırma adlandırma bağlamında almak için cmdlet'i.  

Active Directory etki alanı adına sahip bir orman için *fabrikam.com*, yapılandırma adlandırma bağlamında olan:

`CN=Configuration,DC=fabrikam,DC=com`

Ormanda etki alanına katılmış cihazların otomatik kaydı için SCP'yi nesnesi şu konumdadır:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Nasıl Azure AD Connect dağıttığınız bağlı olarak, SCP nesnesi zaten yapılandırılmış olabilir.
Nesnesinin varlığını doğrulamak ve aşağıdaki Windows PowerShell betiğini kullanarak bulma değerlerini alma: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

**$Scp. Anahtar sözcükler** çıkış gösteren Azure AD Kiracı bilgileri, örneğin:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Hizmet bağlantı noktası mevcut değilse, bunu çalıştırarak oluşturabilirsiniz `Initialize-ADSyncDomainJoinedComputerSync` Azure AD Connect sunucunuzda cmdlet'i. Bu cmdlet'i çalıştırmak için Kurumsal yönetici kimlik bilgileri gereklidir.  
Cmdlet:

- Azure AD Connect bağlı Active Directory ormanındaki hizmet bağlantı noktası oluşturur. 
- Belirtmenizi gerektirir `AdConnectorAccount` parametresi. Active Directory Bağlayıcısı hesabı Azure ad connect yapılandırıldığı hesap budur. 


Aşağıdaki betiği cmdlet'i kullanmak için bir örnek gösterilmektedir. Bu betikte `$aadAdminCred = Get-Credential` bir kullanıcı adı gerekir. Kullanıcı asıl adı (UPN) biçiminde kullanıcı adı sağlamanız gerekir (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

`Initialize-ADSyncDomainJoinedComputerSync` Cmdlet:

- Bir etki alanı denetleyicisinde çalışan Active Directory Web hizmetleri kullanan AD DS araçları ve Active Directory PowerShell modülü kullanır. Active Directory Web Hizmetleri Windows Server 2008 R2 çalıştıran etki alanı denetleyicilerinde ve üzerinde desteklenir.
- Yalnızca tarafından desteklenen **MSOnline PowerShell modülü sürüm 1.1.166.0**. Bu modül indirmek için bunu kullanın [bağlantı](https://msconfiggallery.cloudapp.net/packages/MSOnline/1.1.166.0/).   
- AD DS araçları yüklü değilse `Initialize-ADSyncDomainJoinedComputerSync` başarısız olur.  AD DS araçları özellikleri - uzak sunucu yönetim araçları - Rol Yönetim Araçları altında Sunucu Yöneticisi üzerinden yüklenebilir.

Windows Server 2008 veya önceki sürümlerini çalıştıran etki alanı denetleyicileri için hizmet bağlantı noktası oluşturmak için aşağıdaki betiği kullanın.

Bir Çoklu orman yapılandırması, hizmet bağlantı noktası bilgisayarları bulunduğu her ormanda oluşturmak için aşağıdaki betiği kullanmanız gerekir:
 
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

Yukarıdaki betik

- `$verifiedDomain = "contoso.com"` Azure AD'de doğrulanmış etki alanı adlarınızı biriyle değiştirmeniz gereken bir yer tutucudur. Kullanabilmeniz için önce etki alanının sahibi gerekecektir.

Doğrulanmış etki alanı adları hakkında daha fazla ayrıntı için bkz. [Azure Active Directory'ye özel etki alanı adı ekleme](../active-directory-domains-add-azure-portal.md).  
Doğrulanmış şirket etki alanlarınızın listesini almak için kullanabileceğiniz [Get-AzureADDomain](/powershell/module/Azuread/Get-AzureADDomain?view=azureadps-2.0) cmdlet'i. 

![Get-AzureADDomain](./media/hybrid-azuread-join-manual-steps/01.png)

## <a name="setup-issuance-of-claims"></a>Talep verme Kurulumu

Federasyon Azure AD yapılandırması cihazları kullanan Active Directory Federasyon Hizmetleri (AD FS) üzerinde veya 3. taraf Federasyon Hizmeti için Azure AD kimlik doğrulaması için şirket. Cihazlar, Azure Active Directory cihaz kayıt Hizmeti'ne karşı (Azure DRS) kaydetmek için erişim belirteci almak için kimlik doğrulaması.

Windows cihazları şirket içi Federasyon Hizmeti tarafından barındırılan tümleşik Windows doğrulaması etkin bir WS-Trust uç (1.3 veya 2005 sürümleri) kullanarak kimlik doğrulaması.

> [!NOTE]
> AD FS ya da kullanırken **adfs/services/güven/13/windowstransport** veya **adfs/services/güven/2005/windowstransport** etkinleştirilmesi gerekir. Ayrıca Web kimlik doğrulaması Ara sunucusu kullanıyorsanız, bu uç nokta proxy'si aracılığıyla yayımlandığından emin olun. Hangi uç noktaları altında AD FS Yönetim Konsolu aracılığıyla etkinleştirilir gördüğünüz **hizmet > uç noktalar**.
>
>Şirket içi Federasyon hizmetinizin AD FS yoksa, WS-Trust 1.3 veya 2005 uç noktaları ve bunlar meta veri değişimi dosyası (MEX) aracılığıyla yayımlanan destekledikleri emin olmak için satıcınızın yönergeleri izleyin.

Aşağıdaki talep tamamlanması, cihaz kaydı için Azure DRS'tarafından alınan belirteç bulunmalıdır. Azure DRS, ardından yeni oluşturulan bir cihaz nesnesi bilgisayar hesabı şirket içi ile ilişkilendirmek için Azure AD Connect tarafından kullanılan bu bilgilerin bazıları ile Azure AD'de bir cihaz nesnesi oluşturacak.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Birden çok doğrulanmış etki alanı adı varsa, bilgisayarlar için aşağıdaki talep vermeniz gerekir:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Zaten bir Immutableıd talebi (örneğin, alternatif bir oturum açma kimliği) veriyorsanız bilgisayarlar için karşılık gelen bir talep sağlamanız gerekir:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

Aşağıdaki bölümlerde, hakkında bilgi edinin:
 
- Her talep değerler içermelidir
- Nasıl bir tanımı AD FS'de görünmesi

Tanımı değerleri mevcut olup olmadığını veya bunları oluşturmanız gerekip gerekmediğini doğrulamak için yardımcı olur.

> [!NOTE]
> Şirket içi Federasyon sunucunuz için AD FS kullanmıyorsanız, bu talepleri vermek için uygun bir yapılandırma oluşturmak için satıcınızın yönergeleri izleyin.

### <a name="issue-account-type-claim"></a>Sorun hesap türü talep

**`http://schemas.microsoft.com/ws/2012/01/accounttype`** -Bu talep değerini içermelidir **DJ**, cihaz etki alanına katılmış bir bilgisayar olarak tanımlar. AD FS'de, şuna benzer bir verme dönüştürme kural ekleyebilirsiniz:

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

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a>ObjectGUID bilgisayar hesabı şirket içi sorun

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** -Bu talebi içermelidir **objectGUID** şirket içi bilgisayar hesabının değeri. AD FS'de, şuna benzer bir verme dönüştürme kural ekleyebilirsiniz:

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
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a>ObjectSID bilgisayar hesabı şirket içi sorun

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** -Bu talebi içermelidir **objectSID** şirket içi bilgisayar hesabının değeri. AD FS'de, şuna benzer bir verme dönüştürme kural ekleyebilirsiniz:

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

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>Birden çok etki alanı adlarını Azure AD'de doğrulanmış, bilgisayar için İssuerıd sorunu

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** -Herhangi bir şirket içi Federasyon Hizmeti (AD FS veya 3. taraf) ile bağlanan doğrulanmış etki alanı adlarını Tekdüzen Kaynak Tanımlayıcısı (URI) Bu talebi içermelidir belirteci verilirken. ' De AD FS, yukarıdaki olanları sonra belirli bir sıraya göre dışındaki görünmesi verme dönüştürme kuralları ekleyebilirsiniz. Kullanıcılar için kural açıkça vermek için bu kurallardan biri gereklidir unutmayın. Kurallarında, bilgisayar kimlik doğrulaması ve kullanıcı olup olmadığınızı belirlemek ilk kural eklenir.

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

- `<verified-domain-name>` Azure AD'de doğrulanmış etki alanı adlarınızı biriyle değiştirmeniz gereken bir yer tutucudur. Örneğin, değer = "http://contoso.com/adfs/services/trust/"



Doğrulanmış etki alanı adları hakkında daha fazla ayrıntı için bkz. [Azure Active Directory'ye özel etki alanı adı ekleme](../active-directory-domains-add-azure-portal.md).  

Doğrulanmış şirket etki alanlarınızın listesini almak için kullanabileceğiniz [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet'i. 

![Get-MsolDomain](./media/hybrid-azuread-join-manual-steps/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>Kullanıcılar için birer mevcut olduğunda (kimliği ayarlanır örn alternatif bir oturum açma) bilgisayar için Immutableıd sorunu

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** -Bu talep, bilgisayarlar için geçerli bir değer içermesi gerekir. AD FS'de, aşağıdaki gibi bir verme dönüştürme kural oluşturabilirsiniz:

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

Aşağıdaki betik, verme oluşturulmasıyla kurallarına yukarıda açıklanan dönüştürme yardımcı olur.

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

- Bu betik, mevcut kurallar için kurallar ekler. Betiği iki kez çalıştırmayın çünkü kural kümesinin iki kez eklenir. Karşılık gelen hiçbir kural (karşılık gelen koşullarda) bu talepler için betiği yeniden çalıştırmadan önce bulunduğundan emin olun.

- Birden çok doğrulanmış etki alanı adı (Get-MsolDomains cmdlet'i aracılığıyla veya Azure AD portalında gösterildiği gibi) varsa, değerini ayarlamak **$multipleVerifiedDomainNames** betikteki **$true**. Ayrıca Azure AD Connect tarafından veya başka bir yolla aracılığıyla oluşturulmuş olabilir herhangi mevcut issuerıd talebi kaldırdığınızdan emin olun. Bu kural için bir örnek aşağıda verilmiştir:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Zaten yayımlandı durumunda bir **Immutableıd** ayarlayın, kullanıcı hesapları için talep **$immutableIDAlreadyIssuedforUsers** betikteki **$true**.

## <a name="enable-windows-down-level-devices"></a>Windows alt düzey cihazları etkinleştirme

Windows alt düzey cihazlar etki alanına katılmış cihazlarınızı bazıları için gerekirse:

- Kullanıcıların aygıtlarını kaydetmesini sağlamak için Azure AD'de bir ilke ayarlayın.
 
- Destek talebi şirket içi Federasyon hizmetinizi yapılandırmak **tümleşik Windows kimlik doğrulaması (IWA)** cihaz kaydı için.
 
- Azure AD cihaz kimlik doğrulama uç noktası, cihaz kimlik doğrulaması yapılırken sertifika istemleri önlemek için yerel Intranet bölgesine ekleyin.

### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Kullanıcıların aygıtlarını kaydetmesini sağlamak için Azure AD'de İlkesi ayarlama

Windows alt düzey cihazları kaydetmek için ayarı, kullanıcıların cihazları Azure AD'ye kaydetme izin vermek için ayarlanmış olduğundan emin olmanız gerekir. Azure portalında bu ayarı altında bulabilirsiniz:

`Azure Active Directory > Users and groups > Device settings`
    
Aşağıdaki ilke ayarlanmalıdır **tüm**: **kullanıcıların cihazlarını Azure AD'ye kaydetme**

![Cihaz kaydetme](./media/hybrid-azuread-join-manual-steps/23.png)


### <a name="configure-on-premises-federation-service"></a>Şirket içi Federasyon hizmetini yapılandırın 

Şirket içi Federasyon hizmetinizin verme desteklemelidir **authenticationmethod** ve **wiaormultiauthn** talep tutan Azure AD bağlı olan taraf için kimlik doğrulama isteği aldığında bir resouce_params parametre gösterildiği gibi kodlanmış bir değer ile aşağıda:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Böyle bir istek geldiğinde, şirket içi Federasyon Hizmeti, tümleşik Windows kimlik doğrulaması kullanarak kullanıcı kimlik doğrulaması yapmalıdır ve başarılı olduktan sonra aşağıdaki iki talep yayımlamanız gerekir:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

AD FS'de kimlik doğrulama yöntemini geçişleri bütünlüklü bir verme dönüştürme kuralı eklemeniz gerekir.  

**Bu kural eklemek için:**

1. AD FS yönetim konsolunda Git `AD FS > Trust Relationships > Relying Party Trusts`.
2. Microsoft Office 365 kimlik platformu bağlı olan taraf güveni nesneye sağ tıklayın ve ardından **talep kurallarını Düzenle**.
3. Üzerinde **verme dönüştürme kuralları** sekmesinde **Kuralı Ekle**.
4. İçinde **talep kuralı** şablon listesinden **talepleri özel kural kullanarak Gönder**.
5. **İleri**’yi seçin.
6. İçinde **talep kuralı adı** kutusuna **kimlik doğrulama yöntemi talep kuralı**.
7. İçinde **talep kuralı** aşağıdaki kural yazın:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. Federasyon sunucunuzda değiştirdikten sonra aşağıdaki PowerShell komutunu yazın **\<RPObjectName\>** ile bağlı olan taraf nesne adı, Azure AD bağlı olan taraf güven nesnesi. Bu nesne genellikle adlı **Microsoft Office 365 kimlik platformu**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a>Azure AD cihaz kimlik doğrulama uç noktası yerel Intranet bölgesine ekleyin.

Sertifika önlemek için kayıt cihazlardaki kullanıcılar yerel Intranet bölgesine Internet Explorer'da aşağıdaki URL'yi eklemek için etki alanına katılmış cihazlar için bir ilke gönderebilmek için Azure ad kimlik doğrulaması sırasında ister:

`https://device.login.microsoftonline.com`

## <a name="control-deployment-and-rollout"></a>Denetim dağıtım ve sunum

Gerekli adımları tamamladıktan sonra etki alanına katılmış cihazlar otomatik olarak Azure AD'ye katılmak hazırdır:

- Windows 10 Yıldönümü güncelleştirmesi ve Windows Server 2016 çalıştıran tüm etki alanına katılmış cihazlar Azure AD'ye cihaz kaydı otomatik olarak yeniden başlatın veya kullanıcı oturum açma. 

- Yeni cihazlar etki alanına katılma işlemi tamamlandıktan sonra cihaz yeniden başlatıldığında, Azure AD'ye kaydetme.

- Kayıtlı (örneğin, Intune için) daha önce Azure AD cihazları için geçiş "*etki alanına katılmış, AAD kayıtlı*"; Ancak, etki alanının gelen normal akıştaki nedeniyle tüm cihazlardaki tamamlamak bu işlem biraz zaman alabilir ve kullanıcı etkinliği.

### <a name="remarks"></a>Açıklamalar

- Bir Grup İlkesi nesnesi veya Windows 10 ve Windows Server 2016 etki alanına katılmış bilgisayarların otomatik kayıt piyasaya sürümü denetlemek için System Center Configuration Manager istemcisi ayarı kullanabilirsiniz. **Bu cihazlar otomatik olarak Azure AD'ye kaydetme istemediğiniz veya kayıt denetlemek istiyorsanız**, önce bu cihazlara otomatik kayıt devre dışı bırakma, Grup İlkesi alma gerekir sonra veya yapılandırma İstemci altındaki bulut Hizmetleri ayarı yapılandırmalısınız Yöneticisi -> otomatik olarak kaydı yeni Windows 10 etki alanına katılmış cihazlar "tüm yapılandırma adımlarını başlatmadan önce Hayır", Azure Active Directory ile. Bitirdikten sonra yapılandırmak, ve test hazır olduğunuzda, Grup İlkesi yalnızca sınama cihazları için otomatik kaydı etkinleştirme kullanıma alma ve diğer tüm cihazlar aynı tercih olmalıdır.

- Windows alt düzey bilgisayar dağıtım için dağıtabileceğiniz bir [Windows Installer paketi](#windows-installer-packages-for-non-windows-10-computers) seçtiğiniz bilgisayarlara.

- Windows 8.1 etki alanına katılmış cihazlar için Grup İlkesi nesnesi gönderirseniz, bir birleştirme girişiminde; Ancak, kullanmanız önerilir [Windows Installer paketi](#windows-installer-packages-for-non-windows-10-computers) tüm Windows alt düzey cihazları eklemek için. 

### <a name="create-a-group-policy-object"></a>Bir Grup İlkesi nesnesi oluşturun 

Geçerli Windows bilgisayarların piyasaya sürümü denetlemek için dağıtmanız **bilgisayarları etki alanına katılmış cihaz olarak kaydetme** kaydolmak istediğiniz cihazlara Grup İlkesi nesnesi. Örneğin, bir kuruluş birimi veya güvenlik grubuna ilke dağıtabilirsiniz.

**İlke ayarlamak için:**

1. Açık **Sunucu Yöneticisi'ni**ve ardından `Tools > Group Policy Management`.
2. Geçerli Windows bilgisayarları otomatik kaydı etkinleştirmek için istediğiniz etki alanına karşılık gelen etki alanı düğümüne gidin.
3. Sağ **Grup İlkesi nesneleri**ve ardından **yeni**.
4. Grup İlkesi nesneniz için bir ad yazın. Örneğin, * hibrit Azure AD'ye katılma. 
5. **Tamam** düğmesine tıklayın.
6. Yeni Grup İlkesi nesnenizin sağ tıklayın ve ardından **Düzenle**.
7. Git **Bilgisayar Yapılandırması** > **ilkeleri** > **Yönetim Şablonları** > **Windows Bileşenleri** > **cihaz kaydı**. 
8. Sağ **bilgisayarları etki alanına katılmış cihaz olarak kaydetme**ve ardından **Düzenle**.
   
   > [!NOTE]
   > Bu Grup İlkesi şablonu Grup İlkesi Yönetimi konsolunun önceki sürümlerinden yeniden adlandırıldı. Konsol önceki bir sürümünü kullanıyorsanız, Git `Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`. 

7. Seçin **etkin**ve ardından **Uygula**. Seçmelisiniz **devre dışı bırakılmış** otomatik olarak Azure AD'ye kaydetme bu Grup İlkesi tarafından denetlenen cihazları engellemek için ilke istiyorsanız.

8. **Tamam** düğmesine tıklayın.
9. Grup İlkesi nesnesini, seçtiğiniz bir konuma bağlayın. Örneğin, belirli bir kuruluş birimi olarak bağlayabilirsiniz. Otomatik olarak Azure AD'ye katılan bilgisayarların belirli bir güvenlik grubuna da bağlayabilirsiniz. Tüm etki alanına katılmış Windows 10 ve Windows Server 2016, kuruluşunuzdaki bilgisayarlar için bu ilke ayarlamak için Grup İlkesi nesnesini, etki alanına bağlayın.

### <a name="windows-installer-packages-for-non-windows-10-computers"></a>Windows 10 bilgisayarları için Windows Installer paketleri

Birleştirilmiş bir ortamda Windows alt düzey bilgisayarları etki alanına katılmış katılmak için indirin ve bu Windows Installer (.msi) paketi İndirme Merkezi'nden yüklemek [Windows 10 bilgisayarları için Microsoft çalışma alanına katılma](https://www.microsoft.com/en-us/download/details.aspx?id=53554) Sayfa.

System Center Configuration Manager gibi bir yazılım dağıtım sistemi kullanarak pakete dağıtabilirsiniz. Paket ile standart sessiz yükleme seçeneklerini destekler *sessiz* parametresi. System Center Configuration Manager geçerli dalının tamamlanmış kayıtları izleme yeteneği gibi daha önceki sürümlerin ek avantajlar sunar. Daha fazla bilgi için [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).

Yükleyici, kullanıcının bağlamında çalışan sistemdeki zamanlanmış bir görev oluşturur. Windows için kullanıcının oturum açtığı zaman görevi tetiklenir. Görev, kullanıcı kimlik bilgileriyle tümleşik Windows kimlik doğrulamasını kullanarak kimlik doğrulaması sonra Azure AD ile cihaz sessizce birleştirir. Cihaz zamanlanan görevde görmek için Git **Microsoft** > **çalışma alanına katılma**ve ardından Görev Zamanlayıcı Kitaplığı'na gidin.

## <a name="verify-joined-devices"></a>Katılan cihazlar doğrulayın

Kuruluşunuzda başarılı katılan cihazlar kullanarak denetleyebilirsiniz [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet'inde [Azure Active Directory PowerShell Modülü](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

Bu cmdlet'in çıktısı, kayıtlı ve Azure AD'ye katılmış cihazları gösterir. Tüm cihazlar almak için kullanın **-tüm** parametresi ve bunları filtreleyin kullanarak **deviceTrustType** özelliği. Etki alanına katılmış cihazlar değerine sahip **etki alanına katılmış**.



## <a name="troubleshoot-your-implementation"></a>Uygulamanızda sorun giderme

Sorunları yaşıyorsanız katılmış Windows cihazlar karma tamamlama ile Azure AD'ye katılım etki alanı için bkz:

- [Windows cihazları için sorun giderme hibrit Azure AD'ye katılma](troubleshoot-hybrid-join-windows-current.md)
- [Windows alt düzey cihazları için sorun giderme hibrit Azure AD'ye katılma](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'de cihaz yönetimine giriş](overview.md)



<!--Image references-->
[1]: ./media/hybrid-azuread-join-manual-steps/12.png
