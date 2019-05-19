---
title: 'Öğretici: SAP Cloud Platform kimlik doğrulaması ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: SAP Cloud Platform kimlik doğrulaması ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e242e85525b446fcbe8a2ec05da539fb45acf487
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65868003"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Öğretici: SAP Cloud Platform kimlik doğrulaması ile Azure Active Directory Tümleştirme

Bu öğreticide, SAP Cloud Platform kimlik doğrulaması Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
SAP Cloud Platform kimlik doğrulaması, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* SAP Cloud Platform kimlik doğrulaması erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak SAP Cloud Platform kimlik doğrulaması için (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

SAP Cloud Platform kimlik doğrulaması ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* SAP Cloud Platform kimlik doğrulaması çoklu oturum açmayı abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SAP Cloud Platform kimlik doğrulaması destekler **SP** ve **IDP** tarafından başlatılan

Teknik ayrıntılara girmeden önce bakmak için gideceğinizi kavramları anlamak için önemlidir. SAP Cloud Platform kimlik doğrulaması ve Active Directory Federasyon Hizmetleri, uygulamaları veya Hizmetleri (bir IDP) olarak Azure AD ile SAP uygulamaları tarafından korunan ve SAP bulut tarafından korunan hizmetleri arasında SSO uygulamanızı etkinleştirme Platform kimlik doğrulaması.

Şu anda SAP Cloud Platform kimlik doğrulaması, SAP uygulamaları için bir ara sunucu kimlik sağlayıcısı olarak görev yapar. Azure Active Directory, modunu bu kurulumunda önde gelen kimlik sağlayıcısı olarak görev yapar. 

Aşağıdaki diyagramda, bu ilişkiyi göstermektedir:

![Bir Azure AD test kullanıcısı oluşturma](./media/sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Bu kurulum ile SAP Cloud Platform kimlik doğrulaması kiracınızın, Azure Active Directory'de güvenilen bir uygulama olarak yapılandırılır.

Daha sonra tüm SAP uygulamalarını ve bu şekilde korumak istediğiniz hizmetleri SAP Cloud Platform kimlik doğrulaması Yönetimi konsolunda yapılandırılır.

Bu nedenle, SAP uygulama ve hizmetlere erişim izni verme için yetkilendirme SAP Cloud Platform kimlik doğrulaması (aksine, Azure Active Directory) içinde gerçekleşmesi gerekir.

SAP Cloud Platform kimlik doğrulaması Azure Active Directory Marketi üzerinden bir uygulama olarak yapılandırarak, tek bir talep veya SAML onaylamalarını yapılandırmanız gerekmez.

> [!NOTE]
> Şu anda yalnızca Web SSO her iki taraf tarafından test edilmiştir. Uygulama API veya API iletişim için gerekli olan akışların çalışması gerekir, ancak henüz test edilmemiştir. Sonraki etkinliklerin sırasında sınanır.

## <a name="adding-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Galeriden SAP Cloud Platform kimlik doğrulaması ekleme

Azure AD'de SAP Cloud Platform kimlik doğrulaması tümleştirmesini yapılandırmak için SAP Cloud Platform kimlik doğrulaması Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SAP Cloud Platform kimlik doğrulaması eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SAP Cloud Platform kimlik doğrulaması**seçin **SAP Cloud Platform kimlik doğrulaması** sonucu panelinden ardından **Ekle** düğmesi uygulama ekleyin.

     ![Sonuç listesinde SAP Cloud Platform kimlik doğrulaması](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD'ye tek temelinde oturum açma adlı bir test kullanıcısı [uygulama adı] ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve [uygulama adı] ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma [uygulama adı] ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SAP Cloud Platform kimlik kimlik doğrulaması çoklu oturum açmayı yapılandırma](#configure-sap-cloud-platform-identity-authentication-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SAP Cloud Platform kimlik doğrulaması test kullanıcısı oluşturma](#create-sap-cloud-platform-identity-authentication-test-user)**  - bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı SAP Cloud Platform kimlik doğrulaması sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma [uygulama adı] ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **SAP Cloud Platform kimlik doğrulaması** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** yapılandırmak istiyorsanız, bölüm **IDP** intiated modu, aşağıdaki adımları gerçekleştirin:

    ![SAP Cloud Platform kimlik kimlik doğrulaması etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `<IAS-tenant-id>.accounts.ondemand.com`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<IAS-tenant-id>.accounts.ondemand.com/saml2/idp/acs/<IAS-tenant-id>.accounts.ondemand.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [SAP Cloud Platform kimlik kimlik doğrulama istemcisi Destek ekibine](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) bu değerleri almak için. Tanımlayıcı değeri anlamıyorsanız, SAP Cloud Platform kimlik doğrulaması belgeleri hakkında okuyun [Kiracı SAML 2.0 Yapılandırması](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![SAP Cloud Platform kimlik kimlik doğrulaması etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `{YOUR BUSINESS APPLICATION URL}`

    > [!NOTE]
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. Lütfen oturum açma, belirli iş uygulama URL'sini kullanın. İlgili kişi [SAP Cloud Platform kimlik kimlik doğrulama istemcisi Destek ekibine](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) herhangi bir konuda şüpheleriniz varsa.

6. SAP Cloud Platform kimlik doğrulaması uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

7. SAP uygulama gibi bir öznitelik bekliyorsa **firstName**, ekleme **firstName** özniteliğini **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri**  iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin öznitelik adı firstName yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, öznitelik değeri user.givenname seçin.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **meta veri XML**bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

9. Üzerinde **SAP Cloud Platform kimlik doğrulaması ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-sap-cloud-platform-identity-authentication-single-sign-on"></a>SAP Cloud Platform kimlik kimlik doğrulaması çoklu oturum açmayı yapılandırın

1. Uygulamanız için yapılandırılmış SSO edinmek için SAP Cloud Platform kimlik doğrulaması yönetim konsoluna gidin. URL şu desene sahip: `https://<tenant-id>.accounts.ondemand.com/admin`. Ardından SAP Cloud Platform kimlik doğrulaması hakkında belgeleri okuyun [Microsoft Azure AD ile tümleştirme](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).

2. Azure portalında **Kaydet** düğmesi.

3. Yalnızca ekleme ve başka bir SAP uygulama için SSO'yu etkinleştirmek isterseniz, aşağıdaki ile devam edin. Bölümünde adımları yineleyin **galerisinden SAP Cloud Platform kimlik doğrulaması ekleme**.

4. Azure portalında, üzerinde **SAP Cloud Platform kimlik doğrulaması** uygulama tümleştirme sayfasında **bağlantılı oturum açma**.

    ![Bağlantılı oturum açmayı yapılandırın](./media/sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)

5. Yapılandırmayı kaydedin.

> [!NOTE]
> Yeni uygulama, çoklu oturum açma yapılandırması önceki SAP uygulamasının yararlanır. SAP Cloud Platform kimlik doğrulaması yönetim konsolunda aynı Kurumsal kimlik sağlayıcıları kullandığınızdan emin olun.

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

Bu bölümde, SAP Cloud Platform kimlik doğrulaması için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SAP Cloud Platform kimlik doğrulaması**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **SAP Cloud Platform kimlik doğrulaması**.

    ![Uygulamalar listesinde SAP Cloud Platform kimlik doğrulaması bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sap-cloud-platform-identity-authentication-test-user"></a>SAP Cloud Platform kimlik doğrulaması test kullanıcısı oluşturma

SAP Cloud Platform kimlik doğrulaması bir kullanıcı oluşturmanız gerekmez. Azure AD kullanıcı deposunda olan kullanıcılar, SSO işlevselliğini kullanabilirsiniz.

SAP Cloud Platform kimlik doğrulaması Kimlik Federasyonu seçeneğini destekler. Bu seçenek, Kurumsal kimlik sağlayıcısı tarafından doğrulanır kullanıcılar kullanıcı deposu, SAP Cloud Platform kimlik doğrulaması içinde olup olmadığını denetlemek için uygulamanın sağlar.

Kimlik Federasyonu seçeneği varsayılan olarak devre dışıdır. Kimlik Federasyonu etkinleştirilirse, SAP Cloud Platform kimlik doğrulaması içeri aktarılan kullanıcılar uygulamaya erişebilir.

Etkinleştirmek veya SAP Cloud Platform kimlik doğrulaması ile Kimlik Federasyonu devre dışı bırakma hakkında daha fazla bilgi için "Etkinleştirme kimlik Federasyon ile SAP Cloud Platform kimlik doğrulaması" bölümüne bakın. [ile Kimlik Federasyonu yapılandırma Kullanıcı Store, SAP Cloud Platform kimlik doğrulaması](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SAP Cloud Platform kimlik doğrulaması kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama SAP Cloud Platform kimlik doğrulaması için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
