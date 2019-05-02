---
title: 'Öğretici: Azure Active Directory SensoScientific kablosuz sıcaklık sistem izleme ile tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve SensoScientific kablosuz sıcaklık izleme sistemi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6dc228f3473a4ddca8b5b68cdd99f1fced1a04cd
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64922235"
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Öğretici: Azure Active Directory SensoScientific kablosuz sıcaklık sistem izleme ile tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile SensoScientific kablosuz sıcaklık izleme sistemi tümleştirme konusunda bilgi edinin.
Azure AD ile SensoScientific kablosuz sıcaklık izleme sistemi tümleştirme ile aşağıdaki avantajları sağlar:

* SensoScientific kablosuz sıcaklık izleme sistemi erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak SensoScientific kablosuz sıcaklık izleme sistemine (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi SensoScientific kablosuz sıcaklık izleme sistemi ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik SensoScientific kablosuz sıcaklık izleme sistemi çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SensoScientific kablosuz Sıcaklık İzleme sisteminin desteklediği **IDP** tarafından başlatılan

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a>Galeriden SensoScientific kablosuz sıcaklık sistem izleme ekleme

Azure AD'de SensoScientific kablosuz sıcaklık sistem izleme tümleştirmesini yapılandırmak için SensoScientific kablosuz sıcaklık izleme sistemi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SensoScientific kablosuz sıcaklık sistem izleme eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SensoScientific kablosuz sıcaklık izleme sistemi**seçin **SensoScientific kablosuz sıcaklık izleme sistemi** sonucu panelinden ardından **Ekle**  uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde SensoScientific kablosuz sıcaklık izleme sistemi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SensoScientific kablosuz sıcaklık adlı bir test kullanıcı tabanlı sistem izleme test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı SensoScientific kablosuz sıcaklık izleme sistemi arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SensoScientific kablosuz sıcaklık izleme sistemi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SensoScientific kablosuz sıcaklık izleme sistemi çoklu oturum açmayı yapılandırma](#configure-sensoscientific-wireless-temperature-monitoring-system-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SensoScientific kablosuz sıcaklık izleme sistemi test kullanıcısı oluşturma](#create-sensoscientific-wireless-temperature-monitoring-system-test-user)**  - bir karşılığı Britta simon'un SensoScientific kablosuz sıcaklık kullanıcı Azure AD gösterimini bağlı sistem izleme sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma SensoScientific kablosuz sıcaklık izleme sistemi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **SensoScientific kablosuz sıcaklık izleme sistemi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde kullanıcısının clonedatabase'i uygulama zaten Azure ile önceden tümleşik olarak herhangi bir adımı gerçekleştirmek.

    ![SensoScientific kablosuz sıcaklık izleme sistemi etki alanı ve URL'ler tek oturum açma bilgileri](common/preintegrated.png)

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **SensoScientific kablosuz sıcaklık izleme sistemi kurmalısınız** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-sensoscientific-wireless-temperature-monitoring-system-single-sign-on"></a>İzleme sistemi çoklu oturum açma SensoScientific kablosuz sıcaklık yapılandırın

1. SensoScientific kablosuz sıcaklık izleme sistemi uygulamanıza yönetici olarak oturum açın.

1. Üst gezinti menüsünde **yapılandırma** ve Git **yapılandırma** altında **çoklu oturum açma** üzerinde tek oturum ayarları'nı açın ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png)

    a. Seçin **verenin adı** olarak Azure AD.

    b. İçinde **veren URL'si** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** , Azure Portalı'ndan kopyaladığınız.

    c. İçinde **çoklu oturum açma hizmeti URL'si** metin kutusu, yapıştırma **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    d. İçinde **çoklu oturum kapatma hizmeti URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    e. Azure Portalı'ndan yüklemiş ve buraya yükleyin sertifika göz atın.

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
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SensoScientific kablosuz sıcaklık izleme sistemine erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SensoScientific kablosuz sıcaklık izleme sistemi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **SensoScientific kablosuz sıcaklık izleme sistemi**.

    ![Uygulamalar listesinde SensoScientific kablosuz sıcaklık izleme sistemi bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>SensoScientific kablosuz sıcaklık izleme sistemi test kullanıcısı oluşturma

SensoScientific kablosuz sıcaklık izleme sistemi için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar SensoScientific kablosuz sıcaklık izleme sistemine sağlanması gerekir. Çalışmak [SensoScientific kablosuz sıcaklık izleme sistemi, Destek ekibine](https://www.sensoscientific.com/contact-us/) SensoScientific kablosuz sıcaklık izleme sistemi platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SensoScientific kablosuz sıcaklık izleme sistemi kutucuğa tıkladığınızda, size otomatik olarak SensoScientific kablosuz sıcaklık SSO'yu ayarlama sistem izleme için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

