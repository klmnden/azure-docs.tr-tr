---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile iLMS | Microsoft Docs'
description: Azure Active Directory ve iLMS arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee12bfc8a79b6abcfdf2978d4e640c34f801e346
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65989732"
---
# <a name="tutorial-integrate-ilms-with-azure-active-directory"></a>Öğretici: İLMS Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iLMS tümleştirme öğreneceksiniz. İLMS Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* İLMS erişimi, Azure AD'de denetler.
* Otomatik olarak iLMS Azure AD'ye hesaplarıyla oturum olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* iLMS çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. iLMS destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-ilms-from-the-gallery"></a>Galeriden iLMS ekleme

Azure AD'de iLMS tümleştirmesini yapılandırmak için iLMS Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **iLMS** arama kutusuna.
1. Seçin **iLMS** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO ile iLMS adlı bir test kullanıcısı kullanarak test etme **Britta Simon**. Çalışmak SSO için iLMS içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile iLMS sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SSO iLMS yapılandırma](#configure-ilms-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[İLMS test kullanıcısı oluşturma](#create-ilms-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı iLMS içinde bir karşılığı vardır.
6. **[Test SSO](#test-sso)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **iLMS** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak istiyorsanız, sayfa **IDP** başlatılan modu, aşağıdaki alanlar için değerleri girin:

    a. İçinde **tanımlayıcı** metin kutusu, yapıştırma **tanımlayıcı** değer, kopyalama **hizmet sağlayıcısı** SAML ayarlarının iLMS Yönetim Portalı'nda.

    b. İçinde **yanıt URL'si** metin kutusu, yapıştırma **uç noktası (URL)** değer, kopyalama **hizmet sağlayıcısı** SAML ayarlarının iLMS yönetim portalında şu desene sahip `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

1. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusu, yapıştırma **uç noktası (URL)** değer, kopyalama **hizmet sağlayıcısı** olarak iLMS Yönetim Portalı'nda SAML ayarları bölümü `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

1. JIT sağlamayı etkinleştirmek için özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını iLMS uygulamanızı bekliyor. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** kullanıcı öznitelikleri iletişim kutusunu açmak için simge.

    > [!NOTE]
    > Etkinleştirmek sahip olduğunuz **Un-recognized kullanıcı hesabı oluşturma** bu öznitelikleri eşlemek için iLMS içinde. Yönergeleri izleyerek [burada](https://support.inspiredelearning.com/help/adding-updating-and-managing-users#just-in-time-provisioning-with-saml-single-signon) öznitelikleri yapılandırma hakkında bir fikir edinmek için.

1. Yukarıdaki için ayrıca iLMS uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:

    | Ad | Kaynak özniteliği|
    | --------|------------- |
    | Bölme | User.Department |
    | bölge | User.State |
    | Bölüm | user.jobtitle |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Üzerinde **iLMS kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-ilms-sso"></a>İLMS SSO yapılandırma

1. Farklı bir web tarayıcı penceresinde oturum açın, **iLMS Yönetici portalı** yönetici olarak.

2. Tıklayın **SSO:SAML** altında **ayarları** SAML ayarlarını açın ve aşağıdaki adımları gerçekleştirmek için sekmesinde:

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/1.png)

3. Genişletin **hizmet sağlayıcısı** bölümü ve kopyalama **tanımlayıcı** ve **uç noktası (URL)** değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/2.png) 

4. Altında **kimlik sağlayıcısı** bölümünde **meta verileri içeri aktarma**.

5. Seçin **Federasyon meta verileri** dosya, Azure portalından indirildiği **SAML imzalama sertifikası** bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/tutorial_ilms_ssoconfig1.png)

6. Sağlama kaldırma için iLMS hesaplarını oluşturmak için JIT etkinleştirmek istiyorsanız-kullanıcıların tanıyacak, aşağıdaki adımları izleyin:

    a. Denetleme **beklemediğiniz tanınan bir kullanıcı hesabı oluşturma**.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/tutorial_ilms_ssoconfig2.png)

    b. Azure AD'de iLMS öznitelikleri ile harita öznitelikleri. Öznitelik sütunu, öznitelik adı veya varsayılan değeri belirtin.

    c. Git **iş kuralları** sekmesinde ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/5.png)

    d. Denetleme **Un-recognized bölgeler oluşturun, bölümler ve Departmanlar** bölgeler, bölümler ve zamanında çoklu oturum açma zaten var olmayan bölümler oluşturmak için.

    e. Denetleme **güncelleştirme kullanıcı profili sırasında oturum açma** her çoklu oturum açma ile kullanıcının profilini güncelleştirilip güncelleştirilmediğini belirtmek için.

    f. Varsa **kullanıcı profili olmayan zorunlu alanlar için güncelleştirme boş değerler** seçeneği, oturum açma temel boş isteğe bağlı profili alanlar da alanlar boş değerler içerecek şekilde kullanıcı iLMS profilinin neden.

    g. Denetleme **hatası bildirim e-posta Gönder** ve hatası bildirim e-postaları almak istediğiniz kullanıcının e-posta girin.

7. Tıklayın **Kaydet** düğmesini kullanarak ayarları kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/save.png)

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

Bu bölümde, Azure çoklu oturum açma iLMS erişim vererek kullanmak Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **iLMS**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-ilms-test-user"></a>İLMS test kullanıcısı oluşturma

Uygulama, zaman kullanıcı sağlamayı ve kimlik doğrulama kullanıcıları otomatik olarak uygulama oluşturulduktan sonra sadece destekler. JIT çalışır, tıkladıysanız **Un-recognized kullanıcı hesabı oluşturma** iLMS Yönetici portalı SAML yapılandırma ayarı sırasında onay kutusu.

Bir kullanıcı el ile oluşturmanız gerekiyorsa, ardından aşağıdaki adımları uygulayın:

1. İLMS şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayın **kullanıcı Kaydet** altında **kullanıcılar** açmak için sekmesinde **kullanıcı Kaydet** sayfası.

   ![Çalışan Ekle](./media/ilms-tutorial/3.png)

3. Üzerinde **kullanıcı Kaydet** sayfasında, aşağıdaki adımları gerçekleştirin.

    ![Çalışan Ekle](./media/ilms-tutorial/create_testuser_add.png)

    a. İçinde **ad** metin Britta gibi ilk tür adı.

    b. İçinde **Soyadı** metin Simon gibi soyadı yazın.

    c. İçinde **e-posta kimliği** metin kutusuna kullanıcı e-posta adresi türü ister BrittaSimon@contoso.com.

    d. İçinde **bölge** açılır listesinde, bölge için bir değer seçin.

    e. İçinde **bölme** açılır listesinde, bölme için bir değer seçin.

    f. İçinde **departmanı** açılır listesinde, departman için bir değer seçin.

    g. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Seçerek kullanıcı için kayıt e-posta gönderebilir **kayıt posta Gönder** onay kutusu.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde iLMS kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama iLMS için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
