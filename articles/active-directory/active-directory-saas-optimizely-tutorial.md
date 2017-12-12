---
title: "Öğretici: Azure Active Directory Tümleştirme ile Optimizely | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Optimizely arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: e44e1621632bf2c8a3c4050b718d2721f5132d06
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Öğretici: Azure Active Directory Tümleştirme Optimizely ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Optimizely tümleştirmek öğrenin.

Optimizely Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Optimizely erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Optimizely (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Optimizely ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Optimizely çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Optimizely ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-optimizely-from-the-gallery"></a>Galeriden Optimizely ekleme
Azure AD Optimizely tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Optimizely eklemeniz gerekir.

**Galeriden Optimizely eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Optimizely**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_search.png)

5. Sonuçlar panelinde seçin **Optimizely**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Optimizely ile test etme

Tekli çalışmaya oturum için Azure AD Optimizely karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Optimizely ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Optimizely içinde.

Yapılandırma ve Azure AD çoklu oturum açma Optimizely ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Optimizely test kullanıcısı oluşturma](#creating-an-optimizely-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Optimizely sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Optimizely uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Optimizely yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Optimizely** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_samlbase.png)

3. Üzerinde **Optimizely etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://app.optimizely.net/<instance name>`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`urn:auth0:optimizely:contoso`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Değer, daha sonra öğreticide açıklandığı tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirir. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-optimizely-tutorial/tutorial_general_400.png)

6. Üzerinde **Optimizely yapılandırma** 'yi tıklatın **yapılandırma Optimizely** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_configure.png) 

7. Çoklu oturum açma yapılandırmak için **Optimizely** tarafı, Optimizely hesap yöneticinize başvurun ve indirilen sağlamak **sertifika (Base64)**, ve **SAML çoklu oturum açma hizmet URL'si**. 

8. E-postanıza yanıtta Optimizely üzerinde oturum URL'si (SP tarafından başlatılan SSO'yu) ve tanımlayıcı (hizmet sağlayıcısı varlık kimliği) değerlerini sağlar.

    a. Kopya **SP tarafından başlatılan SSO'yu URL** Optimizely ve içine yapıştırma tarafından sağlanan **oturum üzerinde URL'si** metin kutusuna **Optimizely etki alanı ve URL'ler** Azure Portal'daki bölümü 

    b. Kopya **hizmet sağlayıcısı varlık kimliği** Optimizely ve içine yapıştırma tarafından sağlanan **tanımlayıcısı** metin kutusuna **Optimizely etki alanı ve URL'ler** Azure Portal'daki bölümü 

9. Farklı bir tarayıcı penceresinde Optimizely uygulamanıza oturum.

10. Hesap adı üst sağ alt köşesinde tıklatın ve ardından **hesap ayarlarını**.
   
    ![Azure AD çoklu oturum açma](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

11. Hesap sekmesinde, onay kutusunu **etkinleştirmek SSO** çoklu oturum açma içinde altında **genel bakış** bölümü.
   
    ![Azure AD çoklu oturum açma](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)
    
12. Tıklatın **Kaydet**

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-optimizely-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-optimizely-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** Britta Simon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-an-optimizely-test-user"></a>Bir Optimizely test kullanıcısı oluşturma

Bu bölümde, Optimizely içinde Britta Simon adlı bir kullanıcı oluşturun.

1. Giriş sayfasında, seçin **ortak** sekmesi.

2. Yeni bir ortak çalışanı projeye eklemek için tıklatın **yeni bir ortak çalışanı**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3. E-posta adresi doldurun ve bunları bir rol atayın. Tıklatın **davet**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. Bir e-posta davet alırlar. E-posta adresini kullanarak Optimizely için oturum açma sahiptirler.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Optimizely için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Optimizely için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Optimizely**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Optimizely parçasında tıklattığınızda, otomatik olarak Optimizely uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png

