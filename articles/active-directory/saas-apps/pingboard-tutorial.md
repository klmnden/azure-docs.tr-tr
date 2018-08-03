---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Pingboard | Microsoft Docs'
description: Azure Active Directory ve Pingboard arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: 794e3f6fe568d76f0687caa36709185f2a538270
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436109"
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Öğretici: Azure Active Directory Pingboard ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Pingboard tümleştirme konusunda bilgi edinin.

Azure AD ile Pingboard tümleştirme ile aşağıdaki avantajları sağlar:

- Pingboard erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Pingboard (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Pingboard yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Pingboard çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Pingboard ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-pingboard-from-the-gallery"></a>Galeriden Pingboard ekleme
Azure AD'de Pingboard tümleştirmesini yapılandırmak için Pingboard Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Pingboard eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar][2]

1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Pingboard**seçin **Pingboard** sonuç paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Pingboard](./media/pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Pingboard sınayın.

Tek iş için oturum açma için Azure AD ne Pingboard karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Pingboard ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** Pingboard içinde.

Yapılandırma ve Azure AD çoklu oturum açma Pingboard ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Pingboard test kullanıcısı oluşturma](#create-a-pingboard-test-user)**  - kullanıcı Azure AD gösterimini bağlı Pingboard Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Pingboard uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Pingboard yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Pingboard** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1.  Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/pingboard-tutorial/tutorial_pingboard_samlbase.png)

1. Üzerinde **Pingboard etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Oturum açma bilgileri IDP çoklu Pingboard etki alanı ve URL'ler](./media/pingboard-tutorial/tutorial_pingboard_url.png)

    a. İçinde **tanımlayıcı** metin değeri olarak yazın: `http://app.pingboard.com/sp`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<entity-id>.pingboard.com/auth/saml/consume`

1. Denetleme **Gelişmiş URL ayarlarını göster**, uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Oturum açma bilgileri SP çoklu Pingboard etki alanı ve URL'ler](./media/pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

     İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak URL'yi yazın: `https://<sub-domain>.pingboard.com/sign_in`

    > [!NOTE]
    > Lütfen bu değerler gerçek olmadığına dikkat edin. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Pingboard istemci Destek ekibine](https://support.pingboard.com/) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda XML dosyasını kaydedin.

    ![Pingboard meta veri xml](./media/pingboard-tutorial/tutorial_pingboard_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pingboard-tutorial/tutorial_general_400.png)

1. SSO Pingboard tarafında yapılandırmak için yeni bir tarayıcı penceresi açın ve Pingboard hesabınızda oturum açın. Çoklu oturum açma ayarlamak için Pingboard yönetici olması gerekir.

1. Üstteki menüden,, seçin **uygulamaları > tümleştirmeleri**

    ![Çoklu oturum açmayı yapılandırın](./media/pingboard-tutorial/Pingboard_integration.png)

1. Üzerinde **tümleştirmeler** sayfasında, bulmak **"Azure Active Directory"** kutucuğuna ve üzerine tıklayın.

    ![Pingboard tek oturum açma tümleştirmesi](./media/pingboard-tutorial/Pingboard_aad.png)

1. Click izleyen kalıcı olarak **"Yapılandır"**

    ![Pingboard Yapılandırma düğmesi](./media/pingboard-tutorial/Pingboard_configure.png)

1. Sonraki sayfada, "Azure SSO tümleştirme etkinleştirildiğini" dikkat edin. İndirilen meta veri XML dosyasını Not Defteri'nde açın ve içeriği yapıştırın **IDP meta verileri**.

    ![Pingboard SSO yapılandırma ekranında](./media/pingboard-tutorial/Pingboard_sso_configure.png)

1. Dosya doğrulanır ve her şeyin doğru ise, çoklu oturum açma şimdi etkinleştirilir.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/pingboard-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/pingboard-tutorial/create_aaduser_02.png)

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.

    ![Ekle düğmesi](./media/pingboard-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/pingboard-tutorial/create_aaduser_04.png)

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-pingboard-test-user"></a>Pingboard test kullanıcısı oluşturma

Bu bölümün amacı Pingboard Britta Simon adlı bir kullanıcı oluşturmaktır. Pingboard otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](pingboard-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. Pingboard şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayın **"Çalışan Ekle"** düğmesini **dizin** sayfası.

    ![Çalışan Ekle](./media/pingboard-tutorial/create_testuser_add.png)

1. Üzerinde **"Çalışan Ekle"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kişileri davet edin](./media/pingboard-tutorial/create_testuser_name.png)

    a. İçinde **tam adı** metin kutusuna kullanıcının tam adını yazın ister **Britta Simon**.

    b. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister **brittasimon@contoso.com**.

    c. İçinde **iş unvanı** metin Britta simon'un iş unvanı yazın.

    d. İçinde **konumu** açılır listesinde, Britta Simon konumunu seçin.

    e. **Ekle**'ye tıklayın.

1. Kullanıcı eklenmesini doğrulamak için bir onay ekranı göründüğünde.

    ![Onayla](./media/pingboard-tutorial/create_testuser_confirm.png)

    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Pingboard erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Pingboard için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Pingboard**.

    ![Uygulamalar listesinde Pingboard bağlantı](./media/pingboard-tutorial/tutorial_pingboard_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

Erişim panelinde Pingboard kutucuğa tıkladığınızda, otomatik olarak Pingboard uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](pingboard-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/pingboard-tutorial/tutorial_general_01.png
[2]: ./media/pingboard-tutorial/tutorial_general_02.png
[3]: ./media/pingboard-tutorial/tutorial_general_03.png
[4]: ./media/pingboard-tutorial/tutorial_general_04.png

[100]: ./media/pingboard-tutorial/tutorial_general_100.png

[200]: ./media/pingboard-tutorial/tutorial_general_200.png
[201]: ./media/pingboard-tutorial/tutorial_general_201.png
[202]: ./media/pingboard-tutorial/tutorial_general_202.png
[203]: ./media/pingboard-tutorial/tutorial_general_203.png