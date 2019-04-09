---
title: 'Öğretici: Palo Alto Networks - açıklık ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Palo Alto Networks - açıklık arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a5ea18d3-3aaf-4bc6-957c-783e9371d0f1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: jeedes
ms.openlocfilehash: 375b58f47454a7f5904bcdbd132b8318091e9ebd
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59275088"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---aperture"></a>Öğretici: Palo Alto Networks - açıklık ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile açıklık Palo Alto Networks - tümleştirme konusunda bilgi edinin.
Palo Alto Networks tarafından sağlanan-açıklık Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Palo Alto Networks - açıklık erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Palo Alto Networks - açıklık (çoklu oturum açma) ile kendi Azure AD hesapları için oturum açmış, kullanıcılarınızın etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Palo Alto Networks - açıklığı, Azure AD tümleştirmesi yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Palo Alto Networks tarafından sağlanan-açıklık çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Palo Alto Networks tarafından sağlanan - açıklık destekler **SP** ve **IDP** tarafından başlatılan

## <a name="adding-palo-alto-networks---aperture-from-the-gallery"></a>Palo Alto Networks tarafından sağlanan - galerisinden açıklık ekleme

Palo Alto Networks - Azure AD'ye açıklık tümleştirmesini yapılandırmak için listenize yönetilen SaaS uygulamalarının Diyafram galerisinden Palo Alto Networks - eklemeniz gerekir.

**Palo Alto Networks - galerisinden açıklık eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Palo Alto Networks - açıklık**seçin **Palo Alto Networks - açıklık** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Palo Alto Networks tarafından sağlanan-sonuç listesinde açıklık](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks ile test etme - açıklık adlı bir test kullanıcı tabanlı **Britta Simon**.
Çoklu oturum açma iş, bir Azure AD kullanıcısının Palo Alto Networks - ilgili kullanıcı arasında bir bağlantı ilişki açıklık kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks ile-test etmek için açıklığı, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Palo Alto Networks tarafından sağlanan - açıklık çoklu oturum açmayı yapılandırma](#configure-palo-alto-networks---aperture-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Palo Alto Networks tarafından sağlanan-açıklık test kullanıcısı oluşturma](#create-palo-alto-networks---aperture-test-user)**  - Palo Alto Networks - kullanıcı Azure AD gösterimini bağlı açıklık Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Palo Alto Networks - açıklığı, Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Palo Alto Networks - açıklık** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Palo Alto Networks tarafından sağlanan-açıklık etki alanı ve URL'ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/auth`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Palo Alto Networks tarafından sağlanan-açıklık etki alanı ve URL'ler çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/sign_in`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Palo Alto Networks - açıklık istemci Destek ekibine](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **Palo Alto Networks - açıklık ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-palo-alto-networks---aperture-single-sign-on"></a>Palo Alto Networks tarafından sağlanan - açıklık çoklu oturum açmayı yapılandırın

1. Bir başka web tarayıcı penceresinde Palo Alto Networks - açıklık yönetici olarak oturum açın.

2. Üst menü çubuğunda **ayarları**.

    ![Ayarlar sekmesi](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_settings.png)

3. Gidin **uygulama** bölümünde **kimlik doğrulaması** menüsünde sol tarafındaki form.

    ![Kimlik doğrulama sekmesi](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_auth.png)
    
4. Üzerinde **kimlik doğrulaması** sayfasında aşağıdaki adımları gerçekleştirin:
    
    ![Kimlik doğrulama sekmesi](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_singlesignon.png)

    a. Denetleme **etkinleştirme tek oturum-On(Supported SSP Providers are Okta, One login)** gelen **çoklu oturum açma** alan.

    b. İçinde **kimlik sağlayıcı kimliği** metin değerini yapıştırın **Azure AD tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız.

    c. Tıklayın **Dosya Seç** Azure AD'den yüklenen sertifikayı karşıya yüklemek için **kimlik sağlayıcısı sertifikası** alan.

    d. İçinde **kimlik sağlayıcısı SSO URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    e. IDP bilgileri gözden geçirin **açıklık bilgisi** bölümünde ve'deki Sertifika Yükle **açıklık anahtar** alan.

    f. **Kaydet**’e tıklayın.

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

Bu bölümde, Azure çoklu oturum açma Palo Alto Networks - açıklık erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Palo Alto Networks - açıklık**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Palo Alto Networks - açıklık**.

    ![-Açıklık bağlantı uygulamalar listesinde Palo Alto ağlar](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-palo-alto-networks---aperture-test-user"></a>Palo Alto Networks tarafından sağlanan-açıklık test kullanıcısı oluşturma

Bu bölümde, Britta Simon Palo Alto Networks - açıklık adlı bir kullanıcı oluşturun. Çalışmak [Palo Alto Networks - açıklık istemci Destek ekibine](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) Palo Alto Networks - açıklık platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Palo Alto Networks - açıklık kutucuk erişim Paneli'nde tıklattığınızda, otomatik olarak Palo Alto Networks - SSO'yu ayarlama açıklık için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

