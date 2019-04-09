---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Teamphoria | Microsoft Docs'
description: Azure Active Directory ve Teamphoria arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 11d0a0a67d1ce5049166cab3d9ca3e4903b6b460
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59277213"
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Öğretici: Teamphoria ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Teamphoria tümleştirme konusunda bilgi edinin.
Azure AD ile Teamphoria tümleştirme ile aşağıdaki avantajları sağlar:

* Teamphoria erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Teamphoria için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Teamphoria yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik Teamphoria çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Teamphoria destekler **SP** tarafından başlatılan

## <a name="adding-teamphoria-from-the-gallery"></a>Galeriden Teamphoria ekleme

Azure AD'de Teamphoria tümleştirmesini yapılandırmak için Teamphoria Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Teamphoria eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Teamphoria**seçin **Teamphoria** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Teamphoria](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Teamphoria adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Teamphoria ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Teamphoria ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Teamphoria çoklu oturum açmayı yapılandırma](#configure-teamphoria-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Teamphoria test kullanıcısı oluşturma](#create-teamphoria-test-user)**  - kullanıcı Azure AD gösterimini bağlı Teamphoria Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Teamphoria yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Teamphoria** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Teamphoria etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-intiated.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<sub-domain>.teamphoria.com/login`

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirmeniz gerekiyor. İlgili kişi [Teamphoria istemci Destek ekibine](https://www.teamphoria.com/) oturum açma URL'sini alabilirsiniz. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Teamphoria kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-teamphoria-single-sign-on"></a>Teamphoria tek oturum açmayı yapılandırın

1. Çoklu oturum açmayı yapılandırma **Teamphoria** tarafını Teamphoria uygulamanızı yönetici olarak oturum açın.

1. Git **yönetici ayarları** sol araç çubuğundaki ve yapılandırma sekmesi altındaki seçeneği tıklatın **çoklu oturum açma** SSO Yapılandırması penceresi açmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/admin_sso_configure.png)

1. Tıklayarak **yeni kimlik SAĞLAYICISI Ekle** SSO ayarlarını ekleme formunu açmak için sağ üst köşedeki seçeneği.

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/add_new_identity_provider.png)

1. Aşağıdaki - açıklandığı gibi alanlarda ayrıntıları girin

    ![Çoklu oturum açmayı yapılandırın](./media/teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **GÖRÜNEN AD**: Yönetici sayfasına eklenti görünen adını girin.

    b. **DÜĞME ADI**: SSO ile oturum açma için oturum açma sayfasında görüntülenecek sekmenin adı.

    c. **SERTİFİKA**: Daha önce Not Defteri'nde Azure portalından indirdiğiniz sertifikayı açın, aynı içeriğini kopyalayıp buraya kutuya yapıştırın.

    d. **GİRİŞ NOKTASI**: Yapıştırma **oturum açma URL'si** daha önce Azure portaldan kopyaladığınız.

    e. Geçiş seçeneği **ON** tıklayın **Kaydet**.

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

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Teamphoria erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Teamphoria**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Teamphoria**.

    ![Uygulamalar listesinde Teamphoria bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-teamphoria-test-user"></a>Teamphoria test kullanıcısı oluşturma

Teamphoria için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Teamphoria sağlanması gerekir. Teamphoria söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Teamphoria şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayarak **yönetici** ayarları altında ve sol araç çubuğundaki **Yönet** sekmesine tıklatın **kullanıcılar** kullanıcılar için Yönetim sayfasını açın.

    ![Çalışan Ekle](./media/teamphoria-tutorial/admin_manage_users.png)

1. Tıklayarak **el ile davet** seçeneği.

    ![Kişileri davet edin](./media/teamphoria-tutorial/admin_manage_add_users.png)

1. Bu sayfada, eylemi gerçekleştirin.

    ![Kişileri davet edin](./media/teamphoria-tutorial/manual_user_invite.png)

    a. İçinde **e-posta adresi** metin girin **e-posta adresi** kullanıcının BrittaSimon gibi.

    b. İçinde **ad** metin gibi kullanıcının ilk adını girin **Britta**.

    c. İçinde **SOYADI** metin gibi kullanıcının soyadını girin **Simon**.

    d. Tıklayın **davet 1 kullanıcı**. Kullanıcı sistemde oluşturulmasına daveti kabul etmek gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Teamphoria kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Teamphoria için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

