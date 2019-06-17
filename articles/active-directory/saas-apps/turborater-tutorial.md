---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile TurboRater | Microsoft Docs'
description: Azure Active Directory ve TurboRater arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: abb116b8-8024-4cc6-bc81-f32ef490ea17
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 3/8/2019
ms.author: jeedes
ms.openlocfilehash: 3777cf09ec669fe3df6bca13f6960f53c689767c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67088283"
---
# <a name="tutorial-azure-active-directory-integration-with-turborater"></a>Öğretici: TurboRater ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile TurboRater tümleştirme konusunda bilgi edinin.

Azure AD ile TurboRater tümleştirme ile aşağıdaki avantajları sağlar:

* TurboRater erişimi, Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınız için TurboRater (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla ayrıntı için bkz: [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile TurboRater yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
* Çoklu oturum etkin açma TurboRater abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

TurboRater IDP tarafından başlatılan çoklu oturum açma (SSO) destekler.

## <a name="add-turborater-from-the-azure-marketplace"></a>Azure Market'ten TurboRater Ekle

Azure AD'ye TurboRater tümleştirmesini yapılandırmak için TurboRater Azure Market'te yönetilen SaaS uygulamaları listesine eklemeniz gerekir:

1. [Azure Portal](https://portal.azure.com?azure-portal=true) oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.

    ![Azure Active Directory seçeneği](common/select-azuread.png)

1. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar seçeneği](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için seçin **+ yeni uygulama** bölmenin üstünde.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **TurboRater**. Arama sonuçlarında seçin **TurboRater**ve ardından **Ekle** uygulama eklemek için.

    ![Sonuç listesinde TurboRater](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma TurboRater adlı bir test kullanıcı tabanlı test **B Simon**. Tek iş için oturum açma için TurboRater içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma TurboRater ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
1. **[TurboRater çoklu oturum açmayı yapılandırma](#configure-turborater-single-sign-on)**  üzerinde uygulama tarafından çoklu oturum açma ayarları yapılandırmak için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma b Simon ile test etmek için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açmayı kullanmak b Simon etkinleştirmek için.
1. **[TurboRater test kullanıcısı oluşturma](#create-a-turborater-test-user)**  kullanan Azure AD kullanıcı için bağlantılı adlandırılmış b Simon TurboRater, böylece bir kullanıcı b Simon adlı.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile TurboRater yapılandırmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **TurboRater** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma seçeneği yapılandırın](common/select-sso.png)

1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** bölmesinde seçin **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

1. Üzerinde **yukarı çoklu oturum açma SAML ile Ayarla** sayfasında **Düzenle** (açmak için kalem simgesi) **temel SAML yapılandırma** bölmesi.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. İçinde **temel SAML yapılandırma** bölmesinde, aşağıdaki adımları uygulayın:

    ![Oturum açma bilgileri çoklu TurboRater etki alanı ve URL'ler](common/idp-intiated.png)

    1. İçinde **tanımlayıcı (varlık kimliği)** kutusunda, bir URL girin:

       `https://www.itcdataservices.com`

    1. İçinde **yanıt URL'si (onay belgesi tüketici hizmeti URL'si)** kutusunda, URL şu biçimi kullanarak girin:

       | Ortam | URL'si |
       | ---------------| --------------- |
       | Test etme  | `https://ratingqa.itcdataservices.com/webservices/imp/saml/login` |
       | Canlı  | `https://www.itcratingservices.com/webservices/imp/saml/login` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısıyla güncelleştirin ve yanıt URL'si. Bu değerleri almak için iletişime geçin [TurboRater Destek ekibine](https://www.getitc.com/support). Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölmesinde Azure portalında.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** bölmesinde, **SAML imzalama sertifikası** bölümünden **indirme** indirmek için **Federasyon meta veri XML**  seçeneklerle ve bilgisayarınıza kaydedin.

    ![Federasyon meta verileri XML yükleme seçeneği](common/metadataxml.png)

1. İçinde **TurboRater kümesi** bölümünde, URL veya gereken URL'leri kopyalayın:

   * **Oturum açma URL'si**
   * **Azure AD tanımlayıcısı**
   * **Oturum kapatma URL'si**

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-turborater-single-sign-on"></a>TurboRater çoklu oturum açmayı yapılandırın

Çoklu oturum açma TurboRater tarafında yapılandırmak için indirilen Federasyon meta verileri XML göndermek gereken ve Azure portalında URL'leri uygun kopyalanır [TurboRater Destek ekibine](https://www.getitc.com/support). TurboRater takım SAML SSO bağlantının her iki kenarı da düzgün ayarlandığından emin olun.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**   > **kullanıcılar** > **tüm kullanıcılar**.

    ![Kullanıcılar ve "Tüm kullanıcılar" seçenekleri](common/users.png)

1. Ekranın üst kısmında seçin **+ yeni kullanıcı**.

    ![Yeni kullanıcı seçeneği](common/new-user.png)

1. İçinde **kullanıcı** bölmesinde, aşağıdaki adımları uygulayın:

    ![Kullanıcı bölmesi](common/user-properties.png)

    1. İçinde **adı** kutusuna **BSimon**.
  
    1. İçinde **kullanıcı adı** kutusuna **BSimon\@\<yourcompanydomain >.\< Uzantı >** . Örneğin, **BSimon\@contoso.com**.

    1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma için TurboRater erişimleri vererek kullanmak b Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar** > **tüm uygulamaları** > **TurboRater**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **TurboRater**.

    ![Uygulamalar listesinde TurboRater](common/all-applications.png)

1. Sol bölmede altında **Yönet**seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" seçeneği](common/users-groups-blade.png)

1. Seçin **+ Ekle kullanıcı**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** bölmesi.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** bölmesinde **b Simon** içinde **kullanıcılar** listeleyin ve ardından **seçin** bölmesinin alt kısmındaki.

1. SAML onaylaması rol değerinde ardından içinde beklediğiniz varsa **rolü Seç** bölmesinde, listeden bir kullanıcı için uygun rolü seçin. Bölmesinin en altında seçin **seçin**.

1. İçinde **atama Ekle** bölmesinde **atama**.

### <a name="create-a-turborater-test-user"></a>TurboRater test kullanıcısı oluşturma

Bu bölümde, B. Simon TurboRater içinde adlı bir kullanıcı oluşturun. Çalışmak [TurboRater Destek ekibine](https://www.getitc.com/support) b Simon TurboRater kullanıcı olarak eklenecek. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, uygulamalarım portalını kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **TurboRater** uygulamalarım portalında, otomatik olarak kendisi için ayarladığınız çoklu oturum açmayı TurboRater aboneliğe oturum. Uygulamalarım portal hakkında daha fazla bilgi için bkz. [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
