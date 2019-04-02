---
title: 'Öğretici: SharePoint şirket içi ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SharePoint şirket içi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 122234e164735270d5566dbaa5d3e8b2d49c141a
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58794011"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Öğretici: SharePoint şirket içi ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SharePoint şirket içi tümleştirme konusunda bilgi edinin.
SharePoint şirket içi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* SharePoint şirket içi erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak SharePoint şirket içi (çoklu oturum açma) ile kendi Azure AD hesapları için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

SharePoint şirket içi ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* SharePoint şirket tek oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SharePoint şirket içi destekler **SP** tarafından başlatılan

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>SharePoint şirket içi galeri ekleme

Azure AD'de şirket SharePoint tümleştirmesini yapılandırmak için SharePoint şirket içi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SharePoint şirket içi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SharePoint şirket içi**seçin **SharePoint şirket içi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![SharePoint şirket içi sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SharePoint adlı bir test kullanıcı şirket tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı SharePoint'te arasında bir bağlantı ilişki kurulması gerekiyorsa şirket içi.

Yapılandırma ve Azure AD çoklu oturum açma SharePoint şirket içi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SharePoint şirket içi çoklu oturum açmayı yapılandırma](#configure-sharepoint-on-premises-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure portalda bir Azure AD güvenlik grubu oluşturma](#create-an-azure-ad-security-group-in-the-azure-portal)**  - Azure AD çoklu oturum açma için yeni bir güvenlik grubu etkinleştirmek için.
5. **[SharePoint için erişim verme şirket içi güvenlik grubu](#grant-access-to-sharepoint-on-premises-security-group)**  -belirli bir grubu Azure AD'ye erişim.
6. **[Azure Portal'da Azure AD güvenlik grubu atayın](#assign-the-azure-ad-security-group-in-the-azure-portal)**  - Azure AD kimlik doğrulaması için belirli bir gruba atayın.
7. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

SharePoint şirket içi ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **SharePoint şirket içi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SharePoint şirket içi etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<YourSharePointServerURL>/_trust/default.aspx`

    b. İçinde **tanımlayıcı** kutusuna şu biçimi kullanarak bir URL yazın: `urn:sharepoint:federation`

    c. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<YourSharePointServerURL>/_trust/default.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [SharePoint şirket içi istemci Destek ekibine](https://support.office.com/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

    > [!Note]
    > Lütfen yapılandırma için PowerShell betik daha sonra kullanmak gereken sertifika dosyası, yüklediğiniz dosya yolu unutmayın.

6. Üzerinde **SharePoint şirket içi kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın. İçin **çoklu oturum açma hizmeti URL'si**, değeri şu biçimi kullanın: `https://login.microsoftonline.com/_my_directory_id_/wsfed`

    > [!Note]
    > _my_directory_id_ Azure Ad aboneliğinin Kiracı kimliği.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

    > [!NOTE]
    > SharePoint şirket içi uygulamanın kullandığı SAML 1.1 SAML 1.1 verdiği belirteci için Azure AD, SharePoint server ve kimlik doğrulamasından sonra WS Fed isteği bekliyor. belirteci.

### <a name="configure-sharepoint-on-premises-single-sign-on"></a>SharePoint şirket içi çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde bir SharePoint şirket içi şirket sitenize yönetici olarak oturum açın.

2. **SharePoint Server 2016'da yeni bir güvenilen kimlik sağlayıcı yapılandırma**

    SharePoint Server 2016 sunucuya oturum açın ve SharePoint 2016 Yönetim Kabuğu'nu açın. $Wsfedurl (çoklu oturum açma hizmeti URL'si) ve $filepath $realm (tanımlayıcı değeri) bölümünden SharePoint şirket içi etki alanı ve URL'ler Azure portalında, değerleri doldurun (sertifika dosyası karşıdan dosya yolu) Azure portal ve çalıştırma Yeni bir güvenilen kimlik sağlayıcı yapılandırmak için aşağıdaki komutları.

    > [!TIP]
    > PowerShell kullanarak yeni ya da PowerShell işleyişi hakkında daha fazla bilgi istiyorsanız bkz [SharePoint PowerShell](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps).

    ```powershell
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have downloaded from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $map4 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress" -IncomingClaimTypeDisplayName "Email" -SameAsIncoming
    $map5 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.microsoft.com/ws/2008/06/identity/claims/role" -IncomingClaimTypeDisplayName "Role" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3,$map4 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Ardından, uygulamanız için güvenilen kimlik sağlayıcı etkinleştirmek için bu adımları izleyin:

    a. Merkezi Yönetim'de gidin **Web uygulamasını Yönet** ve Azure AD ile güvenli hale getirmek için istediğiniz web uygulamasını seçin.

    b. Şeritte tıklayın **kimlik doğrulama sağlayıcıları** ve kullanmak istediğiniz bölgeyi seçin.

    c. Seçin **güvenilen kimlik sağlayıcı** adlı yeni kaydettiğiniz kimlik sağlayıcısı seçip *AzureAD*.

    d. Oturum açma sayfası URL'sini ayarda seçin **özel oturum açma sayfası** ve "/_trust/" değerini sağlayın.

    e. **Tamam** düğmesine tıklayın.

    ![Kimlik doğrulama sağlayıcısını yapılandırma](./media/sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

    > [!NOTE]
    > Bazı dış kullanıcıların UPN değeri şuna benzer karıştırılmış olarak bu tek oturum açma tümleştirmesi kullanılmak üzere mümkün olacaktır `MYEMAIL_outlook.com#ext#@TENANT.onmicrosoft.com`. Yakında size konfigurace UPN kullanıcı türüne bağlı olarak işlemek nasıl olanak sağlayacak. Bundan sonra tüm Konuk kullanıcılar SSO sorunsuz bir şekilde kuruluş çalışanları kullanabilmek için olmalıdır.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="create-an-azure-ad-security-group-in-the-azure-portal"></a>Azure portalında bir Azure AD güvenlik grubu oluşturma

1. Tıklayarak **Azure Active Directory > tüm gruplar**.

    ![Bir Azure AD güvenlik grubu oluşturma](./media/sharepoint-on-premises-tutorial/allgroups.png)

2. Tıklayın **yeni grup**:

    ![Bir Azure AD güvenlik grubu oluşturma](./media/sharepoint-on-premises-tutorial/newgroup.png)

3. Doldurun **grup türü**, **grup adı**, **Grup açıklaması**, **üyelik türü**. Üye, seçin sonra arayın veya gruba eklemek istediğiniz üye tıklayarak için oka tıklayın. Tıklayarak **seçin** seçilen üyeleri eklemek için ardından **Oluştur**.

    ![Bir Azure AD güvenlik grubu oluşturma](./media/sharepoint-on-premises-tutorial/addingmembers.png)

    > [!NOTE]
    > Şirket içi SharePoint için Azure Active Directory güvenlik gruplarına atamak için yüklemek ve yapılandırmak gerekli olacaktır [AzureCP](https://yvand.github.io/AzureCP/) şirket içi SharePoint grubu, geliştirme veya alternatif bir özel talep yapılandırma SharePoint için sağlayıcı.  AzureCP kullanmazsanız, kendi özel talep sağlayıcısı oluşturmak için belgenin sonundaki daha fazla bilgi bölümüne bakın.

### <a name="grant-access-to-sharepoint-on-premises-security-group"></a>SharePoint için erişim verme şirket güvenlik grubu

**Uygulama kaydı üzerinde güvenlik grupları ve izinleri yapılandırma**

1. Azure portalında **Azure Active Directory**, ardından **uygulama kayıtları**.

    ![Kurumsal uygulamalar dikey penceresi](./media/sharepoint-on-premises-tutorial/appregistrations.png)

2. Arama kutusuna yazın ve **SharePoint şirket içi**.

    ![SharePoint şirket içi sonuç listesinde](./media/sharepoint-on-premises-tutorial/appsearch.png)

3. Tıklayarak **bildirim**.

    ![Bildirim seçeneği](./media/sharepoint-on-premises-tutorial/manifest.png)

4. Değiştirme `groupMembershipClaims`: `NULL`, `groupMembershipClaims`: `SecurityGroup`. ' A tıklayarak kaydederken

    ![Bildirimi Düzenle](./media/sharepoint-on-premises-tutorial/manifestedit.png)

5. Tıklayarak **ayarları**, ardından **gerekli izinler**.

    ![Gerekli izinler](./media/sharepoint-on-premises-tutorial/settings.png)

6. Tıklayarak **ekleme** ardından **bir API seçin**.

    ![API erişim](./media/sharepoint-on-premises-tutorial/required_permissions.png)

7. Her ikisini de Ekle **Windows Azure Active Directory** ve **Microsoft Graph API**, ama yalnızca teker teker seçmek mümkün olur.

    ![API seçin](./media/sharepoint-on-premises-tutorial/permissions.png)

8. Windows Azure Active Directory'yi seçin, dizin verilerini okuma denetleyin ve Seç'e tıklayın. Geri dönün ve Microsoft Graph ekleyin ve dizin verilerini okuma onun için de seçin.  Seç'e tıklayın ve tıklayarak bitti.

    ![Erişimi Etkinleştir](./media/sharepoint-on-premises-tutorial/readpermission.png)

9. Gerekli ayarları şimdi tıklayın **izinleri verin** ve ardından tıklayın Evet izin verin.

    ![İzin Ver](./media/sharepoint-on-premises-tutorial/grantpermission.png)

    > [!NOTE]
    > İzinler başarıyla verildi olmadığını belirlemek için bildirimleri altında kontrol edin.  Değilse, AzureCP düzgün çalışmaz ve SharePoint şirket içi Azure Active Directory güvenlik grupları ile yapılandırmak mümkün olmayacaktır.

10. SharePoint şirket içi grubu ya da alternatif bir özel talep sağlayıcısı çözümünü AzureCP yapılandırın.  Bu örnekte, AzureCP kullanıyoruz.

    > [!NOTE]
    > Lütfen AzureCP değil bir Microsoft ürünü veya Microsoft teknik destek birimi tarafından desteklenen unutmayın. İndirme, yükleme ve şirket içi SharePoint grubu AzureCP yapılandırın https://yvand.github.io/AzureCP/ 

11. **Şirket içi SharePoint Azure Active Directory güvenlik grubuna erişim izni** :-gruplara erişim verilmesi SharePoint uygulamasına üzerinde-permise.  Web uygulamasına erişmek için izinleri ayarlamak için aşağıdaki adımları kullanın.

12. Merkezi Yönetim Uygulama Yönetimi, web uygulamalarını yönetme ve ardından Şeritteki etkinleştirin ve kullanıcı ilkesini web uygulamasını seçin.

    ![Merkezi Yönetim](./media/sharepoint-on-premises-tutorial/centraladministration.png)

13. Web uygulaması için ilke altında kullanıcılar ekleyin ve ardından İleri tıklayarak bölgesi seçin tıklayın.  Adres Defteri tıklayın.

    ![Web uygulaması için ilke](./media/sharepoint-on-premises-tutorial/webapp-policy.png)

14. Arayın ve Azure Active Directory güvenlik grubuna ekleyin ve Tamam'ı tıklatın.

    ![Güvenlik grubu ekleme](./media/sharepoint-on-premises-tutorial/securitygroup.png)

15. İzinleri seçin ve ardından Son'a tıklayın.

    ![Güvenlik grubu ekleme](./media/sharepoint-on-premises-tutorial/permissions1.png)

16. Web uygulaması, Azure Active Directory grubuna eklenen ilke altında bakın.  Grup talebi, Azure Active Directory güvenlik grubu nesne kimliği için kullanıcı adı gösterir.

    ![Güvenlik grubu ekleme](./media/sharepoint-on-premises-tutorial/addgroup.png)

17. SharePoint site koleksiyonuna gidin ve burada, grubun de ekleyin. Site Ayarları'nı tıklatın ve ardından Site izinleri ve İzinler'e tıklayın.  İçin Grup rol talep, izin düzeyi atayın ve Paylaşım'ye tıklayın.

    ![Güvenlik grubu ekleme](./media/sharepoint-on-premises-tutorial/grantpermission1.png)

### <a name="configuring-one-trusted-identity-provider-for-multiple-web-applications"></a>Birden çok web uygulaması için bir güvenilen kimlik sağlayıcı yapılandırma

Yapılandırma, tek bir web uygulaması için çalışır, ancak aynı güvenilen kimlik sağlayıcı birden çok web uygulamaları için kullanmak istiyorsanız, ek yapılandırma gerekir. Örneğin, biz URL'sini kullanmak üzere bir web uygulaması genişletilmiş varsayılır `https://portal.contoso.local` ve kullanıcıların kimliğini doğrulamak artık istediğiniz `https://sales.contoso.local` de. Bunu yapmak için biz WReply parametresi ve bunları Azure AD'de yanıt URL'si eklemek için uygulama kaydı güncelleştirmek için kimlik sağlayıcısı güncelleştirmeniz gerekir.

1. Azure Portalı'nda Azure AD dizinini açın. Tıklayın **uygulama kayıtları**, ardından **tüm uygulamaları görüntüle**. Daha önce oluşturduğunuz uygulama (SharePoint SAML tümleştirme).

2. Tıklayın **ayarları**.

3. Ayarlar dikey penceresinde **yanıt URL'leri**.

4. Ek web uygulamasının URL'sini ekleyin `/_trust/default.aspx` URL'sine eklenir (gibi `https://sales.contoso.local/_trust/default.aspx`) tıklayıp **Kaydet**.

5. SharePoint sunucusunda açın **SharePoint 2016 Yönetim Kabuğu'nu** ve daha önce kullandığınız güvenilen kimlik belirteci veren adını kullanarak aşağıdaki komutları yürütün.

    ```powershell
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```

6. Merkezi Yönetim web uygulamasına gidin ve mevcut güvenilen kimlik sağlayıcı etkinleştirin. Ayrıca özel bir oturum açma sayfası olarak oturum açma sayfası URL'sini yapılandırmayı unutmayın `/_trust/`.

7. Merkezi Yönetim web uygulamasına tıklayıp seçin **kullanıcı ilkesi**. Bu makalede daha önce gösterildiği gibi uygun izinlere sahip bir kullanıcı ekleyin.

### <a name="fixing-people-picker"></a>Kişi Seçici düzeltme

Kullanıcılar artık SharePoint 2016'yı kullanarak Azure ad kimlikleri üzerinde oturum açabileceğiniz, ancak yine de kullanıcı deneyimini geliştirme vardır. Örneğin, bir kullanıcı için arama birden çok arama sonuçları Kişi Seçici gösterir. Talep eşleme içinde oluşturulan 3 talep türlerinin her biri için bir arama sonucu yok. Kişi Seçici'yi kullanarak bir kullanıcı seçmek için tam kullanıcı adı girin ve seçin **adı** sonucu talep.

![Talep arama sonuçları](./media/sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Doğrulama yoktur aramak için değerleri için yazım hatası açabilir veya kazayla yanlış seçme kullanıcıların talep türü gibi atamak için **Soyadı** talep. Bu, kullanıcıların kaynakları başarılı bir şekilde erişmesini engelleyebilir.

Bu senaryo ile yardımcı olmak için yoktur açık kaynaklı adlı çözüm [AzureCP](https://yvand.github.io/AzureCP/) , SharePoint 2016 için bir özel talep sağlayıcı sağlar. Hangi kullanıcıların girin ve gerçekleştirme gidermek için Azure AD Graph kullanacağı doğrulama. Daha fazla bilgi [AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-security-group-in-the-azure-portal"></a>Azure portalında Azure AD güvenlik grubu atayın

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SharePoint şirket içi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **SharePoint şirket içi**.

    ![Uygulamalar listesinden SharePoint şirket içi bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle**.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanın ve Select üyeleri bölümüne eklemek için Grup tıklayarak istediğiniz güvenlik grubunu arayın. Tıklayın **seçin**, ardından **atama**.

    ![Arama güvenlik grubu](./media/sharepoint-on-premises-tutorial/securitygroup1.png)

    > [!NOTE]
    > Grup Azure Portal'da Kurumsal uygulamasına başarıyla atanıp bildirilmesi için menü çubuğunda bildirimler denetleyin.

### <a name="create-sharepoint-on-premises-test-user"></a>SharePoint şirket içi test kullanıcısı oluşturma

Bu bölümde, şirket içinde SharePoint Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [SharePoint şirket içi Destek ekibine](https://support.office.com/) SharePoint şirket içi platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SharePoint şirket içi kutucuğa tıkladığınızda, size otomatik olarak SharePoint SSO'yu ayarlama şirket içine oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)