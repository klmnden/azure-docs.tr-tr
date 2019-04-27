---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Replicon | Microsoft Docs'
description: Azure Active Directory ve Replicon arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ade40287bd38580a1e3f6377e54017bfe92bf452
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60527072"
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Öğretici: Replicon ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Replicon tümleştirme konusunda bilgi edinin.

Azure AD ile Replicon tümleştirme ile aşağıdaki avantajları sağlar:

- Replicon erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Replicon (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Replicon yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Replicon çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Replicon ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-replicon-from-the-gallery"></a>Galeriden Replicon ekleme
Azure AD'de Replicon tümleştirmesini yapılandırmak için Replicon Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Replicon eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Replicon**seçin **Replicon** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde replicon](./media/replicon-tutorial/tutorial_replicon_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Replicon sınayın.

Tek iş için oturum açma için Azure AD ne Replicon karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Replicon ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Replicon içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Replicon ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Replicon test kullanıcısı oluşturma](#create-a-replicon-test-user)**  - kullanıcı Azure AD gösterimini bağlı Replicon Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Replicon uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Replicon yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Replicon** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/replicon-tutorial/tutorial_replicon_samlbase.png)

3. Üzerinde **Replicon etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Replicon etki alanı ve URL'ler tek oturum açma bilgileri](./media/replicon-tutorial/tutorial_replicon_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://na2.replicon.com/<companyname>/saml2/sp-sso/post`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://global.replicon.com/<companyname>`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://global.replicon.com/!/saml2/<companyname>/sso/post`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcıya ve yanıt URL'si ile güncelleştirin. İlgili kişi [Replicon istemci Destek ekibine](https://www.replicon.com/customerzone/contact-support) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/replicon-tutorial/tutorial_replicon_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/replicon-tutorial/tutorial_general_400.png)

6. Farklı bir web tarayıcı penceresinde Replicon şirket sitenize yönetici olarak oturum.

7. SAML 2.0 yapılandırmak için aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik doğrulamasını etkinleştirme](./media/replicon-tutorial/ic777805.png "etkinleştirme SAML kimlik doğrulaması")

    a. Görüntülenecek **EnableSAML Authentication2** iletişim kutusunda, aşağıdaki URL'nize, sonra şirket anahtarınızı ekleyin: `/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

    * Tam URL şeması aşağıda gösterilmiştir: `https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

   b. Tıklayın **+** genişletmek için **v20Configuration** bölümü.

   c. Tıklayın **+** genişletmek için **metaDataConfiguration** bölümü.

   d. Tıklayın **Dosya Seç**, kimlik sağlayıcısı meta verileri XML dosyanızı seçin ve tıklayın **Gönder**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/replicon-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/replicon-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/replicon-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/replicon-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-replicon-test-user"></a>Replicon test kullanıcısı oluşturma

Bu bölümün amacı Replicon Britta Simon adlı bir kullanıcı oluşturmaktır.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. Bir web tarayıcısı penceresinde Replicon şirket sitenize yönetici olarak oturum.

2. Git **Yönetim \> kullanıcılar**.

    ![Kullanıcılar](./media/replicon-tutorial/ic777806.png "kullanıcılar")

3. Tıklayın **+ Kullanıcı Ekle**.

    ![Kullanıcı ekleme](./media/replicon-tutorial/ic777807.png "kullanıcı ekleme")

4. İçinde **kullanıcı profili** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı profili](./media/replicon-tutorial/ic777808.png "kullanıcı profili")

    a. İçinde **oturum açma adı** metin türü Azure AD e-posta adresi gibi sağlamak istediğiniz Azure AD kullanıcısının **BrittaSimon\@contoso.com**.

    b. Olarak **kimlik doğrulama türü**seçin **SSO**.

    c. İçinde **departmanı** metin kutusu, kullanıcının bölüm adını yazın.

    d. Olarak **çalışan türü**seçin **yönetici**.

    e. Tıklayın **kullanıcı profili Kaydet**.

>[!NOTE]
>Herhangi diğer Replicon kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için Replicon tarafından sağlanan.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Replicon erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Replicon için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Replicon**.

    ![Uygulamalar listesinde Replicon bağlantı](./media/replicon-tutorial/tutorial_replicon_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Replicon kutucuğa tıkladığınızda, otomatik olarak Replicon uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/replicon-tutorial/tutorial_general_01.png
[2]: ./media/replicon-tutorial/tutorial_general_02.png
[3]: ./media/replicon-tutorial/tutorial_general_03.png
[4]: ./media/replicon-tutorial/tutorial_general_04.png

[100]: ./media/replicon-tutorial/tutorial_general_100.png

[200]: ./media/replicon-tutorial/tutorial_general_200.png
[201]: ./media/replicon-tutorial/tutorial_general_201.png
[202]: ./media/replicon-tutorial/tutorial_general_202.png
[203]: ./media/replicon-tutorial/tutorial_general_203.png
