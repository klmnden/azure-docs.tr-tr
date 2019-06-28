---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Ungerboeck yazılım | Microsoft Docs'
description: Azure Active Directory ve Ungerboeck yazılım arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bd861069-8a15-4d29-aa26-0d7be3a0a94a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 69c7a7a8fe0ef346cb26e3b0b4f14d3dae7211b9
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67370309"
---
# <a name="tutorial-integrate-ungerboeck-software-with-azure-active-directory"></a>Öğretici: Ungerboeck yazılım Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Ungerboeck yazılım tümleştirme öğreneceksiniz. Ungerboeck yazılım Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Ungerboeck yazılım erişimi, Azure AD'de denetler.
* Otomatik olarak Ungerboeck yazılım için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* Ungerboeck yazılım çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Destekleyen Ungerboeck yazılımı **SP** SSO başlattı.

## <a name="adding-ungerboeck-software-from-the-gallery"></a>Galeriden Ungerboeck yazılım ekleme

Azure AD'de Ungerboeck yazılım tümleştirmesini yapılandırmak için Ungerboeck yazılım Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Ungerboeck yazılım** arama kutusuna.
1. Seçin **Ungerboeck yazılım** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO ile Ungerboeck adlı bir test kullanıcı kullanarak yazılım test etme **B.Simon**. Çalışmak SSO için bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki Ungerboeck yazılımda oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile Ungerboeck yazılım test etme hakkında bilgi için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Ungerboeck yazılım SSO'yu yapılandırarak](#configure-ungerboeck-software-sso)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma B.Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  B.Simon Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[Ungerboeck yazılım test kullanıcısı oluşturma](#create-ungerboeck-software-test-user)**  B.Simon bir karşılığı kullanıcı Azure AD gösterimini bağlı Ungerboeck yazılım sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Ungerboeck yazılım** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, aşağıdaki alanlar için değerleri girin:

    1. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.ungerboeck.com/prod`

    1. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın:
    
       **Üretim ortamı için**:

       | |
       |-|
       | `https://<SUBDOMAIN>.ungerboeck.com/prod` |
       | `https://<SUBDOMAIN>.ungerboeck.net/prod` |
       | `https://<SUBDOMAIN>.ungerboeck.io/prod` |
       | |

        **Test ortamı için**:

       | |
       |-|
       | `https://<SUBDOMAIN>.ungerboeck.com/test` |
       | `https://<SUBDOMAIN>.ungerboeck.net/test` |
       | `https://<SUBDOMAIN>.ungerboeck.io/test` |
       | |

   > [!NOTE]
   > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve daha sonra açıklanan tanımlayıcısı ile güncelleştirme **yapılandırma Ungerboeck yazılım çoklu oturum açma** öğreticinin bölümünde.

1. İçinde **SAML imzalama sertifikası** bölümünde **Düzenle** açmak için düğmeyi **SAML imzalama sertifikası** iletişim.

    ![SAML imzalama sertifikası Düzenle](common/edit-certificate.png)

1. İçinde **SAML imzalama sertifikası** bölümünde, kopya **parmak izi** ve bilgisayarınıza kaydedin.

    ![Parmak izi değerini kopyalayın](common/copy-thumbprint.png)

1. Üzerinde **Ungerboeck yazılımını kurma** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-ungerboeck-software-sso"></a>Ungerboeck yazılım SSO yapılandırma

Çoklu oturum açmayı yapılandırma **Ungerboeck yazılım** tarafını göndermek için ihtiyacınız **parmak izi değerini** ve uygun Azure portalına kopyalanan URL'lerden [Ungerboeck yazılım Destek ekibine](mailto:Rhonda.Jannings@ungerboeck.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı B.Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `B.Simon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Ungerboeck yazılıma erişim vererek, Azure çoklu oturum açma kullanılacak B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Ungerboeck yazılım**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-ungerboeck-software-test-user"></a>Ungerboeck yazılım test kullanıcısı oluşturma

Bu bölümde, B.Simon Ungerboeck yazılım adlı bir kullanıcı oluşturun. Çalışmak [Ungerboeck yazılım Destek ekibine](mailto:Rhonda.Jannings@ungerboeck.com) Ungerboeck yazılım platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Ungerboeck yazılım kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Ungerboeck yazılım için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)