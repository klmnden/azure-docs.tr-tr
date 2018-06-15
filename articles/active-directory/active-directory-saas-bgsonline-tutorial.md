---
title: 'Öğretici: Azure Active Directory Tümleştirme edici Online ile | Microsoft Docs'
description: Azure Active Directory edici Online arasında tek oturum açma yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4fd6b29b-1b46-4fd1-9f5e-16b1c9d892cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: fc551aaa0a1383376d345f922c3e226cb88c4d26
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34338661"
---
# <a name="tutorial-azure-active-directory-integration-with-bgs-online"></a>Öğretici: Azure Active Directory Tümleştirme edici Online ile

Bu öğreticide, Azure Active Directory (Azure AD) ile edici çevrimiçi tümleştirmek öğrenin.

EDİCİ çevrimiçi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- EDİCİ çevrimiçi erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak edici Online'a (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme edici Online ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir edici çevrimiçi çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. EDİCİ çevrimiçi galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-bgs-online-from-the-gallery"></a>EDİCİ çevrimiçi galeriden ekleme
Azure AD edici Online tümleştirilmesi yapılandırmak için edici çevrimiçi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**EDİCİ çevrimiçi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **edici çevrimiçi**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_search.png)

5. Sonuçlar panelinde seçin **edici çevrimiçi**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma edici "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Online ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen edici çevrimiçi bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı edici çevrimiçi arasında bir bağlantı ilişkisi kurulması gerekir.

Çevrimiçi edici olarak değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma edici Online ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[EDİCİ çevrimiçi bir test kullanıcısı oluşturma](#creating-a-bgs-online-test-user)**  - edici kullanıcı Azure AD gösterimini bağlantılı çevrimiçi Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma edici çevrimiçi uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma edici Online ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **edici çevrimiçi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_samlbase.png)

3. Üzerinde **edici çevrimiçi etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:

    Üretim ortamı için bu deseni kullanır `https://<company name>.millwardbrown.report` 

    Test ortamı için bu deseni kullanır `https://millwardbrown.marketingtracker.nl/mt5/`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    
    Üretim ortamı için bu deseni kullanır `https://<company name>.millwardbrown.report/sso/saml/AssertionConsumerService.aspx` 
      
    Test ortamı için bu deseni kullanır `https://millwardbrown.marketingtracker.nl/mt5/sso/saml/AssertionConsumerService.aspx`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [edici çevrimiçi destek ekibi](mailTo:bgsdashboardteam@millwardbrown.com) bu değerleri almak için.
 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-bgsonline-tutorial/tutorial_general_400.png)

6. Üzerinde **edici çevrimiçi yapılandırma** 'yi tıklatın **edici çevrimiçi yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_configure.png) 

7. Çoklu oturum açma yapılandırmak için **edici çevrimiçi** yan, indirilen göndermek için ihtiyacınız **meta veri XML** ve **SAML çoklu oturum açma hizmet URL'si** için [edici çevrimiçi destek ekibi](mailto:bgsdashboardteam@millwardbrown.com). 


> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-bgs-online-test-user"></a>EDİCİ çevrimiçi bir test kullanıcısı oluşturma

Bu bölümde, Britta Simon edici çevrimiçi olarak adlandırılan bir kullanıcı oluşturun. Çalışmak [edici çevrimiçi destek ekibi](mailto:bgsdashboardteam@millwardbrown.com) edici çevrimiçi platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta edici Online'a erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon edici Online'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **edici çevrimiçi**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD SSO yapılandırmanızı sınayın.

Erişim panelinde edici çevrimiçi kutucuğa tıkladığınızda, otomatik olarak edici çevrimiçi uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_203.png

