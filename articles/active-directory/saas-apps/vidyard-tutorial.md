---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Vidyard | Microsoft Docs'
description: Azure Active Directory ve Vidyard arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bed7df23-6e13-4e7c-b4cc-53ed4804664d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 5ff939fa514e067c26daf609270ef49de8c16db2
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66143171"
---
# <a name="tutorial-azure-active-directory-integration-with-vidyard"></a>Öğretici: Vidyard ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Vidyard tümleştirme konusunda bilgi edinin.
Azure AD ile Vidyard tümleştirme ile aşağıdaki avantajları sağlar:

* Vidyard erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Vidyard için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Vidyard yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik Vidyard çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Vidyard destekler **SP** ve **IDP** tarafından başlatılan

* Vidyard destekler **zamanında** kullanıcı sağlama

## <a name="adding-vidyard-from-the-gallery"></a>Galeriden Vidyard ekleme

Azure AD'de Vidyard tümleştirmesini yapılandırmak için Vidyard Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Vidyard eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Vidyard**seçin **Vidyard** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Vidyard](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Vidyard adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Vidyard ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Vidyard ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Vidyard çoklu oturum açmayı yapılandırma](#configure-vidyard-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Vidyard test kullanıcısı oluşturma](#create-vidyard-test-user)**  - kullanıcı Azure AD gösterimini bağlı Vidyard Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Vidyard yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Vidyard** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Vidyard etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://secure.vidyard.com/sso/saml/<unique id>/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://secure.vidyard.com/sso/saml/<unique id>/consume`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Vidyard etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://secure.vidyard.com/sso/saml/<unique id>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma, öğreticinin ilerleyen bölümlerinde açıklanan URL ile güncelleştirir. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **Vidyard kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-vidyard-single-sign-on"></a>Vidyard tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Vidyard yazılım şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Vidyard Panoda **grubu** > **güvenlik**

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure1.png)

3. Tıklayın **yeni profili** sekmesi.

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure2.png)

4. İçinde **SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure3.png)

    a. Lütfen genel profil adı girin **profil adı** metin.

    b. Kopyalama **SSO kullanıcı oturum açma sayfasına** yapıştırın ve değer **oturum açma URL'si** metin kutusunda **temel SAML yapılandırma** bölümü Azure portalı.

    c. Kopyalama **ACS URL** yapıştırın ve değer **yanıt URL'si** metin kutusunda **temel SAML yapılandırma** bölümü Azure portalı.

    d. Kopyalama **veren/meta veri URL'si** yapıştırın ve değer **tanımlayıcı** metin kutusunda **temel SAML yapılandırma** bölümü Azure portalı.

    e. Not Defteri'nde Azure portalından indirilen sertifika dosyanızı açın ve ardından yapıştırın **X.509 sertifikası** metin.

    f. İçinde **SAML uç nokta URL'si** metin değerini yapıştırın **oturum açma URL'si** Azure portaldan kopyaladığınız.

    g. **Onayla**'ya tıklayın.

5. Çoklu oturum açma sekmesinden seçin **atama** yanındaki mevcut bir profili

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure4.png)

    > [!NOTE]
    > SSO profili oluşturduktan sonra kullanıcılar Azure üzerinden erişim gerektiren herhangi bir gruplarına atayın. Kullanıcı Grup içinde atanmış olan mevcut değilse Vidyard otomatik olarak bir kullanıcı hesabı oluşturun ve gerçek zamanlı rollerine atayın.

6. Görünür olan Kuruluş grubunuzu seçin **kullanılabilir gruplar atanacak**.

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure5.png)

7. Atanan gruplar altında gördüğünüz **şu an atanan grup**. Bir rol için grubu tıklatın ve kuruluş göre seçin **Onayla**.

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure6.png)

    > [!NOTE]
    > Daha fazla bilgi için [bu belge](https://knowledge.vidyard.com/hc/articles/360009990033-SAML-based-Single-Sign-On-SSO-in-Vidyard).

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Vidyard erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Vidyard**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Vidyard**.

    ![Uygulamalar listesinde Vidyard bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-vidyard-test-user"></a>Vidyard test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Vidyard oluşturulur. Vidyard just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı Vidyard içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Vidyard Destek ekibine](mailto:support@vidyard.com).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Vidyard kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Vidyard için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

