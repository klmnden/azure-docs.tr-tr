---
title: 'Öğretici: Yardımcısı yardımcı olan Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Çoklu oturum açma Yardımcısı Yardımcısı ile Azure Active Directory arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 5f42e4d7-4d92-4096-a0d5-02fa438a5dfd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/31/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 19969588001bd48bd1c998fe49061db80d94c636
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66518525"
---
# <a name="tutorial-integrate-helper-helper-with-azure-active-directory"></a>Öğretici: Yardımcısı Yardımcısı Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Yardımcısı yardımcı tümleştirme öğreneceksiniz. Yardımcısı Yardımcısı, Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Yardımcısı Yardımcısı erişimi, Azure AD'de denetler.
* Otomatik olarak Yardımcısı Yardımcısı için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Yardımcısı Yardımcısı çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Yardımcısı Yardımcısı destekler **SP ve IDP** SSO ve destekler başlatılan **zamanında** kullanıcı sağlama.

## <a name="adding-helper-helper-from-the-gallery"></a>Galeriden Yardımcısı Yardımcısı ekleme

Azure AD'ye yardımcı Yardımcısı tümleştirmesini yapılandırmak için yardımcı Yardımcısı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Yardımcısı Yardımcısı** arama kutusuna.
1. Seçin **Yardımcısı Yardımcısı** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırmanıza ve sınamanıza yardımcı olarak adlandırılan bir test kullanıcısı kullanarak yardımcı olan Azure AD SSO **b Simon**. Çalışmak SSO için yardımcı yardımcı bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırmanıza ve sınamanıza yardımcı yardımcı olan Azure AD SSO için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Yardımcısı Yardımcısı yapılandırma](#configure-helper-helper)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma b Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açmayı kullanmak b Simon etkinleştirmek için.
5. **[Yardımcısı Yardımcısı test kullanıcısı oluşturma](#create-helper-helper-test-user)**  b Simon bir karşılığı kullanıcı Azure AD gösterimini bağlı Yardımcısı yardımcı olması için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Yardımcısı Yardımcısı** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası** yapılandırmak isterseniz **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    >[!NOTE]
    >URL'ye Git `https://sso.helperhelper.com/saml/<customer_id>` hizmet sağlayıcısı meta veri dosyası alınamıyor. İlgili kişi [Yardımcısı Yardımcısı istemci Destek ekibine](mailto:info@helperhelper.com) için `<customer_id>`.

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik temel SAML yapılandırma bölümünde doldurulur.

    > [!Note]
    > Varsa **tanımlayıcı** ve **yanıt URL'si** değerlerin değil otomatik polulated alın ve ardından değerleri ihtiyacınıza göre el ile girin.

1. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://sso.helperhelper.com/saml/<customer_id>/login`

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Yardımcısı Yardımcısı istemci Destek ekibine](mailto:info@helperhelper.com) bu değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** Azure portal.l bölümünde.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde not defteri kaydedin .

   ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

1. Üzerinde **ayarlama Yardımcısı yardımcı** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-helper-helper"></a>Yardımcısı Yardımcısı'nı yapılandırma

Çoklu oturum açmayı yapılandırma **Yardımcısı Yardımcısı** tarafını göndermek için ihtiyacınız **uygulama Federasyon meta verileri URL'sini** için [Yardımcısı Yardımcısı Destek ekibine](mailto:info@helperhelper.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı b Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B. Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `BrittaSimon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Yardımcısı yardımcıya erişim vererek kullanmak b Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Yardımcısı Yardımcısı**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **b Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-helper-helper-test-user"></a>Yardımcısı Yardımcısı test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Yardımcısı yardımcı oluşturulur. Yardımcısı Yardımcısı just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yardımcısı yardımcı bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Yardımcısı Yardımcısı kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Yardımcısı Yardımcısı için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)