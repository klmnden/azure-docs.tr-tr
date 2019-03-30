---
title: 'Öğretici: Pega sistemler ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Pega sistemleri arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 31acf80f-1f4b-41f1-956f-a9fbae77ee69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 59dc9f82251e7a406e6fe1339fdb55b4880cd74d
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649195"
---
# <a name="tutorial-azure-active-directory-integration-with-pega-systems"></a>Öğretici: Pega sistemler ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Pega sistemleri tümleştirme konusunda bilgi edinin.
Azure AD ile Pega sistemleri tümleştirme ile aşağıdaki avantajları sağlar:

* Pega sistemlerine erişimi olan Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Pega sistemlerine (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Pega sistemleriyle yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Tek oturum açma etkin abonelik Pega sistemleri

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Pega sistemlerini destekleyen **SP** ve **IDP** tarafından başlatılan

## <a name="adding-pega-systems-from-the-gallery"></a>Galeriden Pega sistemleri ekleme

Azure AD'de Pega sistemleri tümleştirmesini yapılandırmak için Pega sistemleri Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Pega sistemleri eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Pega sistemleri**seçin **Pega sistemleri** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Pega sistemleri](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Pega adlı bir test kullanıcı tabanlı sistemler ile Azure AD çoklu oturum açmayı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı Pega sistemleri arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Pega sistemlerle sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Pega sistemleri çoklu oturum açmayı yapılandırma](#configure-pega-systems-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Pega sistemleri test kullanıcısı oluşturma](#create-pega-systems-test-user)**  - kullanıcı Azure AD gösterimini bağlı Pega sistemlerinde Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Pega sistemleriyle yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Pega sistemleri** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Pega sistemleri etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/sp/<INSTANCEID>`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/PRRestService/WebSSO/SAML/AssertionConsumerService`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Pega sistemleri etki alanı ve URL'ler tek oturum açma bilgileri](common/both-advanced-urls.png)

    a. İçinde **oturum açma URL'si** metin kutusuna oturum açma URL değeri yazın.

    b. İçinde **geçiş durumu** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<CUSTOMERNAME>.pegacloud.io/prweb/sso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si, oturum açma URL'si ile güncelleştirin ve durumunu URL geçiş. Bu öğreticinin ilerleyen bölümlerinde açıklanan Pega uygulama tanımlayıcısı ve yanıt URL'si değerleri bulabilirsiniz. Geçiş durumu için kişi [Pega sistemlerini istemci Destek ekibine](https://www.pega.com/contact-us) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Pega sistemleri uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** açmak için simgeyi **kullanıcı öznitelikleri** iletişim.

    ![görüntü](common/edit-attribute.png)

7. Yukarıdaki için ayrıca Pega sistemleri uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:

    | Ad | Kaynak özniteliği|
    | ------------------- | -------------------- |
    | Kullanıcı Kimliği | *********** |
    | CN =  | *********** |
    | posta | *********** |
    | accessgroup | *********** |
    | Kuruluş | *********** |
    | orgdivision | *********** |
    | orgunit | *********** |
    | Çalışma grubu | *********** |
    | Telefon | *********** |

    > [!NOTE]
    > Müşteri belirli değerleri şunlardır. Lütfen, uygun değerleri sağlayın.

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![görüntü](common/new-save-attribute.png)

    ![görüntü](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

9. Üzerinde **Pega sistemi'kurmak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-pega-systems-single-sign-on"></a>Pega sistemleri çoklu oturum açmayı yapılandırın

1. Çoklu oturum açmayı yapılandırma **Pega sistemleri** yanı, açık **Pega portalı** yönetici hesabıyla başka bir tarayıcı penceresinde.

2. Seçin **oluşturma** -> **SysAdmin** -> **kimlik doğrulama hizmeti**.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin.png)
    
3. Aşağıdaki işlemleri gerçekleştirin **oluşturma kimlik doğrulama hizmeti** ekran:

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin1.png)

    a. Seçin **SAML 2.0** türünden

    b. İçinde **adı** metin herhangi adı ör. Azure AD SSO girin

    c. İçinde **kısa açıklama** metin kutusuna bir açıklama girin  

    d. Tıklayarak **oluştur ve Aç** 
    
4. İçinde **kimlik sağlayıcıyı (IDP) bilgi** bölümünde, tıklayarak **IDP alma meta verileri** ve Azure portalından indirdiğiniz meta veri dosyasına göz atın. Tıklayın **Gönder** meta verileri yüklenemedi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin2.png)
    
5. Aşağıda gösterildiği gibi bu IDP verilerini doldurulur.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin3.png)
    
6. Aşağıdaki işlemleri gerçekleştirin **hizmet sağlayıcısı (SP) ayarları** bölümü:

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin4.png)

    a. Kopyalama **varlık kimliği** için yapıştırın ve değer **tanımlayıcı** metin kutusunda **temel SAML yapılandırma** Azure portalında.

    b. Kopyalama **onaylama tüketici hizmeti (ACS) konumu** için yapıştırın ve değer **yanıt URL'si** metin kutusunda **temel SAML yapılandırma** Azure portalında.

    c. Seçin **imzalama isteğini devre dışı**.

7. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Pega sisteme erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Pega sistemleri**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Pega sistemleri**.

    ![Uygulamalar listesinde Pega sistemleri bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-pega-systems-test-user"></a>Pega sistemleri test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Pega sistemlerinde adlı bir kullanıcı oluşturmaktır. Çalışmak [Pega sistemlerini istemci Destek ekibine](https://www.pega.com/contact-us) Pega sistemlerinde kullanıcıları oluşturmak için.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Pega sistemleri kutucuğa tıkladığınızda, otomatik olarak SSO'yu ayarlama Pega sistemleri için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)