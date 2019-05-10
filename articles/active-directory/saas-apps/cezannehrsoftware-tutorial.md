---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Cezanne ik yazılım | Microsoft Docs'
description: Azure Active Directory ve Cezanne ik yazılım arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 62b42e15-c282-492d-823a-a7c1c539f2cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc4d96b900090cd217b4b49b1af2f09762c0da84
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65407039"
---
# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Öğretici: Cezanne ik yazılım ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Cezanne ik yazılım tümleştirme konusunda bilgi edinin.
Azure AD ile Cezanne ik yazılım tümleştirme ile aşağıdaki avantajları sağlar:

* Cezanne ik yazılım erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Cezanne ik yazılım için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Cezanne ik yazılımıyla yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Cezanne ik yazılım çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Destekleyen Cezanne ik yazılımı **SP** tarafından başlatılan

## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Galeriden Cezanne ik yazılım ekleme

Azure AD'de Cezanne ik yazılım tümleştirmesini yapılandırmak için Cezanne ik yazılım Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Cezanne ik yazılım eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Cezanne ik yazılım**seçin **Cezanne ik yazılım** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Cezanne ik yazılım](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Cezanne ik yazılım adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Cezanne ik yazılımla ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Cezanne ik yazılım ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Cezanne ik yazılım çoklu oturum açmayı yapılandırma](#configure-cezanne-hr-software-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Cezanne ik yazılım test kullanıcısı oluşturma](#create-cezanne-hr-software-test-user)**  - kullanıcı Azure AD gösterimini bağlı Cezanne ik yazılım Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Cezanne ik yazılımıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Cezanne ik yazılım** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Cezanne ik yazılım etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://w3.cezanneondemand.com/CezanneOnDemand/-/<tenantidentifier>`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna URL'yi yazın: `https://w3.cezanneondemand.com/CezanneOnDemand/`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://w3.cezanneondemand.com:443/cezanneondemand/-/<tenantidentifier>/Saml/samlp`
    
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve yanıt URL'si ile güncelleştirin. İlgili kişi [Cezanne ik yazılım istemcisi Destek ekibine](https://cezannehr.com/services/support/) bu değerleri almak için.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Cezanne ik yazılımını kurma** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-cezanne-hr-software-single-sign-on"></a>İK Cezanne yazılım çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Cezanne ik yazılım kiracınıza yönetici olarak oturum.

2. Sol gezinti bölmesinde **sistemi Kurulum**. Git **güvenlik ayarları**. Ardından gidin **çoklu oturum açma yapılandırması**.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

3. İçinde **şu çoklu oturum açma (SSO) hizmet kullanarak oturum açmasına imkan tanıyın** paneli, onay **SAML 2.0** kutusunda ve seçin **Gelişmiş Yapılandırma** seçeneği.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

4. Tıklayın **yeni Ekle** düğmesi.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

5. Aşağıdaki adımları gerçekleştirin **SAML 2.0 kimlik SAĞLAYICISI** bölümü.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    a. Kimlik sağlayıcınız olarak adını **görünen ad**.

    b. İçinde **varlığı tanımlayıcısı** metin değerini yapıştırın **Azure Ad tanımlayıcısı** hangi Azure portaldan kopyaladığınız.

    c. Değişiklik **SAML bağlama** 'Gönderisinin'.

    d. İçinde **güvenlik belirteci Hizmeti uç noktası** metin değerini yapıştırın **oturum açma URL'si** hangi Azure portaldan kopyaladığınız.

    e. Kullanıcı Kimliği öznitelik adı metin kutusuna girin `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    f. Tıklayın **karşıya** Azure portalından indirilen sertifikayı karşıya yüklemek için simge.

    g. **Tamam** düğmesine tıklayın.

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

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

Bu bölümde, Azure çoklu oturum açma Cezanne ik yazılıma erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Cezanne ik yazılım**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Cezanne ik yazılım**.

    ![Uygulamalar listesinde Cezanne ik yazılım bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-cezanne-hr-software-test-user"></a>Cezanne ik yazılım test kullanıcısı oluşturma

Azure AD kullanıcılarının Cezanne ik yazılımına oturum etkinleştirmek için bunlar Cezanne ik yazılımına sağlanması gerekir. Cezanne ik söz konusu olduğunda, sağlama elle bir görevin yazılımdır.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Cezanne ik yazılım şirket sitenize yönetici olarak oturum açın.

2. Sol gezinti bölmesinde **sistemi Kurulum**. Git **kullanıcıları yönetme**. Ardından gidin **yeni kullanıcı Ekle**.

    ![Yeni kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "yeni kullanıcı")

3. Üzerinde **kişi ayrıntıları** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeni kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "yeni kullanıcı")

    a. Ayarlama **iç kullanıcı** OFF olarak.

    b. İçinde **ad** metin kutusu, kullanıcının ilk adını yazın ister **Britta**.  

    c. İçinde **Soyadı** metin kullanıcı adının türünü ister **Simon**.

    d. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

4. Üzerinde **hesap bilgileri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeni kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "yeni kullanıcı")

    a. İçinde **kullanıcıadı** metin kutusuna kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    b. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.

    c. Seçin **ik Professional** olarak **güvenlik rolü**.

    d. **Tamam**'ı tıklatın.

5. Gidin **çoklu oturum açma** sekmenize **yeni Ekle** içinde **SAML 2.0 tanımlayıcıları** alan.

    ![Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "kullanıcı")

6. Kimlik sağlayıcınızı seçin **kimlik sağlayıcısı** ve metin kutusundaki **kullanıcı tanımlayıcısı**, Britta Simon hesabının e-posta adresi girin.

    ![Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "kullanıcı")

7. Tıklayın **Kaydet** düğmesi.

    ![Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "kullanıcı")

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Cezanne ik yazılım kutucuğa tıkladığınızda, otomatik olarak Cezanne ik SSO'yu ayarlama yazılım için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
