---
title: "Öğretici: Azure Active Directory Tümleştirme 10.000 ft planına sahip | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile 10.000 ft planları arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b60c955e-8fa3-4872-a897-c4e81fd7beac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 76e91c1a4f52337fe7e24d2c7dc727d1fb0ebef2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-10000ft-plans"></a>Öğretici: Azure Active Directory Tümleştirme 10.000 ft planına sahip

Bu öğreticide, Azure Active Directory (Azure AD) ile 10.000 ft planlarının tümleştirmenize öğrenin.

10.000 ft planları Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- 10.000 ft planları erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak 10.000 ft planlara (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme 10.000 ft planlarıyla yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir 10.000 ft planları çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme alabilirsiniz [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden planları 10.000 ft ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-10000ft-plans-from-the-gallery"></a>Galeriden planları 10.000 ft ekleme
Azure AD 10.000 ft planları tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden 10.000 ft planları eklemeniz gerekir.

**Galeriden 10.000 ft planları eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **10.000 ft planları**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_search.png)

5. Sonuçlar panelinde seçin **10.000 ft planları**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma 10.000 ft "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı planları ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen 10.000 ft planlarda bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının 10.000 ft ilgili kullanıcı arasında bir bağlantı ilişkisi planları gerekiyor kurulamıyor.

10.000 ft planlarda değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma 10.000 ft planları ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Kullanıcı test planları 10.000 ft oluşturma](#creating-a-10000ft-plans-test-user)**  - bir Britta Simon karşılık gelen kullanıcı Azure AD gösterimini bağlı 10.000 ft planları içinde olması.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma 10.000 ft planları uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma 10.000 ft planlarıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **10.000 ft planları** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_samlbase.png)

3. Üzerinde **10.000 ft planları etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın:`https://app.10000ft.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`https://app.10000ft.com/saml/metadata`

    > [!NOTE] 
    > Değeri **tanımlayıcısı** özel bir etki alanı varsa farklıdır. Kişi [10.000 ft planları destek ekibi](https://www.10000ft.com/plans/support) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Raw)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_400.png)

6. Üzerinde **10.000 ft planları yapılandırma** 'yi tıklatın **yapılandırma 10.000 ft planları** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_configure.png) 

7. Çoklu oturum açma yapılandırmak için **10.000 ft planları** yan, indirilen göndermek için ihtiyacınız **Certificate(Raw), Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [10.000 ft Planları destek ekibi](https://www.10000ft.com/plans/support).

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-10000ft-plans-test-user"></a>Kullanıcı test planları 10.000 ft oluşturma

Bu bölümün amacı Britta Simon 10.000 ft planlarda adlı bir kullanıcı oluşturmaktır. 10.000 ft planları yalnızca zaman kaynak sağlamayı destekler, varsayılan olarak etkin olduğu. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa 10.000 ft planları erişme denemesi sırasında oluşturulur. 

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, başvurmanız gerekir [10.000 ft planları destek ekibi](https://www.10000ft.com/plans/support).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta 10.000 ft planlarına erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**10.000 ft planlarına Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **10.000 ft planları**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.  
Erişim paneli 10.000 ft planları parçasında tıklattığınızda, otomatik olarak 10.000 ft planları uygulamanızı açan.
 
## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_203.png

