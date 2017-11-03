---
title: "Öğretici: Azure Active Directory Tümleştirme ile Litmos | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Litmos arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: jeedes
ms.assetid: cfaae4bb-e8e5-41d1-ac88-8cc369653036
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: ef1b5860ba0a406022bbd11afb366d14bee2c57d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Öğretici: Azure Active Directory Tümleştirme Litmos ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Litmos tümleştirmek öğrenin.

Litmos Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Litmos erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Litmos (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Litmos ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Litmos çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Litmos ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-litmos-from-the-gallery"></a>Galeriden Litmos ekleme
Azure AD Litmos tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Litmos eklemeniz gerekir.

**Galeriden Litmos eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Litmos**seçin **Litmos** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Litmos](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Litmos sınayın.

Tekli çalışmaya oturum için Azure AD Litmos karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Litmos ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Litmos içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Litmos ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Litmos test kullanıcısı oluşturma](#create-a-litmos-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Litmos sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Litmos uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Litmos yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Litmos** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_samlbase.png)

3. Üzerinde **Litmos etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Litmos etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.litmos.com/account/Login`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.litmos.com/integration/samllogin`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Gerçek tanımlayıcı ve yanıt URL'si, daha sonra öğreticide veya kişi açıklanacak bu değerleri güncelleştirmek [Litmos destek ekibi](https://www.litmos.com/contact-us/) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_certificate.png)

5. Özelleştirmeniz gerekiyorsa yapılandırmasının bir parçası olarak **SAML belirteci öznitelikleri** Litmos uygulamanız için.

    ![Öznitelik bölümü](./media/active-directory-saas-litmos-tutorial/tutorial_attribute.png)
           
    | Öznitelik adı   | Öznitelik değeri |   
    | ---------------  | ----------------|
    | FirstName |User.givenName |
    | Soyadı  |User.surname |
    | E-posta |User.Mail |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Öznitelik Ekle](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_04.png)

    ![Öznitelik Dailog Ekle](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.     

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-litmos-tutorial/tutorial_general_400.png)

7. Farklı bir tarayıcı penceresinde Litmos şirket sitenize yönetici olarak oturum.

8. Sol taraftaki gezinti çubuğunda tıklayın **hesapları**.
   
    ![Uygulama tarafında hesaplar bölümü][22] 

9. Tıklatın **tümleştirmeler** sekmesi.
   
    ![Tümleştirme sekmesi][23] 

10. Üzerinde **tümleştirmeler** sekmesinde, aşağı kaydırarak **3 taraf tümleştirmeler**ve ardından **SAML 2.0** sekmesi.
   
    ![SAML 2.0 bölümü][24] 

11. Değerin altında kopyalama **litmos için SAML uç noktası:** ve yapıştırın **yanıt URL'si** metin kutusuna **Litmos etki alanı ve URL'leri** Azure portalı bölümünde. 
   
    ![SAML uç noktası][26] 

12. İçinde **Litmos** uygulama, aşağıdaki adımları gerçekleştirin:
    
     ![Litmos uygulama][25] 
     
     a. Tıklatın **etkinleştirmek SAML**.
    
     b. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **SAML X.509 sertifikası** metin kutusu.
     
     c. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-litmos-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-litmos-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
  
### <a name="create-a-litmos-test-user"></a>Litmos test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Litmos adlı bir kullanıcı oluşturmaktır.  
Zaman içinde sadece Litmos uygulamasının desteklediği sağlama. Bunun anlamı bir kullanıcı hesabı otomatik olarak gerekirse erişim paneli kullanarak uygulamayı erişme denemesi sırasında oluşturulur.

**İçinde Litmos Britta Simon adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Farklı bir tarayıcı penceresinde Litmos şirket sitenize yönetici olarak oturum.

2. Sol taraftaki gezinti çubuğunda tıklayın **hesapları**.
   
    ![Uygulama tarafında hesaplar bölümü][22] 

3. Tıklatın **tümleştirmeler** sekmesi.
   
    ![Tümleştirmeler sekmesi][23] 

4. Üzerinde **tümleştirmeler** sekmesinde, aşağı kaydırarak **3 taraf tümleştirmeler**ve ardından **SAML 2.0** sekmesi.
   
    ![SAML 2.0][24] 
    
5. Seçin **otomatik olarak üret kullanıcılar**
   
    ![Otomatik olarak üret kullanıcılar][27] 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Litmos için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Litmos için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Litmos**.

    ![Uygulamalar listesinde Litmos bağlantı](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.  

Erişim paneli Litmos parçasında tıklattığınızda, otomatik olarak Litmos uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[100]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png

