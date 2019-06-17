---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile IdeaScale | Microsoft Docs'
description: Azure Active Directory ve IdeaScale arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a312e0ca70ef3a37b43f1ffb68a974a8aedaf982
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67100789"
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Öğretici: IdeaScale ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile IdeaScale tümleştirme konusunda bilgi edinin.
Azure AD ile IdeaScale tümleştirme ile aşağıdaki avantajları sağlar:

* IdeaScale erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) IdeaScale için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile IdeaScale yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik IdeaScale çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* IdeaScale destekler **SP** tarafından başlatılan

## <a name="adding-ideascale-from-the-gallery"></a>Galeriden IdeaScale ekleme

Azure AD'de IdeaScale tümleştirmesini yapılandırmak için IdeaScale Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden IdeaScale eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **IdeaScale**seçin **IdeaScale** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde IdeaScale](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma IdeaScale adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının IdeaScale ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma IdeaScale ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[IdeaScale çoklu oturum açmayı yapılandırma](#configure-ideascale-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[IdeaScale test kullanıcısı oluşturma](#create-ideascale-test-user)**  - kullanıcı Azure AD gösterimini bağlı IdeaScale Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile IdeaScale yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **IdeaScale** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![IdeaScale etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.ideascale.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın:
    
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [IdeaScale istemci Destek ekibine](https://support.ideascale.com/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **IdeaScale kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-ideascale-single-sign-on"></a>IdeaScale tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde IdeaScale şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Git **topluluk ayarları**.

    ![Topluluk ayarları](./media/ideascale-tutorial/ic790847.png "topluluk ayarları")

3. Git **güvenlik \> çoklu oturum açma ayarları**.

    ![Çoklu oturum açma ayarları](./media/ideascale-tutorial/ic790848.png "çoklu oturum açma ayarları")

4. Olarak **çoklu oturum açma türü**seçin **SAML 2.0**.

    ![Çoklu oturum açma türü](./media/ideascale-tutorial/ic790849.png "çoklu oturum açma türü")

5. Üzerinde **çoklu oturum açma ayarları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma ayarları](./media/ideascale-tutorial/ic790850.png "çoklu oturum açma ayarları")

    a. İçinde **SAML IDP varlık kimliği** metin değerini yapıştırın **Azure Ad tanımlayıcısı** , Azure Portalı'ndan kopyaladığınız.

    b. Not Defteri'ne Azure portalından indirilen meta veri dosyası açın, içeriğini kopyalayın ve yapıştırın **SAML IDP meta verileri** metin.

    c. İçinde **oturum kapatma başarı URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    d. Tıklayın **değişiklikleri kaydetmek**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için IdeaScale erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **IdeaScale**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **IdeaScale**.

    ![Uygulamalar listesinde IdeaScale bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-ideascale-test-user"></a>IdeaScale test kullanıcısı oluşturma

Azure AD kullanıcılarının IdeaScale oturum etkinleştirmek için bunlar içinde IdeaScale için sağlanması gerekir. IdeaScale söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **IdeaScale** şirketinizin sitesi yöneticisi olarak.

2. Git **topluluk ayarları**.

    ![Topluluk ayarları](./media/ideascale-tutorial/ic790847.png "topluluk ayarları")

3. Git **temel ayarları \> üye yönetim**.

4. Tıklayın **üye ekleme**.

    ![Üye yönetim](./media/ideascale-tutorial/ic790852.png "üye yönetim")

5. Yeni Üye Ekle bölümünde aşağıdaki adımları gerçekleştirin:

    ![Yeni üye ekler](./media/ideascale-tutorial/ic790853.png "yeni üye ekler")

    a. İçinde **e-posta adreslerini** metin kutusu, türü e-posta adresi geçerli bir Azure ad hesabına sağlamak istiyor.

    b. Tıklayın **değişiklikleri kaydetmek**.

    > [!NOTE]
    > Azure Active Directory hesap sahibi bu etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alır.

> [!NOTE]
> Herhangi diğer IdeaScale kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak IdeaScale tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli IdeaScale kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama IdeaScale için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

