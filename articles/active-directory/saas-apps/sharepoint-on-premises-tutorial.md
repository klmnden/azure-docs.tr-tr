---
title: 'Öğretici: SharePoint şirket içi ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SharePoint şirket içi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7d3cb6e3ba1aac9c225abc107887525fc0bbae9d
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57780618"
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
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SharePoint şirket içi test kullanıcısı için erişim verin](#grant-access-to-sharepoint-on-premises-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı bir karşılığı Britta simon'un SharePoint şirket içi sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

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
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcıya ve yanıt URL'si ile güncelleştirin. İlgili kişi [SharePoint şirket içi istemci Destek ekibine](https://support.office.com/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

    > [!Note]
    > Sertifika dosyasını indirdiğiniz dosya yolunu not alın. Yapılandırma için PowerShell betiğini dosyasında daha sonra ihtiyacınız var.

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

    ![Kimlik doğrulama sağlayıcısını yapılandırma](./media/sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

    > [!NOTE]
    > Bazı dış kullanıcılar UPN gibi karıştırılmış bir değere sahip olabilir, bu tek oturum açma tümleştirmesi kullanmanız mümkün olmayacaktır `MYEMAIL_outlook.com#ext#@TENANT.onmicrosoft.com`. Yakında size özel uygulama yapılandırması, kullanıcı türüne bağlı olarak UPN işlemek izin verir. Bundan sonra tüm Konuk kullanıcılar SSO sorunsuz bir şekilde kuruluş çalışanları kullanabilmek için olmalıdır.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="grant-access-to-sharepoint-on-premises-test-user"></a>SharePoint şirket içi test kullanıcısı için erişim izni ver

Azure AD ile oturum açın ve SharePoint erişimi kullanıcılar erişim verilmesi uygulamayı web uygulamasına erişmek için izinleri ayarlamak için aşağıdaki adımları kullanın.

1. Merkezi Yönetim'de tıklayın **Uygulama Yönetimi**.

2. Üzerinde **Uygulama Yönetimi** sayfasında **Web uygulamaları** bölümünde **web uygulamalarını yönetme**.

3. Uygun web uygulamasını tıklatın ve ardından **kullanıcı ilkesi**.

4. Web uygulaması için ilke içinde tıklayın **Add Users**.

    ![Kendi adı talebi tarafından bir kullanıcı için arama](./media/sharepoint-on-premises-tutorial/fig11-searchbynameclaim.png)

5. İçinde **Add Users** iletişim kutusunda, uygun bölgeyi **bölgeleri**ve ardından **sonraki**.

6. İçinde **Web uygulaması için ilke** iletişim kutusundaki **kullanıcıların seçmesine** bölümünde **Gözat** simgesi.

7. İçinde **Bul** metin kutusuna **kullanıcı asıl adı (UPN)** hangi SharePoint şirket içi uygulama, Azure AD'de yapılandırdıysanız ve tıklayın değeri **arama**. </br>Örnek: *brittasimon@contoso.com*.

8. Liste görünümünde AzureAD başlığı, name özelliği seçin ve **Ekle** ardından **Tamam** iletişim kutusunu kapatmak için.

9. İzinler'de tıklayın **tam denetim**.

    ![Bir talep kullanıcıya tam denetim izni verme](./media/sharepoint-on-premises-tutorial/fig12-grantfullcontrol.png)

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

![Talep arama sonuçları](./media/sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Doğrulama yoktur aramak için değerleri için yazım hatası açabilir veya kazayla yanlış seçme kullanıcıların talep türü gibi atamak için **Soyadı** talep. Bu, kullanıcıların kaynakları başarılı bir şekilde erişmesini engelleyebilir.

Bu senaryo ile yardımcı olmak için yoktur açık kaynaklı adlı çözüm [AzureCP](https://yvand.github.io/AzureCP/) , SharePoint 2016 için bir özel talep sağlayıcı sağlar. Hangi kullanıcıların girin ve gerçekleştirme gidermek için Azure AD Graph kullanacağı doğrulama. Daha fazla bilgi [AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SharePoint şirket içi erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SharePoint şirket içi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **SharePoint şirket içi**.

    ![Uygulamalar listesinden SharePoint şirket içi bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sharepoint-on-premises-test-user"></a>SharePoint şirket içi test kullanıcısı oluşturma

Bu bölümde, şirket içinde SharePoint Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [SharePoint şirket içi Destek ekibine](https://support.office.com/) SharePoint şirket içi platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SharePoint şirket içi kutucuğa tıkladığınızda, size otomatik olarak SharePoint SSO'yu ayarlama şirket içine oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
