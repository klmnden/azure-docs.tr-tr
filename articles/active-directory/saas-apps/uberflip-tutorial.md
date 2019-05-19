---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Uberflip | Microsoft Docs'
description: Azure Active Directory ve Uberflip arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 754b1f5b-6694-4fd6-9e1e-9fad769c64db
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 69e86e486a9cdb058b972bda5176c14e15f4630a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65865677"
---
# <a name="tutorial-azure-active-directory-integration-with-uberflip"></a>Öğretici: Uberflip ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Uberflip tümleştirme konusunda bilgi edinin.

Azure AD ile Uberflip tümleştirme ile aşağıdaki avantajları sağlar:

* Uberflip erişimi, Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınız için Uberflip (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açmanız etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla ayrıntı için bkz: [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Uberflip yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
* Çoklu oturum etkin açma Uberflip abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

Uberflip aşağıdaki özellikleri destekler:

* SP tarafından başlatılan ve IDP tarafından başlatılan çoklu oturum açma (SSO).
* Just-ın-time kullanıcı sağlama.

## <a name="add-uberflip-from-the-azure-marketplace"></a>Azure Market'ten Uberflip Ekle

Azure AD'ye Uberflip tümleştirmesini yapılandırmak için Uberflip Azure Market'te yönetilen SaaS uygulamaları listesine eklemeniz gerekir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol bölmede **Azure Active Directory**’yi seçin.

   ![Azure Active Directory seçeneği](common/select-azuread.png)

1. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

   ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için seçin **+ yeni uygulama** bölmenin üstünde.

   ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **Uberflip**. Arama sonuçlarında seçin **Uberflip**ve ardından **Ekle** uygulama eklemek için.

   ![Sonuç listesinde Uberflip](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Uberflip adlı bir test kullanıcı tabanlı test **Britta Simon**. Tek iş için oturum açma için Uberflip içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Uberflip ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
1. **[Uberflip çoklu oturum açmayı yapılandırma](#configure-uberflip-single-sign-on)**  üzerinde uygulama tarafından çoklu oturum açma ayarları yapılandırmak için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
1. **[Bir Uberflip test kullanıcısı oluşturma](#create-an-uberflip-test-user)**  kullanan Azure AD kullanıcı için bağlantılı adlandırılmış Britta Simon Uberflip, böylece bir kullanıcı Britta Simon adlı.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Uberflip yapılandırmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **Uberflip** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma seçeneği yapılandırın](common/select-sso.png)

1. İçinde **tek bir oturum açma yönteminizi seçmeniz** bölmesinde seçin **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

1. Üzerinde **yukarı çoklu oturum açma SAML ile Ayarla** bölmesinde **Düzenle** (açmak için kalem simgesi) **temel SAML yapılandırma** bölmesi.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** bölmesinde, yapılandırmak istediğiniz hangi SSO modunuza bağlı olarak aşağıdakilerden birini yapın:

   * Uygulamayı IDP tarafından başlatılan SSO'yu modunda yapılandırmak için **yanıt URL'si (onay belgesi tüketici hizmeti URL'si)** kutusunda, URL şu biçimi kullanarak girin:

     `https://app.uberflip.com/sso/saml2/<IDPID>/<ACCOUNTID>`

     ![Oturum açma bilgileri çoklu Uberflip etki alanı ve URL'ler](common/both-replyurl.png)

     > [!NOTE]
     > Bu değer, gerçek değil. Bu değer, gerçek yanıt URL'si ile güncelleştirin. Gerçek değeri almak için iletişime geçin [Uberflip Destek ekibine](mailto:support@uberflip.com). Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölmesinde Azure portalında.

   * SP tarafından başlatılan SSO'yu modunda uygulamayı yapılandırmak için seçin **ek URL'lerini ayarlayın**hem de **oturum açma URL'si** kutusunda, bu URL'yi girin:

     `https://app.uberflip.com/users/login`

     ![Oturum açma bilgileri çoklu Uberflip etki alanı ve URL'ler](common/both-signonurl.png)

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** bölmesinde, **SAML imzalama sertifikası** bölümünden **indirme** indirmek için **Federasyon meta veri XML**  seçeneklerle ve bilgisayarınıza kaydedin.

   ![Federasyon meta verileri XML yükleme seçeneği](common/metadataxml.png)

1. İçinde **Uberflip kümesi** bölmesinde, URL veya gereken URL'leri kopyalayın:

   * **Oturum açma URL'si**
   * **Azure AD tanımlayıcısı**
   * **Oturum kapatma URL'si**

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-uberflip-single-sign-on"></a>Uberflip çoklu oturum açmayı yapılandırın

Çoklu oturum açma Uberflip tarafında yapılandırmak için indirilen Federasyon meta verileri XML göndermek gereken ve Azure portalında URL'leri uygun kopyalanır [Uberflip Destek ekibine](mailto:support@uberflip.com). Uberflip takım SAML SSO bağlantının her iki kenarı da düzgün ayarlandığından emin olun.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

1. Azure portalında, sol bölmede seçin **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Uberflip erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar** > **tüm uygulamaları** > **Uberflip**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **Uberflip**.

    ![Uygulamalar listesinde Uberflip](common/all-applications.png)

1. Sol bölmede altında **Yönet**seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" seçeneği](common/users-groups-blade.png)

1. Seçin **+ Ekle kullanıcı**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** bölmesi.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** bölmesinde **Britta Simon** içinde **kullanıcılar** listeleyin ve ardından **seçin** bölmesinin alt kısmındaki.

1. SAML onaylaması rol değerinde ardından içinde beklediğiniz varsa **rolü Seç** bölmesinde, listeden bir kullanıcı için uygun rolü seçin. Bölmesinin en altında seçin **seçin**.

1. İçinde **atama Ekle** bölmesinde **atama**.

### <a name="create-an-uberflip-test-user"></a>Bir Uberflip test kullanıcısı oluşturma

Britta Simon adlı bir kullanıcı Uberflip oluşturuldu. Bu kullanıcı oluşturmak için herhangi bir şey yapmanız gerekmez. Uberflip just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Britta Simon adlı bir kullanıcı Uberflip içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Uberflip Destek ekibine](mailto:support@uberflip.com).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, uygulamalarım portalını kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **Uberflip** uygulamalarım portalında, otomatik olarak kendisi için ayarladığınız çoklu oturum açmayı Uberflip aboneliğe oturum. Uygulamalarım portal hakkında daha fazla bilgi için bkz. [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)