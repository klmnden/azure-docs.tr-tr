---
title: 'Öğretici: Azure Active Directory Tümleştirme ile SharePoint şirket içi | Microsoft Docs'
description: Azure Active Directory ve SharePoint şirket içi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2018
ms.author: jeedes
ms.openlocfilehash: 31f4c64296c180ac441f2fb691514a4b9fb35a57
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51687206"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Öğretici: SharePoint şirket içi ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SharePoint şirket içi tümleştirme konusunda bilgi edinin.

SharePoint şirket içi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SharePoint şirket içi erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak SharePoint şirket içi (çoklu oturum açma) ile Azure AD hesaplarına açan, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

SharePoint şirket içi ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir SharePoint şirket tek oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. SharePoint şirket içi galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>SharePoint şirket içi galeri ekleme

Azure AD'de şirket SharePoint tümleştirmesini yapılandırmak için SharePoint şirket içi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SharePoint şirket içi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SharePoint şirket içi**seçin **SharePoint şirket içi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![SharePoint şirket içi sonuç listesinde](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve SharePoint "Britta Simon" adlı bir test kullanıcı şirket tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı SharePoint şirket içi bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı SharePoint'te arasında bir bağlantı ilişki kurulması gerekiyorsa şirket içi.

Yapılandırma ve Azure AD çoklu oturum açma SharePoint şirket içi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SharePoint şirket içi test kullanıcısı için erişim verin](#grant-access-to-sharePoint-on-premises-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı bir karşılığı Britta simon'un SharePoint şirket içi sağlamak için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SharePoint şirket içi uygulamanızda çoklu oturum açmayı yapılandırma.

**SharePoint şirket içi ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SharePoint şirket içi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_samlbase.png)

3. Üzerinde **SharePoint şirket içi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SharePoint şirket içi etki alanı ve URL'ler tek oturum açma bilgileri](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_url1.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<YourSharePointServerURL>/_trust/default.aspx`

    b. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `urn:sharepoint:federation`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<YourSharePointServerURL>/_trust/default.aspx`

4. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_certificate.png)

    > [!Note]
    > Lütfen yapılandırma için PowerShell betik daha sonra kullanmak gereken sertifika dosyası, yüklediğiniz dosya yolu unutmayın.

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media\sharepoint-on-premises-tutorial/tutorial_general_400.png)

6. Üzerinde **SharePoint şirket içi yapılandırma** bölümünde **yapılandırma SharePoint şirket içi** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği** gelen **hızlı başvuru bölümü.** İçin **çoklu oturum açma hizmeti URL'si**, değeri şu biçimi kullanın: `https://login.microsoftonline.com/_my_directory_id_/wsfed` 

    > [!Note]
    > _my_directory_id_ Azure Ad aboneliğinin Kiracı kimliği.

    ![SharePoint şirket içi yapılandırma](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_configure.png)

    > [!NOTE]
    > SharePoint şirket içi uygulamanın kullandığı SAML 1.1 SAML 1.1 verdiği belirteci için Azure AD, SharePoint server ve kimlik doğrulamasından sonra WS Fed isteği bekliyor. belirteci.

7. Farklı bir web tarayıcı penceresinde bir SharePoint şirket içi şirket sitenize yönetici olarak oturum açın.

8. **SharePoint Server 2016'da yeni bir güvenilen kimlik sağlayıcı yapılandırma**

    SharePoint Server 2016 sunucuya oturum açın ve SharePoint 2016 Yönetim Kabuğu'nu açın. $Wsfedurl (çoklu oturum açma hizmeti URL'si) ve $filepath $realm (tanımlayıcı değeri) bölümünden SharePoint şirket içi etki alanı ve URL'ler Azure portalında, değerleri doldurun (sertifika dosyası karşıdan dosya yolu) Azure portal ve çalıştırma Yeni bir güvenilen kimlik sağlayıcı yapılandırmak için aşağıdaki komutları.

    > [!TIP]
    > PowerShell kullanarak yeni ya da PowerShell işleyişi hakkında daha fazla bilgi istiyorsanız bkz [SharePoint PowerShell](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps). 

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have downloaded from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $map4 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress" -IncomingClaimTypeDisplayName "Email" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3,$map4 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Ardından, uygulamanız için güvenilen kimlik sağlayıcı etkinleştirmek için bu adımları izleyin:

    a. Merkezi Yönetim'de gidin **Web uygulamasını Yönet** ve Azure AD ile güvenli hale getirmek için istediğiniz web uygulamasını seçin.

    b. Şeritte tıklayın **kimlik doğrulama sağlayıcıları** ve kullanmak istediğiniz bölgeyi seçin.

    c. Seçin **güvenilen kimlik sağlayıcı** adlı yeni kaydettiğiniz kimlik sağlayıcısı seçip *AzureAD*.

    d. Oturum açma sayfası URL'sini ayarda seçin **özel oturum açma sayfası** ve "/_trust/" değerini sağlayın.

    e. **Tamam** düğmesine tıklayın.

    ![Kimlik doğrulama sağlayıcısını yapılandırma](./media\sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

    > [!NOTE]
    > Bazı dış kullanıcıların UPN değeri şuna benzer karıştırılmış olarak bu tek oturum açma tümleştirmesi kullanılmak üzere mümkün olacaktır `MYEMAIL_outlook.com#ext#@TENANT.onmicrosoft.com`. Yakında size konfigurace UPN kullanıcı türüne bağlı olarak işlemek nasıl olanak sağlayacak. Bundan sonra tüm Konuk kullanıcılar SSO sorunsuz bir şekilde kuruluş çalışanları kullanabilmek için olmalıdır.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media\sharepoint-on-premises-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media\sharepoint-on-premises-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media\sharepoint-on-premises-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media\sharepoint-on-premises-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="grant-access-to-sharepoint-on-premises-test-user"></a>SharePoint şirket içi test kullanıcısı için erişim izni ver

Azure AD ile oturum açın ve SharePoint erişimi kullanıcılar erişim verilmesi uygulamayı web uygulamasına erişmek için izinleri ayarlamak için aşağıdaki adımları kullanın.

1. Merkezi Yönetim'de tıklayın **Uygulama Yönetimi**.

2. Üzerinde **Uygulama Yönetimi** sayfasında **Web uygulamaları** bölümünde **web uygulamalarını yönetme**.

3. Uygun web uygulamasını tıklatın ve ardından **kullanıcı ilkesi**.

4. Web uygulaması için ilke içinde tıklayın **Add Users**.

    ![Kendi adı talebi tarafından bir kullanıcı için arama](./media\sharepoint-on-premises-tutorial/fig11-searchbynameclaim.png)

5. İçinde **Add Users** iletişim kutusunda, uygun bölgeyi **bölgeleri**ve ardından **sonraki**.

6. İçinde **Web uygulaması için ilke** iletişim kutusundaki **kullanıcıların seçmesine** bölümünde **Gözat** simgesi.

7. İçinde **Bul** metin kutusuna **kullanıcı asıl adı (UPN)** hangi SharePoint şirket içi uygulama, Azure AD'de yapılandırdıysanız ve tıklayın değeri **arama**. </br>Örnek: *brittasimon@contoso.com*.

8. Liste görünümünde AzureAD başlığı, name özelliği seçin ve **Ekle** ardından **Tamam** iletişim kutusunu kapatmak için.

9. İzinler'de tıklayın **tam denetim**.

    ![Bir talep kullanıcıya tam denetim izni verme](./media\sharepoint-on-premises-tutorial/fig12-grantfullcontrol.png)

10. Tıklayın **son**ve ardından **Tamam**.

### <a name="configuring-one-trusted-identity-provider-for-multiple-web-applications"></a>Birden çok web uygulaması için bir güvenilen kimlik sağlayıcı yapılandırma

Yapılandırma, tek bir web uygulaması için çalışır, ancak aynı güvenilen kimlik sağlayıcı birden çok web uygulamaları için kullanmak istiyorsanız, ek yapılandırma gerekir. Örneğin, biz URL'sini kullanmak üzere bir web uygulaması genişletilmiş varsayılır `https://portal.contoso.local` ve kullanıcıların kimliğini doğrulamak artık istediğiniz `https://sales.contoso.local` de. Bunu yapmak için biz WReply parametresi ve bunları Azure AD'de yanıt URL'si eklemek için uygulama kaydı güncelleştirmek için kimlik sağlayıcısı güncelleştirmeniz gerekir.

1. Azure Portalı'nda Azure AD dizinini açın. Tıklayın **uygulama kayıtları**, ardından **tüm uygulamaları görüntüle**. Daha önce oluşturduğunuz uygulama (SharePoint SAML tümleştirme).

2. Tıklayın **ayarları**.

3. Ayarlar dikey penceresinde **yanıt URL'leri**. 

4. Ek web uygulamasının URL'sini ekleyin `/_trust/default.aspx` URL'sine eklenir (gibi `https://sales.contoso.local/_trust/default.aspx`) tıklayıp **Kaydet**.

5. SharePoint sunucusunda açın **SharePoint 2016 Yönetim Kabuğu'nu** ve daha önce kullandığınız güvenilen kimlik belirteci veren adını kullanarak aşağıdaki komutları yürütün.

    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```
6. Merkezi Yönetim web uygulamasına gidin ve mevcut güvenilen kimlik sağlayıcı etkinleştirin. Ayrıca özel bir oturum açma sayfası olarak oturum açma sayfası URL'sini yapılandırmayı unutmayın `/_trust/`.

7. Merkezi Yönetim web uygulamasına tıklayıp seçin **kullanıcı ilkesi**. Bu makalede daha önce gösterildiği gibi uygun izinlere sahip bir kullanıcı ekleyin.

### <a name="fixing-people-picker"></a>Kişi Seçici düzeltme

Kullanıcılar artık SharePoint 2016'yı kullanarak Azure ad kimlikleri üzerinde oturum açabileceğiniz, ancak yine de kullanıcı deneyimini geliştirme vardır. Örneğin, bir kullanıcı için arama birden çok arama sonuçları Kişi Seçici gösterir. Talep eşleme içinde oluşturulan 3 talep türlerinin her biri için bir arama sonucu yok. Kişi Seçici'yi kullanarak bir kullanıcı seçmek için tam kullanıcı adı girin ve seçin **adı** sonucu talep.

![Talep arama sonuçları](./media\sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Doğrulama yoktur aramak için değerleri için yazım hatası açabilir veya kazayla yanlış seçme kullanıcıların talep türü gibi atamak için **Soyadı** talep. Bu, kullanıcıların kaynakları başarılı bir şekilde erişmesini engelleyebilir.

Bu senaryo ile yardımcı olmak için yoktur açık kaynaklı adlı çözüm [AzureCP](https://yvand.github.io/AzureCP/) , SharePoint 2016 için bir özel talep sağlayıcı sağlar. Hangi kullanıcıların girin ve gerçekleştirme gidermek için Azure AD Graph kullanacağı doğrulama. Daha fazla bilgi [AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SharePoint şirket içi erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**SharePoint şirket içi Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **SharePoint şirket içi**.

    ![Uygulamalar listesinden SharePoint bağlantısı](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SharePoint şirket içi kutucuğa tıkladığınızda, otomatik olarak oturum açma için SharePoint şirket içi uygulamanızın almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [SharePoint sunucu kimlik doğrulaması için Azure AD kullanma](https://docs.microsoft.com/office365/enterprise/using-azure-ad-for-sharepoint-server-authentication)

<!--Image references-->

[1]: ./media\sharepoint-on-premises-tutorial/tutorial_general_01.png
[2]: ./media\sharepoint-on-premises-tutorial/tutorial_general_02.png
[3]: ./media\sharepoint-on-premises-tutorial/tutorial_general_03.png
[4]: ./media\sharepoint-on-premises-tutorial/tutorial_general_04.png

[100]: ./media\sharepoint-on-premises-tutorial/tutorial_general_100.png

[200]: ./media\sharepoint-on-premises-tutorial/tutorial_general_200.png
[201]: ./media\sharepoint-on-premises-tutorial/tutorial_general_201.png
[202]: ./media\sharepoint-on-premises-tutorial/tutorial_general_202.png
[203]: ./media\sharepoint-on-premises-tutorial/tutorial_general_203.png
