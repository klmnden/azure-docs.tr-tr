---
title: "Öğretici: Azure Active Directory Tümleştirme ile LearnUpon | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile LearnUpon arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 40e6df0db7651488642e774512f55fbd6805809a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Öğretici: Azure Active Directory Tümleştirme LearnUpon ile

Bu öğreticide, Azure Active Directory (Azure AD) ile LearnUpon tümleştirmek öğrenin.

LearnUpon Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- LearnUpon erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için LearnUpon (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme LearnUpon ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir LearnUpon çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden LearnUpon ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-learnupon-from-the-gallery"></a>Galeriden LearnUpon ekleme
Azure AD LearnUpon tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden LearnUpon eklemeniz gerekir.

**Galeriden LearnUpon eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **LearnUpon**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. Sonuçlar panelinde seçin **LearnUpon**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı LearnUpon sınayın.

Tekli çalışmaya oturum için Azure AD LearnUpon karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının LearnUpon ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

LearnUpon içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma LearnUpon ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[LearnUpon test kullanıcısı oluşturma](#creating-a-learnupon-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı LearnUpon sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma LearnUpon uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile LearnUpon yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **LearnUpon** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. Üzerinde **LearnUpon etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.learnupon.com/saml/consumer`

    > [!NOTE] 
    > Lütfen bu gerçek değer olmadığını unutmayın. Bu değer ile gerçek yanıt URL'si güncelleştirmeniz gerekir. Bu değer kişi almak için [LearnUpon destek ekibi](https://www.learnupon.com/features/support/).



4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (ham)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. Üzerinde **LearnUpon yapılandırma** 'yi tıklatın **yapılandırma LearnUpon** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. Başka bir tarayıcı örneği ve oturum açma LearnUpon bir yönetici hesabıyla açın. 

8. Tıklatın **ayarları** sekmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. Tıklatın **çoklu oturum açma - SAML**ve ardından **genel ayarları** SAML ayarlarını yapılandırmak için.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. İçinde **genel ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. Seçin **etkin**.

    b. Seçin **sürüm** olarak **2.0**.

    c. Seçin **Skip koşullar** olarak **Hayır**.

    d. İçinde **SAML belirteci sonrası parametre adı** metin kutusuna, doğrulandı ve kimlik doğrulaması - örneğin SAML onayı içeren isteği SAML tüketici URL'ye post parametresinin adı belirtilen yukarıda türü **SAMLResponse** .

    e. İçinde **ad tanımlayıcısı biçimi** metin kutusuna, where, SAML onayı kullanıcı tanımlayıcısı (e-posta adresi) gösteren değeri bulunduğu - örneğin türü **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: emailAddress**.
  
    f. İçinde **tanımlamak sağlayıcı konumu** metin kutusuna, karşıya yüklenen simge, Azure portalı oturum açma ekranından tıklatırsanız kullanıcıları gönderildiği gösteren değeri yazın.
  
    g. İçinde **oturum kapatma URL'si** metin kutusuna, Yapıştır **Sign-Out URL** Azure portalından kopyalanan.
    
    h. Tıklatın **parmak baskı siparişi yönetmek**ve indirilen sertifikanızın parmak izi karşıya yükleyin.

11. Tıklatın **kullanıcı ayarlarını**ve ardından aşağıdaki adımları gerçekleştirin:
   
     ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    a. İçinde **ilk ad tanımlayıcısı biçimi** metin kutusuna, bize, SAML onayı burada kullanıcılar firstname içinde söyler değerin bulunduğu - örneğin türü: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
  
    b. İçinde **son ad tanımlayıcısı biçimi** metin kutusuna, bize, SAML onayı burada kullanıcılar lastname içinde söyler değerin bulunduğu - örneğin türü: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname** .

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-learnupon-test-user"></a>LearnUpon test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde LearnUpon adlı bir kullanıcı oluşturmaktır. LearnUpon yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa LearnUpon erişme denemesi sırasında oluşturulur. [Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-single-sign-on).

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, başvurmanız gerekir [LearnUpon destek ekibi](https://www.learnupon.com/features/support/). 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta LearnUpon için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**LearnUpon için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **LearnUpon**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LearnUpon parçasında tıklattığınızda, otomatik olarak LearnUpon uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png

