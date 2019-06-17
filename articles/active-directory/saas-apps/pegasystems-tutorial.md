---
title: 'Öğretici: Pega sistemler ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Bu öğreticide, Azure Active Directory ve Pega sistemleri arasında çoklu oturum açmayı yapılandırma öğreneceksiniz.
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
ms.openlocfilehash: 013e477b66d2772698ce5c9cc61a59f8a5a04a5a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67094893"
---
# <a name="tutorial-azure-active-directory-integration-with-pega-systems"></a>Öğretici: Pega sistemler ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Pega sistemlerini tümleştirin öğreneceksiniz.

Bu tümleştirme aşağıdaki avantajları sağlar:

* Azure AD Pega sistemlerine erişimi denetlemek için kullanabilirsiniz.
* Otomatik olarak Pega sistemlerine (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure aboneliğiniz yoksa, [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Pega sistemleriyle yapılandırmak için sahip olmanız gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, oturum açabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Tekli etkin oturum sahip Pega sistemleri aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Pega sistemleri SP tarafından başlatılan ve IDP tarafından başlatılan SSO'yu destekler.

## <a name="add-pega-systems-from-the-gallery"></a>Galeriden Pega sistemleri Ekle

Azure AD'de Pega sistemlerinin tümleştirmeyi ayarlamak için yönetilen SaaS uygulamaları listenize Galeriden Pega sistemleri eklemeniz gerekir.

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

    ![Yeni uygulama seçme](common/add-new-app.png)

4. Arama kutusuna **Pega sistemleri**. Seçin **Pega sistemleri** seçin ve arama sonuçlarını **Ekle**.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma Pega sistemlerle Britta Simon adlı bir test kullanıcısı kullanarak test edin.
Çoklu oturum açmayı etkinleştirmek için Pega sistemlerinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir ilişki yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Pega sistemleri ile test etmek için bu adımları tamamlamak gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)**  kullanıcılarınız için özelliği etkinleştirmek için.
2. **[Pega sistemleri çoklu oturum açmayı yapılandırma](#configure-pega-systems-single-sign-on)**  uygulama tarafında.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açmayı test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açma için kullanıcı etkinleştirmek için.
5. **[Pega sistemleri test kullanıcısı oluşturma](#create-a-pega-systems-test-user)**  kullanıcı Azure AD gösterimini bağlı.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirmeniz.

Azure AD çoklu oturum açma Pega sistemleriyle yapılandırmak için şu adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **Pega sistemleri** uygulama tümleştirme sayfasında **çoklu oturum açma**:

    ![Çoklu oturum açma seçin](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusunda:

    ![Düzenle simgesi](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** iletişim kutusu, IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları tamamlayın.

    ![Temel SAML yapılandırma iletişim kutusu](common/idp-intiated.png)

    1. İçinde **tanımlayıcı** kutusuna, bu düzende bir URL girin:

       `https://<customername>.pegacloud.io:443/prweb/sp/<instanceID>`

    1. İçinde **yanıt URL'si** kutusuna, bu düzende bir URL girin:

       `https://<customername>.pegacloud.io:443/prweb/PRRestService/WebSSO/SAML/AssertionConsumerService`

5. SP tarafından başlatılan modunda uygulama yapılandırmak isteyip istemediğinizi seçin **ek URL'lerini ayarlayın** ve aşağıdaki adımları tamamlayın.

    ![Pega sistemleri etki alanı ve URL'ler tek oturum açma bilgileri](common/both-advanced-urls.png)

    1. İçinde **oturum açma URL'si** kutusunda, oturum açma URL değeri girin.

    1. İçinde **geçiş durumu** kutusuna, bu düzende bir URL girin: `https://<customername>.pegacloud.io/prweb/sso`

    > [!NOTE]
    > Değerleri burada yer tutucuları sağlanır. İhtiyacınız olan gerçek tanımlayıcısı kullanın, yanıt URL'si, URL ve geçişi durumu URL'si. Tanımlayıcısını alın ve bu öğreticinin ilerleyen bölümlerinde açıklandığı gibi bir Pega uygulamasından URL'si değerleri yanıtla. Geçiş durumu değerini almak için iletişime geçin [Pega sistemleri Destek ekibine](https://www.pega.com/contact-us). Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Pega sistemleri uygulamanın SAML onaylamalarını belirli bir biçiminde olması gerekir. Bunları doğru biçimde almak için SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini eklemeniz gerekir. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler gösterilmiştir. Seçin **Düzenle** açmak için simgeyi **kullanıcı öznitelikleri** iletişim kutusunda:

    ![Kullanıcı öznitelikleri](common/edit-attribute.png)

7. Önceki ekran görüntüsünde gösterilen özniteliklere ek olarak SAML yanıtta geçirilecek birkaç daha fazla öznitelik Pega sistemleri uygulamanın gerektirir. İçinde **kullanıcı taleplerini** bölümünü **kullanıcı öznitelikleri** iletişim kutusunda, bu SAML belirteci öznitelikleri eklemek için aşağıdaki adımları tamamlayın:

    
   - `uid`
   - `cn`
   - `mail`
   - `accessgroup`  
   - `organization`  
   - `orgdivision`
   - `orgunit`
   - `workgroup`  
   - `Phone`

    > [!NOTE]
    > Bu değerler, kuruluşunuz için özeldir. Uygun değerleri sağlayın.

    1. Seçin **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim kutusunda:

    ![Yeni Talep Ekle seçin](common/new-save-attribute.png)

    ![Kullanıcı talepleri iletişim kutusu yönetme](common/new-attribute-details.png)

    1. İçinde **adı** kutusunda, ilgili satır için gösterilen öznitelik adını girin.

    1. Bırakın **Namespace** kutusunu boş.

    1. İçin **kaynak**seçin **özniteliği**.

    1. İçinde **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri seçin.

    1. **Tamam**’ı seçin.

    1. **Kaydet**’i seçin.

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** yanındaki bağlantı **Federasyon meta veri XML** , gereksinimlerinize göre ve bilgisayarınızdaki sertifika Kaydet:

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

9. İçinde **Pega sistemi'kurmak** bölümünde, gereksinimlerinize göre uygun URL'ler kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    1. **Oturum açma URL'si**.

    1. **Azure AD tanımlayıcısı**.

    1. **Oturum kapatma URL'si**.

### <a name="configure-pega-systems-single-sign-on"></a>Pega sistemleri çoklu oturum açmayı yapılandırın

1. Çoklu oturum açmayı yapılandırma **Pega sistemleri** yan, Pega portalında oturum başka bir tarayıcı penceresinde bir yönetici hesabıyla.

2. Seçin **oluşturma** > **SysAdmin** > **kimlik doğrulama hizmeti**:

    ![Kimlik doğrulama hizmeti seçin](./media/pegasystems-tutorial/tutorial_pegasystems_admin.png)
    
3. Aşağıdaki adımları tamamlayın **oluşturma kimlik doğrulama hizmeti** ekran.

    ![Kimlik doğrulama hizmeti ekran oluşturma](./media/pegasystems-tutorial/tutorial_pegasystems_admin1.png)

    1. İçinde **türü** listesinden **SAML 2.0**.

    1. İçinde **adı** kutusuna, herhangi bir ad girin (örneğin, **Azure AD SSO**).

    1. İçinde **kısa açıklama** kutusunda, bir açıklama girin.  

    1. Seçin **oluştur ve açık**.
    
4. İçinde **kimlik sağlayıcıyı (IDP) bilgi** bölümünden **IDP alma meta verileri** ve Azure portalından indirdiğiniz meta veri dosyasına göz atın. Tıklayın **Gönder** meta verileri yüklemek için:

    ![Kimlik sağlayıcısı (IDP) bilgi bölümü](./media/pegasystems-tutorial/tutorial_pegasystems_admin2.png)
    
    IDP verilerini içeri aktarma burada gösterildiği gibi doldurur:

    ![İçeri aktarılan IDP verileri](./media/pegasystems-tutorial/tutorial_pegasystems_admin3.png)
    
6. Aşağıdaki tüm adımları **hizmet sağlayıcısı (SP) ayarları** bölümü.

    ![Hizmet sağlayıcısı ayarları](./media/pegasystems-tutorial/tutorial_pegasystems_admin4.png)

    1. Kopyalama **varlık kimliği** yapıştırın ve değer **tanımlayıcı** kutusunda **temel SAML yapılandırma** bölümünde Azure portalında.

    1. Kopyalama **onaylama tüketici hizmeti (ACS) konumu** yapıştırın ve değer **yanıt URL'si** kutusunda **temel SAML yapılandırma** bölümünde Azure portalında.

    1. Seçin **imzalama isteğini devre dışı**.

7. **Kaydet**’i seçin.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure portalında **Azure Active Directory** seçin sol bölmede **kullanıcılar**ve ardından **tüm kullanıcılar**:

    ![Tüm kullanıcıları seçin](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üst kısmındaki:

    ![Yeni bir kullanıcı seçin](common/new-user.png)

3. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları tamamlayın.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** kutusuna **@ brittasimon\<yourcompanydomain >.\< Uzantı >** . (Örneğin, BrittaSimon@contoso.com.)

    c. Seçin **Show parola**ve ardından içinde bir değer yazın **parola** kutusu.

    d. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Pega sisteme erişim vererek kullanmak Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **Pega sistemleri**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde seçin **Pega sistemleri**.

    ![Uygulamaların listesi](common/all-applications.png)

3. Sol bölmede seçin **kullanıcılar ve gruplar**:

    ![Kullanıcıları ve grupları seçin](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Kullanıcı Ekle seçeneğini belirleme](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıların listesini ve ardından **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması rol değeri de beklediğiniz **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Tıklayın **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="create-a-pega-systems-test-user"></a>Pega sistemleri test kullanıcısı oluşturma

Sonra Britta Simon Pega sistemlerinde adlı bir kullanıcı oluşturmanız gerekir. Çalışmak [Pega sistemleri Destek ekibine](https://www.pega.com/contact-us) kullanıcıları oluşturmak için.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Şimdi Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test gerekir.

Erişim Paneli'nde Pega sistemleri kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Pega sistemleri örneği için oturum açmanız. Daha fazla bilgi için [erişim ve kullanım uygulamaları uygulamalarım portalında](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)