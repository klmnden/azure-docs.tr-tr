---
title: 'Öğretici: GmbH çözünürlüğün Confluence için SAML SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma SAML SSO Confluence için Azure Active Directory arasındaki GmbH çözünürlüğün yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54a248f64f4534a14d815ffe5865909024e20d31
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092141"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a>Öğretici: GmbH çözünürlüğün Confluence için SAML SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile GmbH çözünürlüğün Confluence için SAML SSO tümleştirme konusunda bilgi edinin.
SAML SSO için Confluence çözünürlüğün GmbH Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* SAML SSO için Confluence GmbH çözünürlüğün erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Confluence için SAML SSO (çoklu oturum açma) GmbH çözünürlüğüyle kendi Azure AD hesapları ile oturum, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi GmbH çözünürlüğüyle Confluence için SAML SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Confluence çözünürlüğün GmbH çoklu oturum açma için SAML SSO etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SAML SSO için Confluence GmbH destekler çözünürlüğün **SP** ve **IDP** tarafından başlatılan

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-the-gallery"></a>SAML SSO Confluence için GmbH çözünürlüğüyle galeri ekleme

Confluence için SAML SSO, Azure AD'de tümleştirmesini GmbH çözünürlüğün yapılandırmak için SAML SSO için Confluence GmbH çözünürlüğüyle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAML SSO için Confluence GmbH çözünürlüğüyle Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **GmbH çözünürlüğün Confluence için SAML SSO**seçin **GmbH çözünürlüğün Confluence için SAML SSO** sonucu panelinden ardından **Ekle** eklemek için Ekle düğmesine uygulama.

     ![SAML SSO için sonuç listesinde GmbH çözünürlüğün Confluence](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAML SSO ile Confluence GmbH çözünürlüğün adlı bir test kullanıcı tabanlı için test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve SAML SSO Confluence için ilgili kullanıcı çözünürlüğün arasında bir bağlantı ilişki GmbH kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Confluence ile GmbH çözünürlüğün sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SAML SSO için Confluence çözünürlüğüyle GmbH çoklu oturum açmayı yapılandırma](#configure-saml-sso-for-confluence-by-resolution-gmbh-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SAML SSO için çözüm GmbH test kullanıcısı tarafından Confluence oluşturma](#create-saml-sso-for-confluence-by-resolution-gmbh-test-user)**  - kullanıcı Azure AD gösterimini bağlı GmbH çözünürlüğün Confluence için SAML SSO içinde bir karşılığı Britta simon'un sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma SAML SSO için Confluence ile GmbH çözünürlüğüyle yapılandırmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **GmbH çözünürlüğün Confluence için SAML SSO** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümü, uygulamada yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Oturum açma bilgileri tek bir SAML SSO için Confluence çözünürlüğün GmbH etki alanı ve URL'ler](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/samlsso`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/samlsso`

    c. Tıklayın **ek URL'lerini ayarlayın** ve uygulama SP başlatılan modunda yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın:

    ![Oturum açma bilgileri tek bir SAML SSO için Confluence çözünürlüğün GmbH etki alanı ve URL'ler](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [SAML SSO Confluence çözünürlüğün GmbH istemci için Destek ekibine](https://www.resolution.de/go/support) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-saml-sso-for-confluence-by-resolution-gmbh-single-sign-on"></a>SAML SSO için Confluence çözünürlüğüyle GmbH çoklu oturum açmayı yapılandırma

1. Farklı bir web tarayıcı penceresinde oturum açın, **SAML SSO için çözüm GmbH Yönetici portalı tarafından Confluence** yönetici olarak.

2. Dişlisine gelin ve tıklayın **eklentileri**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon1.png)

3. Yönetici erişimi sayfasına yönlendirilirsiniz. Parolayı girin ve tıklatın **Onayla** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon2.png)

4. Altında **ATLASSIAN Market** sekmesinde **yeni eklentileri bulma**. 

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon.png)

5. Arama **SAML çoklu oturum açma (SSO) için Confluence** tıklatıp **yükleme** yeni SAML eklentisini yüklemek için düğme.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon7.png)

6. Eklenti yüklemesi başlatılır. **Kapat**’a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon8.png)

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon9.png)

7.  **Yönet**'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon10.png)
    
8. Tıklayın **yapılandırma** yeni eklentiyi yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon11.png)

9. Bu yeni eklenti ayrıca altında bulunabilir **kullanıcılar ve güvenlik** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon3.png)
    
10. Üzerinde **SAML SingleSignOn eklentisi yapılandırma** sayfasında **yeni IDP ekleme** kimlik sağlayıcısı ayarlarını yapılandırmak için düğmeye.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon4.png)

11. Üzerinde **SAML kimlik sağlayıcınızı seçin** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5a.png)
 
    a. Ayarlama **Azure AD'ye** IDP türü.
    
    b. Ekleme **adı** kimlik sağlayıcısının (ör. Azure AD).
    
    c. Ekleme **açıklama** kimlik sağlayıcısının (ör. Azure AD).
    
    d. **İleri**’ye tıklayın.
    
12. Üzerinde **kimlik sağlayıcı Yapılandırması** sayfasında **sonraki** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5b.png)

13. Üzerinde **meta verileri içeri aktarma SAML IDP** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5c.png)

    a. Tıklayın **yük dosyası** düğmesi ve 5. adımda indirdiğiniz meta veri XML dosyasını seçin.

    b. Tıklayın **alma** düğmesi.
    
    c. İçeri aktarma başarılı olana kadar kısa bir süre bekleyin.
    
    d. Tıklayın **sonraki** düğmesi.
    
14. Üzerinde **kullanıcı kimliği öznitelik ve dönüştürme** sayfasında **sonraki** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5d.png)
    
15. Üzerinde **kullanıcı oluşturma ve güncelleştirme** sayfasında **sonraki & Kaydet** ayarları kaydedin.   
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon6a.png)
    
16. Üzerinde **ayarlarınızı test etme** sayfasında **test atlayın ve el ile yapılandırma** kullanıcı test şimdilik atlamak için. Bu, sonraki bölümde gerçekleştirilir ve bazı ayarlar Azure portalında gerektirir. 
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon6b.png)
    
17. Appearing iletişim okuma **test yolları atlanıyor...** , tıklayın **Tamam**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon6c.png)

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

Bu bölümde, Azure çoklu oturum açma GmbH çözünürlüğüyle Confluence için SAML SSO için erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **GmbH çözünürlüğün Confluence için SAML SSO**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **GmbH çözünürlüğün Confluence için SAML SSO**.

    ![SAML SSO uygulamaları listesinde çözümleme GmbH bağlantısıyla Confluence için](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a>SAML SSO için çözüm GmbH test kullanıcısı tarafından Confluence oluşturma

SAML SSO için Confluence GmbH çözünürlüğüyle oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Confluence için SAML SSO içine GmbH çözüm tarafından sağlanması gerekir.  
SAML SSO GmbH çözünürlüğün Confluence, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak, SAML SSO Confluence çözümleme GmbH şirket site tarafından için oturum açın.

2. Dişlisine gelin ve tıklayın **kullanıcı yönetimi**.

    ![Çalışan Ekle](./media/samlssoconfluence-tutorial/user1.png) 

3. Kullanıcılar bölümü altında **kullanıcı ekleme** sekmesi. Üzerinde **"Bir kullanıcı Ekle"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/samlssoconfluence-tutorial/user2.png) 

    a. İçinde **kullanıcıadı** metin Britta Simon gibi kullanıcının e-posta yazın.

    b. İçinde **tam adı** metin Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin Britta Simon için parolayı yazın.

    e. Tıklayın **parolayı onayla** parolayı yeniden girin.
    
    f. Tıklayın **Ekle** düğmesi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çözüm GmbH kutucuk erişim Paneli'nde tarafından Confluence için SAML SSO'ye tıkladığınızda, otomatik olarak Confluence için SAML SSO için SSO'yu ayarlama çözümleme GmbH tarafından oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

