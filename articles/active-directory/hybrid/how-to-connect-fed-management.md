---
title: Azure AD Connect - AD FS yönetimi ve özelleştirmesi | Microsoft Docs
description: Azure AD Connect ve AD FS oturum açma ile kullanıcı deneyimi Azure AD Connect ve PowerShell özelleştirme ile AD FS yönetimi.
keywords: AD FS, ADFS, AD FS yönetimi, AAD Connect, bağlan, oturum açma AD FS özelleştirmesi, güven, O365, Federasyon, bağlı olan taraf onarın
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 021e13dafcc659337d4096a068e224312e69db1b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60353797"
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a>Azure AD Connect kullanarak Active Directory Federasyon Hizmetleri özelleştirme ve yönetme
Bu makalede, Azure Active Directory (Azure AD) Connect kullanarak Active Directory Federasyon Hizmetleri (AD FS) özelleştirme ve yönetme işlemini açıklamaktadır. Ayrıca, bir AD FS grubu için bir tam yapılandırma yapmanız gerekebilecek diğer ortak bir AD FS görevler içerir.

| Konu | Neleri kapsar |
|:--- |:--- |
| **AD FS yönetme** | |
| [Güveni onarın](#repairthetrust) |Office 365 ile federasyon güveni onarın yapma. |
| [Alternatif bir oturum açma Kimliğini kullanarak Azure AD ile federasyona](#alternateid) | Alternatif oturum açma kimliği kullanarak Federasyonu yapılandırma  |
| [AD FS sunucusu ekleme](#addadfsserver) |Nasıl bir ek AD FS sunucusu ile AD FS grubunu genişletin. |
| [AD FS Web uygulaması Ara sunucusu ekleme](#addwapserver) |Nasıl bir ek bir Web uygulaması Ara sunucusu (WAP) sunucusu ile AD FS grubunu genişletin. |
| [Bir Federasyon etki alanına ekleme](#addfeddomain) |Bir Federasyon etki alanına ekleme. |
| [SSL sertifikasını güncelleştirme](how-to-connect-fed-ssl-update.md)| SSL sertifikası için bir AD FS grubunu güncelleştirme yapma. |
| **AD FS özelleştirme** | |
| [Özel bir şirket logosu veya çizim ekleme](#customlogo) |Şirket logo ve çizim ile AD FS oturum açma sayfasını özelleştirme yapma. |
| [Oturum açma bir açıklama ekleyin](#addsignindescription) |Oturum açma sayfası açıklaması ekleme. |
| [AD FS talep kurallarını değiştirme](#modclaims) |AD FS talep çeşitli Federasyon senaryolarında değişiklik yapma. |

## <a name="manage-ad-fs"></a>AD FS yönetme
Azure AD Connect Sihirbazı'nı kullanarak Azure AD CONNECT'te en düşük kullanıcı katılımıyla çeşitli AD FS ile ilgili görevleri gerçekleştirebilirsiniz. Azure AD Connect sihirbazını çalıştırarak yüklemeyi bitirdikten sonra yeniden ek görevleri gerçekleştirmek için sihirbazı çalıştırabilirsiniz.

## <a name="repairthetrust"></a>Güveni onarın 
Güven onarmak için AD FS ile Azure AD güven ve uygun eylemleri geçerli durumunu denetlemek için Azure AD Connect kullanabilirsiniz. Azure AD onarmak için bu adımları izleyin ve AD FS güven.

1. Seçin **onarım AAD ve AD FS güven** ek görevler listesinden.
   ![Onarım AAD ve AD FS güveni](./media/how-to-connect-fed-management/RepairADTrust1.PNG)

2. Üzerinde **Azure ad Connect** sayfasında, Azure AD için genel yönetici kimlik bilgilerinizi girin ve tıklayın **sonraki**.
   ![Azure AD'ye Bağlanma](./media/how-to-connect-fed-management/RepairADTrust2.PNG)

3. Üzerinde **uzaktan erişim kimlik bilgileri** sayfasında, etki alanı yöneticisi kimlik bilgilerini girin.

   ![Uzaktan erişim kimlik bilgileri](./media/how-to-connect-fed-management/RepairADTrust3.PNG)

    Tıkladıktan sonra **sonraki**, Azure AD Connect için sertifika durumu denetler ve herhangi bir sorunu gösterir.

    ![Sertifika durumu](./media/how-to-connect-fed-management/RepairADTrust4.PNG)

    **Yapılandırma için hazır** sayfası güven onarmak için gerçekleştirilen eylemlerin listesini gösterir.

    ![Yapılandırma için hazır](./media/how-to-connect-fed-management/RepairADTrust5.PNG)

4. Tıklayın **yükleme** güven onarmak için.

> [!NOTE]
> Azure AD Connect can yalnızca Onar veya otomatik olarak imzalanan sertifikaları üzerinde işlem yapma. Azure AD Connect, üçüncü taraf sertifikalarının onarılamıyor.

## <a name="alternateid"></a>AlternateID kullanarak Azure AD ile federasyona 
Şirket içi kullanıcı asıl adı (UPN) ve bulut kullanıcı asıl adı aynı kalmasını önerilir. Şirket içi UPN (ör. yönlendirilemeyen etki alanı kullanıyorsa Contoso.local) veya değiştirilemez yerel uygulama bağımlılıklar nedeniyle öneririz alternatif bir oturum açma kimliği ayarlama Alternatif oturum açma kimliği kullanıcıların UPN, e-posta gibi farklı bir öznitelik oturum oturum bir oturum açma deneyimi yapılandırmanıza olanak sağlar. Azure AD Connect varsayılan olarak Active Directory'de userPrincipalName özniteliği istediğiniz kullanıcı asıl adı. Kullanıcı asıl adı için başka bir öznitelik seçin ve AD FS'yi kullanarak Federasyon, ardından Azure AD Connect alternatif bir oturum açma kimliği için AD FS yapılandırma Kullanıcı asıl adı için farklı bir öznitelik seçerek bir örnek aşağıda gösterilmiştir:

![Alternatif kimlik öznitelik seçimi](./media/how-to-connect-fed-management/attributeselection.png)

AD FS'yi alternatif oturum açma Kimliğini yapılandırma iki ana adımdan oluşur:
1. **Verme talepleri doğru ortaklık kümesi yapılandırma**: Seçili UserPrincipalName özniteliği kullanıcı alternatif kimlik olarak kullanmak için Azure AD bağlı olan taraf güveni talep verme kuralları değiştirilmiştir.
2. **AD FS yapılandırması içinde alternatif oturum açma kimliği etkinleştirme**: AD FS yapılandırması, böylece bu alternatif kimliği kullanarak uygun ormanlardaki kullanıcıların AD FS arayabilirsiniz güncelleştirilir Bu yapılandırma, AD FS (KB2919355 ile) Windows Server 2012 R2 veya üzeri için desteklenir. Azure AD Connect, AD FS sunucuları, 2012 R2 ise, gerekli KB varlığını denetler. KB algılanmazsa, yapılandırma tamamlandıktan sonra bir uyarı aşağıda gösterildiği gibi görüntülenir:

    ![Uyarı KB 2012R2 üzerinde eksik](./media/how-to-connect-fed-management/kbwarning.png)

    Eksik KB durumunda yapılandırmayı düzeltmek için gerekli yükleme [KB2919355](https://go.microsoft.com/fwlink/?LinkID=396590) ve güven kullanarak onarın [onarım AAD ve AD FS güvenini](#repairthetrust).

> [!NOTE]
> AlternateID ve el ile yapılandırma adımları hakkında daha fazla bilgi için okuma [alternatif oturum açma Kimliğini yapılandırma](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)

## <a name="addadfsserver"></a>AD FS sunucusu ekleme 

> [!NOTE]
> AD FS sunucusuna eklemek için Azure AD Connect, PFX sertifikası gerektirir. Azure AD Connect kullanarak AD FS grubunu yapılandırılması durumunda, bu nedenle, bu işlemi gerçekleştirebilir.

1. Seçin **ek bir federasyon sunucusunun dağıtımında**, tıklatıp **sonraki**.

   ![Ek Federasyon sunucusu](./media/how-to-connect-fed-management/AddNewADFSServer1.PNG)

2. Üzerinde **Azure ad Connect** sayfa, Azure AD için genel yönetici kimlik bilgilerinizi girin ve tıklayın **sonraki**.

   ![Azure AD'ye Bağlanma](./media/how-to-connect-fed-management/AddNewADFSServer2.PNG)

3. Etki alanı yönetici kimlik bilgilerini sağlayın.

   ![Etki alanı yönetici kimlik bilgileri](./media/how-to-connect-fed-management/AddNewADFSServer3.PNG)

4. Azure AD Connect, Azure AD Connect ile yeni AD FS grubunuzu yapılandırırken belirttiğiniz PFX dosyasının parolasını ister. Tıklayın **parolasını girin** PFX dosyasının parolasını sağlamak için.

   ![Sertifika parolası](./media/how-to-connect-fed-management/AddNewADFSServer4.PNG)

    ![SSL sertifikasını belirtin](./media/how-to-connect-fed-management/AddNewADFSServer5.PNG)

5. Üzerinde **AD FS sunucuları** sayfasında, sunucu adı veya AD FS grubuna eklenecek IP adresi girin.

   ![AD FS sunucuları](./media/how-to-connect-fed-management/AddNewADFSServer6.PNG)

6. Tıklayın **sonraki**ve en son **yapılandırma** sayfası. Azure AD Connect sunucuları AD FS grubuna ekleme işlemini tamamladıktan sonra bağlantıyı doğrulamak için seçeneği sunulur.

   ![Yapılandırma için hazır](./media/how-to-connect-fed-management/AddNewADFSServer7.PNG)

    ![Yükleme tamamlandı](./media/how-to-connect-fed-management/AddNewADFSServer8.PNG)

## <a name="addwapserver"></a>AD FS WAP sunucusu ekleme 

> [!NOTE]
> WAP sunucusu eklemek için Azure AD Connect, PFX sertifikası gerektirir. Bu nedenle, Azure AD Connect kullanarak AD FS grubunu yapılandırdıysanız yalnızca bu işlemi gerçekleştirebilir.

1. Seçin **Web uygulaması Ara sunucusu dağıtma** kullanılabilir görevler listesinden.

   ![Web uygulaması Ara sunucusu](./media/how-to-connect-fed-management/WapServer1.PNG)

2. Azure genel yönetici kimlik bilgilerini sağlayın.

   ![Azure AD'ye Bağlanma](./media/how-to-connect-fed-management/wapserver2.PNG)

3. Üzerinde **belirtin SSL sertifikası** sayfasında, Azure AD Connect ile AD FS grubunda yapılandırılmış zaman belirttiğiniz PFX dosyasının parolasını sağlayın.
   ![Sertifika parolası](./media/how-to-connect-fed-management/WapServer3.PNG)

    ![SSL sertifikasını belirtin](./media/how-to-connect-fed-management/WapServer4.PNG)

4. WAP sunucusu olarak eklenecek sunucunun ekleyin. WAP sunucusu etki alanına katılmamış çünkü eklenen sunucusuna yönetici kimlik bilgileri için sihirbaz sorar.

   ![Yönetim sunucusu kimlik bilgileri](./media/how-to-connect-fed-management/WapServer5.PNG)

5. Üzerinde **Ara sunucu güveni kimlik bilgileri** sayfasında, Proxy'yi yapılandırmak için yönetici kimlik bilgilerine güven ve AD FS grubunda birincil sunucu erişim sağlayın.

   ![Ara sunucu güveni kimlik bilgileri](./media/how-to-connect-fed-management/WapServer6.PNG)

6. Üzerinde **yapılandırma için hazır** sayfasında, sihirbaz, gerçekleştirilecek eylemlerin listesini gösterir.

   ![Yapılandırma için hazır](./media/how-to-connect-fed-management/WapServer7.PNG)

7. Tıklayın **yükleme** yapılandırmayı tamamlayın. Yapılandırma tamamlandıktan sonra sihirbazın sunucularına bağlantıyı doğrulamak için seçeneği sunar. Tıklayın **doğrulama** bağlantıyı denetlemek için.

   ![Yükleme tamamlandı](./media/how-to-connect-fed-management/WapServer8.PNG)

## <a name="addfeddomain"></a>Bir Federasyon etki alanına ekleme 

Azure AD Connect kullanarak Azure AD ile federasyona eklenmesi için bir etki alanına eklemek kolay bir işlemdir. Azure AD Connect, Federasyon etki alanını ekler ve talep kuralları, Azure AD ile birleştirildiyse birden çok etki alanınız olduğunda dağıtımcı doğru yansıtacak şekilde değiştirir.

1. Bir Federasyon etki alanına eklemek için görevi seçin **ek bir ekleme Azure AD etki alanı**.

   ![Ek Azure AD etki alanı](./media/how-to-connect-fed-management/AdditionalDomain1.PNG)

2. Sihirbazın bir sonraki sayfada, Azure AD için genel yönetici kimlik bilgilerini sağlayın.

   ![Azure AD'ye Bağlanma](./media/how-to-connect-fed-management/AdditionalDomain2.PNG)

3. Üzerinde **uzaktan erişim kimlik bilgileri** sayfasında, etki alanı yönetici kimlik bilgilerini sağlayın.

   ![Uzaktan erişim kimlik bilgileri](./media/how-to-connect-fed-management/additionaldomain3.PNG)

4. Sonraki sayfada, sihirbaz, şirket içi dizininizle devredebilir Azure AD etki alanlarının bir listesini sağlar. Etki alanı listeden seçin.

   ![Azure AD etki alanı](./media/how-to-connect-fed-management/AdditionalDomain4.PNG)

    Etki alanını seçtikten sonra Sihirbazı hakkında daha fazla sihirbazın gerçekleştireceği eylemleri ve yapılandırma etkisini uygun bilgileri sağlar. Henüz Azure AD'de doğrulanmış değil bir etki alanı seçtiğinizde bazı durumlarda, sihirbaz sizin, etki alanı doğrulamanıza yardımcı olacak bilgiler sağlar. Bkz: [Azure Active Directory'ye özel etki alanı adınızı ekleme](../active-directory-domains-add-azure-portal.md) daha fazla ayrıntı için.

5. **İleri**’ye tıklayın. **Yapılandırma için hazır** sayfa, Azure AD Connect gerçekleştireceği eylemlerin bir listesini gösterir. Tıklayın **yükleme** yapılandırmayı tamamlayın.

   ![Yapılandırma için hazır](./media/how-to-connect-fed-management/AdditionalDomain5.PNG)

> [!NOTE]
> Bunların Azure AD'de oturum açabilmeniz için önce eklenen Federasyon etki alanından kullanıcılar eşitlenmelidir.

## <a name="ad-fs-customization"></a>AD FS özelleştirmesi
Aşağıdaki bölümler, AD FS oturum açma sayfanız özelleştirdiğinizde gerçekleştirmeniz gerekebilir yaygın görevlerden bazıları hakkında ayrıntılar sağlar.

## <a name="customlogo"></a>Özel bir şirket logosu veya çizim ekleme 
Gösterilen şirketin logosunu değiştirmek için **oturum** sayfasında, aşağıdaki Windows PowerShell cmdlet'ini ve sözdizimini kullanın.

> [!NOTE]
> Önerilen logosu 260 x 35 boyutlardır \@ 96 DPI bir dosya boyutu 10 KB'den büyük olmaması.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> *TargetName* parametresi gereklidir. Varsayılan AD FS ile birlikte yayınlanan varsayılan temanın adı verilir.

## <a name="addsignindescription"></a>Oturum açma bir açıklama ekleyin 
İçin oturum açma sayfasına bir açıklama eklemek için **oturum açma sayfası**, aşağıdaki Windows PowerShell cmdlet'ini ve sözdizimini kullanın.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## <a name="modclaims"></a>AD FS talep kurallarını değiştirme 
AD FS özel talep kuralları oluşturmak için kullanabileceğiniz bir zengin talep dili destekler. Daha fazla bilgi için [talep kuralı dili rolü](https://technet.microsoft.com/library/dd807118.aspx).

Aşağıdaki bölümlerde, Azure AD ile ilgili bazı senaryolar için özel kurallar nasıl yazabilirsiniz ve AD FS federasyon.

### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Koşullu özniteliğinde mevcut olan bir değerini sabit kimlik
Azure AD Connect nesneleri Azure AD'ye eşitlendiğinde bir kaynak bağlantısı kullanılacak bir özniteliğini belirtmenize olanak sağlar. Özel öznitelik değeri boş değilse, sabit bir kimliği talebi vermek isteyebilirsiniz.

Örneğin, seçtiğiniz **ms-ds-consistencyguid içinde** sorun ve kaynak bağlantısı özniteliği olarak **Immutableıd** olarak **ms-ds-consistencyguid içinde** olasılığına karşı özniteliği bunlara karşı bir değer içeriyor. Öznitelik karşı herhangi bir değer varsa, sorunu **objectGUID** olarak sabit kimliği. Aşağıdaki bölümde açıklandığı gibi özel talep kuralları kümesi oluşturabilirsiniz.

**Kural 1: Sorgu öznitelikleri**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

Bu kuralda değerlerini sorguladığınız **ms-ds-consistencyguid içinde** ve **objectGUID** Active Directory'den kullanıcı. Bir AD FS dağıtımınıza uygun deposu adı için depo adını değiştirin. Aynı zamanda talep türü için uygun bir değişiklik için tanımlanan türü, Federasyon için talep **objectGUID** ve **ms-ds-consistencyguid içinde**.

Kullanarak ayrıca **ekleme** değil **sorunu**, varlık için giden sorun eklemekten kaçının ve ara değerler olarak değerleri kullanabilirsiniz. Sabit kimlik olarak kullanmak üzere hangi değeri sağladıktan sonra bir sonraki kural talebi verecek

**2. kural: MS-ds-consistencyguid içinde kullanıcı için mevcut olup olmadığını denetleyin**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Bu kural adlı geçici bir bayrak tanımlar **idflag** ayarlanmış **useguid** varsa hiçbir **ms-ds-consistencyguid içinde** kullanıcıdan doldurulur. Bunun ardındaki mantığı, AD FS boş talep izin vermeyen gerçeğidir. Talep eklediğinizde, bu nedenle http://contoso.com/ws/2016/02/identity/claims/objectguid ve http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid kural 1'de elde edersiniz bir **msdsconsistencyguid** değeri bir kullanıcı için doldurulur yalnızca talep. Doldurulmuş değil, AD FS boş bir değere sahip ve hemen bıraktığı görür. Tüm nesneleri olacaktır **objectGUID**, kural 1 yürütüldükten sonra bu talebi her zaman var olur.

**3. kural: Varsa, ms-ds-consistencyguid içinde sabit kimlik verme.**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Bu bir örtük olarak **mevcut** denetleyin. Ardından talep değeri varsa, sabit kimliği olarak sorunu Önceki örnekte **NameIdentifier** talep. Bunu ortamınızda sabit kimliği için uygun talep türüne değiştirmek zorunda kalırsınız.

**4. kural: MS-ds-consistencyguid içinde mevcut değilse, sabit kimlik objectGUID sorunu**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

Bu kural, yalnızca geçici bayrağı denetimi **idflag**. Talep, değerini temel alarak karar.

> [!NOTE]
> Bu kurallar sırası önemlidir.

### <a name="sso-with-a-subdomain-upn"></a>Bir alt etki alanı UPN ile SSO

Azure AD Connect kullanarak açıklandığı birleştirilecek birden fazla etki alanı ekleyebilirsiniz [yeni bir Federasyon etki alanına ekleme](how-to-connect-fed-management.md#addfeddomain). Azure AD Connect sürümü 1.1.553.0 ve son doğru talep kuralı oluşturur İssuerıd için otomatik olarak. Azure AD Connect sürümü 1.1.553.0 kullanamazsınız veya en son, önerilir [Azure AD RPT talep kuralları](https://aka.ms/aadrptclaimrules) aracı oluşturabilir ve Azure AD bağlı olan taraf güveni için doğru talep kurallarını ayarlamak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [kullanıcı oturum açma seçenekleri](plan-connect-user-signin.md).
