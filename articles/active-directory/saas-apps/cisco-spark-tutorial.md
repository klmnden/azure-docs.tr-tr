---
title: 'Öğretici: Cisco Webex ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Cisco Webex arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6398bed2d232dc7e8b73daa31885fcd6e03fe224
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67105579"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Öğretici: Cisco Webex ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Cisco Webex tümleştirme konusunda bilgi edinin.
Cisco Webex Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Cisco Webex erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Cisco Webex için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Cisco Webex yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Cisco Webex çoklu oturum açmayı abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Cisco Webex destekler **SP** tarafından başlatılan

* Cisco Webex destekler **otomatik** kullanıcı sağlama

## <a name="adding-cisco-webex-from-the-gallery"></a>Cisco Webex galeri ekleme

Azure AD'de Cisco Webex tümleştirmesini yapılandırmak için Cisco Webex Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Cisco Webex Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Cisco Webex**seçin **Cisco Webex** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Cisco Webex](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Cisco adlı bir test kullanıcı tabanlı Webex test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve Cisco Webex ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Cisco Webex ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Cisco Webex çoklu oturum açmayı yapılandırma](#configure-cisco-webex-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Cisco Webex test kullanıcısı oluşturma](#create-cisco-webex-test-user)**  - kullanıcı Azure AD gösterimini bağlı Cisco Webex Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Cisco Webex yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Cisco Webex** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Cisco Webex etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL yazın: `https://web.ciscospark.com/#/signin`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://idbroker.webex.com/<Org Id>`

    > [!NOTE]
    > Bu tanımlayıcı değerini gerçek değil. Bu değer, gerçek tanımlayıcısıyla güncelleştirin. Hizmet sağlayıcısı meta verileri varsa, karşıya **temel SAML yapılandırma** bölümü sonra **tanımlayıcı (varlık kimliği)** değer otomatik olarak doldurulur otomatik alır.

5. Cisco Webex uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayarak **Düzenle** öznitelikleri eklemek için simge.

    ![image](common/edit-attribute.png)

6. Yukarıdaki için Ayrıca, Cisco Webex uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:
    
    | Ad |  Kaynak özniteliği|
    | ---------------|--------- |
    | Kullanıcı Kimliği | User.userPrincipalName |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

8. Üzerinde **Cisco Webex kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-cisco-webex-single-sign-on"></a>Cisco Webex tek oturum açmayı yapılandırın

1. Oturum [Cisco bulut işbirliği Yönetimi](https://admin.ciscospark.com/) tam yönetici kimlik bilgilerinizle.

2. Seçin **ayarları** altında **kimlik doğrulaması** bölümünde **Değiştir**.

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_10.png)
  
3. Seçin **3. taraf kimlik sağlayıcısı tümleştirin. (Gelişmiş)**  ve sonraki ekrana gidin.

4. Üzerinde **IDP meta verileri içeri aktarma** sayfasında, ya da sürükle ve bırak sayfaya Azure AD'ye meta veri dosyası veya dosya tarayıcı seçeneğini bulun ve Azure AD meta veri dosyasını karşıya yüklemek için kullanın. Ardından, **meta verileri (daha güvenli) bir sertifika yetkilisi tarafından imzalanmış bir sertifika gerektir** tıklatıp **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_11.png)

5. Seçin **SSO Bağlantıyı Sına**ve yeni bir tarayıcı sekmesi oturum açtığında, oturum açarak Azure AD kimlik doğrulaması.

6. Geri dönüp **Cisco bulut işbirliği Yönetimi** tarayıcı sekmesinde. Test başarılı olursa seçin **bu testi başarılı. Çoklu oturum açma seçeneğini etkinleştirin** tıklatıp **sonraki**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Cisco Webex erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Cisco Webex**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Cisco Webex**.

    ![Uygulamalar listesini Cisco Webex bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-cisco-webex-test-user"></a>Cisco Webex test kullanıcısı oluşturma

Bu bölümde, Britta Simon Cisco Webex adlı bir kullanıcı oluşturun. Bu bölümde, Britta Simon Cisco Webex adlı bir kullanıcı oluşturun.

1. Git [Cisco bulut işbirliği Yönetimi](https://admin.ciscospark.com/) tam yönetici kimlik bilgilerinizle.

2. Tıklayın **kullanıcılar** ardından **kullanıcıları yönetme**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. İçinde **yönetme kullanıcı** penceresinde **el ile eklemek veya kullanıcıları değiştirin** tıklatıp **sonraki**.

4. Seçin **adlarını ve e-posta adresi**. Ardından, metin kutusu aşağıdaki gibi doldurun:

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    a. İçinde **ad** metin adı gibi kullanıcı türü **Britta**.

    b. İçinde **Soyadı** metin türü son gibi kullanıcı adını **Simon**.

    c. İçinde **e-posta adresi** metin kutusuna kullanıcı e-posta adresini yazın ister **britta.simon\@contoso.com**.

5. Britta Simon eklemek için artı işaretine tıklayın. Ardından **İleri**'ye tıklayın.

6. İçinde **Hizmetleri kullanıcıları ekleme** penceresinde tıklayın **Kaydet** ardından **son**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Cisco Webex kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Cisco Webex için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı sağlamayı yapılandırma](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-spark-provisioning-tutorial) 
