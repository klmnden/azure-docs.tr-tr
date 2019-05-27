---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile RingCentral | Microsoft Docs'
description: Azure Active Directory ve RingCentral arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 5848c875-5185-4f91-8279-1a030e67c510
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/09/2019
ms.author: jeedes
ms.openlocfilehash: 2e4f45deb8c63640a328e38cebc2ecbe67233c3b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66143132"
---
# <a name="tutorial-integrate-ringcentral-with-azure-active-directory"></a>Öğretici: RingCentral Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile RingCentral tümleştirme öğreneceksiniz. RingCentral Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* RingCentral erişimi, Azure AD'de denetler.
* Otomatik olarak RingCentral için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* RingCentral çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. RingCentral destekler **IDP** tarafından başlatılan

## <a name="adding-ringcentral-from-the-gallery"></a>Galeriden RingCentral ekleme

Azure AD'de RingCentral tümleştirmesini yapılandırmak için RingCentral Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **RingCentral** arama kutusuna.
1. Seçin **RingCentral** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO ile RingCentral adlı bir test kullanıcısı kullanarak test etme **Britta Simon**. Çalışmak SSO için RingCentral içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile RingCentral sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[RingCentral SSO'yu yapılandırarak](#configure-ringcentral-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[RingCentral test kullanıcısı oluşturma](#create-ringcentral-test-user)**  - kullanıcı Azure AD gösterimini bağlı RingCentral Britta simon'un bir karşılığı vardır.
6. **[Test SSO](#test-sso)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **RingCentral** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    1. Tıklayın **meta veri dosyasını karşıya yükleme**.
    1. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.
    1. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik olarak doldurulmuş **temel SAML yapılandırma** bölümü.

    > [!Note]
    > Size **hizmet sağlayıcısı meta veri dosyası** RingCentral SSO yapılandırma sayfasında, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

1. Öğeniz yoksa **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki alanlar için değerleri girin:

    a. İçinde **tanımlayıcı** metin kutusuna bir URL:

    | |
    |--|
    |  `https://sso.ringcentral.com` |
    | `https://ssoeuro.ringcentral.com` |

    b. İçinde **yanıt URL'si** metin kutusuna bir URL:

    | |
    |--|
    | `https://sso.ringcentral.com/sp/ACS.saml2` |
    | `https://ssoeuro.ringcentral.com/sp/ACS.saml2` |

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-ringcentral-sso"></a>RingCentral SSO yapılandırma

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

Bu bölümde, bir test kullanıcısı Britta Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `BrittaSimon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için RingCentral erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **RingCentral**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-ringcentral-test-user"></a>RingCentral test kullanıcısı oluşturma

Bu bölümde, Britta Simon içinde RingCentral adlı bir kullanıcı oluşturun. Çalışmak [RingCentral istemci Destek ekibine](https://success.ringcentral.com/RCContactSupp) RingCentral platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde RingCentral kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama RingCentral için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
