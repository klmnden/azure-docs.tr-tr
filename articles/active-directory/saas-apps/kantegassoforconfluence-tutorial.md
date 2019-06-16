---
title: "Öğretici: Azure Active Directory Tümleştirmesi Confluence Kantega sso'lu | Microsoft Docs"
description: Azure Active Directory ve Kantega SSO için Confluence arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d0d99c14-a6ca-45f2-bb84-633126095e7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 27fa0567eefbb50907c0ed6952333230e874c21d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67099031"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-confluence"></a>Öğretici: Confluence için Kantega SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Kantega SSO Confluence için Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Kantega SSO Confluence için Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Confluence Kantega SSO erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Kantega SSO (çoklu oturum açma) Confluence için oturum, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi için Confluence Kantega SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik Kantega SSO Confluence çoklu oturum açma için etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Confluence için Kantega SSO'yu destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-kantega-sso-for-confluence-from-the-gallery"></a>Galeriden Confluence için Kantega SSO ekleme

Azure AD'ye Confluence için Kantega SSO tümleştirmesini yapılandırmak için Kantega SSO Confluence için Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Kantega SSO için Confluence eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Kantega SSO için Confluence**seçin **Kantega SSO için Confluence** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Confluence için Kantega SSO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Kantega SSO ile Confluence adlı bir test kullanıcı tabanlı için test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Kantega SSO Confluence için ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma için Confluence Kantega SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Kantega SSO Confluence çoklu oturum açma için yapılandırma](#configure-kantega-sso-for-confluence-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Kantega SSO için Confluence test kullanıcısı oluşturma](#create-kantega-sso-for-confluence-test-user)**  - Kantega SSO için kullanıcı Azure AD gösterimini bağlı Confluence Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma için Confluence Kantega SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Kantega SSO için Confluence** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Kantega SSO Confluence etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Kantega SSO Confluence etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Bu değerler, öğreticinin ilerleyen bölümlerinde açıklanan Confluence eklentisi, yapılandırma sırasında alınır.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Üzerinde **Kantega SSO için Confluence ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-kantega-sso-for-confluence-single-sign-on"></a>Kantega SSO Confluence çoklu oturum açma için yapılandırma

1. Farklı bir web tarayıcı penceresinde oturum açın, **Confluence Yönetici portalı** yönetici olarak.

1. Dişlisine gelin ve tıklayın **eklentileri**.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon1.png)

1. Altında **ATLASSIAN Market** sekmesinde **yeni eklentileri bulma**.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon.png)

1. Arama **Confluence SAML Kerberos için Kantega SSO** tıklatıp **yükleme** yeni SAML eklentisini yüklemek için düğme.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon2.png)

1. Eklenti yükleme başlar.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon3.png)

1. Yükleme tamamlandıktan sonra. **Kapat**’a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon33.png)

1. **Yönet**'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon34.png)

1. Tıklayın **yapılandırma** yeni eklentiyi yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon35.png)

1. Bu yeni eklenti ayrıca altında bulunabilir **kullanıcılar ve güvenlik** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon36.png)

1. İçinde **SAML** bölümü. Seçin **Azure Active Directory (Azure AD)** gelen **Ekle kimlik sağlayıcısı** açılır.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon4.png)

1. Abonelik düzeyi olarak **temel**.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon5.png)

1. Üzerinde **uygulama özellikleri** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon6.png)

    a. Kopyalama **uygulama kimliği URI'si** olarak kullanın ve değerini **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** üzerinde **temel SAML yapılandırma** bölümü Azure Portalı'nda.

    b. **İleri**’ye tıklayın.

1. Üzerinde **meta veri içeri aktarma** bölümünde, aşağıdaki adımları uygulayın: 

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon7.png)

    a. Seçin **meta veri dosyası bilgisayarımda**ve Azure portalından indirdiğiniz meta veri dosyası karşıya yükleme.

    b. **İleri**’ye tıklayın.

1. Üzerinde **adı ve SSO konumunu** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon8.png)

    a. Kimlik sağlayıcısı adını eklemek **kimlik sağlayıcı adı** metin (ör. Azure AD).

    b. **İleri**’ye tıklayın.

1. İmzalama sertifikası doğrulayın ve tıklayın **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon9.png)

1. Üzerinde **Confluence kullanıcı hesaplarını** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon10.png)

    a. Seçin **kullanıcılar Confluence'nın iç dizinde gerekirse oluşturun** ve kullanıcılar için uygun Grup adını girin (olabilir birden çok yok. virgülle ayırarak grupları).

    b. **İleri**’ye tıklayın.

1. **Son**'a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon11.png)

1. Üzerinde **etki alanları için Azure AD bilinen** bölümünde, aşağıdaki adımları uygulayın: 

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforconfluence-tutorial/addon12.png)

    a. Seçin **etki alanları** sayfasının sol panelden.

    b. Etki alanı adını girin **etki alanları** metin.

    c. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Confluence Kantega SSO için erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Kantega SSO için Confluence**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Kantega SSO için Confluence**.

    ![Uygulamalar listesinde Confluence bağlantısının Kantega SSO](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-kantega-sso-for-confluence-test-user"></a>Kantega SSO için Confluence test kullanıcısı oluşturma

Confluence için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Confluence sağlanması gerekir. Confluence Kantega SSO söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Kantega SSO Confluence şirket site için bir yönetici olarak oturum açın.

1. Dişlisine gelin ve tıklayın **kullanıcı yönetimi**.

    ![Çalışan Ekle](./media/kantegassoforconfluence-tutorial/user1.png)

1. Kullanıcılar bölümü altında **Add Users** sekmesi. Üzerinde **kullanıcı ekleme** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/kantegassoforconfluence-tutorial/user2.png)

    a. İçinde **kullanıcıadı** metin kutusuna kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    b. İçinde **tam adı** metin Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusu, kullanıcının parolasını yazın.

    e. Tıklayın **parolayı onayla** parolayı yeniden girin.

    f. Tıklayın **Ekle** düğmesi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Kantega SSO erişim panelinde Confluence kutucuğa tıkladığınızda, otomatik olarak için SSO'yu ayarlama Confluence Kantega SSO için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

