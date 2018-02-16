---
title: "Öğretici: Azure Active Directory Tümleştirme Amazon Web Hizmetleri (AWS) ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Amazon Web Hizmetleri (AWS) arasında yapılandırmayı öğrenin."
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
ms.date: 1/16/2017
ms.author: jeedes
ms.openlocfilehash: 0ff14365323d66a101e5847d7959045c3f20dea2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Öğretici: Azure Active Directory Tümleştirme Amazon Web Hizmetleri (AWS)

Bu öğreticide, Amazon Web Hizmetleri (AWS) Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Amazon Web Hizmetleri (AWS) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Amazon Web Hizmetleri (AWS) erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Amazon Web Hizmetleri (AWS) ile Azure AD hesaplarına oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Amazon Web Hizmetleri (AWS) ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Amazon Web Hizmetleri (AWS) çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında kullanmanızı öneririz yok.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [ücretsiz bir aylık deneme almak](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Amazon Web Hizmetleri (AWS) ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Hizmetleri (AWS) Galerisi'nden ekleme
Azure AD tümleştirmeye Amazon Web Hizmetleri (AWS) yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Amazon Web Hizmetleri (AWS) eklemeniz gerekir.

**Amazon Web Hizmetleri (AWS) Galeriden eklemek için aşağıdaki adımları uygulayın:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti bölmesinde seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Amazon Web Hizmetleri (AWS)**. Seçin **Amazon Web Hizmetleri (AWS)** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Sonuçlar listesinde Amazon Web Hizmetleri (AWS)](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Amazon Web Hizmetleri ("Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı AWS) test etme

Tekli çalışmaya oturum için Azure AD için bir kullanıcı Azure AD içinde karşılık gelen kullanıcı Amazon Web Hizmetleri (AWS) olan bilmek ister. Diğer bir deyişle, Amazon Web Hizmetleri (AWS) bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

Amazon Web Hizmetleri (AWS) bağlantı kurmak için değere vermek **kullanıcıadı** aynı değer olarak **kullanıcı adı** Azure AD'de. 

Yapılandırma ve Azure AD çoklu oturum açma Amazon Web Hizmetleri (AWS) ile test etmek için aşağıdaki yapı taşları tamamlayın:

1. [Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on) bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Amazon Web Hizmetleri (AWS) test kullanıcısı oluşturma](#create-an-amazon-web-services-aws-test-user) karşılık gelen Britta Simon, Amazon Web Hizmetleri (AWS) kullanıcı Azure AD gösterimini bağlı olması.
4. [Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user) Britta Azure AD çoklu oturum açma kullanmak Simon etkinleştirmek için.
5. [Test çoklu oturum açma](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Amazon Web Hizmetleri (AWS) uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Amazon Web Hizmetleri (AWS) ile yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında üzerinde **Amazon Web Hizmetleri (AWS)** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Çoklu oturum açma, etkinleştirmek için **çoklu oturum açma** iletişim kutusunda **modu** aşağı açılan listesinden, **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. İçinde **Amazon Web Hizmetleri (AWS) etki alanı ve URL'leri** bölümü, kullanıcı uygulama zaten Azure ile önceden tümleşik olduğundan tüm adımlar yok.

    ![Amazon Web Hizmetleri (AWS) etki alanı ve oturum açma URL'leri tek bilgi](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. Amazon Web Hizmetleri (AWS) yazılım uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** bölümü uygulama tümleştirmesi sayfasında. Aşağıdaki ekran görüntüsünde bir örnek gösterilmektedir:

    ![Çoklu oturum açma özniteliği yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png)  

5. İçinde **kullanıcı öznitelikleri** bölümüne **çoklu oturum açma** kutusuna SAML belirteci özniteliği önceki görüntüde gösterildiği gibi yapılandırın ve ardından aşağıdaki adımları uygulayın:
    
    | Öznitelik adı  | Öznitelik değeri | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rol            | User.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Amazon Web Hizmetleri (AWS) konsolundan tüm rolleri getirmek için Azure AD'de kullanıcı sağlamayı yapılandırın. Aşağıdaki sağlama adımlarına bakın.

    a. Açmak için **özniteliği eklemek** iletişim kutusunda **Ekle özniteliği**.

    ![Çoklu oturum açma özniteliği yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma özniteliği yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. İçinde **Namespace** ilgili satır için gösterilen ad alanı değeri yazın.
    
    d. Seçin **Tamam**.

6. İçinde **SAML imzalama sertifikası** bölümünde, select **meta veri XML**. Ardından meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. **Kaydet**’i seçin.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. Farklı bir tarayıcı penceresinde Amazon Web Hizmetleri (AWS) şirket sitenize yönetici olarak oturum açın.

9. Seçin **konsol giriş**.
   
    ![Oturum açma tek giriş yapılandırın][11]

10. Seçin **kimlik ve erişim yönetimi**. 
   
    ![Çoklu oturum açma kimliğini yapılandırma][12]

11. Seçin **kimlik sağlayıcıları**. Ardından **oluşturma sağlayıcısı**. 
   
    ![Çoklu oturum açma sağlayıcısı yapılandırma][13]

12. İçinde **yapılandırma sağlayıcısı** iletişim kutusunda, aşağıdaki adımları uygulayın: 
   
    ![Yapılandır tek oturum açma iletişim kutusu][14]
 
    a. İçin **sağlayıcı türü**seçin **SAML**.

    b. İçinde **sağlayıcı adı** bir sağlayıcı adı yazın (örneğin: *WAAD*).

    c. İndirilen, karşıya yüklemek için **meta veri dosyası** Azure portalından seçin **Dosya Seç**.

    d. Seçin **sonraki adım**.

13. İçinde **doğrulama sağlayıcı bilgilerini** iletişim kutusunda **oluşturma**. 
    
    ![Çoklu oturum açma doğrulama yapılandırın][15]

14. Seçin **rolleri**. Ardından **yeni rol oluşturma**. 
    
    ![Çoklu oturum açma rollerini yapılandırma][16]

15. İçinde **ayarlamak rol adı** iletişim kutusunda, aşağıdaki adımları uygulayın: 
    
    ![Çoklu oturum açma adı yapılandırma][17] 

    a. İçinde **rol adı** bir rol adı yazın (örneğin, *TestUser*). 

    b. Seçin **sonraki adım**.

16. İçinde **rol türünü seç** iletişim kutusunda, aşağıdaki adımları uygulayın: 
    
    ![Oturum açma tek rol türü yapılandırma][18] 

    a. Seçin **kimlik sağlayıcısı erişim için rol**. 

    b. İçinde **Grant Web çoklu oturum açma (WebSSO) erişim SAML sağlayıcılarına** 'yi tıklatın **seçin**.

17. İçinde **kurmak güven** iletişim kutusunda, aşağıdaki adımları uygulayın:  
    
    ![Çoklu oturum açma güvenini yapılandırma][19] 

    a. Daha önce oluşturduğunuz SAML sağlayıcısı seçin (örneğin: *WAAD*). 
  
    b. Seçin **sonraki adım**.

18. İçinde **doğrulayın rol güven** iletişim kutusunda **sonraki adım**. 
    
    ![Oturum açma tek rol güvenini yapılandırma][32]

19. İçinde **ekleme İlkesi** iletişim kutusunda **sonraki adım**.  
    
    ![Çoklu oturum açma ilkesini yapılandırma][33]

20. İçinde **gözden geçirme** iletişim kutusunda, aşağıdaki adımları uygulayın:   
    
    ![Çoklu oturum açma gözden geçirme yapılandırın][34] 

    a. Seçin **rolü oluşturma**.

    b. Gerektiği gibi birçok rol oluşturmak ve bunları kimlik sağlayıcısı eşleyin.

21. Amazon Web Hizmetleri (AWS) hizmet hesabı kimlik bilgileri, Azure AD Kullanıcı hazırlama, Amazon Web Hizmetleri (AWS) hesabından rolleri getiriliyor için kullanın. Bu görevi başlatmak için giriş Amazon Web Hizmetleri (AWS) konsolunu açın.

22. Seçin **Hizmetleri** > **güvenlik, Identity & Uyumluluk** > **IAM**.

    ![Amazon Web Hizmetleri (AWS) hesabından rolleri getiriliyor](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole1.png)

23. IAM bölümünde **ilkeleri** sekmesi.

    ![Amazon Web Hizmetleri (AWS) hesabından rolleri getiriliyor](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole2.png)

24. Yeni bir ilke oluşturmak için seçin **ilkesi oluşturma**.

    ![Yeni bir ilke oluşturma](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole3.png)
 
25. Amazon Web Hizmetleri (AWS) hesaplarından tüm rolleri getirmek üzere kendi ilkesi oluşturmak için aşağıdaki adımları uygulayın:

    ![Yeni bir ilke oluşturma](./media/active-directory-saas-amazon-web-service-tutorial/policy1.png)

    a. İçinde **ilkesi oluşturma** bölümünde, select **JSON** sekmesi.

    b. İlke belgede aşağıdaki JSON ekleyin:
    
    ```
    
    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",
        
    "Action": [
        
    "iam: ListRoles"
        
    ],

    "Resource": "*"

    }

    ]

    }
    
    ```

    c. İlke doğrulanacak seçin **gözden geçirme İlkesi düğmesi**.

    ![Yeni ilke tanımlama](./media/active-directory-saas-amazon-web-service-tutorial/policy5.png)

26. Tanımlamak **yeni ilke** aşağıdaki adımları alarak:

    ![Yeni ilke tanımlama](./media/active-directory-saas-amazon-web-service-tutorial/policy2.png)

    a. Sağlamak **ilke adı** olarak **AzureAD_SSOUserRole_Policy**.

    b. Aşağıdaki sağlayabilir **açıklama** ilkesi için: **Bu ilkeyi rolleri AWS hesaplarından fetch sağlayacak**.
    
    c. Seçin **ilke Oluştur** düğmesi.
        
27. Amazon Web Hizmetleri (AWS) IAM hizmetinde yeni bir kullanıcı hesabı oluşturmak için aşağıdaki adımları uygulayın:

    a. Seçin **kullanıcılar** Amazon Web Hizmetleri (AWS) IAM konsolunda.

    ![Yeni ilke tanımlama](./media/active-directory-saas-amazon-web-service-tutorial/policy3.png)
    
    b.To, select yeni bir kullanıcı oluşturma **Kullanıcı Ekle** düğmesi.

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/policy4.png)

    c. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları uygulayın:
    
    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/adduser1.png)
    
    * Girin **AzureADRoleManager** kullanıcı adı kutusuna.
    
    * Erişim türü için **programlı erişim** seçeneği. Bu şekilde kullanıcı API'leri çağırmak ve rolleri Amazon Web Hizmetleri (AWS) hesabından getirilemedi.
    
    * Seçin **sonraki izinleri** sağ alt köşesindeki düğmesini.

28. Bu kullanıcı için yeni bir ilke, aşağıdaki adımları uygulayarak oluşturun:

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/adduser2.png)
    
    a. Seçin **mevcut ilkeleri doğrudan ekleme** düğmesi.

    b. Yeni oluşturulan ilkeyi filtre bölümündeki arama **AzureAD_SSOUserRole_Policy**.
    
    c. Seçin **İlkesi**. Ardından **sonraki: gözden geçirme** düğmesi.

29. İlke bağlı kullanıcı için aşağıdaki adımları uygulayarak gözden geçirin:

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/adduser3.png)
    
    a. Kullanıcı adı, erişim türünü ve kullanıcıya eşlenen İlkesi gözden geçirin.
    
    b. Kullanıcı oluşturmak için seçin **kullanıcı oluşturma** kullanıcı oluşturmak için sağ alt köşesindeki düğmesini.

30. Bir kullanıcının kimlik bilgileri, aşağıdaki adımları uygulayarak yükleyin:

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/adduser4.png)
    
    a. Kullanıcı kopyalama **erişim anahtar kimliği** ve **gizli erişim anahtarı**.
    
    b. Amazon Web Hizmetleri (AWS) konsolundan rolleri getirmek için bölüm sağlama Azure AD kullanıcısının içine bu kimlik bilgilerini girin.
    
    c. Seçin **Kapat** sağ alt köşesindeki düğmesini.

31. Gidin **kullanıcı sağlamayı** Azure AD yönetim portalında Amazon Web Hizmetleri (AWS) uygulama bölümünde.

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/provisioning.png)

32. Girin **erişim tuşu** ve **gizli** içinde **gizli** ve **gizli belirteci** sırasıyla alan.

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/provisioning1.png)
    
    a. Amazon Web Hizmetleri (AWS) kullanıcının erişim anahtarını girin **clientsecret** alan.
    
    b. Amazon Web Hizmetleri (AWS) kullanıcı gizliliği girin **gizli belirteci** alan.
    
    c. Seçin **Bağlantıyı Sına** düğmesi. Başarılı bir şekilde bu bağlantıyı sınamak için gerekir.

    d. Ayarı seçerek kaydetmek **kaydetmek** üstündeki düğmesi.
 
33. Kapattığınızdan emin olun sağlama durumu **üzerinde** içinde **ayarları**. Seçerek bunu **üzerinde**ve ardından seçerek **kaydetmek** üstündeki düğmesi.

    ![Kullanıcı ekle](./media/active-directory-saas-amazon-web-service-tutorial/provisioning2.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com) uygulaması kuruluyor sırada. Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesi. Katıştırılmış belgeleri aracılığıyla erişim **yapılandırma** alt bölüm. Daha fazla bilgiyi embedded belgeler özelliği hakkında [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**. Ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Amazon Web Hizmetleri (AWS) test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon Amazon Web Hizmetleri (AWS) adlı bir kullanıcı oluşturmaktır. Amazon Web Hizmetleri (AWS) bir kullanıcı için kendi sistem oluşturulması gereken değil tek oturum açma, burada herhangi bir eylem gerçekleştirmeniz gerekmez.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Amazon Web Hizmetleri (AWS) için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Amazon Web Hizmetleri (AWS) Britta Simon atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulamaları görünümünü açın. Daha sonra dizin görünümüne gidin ve seçin **kurumsal uygulamalar**. Ardından, **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Amazon Web Hizmetleri (Amazon Web Hizmetleri (AWS)**.

    ![Uygulamalar listesinde Amazon Web Hizmetleri (AWS) bağlantısı](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** düğmesi. Ardından **eklemek atama** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklatın **seçin** düğmesi. 

7. İçinde **eklemek atama** iletişim kutusunda **atamak** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim panelinde Amazon Web Hizmetleri (AWS) döşeme seçtiğinizde, otomatik olarak Amazon Web Hizmetleri (AWS) uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

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

