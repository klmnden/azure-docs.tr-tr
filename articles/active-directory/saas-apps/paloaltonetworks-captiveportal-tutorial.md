---
title: 'Öğretici: Palo Alto ağları Captive portalı ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Palo Alto ağları Captive portalı ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 67a0b476-2305-4157-8658-2ec3625850d5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/25/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a62aa573d49ccdd237e692b63a6fda0dd83d52a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65869948"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks-captive-portal"></a>Öğretici: Palo Alto ağları Captive portalı ile Azure Active Directory Tümleştirme

Bu öğreticide, Palo Alto ağları Captive portalı Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Palo Alto ağları Captive portalı Azure AD ile tümleştirdiğinizde aşağıdaki avantajlardan yararlanabilirsiniz:

* Azure AD'de Palo Alto ağları Captive portalına kimlerin erişebildiğini denetleyebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Palo Alto ağları Captive Portalı'nda kullanıcıların Azure AD kullanıcı hesaplarını kullanarak oturum açabilirsiniz.
* Hesaplarınızı tek, merkezi bir konumda, Azure portalında yönetebilir.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi edinmek için [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Palo Alto ağları Captive portalı ile Azure AD tümleştirmek için aşağıdakiler gerekir:

* Bir Azure Active Directory aboneliği. Azure AD yoksa alabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Bir Palo Alto ağları Captive portalı çoklu oturum açma (SSO)-abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

Palo Alto ağları Captive Portal bu senaryoları destekler:

* **IDP tarafından başlatılan çoklu oturum açma**
* **Just-ın-time kullanıcı sağlama**

## <a name="add-palo-alto-networks-captive-portal-from-the-gallery"></a>Palo Alto ağları Captive portalı Galeriden Ekle

Galeride başlamak için Palo Alto ağları Captive portalı yönetilen SaaS uygulamaları listenize ekleyin:

1. İçinde [Azure portalında](https://portal.azure.com), soldaki menüde **Azure Active Directory**.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Menüde kurumsal uygulamalar seçeneği](common/enterprise-applications.png)

3. **Yeni uygulama**’yı seçin.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Palo Alto ağları Captive portalı**. Arama sonuçlarında seçin **Palo Alto Networks - Captive portalı**ve ardından **Ekle**.

     ![Palo Alto Networks tarafından sağlanan-sonuç listesinde Captive portalı](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Palo Alto ağları Captive adlı bir test kullanıcı tabanlı Portal ile Azure AD çoklu oturum açmayı test *Britta Simon*. Tek iş için oturum açma için Palo Alto ağları Captive portalında bir Azure AD kullanıcısı ve aynı kullanıcı arasında ilişki oluşturmanız gerekir. 

Yapılandırma ve Azure AD çoklu oturum açma Palo Alto ağları Captive portalı ile test etmek için aşağıdaki görevleri tamamlayın:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**: Bu özelliği kullanmak kullanıcının etkinleştirir.
2. **[Palo Alto ağları Captive portalı çoklu oturum açmayı yapılandırma](#configure-palo-alto-networks-captive-portal-single-sign-on)**: Uygulamada çoklu oturum açma ayarları yapılandırın.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**: Azure AD çoklu oturum açma kullanıcı ile test *Britta Simon*.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**: Britta Simon, Azure AD çoklu oturum açma kullanacak şekilde ayarlanmış.
5. **Palo Alto ağları Captive portalı test kullanıcısı oluşturma**: Karşılık gelen kullanıcı oluşturma *Britta Simon* portalında Palo Alto ağları Captive için Azure AD kullanıcı bağlı.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**: Yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

İlk olarak, Azure AD çoklu oturum açma Azure portalında etkinleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Palo Alto Networks - Captive portalı** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** bölmesinde seçin **SAML**.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. İçinde **yukarı çoklu oturum açma SAML ile ayarlanmış** bölmesinde kalem seçin **Düzenle** simgesi.

    ![Penciled düzenleme simgesi](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** bölmesinde, aşağıdaki adımları tamamlayın:

    ![Palo Alto ağları Captive portalı temel SAML yapılandırma bölmesi](common/idp-intiated.png)

   1. İçin **tanımlayıcı**, deseni bir URL girin `https://<customer_firewall_host_name>/SAML20/SP`.

   2. İçin **yanıt URL'si**, deseni bir URL girin `https://<customer_firewall_host_name>/SAML20/SP/ACS`.

      > [!NOTE]
      > Bu adımda yer tutucu değerlerini gerçek tanımlayıcısıyla güncelleştirin ve yanıt URL'leri. Gerçek değerlere ulaşmak için ilgili kişi [Palo Alto ağları Captive portalı istemcisi Destek ekibine](https://support.paloaltonetworks.com/support).

5. İçinde **SAML imzalama sertifikası** yanındaki bölümünde **Federasyon meta verileri XML**seçin **indirme**. İndirilen dosyayı bilgisayarınıza kaydedin.

    ![Federasyon meta verileri XML indirme bağlantısı](common/metadataxml.png)

### <a name="configure-palo-alto-networks-captive-portal-single-sign-on"></a>Palo Alto ağları Captive portalı çoklu oturum açmayı yapılandırın

Ardından, çoklu oturum Palo Alto ağları Captive portalında ayarlayın:

1. Bir farklı bir tarayıcı penceresinde Palo Alto Networks tarafından sağlanan Web sitesi için bir yönetici olarak oturum açın.

2. Seçin **cihaz** sekmesi.

    ![Palo Alto Networks tarafından sağlanan Web sitesi cihaz sekmesi](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin1.png)

3. Menüde **SAML kimlik sağlayıcısı**ve ardından **alma**.

    ![İçeri Aktar düğmesi](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin2.png)

4. İçinde **SAML kimlik sağlayıcısı sunucusu Profil alma** iletişim kutusunda, aşağıdaki adımları tamamlayın:

    ![Palo Alto Networks çoklu oturum açmayı yapılandırın](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin3.png)

    1. İçin **profil adı**, gibi bir ad girin **AzureAD CaptivePortal**.
    
    2. Yanındaki **kimlik sağlayıcısı meta verileri**seçin **Gözat**. Azure portalında indirdiğiniz metadata.xml dosyası seçin.
    
    3. **Tamam**’ı seçin.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Ardından, adlı bir test kullanıcısı oluşturma *Britta Simon* Azure portalında:

1. Azure portalında **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı**.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. İçinde **kullanıcı** bölmesinde, aşağıdaki adımları tamamlayın:

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. İçin **adı**, girin **BrittaSimon**.
  
    2. İçin **kullanıcı adı**, girin **BrittaSimon\@\<your_company_domain\>**. Örneğin, **BrittaSimon\@contoso.com**.

    3. İçin **parola**, bir parola girin. Girdiğiniz parola kaydı tutmanızı öneririz. Seçebileceğiniz **Göster parola** parolayı görüntülemek için onay kutusunu.

    4. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Sonra Britta Simon Azure çoklu oturum açma kullanabilmeniz için Palo Alto ağları Captive portalına erişimi verin:

1. Azure portalında **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

2. Uygulamalar listesinde girin **Palo Alto Networks - Captive portalı**ve ardından uygulamayı seçin.

    ![Palo Alto ağları - uygulamalar listesinde Captive Portal bağlantısı](common/all-applications.png)

3. Menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**. Ardından **ataması ekleme** bölmesinde **kullanıcılar ve gruplar**.

    ![Ekle atama bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** bölmesinde, **kullanıcılar** listesinden **Britta Simon**. Seçin **seçin**.

6. SAML onaylaması için bir rol değer eklemek için **rol seçme** bölmesinde, kullanıcı için uygun rolü seçin. Seçin **seçin**.

7. İçinde **ataması ekleme** bölmesinde **atama**.

### <a name="create-a-palo-alto-networks-captive-portal-test-user"></a>Palo Alto ağları Captive portalı test kullanıcısı oluşturma

Ardından, adlı bir kullanıcı oluşturun *Britta Simon* Palo Alto ağları Captive portalında. Palo Alto ağları Captive portalı just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümdeki tüm görevleri tamamlamanız gerekmez. Palo Alto ağları Captive Portalı'nda bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

> [!NOTE]
> Bir kullanıcı el ile oluşturmak istiyorsanız, ilgili kişi [Palo Alto ağları Captive portalı istemcisi Destek ekibine](https://support.paloaltonetworks.com/support).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Palo Alto ağları Captive portalı, bir Windows VM Güvenlik duvarının arkasında yüklenir. Palo Alto ağları Captive portalında çoklu oturum açmayı test etmek için Windows VM'ye Uzak Masaüstü Protokolü (RDP) kullanarak oturum açın. RDP oturumu bir tarayıcı açın ve bir Web sitesine gidin. SSO URL'sini açar ve kimlik doğrulaması istenir. Kimlik doğrulaması tamamlandığında, Web siteleri erişebilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için şu makalelere bakın:

- [Azure Active Directory ile SaaS uygulamalarını tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory'de koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

