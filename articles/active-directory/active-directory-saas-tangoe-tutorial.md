---
title: "Öğretici: Azure Active Directory Tümleştirme Tangoe komutu Premium Mobile ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Tangoe komutu Premium mobil arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 595541e7248a7486e58123927389c552af0e50f7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Öğretici: Azure Active Directory Tümleştirme Tangoe komutu Premium Mobile ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Tangoe komutu Premium mobil tümleştirmek öğrenin.

Tangoe komutu Premium mobil Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tangoe komutu Premium mobil erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Itanium tabanlı sistemler için Tangoe komutu Premium Mobile için (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Tangoe komutu Premium Mobile ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Tangoe komutu Premium mobil çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Tangoe komutu Premium mobil ekleme
2. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-tangoe-command-premium-mobile-from-the-gallery"></a>Galeriden Tangoe komutu Premium mobil ekleme
Azure AD Tangoe komutu Premium mobil tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Tangoe komutu Premium mobil eklemeniz gerekir.

**Galeriden Tangoe komutu Premium mobil eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Tangoe komutu Premium mobil**seçin **Tangoe komutu Premium mobil** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Tangoe komutu Premium mobil Galerisi'nden ekleme ](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Tangoe komutu Premium "Britta Simon" adlı bir test kullanıcı tabanlı Mobile ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Tangoe komutu Premium Mobile'da bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Tangoe komutu Premium Mobile'da arasında bir bağlantı ilişkisi kurulması gerekir.

Tangoe komutu Premium Mobile'da değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Tangoe komutu Premium Mobile ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Tangoe komutu Premium mobil test kullanıcısı oluşturma](#create-a-tangoe-command-premium-mobile-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Tangoe komutu Premium mobil sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Tangoe komutu Premium mobil uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Tangoe komutu Premium Mobile ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Tangoe komutu Premium mobil** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![SAML tabanlı oturum açma](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_samlbase.png)

3. Üzerinde **Tangoe komutu Premium mobil etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Tangoe komutu Premium mobil etki alanı ve URL'leri](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://sso.tangoe.com/sp/ACS.saml2`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Tangoe komutu Premium mobil istemci destek ekibi](https://www.tangoe.com/contact-2/) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![SAML imzalama sertifikası bölümü](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Kaydet düğmesi](./media/active-directory-saas-tangoe-tutorial/tutorial_general_400.png)
    
6. Üzerinde **Tangoe komutu Premium mobil yapılandırma** 'yi tıklatın **yapılandırma Tangoe komutu Premium mobil** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Tangoe komutu Premium mobil yapılandırma bölümü](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_configure.png) 

7. Uygulamanız için yapılandırılmış SSO almak için başvurun, [Tangoe komutu Premium mobil istemci destek ekibi](https://www.tangoe.com/contact-2/) ve şunları sağlar:

   - İndirilen meta veri dosyası
   - **SAML varlık kimliği**
   - **SAML çoklu oturum açma hizmeti URL'si**
   - **Oturum kapatma URL'si**

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tangoe-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Kullanıcıların ve grupların tüm kullanıcılar ->](./media/active-directory-saas-tangoe-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Kullanıcı ekle](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim sayfası](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-tangoe-command-premium-mobile-test-user"></a>Tangoe komutu Premium mobil test kullanıcısı oluşturma

Bu bölümde, Britta Simon Tangoe komutu Premium Mobile'da adlı bir kullanıcı oluşturun. 

Tangoe komutu Premium mobil uygulama tüm kullanıcılar tekli oturum açma işleminden önce uygulamayı sağlanması gerekir. İş, bu nedenle Lütfen ile [Tangoe komutu Premium mobil istemci destek ekibi](https://www.tangoe.com/contact-2/) bu kullanıcıların uygulamaya sağlamak için. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Tangoe komutu Premium mobil erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Tangoe komutu Premium mobil Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Tangoe komutu Premium mobil**.

    ![Tangoe komutu Premium mobil uygulama listesinde](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD SSO yapılandırmanızı sınayın.

Erişim paneli Tangoe komutu Premium mobil parçasında tıklattığınızda, otomatik olarak Tangoe komutu Premium mobil uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png

