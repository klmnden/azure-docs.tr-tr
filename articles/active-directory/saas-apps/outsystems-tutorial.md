---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile OutSystems | Microsoft Docs'
description: Azure Active Directory ve OutSystems arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: cf6f99b7-0604-4db2-a72e-0d1a1d643a08
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/102019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e5768b8f7c368e57d55656f0e82b199995be150
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65780947"
---
# <a name="tutorial-azure-active-directory-integration-with-outsystems"></a>Öğretici: OutSystems ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile OutSystems tümleştirme konusunda bilgi edinin.
OutSystems, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* OutSystems erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) için OutSystems, Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile OutSystems yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* OutSystems tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* OutSystems destekler **SP ve IDP** tarafından başlatılan

* OutSystems destekler **zamanında** kullanıcı sağlama

## <a name="adding-outsystems-from-the-gallery"></a>Galeriden OutSystems ekleme

Azure AD'de OutSystems tümleştirmesini yapılandırmak için OutSystems galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden OutSystems eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **OutSystems**seçin **OutSystems** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde OutSystems](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma OutSystems adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının OutSystems ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma OutSystems ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[OutSystems çoklu oturum açmayı yapılandırma](#configure-outsystems-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[OutSystems test kullanıcısı oluşturma](#create-outsystems-test-user)**  - kullanıcı Azure AD gösterimini bağlı OutSystems Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile OutSystems yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **OutSystems** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![OutSystems etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `http://<YOURBASEURL>/IdP`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<YOURBASEURL>/IdP/SSO.aspx`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![OutSystems etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<YOURBASEURL>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [OutSystems istemci Destek ekibine](mailto:support@outsystems.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Üzerinde **OutSystems kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-outsystems-single-sign-on"></a>OutSystems tek oturum açmayı yapılandırın

Çoklu oturum açmayı yapılandırma **OutSystems** tarafı, indirilen göndermek için ihtiyacınız **Federasyon meta verileri XML** ve uygun Azure portalına kopyalanan URL'lerden [OutSystems Destek ekibine](mailto:support@outsystems.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için OutSystems erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **OutSystems**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **OutSystems**.

    ![Uygulamalar listesinde OutSystems bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-outsystems-test-user"></a>OutSystems test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı OutSystems oluşturulur. OutSystems just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. OutSystems bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli OutSystems kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama OutSystems için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

