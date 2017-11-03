---
title: "Öğretici: Azure Active Directory Tümleştirme Symantec Web güvenlik hizmetini (WSS) | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Symantec Web güvenlik hizmetini (WSS) arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 61576d3a915d209e7355e04432e586dcf66e7c5a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Öğretici: Azure Active Directory Tümleştirme Symantec Web güvenlik hizmetini (WSS)

Bu öğreticide, böylece WSS SAML kimlik doğrulaması kullanarak Azure AD içinde sağlanan bir son kullanıcı kimlik doğrulaması ve kullanıcı veya grubu düzeyi ilkesi kuralları zorla Symantec Web güvenlik hizmetini (WSS) hesabınızı Azure Active Directory (Azure AD) hesabınız ile tümleştirmek öğreneceksiniz.

Symantec Web güvenlik hizmetini (WSS) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tüm son kullanıcılar ve gruplar WSS hesabınızdan, Azure AD portalı tarafından kullanılan yönetin. 

- Son kullanıcıların kendi Azure AD kimlik bilgilerini kullanarak WSS içindeki doğrulatmak izin verin.

- Kullanıcı zorlama etkinleştirmek ve WSS hesabınızda tanımlanan düzeyi ilke kuralları gruplayın.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Symantec Web güvenlik hizmetini (WSS) ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Symantec Web güvenlik hizmetini (WSS) hesabı

> [!NOTE]
> Bu öğreticide adımları test etmek için üretim amaç için kullanılmakta olan bir WSS hesabı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, şu anda bu test için üretim amaçla kullanılan WSS hesabınızı kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD hesabınızda tanımlanan son kullanıcı kimlik bilgilerini kullanarak WSS için çoklu oturum açmayı etkinleştirmek için Azure AD yapılandırır.
Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Symantec Web güvenlik hizmetini (WSS) uygulama ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a>Galeriden Symantec Web güvenlik hizmetini (WSS) ekleme
Azure AD tümleştirmeye Symantec Web güvenlik hizmetini (WSS) yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Symantec Web güvenlik hizmetini (WSS) eklemeniz gerekir.

**Galeriden Symantec Web güvenlik hizmetini (WSS) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Symantec Web güvenlik hizmetini (WSS)**seçin **Symantec Web güvenlik hizmetini (WSS)** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Symantec Web güvenlik hizmetini (WSS)](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Symantec Web güvenlik hizmeti ("Britta Simon" adlı bir test kullanıcı tabanlı WSS) test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Symantec Web güvenlik hizmetini (WSS) bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Symantec Web güvenlik hizmetini (WSS) arasında bir bağlantı ilişkisi kurulması gerekir.

Symantec Web güvenlik hizmetini (WSS) içindeki değeri atamak **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma Symantec Web güvenlik hizmetini (WSS) sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Symantec Web güvenlik hizmetini (WSS) test kullanıcısı oluşturma](#create-a-symantec-web-security-service-wss-test-user)**  - Britta Simon, karşılık gelen Symantec Web güvenlik hizmetini (WSS), kullanıcı Azure AD gösterimini bağlı olması.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Symantec Web güvenlik hizmetini (WSS) uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Symantec Web güvenlik hizmetini (WSS) ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Symantec Web güvenlik hizmetini (WSS)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. Üzerinde **Symantec Web güvenlik hizmetini (WSS) etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Symantec Web güvenlik hizmetini (WSS) etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`https://saml.threatpulse.net:8443/saml/saml_realm`

    b. İçinde **yanıt URL'si** metin kutusuna, URL'yi yazın:`https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`

    > [!NOTE]
    > Temasa [Symantec Web güvenlik hizmetini (WSS) istemci destek ekibi](https://www.symantec.com/contact-us) varsa değerlerini **tanımlayıcısı** ve **yanıt URL'si** herhangi bir nedenden dolayı çalışmıyor.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-symantec-tutorial/tutorial_general_400.png)
    
6. Çoklu oturum açma Symantec Web güvenlik hizmetini (WSS) tarafında yapılandırmak için WSS çevrimiçi belgelerine başvurun. İndirilen **meta veri XML** dosya WSS Portalı'na aktarılması gerekir. Kişi [Symantec Web güvenlik hizmetini (WSS) destek ekibi](https://www.symantec.com/contact-us) WSS portalındaki yapılandırmayla yardıma ihtiyacınız varsa.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-symantec-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-symantec-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-symantec-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-symantec-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a>Symantec Web güvenlik hizmetini (WSS) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Symantec Web güvenlik hizmetini (WSS) adlı bir kullanıcı oluşturun. Karşılık gelen son kullanıcı adı WSS Portalı'nda el ile oluşturulabilir veya birkaç dakika (~ 15 dakika) sonra WSS portalına eşitlenecek Azure AD'de sağlanan kullanıcıları/grupları için bekleyebilirsiniz. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. Web sitelerine göz atmak için kullanılan son kullanıcı makine ortak IP adresini de Symantec Web güvenlik hizmetini (WSS) Portalı'nda sağlanması gerekir.

> [!NOTE]
> Lütfen [burayı](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) makinenizi almak için kullanıcının ortak IP adresi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Symantec Web güvenlik hizmetini (WSS) erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Symantec Web güvenlik hizmetini (WSS) Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Symantec Web güvenlik hizmetini (WSS)**.

    ![Uygulamalar listesinde Symantec Web güvenlik hizmetini (WSS) bağlantısı](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, WSS hesabınızı SAML kimlik doğrulaması için Azure AD kullanacak şekilde yapılandırdıktan artık, çoklu oturum açma işlevselliğini test edeceksiniz.

Web tarayıcınızı açın ve bir siteye göz atın çalıştığınızda web tarayıcınızı WSS, proxy trafiğini yapılandırdıktan sonra Azure oturum açma sayfasına yönlendirilir. Azure AD (diğer bir deyişle, BrittaSimon) içinde sağlanan test son kullanıcı kimlik bilgilerini girin ve ilişkili parola. Kimlik doğrulaması yapıldıktan sonra seçtiğiniz Web sitesine göz atmak mümkün olur. WSS tarafında BrittaSimon kullanıcı olarak o siteye gözatmaya çalıştığınızda WSS blok sayfasını görmelisiniz sonra belirli bir siteye gözatma BrittaSimon engellemek için ilke kuralı oluşturmanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_203.png

