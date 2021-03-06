---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Aha! | Microsoft Docs'
description: Azure Active Directory ve Aha arasında çoklu oturum açmayı yapılandırmayı öğrenin!
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: de27afabe024f08cb80a7b31cfb1b664684315a8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107239"
---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a>Öğretici: Azure Active Directory Tümleştirmesi ile Aha!

Bu öğreticide, Aha tümleştirmeyi öğrenin! Azure Active Directory (Azure AD).
AHA tümleştirme! Azure AD ile aşağıdaki faydaları sağlar:

* Aha erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Aha için oturum açmış, kullanıcıların etkinleştirebilirsiniz! (Çoklu oturum açma) ile Azure AD hesaplarına.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Aha yapılandırılamadı!, aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* AHA! Çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* AHA! destekleyen **SP** tarafından başlatılan
* AHA! destekleyen **zamanında** kullanıcı sağlama

## <a name="adding-aha-from-the-gallery"></a>AHA ekleme! Galeriden

Aha tümleştirmesini yapılandırmak için! Azure AD ile Aha eklemeniz gerekir! Galeriden listenizi yönetilen SaaS uygulamaları için.

**AHA eklemek için! Galeriden, aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Aha!** seçin **Aha!** Sonuç panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![AHA! Sonuçlar listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Aha ile test etme! adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma, bir Azure AD kullanıcısının Aha ilgili kullanıcı arasında bir bağlantı ilişki için! kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Aha ile test etmek için!, şu yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[AHA yapılandırma! Çoklu oturum açma](#configure-aha-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[AHA oluşturun! Kullanıcı test](#create-aha-test-user)**  - Aha içinde bir karşılığı Britta simon'un sağlamak için! Bu kullanıcı Azure AD gösterimini bağlıdır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Aha yapılandırmak için!, aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Aha!** Uygulama Tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![AHA! Etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.aha.io/session/new`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.aha.io`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [Aha! İstemci Destek ekibine](https://www.aha.io/company/contact) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Aha ayarlayın!** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-aha-single-sign-on"></a>AHA yapılandırma! Çoklu oturum açma

1. Farklı bir web tarayıcı penceresinde, Aha için oturum açın! Yönetici olarak şirketin sitesi.

2. Üstteki menüden **ayarları**.

    ![Ayarları](./media/aha-tutorial/IC798950.png "ayarları")

3. Tıklayın **hesabı**.
  
    ![Profili](./media/aha-tutorial/IC798951.png "profili")

4. Tıklayın **güvenlik ve çoklu oturum açma**.

    ![Güvenlik ve çoklu oturum açma](./media/aha-tutorial/IC798952.png "güvenlik ve çoklu oturum açma")

5. İçinde **çoklu oturum açma** bölümünde olarak **kimlik sağlayıcısı**seçin **SAML2.0**.

    ![Güvenlik ve çoklu oturum açma](./media/aha-tutorial/IC798953.png "güvenlik ve çoklu oturum açma")

6. Üzerinde **çoklu oturum açma** yapılandırma sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma](./media/aha-tutorial/IC798954.png "çoklu oturum açma")

    a. İçinde **adı** metin yapılandırmanız için bir ad yazın.

    b. İçin **kullanarak yapılandırma**seçin **meta veri dosyası**.

    c. İndirilen meta verileri dosyanızı karşıya yüklemek için tıklayın **Gözat**.

    d. Tıklayın **güncelleştirme**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Aha erişim vererek Britta Simon etkinleştirin!.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Aha!** .

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Aha!** .

    ![Aha! Uygulamalar listesinde bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-aha-test-user"></a>AHA oluşturun! Test kullanıcısı

Bu bölümde, Britta Simon adlı bir kullanıcı Aha oluşturuldu!. AHA! Varsayılan olarak etkin olan tam zamanında kullanıcı hazırlama, destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı Aha içinde zaten mevcut değilse!, yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Aha tıkladığınızda! Aha için otomatik olarak imzalanan erişim Paneli'nde; kutucuğuna! kendisi için SSO'yu ayarlayın. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
