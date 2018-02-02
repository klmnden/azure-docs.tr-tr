---
title: "Öğretici: Azure Active Directory Tümleştirme VERITAS Kurumsal Vault.cloud SSO | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile VERITAS Kurumsal Vault.cloud SSO arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: jeedes
ms.openlocfilehash: eb9243367d0817d37549fa147c6c5e1d2acf3761
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a>Öğretici: Azure Active Directory Tümleştirme VERITAS Kurumsal Vault.cloud SSO

Bu öğreticide, VERITAS Kurumsal Vault.cloud SSO Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

VERITAS Kurumsal Vault.cloud SSO Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- VERITAS Kurumsal Vault.cloud SSO erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Itanium tabanlı sistemler için VERITAS Kurumsal Vault.cloud SSO için (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme VERITAS Kurumsal Vault.cloud SSO'su yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir VERITAS Kurumsal Vault.cloud SSO çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden VERITAS Kurumsal Vault.cloud SSO ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-the-gallery"></a>Galeriden VERITAS Kurumsal Vault.cloud SSO ekleme
Azure AD VERITAS Kurumsal Vault.cloud SSO tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden VERITAS Kurumsal Vault.cloud SSO eklemeniz gerekir.

**Galeriden VERITAS Kurumsal Vault.cloud SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **VERITAS Kurumsal Vault.cloud SSO**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_search.png)

5. Sonuçlar panelinde seçin **VERITAS Kurumsal Vault.cloud SSO**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma VERITAS Kurumsal Vault.cloud "Britta Simon" adlı bir test kullanıcı tabanlı SSO ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen VERITAS Kurumsal Vault.cloud SSO bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı VERITAS Kurumsal Vault.cloud SSO arasında bir bağlantı ilişkisi kurulması gerekir.

VERITAS Kurumsal Vault.cloud SSO değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma VERITAS Kurumsal Vault.cloud SSO sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[VERITAS Kurumsal Vault.cloud SSO test kullanıcısı oluşturma](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı VERITAS Kurumsal Vault.cloud SSO sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma VERITAS Kurumsal Vault.cloud SSO uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma VERITAS Kurumsal Vault.cloud SSO'su yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **VERITAS Kurumsal Vault.cloud SSO** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_samlbase.png)

3. Üzerinde **VERITAS Kurumsal Vault.cloud SSO etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`

    b. İçinde **tanımlayıcısı** metin kutusuna, veri merkezi başına URL'yi kullanın

    | Veri merkezi| URL |
    |----------|----|
    | Kuzey Amerika| `https://auth.lax.archivecloud.net` |
    | Avrupa | `https://auth.ams.archivecloud.net` |
    | Asya Pasifik| `https://auth.syd.archivecloud.net`|

    c. İçinde **yanıt URL'si** metin kutusuna, veri merkezi başına URL'yi kullanın

    | Veri merkezi| URL |
    |----------|----|
    | Kuzey Amerika| `https://auth.lax.archivecloud.net` |
    | Avrupa | `https://auth.ams.archivecloud.net` |
    | Asya Pasifik| `https://auth.syd.archivecloud.net`|
    
    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [VERITAS Kurumsal Vault.cloud SSO istemci destek ekibi](https://www.veritas.com/support/.html) bu değeri alınamıyor. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-veritas-tutorial/tutorial_general_400.png)

6. Üzerinde **VERITAS Kurumsal Vault.cloud SSO yapılandırma** 'yi tıklatın **yapılandırma VERITAS Kurumsal Vault.cloud SSO** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_configure.png) 

7. Çoklu oturum açma yapılandırmak için **VERITAS Kurumsal Vault.cloud SSO** yan, indirilen göndermek için ihtiyacınız **Certificate(Base64)** ve **SAML çoklu oturum açma hizmet URL'si** için [VERITAS Kurumsal Vault.cloud SSO destek ekibi](https://www.veritas.com/support/.html).

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-veritas-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-veritas-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-veritas-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-veritas-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-veritas-enterprise-vaultcloud-sso-test-user"></a>VERITAS Kurumsal Vault.cloud SSO test kullanıcısı oluşturma

Bu bölümde, Kurumsal Vault.cloud SSO Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [VERITAS Kurumsal Vault.cloud SSO destek ekibi](https://www.veritas.com/support/.html) Kurumsal Vault.cloud SSO platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta VERITAS Kurumsal Vault.cloud SSO için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**VERITAS Kurumsal Vault.cloud SSO Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **VERITAS Kurumsal Vault.cloud SSO**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli VERITAS Kurumsal Vault.cloud SSO parçasında tıklattığınızda, otomatik olarak VERITAS Kurumsal Vault.cloud SSO uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_203.png

