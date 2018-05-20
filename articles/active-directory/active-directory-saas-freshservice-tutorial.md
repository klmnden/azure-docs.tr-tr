---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Freshservice | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Freshservice arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: a9e50b43308754dae78b0f56c19a539939d19676
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Öğretici: Azure Active Directory Tümleştirme Freshservice ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Freshservice tümleştirmek öğrenin.

Freshservice Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Freshservice erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Freshservice (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Freshservice ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Freshservice çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Freshservice ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-freshservice-from-the-gallery"></a>Galeriden Freshservice ekleme
Azure AD Freshservice tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Freshservice eklemeniz gerekir.

**Galeriden Freshservice eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Freshservice**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_search.png)

5. Sonuçlar panelinde seçin **Freshservice**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Freshservice sınayın.

Tekli çalışmaya oturum için Azure AD Freshservice karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Freshservice ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Freshservice içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Freshservice ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Freshservice test kullanıcısı oluşturma](#creating-a-freshservice-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Freshservice sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Freshservice uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Freshservice yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Freshservice** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. Üzerinde **Freshservice etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<democompany>.freshservice.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<democompany>.freshservice.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Freshservice istemci destek ekibi](https://support.freshservice.com/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalama **parmak İZİ** sertifika değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshservice-tutorial/tutorial_general_400.png)

6. Üzerinde **Freshservice yapılandırma** 'yi tıklatın **yapılandırma Freshservice** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_configure.png) 

7. Farklı web tarayıcısı penceresinde Freshservice şirket sitenize yönetici olarak oturum açın.

8. Üstteki menüde tıklatın **yönetici**.
   
    ![Yönetici](./media/active-directory-saas-freshservice-tutorial/ic790814.png "yönetici")

9. İçinde **müşteri portalı**, tıklatın **güvenlik**.
   
    ![Güvenlik](./media/active-directory-saas-freshservice-tutorial/ic790815.png "güvenlik")

10. İçinde **güvenlik** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı](./media/active-directory-saas-freshservice-tutorial/ic790816.png "çoklu oturum açmayı")
   
    a. Anahtar **çoklu oturum açmayı**.

    b. Seçin **SAML SSO**.

    c. İçinde **SAML oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    d. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.

    e. İçinde **güvenlik sertifika parmak izi** metin kutusuna, Yapıştır **parmak İZİ** Azure portalından kopyaladığınız sertifika değeri.

    f. **Kaydet**’e tıklayın
   
> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshservice-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshservice-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshservice-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-freshservice-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-freshservice-test-user"></a>Freshservice test kullanıcısı oluşturma

Azure AD kullanıcıları için FreshService oturum açmak etkinleştirmek için bunların FreshService sağlanmalıdır. FreshService söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **FreshService** yönetici olarak şirket site.

2. Üstteki menüde tıklatın **yönetici**.
   
    ![Yönetici](./media/active-directory-saas-freshservice-tutorial/ic790814.png "yönetici")

3. İçinde **kullanıcı yönetimi** 'yi tıklatın **sahiplerini**.
   
    ![Sahiplerini](./media/active-directory-saas-freshservice-tutorial/ic790818.png "sahiplerini")

4. Tıklatın **yeni istek**.
   
    ![Yeni sahiplerini](./media/active-directory-saas-freshservice-tutorial/ic790819.png "yeni sahiplerini")

5. İçinde **yeni istek** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni İstek](./media/active-directory-saas-freshservice-tutorial/ic790820.png "yeni istek")   

    a. Girin **ad** ve **e-posta** istediğiniz ilgili metin kutularına sağlamayı geçerli bir Azure Active Directory hesabı öznitelikleri.

    b. **Kaydet**’e tıklayın.
   
    >[!NOTE]
    >Azure Active Directory hesap sahibi etkin duruma gelmesi hesabı onaylamak için bir bağlantı içeren bir e-posta alır
    >  

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına FreshService tarafından sağlanan veya herhangi diğer FreshService kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
>  

![Kullanıcı atama][200] 

**Freshservice için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Freshservice**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli Freshservice parçasında tıklattığınızda, otomatik olarak Freshservice uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_203.png

