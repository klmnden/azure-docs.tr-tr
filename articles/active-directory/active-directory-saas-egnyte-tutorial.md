---
title: "Öğretici: Azure Active Directory Tümleştirme ile Egnyte | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Egnyte arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 62d01333b61e73c83588d2d1701c0c300df4ab1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Öğretici: Azure Active Directory Tümleştirme Egnyte ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Egnyte tümleştirmek öğrenin.

Egnyte Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Egnyte erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Egnyte (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Egnyte ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Egnyte çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Egnyte ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-egnyte-from-the-gallery"></a>Galeriden Egnyte ekleme
Azure AD Egnyte tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Egnyte eklemeniz gerekir.

**Galeriden Egnyte eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Egnyte**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_search.png)

5. Sonuçlar panelinde seçin **Egnyte**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Egnyte ile test etme

Tekli çalışmaya oturum için Azure AD Egnyte karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Egnyte ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Egnyte içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Egnyte ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Egnyte test kullanıcısı oluşturma](#creating-an-egnyte-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Egnyte sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Egnyte uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Egnyte yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Egnyte** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. Üzerinde **Egnyte etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.egnyte.com`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [Egnyte istemci destek ekibi](https://www.egnyte.com/corp/contact_egnyte.html) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-egnyte-tutorial/tutorial_general_400.png)

6. Üzerinde **Egnyte yapılandırma** 'yi tıklatın **yapılandırma Egnyte** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_configure.png) 

7. Farklı web tarayıcısı penceresinde Egnyte şirket sitenize yönetici olarak oturum açın.

8. Tıklatın **ayarları**.
   
   ![Ayarları](./media/active-directory-saas-egnyte-tutorial/ic787819.png "ayarları")

9. Menüye tıklayın **ayarları**.

   ![Ayarları](./media/active-directory-saas-egnyte-tutorial/ic787820.png "ayarları")

10. Tıklatın **yapılandırma** sekmesini ve ardından **güvenlik**.

    ![Güvenlik](./media/active-directory-saas-egnyte-tutorial/ic787821.png "güvenlik")

11. İçinde **çoklu oturum açma kimlik doğrulaması** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma kimlik doğrulaması](./media/active-directory-saas-egnyte-tutorial/ic787822.png "çoklu oturum açma kimlik doğrulaması")   
    
    a. Olarak **çoklu oturum açma kimlik doğrulaması**seçin **SAML 2.0**.
   
    b. Olarak **kimlik sağlayıcısı**seçin **Azuread'i**.
   
    c. Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalına kopyalanan **kimlik sağlayıcısı oturum açma URL'si** metin kutusu.
   
    d. Yapıştır **SAML varlık kimliği** Azure portalından kopyalanan **kimlik sağlayıcısı varlık kimliği** metin kutusu.
      
    e. Base-64 kodlanmış sertifikanızı Azure portalından indirdiğiniz Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **kimlik sağlayıcısı sertifikası** metin kutusu.
   
    f. Olarak **kullanıcı eşlemesi varsayılan**seçin **e-posta adresi**.
   
    g. Olarak **etki alanına özgü veren değeriyle kullanın**seçin **devre dışı**.
   
    h. **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-egnyte-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-egnyte-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-egnyte-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-egnyte-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-an-egnyte-test-user"></a>Bir Egnyte test kullanıcısı oluşturma

Azure AD kullanıcıları için Egnyte oturum açmak etkinleştirmek için bunların Egnyte sağlanmalıdır. Egnyte söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı hesaplarını sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Egnyte** yönetici olarak şirket site.

2. Git **ayarları \> kullanıcıları ve grupları**.

3. Tıklatın **yeni kullanıcı Ekle**ve ardından eklemek istediğiniz kullanıcı türünü seçin.
   
   ![Kullanıcıların](./media/active-directory-saas-egnyte-tutorial/ic787824.png "kullanıcılar")

4. İçinde **yeni standart kullanıcı** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Yeni standart kullanıcı](./media/active-directory-saas-egnyte-tutorial/ic787825.png "yeni standart kullanıcı")   

   a. Tür **e-posta**, **kullanıcıadı**ve diğer ayrıntılarını sağlamak istediğiniz geçerli bir Azure Active Directory hesabı.
   
   b. **Kaydet** düğmesine tıklayın.
    
    >[!NOTE]
    >Azure Active Directory hesap sahibi bir bildirim e-posta alacaksınız.
    >

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Egnyte tarafından sağlanan veya herhangi diğer Egnyte kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Egnyte için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Egnyte için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Egnyte**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Egnyte parçasında tıklattığınızda, otomatik olarak Egnyte uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_203.png

