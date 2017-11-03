---
title: "Öğretici: Azure Active Directory Tümleştirme Igloo yazılımıyla | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Igloo yazılımı arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ab3891e11eb33b4d233e4fc967a40c7df06e4f4e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Öğretici: Azure Active Directory Tümleştirme Igloo yazılımıyla

Bu öğreticide, Azure Active Directory (Azure AD) ile Igloo yazılım tümleştirmek öğrenin.

Igloo yazılım Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Igloo yazılım erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Igloo yazılımı (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Igloo yazılımıyla yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Igloo yazılım çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Igloo yazılım ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-igloo-software-from-the-gallery"></a>Galeriden Igloo yazılım ekleme
Azure AD Igloo yazılım tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Igloo yazılım eklemeniz gerekir.

**Galeriden Igloo yazılım eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Igloo yazılım**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_search.png)

5. Sonuçlar panelinde seçin **Igloo yazılım**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmak ve Igloo yazılım "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Igloo yazılım bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Igloo yazılım ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Igloo yazılımda atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma Igloo yazılımıyla sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Igloo yazılım test kullanıcısı oluşturma](#creating-an-igloo-software-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Igloo yazılım sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Igloo yazılım uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Igloo yazılımıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Igloo yazılım** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

3. Üzerinde **Igloo yazılım etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<company name>.igloocommmunities.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<company name>.igloocommmunities.com/saml.digest`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<company name>.igloocommmunities.com/saml.digest`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Igloo yazılım istemci destek ekibi](https://www.igloosoftware.com/services/support) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-igloo-software-tutorial/tutorial_general_400.png)
    
6. Üzerinde **Igloo yazılım yapılandırma** 'yi tıklatın **Igloo yazılımı Yapılandır** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

7. Farklı web tarayıcısı penceresinde Igloo yazılım şirket sitenize yönetici olarak oturum açın.

8. Git **Denetim Masası**.
   
     ![Denetim Masası](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "Denetim Masası")

9. İçinde **üyelik** sekmesini tıklatın, **ayarları oturum**.
   
    ![Ayarları'nda oturum](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "Ayarları'nda oturum açın")

10. SAML yapılandırma bölümünde tıklatın **SAML kimlik doğrulaması yapılandırma**.
   
    ![SAML Yapılandırması](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML yapılandırması")
   
11. İçinde **genel yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Genel yapılandırma](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "genel yapılandırma")

    a. İçinde **bağlantı adı** metin kutusuna, yapılandırmanız için özel bir ad yazın.
   
    b. İçinde **IDP oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
   
    c. İçinde **IDP oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.
    
    d. Seçin **oturum kapatma yanıt ve İstek HTTP türü** olarak **POST**.
   
    e. Açık, **base-64** Not Defteri'nde kodlanmış sertifika Azure Portalı'ndan indirilen, içeriğini, panoya kopyalayın ve yapıştırın kendisine **ortak sertifika** metin kutusu.
    
12. İçinde **yanıt ve kimlik doğrulama Yapılandırması**, aşağıdaki adımları gerçekleştirin:
    
    ![Yanıt ve kimlik doğrulama Yapılandırması](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "yanıt ve kimlik doğrulama yapılandırması")
  
      a. Olarak **kimlik sağlayıcısı**seçin **Microsoft ADFS**.
      
      b. Olarak **tanımlayıcı türü**seçin **e-posta adresi**. 

      c. İçinde **e-posta özniteliği** metin kutusuna, türü **emailaddress**.

      d. İçinde **ad özniteliği** metin kutusuna, türü **givenname**.

      e. İçinde **son Name özniteliği** metin kutusuna, türü **Soyadı**.

13. Yapılandırmayı tamamlamak için aşağıdaki adımları gerçekleştirin:
    
    ![Oturum açma kullanıcı oluşturma](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "oturum açma kullanıcı oluşturma") 

     a. Olarak **oturum açma kullanıcı oluşturma**seçin **yeni bir kullanıcı oturum sitenizdeki oluşturduğunuzda**.

     b. Olarak **ayarlarında oturum**seçin **"Oturum" ekranında kullanım SAML düğmesini**.

     c. **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-an-igloo-software-test-user"></a>Bir Igloo yazılım test kullanıcısı oluşturma

Kullanıcı için Igloo yazılım sağlama yapılandırmanız için eylem öğe yok.  

Atanmış bir kullanıcı Igloo erişim paneli kullanarak yazılım için oturum açma girişiminde bulunduğunda, Igloo yazılım kullanıcının var olup olmadığını denetler.  Hiçbir kullanıcı hesabı varsa kullanılabilir henüz, Igloo yazılım tarafından otomatik olarak oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Igloo yazılıma erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Igloo yazılım atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Igloo yazılım**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Igloo yazılım parçasında tıklattığınızda, otomatik olarak Igloo yazılım uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_203.png

