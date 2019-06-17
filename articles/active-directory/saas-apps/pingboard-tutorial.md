---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Pingboard | Microsoft Docs'
description: Azure Active Directory ve Pingboard arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 341d8dd712b858572ec5df76b176258ca87c8857
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67094436"
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Öğretici: Pingboard ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Pingboard tümleştirme konusunda bilgi edinin.
Azure AD ile Pingboard tümleştirme ile aşağıdaki avantajları sağlar:

* Pingboard erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Pingboard için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Pingboard yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Pingboard çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Pingboard destekler **SP** ve **IDP** tarafından başlatılan

* Pingboard destekler [otomatik kullanıcı hazırlama](https://docs.microsoft.com/azure/active-directory/saas-apps/pingboard-provisioning-tutorial) 

## <a name="adding-pingboard-from-the-gallery"></a>Galeriden Pingboard ekleme

Azure AD'de Pingboard tümleştirmesini yapılandırmak için Pingboard Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Pingboard eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Pingboard**seçin **Pingboard** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Pingboard](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Pingboard adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Pingboard ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Pingboard ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Pingboard çoklu oturum açmayı yapılandırma](#configure-pingboard-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Pingboard test kullanıcısı oluşturma](#create-pingboard-test-user)**  - kullanıcı Azure AD gösterimini bağlı Pingboard Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Pingboard yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Pingboard** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Pingboard etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL yazın: `http://app.pingboard.com/sp`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<entity-id>.pingboard.com/auth/saml/consume`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Pingboard etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<sub-domain>.pingboard.com/sign_in`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Pingboard istemci Destek ekibine](https://support.pingboard.com/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Üzerinde **Pingboard kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-pingboard-single-sign-on"></a>Pingboard tek oturum açmayı yapılandırın

1. SSO Pingboard tarafında yapılandırmak için yeni bir tarayıcı penceresi açın ve Pingboard hesabınızda oturum açın. Çoklu oturum açma ayarlamak için Pingboard yönetici olması gerekir.

2. Üstteki menüden,, seçin **uygulamaları > tümleştirmeleri**

    ![Çoklu oturum açmayı yapılandırın](./media/pingboard-tutorial/Pingboard_integration.png)

3. Üzerinde **tümleştirmeler** sayfasında, bulmak **"Azure Active Directory"** kutucuğuna ve üzerine tıklayın.

    ![Pingboard tek oturum açma tümleştirmesi](./media/pingboard-tutorial/Pingboard_aad.png)

4. Click izleyen kalıcı olarak **"Yapılandır"**

    ![Pingboard Yapılandırma düğmesi](./media/pingboard-tutorial/Pingboard_configure.png)

5. Sonraki sayfada, "Azure SSO tümleştirme etkinleştirildiğini" dikkat edin. İndirilen meta veri XML dosyasını Not Defteri'nde açın ve içeriği yapıştırın **IDP meta verileri**.

    ![Pingboard SSO yapılandırma ekranında](./media/pingboard-tutorial/Pingboard_sso_configure.png)

6. Dosya doğrulanır ve her şeyin doğru ise, çoklu oturum açma şimdi etkinleştirilir.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Pingboard erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Pingboard**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Pingboard**.

    ![Uygulamalar listesinde Pingboard bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-pingboard-test-user"></a>Pingboard test kullanıcısı oluşturma

Bu bölümün amacı Pingboard Britta Simon adlı bir kullanıcı oluşturmaktır. Pingboard otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](pingboard-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. Pingboard şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayın **"Çalışan Ekle"** düğmesini **dizin** sayfası.

    ![Çalışan Ekle](./media/pingboard-tutorial/create_testuser_add.png)

3. Üzerinde **"Çalışan Ekle"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kişileri davet edin](./media/pingboard-tutorial/create_testuser_name.png)

    a. İçinde **tam adı** metin kutusuna kullanıcının tam adını yazın ister **Britta Simon**.

    b. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister **brittasimon@contoso.com** .

    c. İçinde **iş unvanı** metin Britta simon'un iş unvanı yazın.

    d. İçinde **konumu** açılır listesinde, Britta Simon konumunu seçin.

    e. **Ekle**'yi tıklatın.

4. Kullanıcı eklenmesini doğrulamak için bir onay ekranı göründüğünde.

    ![Onayla](./media/pingboard-tutorial/create_testuser_confirm.png)

    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Pingboard kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Pingboard için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı sağlamayı yapılandırma](https://docs.microsoft.com/azure/active-directory/saas-apps/pingboard-provisioning-tutorial)
