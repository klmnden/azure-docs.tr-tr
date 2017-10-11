---
title: "Öğretici: Azure Active Directory Tümleştirme ile Mixpanel | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Mixpanel arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3dd11b3477de1329c1c8e45a6dbf212b1635fd95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a>Öğretici: Azure Active Directory Tümleştirme Mixpanel ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Mixpanel tümleştirmek öğrenin.

Mixpanel Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Mixpanel erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Mixpanel (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Mixpanel ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Mixpanel çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Mixpanel ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-mixpanel-from-the-gallery"></a>Galeriden Mixpanel ekleme
Azure AD Mixpanel tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Mixpanel eklemeniz gerekir.

**Galeriden Mixpanel eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Mixpanel**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_search.png)

5. Sonuçlar panelinde seçin **Mixpanel**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Mixpanel sınayın.

Tekli çalışmaya oturum için Azure AD Mixpanel karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Mixpanel ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Mixpanel içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Mixpanel ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Mixpanel test kullanıcısı oluşturma](#creating-a-mixpanel-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Mixpanel sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Mixpanel uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Mixpanel yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Mixpanel** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_samlbase.png)

3. Üzerinde **Mixpanel etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_url.png)

     İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın:`https://mixpanel.com/login/`

    > [!NOTE] 
    > Lütfen adresindeki kayıt [https://mixpanel.com/register/](https://mixpanel.com/register/) oturum açma kimlik bilgileri ve iletişim kurmak için [Mixpanel destek ekibi](mailto:support@mixpanel.com) , kiracınızın SSO ayarlarını etkinleştirmek için. Ayrıca, üzerinde oturum URL değeri gerekirse Mixpanel destek ekibinden alabilirsiniz. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-mixpanel-tutorial/tutorial_general_400.png)

6. Üzerinde **Mixpanel yapılandırma** 'yi tıklatın **yapılandırma Mixpanel** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_configure.png) 

7. Farklı bir tarayıcı penceresinde Mixpanel uygulamanıza bir yönetici olarak oturum.

8. Sayfanın en altındaki üzerinde biraz tıklatın **gear** sol alt köşesindeki simgesi. 
   
    ![Mixpanel çoklu oturum açma](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_06.png) 

9. Tıklatın **erişim güvenlik** sekmesini ve ardından **Ayarları Değiştir**.
   
    ![Mixpanel ayarları](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_08.png) 

10. Üzerinde **sertifikanızı değiştirme** iletişim sayfasında, tıklatın **dosya** indirilen sertifikanızı karşıya yükleyin ve ardından **sonraki**.
   
    ![Mixpanel ayarları](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_09.png) 

11.  Kimlik doğrulama URL'si metin kutusuna üzerinde **, kimlik doğrulaması URL'sini değiştirmek** iletişim sayfasında, değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** hangi Azure portalından kopyaladığınız ve ardından **sonraki**.
   
   ![Mixpanel ayarları](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_10.png) 

12. **Bitti**’ye tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-mixpanel-test-user"></a>Mixpanel test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Mixpanel adlı bir kullanıcı oluşturmaktır. 

1. Mixpanel şirket sitenize yönetici olarak oturum açma.

2. Sayfanın üzerinde açmak için sol üst köşesinde küçük dişli düğmeyi tıklatın **ayarları** penceresi.

3. Tıklatın **takım** sekmesi.

4. İçinde **ekip üyesine** metin kutusuna, Azure Britta'nın e-posta adresini yazın.
   
    ![Mixpanel ayarları](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_11.png) 

5. Tıklatın **davet**. 

> [!Note]
> Kullanıcı profili ayarlamak için bir e-posta alırsınız.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Mixpanel için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Mixpanel için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Mixpanel**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Mixpanel parçasında tıklattığınızda, otomatik olarak Mixpanel uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_203.png

