---
title: Azure AD Connect - AD FS yönetimi ve özelleştirme | Microsoft Docs
description: Azure AD Connect ve AD FS oturum açma ile kullanıcı deneyimi Azure AD Connect ve PowerShell özelleştirmesini ile AD FS yönetimi.
keywords: AD FS, ADFS, AD FS yönetimi, AAD bağlanma, bağlan, oturum açma, AD FS özelleştirmesi, onarım güven, O365, Federasyon, bağlı olan taraf
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: mtillman
editor: ''
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 719506e35e6abe5ac573c7ceedc1668fd2704bd4
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961698"
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a>Yönetmek ve Azure AD Connect kullanarak Active Directory Federasyon Hizmetleri özelleştirme
Bu makalede, Azure Active Directory (Azure AD) Connect kullanarak nasıl yöneteceğinizi ve Active Directory Federasyon Hizmetleri (AD FS) özelleştirme açıklanır. Ayrıca, bir AD FS grubu için bir tam yapılandırma yapmanız gerekebilecek diğer ortak AD FS görevler içerir.

| Konu | Ne kapsar |
|:--- |:--- |
| **AD FS yönetme** | |
| [Güven onarın](#repairthetrust) |Office 365 ile federasyon güveni onarmak nasıl. |
| [Alternatif oturum açma Kimliğini kullanarak Azure AD ile birleştirmek ](#alternateid) | Alternatif oturum açma Kimliğini kullanarak Federasyon yapılandırma  |
| [Bir AD FS sunucu ekleme](#addadfsserver) |Ek bir AD FS sunucu içeren bir AD FS grubunu genişletin yapma. |
| [Bir AD FS Web uygulaması Ara Sunucusu Ekle](#addwapserver) |Ek bir Web uygulaması Ara sunucusu (WAP) sunucu içeren bir AD FS grubunu genişletin yapma. |
| [Bir Federasyon etki alanına ekleme](#addfeddomain) |Bir Federasyon etki alanına ekleme. |
| [SSL sertifikasını güncelleştir](active-directory-aadconnectfed-ssl-update.md)| SSL sertifikası için bir AD FS grubunu güncelleştirmek nasıl. |
| **AD FS özelleştirme** | |
| [Özel şirket logosu veya çizim ekleme](#customlogo) |Şirket logo ve çizim ile AD FS oturum açma sayfasını özelleştirme yapma. |
| [Bir oturum açma açıklama ekleme](#addsignindescription) |Oturum açma sayfası açıklaması ekleme. |
| [AD FS talep kuralları değiştirme](#modclaims) |AD FS taleplerini çeşitli Federasyon senaryoları için değişiklik yapma. |

## <a name="manage-ad-fs"></a>AD FS yönetme
Azure AD Connect Sihirbazı'nı kullanarak Azure AD CONNECT'te minimum kullanıcı katılımıyla çeşitli AD FS ile ilgili görevleri gerçekleştirebilirsiniz. Azure AD Connect Sihirbazı'nı çalıştırarak yüklemeyi tamamladıktan sonra yeniden ek görevleri gerçekleştirmek için sihirbazı çalıştırabilirsiniz.

## <a name="repairthetrust"></a>Güven onarın 
Azure AD Connect, güven onarmak için AD FS ile Azure AD güven ve uygun eylemi geçerli durumunu denetlemek için kullanabilirsiniz. Azure AD onarmak için aşağıdaki adımları izleyin ve AD FS güven.

1. Seçin **onarım AAD ve ADFS güven** ek görevler listesinden.
   ![Onarım AAD ve ADFS güveni](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)

2. Üzerinde **Azure ad Connect** sayfasında, Azure AD genel yönetici kimlik bilgilerinizi sağlayın ve tıklayın **sonraki**.
   ![Azure AD'ye Bağlanma](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)

3. Üzerinde **uzaktan erişim kimlik bilgileri** sayfasında, etki alanı yöneticisi kimlik bilgilerini girin.

   ![Uzaktan erişim kimlik bilgileri](media/active-directory-aadconnect-federation-management/RepairADTrust3.PNG)

    Tıklattıktan sonra **sonraki**, Azure AD Connect için sertifika durumu denetler ve sorunları gösterir.

    ![Sertifika durumu](media/active-directory-aadconnect-federation-management/RepairADTrust4.PNG)

    **Yapılandırma için hazır** sayfası güven onarmak için gerçekleştirilen eylemlerin listesini gösterir.

    ![Yapılandırma için hazır](media/active-directory-aadconnect-federation-management/RepairADTrust5.PNG)

4. Tıklatın **yükleme** güven onarmak için.

> [!NOTE]
> Azure AD Connect can yalnızca onarma veya otomatik olarak imzalanan sertifikalar vermemizi. Azure AD Connect, üçüncü taraf sertifikalarının onarılamıyor.

## <a name="alternateid"></a>AlternateID kullanarak Azure AD ile birleştirmek 
Şirket içi kullanıcı asıl Name(UPN) ve kullanıcı asıl adı Bulut aynı kalmasını önerilir. Şirket içi UPN yönlendirilemeyen bir etki alanına (örneğin kullanıyorsa Contoso.local) ya da değiştirilemez yerel uygulama bağımlılıkları nedeniyle öneririz alternatif oturum açma kimliği ayarlama Alternatif oturum açma kimliği bir öznitelik posta gibi kendi UPN dışındaki oturum nerede kullanıcılar oturum bir oturum açma deneyimi yapılandırmanıza olanak sağlar. UserPrincipalName özniteliği Active Directory'de Azure AD Connect varsayılan seçenek için kullanıcı asıl adı. Kullanıcı asıl adı için başka bir öznitelik seçin ve AD FS kullanarak federasyonunu, ardından Azure AD Connect alternatif oturum açma kimliği için AD FS yapılandırma Kullanıcı asıl adı için farklı bir öznitelik seçme örneği aşağıda verilmiştir:

![Alternatif kimlik öznitelik seçimi](media/active-directory-aadconnect-federation-management/attributeselection.png)

AD FS için alternatif oturum açma Kimliğini yapılandırma iki ana adımdan oluşur:
1. **Verme talep doğru yetenek kümesini yapılandırmak**: Azure AD bağlı olan taraf verme talebi kurallarında güven değiştirildiğinde seçilen UserPrincipalName özniteliği kullanıcı alternatif kimlik olarak kullanmak için.
2. **AD FS yapılandırması alternatif oturum açma kimliği etkinleştirmek**: AD FS alternatif kimliğini kullanarak uygun ormanda kullanıcıları bakabilirsiniz şekilde AD FS yapılandırması güncelleştirildi Bu yapılandırma, AD FS için Windows Server 2012 R2 (KB2919355) veya sonraki sürümlerde desteklenir. 2012 R2 AD FS sunucuları varsa Azure AD Connect için gerekli KB varlığını denetler. KB algılanmazsa, yapılandırma tamamlandıktan sonra bir uyarı aşağıda gösterildiği gibi gösterilir:

    ![KB 2012R2 üzerinde eksik uyarı](media/active-directory-aadconnect-federation-management/kbwarning.png)

    Eksik KB durumunda yapılandırmasını düzeltmek için gerekli yükleme [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) ve güven kullanarak Onar [AAD onarın ve AD FS güven](#repairthetrust).

> [!NOTE]
> AlternateID ve el ile yapılandırma adımları hakkında daha fazla bilgi için okuma [alternatif oturum açma Kimliğini yapılandırma](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)

## <a name="addadfsserver"></a>Bir AD FS sunucu ekleme 

> [!NOTE]
> Bir AD FS sunucu eklemek için Azure AD Connect PFX sertifikası gerektirir. Bu nedenle, yalnızca, AD FS grubunu Azure AD Connect kullanarak yapılandırdıysanız bu işlemi gerçekleştirebilir.

1. Seçin **ek bir Federasyon Sunucusu'nu Dağıtma**, tıklatıp **sonraki**.

   ![Ek Federasyon sunucusu](media/active-directory-aadconnect-federation-management/AddNewADFSServer1.PNG)

2. Üzerinde **Azure ad Connect** sayfasında, Azure AD genel yönetici kimlik bilgilerinizi girin ve tıklayın **sonraki**.

   ![Azure AD'ye Bağlanma](media/active-directory-aadconnect-federation-management/AddNewADFSServer2.PNG)

3. Etki alanı yönetici kimlik bilgilerini sağlayın.

   ![Etki alanı yönetici kimlik bilgileri](media/active-directory-aadconnect-federation-management/AddNewADFSServer3.PNG)

4. Azure AD Connect, Azure AD Connect ile yeni AD FS grubunuzu yapılandırırken sağlanan PFX dosyasının parolası ister. Tıklatın **parolasını girin** PFX dosyası için parola sağlamak için.

   ![Sertifika parolası](media/active-directory-aadconnect-federation-management/AddNewADFSServer4.PNG)

    ![SSL sertifikasını belirtin](media/active-directory-aadconnect-federation-management/AddNewADFSServer5.PNG)

5. Üzerinde **AD FS sunucuları** sayfasında, sunucu adı veya AD FS grubuna eklenecek IP adresini girin.

   ![AD FS sunucuları](media/active-directory-aadconnect-federation-management/AddNewADFSServer6.PNG)

6. Tıklatın **sonraki**ve en son Git **yapılandırma** sayfası. Azure AD Connect sunucuları için AD FS grubunu eklemeyi tamamladıktan sonra bağlantıyı doğrulamak için seçeneği sunulur.

   ![Yapılandırma için hazır](media/active-directory-aadconnect-federation-management/AddNewADFSServer7.PNG)

    ![Yükleme tamamlandı](media/active-directory-aadconnect-federation-management/AddNewADFSServer8.PNG)

## <a name="addwapserver"></a>Bir AD FS WAP sunucusu ekleme 

> [!NOTE]
> WAP sunucusu eklemek için Azure AD Connect PFX sertifikası gerektirir. Bu nedenle, Azure AD Connect kullanarak AD FS grubunu yapılandırdıysanız bu işlemi yalnızca gerçekleştirebilirsiniz.

1. Seçin **Web uygulaması proxy'si dağıtmak** kullanılabilir görevler listesinden.

   ![Web uygulaması Ara sunucusu](media/active-directory-aadconnect-federation-management/WapServer1.PNG)

2. Azure genel yönetici kimlik bilgilerini sağlayın.

   ![Azure AD'ye Bağlanma](media/active-directory-aadconnect-federation-management/wapserver2.PNG)

3. Üzerinde **belirtin SSL sertifikası** sayfa, Azure AD Connect ile AD FS grubu yapılandırıldığında, sağlanan PFX dosyası için parola sağlayın.
   ![Sertifika parolası](media/active-directory-aadconnect-federation-management/WapServer3.PNG)

    ![SSL sertifikasını belirtin](media/active-directory-aadconnect-federation-management/WapServer4.PNG)

4. WAP sunucusu olarak eklenecek sunucunun ekleyin. WAP sunucunun etki alanına katılmamış çünkü sihirbaz eklenmekte olan sunucuya yönetimsel kimlik bilgilerini ister.

   ![Yönetim sunucusu kimlik bilgileri](media/active-directory-aadconnect-federation-management/WapServer5.PNG)

5. Üzerinde **Proxy güven kimlik bilgilerini** sayfasında, Proxy'yi yapılandırmak için yönetici kimlik bilgilerine güven ve AD FS grubunuzdaki birincil sunucu erişim sağlayın.

   ![Ara sunucu güveni kimlik bilgileri](media/active-directory-aadconnect-federation-management/WapServer6.PNG)

6. Üzerinde **yapılandırma için hazır** sayfasında, Sihirbazı gerçekleştirilecek eylemler listesini gösterir.

   ![Yapılandırma için hazır](media/active-directory-aadconnect-federation-management/WapServer7.PNG)

7. Tıklatın **yükleme** yapılandırmayı tamamlamak için. Yapılandırma tamamlandıktan sonra sihirbazın sunucularına bağlantıyı doğrulamak için seçeneği sunar. Tıklatın **doğrula** bağlantılarını denetlemek için.

   ![Yükleme tamamlandı](media/active-directory-aadconnect-federation-management/WapServer8.PNG)

## <a name="addfeddomain"></a>Bir Federasyon etki alanına ekleme 

Azure AD Connect kullanarak Azure AD ile birleştirilecek bir etki alanına eklemek de kolaydır. Azure AD Connect etki alanını Federasyon ekler ve Azure AD ile birleştirildiyse birden çok etki alanınız olduğunda dağıtımcı doğru bir şekilde yansıtmak üzere talep kurallarına değiştirir.

1. Bir Federasyon etki alanına eklemek için görevi seçin **ek bir ekleme Azure AD etki alanı**.

   ![Ek Azure AD etki alanı](media/active-directory-aadconnect-federation-management/AdditionalDomain1.PNG)

2. Sihirbazın sonraki sayfasında, Azure AD genel yönetici kimlik bilgilerini sağlayın.

   ![Azure AD'ye Bağlanma](media/active-directory-aadconnect-federation-management/AdditionalDomain2.PNG)

3. Üzerinde **uzaktan erişim kimlik bilgileri** sayfasında, etki alanı yönetici kimlik bilgilerini sağlayın.

   ![Uzaktan erişim kimlik bilgileri](media/active-directory-aadconnect-federation-management/additionaldomain3.PNG)

4. Sonraki sayfada, sihirbaz, şirket içi dizininizle Azure AD etki alanlarının bir listesini sağlar. Etki alanı listesinden seçin.

   ![Azure AD etki alanı](media/active-directory-aadconnect-federation-management/AdditionalDomain4.PNG)

    Etki alanını seçtikten sonra sihirbazın daha sihirbazın gerçekleştireceği eylemleri ve yapılandırma etkisini ilgili bilgileri sağlar. Henüz Azure AD'de doğrulanmış olmayan bir etki alanı seçerseniz bazı durumlarda, sihirbaz, etki alanı doğrulamanıza yardımcı olacak bilgiler sağlar. Bkz: [özel etki alanı adınızı Azure Active Directory'ye ekleme](../active-directory-domains-add-azure-portal.md) daha fazla ayrıntı için.

5. **İleri**’ye tıklayın. **Yapılandırma için hazır** sayfası Azure AD Connect gerçekleştireceği eylemlerin listesini gösterir. Tıklatın **yükleme** yapılandırmayı tamamlamak için.

   ![Yapılandırma için hazır](media/active-directory-aadconnect-federation-management/AdditionalDomain5.PNG)

> [!NOTE]
> Azure AD ile oturum açabilmeniz olmaları önce eklenen Federasyon etki alanındaki kullanıcıların eşitlenmelidir.

## <a name="ad-fs-customization"></a>AD FS özelleştirmesi
Aşağıdaki bölümlerde, AD FS oturum açma sayfanız özelleştirdiğinizde gerçekleştirmeniz gerekebilir yaygın görevlerden bazıları hakkında ayrıntılar sağlanmaktadır.

## <a name="customlogo"></a>Özel şirket logosu veya çizim ekleme 
Üzerinde gösterilen şirketin logosunu değiştirmek için **oturum açma** sayfasında, aşağıdaki Windows PowerShell cmdlet'ini ve sözdizimini kullanın.

> [!NOTE]
> Önerilen Logo dosyası boyutu 10 KB'den büyük olmaması 96 dpi'de 260 x 35 boyutlardır.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> *TargetName* parametresi gereklidir. AD FS ile birlikte yayınlanan varsayılan temanın varsayılan olarak adlandırılır.

## <a name="addsignindescription"></a>Bir oturum açma açıklama ekleme 
İçin oturum açma sayfasına bir açıklama eklemek için **oturum açma sayfası**, aşağıdaki Windows PowerShell cmdlet'ini ve sözdizimini kullanın.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## <a name="modclaims"></a>AD FS talep kuralları değiştirme 
AD FS özel talep kuralları oluşturmak için kullanabileceğiniz bir zengin talep dili destekler. Daha fazla bilgi için bkz: [talep kuralı dili rolü](https://technet.microsoft.com/library/dd807118.aspx).

Aşağıdaki bölümlerde Azure AD ile ilgili bazı senaryolar için özel kurallar nasıl yazma ve AD FS federasyon.

### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Bir değeri öznitelikte bulunmasına koşullu bir sabit kimlik
Azure AD Connect nesnelerin Azure AD ile eşitlenir kaynak bağlantısı kullanılacak bir öznitelik belirtmenize olanak sağlar. Özel öznitelik değeri boş değilse, bir sabit kimlik talebi vermek isteyebilirsiniz.

Örneğin, seçebilirsiniz **ms ds consistencyguid** sorunu ve kaynak bağlantısı için öznitelik olarak **İmmutableıd** olarak **ms ds consistencyguid** öznitelik olasılığına karşısında bir değer içeriyor. Öznitelik karşı değer yoksa, sorunu **objectGUID** değişmez kimliği olarak Aşağıdaki bölümde açıklandığı gibi özel talep kuralları kümesi oluşturabilirsiniz.

**Kural 1: Sorgu öznitelikleri**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

Bu kuralda değerlerini sorgulama **ms ds consistencyguid** ve **objectGUID** Active Directory'den kullanıcı için. AD FS dağıtımınıza uygun depolama adlarında için depo adını değiştirin. Aynı zamanda talep türü için uygun değişiklik için tanımlanan türü, Federasyon için talep **objectGUID** ve **ms ds consistencyguid**.

Kullanarak Ayrıca, **ekleme** ve **sorunu**, varlık için giden sorun eklemekten kaçının ve değerleri Ara değer olarak kullanabilirsiniz. Değişmez kimliği olarak kullanmak için hangi değerini kurduktan sonra bir sonraki kuralında talep verecek

**Kural 2: ms ds consistencyguid için kullanıcının var olup olmadığını denetleyin**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Bu kural adlı geçici bir bayrak tanımlar **idflag** ayarlanmış **useguid** yoksa hiçbir **ms ds consistencyguid** kullanıcı için doldurulur. Bu arkasındaki mantığı AD FS boş talep izin vermeyen gerçeğidir. Talep eklediğinizde, bunu http://contoso.com/ws/2016/02/identity/claims/objectguid ve http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid kural 1'de, şunun bir **msdsconsistencyguid** değeri bir kullanıcı için doldurulur yalnızca talep. Doldurulmuş değil, AD FS boş değere sahip olur ve hemen bırakır görür. Tüm nesneler sahip **objectGUID**, kural 1 yürütüldükten sonra bu talep böylece her zaman vardır olur.

**3. kural: varsa, ms ds consistencyguid değişmez kimliği olarak verme**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Örtülü budur **mevcut** denetleyin. Talep değeri varsa, daha sonra değişmez kimliği olarak sorunu Önceki örnekte kullanılmaktadır **NameIdentifier** talep. Bu, ortamınızda değişmez kimliği için uygun talep türü ile değiştirmek zorunda kalırsınız.

**Kural 4: ms ds consistencyGuid mevcut değilse objectGUID değişmez kimliği olarak verme**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

Bu kural, yalnızca geçici bayrağı denetimi **idflag**. Kendi değere göre talep vermek karar.

> [!NOTE]
> Bu kurallar sırası önemlidir.

### <a name="sso-with-a-subdomain-upn"></a>Bir alt etki alanı UPN SSO'su

Azure AD Connect kullanarak açıklandığı gibi birleştirilecek birden fazla etki alanı ekleyebilirsiniz [yeni bir Federasyon etki alanına eklemek](active-directory-aadconnect-federation-management.md#addfeddomain). Azure AD Connect sürümü 1.1.553.0 ve son doğru talep kuralı oluşturur issuerID için otomatik olarak. Azure AD Connect sürüm 1.1.553.0 kullanamazsınız veya son, önerilir [Azure AD RPT talep kuralları](https://aka.ms/aadrptclaimrules) aracı oluşturmak ve Azure AD bağlı olan taraf güveni için doğru talep kurallarını ayarlamak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [kullanıcı oturum açma seçenekleri](active-directory-aadconnect-user-signin.md).
