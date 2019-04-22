---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Hightail | Microsoft Docs'
description: Azure Active Directory ve Hightail arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b57786aaf699f562743bfdd6cf5c2d8f1b41b0d7
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59276091"
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Öğretici: Hightail ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Hightail tümleştirme konusunda bilgi edinin.
Azure AD ile Hightail tümleştirme ile aşağıdaki avantajları sağlar:

* Hightail erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Hightail için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Hightail yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Çoklu oturum açma etkin abonelik hightail

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Destekler hightail **SP ve IDP** tarafından başlatılan
* Destekler hightail **zamanında** kullanıcı sağlama

## <a name="adding-hightail-from-the-gallery"></a>Galeriden Hightail ekleme

Azure AD'de Hightail tümleştirmesini yapılandırmak için Hightail Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Hightail eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Hightail**seçin **Hightail** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuçlar listesinde hightail](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Hightail adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Hightail ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Hightail ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Hightail çoklu oturum açmayı yapılandırma](#configure-hightail-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Hightail test kullanıcısı oluşturma](#create-hightail-test-user)**  - kullanıcı Azure AD gösterimini bağlı Hightail Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Hightail yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Hightail** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** modunda başlatılan aşağıdaki adımı uygulayın:

    ![Etki alanı ve URL'ler tek oturum açma bilgileri hightail](common/both-replyurl.png)

    İçinde **yanıt URL'si** metin kutusunda, URL olarak girin:  `https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`

    > [!NOTE]
    > Yanıt URL'si değeri, gerçek değer değil. Yanıt URL'si değeri, bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek yanıt URL'si ile güncelleştirir.

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Etki alanı ve URL'ler tek oturum açma bilgileri hightail](common/both-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusunda, URL olarak girin:  `https://www.hightail.com/loginSSO`

6. Hightail uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** kullanıcı öznitelikleri iletişim kutusunu açmak için simge.

    ![image](common/edit-attribute.png)

7. Buna ek olarak yukarıdaki uygulama Hightail için SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:

    | Ad | Kaynak özniteliği|
    | -------- |-------- |
    | FirstName | User.givenName |
    | LastName | User.surname |
    | Email | User.Mail |
    | Userıdentity | User.Mail |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

9. Üzerinde **Hightail kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

    > [!NOTE]
    > Çoklu oturum açma sırasında Hightail uygulama yapılandırmadan önce lütfen beyaz liste Hightail ile e-posta etki alanınıza team böylece bu etki alanı kullanan tüm kullanıcılar, çoklu oturum açma işlevini kullanabilirsiniz.

### <a name="configure-hightail-single-sign-on"></a>Yapılandırma Hightail çoklu oturum açma

1. Başka bir tarayıcı penceresinde açmak **Hightail** Yönetim Portalı.

2. Tıklayarak **kullanıcı simgesi** sayfanın sağ üst köşesindeki öğesinden. 

    ![Çoklu oturum açmayı yapılandırın](./media/hightail-tutorial/configure1.png)

3. Tıklayın **görünümü Yönetici Konsolu** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/hightail-tutorial/configure2.png)

4. Üstteki menüden **SAML** sekmesinde ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/hightail-tutorial/configure3.png)

    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si** Azure portaldan kopyaladığınız.

    b. Base-64 kodlanmış sertifikanızı Azure portalından indirdiğiniz Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **SAML sertifikası** metin.

    c. Tıklayın **kopyalama** Örneğiniz için SAML tüketici URL'yi kopyalayın ve yapıştırın **yanıt URL'si** metin kutusunda **temel SAML yapılandırma** bölümü Azure portalı.

    d. Tıklayın **yapılandırmaları kaydetme**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Hightail erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Hightail**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Hightail**.

    ![Uygulamalar listesinde Hightail bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-hightail-test-user"></a>Hightail test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Hightail oluşturulur. Just-ın-time kullanıcı sağlamayı destekler, varsayılan olarak etkin olduğu hightail. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı Hightail içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [Hightail Destek ekibine](mailto:support@hightail.com).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Hightail kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Hightail için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)