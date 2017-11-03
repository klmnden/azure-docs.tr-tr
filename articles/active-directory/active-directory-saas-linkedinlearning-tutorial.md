---
title: "Öğretici: Azure Active Directory Tümleştirme ile LinkedIn öğrenme | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve LinkedIn öğrenme arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: jeedes
ms.openlocfilehash: 6ad28cb3adaa63ddc3d3769a650d26ca6a7e2695
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Öğretici: Azure Active Directory Tümleştirme ile LinkedIn öğrenme

Bu öğreticide, Azure Active Directory (Azure AD) ile LinkedIn öğrenme tümleştirmek öğrenin.

LinkedIn öğrenme Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- LinkedIn öğrenme erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için LinkedIn öğrenme açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme LinkedIn öğrenme ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir LinkedIn öğrenme çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden LinkedIn öğrenme ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-linkedin-learning-from-the-gallery"></a>Galeriden LinkedIn öğrenme ekleme
Azure AD LinkedIn öğrenme tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden LinkedIn öğrenme eklemeniz gerekir.

**Galeriden LinkedIn öğrenme eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **LinkedIn öğrenme**. Sonuçları panelinden tıklatın **LinkedIn öğrenme** uygulama eklemek için.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma LinkedIn "Britta Simon" adlı bir test kullanıcı tabanlı Learning ile test etme.

Tekli çalışmaya oturum için Azure AD LinkedIn öğrenme karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının LinkedIn öğrenme ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** LinkedIn learning'de.

Yapılandırma ve Azure AD çoklu oturum açma LinkedIn Learning ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir LinkedIn öğrenme test kullanıcısı oluşturma](#creating-a-linkedin-learning-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma LinkedIn öğrenme uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma LinkedIn öğrenme ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **LinkedIn öğrenme** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. Farklı web tarayıcısı penceresinde LinkedIn öğrenme kiracınız yönetici olarak oturum.

4. İçinde **hesap Merkezi'nde**, tıklatın **genel ayarları** altında **ayarları**. Ayrıca, seçin **öğrenme - varsayılan** aşağı açılan listeden.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. Tıklatın **veya yük ve tek tek alanların formdan kopyalamak için burayı tıklatın** kopyalayıp **varlık kimliği** ve **onaylama tüketici erişim (ACS) URL'si**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. Azure Portal'da altında **LinkedIn öğrenme etki alanı ve URL'leri**, içinde SSO yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP başlatılan** modu

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    a. İçinde **tanımlayıcısı** metin girin **varlık kimliği** LinkedIn portalından kopyalandığından 

    b. İçinde **yanıt URL'si** metin girin **onaylama tüketici erişim (ACS) Url** LinkedIn portalından kopyalandığından

7. SSO içinde yapılandırmak istiyorsanız, **SP tarafından başlatılan**, ardından yapılandırma bölümündeki Gelişmiş URL Göster ayarı seçeneğini tıklatın ve aşağıdaki deseni ile oturum açma URL'si yapılandırın:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. LinkedIn öğrenme uygulamanızı SAML onaylar SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir. Varsayılan değer olan **kullanıcı tanımlayıcısı** olan **user.userprincipalname** ancak LinkedIn öğrenme bu kullanıcının e-posta adresi ile eşlenecek bekliyor. Bunun için kullanabileceğiniz **user.mail** özniteliği listeden veya kuruluş yapılandırmanızı temel alarak uygun öznitelik değeri kullanın. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinlearning-tutorial/updateusermail.png)
    
9. İçinde **kullanıcı öznitelikleri** 'yi tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** ve özniteliklerini ayarlayın. Kullanıcı adında dört talep eklemesi gerekiyor **e-posta**, **departmanı**, **firstname**, ve **lastname** ve ile eşlenecek değer ise **user.mail**, **user.department**, **user.givenname**, ve **user.surname** sırasıyla

    | Öznitelik adı | Öznitelik değeri |
    | --- | --- |
    | E-posta| User.Mail |    
    | Bölüm| User.Department |
    | FirstName| User.givenName |
    | Soyadı| User.surname |
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinlearning-tutorial/userattribute.png)
    
    a. Tıklatın **özniteliği eklemek** özniteliği iletişim kutusunu açın.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_04.png)

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklatın **Tamam**

10. Aşağıdaki adımları gerçekleştirin **adı** öznitelik -

    a. Özniteliği açmak için tıklatın **öznitelik Düzenle** penceresi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinLearning-tutorial/url_update.png)

    b. URL değerinden silme **ad alanı**.
    
    c. Tıklatın **Tamam** ayarı kaydetmek için.

11. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png) 

12. **Kaydet** düğmesine tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_400.png)

13. Git **LinkedIn yönetici ayarları** bölümü. Karşıya yükleme XML dosyası seçeneğini tıklatarak Azure portalından indirdiğiniz XML dosyasını karşıya yükleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

14. Tıklatın **üzerinde** SSO'yu etkinleştirmek için. SSO durumu değişir **bağlı** için **bağlandı**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın. 

### <a name="creating-a-linkedin-learning-test-user"></a>Bir LinkedIn öğrenme test kullanıcısı oluşturma

Bağlantılı öğrenme uygulama destekler. Kullanıcılar yalnızca zaman kullanıcı sağlama ve kimlik doğrulamasından sonra uygulamayı otomatik olarak oluşturulur. Yönetici ayarları LinkedIn öğrenme portal Çevir ' anahtar sayfa **lisansları otomatik olarak ata** sadece etkinleştirmek için etkin sağlama ve bu da bir kullanıcıya lisans atamak.
   
   ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-linkedinLearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta LinkedIn öğrenme erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**LinkedIn öğrenme Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **LinkedIn öğrenme**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LinkedIn öğrenme parçasında tıkladığınızda, Azure oturum açma sayfasına almanız gerekir ve üzerinde başarılı oturum açma sonra bunu LinkedIn öğrenme uygulamanıza almanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_203.png