---
title: "Öğretici: Azure Active Directory Tümleştirme ile Greenhouse | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Greenhouse arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 78ec1766-4f79-4f16-9a66-d5584c4b6151
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: d3aba4aab8ded8749db2bf8197f57a6763008c60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Öğretici: Azure Active Directory Tümleştirme Greenhouse ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Greenhouse tümleştirmek öğrenin.

Greenhouse Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Greenhouse için erişimi olan Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Greenhouse (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Greenhouse ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Greenhouse çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Greenhouse ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-greenhouse-from-the-gallery"></a>Galeriden Greenhouse ekleme
Azure AD Greenhouse tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Greenhouse eklemeniz gerekir.

**Galeriden Greenhouse eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Greenhouse**seçin **Greenhouse** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde greenhouse](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Greenhouse sınayın.

Tekli çalışmaya oturum için Azure AD Greenhouse karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Greenhouse ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Greenhouse içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Greenhouse ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Greenhouse test kullanıcısı oluşturma](#create-a-greenhouse-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Greenhouse sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Greenhouse uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Greenhouse yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Greenhouse** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_samlbase.png)

3. Üzerinde **Greenhouse etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Greenhouse etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.greenhouse.io`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.greenhouse.io`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Greenhouse istemci destek ekibi](https://www.greenhouse.io/contact) bu değerleri almak için. 
 


4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-greenhouse-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **Greenhouse** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [Greenhouse destek ekibi](http://www.greenhouse.io/contact).

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-greenhouse-test-user"></a>Greenhouse test kullanıcısı oluşturma

Azure AD kullanıcıların Greenhouse oturum etkinleştirmek için bunların Greenhouse sağlanmalıdır. Greenhouse söz konusu olduğunda, sağlama bir el ile bir görevdir.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Greenhouse tarafından sağlanan veya herhangi diğer Greenhouse kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 

**Kullanıcı hesaplarını sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Greenhouse** yönetici olarak şirket site.

2. Üstteki menüde tıklatın **yapılandırma**ve ardından **kullanıcılar**.
   
   ![Kullanıcıların](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "kullanıcılar")

3. Tıklatın **yeni kullanıcılar**.
   
   ![Yeni kullanıcı](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "yeni kullanıcı")

4. İçinde **yeni kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Yeni Kullanıcı Ekle](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "yeni kullanıcı Ekle")

   a. İçinde **girin kullanıcı e-postaları** metin kutusu, geçerli bir Azure Active Directory hesabı sağlamak istediğiniz e-posta adresini yazın.

   b. **Kaydet** düğmesine tıklayın.    
   
      >[!NOTE]
      >Azure Active Directory hesap sahipleri etkin duruma gelmesi hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Greenhouse için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Greenhouse için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Greenhouse**.

    ![Uygulamalar listesinde Greenhouse bağlantı](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Greenhouse parçasında tıklattığınızda, otomatik olarak Greenhouse uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_203.png

