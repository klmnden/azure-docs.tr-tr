---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile RingCentral | Microsoft Docs'
description: Azure Active Directory ve RingCentral arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5848c875-5185-4f91-8279-1a030e67c510
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.openlocfilehash: cf7fd46526eec2f4ac295381498eb14a06b2f36c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59697320"
---
# <a name="tutorial-azure-active-directory-integration-with-ringcentral"></a>Öğretici: RingCentral ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile RingCentral tümleştirme konusunda bilgi edinin.
Azure AD ile RingCentral tümleştirme ile aşağıdaki avantajları sağlar:

* RingCentral erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) RingCentral için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile RingCentral yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik RingCentral çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* RingCentral destekler **SP** tarafından başlatılan

## <a name="adding-ringcentral-from-the-gallery"></a>Galeriden RingCentral ekleme

Azure AD'de RingCentral tümleştirmesini yapılandırmak için RingCentral Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden RingCentral eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **RingCentral**seçin **RingCentral** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde RingCentral](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma RingCentral adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının RingCentral ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma RingCentral ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[RingCentral çoklu oturum açmayı yapılandırma](#configure-ringcentral-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[RingCentral test kullanıcısı oluşturma](#create-ringcentral-test-user)**  - kullanıcı Azure AD gösterimini bağlı RingCentral Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile RingCentral yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **RingCentral** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![Meta veri dosyasını yükleyin](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik olarak doldurulmuş **temel SAML yapılandırma** bölümü.

    ![RingCentral etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL yazın:
    
    | |
    |--|
    | `https://service.ringcentral.com`|
    | `https://service.ringcentral.com.au`|
    | `https://service.ringcentral.co.uk`|
    | `https://service.ringcentral.eu`|

    > [!Note]
    > Size **hizmet sağlayıcısı meta veri dosyası** RingCentral SSO yapılandırma sayfasında, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

5. Öğeniz yoksa **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL:

    | |
    |--|
    | `https://service.ringcentral.com` |
    | `https://service.ringcentral.com.au` |
    | `https://service.ringcentral.co.uk` |
    | `https://service.ringcentral.eu` |

    b. İçinde **tanımlayıcı** metin kutusuna bir URL:

    | |
    |--|
    |  `https://sso.ringcentral.com` |
    | `https://ssoeuro.ringcentral.com` |

    c. İçinde **yanıt URL'si** metin kutusuna bir URL:

    | |
    |--|
    | `https://sso.ringcentral.com/sp/ACS.saml2` |
    | `https://ssoeuro.ringcentral.com/sp/ACS.saml2` |

    ![RingCentral etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-ringcentral-single-sign-on"></a>RingCentral tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde RingCentral için bir güvenlik yöneticisi olarak oturum açın.

1. En üstte tıklayarak **Araçları**.

    ![image](./media/ringcentral-tutorial/ringcentral1.png)

1. Gidin **çoklu oturum açma**.

    ![image](./media/ringcentral-tutorial/ringcentral2.png)

1. Üzerinde **çoklu oturum açma** sayfasındaki **SSO yapılandırma** bölümü, gelen **1. adım** tıklayın **Düzenle** ve aşağıdaki adımları gerçekleştirin:

    ![image](./media/ringcentral-tutorial/ringcentral3.png)

1. Üzerinde **tek oturum açma kurulumunu** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![image](./media/ringcentral-tutorial/ringcentral4.png)

    a. Tıklayın **Gözat** Azure portalından indirdiğiniz meta veri dosyasını karşıya yüklemek için.

    b. Meta veriler karşıya yüklendikten sonra otomatik olarak doldurulmuş değerlerini alma **SSO genel bilgileri** bölümü.

    c. Altında **eşleme özniteliği** bölümünden **harita e-posta özniteliği** olarak `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **Kaydet**’e tıklayın.

    e. Gelen **2. adım** tıklayın **indirme** indirmek için **hizmet sağlayıcısı meta veri dosyası** ve bunu karşıya **temel SAML yapılandırma** bölümü otomatik olarak doldurmak için **tanımlayıcı** ve **yanıt URL'si** Azure portalında değerleri.

    ![image](./media/ringcentral-tutorial/ringcentral6.png) 

    f. Aynı sayfaya gidin **SSO etkinleştirme** bölümünde ve aşağıdaki adımları gerçekleştirin:

    ![image](./media/ringcentral-tutorial/ringcentral5.png)

    * Seçin **etkinleştirme SSO hizmet**.

    * Seçin **SSO veya RingCentral kimlik bilgilerinizle oturum açmasına imkan tanıyın**.

    * **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için RingCentral erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **RingCentral**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **RingCentral**.

    ![Uygulamalar listesinde RingCentral bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-ringcentral-test-user"></a>RingCentral test kullanıcısı oluşturma

Bu bölümde, Britta Simon içinde RingCentral adlı bir kullanıcı oluşturun. Çalışmak [RingCentral istemci Destek ekibine](https://success.ringcentral.com/RCContactSupp) RingCentral platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli RingCentral kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama RingCentral için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
