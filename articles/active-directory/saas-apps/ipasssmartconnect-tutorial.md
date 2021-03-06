---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile iPass SmartConnect | Microsoft Docs'
description: Azure Active Directory ve iPass SmartConnect arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: dee6d039-f9bb-49a2-a408-5ed40ef17d9f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 9417d7b957d69dc802ecb2f9f78723eb7aba08ba
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67099847"
---
# <a name="tutorial-azure-active-directory-integration-with-ipass-smartconnect"></a>Öğretici: İPass SmartConnect ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iPass SmartConnect tümleştirme konusunda bilgi edinin.
Azure AD ile iPass SmartConnect tümleştirme ile aşağıdaki avantajları sağlar:

* İPass SmartConnect erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak iPass SmartConnect (çoklu oturum açma) için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi SmartConnect iPass ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* iPass SmartConnect tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* iPass SmartConnect destekler **SP ve IDP** tarafından başlatılan
* iPass SmartConnect destekler **zamanında** kullanıcı sağlama

## <a name="adding-ipass-smartconnect-from-the-gallery"></a>Galeriden iPass SmartConnect ekleme

Azure AD'de iPass SmartConnect tümleştirmesini yapılandırmak için iPass SmartConnect Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden iPass SmartConnect eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **iPass SmartConnect**seçin **iPass SmartConnect** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![sonuç listesinde iPass SmartConnect](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma iPass SmartConnect tabanlı adlı bir test kullanıcı ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının iPass ilgili kullanıcı arasında bir bağlantı ilişki SmartConnect kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma iPass SmartConnect ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[İPass SmartConnect çoklu oturum açmayı yapılandırma](#configure-ipass-smartconnect-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[İPass SmartConnect test kullanıcısı oluşturma](#create-ipass-smartconnect-test-user)**  - Britta Simon iPass kullanıcı Azure AD gösterimini bağlı SmartConnect içinde bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma SmartConnect iPass ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **iPass SmartConnect** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde kullanıcısının clonedatabase'i uygulama zaten Azure ile önceden tümleşik olarak herhangi bir adımı gerçekleştirmek.

    ![iPass SmartConnect etki alanı ve URL'ler çoklu oturum açma bilgileri](common/preintegrated.png)

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![iPass SmartConnect etki alanı ve URL'ler çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL yazın:  `https://om-activation.ipass.com/ClientActivation/ssolanding.go`

6. iPass SmartConnect uygulama belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

7. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda kullanarak talep Düzenle **düzenleme simgesi** veya talep kullanarak **Ekle yeni talep**SAML belirteci özniteliği yukarıdaki görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Ad |  Kaynak özniteliği|
    | ---------------| ----------|
    | FirstName | User.givenName |
    | Soyadı | User.surname |
    | email | User.userPrincipalName |
    | username | User.userPrincipalName |
    | | |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

9. Üzerinde **SmartConnect iPass kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-ipass-smartconnect-single-sign-on"></a>İPass SmartConnect çoklu oturum açmayı yapılandırın

Çoklu oturum açmayı yapılandırma **iPass SmartConnect** tarafı, indirilen göndermek için ihtiyacınız **Federasyon meta verileri XML** ve uygun Azure portalına kopyalanan URL'lerden [iPass SmartConnect Destek](mailto:help@ipass.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma iPass SmartConnect erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **iPass SmartConnect**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **iPass SmartConnect**.

    ![Uygulamalar listesinde SmartConnect bağlantı iPass](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-ipass-smartconnect-test-user"></a>İPass SmartConnect test kullanıcısı oluşturma

Bu bölümde, Britta Simon SmartConnect iPass içinde adlı bir kullanıcı oluşturun. Çalışmak [iPass SmartConnect Destek ekibine](mailto:help@ipass.com) kullanıcı veya iPass SmartConnect platformu için bir izin verilenler listesi eklenmeli etki alanı eklemek için. Etki alanı ekibi tarafından eklenirse, kullanıcıların otomatik olarak iPass SmartConnect platforma sağlanan. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

**SP tarafından başlatılan akış içinde uygulamayı test etmek için aşağıdaki adımları gerçekleştirin:**

a. Windows iPass SmartConnect istemci indirme [burada](https://om-activation.ipass.com/ClientActivation/ssolanding.go).

![Uygulamalar listesinde SmartConnect bağlantı iPass](./media/ipasssmartconnect-tutorial/testing3.png)

b. İstemci yükleyip başlatın.

c. Tıklayarak **başlama**.

![Uygulamalar listesinde SmartConnect bağlantı iPass](./media/ipasssmartconnect-tutorial/testing1.png) 

d. Etki alanı ile Azure kullanıcı adını girin. Tıklayarak **devam**. Azure oturum açma sayfasına yönlendirilirsiniz

![Uygulamalar listesinde SmartConnect bağlantı iPass](./media/ipasssmartconnect-tutorial/testing2.png) 

e. İstemci etkinleştirme başarılı kimlik doğrulamadan sonra başlatılır. İstemci etkin.

**IDP tarafından başlatılan akış içinde uygulamayı test etmek için aşağıdaki adımları gerçekleştirin:**

a. Oturum açma [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

b. Üzerinde iPass SmartConnect uygulamaya tıklayın.

c. SSA İyileştiricisine sayfa başlatır, tıklayarak **uygulama Windows için indirin** iPass SmartConnect istemci yüklemek için.

![Uygulamalar listesinde SmartConnect bağlantı iPass](./media/ipasssmartconnect-tutorial/testing4.png)

d. Hüküm ve koşulları kabul sonra etkinleştirme, yükleme, istemci ilk başlatmada otomatik olarak ayarlanır sonra başlar.

e. Etkinleştirme başlatılmazsa etkinleştirmeyi SSA İyileştiricisine sayfasında Etkinleştir düğmesini tıklatın.

f. İstemci etkin.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)