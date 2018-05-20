---
title: 'Öğretici: Azure Active Directory Tümleştirme ile öğesine | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory arasında yapılandırmayı öğrenin ve öğesine.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: a7f3fac69143cc4863b65604c6aac314157ba826
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Öğretici: Azure Active Directory Tümleştirme ile özelliği

Bu öğreticide, yani Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Yani Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Erişebilen öğesine Azure AD'de kontrol edebilirsiniz
- Oturum açma için otomatik olarak öğesine almak, kullanıcılarınızın etkinleştirebilirsiniz (çoklu oturum açma) Azure AD hesaplarına sahip
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Ayrıca, ile tümleştirme Azure AD yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir öğesine çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden öğesine ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-namely-from-the-gallery"></a>Galeriden öğesine ekleme
Tümleştirilmesi, yani Azure AD yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden öğesine eklemeniz gerekir.

**Galeriden öğesine eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **öğesine**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-namely-tutorial/tutorial_namely_search.png)

5. Sonuçlar panelinde seçin **öğesine**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile "Britta Simon" adlı bir test kullanıcı öğesine göre test etme.

Tekli çalışmaya oturum için Azure AD içinde karşılık gelen kullanıcı adlı bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı arasındaki bağlantıyı ilişki öğesine kurulması gerekir.

Yani, değeri olarak atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcı adı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD sınamak için çoklu oturum açma adlı, sizinle aşağıdaki yapı taşları tamamlamanız gereken:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Oluşturma bir öğesine test kullanıcısı](#creating-a-namely-test-user)**  - Britta Simon, karşılık gelen sağlamak için yani, bağlı olduğu kullanıcı Azure AD gösterimi.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, yani uygulama yapılandırın.

**Azure AD çoklu oturum açma ile öğesine yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **öğesine** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_samlbase.png)

3. Üzerinde **adlı etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.namely.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.namely.com/saml/metadata`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [öğesine istemci destek ekibi](https://www.namely.com/contact/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_general_400.png)

6. Üzerinde **öğesine yapılandırma** 'yi tıklatın **öğesine yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_configure.png) 

7. Başka bir tarayıcı penceresinde oturum açın, yani şirket site yönetici olarak.

8. Üstteki araç çubuğunda tıklatın **şirket**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

9. **Ayarlar** sekmesine tıklayın.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 

10. Tıklatın **SAML**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 

11. Üzerinde **SAML ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png)
 
    a. Tıklatın **etkinleştirmek SAML**. 

    b. İçinde **kimlik sağlayıcısı SSO URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
    
    c. İndirilen sertifikanızı Not Defteri'nde açın, içeriği Kopyala ve ardından yapıştırın **kimlik sağlayıcısı sertifikası** metin kutusu.
     
    d. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-namely-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-namely-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-namely-test-user"></a>Oluşturma bir öğesine test kullanıcısı

Bu bölümün amacı, yani Britta Simon adlı bir kullanıcı oluşturmaktır.

**Yani Britta Simon adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açma, yani şirket site yönetici olarak.

2. Üstteki araç çubuğunda tıklatın **kişiler**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

3. Tıklatın **Directory** sekmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

4. Tıklatın **yeni bir kişiye eklemek**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_12.png)

5. Üzerinde **Yeni Kişi Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    a. İçinde **ad** metin kutusuna, türü **Britta**.

    b. İçinde **Soyadı** metin kutusuna, türü **Simon**.

    c. İçinde **e-posta** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    d. **Kaydet**’e tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta öğesine erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Yani, aşağıdaki adımları gerçekleştirmek için Britta Simon atamak için:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **öğesine**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-namely-tutorial/tutorial_namely_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Tıkladığınızda öğesine erişim panelinde döşeme, otomatik olarak imzalanan, yani uygulama için açma

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-namely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png

