---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Amazon Web Services (AWS) | Microsoft Docs'
description: Amazon Web Services (AWS) ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 60d2f8109fbd5f11042d915dc7f43f3c9dd602d5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048905"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Öğretici: Azure Active Directory Tümleştirme ile Amazon Web Services (AWS)

Bu öğreticide, Amazon Web Services (AWS) Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Amazon Web Services (AWS) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Amazon Web Services (AWS) erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Amazon Web Services (AWS için) (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Amazon Web Services (AWS) ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Amazon Web Services (AWS) çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Amazon Web Services (AWS) galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Services (AWS) galeri ekleme
Azure AD'de Amazon Web Services (AWS) tümleştirmesini yapılandırmak için yönetilen SaaS uygulamalar listesine Galeriden Amazon Web Services (AWS) eklemeniz gerekir.

**Galeriden Amazon Web Services (AWS) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Amazon Web Services (AWS)** seçin **Amazon Web Services (AWS)** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Amazon Web Services ("Britta Simon" adlı bir test kullanıcı tabanlı AWS) test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Amazon Web Services (AWS) için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Amazon Web Services (AWS) arasında bir bağlantı ilişki kurulması gerekir.

Amazon Web Services (AWS) değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Amazon Web Services (AWS) ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Amazon Web Services (AWS) test kullanıcısı oluşturma](#create-an-amazon-web-services-aws-test-user)**  - Amazon Web Services (AWS) kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Amazon Web Services (AWS) uygulamanızda çoklu oturum açmayı yapılandırın.

**Amazon Web Services (AWS) ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Amazon Web Services (AWS)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. Üzerinde **Amazon Web Services (AWS) etki alanı ve URL'ler** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiştir gibi tüm adımları gerçekleştirin.

    ![Amazon Web Services (AWS) etki alanı ve URL'ler tek oturum açma bilgileri](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. Amazon Web Services (AWS) yazılım uygulaması belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.

    ![Çoklu oturum açma attb yapılandırın](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png) 

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı  | Öznitelik Değeri | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://aws.amazon.com/SAML/Attributes |
    | Rol            | User.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >AWS konsolunda tüm rollerini getirmek için Azure AD'de kullanıcı sağlamayı yapılandırma gerekir. Sağlama adımları bakın.

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırma Ekle](./media/amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma addattb yapılandırın](./media/amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. İçinde **Namespace** metin kutusuna, bu satır için gösterilen ad alanı değeri yazın.
    
    d. **Tamam**’a tıklayın.

6. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/amazon-web-service-tutorial/tutorial_general_400.png)

8. Farklı bir tarayıcı penceresinde, Amazon Web Services (AWS) şirketinizin sitesi için yönetici olarak oturum.

9. Tıklayın **AWS giriş**.
   
    ![Giriş, çoklu oturum açmayı yapılandırın][11]

10. Tıklayın **kimlik ve erişim yönetimi**. 
   
    ![Çoklu oturum açma kimliği yapılandırın][12]

11. Tıklayın **kimlik sağlayıcıları**ve ardından **sağlayıcısı oluşturma**. 
   
    ![Çoklu oturum açma sağlayıcısı yapılandırma][13]

12. Üzerinde **yapılandırma sağlayıcısı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin: 
   
    ![Çoklu oturum açma iletişim yapılandırın][14]
 
    a. Olarak **sağlayıcı türü**seçin **SAML**.

    b. İçinde **sağlayıcı adı** metin kutusuna sağlayıcı adını yazın (örneğin: *WAAD*).

    c. İndirilen yüklenecek **meta veri dosyası** Azure portalından tıklayın **Dosya Seç**.

    d. Tıklayın **sonraki adım**.

13. Üzerinde **doğrulama sağlayıcı bilgileri** iletişim sayfa tıklayın **Oluştur**. 
    
    ![Çoklu oturum açmayı yapılandırma doğrulayın][15]

14. Tıklayın **rolleri**ve ardından **rol oluşturma**. 
    
    ![Çoklu oturum açma rollerini yapılandırma][16]

15. Üzerinde **rol oluşturma** sayfasında, aşağıdaki adımları gerçekleştirin:  
    
    ![Çoklu oturum açma güven yapılandırın][19] 

    a. Seçin **SAML 2.0 Federasyon** altında **güvenilir varlık türü seçin**.

    b. Altında **SAML 2.0 sağlayıcı bölüm seçin**seçin **SAML sağlayıcısı** önceden oluşturulmuş (örneğin: *WAAD*)

    c. Seçin **izin programlı ve AWS Yönetim Konsolu erişim**.
  
    d. Tıklayın **sonraki: izinleri**.

16. Üzerinde **eklemek izinleri ilkeleri** iletişim kutusunda, herhangi bir ilke eklemek gerekmez. Tıklayın **sonraki: gözden geçirme**.  
    
    ![Çoklu oturum açma ilkesi yapılandırma][33]

17. Üzerinde **gözden geçirme** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:   
    
    ![Çoklu oturum açma gözden geçirme yapılandırın][34] 

    a. İçinde **rol adı** metin rol adınızı girin.

    b. İçinde **rol açıklamasını** metin kutusuna bir açıklama girin.

    c. Tıklayın **rolü oluşturma**.

    d. Gerektiğinde kadar rolleri oluşturun ve bunları kimlik sağlayıcısına eşleyin.

18. AWS hizmet hesabı kimlik bilgileri, Azure AD kullanıcı sağlama AWS hesabı rollerini getirmek için kullanın. Bunun için ev AWS konsolu açın.

19. Tıklayarak **Hizmetleri** -> **güvenlik, kimlik ve Uyumluluk** -> **IAM**.

    ![AWS hesabı rolleri getirilirken](./media/amazon-web-service-tutorial/fetchingrole1.png)

20. Seçin **ilkeleri** IAM bölümünde sekmesi.

    ![AWS hesabı rolleri getirilirken](./media/amazon-web-service-tutorial/fetchingrole2.png)

21. Üzerine tıklayarak yeni bir ilke oluşturun **ilkesi oluşturma** AWS hesabı Azure AD kullanıcı sağlama rolleri getirilirken için.

    ![Yeni ilke oluşturma](./media/amazon-web-service-tutorial/fetchingrole3.png)

22. AWS hesapları aşağıdaki adımları gerçekleştirerek tüm rolleri getirilecek kendi ilkenizi oluşturun:

    ![Yeni ilke oluşturma](./media/amazon-web-service-tutorial/policy1.png)

    a. İçinde **"ilkesi oluşturma"** bölümüne tıklayın **"JSON"** sekmesi.

    b. İlke belgeye eklediğiniz JSON aşağıda.
    
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

    c. Tıklayarak **İnceleme İlkesi düğme** ilke doğrulamak için.

    ![Yeni ilkeyi tanımlayın](./media/amazon-web-service-tutorial/policy5.png)

23. Tanımlama **yeni ilke** aşağıdaki adımları gerçekleştirerek:

    ![Yeni ilkeyi tanımlayın](./media/amazon-web-service-tutorial/policy2.png)

    a. Sağlamak **ilke adı** olarak **AzureAD_SSOUserRole_Policy**.

    b. Sağlayabilirsiniz **açıklama** İlkesi **AWS hesapları rollerini getirmek için bu ilke sağlayacak**.
    
    c. Tıklayarak **"İlke oluştur"** düğmesi.

24. Yeni bir kullanıcı hesabı, aşağıdaki adımları uygulayarak AWS IAM hizmetinde oluşturun:

    a. Tıklayarak **kullanıcılar** AWS IAM konsolundaki gezinti.

    ![Yeni ilkeyi tanımlayın](./media/amazon-web-service-tutorial/policy3.png)
    
    b. Tıklayarak **Kullanıcı Ekle** yeni kullanıcı oluşturma düğmesi.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/policy4.png)

    c. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser1.png)
    
    * Kullanıcı adı olarak girin **AzureADRoleManager**.
    
    * Erişim türü seçmek **programlı erişim** seçeneği. Böylece kullanıcı, API'leri çağırmak ve rolleri AWS hesaptan alın.
    
    * Tıklayarak **sonraki izinler** alt sağ köşesindeki düğme.

25. Şimdi aşağıdaki adımları uygulayarak bu kullanıcı için yeni bir ilke oluşturun:

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser2.png)
    
    a. Tıklayarak **mevcut ilkeleri doğrudan Ekle** düğmesi.

    b. Filtre bölümünde yeni oluşturulan ilke arama **AzureAD_SSOUserRole_Policy**.
    
    c. Seçin **ilke** ve ardından **sonraki: gözden geçirme** düğmesi.

26. Bağlı kullanıcı ilkeyi, aşağıdaki adımları gerçekleştirerek gözden geçirin:

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser3.png)
    
    a. Kullanıcı adı, erişim türü ve kullanıcıyla eşlenen İlkesi gözden geçirin.
    
    b. Tıklayarak **kullanıcı oluşturma** kullanıcı oluşturmak için alt sağ köşesindeki düğme.

27. Bir kullanıcının kullanıcı kimlik bilgilerini, aşağıdaki adımları uygulayarak yükleyin:

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser4.png)
    
    a. Kullanıcı kopyalama **erişim anahtarı kimliği** ve **gizli erişim anahtarı**.
    
    b. AWS konsolunda rollerini getirmek için Azure AD kullanıcı bölümü hazırlama bu kimlik bilgilerini girin.
    
    c. Tıklayarak **Kapat** altındaki düğmesini.

28. Gidin **kullanıcı sağlamayı** Amazon Web Services uygulamasının Azure AD yönetim portalında bölümü.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/provisioning.png)

29. Girin **erişim anahtarı** ve **gizli** içinde **gizli** ve **gizli belirteç** sırasıyla alan.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/provisioning1.png)
    
    a. AWS kullanıcı erişim anahtarı girin **clientsecret** alan.
    
    b. AWS kullanıcı gizliliği girin **gizli belirteç** alan.
    
    c. Tıklayarak **Bağlantıyı Sına** düğmesini gereken başarıyla bu bağlantıyı test etmek kullanabilirsiniz.

    d. Tıklayarak ayarı kaydedin **Kaydet** üstünde düğme.
 
30. Artık sağlama durumu etkinleştirdiğinizden emin olun **üzerinde** geçiş yaptıktan ve ardından tıklayarak ayarları bölümünde **Kaydet** üstünde düğme.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/provisioning2.png)

> [!Note]
> Üzerinde çoklu oturum açma için bir Azure hesabında birden fazla AWS hesapları tümleştirmek istiyorsanız, lütfen bakın [bu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-aws-multi-accounts-tutorial) makalesi. 


### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/amazon-web-service-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/amazon-web-service-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/amazon-web-service-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/amazon-web-service-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Bir Amazon Web Services (AWS) test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Amazon Web Services'te (AWS) adlı bir kullanıcı oluşturmaktır. Amazon Web Services (AWS), burada herhangi bir eylem gerçekleştirmeniz gerekmez, sistemde SSO için oluşturulan açmasına gerek yoktur.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Amazon Web Services (AWS) için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Amazon Web Services (AWS) Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Amazon Web Services (AWS)**.

    ![Uygulamalar listesini Amazon Web Services (AWS) bağlantıdaki](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Amazon Web Services (AWS) kutucuğa tıkladığınızda, otomatik olarak Amazon Web Services (AWS) uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/amazon-web-service-tutorial/ic795031.png
[12]: ./media/amazon-web-service-tutorial/ic795032.png
[13]: ./media/amazon-web-service-tutorial/ic795033.png
[14]: ./media/amazon-web-service-tutorial/ic795034.png
[15]: ./media/amazon-web-service-tutorial/ic795035.png
[16]: ./media/amazon-web-service-tutorial/ic795022.png
[17]: ./media/amazon-web-service-tutorial/ic795023.png
[18]: ./media/amazon-web-service-tutorial/ic795024.png
[19]: ./media/amazon-web-service-tutorial/ic795025.png
[32]: ./media/amazon-web-service-tutorial/ic7950251.png
[33]: ./media/amazon-web-service-tutorial/ic7950252.png
[35]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/amazon-web-service-tutorial/ic7950253.png
[36]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
