---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Adobe Experience Manager | Microsoft Docs'
description: Azure Active Directory ve Adobe Experience Manager arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81032fbad21b18b0b7ca2e7662b0c4b4b6c10901
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107278"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Öğretici: Azure Active Directory Tümleştirmesi ile Adobe Experience Manager

Bu öğreticide, Azure Active Directory (Azure AD) ile Adobe Experience Manager tümleştirme konusunda bilgi edinin.
Adobe Experience Manager'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Adobe Experience Manager için erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Adobe Experience Manager oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Adobe Experience Manager ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Adobe Experience Manager çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Adobe Experience Manager destekleyen **SP ve IDP** tarafından başlatılan

* Adobe Experience Manager destekleyen **zamanında** kullanıcı sağlama

## <a name="adding-adobe-experience-manager-from-the-gallery"></a>Adobe Experience Manager galeri ekleme

Azure AD'de, Adobe Experience Manager tümleştirme yapılandırmak için Adobe Experience Manager Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Adobe Experience Manager Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Adobe Experience Manager**seçin **Adobe Experience Manager** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Adobe Experience Manager sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD'ye tek temelinde oturum açma adlı bir test kullanıcısı [uygulama adı] ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve [uygulama adı] ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma [uygulama adı] ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Adobe Experience Manager çoklu oturum açma yapılandırma](#configure-adobe-experience-manager-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Adobe Experience Manager test kullanıcısı oluşturma](#create-adobe-experience-manager-test-user)**  - Adobe deneyimi kullanıcı Azure AD gösterimini bağlı Yöneticisi'nde Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma [uygulama adı] ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Adobe Experience Manager** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Adobe Experience Manager etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna, AEM sunucunuzda de tanımlayan benzersiz bir değer yazın.

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<AEM Server Url>/saml_login`

    > [!NOTE]
    > Yanıt URL'si değeri gerçek değil. Yanıt URL'si değeri ile gerçek yanıt URL'si güncelleştirin. Bu değer almak için iletişime geçin [Adobe Experience Manager istemci Destek ekibine](https://helpx.adobe.com/support/experience-manager.html) bu değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Adobe Experience Manager etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna, Adobe Experience Manager sunucusu URL'nizi yazın.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **Adobe Experience Manager yedekleme kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-adobe-experience-manager-single-sign-on"></a>Adobe Experience Manager tek oturum açmayı yapılandırın

1. Başka bir tarayıcı penceresinde açmak **Adobe Experience Manager** Yönetim Portalı.

2. Seçin **ayarları** > **güvenlik** > **kullanıcılar**.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

3. Seçin **yönetici** veya ilgili herhangi bir kullanıcı.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

4. Seçin **hesap ayarları** > **yönetme TrustStore**.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

5. Altında **CER dosyası ekleme sertifikadan**, tıklayın **sertifika dosyası seçin**. Göz atın ve zaten Azure portalından indirdiğiniz sertifika dosyasını seçin.

    ![Kaydet düğmesi çoklu oturum açmayı yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

6. Sertifika için TrustStore eklenir. Diğer sertifika adını not edin.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

7. Üzerinde **kullanıcılar** sayfasında **kimlik doğrulama hizmeti**.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

8. Seçin **hesap ayarları** > **oluşturma/yönetme KeyStore**. KeyStore için bir parola sağlanarak oluşturun.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

9. Yönetici ekranına dönün. Ardından **ayarları** > **işlemleri** > **Web Konsolu**.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

    Bu yapılandırma sayfasını açar.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

10. Bulma **Adobe Granit SAML 2.0 kimlik doğrulama işleyicisi**. Ardından **Ekle** simgesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

11. Bu sayfada aşağıdaki eylemleri gerçekleştirin.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. İçinde **yolu** kutusuna **/** .

    b. İçinde **IDP URL** kutusuna **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    c. İçinde **IDP sertifika diğer adı** kutusuna **sertifika diğer adı** TrustStore içinde eklenen değer.

    d. İçinde **güvenlik sağlanan varlık kimliği** kutusunda, benzersiz bir değer girin **Azure Ad tanımlayıcısı** Azure portalında yapılandırılmış değer.

    e. İçinde **onay belgesi tüketici hizmeti URL'si** kutusuna **yanıt URL'si** Azure portalında yapılandırılmış değer.

    f. İçinde **anahtarı parola Store** kutusuna **parola** , anahtar deposu ayarlayın.

    g. İçinde **kullanıcı öznitelik kimliği** kutusuna **ad kimliği** veya sizin durumunuzda ilgili başka bir kullanıcı kimliği.

    h. Seçin **otomatik oluştur CRX kullanıcılar**.

    i. İçinde **oturum kapatma URL'si** kutusunda, benzersiz bir değer girin **oturum kapatma URL'si** Azure portalından aldığınız değeri.

    j. **Kaydet**’i seçin.

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

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Adobe Experience Manager erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Adobe Experience Manager**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Adobe Experience Manager**.

    ![Uygulamalar listesinde Adobe Experience Manager bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-adobe-experience-manager-test-user"></a>Adobe Experience Manager test kullanıcısı oluşturma

Bu bölümde, Britta Simon, Adobe Experience Manager adlı bir kullanıcı oluşturun. Seçtiyseniz **otomatik oluştur CRX kullanıcılar** seçeneği, kullanıcıların otomatik olarak oluşturulur başarılı kimlik doğrulamasından sonra.

Kullanıcıları el ile oluşturmak istiyorsanız, çalışmak [Adobe Experience Manager destek ekip](https://helpx.adobe.com/support/experience-manager.html) Adobe Experience Manager platform kullanıcıları eklemek için.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Adobe Experience Manager kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Adobe Experience Manager oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
