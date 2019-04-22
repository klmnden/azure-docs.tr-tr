---
title: 'Öğretici: Marketo ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Marketo arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/19/2019
ms.author: jeedes
ms.openlocfilehash: 09f452a0971e2a0e74e51edd2db44eecda39c204
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59265772"
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Öğretici: Marketo ile Azure Active Directory Tümleştirme

Bu öğreticide, Marketo Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Marketo Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Marketo erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) için Marketo kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi, Marketo ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Marketo çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Marketo destekler **IDP** tarafından başlatılan

## <a name="adding-marketo-from-the-gallery"></a>Marketo galeri ekleme

Marketo tümleştirmesi Azure AD'de yapılandırmak için Marketo Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Marketo eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Marketo**seçin **Marketo** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Marketo](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Marketo adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve Marketo ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Marketo ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Marketo çoklu oturum açmayı yapılandırma](#configure-marketo-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Marketo test kullanıcısı oluşturma](#create-marketo-test-user)**  - kullanıcı Azure AD gösterimini bağlı Marketo Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Marketo'ya yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Marketo** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Marketo etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://saml.marketo.com/sp`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Marketo istemcisinde Destek ekibine](http://investors.marketo.com/contactus.cfm) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Marketo kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-marketo-single-sign-on"></a>Marketo tek oturum açmayı yapılandırın

1. Uygulamanızın Munchkin Kimliği almak için Marketo yönetici kimlik bilgilerini kullanarak oturum açın ve aşağıdaki işlemleri gerçekleştirin:
   
    a. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.
   
    b. Tıklayın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Tümleştirme menüsüne gidin ve tıklayın **Munchkin bağlantı**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_11.png)
   
    d. Munchkin ekranda gösterilen kodu kopyalayın ve kendi yanıt URL'si Azure AD Yapılandırma Sihirbazı tamamlayın.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_12.png) 

2. İzleyin uygulamada SSO yapılandırmak için aşağıdaki adımları:
   
    a. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.
   
    b. Tıklayın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Tümleştirme menüsüne gidin ve tıklayın **çoklu oturum açma**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_07.png) 
   
    d. SAML ayarlarını etkinleştirmek için tıklayın **Düzenle** düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Etkin** çoklu oturum açma ayarları.
   
    f. Yapıştırma **Azure AD tanımlayıcısı**, **verenin kimliği** metin.
   
    g. İçinde **varlık kimliği** metin olarak URL'sini `http://saml.marketo.com/sp`.
   
    h. Kullanıcı Kimliği konum olarak seçin **ad tanımlayıcısı öğesi**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Kullanıcı tanımlayıcınızı değil UPN değeri öznitelik sekme değerinde değişiklik ise.
   
    i. Azure AD Yapılandırma Sihirbazı'ndan yüklediğiniz sertifikayı yükleyin. **Kaydet** ayarlar.
   
    j. Sayfaları yeniden yönlendirme ayarlarını düzenleyin.
   
    k. Yapıştırma **oturum açma URL'si** içinde **oturum açma URL'si** metin.
   
    m. Yapıştırma **oturum kapatma URL'si** içinde **oturum kapatma URL'si** metin.
   
    m. İçinde **hata URL**, kopyalama, **Marketo örnek URL'si** tıklatıp **Kaydet** düğmesini kullanarak ayarları kaydedin.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_10.png)

3. Kullanıcılar için SSO'yu etkinleştirmek üzere, aşağıdaki işlemleri tamamlayın:
   
    a. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.
   
    b. Tıklayın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Gidin **güvenlik** menüsüne ve ardından **oturum açma ayarları**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_13.png)
   
    d. Denetleme **gerektiren SSO** seçeneği ve **Kaydet** ayarlar.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_14.png)

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Marketo erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Marketo**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Marketo**.

    ![Uygulamalar listesini Marketo bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-marketo-test-user"></a>Marketo test kullanıcısı oluşturma

Bu bölümde, Britta Simon Marketo adlı bir kullanıcı oluşturun. Marketo platformunda bir kullanıcı oluşturmak için aşağıdaki adımları izleyin.

1. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.

2. Tıklayın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_06.png) 

3. Gidin **güvenlik** menüsüne ve ardından **kullanıcıları ve rolleri**
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_19.png)  

4. Tıklayın **yeni kullanıcı davet** kullanıcılar sekmesinde bağlantı
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_15.png) 

5. Yeni kullanıcı davet Sihirbazı'nda aşağıdaki bilgileri doldurun.
   
    a. Kullanıcının girmesi **e-posta** metin kutusuna adresi
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_16.png)
   
    b. Girin **ad** metin kutusuna
   
    c. Girin **Soyadı** metin kutusuna
   
    d. **İleri**’ye tıklayın

6. İçinde **izinleri** sekmesinde **userRoles** tıklatıp **İleri**
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_17.png)
7. Tıklayın **Gönder** düğmesini Kullanıcı Davet Gönder
   
    ![Çoklu oturum açmayı yapılandırın](./media/marketo-tutorial/tutorial_marketo_18.png)

8. Kullanıcı e-posta bildirimi alır ve bağlantısına tıklayın ve hesabı etkinleştirmek için parolayı değiştirmesi gerekir. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Marketo kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Marketo için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

