---
title: 'Öğretici: Azure Active Directory Tümleştirme Amazon Web Hizmetleri (AWS) ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Amazon Web Hizmetleri (AWS) arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2018
ms.author: jeedes
ms.openlocfilehash: 018893a2124f1ab9c98e0728bc90ad0a69cf471f
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Öğretici: Azure Active Directory Tümleştirme Amazon Web Hizmetleri (AWS)

Bu öğreticide, Amazon Web Hizmetleri (AWS) Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Amazon Web Hizmetleri (AWS) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Amazon Web Hizmetleri (AWS) erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Amazon Web Hizmetleri (AWS için) (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Amazon Web Hizmetleri (AWS) ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Amazon Web Hizmetleri (AWS) çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Amazon Web Hizmetleri (AWS) ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Galeriden Amazon Web Hizmetleri (AWS) ekleme
Azure AD tümleştirmeye Amazon Web Hizmetleri (AWS) yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Amazon Web Hizmetleri (AWS) eklemeniz gerekir.

**Amazon Web Hizmetleri (AWS) Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Amazon Web Hizmetleri (AWS)**seçin **Amazon Web Hizmetleri (AWS)** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Amazon Web Hizmetleri (AWS)](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Amazon Web Hizmetleri ("Britta Simon" adlı bir test kullanıcı tabanlı AWS) test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Amazon Web Hizmetleri (AWS) bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Amazon Web Hizmetleri (AWS) arasında bir bağlantı ilişkisi kurulması gerekir.

Amazon Web Hizmetleri (AWS) değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Amazon Web Hizmetleri (AWS) ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Amazon Web Hizmetleri (AWS) test kullanıcısı oluşturma](#create-an-amazon-web-services-aws-test-user)**  - karşılık gelen Britta Simon, Amazon Web Hizmetleri (AWS) kullanıcı Azure AD gösterimini bağlı olması.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Amazon Web Hizmetleri (AWS) uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Amazon Web Hizmetleri (AWS) ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Amazon Web Hizmetleri (AWS)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. Üzerinde **Amazon Web Hizmetleri (AWS) etki alanı ve URL'leri** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiş gibi tüm adımları gerçekleştirin.

    ![Amazon Web Hizmetleri (AWS) etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. Amazon Web Hizmetleri (AWS) yazılım uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.

    ![Çoklu oturum açma attb yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png)   

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı  | Öznitelik Değeri | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rol            | User.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >AWS konsolundan tüm rolleri getirmek için Azure AD'de kullanıcı sağlamayı yapılandırmanız gerekir. Hazırlama adımları bakın.

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma addattb yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. İçinde **Namespace** metin kutusuna, ilgili satır için gösterilen ad alanı değeri yazın.
    
    d. **Tamam**’a tıklayın.

6. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. Farklı bir tarayıcı penceresinde Amazon Web Hizmetleri (AWS) şirket sitenize yönetici olarak oturum.

9. Tıklatın **AWS giriş**.
   
    ![Çoklu oturum açma giriş yapılandırın][11]

10. Tıklatın **kimlik ve erişim yönetimi**. 
   
    ![Çoklu oturum açma kimliğini yapılandırma][12]

11. Tıklatın **kimlik sağlayıcıları**ve ardından **oluşturma sağlayıcısı**. 
   
    ![Çoklu oturum açma sağlayıcısı yapılandırma][13]

12. Üzerinde **yapılandırma sağlayıcısı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin: 
   
    ![Çoklu oturum açma iletişim yapılandırın][14]
 
    a. Olarak **sağlayıcı türü**seçin **SAML**.

    b. İçinde **sağlayıcı adı** metin kutusuna, bir sağlayıcı adı yazın (örneğin: *WAAD*).

    c. İndirilen, karşıya yüklemek için **meta veri dosyası** Azure portalından tıklatın **Dosya Seç**.

    d. Tıklatın **sonraki adım**.

13. Üzerinde **sağlayıcısı bilgilerini doğrulayın** iletişim sayfasında, tıklatın **oluşturma**. 
    
    ![Çoklu oturum açma yapılandırma doğrulayın][15]

14. Tıklatın **rolleri**ve ardından **rol oluşturma**. 
    
    ![Çoklu oturum açma rollerini yapılandırma][16]

15. Üzerinde **rol oluşturma** sayfasında, aşağıdaki adımları gerçekleştirin:  
    
    ![Çoklu oturum açma güvenini yapılandırma][19] 

    a. Seçin **SAML 2.0 Federasyon** altında **seçin güvenilir varlığın türü**.

    b. Altında **bir SAML 2.0 sağlayıcı bölümü seçin**seçin **SAML sağlayıcısı** daha önce oluşturduğunuz (örneğin: *WAAD*)

    c. Seçin **izin programlı ve AWS Yönetim Konsolu erişim**.
  
    d. Tıklatın **sonraki: izinleri**.

16. Üzerinde **ekleme izinleri ilkeleri** iletişim kutusunda, tıklatın **sonraki: gözden geçirme**.  
    
    ![Çoklu oturum açma ilkesini yapılandırma][33]

17. Üzerinde **gözden geçirme** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:   
    
    ![Çoklu oturum açma gözden geçirme yapılandırın][34] 

    a. İçinde **rol adı** metin kutusuna, rolü adı girin.

    b. İçinde **rol açıklaması** metin kutusuna, bir açıklama girin.

    a. Tıklatın **rolü oluşturma**.

    b. Gerektiğinde kadar rolleri oluşturun ve bunları kimlik sağlayıcısı eşleyin.

18. AWS hizmeti hesabı kimlik bilgileri, Azure AD Kullanıcı hazırlama AWS hesabından rolleri getiriliyor için kullanın. Bunun için giriş AWS konsolu açın.

19. Tıklayın **Hizmetleri** -> **güvenlik, Identity & Uyumluluk** -> **IAM**.

    ![AWS hesabından rolleri getiriliyor](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole1.png)

20. Seçin **ilkeleri** IAM bölümündeki sekmesinde.

    ![AWS hesabından rolleri getiriliyor](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole2.png)

21. Tıklayarak yeni bir ilke oluşturmak **ilkesi oluşturma** için Azure AD Kullanıcı hazırlama AWS hesabından rolleri getiriliyor.

    ![Yeni ilke oluşturma](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole3.png)

22. Aşağıdaki adımları gerçekleştirerek tüm rolleri AWS hesaplarından getirmek için kendi ilke oluştur:

    ![Yeni ilke oluşturma](./media/active-directory-saas-amazon-web-service-tutorial/policy1.png)

    a. İçinde **"ilkesi oluşturma"** bölümüne tıklatın **"JSON"** sekmesi.

    b. İlke belgede ekleme JSON aşağıda.
    
    ```
    
    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",
        
    "Action": [
        
    "iam:ListRoles"
        
    ],

    "Resource": "*"

    }

    ]

    }
    
    ```

    c. Tıklayın **gözden geçirme İlkesi düğmesi** İlkesi doğrulanacak.

    ![Yeni ilke tanımlama](./media/active-directory-saas-amazon-web-service-tutorial/policy5.png)

23. Tanımlamak **yeni ilke** aşağıdaki adımları gerçekleştirerek:

    ![Yeni ilke tanımlama](./media/active-directory-saas-amazon-web-service-tutorial/policy2.png)

    a. Sağlamak **ilke adı** olarak **AzureAD_SSOUserRole_Policy**.

    b. Size sağlayabilir **açıklama** İlkesi **AWS hesaplarından rolleri getirmek için bu ilkeyi sağlayacak**.
    
    c. Tıklayın **"İlke oluştur"** düğmesi.

24. Üzerinde **gözden geçirme** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:   
    
    ![Çoklu oturum açma gözden geçirme yapılandırın][34] 

    a. Tıklatın **rolü oluşturma**.

    b. Gerektiğinde kadar rolleri oluşturun ve bunları kimlik sağlayıcısı eşleyin.


25. Yeni bir kullanıcı hesabı, aşağıdaki adımları gerçekleştirerek AWS IAM hizmetinde oluşturun:

    a. Tıklayın **kullanıcılar** AWS IAM konsolundaki gezinti.

    ![Yeni ilke tanımlama](./media/active-directory-saas-amazon-web-service-tutorial/policy3.png)
    
    b. Tıklayın **Kullanıcı Ekle** yeni bir kullanıcı oluşturmak için düğmeye.

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/policy4.png)

    c. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/adduser1.png)
    
    * Kullanıcı adı olarak girin **AzureADRoleManager**.
    
    * Erişim türünü seçin **programlı erişim** seçeneği. Bu şekilde kullanıcı API'leri çağırmak ve rolleri AWS hesabından getirilemedi.
    
    * Tıklayın **sonraki izinleri** sağ alt köşesindeki düğmesini.

26. Şimdi aşağıdaki adımları gerçekleştirerek bu kullanıcı için yeni bir ilke oluşturun:

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/adduser2.png)
    
    a. Tıklayın **mevcut ilkeleri doğrudan ekleme** düğmesi.

    b. Yeni oluşturulan ilkeyi filtre bölümündeki arama **AzureAD_SSOUserRole_Policy**.
    
    c. Seçin **İlkesi** tıklayın **sonraki: gözden geçirme** düğmesi.

27. İlke bağlı kullanıcı için aşağıdaki adımları gerçekleştirerek gözden geçirin:

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/adduser3.png)
    
    a. Kullanıcı adı, erişim türünü ve kullanıcıya eşlenen İlkesi gözden geçirin.
    
    b. Tıklayın **kullanıcı oluşturma** kullanıcı oluşturmak için sağ alt köşedeki düğmesini.

28. Bir kullanıcının kullanıcı kimlik bilgileri, aşağıdaki adımları uygulayarak yükleyin:

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/adduser4.png)
    
    a. Kullanıcı kopyalama **erişim anahtar kimliği** ve **gizli erişim anahtarı**.
    
    b. AWS Konsolu rolleri getirmek için Azure AD kullanıcı bölümü hazırlama içine bu kimlik bilgilerini girin.
    
    c. Tıklayın **Kapat** altındaki düğmesini.

29. Gidin **kullanıcı sağlamayı** Azure AD yönetim portalında Amazon Web Services uygulama bölümü.

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/provisioning.png)

30. Girin **erişim tuşu** ve **gizli** içinde **gizli** ve **gizli belirteci** sırasıyla alan.

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/provisioning1.png)
    
    a. AWS kullanıcı erişim anahtarını girin **clientsecret** alan.
    
    b. AWS kullanıcı gizliliği girin **gizli belirteci** alan.
    
    c. Tıklayın **Bağlantıyı Sına** düğmesini başarıyla bu bağlantıyı sınamak için gerekir.

    d. Tıklayarak ayarı kaydedin **kaydetmek** üstündeki düğmesi.
 
31. Şimdi sağlama durumu etkinleştirdiğinizden emin olun **üzerinde** üzerinde geçiş yapma ve üzerinde tıklatarak ayarları bölümünde **kaydetmek** üstündeki düğmesi.

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Amazon Web Hizmetleri (AWS) test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon Amazon Web Hizmetleri (AWS) adlı bir kullanıcı oluşturmaktır. Amazon Web Hizmetleri (AWS), burada her türlü eylemi gerçekleştirmek gerek kalmaması bunların sistemde SSO için oluşturulan kullanıcıya gerekmez.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Amazon Web Hizmetleri (AWS) erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Amazon Web Hizmetleri (AWS) Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Amazon Web Hizmetleri (AWS)**.

    ![Uygulamalar listesinde Amazon Web Hizmetleri (AWS) bağlantısı](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Amazon Web Hizmetleri (AWS) parçasında tıklattığınızda, otomatik olarak Amazon Web Hizmetleri (AWS) uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png

