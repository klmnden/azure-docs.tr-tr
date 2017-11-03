---
title: "Öğretici: Azure Active Directory Tümleştirme tanı ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve tanı arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 97d85183d0307c41a3b879d440d87a6fb0c53190
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Öğretici: Tanı Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile tanı tümleştirmek öğrenin.

Tanı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tanı erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için tanı (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme tanı ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir tanı çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden tanı ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-recognize-from-the-gallery"></a>Galeriden tanı ekleme
Azure AD tanı tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden tanı eklemeniz gerekir.

**Tanı Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **tanı**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_search.png)

5. Sonuçlar panelinde seçin **tanı**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı tanı sınayın.

Tekli çalışmaya oturum için Azure AD tanı karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının tanı ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Tanı içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma tanı ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Tanı test kullanıcısı oluşturma](#creating-a-recognize-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı tanı sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma tanı uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile tanı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **tanı** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_samlbase.png)

3. Üzerinde **etki alanı tanıması ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://recognizeapp.com/<your-domain>/saml/sso`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://recognizeapp.com/<your-domain>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [tanıması istemci destek ekibi](mailto:support@recognizeapp.com) oturum açma URL'si almak için tanımlayıcı değeri öğreticide daha sonra açıklanan SSO ayarları bölümünden servis sağlayıcı meta verileri URL'sini açarak alabilirsiniz. . 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-recognize-tutorial/tutorial_general_400.png)

6. Üzerinde **Tanı Yapılandırması** 'yi tıklatın **Yapılandırma tanı** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_configure.png) 

7. Farklı web tarayıcısı penceresinde tanı kiracınız yönetici olarak oturum.

8. Bölmenin sağ üst köşesinde tıklatın **menü**. Git **şirket yönetici**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

9. Sol gezinti bölmesinde tıklatın **ayarları**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

10. Aşağıdaki adımları gerçekleştirin **SSO ayarları** bölümü.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)
    
    a. Olarak **etkinleştirmek SSO**seçin **ON**.

    b. İçinde **IDP varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.
    
    c. İçinde **Sso hedef url** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
    
    d. İçinde **Slo hedef url** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan. 
    
    e. İndirilen açmak **sertifika (Base64)** dosyasını Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **sertifika** metin kutusu.
    
    f. Tıklatın **Ayarları Kaydet** düğmesi. 

11. Yanında **SSO ayarları** bölümünde, altında URL'yi kopyalayın **hizmet sağlayıcısı meta veri URL'sini**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

12. Açık **meta veri URL'sini bağlantı** meta veri belgesi indirmek için boş bir tarayıcı altında. Sonra EntityDescriptor value(entityID) dosyasından kopyalayın ve yapıştırın **tanımlayıcısı** metin kutusuna **etki alanı tanıması ve URL'leri bölümüne** Azure Portal'daki.
    
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-recognize-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-recognize-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-recognize-test-user"></a>Tanı test kullanıcısı oluşturma

Azure AD kullanıcıların tanı oturum etkinleştirmek için bunların tanı sağlanmalıdır. Tanı söz konusu olduğunda, sağlama bir el ile bir görevdir.

Bu uygulama SCIM'yi sağlama desteklemiyor, ancak kullanıcılar sağlayan bir alternatif kullanıcı eşitleme sahiptir. 

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Tanı şirket sitenize yönetici olarak oturum açın.

2. Bölmenin sağ üst köşesinde tıklatın **menü**. Git **şirket yönetici**.

3. Sol gezinti bölmesinde tıklatın **ayarları**.

4. Aşağıdaki adımları gerçekleştirin **kullanıcı eşitleme** bölümü.
   
   ![Yeni kullanıcı](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "yeni kullanıcı")
   
   a. Olarak **eşitlemenin etkin**seçin **ON**.
   
   b. Olarak **Seç eşitleme sağlayıcısı**seçin **Microsoft / Office 365**.
   
   c. Tıklatın **kullanıcı eşitleme çalıştırmak**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta tanı için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Tanı için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **tanı**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli tanı parçasında tıklattığınızda, otomatik olarak tanı uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png

