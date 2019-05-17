---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile HubSpot | Microsoft Docs'
description: Azure Active Directory ve HubSpot arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 57343ccd-53ea-4e62-9e54-dee2a9562ed5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d74cc1665867568032bb1343e4f2c26c50fe15a
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65770182"
---
# <a name="tutorial-azure-active-directory-integration-with-hubspot"></a>Öğretici: HubSpot ile Azure Active Directory Tümleştirme

Bu öğreticide, HubSpot, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

HubSpot Azure AD ile tümleştirme, aşağıdaki avantajları sunar:

* Azure AD HubSpot erişimi denetlemek için kullanabilirsiniz.
* Kullanıcıları otomatik olarak HubSpot için kendi Azure AD hesapları (çoklu oturum açma) ile oturum açmanız.
* Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile HubSpot yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) başlamadan önce.
* Çoklu oturum etkin açma HubSpot abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin ve HubSpot Azure AD ile tümleştirme yapılandırın.

HubSpot, aşağıdaki özellikleri destekler:

* **SP tarafından başlatılan çoklu oturum açma**
* **IDP tarafından başlatılan çoklu oturum açma**

## <a name="add-hubspot-in-the-azure-portal"></a>Azure portalında HubSpot Ekle

HubSpot Azure AD ile tümleştirmek için HubSpot, yönetilen SaaS uygulamalar listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol menüde **Azure Active Directory**.

    ![Azure Active Directory seçeneği](common/select-azuread.png)

1. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Bir uygulama eklemek için seçin **yeni uygulama**.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **HubSpot**. Arama sonuçlarında seçin **HubSpot**ve ardından **Ekle**.

    ![Sonuç listesinde HubSpot](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma HubSpot adlı bir test kullanıcı tabanlı test **Britta Simon**. Tek iş için oturum açma için HubSpot bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bağlı bir ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma HubSpot ile test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

| Görev | Açıklama |
| --- | --- |
| **[Azure AD çoklu oturum açmayı yapılandırın](#configure-azure-ad-single-sign-on)** | Bu özelliği kullanmak olanak sağlar. |
| **[HubSpot çoklu oturum açmayı yapılandırın](#configure-hubspot-single-sign-on)** | Uygulamada çoklu oturum açma ayarları yapılandırır. |
| **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)** | Testleri Azure AD çoklu oturum açma kullanıcı Britta Simon adı. |
| **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)** | Azure AD çoklu oturum açmayı kullanmak Britta Simon sağlar. |
| **[HubSpot test kullanıcısı oluşturma](#create-a-hubspot-test-user)** | Kullanıcı Azure AD gösterimini bağlı HubSpot içinde bir karşılığı Britta simon'un oluşturur. |
| **[Çoklu oturum açma testi](#test-single-sign-on)** | Yapılandırma çalıştığını doğrular. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma ile HubSpot Azure portalında yapılandırın.

1. İçinde [Azure portalında](https://portal.azure.com/), **HubSpot** uygulama tümleştirme bölmesinde **çoklu oturum açma**.

    ![Çoklu oturum açma seçeneği yapılandırın](common/select-sso.png)

1. İçinde **tek bir oturum açma yönteminizi seçmeniz** bölmesinde seçin **SAML** veya **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

1. İçinde **yukarı çoklu oturum açma SAML ile ayarlayın** bölmesinde **Düzenle** (açmak için kalem simgesi) **temel SAML yapılandırma** bölmesi.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. İçinde **temel SAML yapılandırma** bölmesinde yapılandırmak için *IDP tarafından başlatılan modu*, aşağıdaki adımları tamamlayın:

    1. İçinde **tanımlayıcı** kutusunda, aşağıdaki desenin bir URL girin: https:\//api.hubspot.com/login-api/v1/saml/login?portalId=\<Müşteri Kimliği\>.

    1. İçinde **yanıt URL'si** kutusunda, aşağıdaki desenin bir URL girin: https:\//api.hubspot.com/login-api/v1/saml/acs?portalId=\<Müşteri Kimliği\>.

    ![Oturum açma bilgileri çoklu HubSpot etki alanı ve URL'ler](common/idp-intiated.png)

    > [!NOTE]
    > URL'leri biçimlendirmek için Ayrıca gösterilen desenleri başvurabilirsiniz **temel SAML yapılandırma** bölmesinde Azure portalında.

1. Uygulamayı yapılandırmak için *SP tarafından başlatılan* modu:

    1. Seçin **ek URL'lerini ayarlayın**.

    1. İçinde **oturum açma URL'si** kutusuna **https:\//app.hubspot.com/login**.

    ![Küme ek URL'ler seçeneği](common/metadata-upload-additional-signon.png)

1. İçinde **yukarı çoklu oturum açma SAML ile ayarlanmış** bölmesinde, **SAML imzalama sertifikası** bölümünden **indirme** yanındaki **sertifika (Base64)**. Gereksinimlerinize göre bir indirme seçeneğini seçin. Sertifika bilgisayarınıza kaydedin.

    ![Sertifika (Base64) yükleme seçeneği](common/certificatebase64.png)

1. İçinde **HubSpot kümesi** bölümünde, gereksinimlerinize göre aşağıdaki URL'ler kopyalayın:

    * Oturum Açma URL'si:
    * Azure AD Tanımlayıcısı
    * Oturum Kapatma URL'si

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-hubspot-single-sign-on"></a>HubSpot çoklu oturum açmayı yapılandırın

1. Tarayıcınızda yeni bir sekme açın ve HubSpot yönetici hesabınızda oturum açın.

1. Seçin **ayarları** sayfanın sağ üst köşesindeki simgesi.

    ![HubSpot, ayarlar simgesine](./media/hubspot-tutorial/config1.png)

1. Seçin **hesap varsayılanları**.

    ![HubSpot hesabı varsayılan olarak seçeneği](./media/hubspot-tutorial/config2.png)

1. Ekranı aşağı kaydırarak **güvenlik** bölümüne ve ardından **ayarlanan**.

    ![HubSpot seçeneği ayarlama](./media/hubspot-tutorial/config3.png)

1. İçinde **tek oturum açma kurulumunu** bölümünde, aşağıdaki adımları tamamlayın:

    1. İçinde **dinleyici URL'si (hizmet sağlayıcı varlık kimliği)** kutusunda **kopyalama** değeri kopyalamak için. Azure portalında, **temel SAML yapılandırma** bölmesinde değerinde yapıştırın **tanımlayıcı** kutusu.

    1. İçinde **URl, ACS, alıcı ya da yeniden yönlendirme oturum** kutusunda **kopyalama** değeri kopyalamak için. Azure portalında, **temel SAML yapılandırma** bölmesinde değerinde yapıştırın **yanıt URL'si** kutusu.

    1. İçinde HubSpot, içinde **kimlik sağlayıcı tanımlayıcısı veya veren URL'si** kutusunda, değeri yapıştırın **Azure AD tanımlayıcısı** Azure portalında kopyaladığınız.

    1. İçinde HubSpot, içinde **kimlik sağlayıcısının çoklu oturum açma URL'si** kutusunda, değeri yapıştırın **oturum açma URL'si** Azure portalında kopyaladığınız.

    1. İndirdiğiniz Certificate(Base64) dosyayı Windows Not Defteri'nde açın. Seçin ve dosyasının içeriğini kopyalayın. Ardından, HubSpot yapıştırın **X.509 sertifikası** kutusu.

    1. Seçin **doğrulayın**.

        ![Çoklu oturum açma bölümünde HubSpot Ayarla](./media/hubspot-tutorial/config4.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

1. Azure portalında **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.

    ![Kullanıcılar ve tüm kullanıcılar seçenekleri](common/users.png)

1. Seçin **yeni kullanıcı**.

    ![Yeni kullanıcı seçeneği](common/new-user.png)

1. İçinde **kullanıcı** bölmesinde, aşağıdaki adımları tamamlayın:

    1. İçinde **adı** kutusuna **BrittaSimon**.
  
    1. İçinde **kullanıcı adı** kutusuna **brittasimon\@\<your-şirket etki alanı >.\< Uzantı\>**. Örneğin, **brittasimon\@contoso.com**.

    1. Seçin **Show parola** onay kutusu. Görüntülenen değer azaltma **parola** kutusu.

    1. **Oluştur**’u seçin.

    ![Kullanıcı bölmesi](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Filiz Azure çoklu oturum açma kullanabilmeniz için bu bölümde, Britta Simon HubSpot için erişim.

1. Azure portalında **kurumsal uygulamalar** > **tüm uygulamaları** > **HubSpot**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **HubSpot**.

    ![Uygulamalar listesinde HubSpot](common/all-applications.png)

1. Menüde **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar seçeneği](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**. Ardından **ataması ekleme** bölmesinde **kullanıcılar ve gruplar**.

    ![Ekle atama bölmesi](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** bölmesinde **Britta Simon** kullanıcılar listesinde. **Seç**’i seçin.

1. SAML onaylaması rol değeri de beklediğiniz varsa **rol seçme** bölmesinde, listeden kullanıcı için uygun rolü seçin. **Seç**’i seçin.

1. İçinde **atama Ekle** bölmesinde **atama**.

### <a name="create-a-hubspot-test-user"></a>HubSpot test kullanıcısı oluşturma

Azure AD etkinleştirmek için HubSpot, kullanıcının oturum açmak için bir kullanıcı HubSpot sağlanması gerekir. HubSpot sağlama bir el ile gerçekleştirilen bir görevdir.

Bir kullanıcı hesabı HubSpot sağlamak için:

1. HubSpot şirketinizin sitesi için yönetici olarak oturum açın.

1. Seçin **ayarları** sayfanın sağ üst köşesindeki simgesi.

    ![HubSpot, ayarlar simgesine](./media/hubspot-tutorial/config1.png)

1. Seçin **kullanıcılar ve takımlar**.

    ![Kullanıcılar ve takımlar HubSpot seçeneği](./media/hubspot-tutorial/user1.png)

1. Seçin **kullanıcı oluşturma**.

    ![Kullanıcı seçeneğini HubSpot](./media/hubspot-tutorial/user2.png)

1. İçinde **Ekle e-posta addess(es)** kutusuna, kullanıcının e-posta adresi biçimi brittasimon içinde girin\@contoso.com ve ardından **sonraki**.

    ![HubSpot Oluştur kullanıcılar bölümünde kutusunda Ekle e-posta adresleri](./media/hubspot-tutorial/user3.png)

1. İçinde **kullanıcılar oluşturma** bölümünde, her sekmesini seçin. Her sekme, kullanıcı için izinleri ve ilgili seçenekleri ayarlayın. Sonra **İleri**’yi seçin.

    ![HubSpot Oluştur kullanıcılar bölümünde sekmeleri](./media/hubspot-tutorial/user4.png)

1. Kullanıcı Davet göndermeye seçin **Gönder**.

    ![HubSpot gönderme seçeneği](./media/hubspot-tutorial/user5.png)

    > [!NOTE]
    > Kullanıcı, kullanıcı daveti kabul ettikten sonra etkinleştirilir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, uygulamalarım portalını kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Çoklu oturum açma, getirdiğinizde seçtiğinizde **HubSpot** uygulamalarım portalında, otomatik olarak HubSpot için oturum açtınız. Uygulamalarım portal hakkında daha fazla bilgi için bkz. [erişim ve kullanım uygulamaları uygulamalarım portalında](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bu makaleleri gözden geçirin:

- [İçin Azure Active Directory ile SaaS uygulamalarını tümleştirme konusundaki öğreticilerin listesine](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
