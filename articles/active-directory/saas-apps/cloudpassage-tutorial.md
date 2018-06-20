---
title: 'Öğretici: Azure Active Directory Tümleştirme ile CloudPassage | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile CloudPassage arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 6377eeb06705528f567aaf6f29461db0023c27b4
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36223136"
---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a>Öğretici: Azure Active Directory Tümleştirme CloudPassage ile

Bu öğreticide, Azure Active Directory (Azure AD) ile CloudPassage tümleştirmek öğrenin.

CloudPassage Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- CloudPassage erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için CloudPassage (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme CloudPassage ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir CloudPassage çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden CloudPassage ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-cloudpassage-from-the-gallery"></a>Galeriden CloudPassage ekleme
Azure AD CloudPassage tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden CloudPassage eklemeniz gerekir.

**Galeriden CloudPassage eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **CloudPassage**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/tutorial_cloudpassage_search.png)

5. Sonuçlar panelinde seçin **CloudPassage**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/tutorial_cloudpassage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı CloudPassage ile test etme

Tekli çalışmaya oturum için Azure AD CloudPassage karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının CloudPassage ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

CloudPassage içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma CloudPassage ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[CloudPassage test kullanıcısı oluşturma](#creating-a-cloudpassage-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı CloudPassage sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma CloudPassage uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile CloudPassage yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **CloudPassage** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_samlbase.png)

3. Üzerinde **CloudPassage etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://portal.cloudpassage.com/saml/init/accountid`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://portal.cloudpassage.com/saml/consume/accountid`. Tıklayarak, bu öznitelik için değeriniz alabilirsiniz **SSO Kurulum belgeleri** içinde **çoklu oturum açma ayarları** CloudPassage portalınızı bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_05.png)
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [CloudPassage istemci destek ekibi](https://www.cloudpassage.com/company/contact/) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_certificate.png) 

5. SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde SAML onaylar CloudPassage uygulamanızı bekliyor. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
   
   ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_25.png) 

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |
    | FirstName |User.givenName |
    | Soyadı |User.surname |
    | e-posta |User.Mail |
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

7. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_general_400.png)
    
8. Üzerinde **CloudPassage yapılandırma** 'yi tıklatın **yapılandırma CloudPassage** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_configure.png) 

9. Farklı bir tarayıcı penceresinde CloudPassage şirket sitenize yönetici olarak oturum.

10. Üstteki menüde tıklatın **ayarları**ve ardından **Site Yönetimi**. 
   
    ![Çoklu oturum açmayı yapılandırın][12]

11. Tıklatın **kimlik doğrulama ayarlarını** sekmesi. 
   
    ![Çoklu oturum açmayı yapılandırın][13]

12. İçinde **çoklu oturum açma ayarları** bölümünde, aşağıdaki adımları gerçekleştirin: 
   
    ![Çoklu oturum açmayı yapılandırın][14]

    a. Seçin **etkinleştirmek tek sign-on(SSO) (SSO Kurulum belgeleri)** onay kutusu.
    
    b. Yapıştır **SAML varlık kimliği** içine **SAML veren URL'si** metin kutusu.
  
    c. Yapıştır **SAML çoklu oturum açma hizmet URL'si** içine **SAML uç nokta URL'si** metin kutusu.
  
    d. Yapıştır **Sign-Out URL** içine **oturum kapatma giriş sayfası** metin kutusu.
  
    e. İndirilen sertifikanızı Not Defteri'nde açın, indirilen sertifika içeriğini, panoya kopyalayın ve ardından yapıştırın **x 509 Sertifika** metin kutusu.
  
    f. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cloudpassage-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-cloudpassage-test-user"></a>CloudPassage test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde CloudPassage adlı bir kullanıcı oluşturmaktır.

**İçinde CloudPassage Britta Simon adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açma için sizin **CloudPassage** yönetici olarak şirket site. 

2. Üstteki araç çubuğunda tıklatın **ayarları**ve ardından **Site Yönetimi**. 
   
   ![CloudPassage test kullanıcısı oluşturma][22] 

3. Tıklatın **kullanıcılar** sekmesini ve ardından **yeni kullanıcı Ekle**. 
   
   ![CloudPassage test kullanıcısı oluşturma][23]

4. İçinde **yeni kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin: 
   
   ![CloudPassage test kullanıcısı oluşturma][24]
    
    a. İçinde **ad** metin kutusuna, Britta yazın. 
  
    b. İçinde **Soyadı** metin kutusuna, Simon yazın.
  
    c. İçinde **kullanıcıadı** metin kutusuna, **e-posta** textbox ve **yeniden e-posta** metin kutusuna, Azure AD'de Britta'nin kullanıcı adını yazın.
  
    d. Olarak **erişim türü**seçin **hale Portal erişimi etkinleştir**.
  
    e. **Ekle**'ye tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta CloudPassage için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**CloudPassage için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **CloudPassage**.

    ![Çoklu oturum açmayı yapılandırın](./media/cloudpassage-tutorial/tutorial_cloudpassage_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Erişim paneli CloudPassage parçasında tıklattığınızda, otomatik olarak CloudPassage uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/cloudpassage-tutorial/tutorial_general_04.png
[12]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_17.png

[100]: ./media/cloudpassage-tutorial/tutorial_general_100.png

[200]: ./media/cloudpassage-tutorial/tutorial_general_200.png
[201]: ./media/cloudpassage-tutorial/tutorial_general_201.png
[202]: ./media/cloudpassage-tutorial/tutorial_general_202.png
[203]: ./media/cloudpassage-tutorial/tutorial_general_203.png

