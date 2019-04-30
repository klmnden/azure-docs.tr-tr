---
title: 'Öğretici: LinkedIn yükseltmesine ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve LinkedIn yükseltmesine arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 92af3a18be8471013e93afad47146bdbabd4be18
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128796"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Öğretici: Azure Active Directory tümleştirmesiyle LinkedIn Yükselt

Bu öğreticide, LinkedIn yükseltmesine Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Azure AD ile tümleştirme yükseltmesine LinkedIn ile aşağıdaki avantajları sağlar:

* LinkedIn yükseltmesine erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) LinkedIn yükseltmek için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

LinkedIn yükseltmesine ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* LinkedIn yükseltmesine çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* LinkedIn yükseltmesine destekler **SP ve IDP** tarafından başlatılan

* LinkedIn yükseltmesine destekler **zamanında** kullanıcı sağlama

* LinkedIn yükseltmesine destekler [ **otomatik** kullanıcı sağlama](linkedinelevate-provisioning-tutorial.md)

## <a name="adding-linkedin-elevate-from-the-gallery"></a>LinkedIn yükseltmesine galeri ekleme

LinkedIn yükseltmesine tümleştirmesini Azure AD'de yapılandırmak için LinkedIn yükseltmesine Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**LinkedIn yükseltmesine Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **LinkedIn yükseltmesine**seçin **LinkedIn yükseltmesine** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde LinkedIn Yükselt](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma adlı bir test kullanıcı tabanlı LinkedIn yükseltmesine ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili LinkedIn yükseltmesine kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma yükseltmesine LinkedIn ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[LinkedIn yükseltmesine çoklu oturum açmayı yapılandırma](#configure-linkedin-elevate-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[LinkedIn yükseltmesine test kullanıcısı oluşturma](#create-linkedin-elevate-test-user)**  - LinkedIn kullanıcı Azure AD gösterimini bağlı Yükselt Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

LinkedIn yükseltmesine ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **LinkedIn yükseltmesine** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![LinkedIn yükseltme etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna **varlık kimliği** değeri kopyaladığınız varlık kimliği değeri LinkedIn portalından Bu öğreticinin ilerleyen bölümlerinde açıklanmıştır.

    b. İçinde **yanıt URL'si** metin kutusuna **onaylama tüketici erişim (ACS) URL'si** değeri, LinkedIn portaldan değerini bu öğreticinin ilerleyen bölümlerinde açıklanan onaylama tüketici erişim (ACS) URL'sini kopyalayın.

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![LinkedIn yükseltme etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>`

6. LinkedIn yükseltmesine uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Varsayılan öznitelikler listesinde aşağıdaki ekran görüntüsünde gösterilmektedir oysa **NameIdentifier** ile eşlenmiş **user.userprincipalname**. LinkedIn yükseltmesine uygulama NameIdentifier ile eşlenmesini bekliyor **user.mail**, öznitelik eşlemesi düzenleme simgesine tıklayarak düzenleme ve öznitelik eşlemesini değiştirmek gerekmez.

    ![image](common/edit-attribute.png)

7. Yukarıdaki için Ayrıca, LinkedIn yükseltmesine uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. Kullanıcı taleplerini bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:

    | Ad | Kaynak özniteliği|
    | -------| -------------|
    | Bölüm | User.Department |

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

9. Üzerinde **LinkedIn yükseltmesine kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-linkedin-elevate-single-sign-on"></a>LinkedIn yapılandırma çoklu oturum açma Yükselt

1. Farklı bir web tarayıcı penceresinde LinkedIn yükseltmesine kiracınıza yönetici olarak oturum.

1. İçinde **hesap Merkezi**, tıklayın **genel ayarları** altında **ayarları**. Ayrıca, seçin **yükseltme - AAD Test yükseltmesine** aşağı açılan listeden.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_01.png)

1. Tıklayarak **veya yüklemek ve tek tek alanları formdan kopyalamak için burayı tıklatın** ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_03.png)

    a. Kopyalama **varlık kimliği** yapıştırın **tanımlayıcı** metin kutusu **temel SAML yapılandırma** Azure portalında.

    b. Kopyalama **onaylama tüketici erişim (ACS) URL'si** yapıştırın **yanıt URL'si** metin kutusu **temel SAML yapılandırma** Azure portalında.

1. Git **LinkedIn yönetici ayarları** bölümü. Azure portalından karşıya yükleme XML dosyası seçeneğine tıklayıp yüklediğiniz XML dosyasını karşıya yükleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial_linkedin_metadata_03.png)

1. Tıklayın **üzerinde** SSO'yu etkinleştirmek üzere. SSO durumu değişir **bağlı** için **bağlandı**

    ![Çoklu oturum açmayı yapılandırın](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, LinkedIn yükseltmek için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **LinkedIn yükseltmesine**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **LinkedIn yükseltmesine**.

    ![Uygulamalar listesinde LinkedIn Yükselt bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-linkedin-elevate-test-user"></a>LinkedIn yükseltmesine test kullanıcısı oluşturma

LinkedIn yükseltmesine uygulama zamanı kullanıcı sağlamayı ve kimlik doğrulaması kullanıcılar uygulamaya otomatik olarak oluşturulacak sonra sadece destekler. Yönetici ayarları LinkedIn yükseltmesine portal Çevir ' anahtar sayfasında **lisansları otomatik olarak ata** etkin tam zamanında sağlama ve bu da bir lisans kullanıcıya atar. LinkedIn yükseltmesine de destekler otomatik kullanıcı hazırlama, daha fazla ayrıntı bulabilirsiniz [burada](linkedinelevate-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

   ![Bir Azure AD test kullanıcısı oluşturma](./media/linkedinelevate-tutorial/LinkedinUserprovswitch.png)

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde LinkedIn yükseltmesine kutucuğa tıkladığınızda, otomatik olarak SSO'yu ayarlama LinkedIn yükseltmek için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)