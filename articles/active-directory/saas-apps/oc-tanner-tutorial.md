---
title: 'Öğretici: O.C. ile Azure Active Directory Tümleştirme Etikan - AppreciateHub | Microsoft Docs'
description: Azure Active Directory ve O.C. arasında çoklu oturum açmayı yapılandırma hakkında bilgi edinin Tanner - AppreciateHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 21cbef1532928d51ba0c9f11e80304933df505b1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60242817"
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Öğretici: O.C. ile Azure Active Directory Tümleştirme Tanner - AppreciateHub

Bu öğreticide, O.C. tümleştirmeyi öğrenin Etikan - AppreciateHub ile Azure Active Directory (Azure AD).
O.C. tümleştirme Azure AD ile AppreciateHub Etikan - ile aşağıdaki avantajları sağlar:

* O.C. erişimi, Azure AD'de denetleyebilirsiniz Tanner - AppreciateHub.
* Otomatik olarak O.C. için oturum açmış, kullanıcıların etkinleştirebilirsiniz. Etikan - Azure AD hesaplarıyla AppreciateHub (çoklu oturum açma).
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile O.C. yapılandırmak için Etikan - AppreciateHub, aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* O.C. Etikan - aboneliği etkin AppreciateHub çoklu oturum açma

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* O.C. AppreciateHub Etikan - destekleyen **IDP** tarafından başlatılan

## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>O.C. ekleme Etikan - AppreciateHub Galerisi

O.C. tümleştirmesini yapılandırmak için Etikan - AppreciateHub Azure AD'ye ihtiyacınız O.C. eklemek Etikan - galerisinden AppreciateHub listenizi yönetilen SaaS uygulamaları için.

**O.C. eklemek için Etikan - AppreciateHub galerisinden, aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **O.C. Etikan - AppreciateHub**seçin **O.C. Etikan - AppreciateHub** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![O.C. Etikan - AppreciateHub sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma O.C. ile test etme Etikan - AppreciateHub adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma, bir Azure AD kullanıcısının O.C. ilgili kullanıcı arasında bir bağlantı ilişkisi Etikan - AppreciateHub kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma O.C. ile test etmek için Etikan - AppreciateHub, aşağıdaki yapı taşlarını tamamlamanız gereken:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[O.C. yapılandırın -AppreciateHub çoklu oturum açma Etikan](#configure-oc-tanner---appreciatehub-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[O.C. oluşturma Etikan - AppreciateHub test kullanıcısı](#create-oc-tanner---appreciatehub-test-user)**  - O.C. içinde bir karşılığı Britta simon'un sağlamak için Etikan - kullanıcı Azure AD gösterimini bağlı AppreciateHub.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile O.C. yapılandırmak için Etikan - AppreciateHub, aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **O.C. Etikan - AppreciateHub** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    >[!NOTE]
    >İndirebileceğiniz **hizmet sağlayıcısı meta veri dosyası** gelen [burada](https://fed.appreciatehub.com/fed/sp/metadata)

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![Meta veri dosyasını yükleyin](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik temel SAML yapılandırma bölümünde doldurulur.

     ![O.C. Etikan - AppreciateHub etki alanı ve URL'ler tek bilgi'oturum açma](common/idp-intiated.png)

    > [!Note]
    > Varsa **tanımlayıcı** ve **yanıt URL'si** değerlerin değil otomatik polulated alın ve ardından Lütfen değerlerin ihtiyacınıza göre el ile doldurun. İlgili kişi [O.C. Etikan - AppreciateHub istemci Destek ekibine](mailto:sso@octanner.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **O.C. ayarlayın Etikan - AppreciateHub** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-oc-tanner---appreciatehub-single-sign-on"></a>O.C. yapılandırın Etikan - AppreciateHub çoklu oturum açma

Çoklu oturum açmayı yapılandırma **O.C. Etikan - AppreciateHub** tarafı, indirilen göndermek için ihtiyacınız **Federasyon meta verileri XML** ve uygun Azure portalına kopyalanan URL'lerden [O.C. Etikan - AppreciateHub Destek ekibine](mailto:sso@octanner.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü kullanıcı gibi BrittaSimon@contoso.com.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için O.C. erişim vererek Britta Simon etkinleştir Tanner - AppreciateHub.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **O.C. Etikan - AppreciateHub**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **O.C. Etikan - AppreciateHub**.

    ![O.C. Etikan - uygulamalar listesinde AppreciateHub bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-oc-tanner---appreciatehub-test-user"></a>O.C. oluşturma Etikan - AppreciateHub test kullanıcısı

Bu bölümün amacı O.C. Britta Simon adlı bir kullanıcı oluşturmaktır. Tanner - AppreciateHub.

**Britta Simon O.C. içinde adlı bir kullanıcı oluşturmak için Etikan - AppreciateHub, aşağıdaki adımları gerçekleştirin:**

Sorun, [O.C. Etikan - AppreciateHub Destek ekibine](mailto:sso@octanner.com) Nameıd özniteliği olarak, Azure AD'de Britta simon'un kullanıcı adı ile aynı değere sahip bir kullanıcı oluşturun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

O.C. tıkladığınızda Etikan - AppreciateHub kutucuk erişim Paneli'nde, otomatik olarak için O.C. açmış olmanız Etikan - AppreciateHub SSO'yu ayarlama. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
