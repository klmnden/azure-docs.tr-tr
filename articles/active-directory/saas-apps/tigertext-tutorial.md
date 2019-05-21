---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle TigerText güvenli Messenger | Microsoft Docs'
description: Azure Active Directory ve TigerText güvenli Messenger arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: 840b1fe55556cfd853e0928164891d6b21b17cc2
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956873"
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>Öğretici: TigerText güvenli Messenger ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile TigerText güvenli Messenger tümleştirme konusunda bilgi edinin.

Azure AD ile TigerText güvenli Messenger tümleştirme ile aşağıdaki avantajları sağlar:

* TigerText Messenger güvenli erişimi, Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın TigerText güvenli Messenger (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla ayrıntı için bkz: [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi TigerText güvenli Messenger ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
* Çoklu oturum etkin açma TigerText güvenli Messenger abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin ve TigerText güvenli Messenger Azure AD ile tümleştirme yapılandırın.

TigerText güvenli Messenger SP tarafından başlatılan çoklu oturum açma (SSO) destekler.

## <a name="add-tigertext-secure-messenger-from-the-azure-marketplace"></a>Azure Market'ten TigerText güvenli Messenger Ekle

Azure AD'de TigerText güvenli Messenger'ın tümleştirmesini yapılandırmak için Azure Marketi'nden yönetilen SaaS uygulamaları listenize TigerText güvenli Messenger eklemeniz gerekir:

1. [Azure Portal](https://portal.azure.com?azure-portal=true) oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.

    ![Azure Active Directory seçeneği](common/select-azuread.png)

1. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için seçin **+ yeni uygulama** bölmenin üstünde.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **TigerText güvenli Messenger**. Arama sonuçlarında seçin **TigerText güvenli Messenger**ve ardından **Ekle** uygulama eklemek için.

    ![Sonuç listesinde TigerText güvenli Messenger](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma TigerText güvenli Messenger adlı bir test kullanıcı tabanlı test **Britta Simon**. Tek iş için oturum açma için güvenli TigerText Messenger'ın bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma TigerText güvenli Messenger ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
1. **[TigerText Messenger güvenli çoklu oturum açmayı yapılandırma](#configure-tigertext-secure-messenger-single-sign-on)**  üzerinde uygulama tarafından çoklu oturum açma ayarları yapılandırmak için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
1. **[TigerText güvenli Messenger test kullanıcısı oluşturma](#create-a-tigertext-secure-messenger-test-user)**  kullanıcı yani kullanan Azure AD kullanıcı için bağlantılı adlandırılmış Britta Simon TigerText güvenli Messenger adlı Britta Simon.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma TigerText güvenli Messenger ile yapılandırmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **TigerText güvenli Messenger** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma seçeneği yapılandırın](common/select-sso.png)

1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** bölmesinde seçin **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

1. Üzerinde **yukarı çoklu oturum açma SAML ile Ayarla** bölmesinde **Düzenle** (açmak için kalem simgesi) **temel SAML yapılandırma** bölmesi.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. İçinde **temel SAML yapılandırma** bölmesinde, aşağıdaki adımları uygulayın:

    ![Oturum açma bilgileri tek bir güvenli TigerText Messenger etki alanı ve URL'ler](common/sp-identifier.png)

    1. İçinde **oturum açma URL'si** kutusunda, bir URL girin:

       `https://home.tigertext.com`

    1. İçinde **tanımlayıcı (varlık kimliği)** kutusuna şu biçimi kullanarak bir URL yazın:

       `https://saml-lb.tigertext.me/v1/organization/<instance ID>`

    > [!NOTE]
    > **Tanımlayıcı (varlık kimliği)** gerçek değeri değil. Bu değer, gerçek tanımlayıcısıyla güncelleştirin. Değer almak için iletişime geçin [TigerText güvenli Messenger Destek ekibine](mailto:prosupport@tigertext.com). Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölmesinde Azure portalında.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** bölmesinde, **SAML imzalama sertifikası** bölümünden **indirme** indirmek için **Federasyon meta veri XML**  seçeneklerle ve bilgisayarınıza kaydedin.

    ![Federasyon meta verileri XML yükleme seçeneği](common/metadataxml.png)

1. İçinde **TigerText güvenli Messenger kümesi** bölümünde, URL veya gereken URL'leri kopyalayın:

   * **Oturum açma URL'si**
   * **Azure AD tanımlayıcısı**
   * **Oturum kapatma URL'si**

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-tigertext-secure-messenger-single-sign-on"></a>TigerText Messenger güvenli çoklu oturum açmayı yapılandırın

Çoklu oturum açma TigerText güvenli Messenger tarafında yapılandırmak için indirilen Federasyon meta verileri XML göndermek gereken ve Azure portalında URL'leri uygun kopyalanmış [TigerText güvenli Messenger Destek ekibine](mailto:prosupport@tigertext.com). TigerText güvenli Messenger takım SAML SSO bağlantının her iki kenarı da düzgün ayarlandığından emin olun.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**   > **kullanıcılar** > **tüm kullanıcılar**.

    ![Kullanıcılar ve "Tüm kullanıcılar" seçenekleri](common/users.png)

1. Ekranın üst kısmında seçin **+ yeni kullanıcı**.

    ![Yeni kullanıcı seçeneği](common/new-user.png)

1. İçinde **kullanıcı** bölmesinde, aşağıdaki adımları uygulayın:

    ![Kullanıcı bölmesi](common/user-properties.png)

    1. İçinde **adı** kutusuna **BrittaSimon**.
  
    1. İçinde **kullanıcı adı** kutusuna **BrittaSimon\@\<yourcompanydomain >.\< Uzantı >**. Örneğin, **BrittaSimon\@contoso.com**.

    1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma TigerText Messenger'a güvenli erişimi vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar** > **tüm uygulamaları** > **TigerText güvenli Messenger**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **TigerText güvenli Messenger**.

    ![Uygulamalar listesinde TigerText güvenli Messenger](common/all-applications.png)

1. Sol bölmede altında **Yönet**seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" seçeneği](common/users-groups-blade.png)

1. Seçin **+ Ekle kullanıcı**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** bölmesi.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** bölmesinde **Britta Simon** içinde **kullanıcılar** listeleyin ve ardından **seçin** bölmesinin alt kısmındaki.

1. SAML onaylaması rol değerinde ardından içinde beklediğiniz varsa **rolü Seç** bölmesinde, listeden bir kullanıcı için uygun rolü seçin. Bölmesinin en altında seçin **seçin**.

1. İçinde **atama Ekle** bölmesinde **atama**.

### <a name="create-a-tigertext-secure-messenger-test-user"></a>TigerText güvenli Messenger test kullanıcısı oluşturma

Bu bölümde, Britta Simon TigerText güvenli Messenger'da adlı bir kullanıcı oluşturun. Çalışmak [TigerText güvenli Messenger Destek ekibine](mailto:prosupport@tigertext.com) Britta Simon TigerText Messenger güvenli bir kullanıcı olarak eklenecek. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, uygulamalarım portalını kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **TigerText güvenli Messenger** uygulamalarım portalında, otomatik olarak kendisi için ayarladığınız çoklu oturum açmayı TigerText güvenli Messenger aboneliğe oturum. Uygulamalarım portal hakkında daha fazla bilgi için bkz. [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

* [İçin Azure Active Directory ile SaaS uygulamalarını tümleştirme konusundaki öğreticilerin listesine](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
