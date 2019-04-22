---
title: 'Öğretici: Mozy Enterprise ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Mozy kuruluş arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.openlocfilehash: a0f21165af0bcbd8bda28f0eae20d3ee837f3be9
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59275666"
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Öğretici: Mozy Enterprise ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile Mozy Kurumsal tümleştirme konusunda bilgi edinin.
Azure AD ile Mozy Kurumsal tümleştirme ile aşağıdaki avantajları sağlar:

* Mozy Kurumsal erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Mozy kuruluş (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Kurumsal Mozy yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Mozy Kurumsal çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Mozy Kurumsal destekler **SP** tarafından başlatılan

## <a name="adding-mozy-enterprise-from-the-gallery"></a>Galeriden Mozy Kurumsal ekleme

Azure AD'de Mozy Kurumsal tümleştirmesini yapılandırmak için Mozy Kurumsal Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Mozy kuruluş eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Mozy Kurumsal**seçin **Mozy Kurumsal** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Mozy Enterprise](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Mozy Enterprise ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı Mozy kuruluştaki arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Mozy Enterprise ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Mozy Kurumsal çoklu oturum açmayı yapılandırma](#configure-mozy-enterprise-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Mozy Kurumsal test kullanıcısı oluşturma](#create-mozy-enterprise-test-user)**  - Mozy kuruluştaki kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Mozy Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Mozy Kurumsal** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Mozy Kurumsal etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<tenantname>.Mozyenterprise.com`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Mozy Kurumsal İstemci Destek ekibine](http://support.mozy.com/) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Mozy ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-mozy-enterprise-single-sign-on"></a>Mozy Kurumsal çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Mozy Kuruluş şirket sitenize yönetici olarak oturum.

2. İçinde **yapılandırma** bölümünde **kimlik doğrulama İlkesi**.
   
    ![Kimlik doğrulama İlkesi](./media/mozy-enterprise-tutorial/ic777314.png "kimlik doğrulama İlkesi")

3. Üzerinde **kimlik doğrulama İlkesi** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik doğrulama İlkesi](./media/mozy-enterprise-tutorial/ic777315.png "kimlik doğrulama İlkesi")
   
    a. Seçin **dizin hizmeti** olarak **sağlayıcısı**.
   
    b. Seçin **LDAP göndermeyi kullanmak**.
   
    c. Tıklayın **SAML kimlik doğrulaması** sekmesi.
   
    d. Yapıştırma **oturum açma URL'si**, hangi Azure portaldan kopyaladığınız **kimlik doğrulaması URL'sini** metin.
   
    e. Yapıştırma **Azure AD tanımlayıcısı**, hangi Azure portaldan kopyaladığınız **SAML uç noktası** metin.
   
    f. İndirilen base-64 kodlanmış sertifika Not Defteri'nde açın, içeriğini, panoya kopyalayın ve tüm sertifika içine yapıştırın **SAML sertifikası** metin.
   
    g. Seçin **yöneticilerin, ağ kimlik bilgileriyle oturum SSO etkinleştirme**.
   
    h. Tıklayın **değişiklikleri kaydetmek**.

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

Bu bölümde, Azure çoklu oturum açma için Mozy Kurumsal erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Mozy Kurumsal**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Mozy Kurumsal**.

    ![Uygulamalar listesinde Mozy Kurumsal bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-mozy-enterprise-test-user"></a>Mozy Kurumsal test kullanıcısı oluşturma

Azure AD kullanıcılarının Mozy kuruma oturum etkinleştirmek için bunlar Mozy kuruma sağlanması gerekir. Mozy Enterprise söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

>[!NOTE]
>Herhangi diğer Mozy Kurumsal kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Mozy kuruluş tarafından sağlanan.

**Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Mozy Kurumsal** Kiracı.

2. Tıklayın **kullanıcılar**ve ardından **yeni kullanıcı Ekle**.
   
    ![Kullanıcılar](./media/mozy-enterprise-tutorial/ic777317.png "kullanıcılar")
   
    >[!NOTE]
    >**Yeni kullanıcı Ekle** seçeneği yalnızca, yalnızca görüntülenen **Mozy** sağlayıcı altında olarak seçilen **kimlik doğrulama İlkesi**. SAML kimlik doğrulaması yapılandırılmışsa, ardından kullanıcıları otomatik olarak, ilk oturum açma aracılığıyla çoklu oturum açma üzerinde üzerinde eklenir.
    
3. Yeni kullanıcı iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı ekleme](./media/mozy-enterprise-tutorial/ic777318.png "kullanıcı ekleme")
   
    a. Gelen **bir grubu seçin** listesinde, bir grubu seçin.
   
    b. Gelen **kullanıcı türüne** listesinde, bir tür seçin.
   
    c. İçinde **kullanıcıadı** metin kutusuna, Azure AD kullanıcı adını yazın.
   
    d. İçinde **e-posta** metin kutusuna, Azure AD kullanıcısının e-posta adresini yazın.
   
    e. Seçin **kullanıcı yönerge e-posta Gönder**.
   
    f. Tıklayın **kullanıcıları ekleme**.

     >[!NOTE]
     > Kullanıcı oluşturduktan sonra bunu etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren Azure AD kullanıcı bir e-posta gönderilir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Mozy Kurumsal kutucuğa tıkladığınızda, size otomatik olarak Mozy SSO'yu ayarlayın kuruluş oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

