---
title: 'Öğretici: Azure Active Directory Tümleştirme SharePoint şirket içi | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve SharePoint şirket içi arasında yapılandırmayı öğrenin.
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
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 1657d686337d067a8df9107b7d102fc34c1f1b8d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655292"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Öğretici: Azure Active Directory Tümleştirme SharePoint şirket içi

Bu öğreticide, Azure Active Directory (Azure AD) ile SharePoint şirket içi tümleştirme öğrenin.

SharePoint şirket içi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SharePoint şirket içi erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak SharePoint şirket içi (çoklu oturum açma) Azure AD hesaplarına sahip açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme SharePoint şirket içi ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SharePoint tek oturum açma etkin abonelik şirket içi

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. SharePoint şirket içi Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>SharePoint şirket içi Galeriden ekleme
SharePoint şirket içi tümleştirme Azure AD'ye yapılandırmak için SharePoint şirket içi Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**SharePoint şirket içi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SharePoint şirket içi**seçin **SharePoint şirket içi** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![SharePoint şirket içi sonuçlar listesinde](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve Azure AD çoklu oturum açma SharePoint "Britta Simon" adlı bir test kullanıcı şirket tabanlı sınayın.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen SharePoint şirket içi bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı SharePoint arasında bir bağlantı ilişkisi kurulmasını gereksinimlerini şirket içi.

Yapılandırmak ve Azure AD çoklu oturum açma SharePoint şirket içi sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SharePoint şirket içi test kullanıcısı oluşturma](#create-a-sharePoint-on-premises-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı bir karşılık gelen Britta Simon, SharePoint şirket içi sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SharePoint şirket içi uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma SharePoint şirket içi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SharePoint şirket içi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_samlbase.png)

3. Üzerinde **SharePoint şirket içi etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SharePoint şirket içi etki alanı ve URL'leri tek oturum açma bilgileri](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_url1.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<YourSharePointServerURL>/_trust/default.aspx`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `urn:sharepoint:<YourSharePointServerURL>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [SharePoint şirket içi istemci destek ekibi](https://support.office.com/) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve bilgisayarınızda meta veri dosyası .cer uzantılı olarak kaydedin. Kopyalayın ve indirilen meta veri dosyasının tam yolu Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_certificate.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_400.png)

6. Üzerinde **SharePoint şirket içi yapılandırma** 'yi tıklatın **yapılandırma SharePoint şirket içi** açmak için **yapılandırma oturum açma** penceresi. Kopya **çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![SharePoint şirket içi yapılandırma](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_configure.png)

    > [!NOTE]
    > SharePoint şirket içi uygulama kullanan SAML 1.1 SAML 1.1 verdiği WS Fed istek SharePoint server ve kimlik doğrulamasından sonra Azure AD bekliyor şekilde belirteci. belirteç.

7. Farklı web tarayıcısı penceresinde SharePoint şirket içi şirket siteniz için bir yönetici olarak oturum açın.

8. **SharePoint Server 2016'da yeni bir güvenilir kimlik sağlayıcı yapılandırma**

    SharePoint Server 2016 sunucuda oturum açın ve SharePoint 2016 Yönetim Kabuğu'nu açın. $Realm, $wsfedurl ve Azure portalından $filepath değerlerini doldurun ve yeni güvenilen kimlik sağlayıcısı yapılandırmak için aşağıdaki komutları çalıştırın.

    > [!TIP]
    > PowerShell kullanarak yeni ya da istediğiniz PowerShell nasıl çalıştığı hakkında daha fazla bilgi için bkz: [SharePoint PowerShell](https://docs.microsoft.com/en-us/powershell/sharepoint/overview?view=sharepoint-ps). 

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have copied from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Ardından, uygulamanız için güvenilen kimlik sağlayıcısı etkinleştirmek için şu adımları izleyin:

    a. Merkezi Yönetim'deki gidin **yönetmek Web uygulaması** ve Azure AD ile güvenli hale getirmek istediğiniz web uygulamasını seçin.

    b. Şeritte tıklatın **kimlik doğrulama sağlayıcıları** ve kullanmak istediğiniz bölgeyi seçin.

    c. Seçin **güvenilen kimlik sağlayıcısı** adlı yalnızca kayıtlı tanımla sağlayıcısı seçip *Azuread'i*.

    d. Oturum açma sayfası URL'si ayarı seçin **özel oturum açma sayfası** ve "/_trust/" değeri belirtin.

    e. **Tamam**’a tıklayın.

    ![Kimlik doğrulama sağlayıcısı yapılandırma](./media\active-directory-saas-sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media\active-directory-saas-sharepoint-on-premises-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media\active-directory-saas-sharepoint-on-premises-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media\active-directory-saas-sharepoint-on-premises-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media\active-directory-saas-sharepoint-on-premises-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-sharepoint-on-premises-test-user"></a>SharePoint şirket içi test kullanıcısı oluşturma

1. Merkezi Yönetim'deki tıklatın **Uygulama Yönetimi**.

2. Üzerinde **Uygulama Yönetimi** sayfasında **Web uygulamaları** 'yi tıklatın **web uygulamalarını yönetme**.

3. Uygun web uygulaması'nı tıklatın ve ardından **kullanıcı ilkesi**.

4. Web uygulaması için ilkesinde tıklatın **Kullanıcı Ekle**.

    ![Bir kullanıcı tarafından kendi adı talebi arama](./media\active-directory-saas-sharepoint-on-premises-tutorial/fig11-searchbynameclaim.png)

5. İçinde **Kullanıcı Ekle** iletişim kutusunda, uygun bölgeyi **bölgeleri**ve ardından **sonraki**.

6. İçinde **Web uygulaması için ilke** iletişim kutusunda **seçmelerini** 'yi tıklatın **Gözat** simgesi.

7. İçinde **Bul** textbox, dizininizdeki bir kullanıcı oturum açma adını yazın ve tıklatın **arama**. </br>Örnek: *demouser@blueskyabove.onmicrosoft.com*.

8. Azuread'i başlığı altında liste görünümü, name özelliği seçin ve **Ekle** ardından **Tamam** iletişim kutusunu kapatmak için.

9. İzinler'i tıklatın **tam denetim**.

    ![Bir talep kullanıcıya tam denetim izni verme](./media\active-directory-saas-sharepoint-on-premises-tutorial/fig12-grantfullcontrol.png)

10. Tıklatın **son**ve ardından **Tamam**.

### <a name="fixing-people-picker"></a>Kişi Seçici çözme

Kullanıcılar artık SharePoint 2016 kullanarak Azure AD'den kimlikleri içine oturum açabilir ancak hala kullanıcı deneyimine yönelik geliştirme olanaklarını vardır. Örneğin, bir kullanıcı için arama birden çok arama sonuçları kişiler Seçici'de sunar. Talep eşleme içinde oluşturulan 3 talep türlerinin her biri için bir arama sonuç yok. Kişiler seçicisini kullanarak bir kullanıcı seçmek için tam kullanıcı adı yazın ve seçin **adı** sonuç talep.

![Talep arama sonuçları](./media\active-directory-saas-sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Doğrulama olmaz aramak için değerleri yazım hatası için yol açabilir veya kazayla yanlış seçme kullanıcıların talep türü gibi atamak için **Soyadı** talep. Bu, kullanıcıların kaynakları başarılı bir şekilde erişmesini engelleyebilir.

Bu senaryoyla yardımcı olmak için yoktur açık kaynaklı adlı çözüm [AzureCP](https://yvand.github.io/AzureCP/) SharePoint 2016 için bir özel talep sağlayıcı sağlar. Hangi kullanıcıların girin ve gerçekleştirmek çözmek için Azure AD grafik kullanacağı doğrulama. Hakkında daha fazla bilgi edinin [AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta SharePoint şirket içi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SharePoint şirket içi Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **SharePoint şirket içi**.

    ![Uygulamalar listesinde SharePoint bağlantısı](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_app.png)

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SharePoint şirket içi parçasında tıkladığınızda, otomatik olarak oturum açma SharePoint şirket içi uygulamanızı almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_01.png
[2]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_02.png
[3]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_03.png
[4]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_04.png

[100]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_100.png

[200]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_200.png
[201]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_201.png
[202]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_202.png
[203]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_203.png

