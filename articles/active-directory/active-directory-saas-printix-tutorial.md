---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Printix | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Printix arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4aea7320-b2d5-49e0-9b63-aeaff0f6fe66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 23f77e791cbc171c5007795ebd2652fb3835efed
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34351564"
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a>Öğretici: Azure Active Directory Tümleştirme Printix ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Printix tümleştirmek öğrenin.

Printix Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Printix erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Printix (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Printix ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Printix çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Printix ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-printix-from-the-gallery"></a>Galeriden Printix ekleme
Azure AD Printix tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Printix eklemeniz gerekir.

**Galeriden Printix eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Printix**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-printix-tutorial/tutorial_printix_search.png)

5. Sonuçlar panelinde seçin **Printix**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Printix sınayın.

Tekli çalışmaya oturum için Azure AD Printix karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Printix ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Printix içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Printix ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Printix test kullanıcısı oluşturma](#creating-a-printix-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Printix sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Printix uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Printix yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Printix** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-printix-tutorial/tutorial_printix_samlbase.png)

3. Üzerinde **Printix etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-printix-tutorial/tutorial_printix_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.printix.net`

    > [!NOTE] 
    > Değer gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [Printix istemci destek ekibi](mailto:support@printix.net) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-printix-tutorial/tutorial_printix_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-printix-tutorial/tutorial_general_400.png)

6. Printix kiracınız yönetici olarak oturum.

7. Üstteki menüde sağ üst köşesinde simgesini tıklatın ve seçin "**kimlik doğrulaması**".
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-printix-tutorial/tutorial_printix_06.png)

8. Üzerinde **Kurulum** sekmesine **etkinleştirmek Azure/Office 365 kimlik doğrulaması**
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-printix-tutorial/tutorial_printix_07.png)

9. Üzerinde **Azure** sekmesinde, metin kutusuna Federasyon meta verileri URL'sine Giriş "**Federasyon meta veri belgesi**". 

    Azure AD'den indirilen meta veri xml dosyası ekleme [Printix destek ekibi](mailto:support@printix.net). Ardından xml dosyasını karşıya yüklemek ve Federasyon meta veri URL'sini girin.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-printix-tutorial/tutorial_printix_08.png)
   
10. Tıklayın "**Test**"düğmesini tıklatın ve tıklatın"**Tamam**" testi başarılı olursa düğmesine tıklayın.
   
     Azure active directory sayfasında tıkladıktan sonra gösterilir **test** düğmesi. "Test başarılı oldu" Buraya pop Azure test hesabınız kimlik bilgilerini girdikten sonra bir iletiyi "test Tamam ayarları" anlamına gelir. Ardından **Tamam** düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-printix-tutorial/tutorial_printix_09.png)

11. Tıklatın **kaydetmek** düğmesini "**kimlik doğrulaması**" sayfası.


> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-printix-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-printix-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-printix-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-printix-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-printix-test-user"></a>Printix test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Printix adlı bir kullanıcı oluşturmaktır. Printix yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa Printix erişme denemesi sırasında oluşturulur. 

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, başvurmanız gerekir [Printix destek ekibi](mailto:support@printix.net).
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Printix için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Printix için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Printix**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-printix-tutorial/tutorial_printix_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Printix parçasında tıklattığınızda, otomatik olarak Printix uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-printix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-printix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-printix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-printix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-printix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-printix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-printix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-printix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-printix-tutorial/tutorial_general_203.png

