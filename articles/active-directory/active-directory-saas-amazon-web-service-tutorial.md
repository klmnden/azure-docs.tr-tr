---
title: "Öğretici: Azure Active Directory Tümleştirme Amazon Web Hizmetleri (AWS) ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Amazon Web Hizmetleri (AWS) arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 1678d44fc7769c2015c3779ce713870af7a40de9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Öğretici: Azure Active Directory Tümleştirme Amazon Web Hizmetleri (AWS)

Bu öğreticide, Amazon Web Hizmetleri (AWS) Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Amazon Web Hizmetleri (AWS) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Amazon Web Hizmetleri (AWS) erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Amazon Web Hizmetleri (AWS için) (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with Amazon Web Services (AWS), it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Ön koşullar

Amazon Web Hizmetleri (AWS) ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Amazon Web Hizmetleri (AWS) çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Amazon Web Hizmetleri (AWS) ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Galeriden Amazon Web Hizmetleri (AWS) ekleme
Azure AD tümleştirmeye Amazon Web Hizmetleri (AWS) yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Amazon Web Hizmetleri (AWS) eklemeniz gerekir.

**Amazon Web Hizmetleri (AWS) Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Amazon Web Hizmetleri (AWS)**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. Sonuçlar panelinde seçin **Amazon Web Hizmetleri (AWS)**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Amazon Web Hizmetleri ("Britta Simon" adlı bir test kullanıcı tabanlı AWS) test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Amazon Web Hizmetleri (AWS) bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Amazon Web Hizmetleri (AWS) arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Amazon Web Hizmetleri (AWS).

Yapılandırma ve Azure AD çoklu oturum açma Amazon Web Hizmetleri (AWS) ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Amazon Web Hizmetleri test kullanıcısı oluşturma](#creating-an-amazon-web-services-test-user)**  - karşılık gelen Britta Simon, Amazon Web Hizmetleri (AWS) Azure AD gösterimini her için bağlantılı olması.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Amazon Web Hizmetleri (AWS) uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Amazon Web Hizmetleri (AWS) ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Portalı'nda üzerinde **Amazon Web Hizmetleri (AWS)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. Üzerinde **Amazon Web Hizmetleri (AWS) etki alanı ve URL'leri** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiş gibi tüm adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. Amazon Web Hizmetleri (AWS) yazılım uygulaması SAML onaylar belirli bir biçimde bekliyor. Lütfen bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı  | Öznitelik Değeri | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://aws.Amazon.com/SAML/Attributes |
    | Rol            | User.assignedroles |  https://aws.Amazon.com/SAML/Attributes |
    
    >[!TIP]
    >AWS konsolundan tüm rolleri getirmek için Azure AD'de kullanıcı sağlamayı yapılandırmanız gerekir. Lütfen aşağıdaki sağlama adımları bakın.

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın. Yukarıda verilen Namespace değerin ekleyin.
    
    d. **Tamam**’a tıklayın.

7. Tıklatın **kaydetmek** Azure üzerinde ayarları kaydetmek için düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. Farklı bir tarayıcı penceresinde Amazon Web Hizmetleri (AWS) şirket sitenize yönetici olarak oturum.

9. Tıklatın **konsol giriş**.
   
    ![Çoklu oturum açmayı yapılandırın][11]

10. Tıklatın **IAM** gelen **güvenlik, Identity & Uyumluluk** hizmet.
   
    ![Çoklu oturum açmayı yapılandırın][12]

11. Tıklatın **kimlik sağlayıcıları**ve ardından **oluşturma sağlayıcısı**.
   
    ![Çoklu oturum açmayı yapılandırın][13]

12. Üzerinde **yapılandırma sağlayıcısı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın][14]
 
    a. Olarak **sağlayıcı türü**seçin **SAML**.

    b. İçinde **sağlayıcı adı** metin kutusuna, bir sağlayıcı adı yazın (örneğin: *WAAD*).

    c. İndirilen meta veri dosyanızı karşıya yüklemek için tıklayın **Dosya Seç**.

    d. Tıklatın **sonraki adım**.

13. Üzerinde **sağlayıcısı bilgilerini doğrulayın** iletişim sayfasında, tıklatın **oluşturma**. 
    
    ![Çoklu oturum açmayı yapılandırın][15]

14. Tıklatın **rolleri**ve ardından **yeni rol oluşturma**. 
    
    ![Çoklu oturum açmayı yapılandırın][16]

15. Üzerinde **ayarlamak rol adı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin: 
    
    ![Çoklu oturum açmayı yapılandırın][17] 

    a. İçinde **rol adı** metin kutusuna, bir rol adı yazın (örneğin: *TestUser*). 

    b. Tıklatın **sonraki adım**.

16. Üzerinde **rol türünü seç** iletişim kutusunda, aşağıdaki adımları gerçekleştirin: 
    
    ![Çoklu oturum açmayı yapılandırın][18] 

    a. Seçin **kimlik sağlayıcısı erişim için rol**. 

    b. İçinde **Grant Web çoklu oturum açma (WebSSO) erişim SAML sağlayıcılarına** 'yi tıklatın **seçin**.

17. Üzerinde **kurmak güven** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:  
    
    ![Çoklu oturum açmayı yapılandırın][19] 

    a. SAML sağlayıcısı, daha önce oluşturmuş SAML sağlayıcısını seçin (örneğin: *WAAD*)
  
    b. Tıklatın **sonraki adım**.

18. Üzerinde **doğrulayın rol güven** iletişim kutusunda, tıklatın **sonraki adım**.
    
    ![Çoklu oturum açmayı yapılandırın][32]

19. Üzerinde **ekleme İlkesi** iletişim kutusunda, tıklatın **sonraki adım**.
    
    ![Çoklu oturum açmayı yapılandırın][33]

20. Üzerinde **gözden geçirme** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın][34]
 
    a. Tıklatın **rolü oluşturma**.

    b. Gerektiğinde kadar rolleri oluşturun ve bunları kimlik sağlayıcısı eşleyin.

21. AWS tüm rolleri getirmek için sağlama kullanıcı Şimdi Yapılandır

    a. Kök hesabınızla AWS konsolu açma

    b. Sağ üst köşeden adınıza tıklayın ve ardından **My güvenlik kimlik bilgileri** seçeneği. Bu ekran yukarı bir uyarı iletisi açar. Düğmesini **güvenlik kimlik bilgileri** ekran geçirmek için düğmesi.
        
       ![Çoklu oturum açmayı yapılandırın][36]

       ![Çoklu oturum açmayı yapılandırın][37]

    c. Erişim tuşları bölümünde tıklayın **yeni erişim anahtarı oluştur** düğmesi. Bu, erişim anahtarı kimliği ve bir belirteç değeri oluşturur.
    
       ![Çoklu oturum açmayı yapılandırın][38]

    d. Bu değerleri kopyalayın ve böylece bu kaybetmeyin Ayrıca, indirin.

    e. Amazon Web Hizmetleri (AWS) uygulama tümleştirmesi sayfasında Azure Portalı'nda tıklatın **sağlama**.
        
       ![Çoklu oturum açmayı yapılandırın][35]

    f. Hazırlama modu ayarlamak **otomatik**
        
       ![Çoklu oturum açmayı yapılandırın][39]

    g. Artık **clientsecret** ve **gizli belirteci** AWS konsolundan kopyaladığınız karşılık gelen değerler yapıştırın.
    
    h. Tıklayabilirsiniz **Bağlantıyı Sına** düğmesi bağlantısını test edin. Başarılı olduktan sonra sağlama Bağlayıcısı'nı başlatabilirsiniz.
       
       ![Çoklu oturum açmayı yapılandırın][40]

    ı. Sağlama durumu şimdi etkinleştirmek **üzerinde**. Bu uygulamadan rolleri getiriliyor başlatır.

       ![Çoklu oturum açmayı yapılandırın][41]

    > [!NOTE]
    > Azure AD sağlama hizmeti çalıştığında AWS rollerden eşitlemek için bir süre sonra her. Tüm kimlik sağlayıcısı AWS rolleri Azure AD ile bağlı ve kullanıcılara veya gruplara uygulama atarken kullanabilmek için görmeniz gerekir.

<!--### Next steps

To ensure users can sign-in to Amazon Web Services (AWS) after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Amazon Web Services (AWS) in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-an-amazon-web-services-test-user"></a>Amazon Web Hizmetleri test kullanıcısı oluşturma

Amazon Web Hizmetleri (AWS) oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar Amazon Web Hizmetleri (AWS) içine sağlanmalıdır. Amazon Web Hizmetleri (AWS) söz konusu olduğunda sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Amazon Web Hizmetleri (AWS)** yönetici olarak şirket site.

2. Tıklatın **konsol giriş** simgesi. 
   
    ![Çoklu oturum açmayı yapılandırın][11]

3. Kimlik'i tıklatın ve erişim yönetimi. 
   
    ![Çoklu oturum açmayı yapılandırın][28]

4. Panoda tıklatın **kullanıcılar**ve ardından **Create New Users**. 
   
    ![Çoklu oturum açmayı yapılandırın][29]

5. Kullanıcı oluştur iletişim kutusunda, aşağıdaki adımları gerçekleştirin: 
   
    ![Çoklu oturum açmayı yapılandırın][30]   
    
    a. İçinde **kullanıcı adlarını girin** metin kutuları, Azure AD'de Brita Simon'ın kullanıcı adı (userprincipalname) yazın.

    b. Tıklatın **oluşturun.**
        
### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Amazon Web Hizmetleri (AWS) için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**Amazon Web Hizmetleri (AWS) Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Amazon Web Hizmetleri (AWS)**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Üzerinde **rolü Seç** sekmesinde, kullanıcı için uygun rolü seçin. Bu roller rol adı ve kimlik sağlayıcısı adı ile gösterilir. Bu şekilde, AWS rollerden kolayca tanımlayabilirsiniz.

8. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Amazon Web Hizmetleri (AWS) parçasında tıklattığınızda, otomatik olarak Amazon Web Hizmetleri (AWS) uygulamanıza açan. 

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
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
