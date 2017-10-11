---
title: "Öğretici: Azure Active Directory Tümleştirme ile Lucidchart | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Lucidchart arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1068d364-11f3-43b5-bd6d-26f00ecd5baa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 2dea669f03c893632c08d30feeb3173efc2d8243
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Öğretici: Azure Active Directory Tümleştirme Lucidchart ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Lucidchart tümleştirmek öğrenin.

Lucidchart Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Lucidchart erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Lucidchart (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Lucidchart ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Lucidchart çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Lucidchart ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-lucidchart-from-the-gallery"></a>Galeriden Lucidchart ekleme
Azure AD Lucidchart tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Lucidchart eklemeniz gerekir.

**Galeriden Lucidchart eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Lucidchart**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_search.png)

5. Sonuçlar panelinde seçin **Lucidchart**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Lucidchart sınayın.

Tekli çalışmaya oturum için Azure AD Lucidchart karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Lucidchart ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Lucidchart içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Lucidchart ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Lucidchart test kullanıcısı oluşturma](#creating-a-lucidchart-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Lucidchart sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Lucidchart uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Lucidchart yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Lucidchart** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_samlbase.png)

3. Üzerinde **Lucidchart etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın:`https://chart2.office.lucidchart.com/saml/sso/azure`

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-lucidchart-tutorial/tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde Lucidchart şirket sitenize yönetici olarak oturum açın.

7. Üstteki menüde tıklatın **takım**.
   
    ![Takım](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "ekibi")

8. Tıklatın **uygulamaları \> SAML yönetmek**.
   
    ![SAML yönetmek](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "SAML yönetme")

9. Üzerinde **SAML kimlik doğrulaması ayarlarını** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    a. Seçin **SAML kimlik doğrulamasını etkinleştir**ve ardından **isteğe bağlı**.

    ![SAML kimlik doğrulaması ayarlarını](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "SAML kimlik doğrulama ayarları")
 
    b. İçinde **etki alanı** metin kutusuna, etki alanınızın yazın ve ardından **değişiklik sertifika**.

    ![Sertifika değiştirme](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "sertifika değiştirme")
 
    c. İndirilen meta veri dosyanızı açın, içeriği Kopyala ve ardından yapıştırın **meta veriler karşıya** metin kutusu.

    ![Meta veriler karşıya](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "meta veriler karşıya yükle")
 
    d. Seçin **takım yeni kullanıcılara otomatik olarak Ekle**ve ardından **değişiklikleri kaydetmek**.

    ![Değişiklikleri kaydetmek](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "Değişiklikleri Kaydet")

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-lucidchart-test-user"></a>Lucidchart test kullanıcısı oluşturma

Kullanıcı için Lucidchart hazırlama yapılandırmanız için eylem öğe yok.  Atanmış bir kullanıcı erişim paneli kullanılarak Lucidchart oturum açmaya çalıştığında Lucidchart kullanıcının var olup olmadığını denetler.  

Hiçbir kullanıcı hesabı varsa kullanılabilir henüz, Lucidchart tarafından otomatik olarak oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Lucidchart için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Lucidchart için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Lucidchart**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Lucidchart parçasında tıklattığınızda, otomatik olarak Lucidchart uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_203.png

