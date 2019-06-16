---
title: 'Öğretici: Kontrol noktası CloudGuard Dome9 yay ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve kontrol noktası CloudGuard Dome9 yay arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fdaaab8257d3a79130902e1ba0466f9cf15484f4
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147176"
---
# <a name="tutorial-integrate-check-point-cloudguard-dome9-arc-with-azure-active-directory"></a>Öğretici: Kontrol noktası CloudGuard Dome9 yay Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile kontrol noktası CloudGuard Dome9 yay tümleştirme öğreneceksiniz. Kontrol noktası CloudGuard Dome9 yay Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Kontrol noktası CloudGuard Dome9 yay erişimi, Azure AD'de denetler.
* Otomatik olarak kontrol noktası CloudGuard Dome9 Yaya kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Kontrol noktası CloudGuard Dome9 yay çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Kontrol noktası CloudGuard Dome9 yay destekler **SP ve IDP** SSO başlattı.

## <a name="adding-check-point-cloudguard-dome9-arc-from-the-gallery"></a>Kontrol noktası CloudGuard Dome9 yay galeri ekleme

Azure AD'de kontrol noktası CloudGuard Dome9 yay tümleştirmesini yapılandırmak için kontrol noktası CloudGuard Dome9 yay Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **kontrol noktası CloudGuard Dome9 yay** arama kutusuna.
1. Seçin **kontrol noktası CloudGuard Dome9 yay** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve test etme noktası CloudGuard Dome9 adlı bir test kullanıcı kullanarak yay denetleme ile Azure AD SSO **B.Simon**. Çalışmak SSO için noktası CloudGuard Dome9 yay denetleyin bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO kontrol noktası CloudGuard Dome9 yay ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Kontrol noktası CloudGuard Dome9 yay yapılandırma](#configure-check-point-cloudguard-dome9-arc)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma B.Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  B.Simon Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[Kontrol noktası CloudGuard Dome9 yay test kullanıcısı oluşturma](#create-check-point-cloudguard-dome9-arc-test-user)**  B.Simon bir karşılığı kontrol noktası CloudGuard Dome9 kullanıcı Azure AD gösterimini bağlı yay sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **kontrol noktası CloudGuard Dome9 yay** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma** .
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://secure.dome9.com/`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > Öğreticinin ilerleyen bölümlerinde açıklanan dome9 Yönetim Portalı'nda, şirket adı değeri seçer.

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://secure.dome9.com/sso/saml/<yourcompanyname>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [kontrol noktası CloudGuard Dome9 yay istemci Destek ekibine](mailto:Dome9@checkpoint.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Denetim noktası CloudGuard Dome9 yay uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** kullanıcı öznitelikleri iletişim kutusunu açmak için simge.

    ![image](common/edit-attribute.png)

7. Yukarıdaki için Ayrıca, kontrol noktası CloudGuard Dome9 yay uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki: 

    | Ad |  Kaynak özniteliği|
    | ---------------| --------------- |
    | İz | User.assignedroles |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Üzerinde **noktası CloudGuard Dome9 yay denetleyin ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-check-point-cloudguard-dome9-arc"></a>Kontrol noktası CloudGuard Dome9 yay yapılandırın

1. Farklı bir web tarayıcı penceresinde bir kontrol noktası CloudGuard Dome9 yay şirket sitenize yönetici olarak oturum açın.

2. Tıklayarak **profil ayarları** 'a tıklayın ve sağ üst köşedeki **hesap ayarları**. 

    ![Noktası CloudGuard Dome9 yay yapılandırmasını denetleyin](./media/dome9arc-tutorial/configure1.png)

3. Gidin **SSO** ve ardından **etkinleştirme**.

    ![Noktası CloudGuard Dome9 yay yapılandırmasını denetleyin](./media/dome9arc-tutorial/configure2.png)

4. SSO Yapılandırması bölümünde aşağıdaki adımları gerçekleştirin:

    ![Noktası CloudGuard Dome9 yay yapılandırmasını denetleyin](./media/dome9arc-tutorial/configure3.png)

    a. Şirket adı girmeniz **hesap kimliği** metin. Bu değer yanıt URL'si Azure Portalı'nda belirtildiği şekilde, **temel SAML yapılandırma** bölümü.

    b. İçinde **veren** metin değerini yapıştırın **Azure AD tanımlayıcısı**, Azure portalında form kopyaladığınız.

    c. İçinde **IDP uç nokta URL'si** metin değerini yapıştırın **oturum açma URL'si**, Azure portalında form kopyaladığınız.

    d. İndirilen, Base64 olarak kodlanmış sertifika Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **X.509 sertifikası** metin.

    e. **Kaydet**’e tıklayın.

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

Bu bölümde, kontrol noktası CloudGuard Dome9 Yaya erişim vererek, Azure çoklu oturum açma kullanılacak B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **kontrol noktası CloudGuard Dome9 yay**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-check-point-cloudguard-dome9-arc-test-user"></a>Kontrol noktası CloudGuard Dome9 yay test kullanıcısı oluşturma

Kontrol noktası CloudGuard Dome9 Yaya oturum açmak Azure AD kullanıcılarının etkinleştirmek için kullanıcılar uygulamaya sağlanması gerekir. Noktası CloudGuard Dome9 yay, just-ın-time sağlamayı destekler ancak kullanıcı sahip belirli seçmek olan düzgün çalışması denetleme **rol** ve aynı kullanıcıya atayın.

   >[!Note]
   >İçin **rol** oluşturma ve diğer kişi ayrıntıları [kontrol noktası CloudGuard Dome9 yay istemci Destek ekibine](mailto:Dome9@checkpoint.com).

**Bir kullanıcı hesabını el ile sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Kontrol noktası CloudGuard Dome9 yay şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayarak **kullanıcıları ve rolleri** ve ardından **kullanıcılar**.

    ![Çalışan Ekle](./media/dome9arc-tutorial/user1.png)

3. Tıklayın **Kullanıcı Ekle**.

    ![Çalışan Ekle](./media/dome9arc-tutorial/user2.png)

4. İçinde **Create User** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/dome9arc-tutorial/user3.png)

    a. İçinde **e-posta** metin kutusuna kullanıcı e-posta türünü ister B.Simon@contoso.com.

    b. İçinde **ad** metin adı b gibi kullanıcı türü

    c. İçinde **Soyadı** metin son Simon gibi kullanıcının adını yazın.

    d. Olun **SSO kullanıcı** olarak **üzerinde**.

    e. Tıklayın **Oluştur**.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde kontrol noktası CloudGuard Dome9 yay kutucuğu seçtiğinizde, otomatik olarak kontrol noktası CloudGuard Dome9 SSO'yu ayarlama yay için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)