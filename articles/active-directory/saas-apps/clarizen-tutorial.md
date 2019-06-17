---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Clarizen | Microsoft Docs'
description: Azure Active Directory ve Clarizen arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/21/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88f42a849d5c483feb9325072fd65769839672e4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67105374"
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Öğretici: Clarizen ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Clarizen tümleştirme konusunda bilgi edinin.
Azure AD ile Clarizen tümleştirme ile aşağıdaki avantajları sağlar:

* Clarizen erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Clarizen için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Clarizen yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Clarizen çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Clarizen destekler **IDP** tarafından başlatılan

## <a name="adding-clarizen-from-the-gallery"></a>Galeriden Clarizen ekleme

Azure AD'de Clarizen tümleştirmesini yapılandırmak için Clarizen Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Clarizen eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Clarizen**seçin **Clarizen** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Clarizen](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Clarizen adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Clarizen ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Clarizen ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Clarizen çoklu oturum açmayı yapılandırma](#configure-clarizen-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Clarizen test kullanıcısı oluşturma](#create-clarizen-test-user)**  - kullanıcı Azure AD gösterimini bağlı Clarizen Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Clarizen yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Clarizen** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Clarizen etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir değer yazın: `Clarizen`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx`

    > [!NOTE]
    > Bunlar gerçek değerlerin değildir. Gerçek tanımlayıcısı kullanın ve yanıt URL'si gerekir. Burada, benzersiz bir dize değerini tanımlayıcı olarak kullanmanızı öneririz. Gerçek değerlere ulaşmak için ilgili kişi [Clarizen Destek ekibine](https://success.clarizen.com/hc/en-us/requests/new).

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Clarizen kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-clarizen-single-sign-on"></a>Clarizen tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Clarizen şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Kullanıcı adınıza tıklayın ve ardından **ayarları**.

    ![Kullanıcı adınızın "Ayarlar" a tıklandığında](./media/clarizen-tutorial/tutorial_clarizen_001.png "ayarları")

1. Tıklayın **genel ayarları** sekmesi. Ardından yanındaki **şirket dışı kimlik doğrulaması**, tıklayın **Düzenle**.

    !["Genel ayarları" sekmesine](./media/clarizen-tutorial/tutorial_clarizen_002.png "genel ayarlar")

1. İçinde **şirket dışı kimlik doğrulaması** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![İletişim kutusu "Federe kimlik doğrulaması"](./media/clarizen-tutorial/tutorial_clarizen_003.png "şirket dışı kimlik doğrulaması")

    a. Seçin **etkinleştir federe kimlik doğrulaması**.

    b. Tıklayın **karşıya** indirilen sertifikanızı karşıya yüklemek için.

    c. İçinde **oturum açma URL'si** kutusunda, değeri girin **oturum açma URL'si** penceresinden Azure AD uygulama yapılandırması.

    d. İçinde **oturum kapatma URL'si** kutusunda, değeri girin **oturum kapatma URL'si** penceresinden Azure AD uygulama yapılandırması.

    e. Seçin **POST kullanın**.

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
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Clarizen erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Clarizen**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Clarizen**.

    ![Uygulamalar listesinde Clarizen bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-clarizen-test-user"></a>Clarizen test kullanıcısı oluşturma

Bu bölümün amacı Clarizen Britta Simon adlı bir kullanıcı oluşturmaktır.

**Kullanıcı el ile oluşturmanız gerekiyorsa, lütfen aşağıdaki adımları uygulayın:**

Azure AD kullanıcıları için Clarizen oturum açmak etkinleştirmek için kullanıcı hesapları hazırlamanız gerekir. Clarizen söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

1. Clarizen şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayın **kişiler**.

    !["Kişiler" tıklayarak](./media/clarizen-tutorial/create_aaduser_001.png "kişiler")

3. Tıklayın **kullanıcı davet**.

    !["Kullanıcı davet et" düğmesini](./media/clarizen-tutorial/create_aaduser_002.png "kullanıcılar davet edin")

1. İçinde **davet kişiler** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![İletişim kutusu "Kişileri davet edin"](./media/clarizen-tutorial/create_aaduser_003.png "kişileri davet edin")

    a. İçinde **e-posta** Britta Simon hesabın e-posta adresini yazın.

    b. Tıklayın **davet**.

    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin.


### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Clarizen kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Clarizen için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
