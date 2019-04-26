---
title: 'Öğretici: FirmPlay - çalışan danışmanlık işe alma için Azure Active Directory tümleştirmesiyle | Microsoft Docs'
description: Azure Active Directory ve FirmPlay - işe alma için çalışan danışmanlık arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: dc1d7032f80babf8686397ae4dcf0043f456acbd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60278644"
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a>Öğretici: Azure Active Directory tümleştirmesiyle FirmPlay - çalışan danışmanlık için işe alma

Bu öğreticide, Azure Active Directory (Azure AD) ile işe alma için çalışan danışmanlık FirmPlay - tümleştirme konusunda bilgi edinin.
FirmPlay - Azure AD ile işe alma için çalışan danışmanlık tümleştirme ile aşağıdaki avantajları sağlar:

* FirmPlay - çalışan danışmanlık işe alma için erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak FirmPlay - çalışan danışmanlık işe alma (çoklu oturum açma) ile kendi Azure AD hesapları için oturum, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi FirmPlay - için işe alma, çalışan danışmanlık ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* FirmPlay - çalışan danışmanlık işe alma çoklu oturum açma için abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* İşe alma için çalışan danışmanlık FirmPlay - destekleyen **SP** tarafından başlatılan

## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-the-gallery"></a>İşe alma Galerisi için çalışan danışmanlık FirmPlay - ekleme

FirmPlay - Azure AD'ye işe alma için çalışan danışmanlık tümleştirmesini yapılandırmak için işe alma galerisinden listenize yönetilen SaaS uygulamaları için çalışan danışmanlık FirmPlay - eklemeniz gerekir.

**İşe alma Galerisi için çalışan danışmanlık FirmPlay - eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **FirmPlay - işe alma için çalışan danışmanlık**seçin **FirmPlay - işe alma için çalışan danışmanlık** sonucu panelinden ardından **Ekle** düğmesi uygulama ekleyin.

     ![FirmPlay - çalışan danışmanlık için sonuç listesinde işe alma](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma FirmPlay ile test etme - işe alma için çalışan danışmanlık adlı bir test kullanıcı tabanlı **Britta Simon**.
İş, bir Azure AD kullanıcısının FirmPlay - ilgili kullanıcı arasında bir bağlantı ilişki için çoklu oturum açma için işe alma için çalışan danışmanlık kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma FirmPlay ile-test etmek için çalışan danışmanlık işe alma için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[FirmPlay - çalışan danışmanlık işe çoklu oturum açma için yapılandırma](#configure-firmplay---employee-advocacy-for-recruiting-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[İşe alma test kullanıcısı için çalışan danışmanlık FirmPlay - oluşturma](#create-firmplay---employee-advocacy-for-recruiting-test-user)**  - FirmPlay içinde bir karşılığı Britta simon'un sahip - çalışan, işe alma için danışmanlık kullanıcı Azure AD gösterimini bağlantılıdır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma FirmPlay - çalışan danışmanlık işe alma için yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **FirmPlay - işe alma için çalışan danışmanlık** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![FirmPlay - çalışan danışmanlık işe etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<your-subdomain>.firmplay.com/`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [FirmPlay - işe istemci destek ekibi için çalışan danışmanlık](mailto:engineering@firmplay.com) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **FirmPlay - çalışan danışmanlık işe alma için ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-firmplay---employee-advocacy-for-recruiting-single-sign-on"></a>Çoklu oturum açma işe alma için çalışan danışmanlık FirmPlay - yapılandırma

Çoklu oturum açmayı yapılandırma **FirmPlay - işe alma için çalışan danışmanlık** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)** ve uygun URL'ler için Azure portalından [ FirmPlay - işe alma destek ekibi için çalışan danışmanlık](mailto:engineering@firmplay.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

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

Bu bölümde, Azure çoklu oturum açma FirmPlay - çalışan danışmanlık işe alma için erişim vermeye tarafından kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **FirmPlay - işe alma için çalışan danışmanlık**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **FirmPlay - işe alma için çalışan danışmanlık**.

    ![FirmPlay - uygulamalar listesini işe alma bağlantısı için çalışan danışmanlık](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-firmplay---employee-advocacy-for-recruiting-test-user"></a>İşe alma test kullanıcısı için çalışan danışmanlık FirmPlay - oluşturma

Bu bölümde, Britta Simon FirmPlay - işe alma için çalışan danışmanlık adlı bir kullanıcı oluşturun. Çalışmak [FirmPlay - işe alma destek ekibi için çalışan danışmanlık](mailto:engineering@firmplay.com) FirmPlay - çalışan danışmanlık işe alma platform için kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

FirmPlay - çalışan danışmanlık erişim Paneli'nde işe alma kutucuk için tıkladığınızda, otomatik olarak için FirmPlay - çalışan danışmanlık işe alma için oturum SSO'yu ayarlamak için. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

