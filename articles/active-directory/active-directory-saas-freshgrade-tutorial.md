---
title: "Öğretici: Azure Active Directory Tümleştirme ile FreshGrade | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile FreshGrade arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1055bba6-f4df-462e-bc9b-1ad5ada0f638
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: f9cdf73643a8aa18506e59eff513b98282f60832
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshgrade"></a>Öğretici: Azure Active Directory Tümleştirme FreshGrade ile

Bu öğreticide, Azure Active Directory (Azure AD) ile FreshGrade tümleştirmek öğrenin.

FreshGrade Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- FreshGrade erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için FreshGrade (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme FreshGrade ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir FreshGrade çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden FreshGrade ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-freshgrade-from-the-gallery"></a>Galeriden FreshGrade ekleme
Azure AD FreshGrade tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden FreshGrade eklemeniz gerekir.

**Galeriden FreshGrade eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **FreshGrade**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_search.png)

5. Sonuçlar panelinde seçin **FreshGrade**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı FreshGrade sınayın.

Tekli çalışmaya oturum için Azure AD FreshGrade karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının FreshGrade ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

FreshGrade içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma FreshGrade ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[FreshGrade test kullanıcısı oluşturma](#creating-a-freshgrade-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı FreshGrade sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma FreshGrade uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile FreshGrade yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **FreshGrade** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_samlbase.png)

3. Üzerinde **FreshGrade etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, aşağıdaki desenleri kullanarak URL'sini yazın: 
      | |
      |--|
      | `https://<subdomain>.freshgrade.com/login` |    
      | `https://<subdomain>.onboarding.freshgrade.com/login` |

    b. İçinde **tanımlayıcısı** metin kutusuna, aşağıdaki desenleri kullanarak URL'sini yazın: 
      | |
      |--|
      | `https://login.onboarding.freshgrade.com:443/saml/metadata/alias/<instancename>` |      
      | `https://login.freshgrade.com:443/saml/metadata/alias/<instancename>` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [FreshGrade istemci destek ekibi](mailTo:support@freshgrade.com) bu değerleri almak için. 
 


4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshgrade-tutorial/tutorial_general_400.png)

6. Üzerinde **FreshGrade yapılandırma** 'yi tıklatın **yapılandırma FreshGrade** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_configure.png) 

7. Oluşturulacak **meta veri** url, aşağıdaki adımları gerçekleştirin:

    a. Tıklatın **uygulama kayıtlar**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_appregistrations.png)
   
    b. Tıklatın **uç noktaları** açmak için **uç noktaları** iletişim kutusu.  
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_endpointicon.png)

    c. Kopyalamak için Kopyala düğmesini tıklatın **FEDERASYON meta veri belgesi** URL'yi kopyalayıp Not Defteri'ne yapıştırın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_endpoint.png)
     
    d. Şimdi özellik sayfasına gidin **FreshGrade** ve kopyalama **uygulama kimliği** kullanarak **kopyalama** düğmesine tıklayın ve Not Defteri'ne yapıştırın.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_appid.png)

    e. Oluştur **meta veri URL'sini** şu biçimi kullanarak:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

8. Çoklu oturum açma yapılandırmak için **FreshGrade** yan, ihtiyacınız göndermek **meta veri URL'sini** ve **SAML çoklu oturum açma hizmet URL'si** için [FreshGrade destek ekibi ](mailTo:support@freshgrade.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-freshgrade-test-user"></a>FreshGrade test kullanıcısı oluşturma

Bu bölümde, FreshGrade içinde Britta Simon adlı bir kullanıcı oluşturun. Lütfen çalışmak [FreshGrade destek ekibi](mailTo:support@freshgrade.com) FreshGrade platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta FreshGrade için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**FreshGrade için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **FreshGrade**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli FreshGrade parçasında tıklattığınızda, otomatik olarak FreshGrade uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_203.png

