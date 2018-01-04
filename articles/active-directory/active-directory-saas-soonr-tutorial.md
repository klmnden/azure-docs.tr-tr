---
title: "Öğretici: Azure Active Directory Tümleştirme ile Soonr çalışma | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Soonr çalışma arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 55e99b2dd566dbf5a3bcb86974210c0399584e72
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Öğretici: Azure Active Directory Tümleştirme ile Soonr çalışma

Bu öğreticide, Azure Active Directory (Azure AD) ile Soonr çalışma alanına tümleştirmek öğrenin.

Soonr çalışma alanına Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Soonr çalışma alanına erişimi olan Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Soonr çalışma alanına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Soonr çalışma alanınız ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Soonr çalışma alanı çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Soonr çalışma alanına ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-soonr-workplace-from-the-gallery"></a>Galeriden Soonr çalışma alanına ekleme
Azure AD Soonr çalışma alanına tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Soonr çalışma alanına eklemeniz gerekir.

**Galeriden Soonr çalışma alanına eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Soonr çalışma alanına**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_search.png)

5. Sonuçlar panelinde seçin **Soonr çalışma alanına**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve "Britta Simon" adlı bir test kullanıcı Soonr çalışma alanı ile Azure AD çoklu oturum açmayı test temel.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Soonr çalışma bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Soonr çalışma arasında bir bağlantı ilişkisi kurulması gerekir.

Soonr çalışma alanı'ndaki değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Soonr çalışma alanı ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Soonr çalışma alanına test kullanıcısı oluşturma](#creating-a-soonr-workplace-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Soonr çalışma sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Soonr çalışma alanına uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Soonr çalışma alanınız ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Soonr çalışma alanına** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_samlbase.png)

3. Üzerinde **Soonr çalışma alanına etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<servername>.soonr.com/singlesignon/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<servername>.soonr.com/singlesignon/saml/SSO`

4. Üzerinde **Soonr çalışma alanına etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **SP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_url1.png)

    a. Tıklayın **Göster Gelişmiş URL ayarları**.

    b. İçinde **oturum üzerinde URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<servername>.soonr.com/singlesignon/saml/SSO`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri güncelleştirmek gerçek tanımlayıcısı ile oturum açma URL'si ve yanıt URL'si. Kişi [Soonr çalışma alanına destek ekibi](https://awp.autotask.net/help/) bu değerleri almak için.
 
5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-soonr-tutorial/tutorial_general_400.png)

7. Üzerinde **Soonr çalışma alanına yapılandırma** 'yi tıklatın **yapılandırma Soonr çalışma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_configure.png) 

8. Çoklu oturum açma yapılandırmak için **Soonr çalışma alanına** yan, indirilen göndermek için ihtiyacınız **meta veri XML**, **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [Soonr çalışma alanına destek ekibi](https://awp.autotask.net/help/). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

    >[!Note]
    >Autotask çalışma alanına yapılandırma ile ilgili Yardım gerekiyorsa, lütfen bkz [bu sayfayı](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) iş yeri hesabınızı Yardım almak için.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-soonr-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-soonr-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-soonr-workplace-test-user"></a>Soonr çalışma alanına test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon Soonr çalışma alanında adlı bir kullanıcı oluşturmaktır. Çalışmak [Soonr çalışma alanına destek ekibi](https://awp.autotask.net/help/) platform bir kullanıcı oluşturmak için. Soonr ile destek bileti yükseltebilirsiniz <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">burada</a>.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Soonr çalışma alanına erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Soonr çalışma alanına atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Soonr çalışma alanına**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.  

Erişim paneli Soonr çalışma alanına parçasında tıklattığınızda, otomatik olarak Soonr çalışma alanına uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png

