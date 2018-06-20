---
title: 'Öğretici: Azure Active Directory Tümleştirme ile ThirdLight | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile ThirdLight arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 168aae9a-54ee-4c2b-ab12-650a2c62b901
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 42be83af52298e1fc6b01793b7692a3fcd7d6250
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36218279"
---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Öğretici: Azure Active Directory Tümleştirme ThirdLight ile

Bu öğreticide, Azure Active Directory (Azure AD) ile ThirdLight tümleştirmek öğrenin.

ThirdLight Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- ThirdLight erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için ThirdLight (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme ThirdLight ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir ThirdLight çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden ThirdLight ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-thirdlight-from-the-gallery"></a>Galeriden ThirdLight ekleme
Azure AD ThirdLight tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden ThirdLight eklemeniz gerekir.

**Galeriden ThirdLight eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **ThirdLight**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thirdlight-tutorial/tutorial_thirdlight_search.png)

5. Sonuçlar panelinde seçin **ThirdLight**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thirdlight-tutorial/tutorial_thirdlight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı ThirdLight ile test etme

Tekli çalışmaya oturum için Azure AD ThirdLight karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ThirdLight ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** ThirdLight içinde.

Yapılandırma ve Azure AD çoklu oturum açma ThirdLight ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[ThirdLight test kullanıcısı oluşturma](#creating-a-thirdlight-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı ThirdLight sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ThirdLight uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile ThirdLight yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **ThirdLight** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/thirdlight-tutorial/tutorial_thirdlight_samlbase.png)

3. Üzerinde **ThirdLight etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/thirdlight-tutorial/tutorial_thirdlight_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.thirdlight.com/`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.thirdlight.com/saml/sp`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve Identiifer ile güncelleştirin. Kişi [ThirdLight istemci destek ekibi](https://www.thirdlight.com/support) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/thirdlight-tutorial/tutorial_thirdlight_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/thirdlight-tutorial/tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde ThirdLight şirket sitenize yönetici olarak oturum açın.

7. Git **yapılandırma \> Sistem Yönetimi**ve ardından **SAML2**.
   
    ![Sistem Yönetimi](./media/thirdlight-tutorial/ic805843.png "Sistem Yönetimi")

8. SAML2 yapılandırma bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![SAML çoklu oturum açma](./media/thirdlight-tutorial/ic805844.png "SAML çoklu oturum açma")   

     a. Seçin **SAML2 çoklu oturum açmayı etkinleştir**.
 
     b. Olarak **IDP meta veriler için kaynak**seçin **XML yük IDP meta verilerini**.
 
     c. İndirilen meta veri dosyasını açın, içeriği Kopyala ve ardından yapıştırın **IDP meta veri XML** metin kutusu. 
     
     d. Tıklatın **kaydetmek SAML2 ayarları**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/thirdlight-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/thirdlight-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/thirdlight-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/thirdlight-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** Britta Simon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-thirdlight-test-user"></a>ThirdLight test kullanıcısı oluşturma

Azure AD kullanıcıları için ThirdLight oturum açmak etkinleştirmek için bunların ThirdLight sağlanmalıdır.  
ThirdLight söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **ThirdLight** yönetici olarak şirket site.

2. Git **kullanıcılar** sekmesi.

3. Seçin **kullanıcılar ve gruplar**.

4. Tıklatın **yeni kullanıcı Ekle** düğmesi.

5. Girin **kullanıcı adı, ad veya açıklama, e-posta, hazır veya yeni üyeler grup seçin** sağlamak istediğiniz geçerli bir AAD hesabının.

6. **Oluştur**’a tıklayın.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Thirdlight tarafından sağlanan veya herhangi diğer Thirdlight kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta ThirdLight için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**ThirdLight için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ThirdLight**.

    ![Çoklu oturum açmayı yapılandırın](./media/thirdlight-tutorial/tutorial_thirdlight_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ThirdLight parçasında tıklattığınızda, otomatik olarak ThirdLight uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/thirdlight-tutorial/tutorial_general_01.png
[2]: ./media/thirdlight-tutorial/tutorial_general_02.png
[3]: ./media/thirdlight-tutorial/tutorial_general_03.png
[4]: ./media/thirdlight-tutorial/tutorial_general_04.png

[100]: ./media/thirdlight-tutorial/tutorial_general_100.png

[200]: ./media/thirdlight-tutorial/tutorial_general_200.png
[201]: ./media/thirdlight-tutorial/tutorial_general_201.png
[202]: ./media/thirdlight-tutorial/tutorial_general_202.png
[203]: ./media/thirdlight-tutorial/tutorial_general_203.png

