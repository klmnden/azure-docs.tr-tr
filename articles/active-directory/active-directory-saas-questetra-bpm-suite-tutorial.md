---
title: 'Öğretici: Azure Active Directory Tümleştirme Questetra BPM Suite ile | Microsoft Docs'
description: Çoklu oturum açma Questetra BPM Suite ile Azure Active Directory arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: 122473da723cb101e0f0f9226b34aa3294477657
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Öğretici: Azure Active Directory Tümleştirme Questetra BPM Suite ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Questetra BPM Suite tümleştirmek öğrenin.

Questetra BPM Suite Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Questetra BPM Suite erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Questetra BPM paketine açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Questetra BPM paketiyle yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Questetra BPM Suite çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Questetra BPM paketi ekleme
2. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-questetra-bpm-suite-from-the-gallery"></a>Galeriden Questetra BPM paketi ekleme
Azure AD Questetra BPM Suite tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Questetra BPM paketi eklemeniz gerekir.

**Galeriden Questetra BPM paketi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Questetra BPM Suite**seçin **Questetra BPM Suite** sonuç paneli ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Galerisi'nden ekleme](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Questetra BPM "Britta Simon" adlı bir test kullanıcı tabanlı Suite ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Questetra BPM grubundaki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Questetra BPM paketindeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Questetra BPM paketindeki atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Questetra BPM Suite ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Questetra BPM Suite test kullanıcısı oluşturma](#create-a-questetra-bpm-suite-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Questetra BPM Suite sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Questetra BPM Suite uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Questetra BPM paketiyle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Questetra BPM Suite** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![SAML Tabanlı Oturum Açma](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_samlbase.png)

3. Üzerinde **Questetra BPM Suite etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Questetra BPM Suite etki alanı ve URL'ler bölümü](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.questetra.net/saml/SSO/alias/bpm`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.questetra.net/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Bu değerleri alabilirsiniz **SP bilgi** bölümünde, **Questetra BPM Suite** daha sonra öğreticide veya kişi içinde açıklanan şirket site [Questetra BPM Suite istemci desteği Takım](https://www.questetra.com/contact/). 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base 64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![SAML imzalama sertifikası bölümü](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Kaydet Düğmesi](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_400.png)

6. Üzerinde **Questetra BPM paketi yapılandırma** 'yi tıklatın **Questetra BPM paketi yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Questetra BPM paketi yapılandırma bölümü](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_configure.png) 

7. Farklı web tarayıcısı penceresinde oturum açın, **Questetra BPM Suite** yönetici olarak şirket site.

8. Üstteki menüde tıklatın **sistem ayarlarını**. 
   
    ![Azure AD çoklu oturum açma][10]

9. Açmak için **SingleSignOnSAML** sayfasında, **SSO (SAML)**. 
   
    ![Azure AD çoklu oturum açma][11]

10. Üzerinde **Questetra BPM Suite** site, şirket içinde **SP bilgi** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. Kopya **ACS URL**ve ardından yapıştırın **oturum üzerinde URL'si** metin kutusuna **Questetra BPM Suite etki alanı ve URL'leri** Azure portalından bölümü.
    
    b. Kopya **varlık kimliği**ve ardından yapıştırın **tanımlayıcısı** metin kutusuna **Questetra BPM Suite etki alanı ve URL'leri** Azure portalından bölümü.

11. Üzerinde **Questetra BPM Suite** şirket site, aşağıdaki adımları gerçekleştirin: 
   
    ![Çoklu oturum açmayı yapılandırın][15]
   
    a. Seçin **çoklu oturum açmayı etkinleştir**.
   
    b. İçinde **varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.
    
    c. İçinde **oturum açma sayfası URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
    
    d. İçinde **oturum kapatma sayfası URL'si** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.
    
    e. İçinde **NameID biçimi** metin kutusuna, türü `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    f. Açık, **Base-64** Not Defteri'nde kodlanmış sertifika indirilen Azure portalından, içeriğini, panoya kopyalayın ve ardından yapıştırın **doğrulama sertifikası** metin kutusu. 

    g. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-questetra-bpm-suite-test-user"></a>Questetra BPM Suite test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon Questetra BPM paketindeki adlı bir kullanıcı oluşturmaktır.

**Britta Simon Questetra BPM paketindeki adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Questetra BPM Suite şirket sitenize yönetici olarak oturum açma.
2. Git **sistem ayarlarını > kullanıcı listesi > Yeni kullanıcı**. 
3. Yeni kullanıcı iletişim kutusunda, aşağıdaki adımları gerçekleştirin: 
   
    ![Test kullanıcısı oluşturma][300] 
   
    a. İçinde **adı** metin kutusuna, türü **adı** kullanıcının **britta.simon@contoso.com**.
   
    b. İçinde **e-posta** metin kutusuna, türü **e-posta** kullanıcının **britta.simon@contoso.com**
   
    c. İçinde **parola** metin kutusuna, bir **parola** kullanıcının.
    
    d. Tıklatın **yeni kullanıcı Ekle**.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Questetra BPM paketine erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Questetra BPM paketine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Questetra BPM Suite**.

    ![Uygulamalar listesinde Questetra BPM paketi](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Questetra BPM Suite parçasında tıklattığınızda, otomatik olarak Questetra BPM Suite uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[15]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_08.png

[100]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_203.png
[300]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 

