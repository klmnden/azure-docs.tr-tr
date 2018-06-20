---
title: 'Öğretici: Azure Active Directory Tümleştirme Origamisi ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Origamisi arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 019d05cfb0339341e541e6fb3327568e8639bcda
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36230143"
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a>Öğretici: Azure Active Directory Tümleştirme ile Origamisi

Bu öğreticide, Azure Active Directory (Azure AD) ile Origamisi tümleştirmek öğrenin.

Origamisi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Origamisi erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Origamisi (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Origamisi ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Origamisi çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Origamisi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-origami-from-the-gallery"></a>Galeriden Origamisi ekleme
Azure AD Origamisi tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Origamisi eklemeniz gerekir.

**Galeriden Origamisi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Origamisi**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/origami-tutorial/tutorial_origami_search.png)

5. Sonuçlar panelinde seçin **Origamisi**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/origami-tutorial/tutorial_origami_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmak ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Origamisi sınayın.

Tekli çalışmaya oturum için Azure AD Origamisi karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Origamisi ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Origamisi içinde atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Origamisi ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Origamisi test kullanıcısı oluşturma](#creating-an-origami-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Origamisi sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Origamisi uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Origamisi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Origamisi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_samlbase.png)

3. Üzerinde **Origamisi etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://live.origamirisk.com/origami/account/login?account=<companyname>`

    > [!NOTE] 
    > Değer gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [Origamisi istemci destek ekibi](https://wordpress.org/support/theme/origami) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_general_400.png)

6. Üzerinde **Origamisi yapılandırma** 'yi tıklatın **yapılandırma Origamisi** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_configure.png) 

7. Yönetici hakları Origamisi hesabıyla oturum açın.

8. Üstteki menüde tıklatın **yönetici**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_51.png)

9. Üzerinde tek oturum Kurulumu iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_531.png)

    a. Seçin **etkinleştirmek çoklu oturum açma**.

    b. İçinde **kimlik sağlayıcısının oturum açma sayfası URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    c. İçinde **kimlik sağlayıcısının Sign-out sayfa URL'si** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

    d. Tıklatın **Gözat** Azure portalından indirdiğiniz sertifikayı karşıya yüklemek için.

    e. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/origami-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/origami-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/origami-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/origami-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-origami-test-user"></a>Bir Origamisi test kullanıcısı oluşturma

Bu bölümde, içinde Origamisi Britta Simon adlı bir kullanıcı oluşturun. 

1. Yönetici hakları Origamisi hesabıyla oturum açın.

2. Üstteki menüde tıklatın **yönetici**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_51.png)

3. Üzerinde **kullanıcılar ve güvenlik** iletişim kutusunda, tıklatın **kullanıcılar**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_54.png)

4. Tıklatın **yeni kullanıcı Ekle**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_55.png)

5. Yeni Kullanıcı Ekle iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_56.png)

    a. İçinde **kullanıcı adı** metin kutusuna, bir kullanıcı gibi e-posta girin **brittasimon@contoso.com**.

    b. İçinde **parola** metin kutusuna, bir parola yazın.

    c. İçinde **parolayı onayla** metin kutusuna, parolayı yeniden yazın.

    d. İçinde **ad** metin gibi kullanıcının ilk adını girin **Britta**.

    e. İçinde **Soyadı** metin kutusuna, son kullanıcı gibi adını **Simon**.

    f. **Kaydet**’e tıklayın.
   
    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_57.png)

6. Ata **kullanıcı rolleri** ve **istemci erişimi** kullanıcı. 
   
    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Origamisi için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**İçin Origamisi Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Origamisi**.

    ![Çoklu oturum açmayı yapılandırın](./media/origami-tutorial/tutorial_origami_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Origamisi parçasında tıklattığınızda, otomatik olarak Origamisi uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/origami-tutorial/tutorial_general_01.png
[2]: ./media/origami-tutorial/tutorial_general_02.png
[3]: ./media/origami-tutorial/tutorial_general_03.png
[4]: ./media/origami-tutorial/tutorial_general_04.png

[100]: ./media/origami-tutorial/tutorial_general_100.png

[200]: ./media/origami-tutorial/tutorial_general_200.png
[201]: ./media/origami-tutorial/tutorial_general_201.png
[202]: ./media/origami-tutorial/tutorial_general_202.png
[203]: ./media/origami-tutorial/tutorial_general_203.png

