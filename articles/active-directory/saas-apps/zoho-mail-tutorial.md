---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Zoho | Microsoft Docs'
description: Azure Active Directory ve Zoho arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 69c2599a2ddd09cbaf869bf4d9e21a8032855cce
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67086174"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a>Öğretici: Zoho ile Azure Active Directory Tümleştirme

Bu öğreticide, Zoho, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
Azure AD ile Zoho tümleştirme ile aşağıdaki avantajları sağlar:

* Zoho erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Zoho için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Zoho yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Zoho çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Zoho destekler **SP** tarafından başlatılan

## <a name="adding-zoho-from-the-gallery"></a>Galeriden Zoho ekleme

Azure AD'de Zoho tümleştirmesini yapılandırmak için Zoho Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Zoho eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zoho**seçin **Zoho** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Zoho](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Zoho adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Zoho ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Zoho ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Zoho çoklu oturum açmayı yapılandırma](#configure-zoho-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Zoho test kullanıcısı oluşturma](#create-zoho-test-user)**  - kullanıcı Azure AD gösterimini bağlı Zoho Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Zoho yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Zoho** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Zoho etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<company name>.zohomail.com`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Zoho istemci Destek ekibine](https://www.zoho.com/mail/contact.html) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Zoho kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-zoho-single-sign-on"></a>Zoho tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Zoho posta şirket sitenize yönetici olarak oturum.

2. Git **Denetim Masası**.
   
    ![Denetim Masası](./media/zoho-mail-tutorial/ic789607.png "Denetim Masası")

3. Tıklayın **SAML kimlik doğrulaması** sekmesi.
   
    ![SAML kimlik doğrulaması](./media/zoho-mail-tutorial/ic789608.png "SAML kimlik doğrulaması")

4. İçinde **SAML kimlik doğrulama ayrıntıları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SAML kimlik doğrulama ayrıntıları](./media/zoho-mail-tutorial/ic789609.png "SAML kimlik doğrulaması ayrıntıları")
   
    a. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    b. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    c. İçinde **parola URL'yi Değiştir** metin kutusu, yapıştırma **parola URL'yi Değiştir** , Azure Portalı'ndan kopyaladığınız.
       
    d. Not Defteri'nde Azure portalından indirilen, base-64 kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **PublicKey** metin.
   
    e. Olarak **algoritması**seçin **RSA**.
   
    f. **Tamam**'ı tıklatın.

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

Bu bölümde, Zoho için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Zoho**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Zoho**.

    ![Uygulamalar listesini Zoho bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-zoho-test-user"></a>Zoho test kullanıcısı oluşturma

Azure AD kullanıcılarının Zoho posta oturum etkinleştirmek için bunlar Zoho posta sağlanması gerekir. Zoho posta söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

> [!NOTE]
> Herhangi diğer Zoho posta kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Zoho e-posta yoluyla sağlanan.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **Zoho posta** yönetici olarak şirketin site.

1. Git **Denetim Masası \> posta ve belgeler**.

1. Git **kullanıcı ayrıntıları \> kullanıcı ekleme**.
   
    ![Kullanıcı ekleme](./media/zoho-mail-tutorial/ic789611.png "kullanıcı ekleme")

1. Üzerinde **kullanıcı ekleme** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı ekleme](./media/zoho-mail-tutorial/ic789612.png "kullanıcı ekleme")
   
    a. İçinde **ad** metin kutusu, kullanıcının ilk adını yazın ister **Britta**.

    b. İçinde **Soyadı** metin kullanıcı adının türünü ister **Simon**.

    c. İçinde **e-posta kimliği** metin kutusuna kullanıcı e-posta kimliği türünü ister **brittasimon\@contoso.com**.

    d. İçinde **parola** metin kutusu, kullanıcının parolasını girin.
   
    e. **Tamam**'ı tıklatın.  
      
    > [!NOTE]
    > Azure Active Directory hesap sahibi bu etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Zoho kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Zoho için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

