---
title: "Öğretici: Azure Active Directory Tümleştirme Softeon WMS ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Softeon WMS arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 07c5de0d-90aa-43b3-b24e-0cc334b2f9b0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jeedes
ms.openlocfilehash: fa0897c89d01a6e4ada867b6b8e47c95f2988e65
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-softeon-wms"></a>Öğretici: Azure Active Directory Tümleştirme Softeon WMS ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Softeon WMS tümleştirmek öğrenin.

Softeon WMS Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Softeon WMS erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Softeon WMS açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Softeon WMS ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Softeon WMS çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Softeon WMS ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-softeon-wms-from-the-gallery"></a>Galeriden Softeon WMS ekleme
Azure AD Softeon WMS tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Softeon WMS eklemeniz gerekir.

**Galeriden Softeon WMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Softeon WMS**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_search.png)

5. Sonuçlar panelinde seçin **Softeon WMS**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve Softeon WMS ile Azure AD çoklu oturum açmayı test "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı

Tekli çalışmaya oturum için Azure AD Softeon WMS karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Softeon WMS ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Softeon WMS içinde atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Softeon WMS ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Softeon WMS test kullanıcısı oluşturma](#creating-a-softeon-wms-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Softeon WMS sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Softeon WMS uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Softeon WMS ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Softeon WMS** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_samlbase.png)

3. Üzerinde **Softeon WMS etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.softeon.com/<instancename>`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.softeon.com/sp`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Softeon WMS istemci destek ekibi](mailto:contact@softeon.com) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-softeon-tutorial/tutorial_general_400.png)

6. Üzerinde **Softeon WMS yapılandırma** 'yi tıklatın **yapılandırma Softeon WMS** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_configure.png) 

7. Çoklu oturum açma yapılandırmak için **Softeon WMS** yan, indirilen göndermek için ihtiyacınız **sertifika (Base64), SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [Softeon WMS destek ekibi ](mailto:contact@softeon.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-softeon-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-softeon-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-softeon-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-softeon-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-softeon-wms-test-user"></a>Softeon WMS test kullanıcısı oluşturma

Uygulama, süresi kullanıcı sağlama ve kimlik doğrulama kullanıcılar uygulamada otomatik olarak oluşturulduktan sonra hemen destekler.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Softeon WMS erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Softeon WMS atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Softeon WMS**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Softeon WMS parçasında tıkladığınızda, oturum açma sayfasına Softeon WMS uygulamasının almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_203.png

