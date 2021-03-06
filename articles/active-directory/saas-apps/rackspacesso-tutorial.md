---
title: 'Öğretici: Rackspace SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Rackspace SSO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 36b398be-2f7e-4ce8-9031-53587299bc4a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.openlocfilehash: 31826f5d4d88c977f859a009bface2fddf3a1c88
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67093191"
---
# <a name="tutorial-azure-active-directory-integration-with-rackspace-sso"></a>Öğretici: Rackspace SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Rackspace SSO tümleştirme konusunda bilgi edinin.
Rackspace SSO, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Rackspace SSO erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Rackspace SSO (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Rackspace SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik rackspace SSO çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Rackspace SSO'yu destekler **SP** tarafından başlatılan

## <a name="adding-rackspace-sso-from-the-gallery"></a>Galeriden rackspace SSO ekleme

Azure AD'de Rackspace SSO tümleştirmesini yapılandırmak için Rackspace SSO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden rackspace SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Rackspace SSO**seçin **Rackspace SSO** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde rackspace SSO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Rackspace SSO ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**.
Çoklu oturum açma ile Rackspace kullanırken, Rackspace kullanıcılar Rackspace portalda oturum ilk kez otomatik olarak oluşturulur. 

Yapılandırma ve Azure AD çoklu oturum açma Rackspace SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Rackspace SSO çoklu oturum açmayı yapılandırma](#configure-rackspace-sso-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Öznitelik eşlemesi Rackspace Denetim Masası'nda ayarlama](#set-up-attribute-mapping-in-the-rackspace-control-panel)**  - Azure AD kullanıcılarını Rackspace rolleri atamak için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Rackspace SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Rackspace SSO** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, karşıya yükleme **hizmet sağlayıcısı meta veri dosyası** karşıdan yükleyebileceğiniz [URL](https://login.rackspace.com/federate/sp.xml) ve aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![image](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![image](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra gereken URL'leri otomatik olarak doldurulur otomatik alın.

    d. İçinde **oturum açma URL'si** metin kutusuna bir URL yazın: `https://login.rackspace.com/federate/`

    ![Rackspace SSO etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)   

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

Bu dosya, gerekli Kimlik Federasyonu yapılandırma ayarlarını doldurmak için Rackspace için yüklenir.

### <a name="configure-rackspace-sso-single-sign-on"></a>Rackspace SSO çoklu oturum açmayı yapılandırın

Çoklu oturum açmayı yapılandırma **Rackspace SSO** yan:

1. Belgelerine bakın [Denetim Masası'na bir kimlik sağlayıcısı ekleyin](https://developer.rackspace.com/docs/rackspace-federation/gettingstarted/add-idp-cp/)
1. Bu adımları yol göstereceğiz:
    1. Yeni bir kimlik sağlayıcısı oluşturma
    1. Kullanıcıların oturum açarken şirketinizin tanımlamak için kullanacağı bir e-posta etki alanı belirtin.
    1. Karşıya yükleme **Federasyon meta verileri XML** daha önce Azure Denetim Masası'ndan indirilen.

Bu, Azure ve Rackspace bağlanmak için gereken temel SSO ayarları doğru şekilde yapılandıracaksınız.

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

Bu bölümde, Rackspace SSO için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Rackspace SSO**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Rackspace SSO**.

    ![Uygulamalar listesinde Rackspace SSO bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="set-up-attribute-mapping-in-the-rackspace-control-panel"></a>Eşleme özniteliği Rackspace Denetim Masası'nda ayarlayın

Rackspace kullanan bir **özniteliği eşleme ilkesi** Rackspace rollerin ve grupların çoklu oturum açmayı kullanıcılara atamak için. **Özniteliği eşleme ilkesi** Azure AD SAML talep Rackspace gerektirir kullanıcı yapılandırma alanlarına çevirir. Daha fazla belge içinde Rackspace bulunabilir [öznitelik eşlemesi temelleri belgeleri](https://developer.rackspace.com/docs/rackspace-federation/attribmapping-basics/). Bazı önemli noktalar:

* Çeşitli izin düzeyleriyle Azure AD gruplarını kullanarak Rackspace erişimi atamak istiyorsanız, azure'da grupları talep etkinleştirmeniz gerekecek **Rackspace SSO** çoklu oturum açma ayarları. **Özniteliği eşleme ilkesi** istenen Rackspace rollerin ve grupların bu gruplara eşleştirmek için kullanılacak:

    ![Grup ayarları talep](common/sso-groups-claim.png)

* Varsayılan olarak, Azure AD grubunun adı yerine SAML talep UID Azure AD grupları gönderir. Ancak, şirket içi Active Directory'nizi Azure ad eşitleme, grupları gerçek adlarını gönderme seçeneği vardır:

    ![Grup adı ayarları talep](common/sso-groups-claims-names.png)

Aşağıdaki örnek **özniteliği eşleme ilkesi** gösterir:
1. Rackspace kullanıcının adını ayarını `user.name` SAML talep. Herhangi bir talep kullanılabilir, ancak bu kullanıcının e-posta adresini içeren bir alana ayarlamak için yaygın olarak kullanılır.
1. Rackspace rolleri ayarlama `admin` ve `billing:admin` grubu adı ya da Grup UID tarafından bir Azure AD grubu eşleşen tarafından bir kullanıcı. A *değiştirme* , `"{0}"` içinde `roles` alan kullanılır ve sonuçlarına göre değiştirilecek `remote` kural ifadeler.
1. Kullanarak `"{D}"` *varsayılan değiştirme* SAML exchange standart ve iyi bilinen SAML Taleplerde arayarak ek SAML alanları almak Rackspace izin vermek için.

```yaml
---
mapping:
    rules:
    - local:
        user:
          domain: "{D}"
          name: "{At(http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name)}"
          email: "{D}"
          roles:
              - "{0}"
          expire: "{D}"
      remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.microsoft.com/ws/2008/06/identity/claims/groups')='7269f9a2-aabb-9393-8e6d-282e0f945985') then ('admin', 'billing:admin') else (),
                if (mapping:get-attributes('http://schemas.microsoft.com/ws/2008/06/identity/claims/groups')='MyAzureGroup') then ('admin', 'billing:admin') else ()
              )
            multiValue: true
  version: RAX-1
```
> [!TIP]
> YAML söz dizimi, ilke dosyasını düzenlerken doğrulayan bir metin düzenleyicisi kullandığınızdan emin olun.

Rackspace bkz [öznitelik eşlemesi temelleri belgeleri](https://developer.rackspace.com/docs/rackspace-federation/attribmapping-basics/) daha fazla örnek için.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Rackspace SSO kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Rackspace SSO için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

Ayrıca **doğrulama** düğmesine **Rackspace SSO** çoklu oturum açma ayarları:

   ![SSO doğrula düğmesi](common/sso-validate-sign-on.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

