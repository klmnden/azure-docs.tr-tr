---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle InstaVR Görüntüleyicisi | Microsoft Docs'
description: Azure Active Directory ve InstaVR Görüntüleyicisi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 13ffa29f-d0a5-4b21-b296-cfd76f380940
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c8acc835a7f18ee673f0857f65d49eed59638a6d
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65898086"
---
# <a name="tutorial-azure-active-directory-integration-with-instavr-viewer"></a>Öğretici: Azure Active Directory tümleştirmesiyle InstaVR Görüntüleyicisi

Bu öğreticide, Azure Active Directory (Azure AD) ile InstaVR Görüntüleyicisi tümleştirme konusunda bilgi edinin.
Azure AD ile InstaVR Görüntüleyicisi tümleştirme ile aşağıdaki avantajları sağlar:

* InstaVR Görüntüleyicisi erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak InstaVR Görüntüleyicisi (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi InstaVR Görüntüleyici ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik InstaVR Görüntüleyicisi çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* InstaVR Görüntüleyicisi'ni destekleyen **SP** tarafından başlatılan
* InstaVR Görüntüleyicisi'ni destekleyen **zamanında** kullanıcı sağlama

## <a name="adding-instavr-viewer-from-the-gallery"></a>Galeriden InstaVR Görüntüleyicisi ekleme

Azure AD'de InstaVR Görüntüleyicisi tümleştirmesini yapılandırmak için InstaVR Görüntüleyicisi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden InstaVR Görüntüleyicisi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **InstaVR Görüntüleyicisi**seçin **InstaVR Görüntüleyicisi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde InstaVR Görüntüleyicisi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve InstaVR Görüntüleyicisi ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının InstaVR Görüntüleyicisindeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma InstaVR Görüntüleyicisi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[InstaVR Görüntüleyicisi çoklu oturum açmayı yapılandırma](#configure-instavr-viewer-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[InstaVR Görüntüleyicisi'ni test kullanıcısı oluşturma](#create-instavr-viewer-test-user)**  - kullanıcı Azure AD gösterimini bağlı InstaVR Görüntüleyicisi'nde Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma InstaVR Görüntüleyici ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **InstaVR Görüntüleyicisi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![InstaVR Görüntüleyicisi etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://console.instavr.co/auth/saml/login/<WEBPackagedURL>`

    > [!NOTE]
    > Oturum açma URL'si için herhangi bir sabit desen yoktur. Paketleme InstaVR Görüntüleyicisi müşterinin Web oluşturulur. Bu paket ve her müşteri için benzersizdir. Tam oturum açma URL'si InstaVR görüntüleyicisini oturum açmalısınız almak için örnek ve paketleme web.

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://console.instavr.co/auth/saml/sp/<WEBPackagedURL>`

    > [!NOTE]
    > Tanımlayıcı değerini gerçek değil. Bu değer, bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek tanımlayıcı değerini güncelleştirin.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve **Federasyon meta veri dosyası** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadata-certificatebase64.png)

6. Üzerinde **InstaVR Görüntüleyicini ayarlayın** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-instavr-viewer-single-sign-on"></a>InstaVR Görüntüleyicisi çoklu oturum açmayı yapılandırın

1. Yeni bir web tarayıcı penceresi ve günlük Görüntüleyici InstaVR şirket sitenize yönetici olarak açın.

2. Tıklayarak **kullanıcı simgesi** seçip **hesabı**.

    ![InstaVR Görüntüleyicisi'ni yapılandırma](media/instavr-viewer-tutorial/tutorial-instavr-viewer-account.png)

3. Ekranı aşağı kaydırarak **SAML kimlik doğrulaması** ve aşağıdaki adımları gerçekleştirin:

    ![InstaVR Görüntüleyicisi'ni yapılandırma](media/instavr-viewer-tutorial/tutorial-instavr-viewer-configure.png)

    a. İçinde **SSO URL** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    b. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri.

    c. İçinde **varlık kimliği** metin kutusu, yapıştırma **Azure Ad tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    d. İndirilen sertifika dosyanızı karşıya yüklemek için tıklayın **güncelleştirme**.

    e. İndirilen Federasyon meta verileri dosyanızı karşıya yüklemek için tıklayın **güncelleştirme**.

    f. Kopyalama **varlık kimliği** yapıştırın ve değer **tanımlayıcı (varlık kimliği)** metin kutusunu **temel SAML yapılandırma** bölümünde Azure portalında.

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

Bu bölümde, Azure çoklu oturum açma InstaVR Görüntüleyicisi'ne erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **InstaVR Görüntüleyicisi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **InstaVR Görüntüleyicisi**.

    ![Uygulamalar listesinde InstaVR Görüntüleyicisi bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-instavr-viewer-test-user"></a>InstaVR Görüntüleyicisi'ni test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı InstaVR Görüntüleyicisi'nde oluşturulur. InstaVR Görüntüleyicisi just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı InstaVR Görüntüleyicisi'nde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur. Herhangi bir sorunla karşılaştığı için iletişim kurun [InstaVR Görüntüleyicisi, Destek ekibine](mailto:contact@instavr.co).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

1. Yeni bir web tarayıcı penceresi ve günlük Görüntüleyici InstaVR şirket sitenize yönetici olarak açın.

2. Seçin **paket** seçin ve sol gezinti bölmesi **Web için paket oluşturma**.

    ![InstaVR Görüntüleyicisi'ni yapılandırma](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing1.png)

3. **Download** (İndir) seçeneğini belirleyin.

    ![InstaVR Görüntüleyicisi'ni yapılandırma](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing2.png)

4. Seçin **barındırılan sayfasını aç** bundan sonra bu yönlendirilir Azure AD oturum açma için.

    ![InstaVR Görüntüleyicisi'ni yapılandırma](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing3.png)

5. Üzerinden SSO, Azure AD'ye başarıyla oturum açmak için Azure AD kimlik bilgilerinizi girin.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
