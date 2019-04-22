---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile iLMS | Microsoft Docs'
description: Azure Active Directory ve iLMS arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9bcb465f76e09675333e6e608249cba11f722e3
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59274665"
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a>Öğretici: İLMS ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iLMS tümleştirme konusunda bilgi edinin.
Azure AD ile iLMS tümleştirme ile aşağıdaki avantajları sağlar:

* İLMS erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) iLMS için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile iLMS yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* iLMS tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* iLMS destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-ilms-from-the-gallery"></a>Galeriden iLMS ekleme

Azure AD'de iLMS tümleştirmesini yapılandırmak için iLMS Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden iLMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **iLMS**seçin **iLMS** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![sonuç listesinde iLMS](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma iLMS adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının iLMS ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma iLMS ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Çoklu oturum açma iLMS yapılandırma](#configure-ilms-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[İLMS test kullanıcısı oluşturma](#create-ilms-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı iLMS içinde bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile iLMS yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **iLMS** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![etki alanı ve URL'ler iLMS çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusu, yapıştırma **tanımlayıcı** değer, kopyalama **hizmet sağlayıcısı** SAML ayarlarının iLMS Yönetim Portalı'nda.

    b. İçinde **yanıt URL'si** metin kutusu, yapıştırma **uç noktası (URL)** değer, kopyalama **hizmet sağlayıcısı** SAML ayarlarının iLMS yönetim portalında şu desene sahip `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![etki alanı ve URL'ler iLMS çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusu, yapıştırma **uç noktası (URL)** değer, kopyalama **hizmet sağlayıcısı** olarak iLMS Yönetim Portalı'nda SAML ayarları bölümü `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

6. JIT sağlamayı etkinleştirmek için özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını iLMS uygulamanızı bekliyor. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** kullanıcı öznitelikleri iletişim kutusunu açmak için simge.

    ![image](common/edit-attribute.png)

    > [!NOTE]
    > Etkinleştirmek sahip olduğunuz **Un-recognized kullanıcı hesabı oluşturma** bu öznitelikleri eşlemek için iLMS içinde. Yönergeleri izleyerek [burada](http://support.inspiredelearning.com/customer/portal/articles/2204526) öznitelikleri yapılandırma hakkında bir fikir edinmek için.

7. Yukarıdaki için ayrıca iLMS uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:

    | Ad | Kaynak özniteliği|
    | --------|------------- |
    | Bölme | User.Department |
    | bölge | User.State |
    | Bölüm | user.jobtitle |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

9. Üzerinde **iLMS kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-ilms-single-sign-on"></a>Çoklu oturum açma iLMS yapılandırın

1. Farklı bir web tarayıcı penceresinde oturum açın, **iLMS Yönetici portalı** yönetici olarak.

2. Tıklayın **SSO:SAML** altında **ayarları** SAML ayarlarını açın ve aşağıdaki adımları gerçekleştirmek için sekmesinde:

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/1.png)

3. Genişletin **hizmet sağlayıcısı** bölümü ve kopyalama **tanımlayıcı** ve **uç noktası (URL)** değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/2.png) 

4. Altında **kimlik sağlayıcısı** bölümünde **meta verileri içeri aktarma**.

5. Seçin **Federasyon meta verileri** dosya, Azure Portalı'ndan indirildiği **SAML imzalama sertifikası** bölümü.

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

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma iLMS erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **iLMS**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **iLMS**.

    ![Uygulamalar listesinde iLMS bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

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

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli iLMS kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama iLMS için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)