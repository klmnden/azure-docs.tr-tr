---
title: "Öğretici: Azure Active Directory Tümleştirme halojensiz yazılımıyla | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve halojensiz yazılımı arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: af9c9d4b9bd562169bfe6c9158254beec5c3e1e7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Öğretici: Azure Active Directory Tümleştirme halojensiz yazılımıyla

Bu öğreticide, halojensiz yazılım Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Halojensiz yazılım Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Halojensiz yazılım erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak halojensiz yazılımı (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme halojensiz yazılımıyla yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir halojensiz yazılım çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden halojensiz yazılım ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-halogen-software-from-the-gallery"></a>Galeriden halojensiz yazılım ekleme

Azure AD halojensiz yazılım tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden halojensiz yazılım eklemeniz gerekir.

**Galeriden halojensiz yazılım eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **halojensiz yazılım**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_search.png)

5. Sonuçlar panelinde seçin **halojensiz yazılım**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmak ve halojensiz yazılım "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen halojensiz yazılım bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının halojensiz yazılım ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri halojensiz yazılımda atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma halojensiz yazılımıyla sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Halojensiz yazılım test kullanıcısı oluşturma](#creating-a-halogen-software-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı halojensiz yazılım sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma halojensiz yazılım uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma halojensiz yazılımıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **halojensiz yazılım** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_samlbase.png)

3. Üzerinde **halojensiz yazılım etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://global.hgncloud.com/<companyname>`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://global.halogensoftware.com/<companyname>`,`https://global.hgncloud.com/<companyname>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [halojensiz yazılım istemci destek ekibi](https://support.halogensoftware.com/) bu değerleri almak için. 
 


4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-halogen-software-tutorial/tutorial_general_400.png)

6. Farklı bir tarayıcı penceresinde için oturum, **halojensiz yazılım** yönetici olarak uygulama.

7. Tıklatın **seçenekleri** sekmesi. 
   
    ![Azure AD Connect nedir?][12]

8. Sol gezinti bölmesinde **SAML Yapılandırması**. 
   
    ![Azure AD Connect nedir?][13]

9. Üzerinde **SAML Yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin: 

    ![Azure AD Connect nedir?][14]

     a. Olarak **benzersiz tanımlayıcı**seçin **NameID**.

     b. Olarak **benzersiz tanıtıcı eşlemeleri için**seçin **kullanıcıadı**.
  
     c. İndirilen meta veri dosyanızı karşıya yüklemek için tıklayın **Gözat** dosyasını seçmek için ve ardından **dosyasını karşıya yükle**.
 
     d. Yapılandırmanızı sınamak için **testi Çalıştır**. 
    
    >[!NOTE]
    >Öğesinin ileti beklemesi gerekir "*SAML test tamamlandıktan. Lütfen bu pencereyi kapatmak*". Ardından, açılan tarayıcı penceresini kapatın. **Etkinleştirmek SAML** onay kutusunu test olmuşsa yalnızca etkin. 
     
     e. Seçin **etkinleştirmek SAML**.
    
     f. Tıklatın **değişiklikleri kaydetmek**. 

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, tür adı olarak **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-halogen-software-test-user"></a>Halojensiz yazılım test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon halojensiz yazılım adlı bir kullanıcı oluşturmaktır.

**Britta Simon halojensiz yazılım adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **halojensiz yazılım** yönetici olarak uygulama.

2. Tıklatın **kullanıcı Merkezi** sekmesini ve ardından **kullanıcı oluştur**.
   
    ![Azure AD Connect nedir?][300]  

3. Üzerinde **yeni kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Azure AD Connect nedir?][301]

    a. İçinde **ad** metin kutusuna, tür ilk gibi kullanıcı adını **Britta**.
    
    b. İçinde **Soyadı** metin kutusuna, türü kullanıcının soyadını gibi **Simon**. 

    c. İçinde **kullanıcıadı** metin kutusuna, türü **Britta Simon**, Azure portalında olduğu gibi kullanıcı adı.

    d. İçinde **parola** metin kutusuna, Britta parolasını yazın.
    
    e. **Kaydet** düğmesine tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta halojensiz yazılıma erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon halojensiz yazılım atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **halojensiz yazılım**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Erişim paneli halojensiz yazılım parçasında tıklattığınızda, otomatik olarak halojensiz yazılım uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png

[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png

[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png

[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_203.png

[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png

[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png
