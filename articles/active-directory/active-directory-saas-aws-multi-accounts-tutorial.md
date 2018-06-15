---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Amazon Web Hizmetleri (birden çok hesabı bağlamak için AWS) | Microsoft Docs'
description: Çoklu oturum açma Azure AD arasında yapılandırmayı öğrenin ve birden fazla hesap Amazon Web Hizmetleri (AWS).
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
ms.openlocfilehash: a6a7861b95f82180b72045f10db99d5bc45eed73
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34345665"
---
# <a name="tutorial-azure-active-directory-integration-with-multiple-amazon-web-services-aws-accounts"></a>Öğretici: Azure Active Directory Tümleştirme ile birden çok Amazon Web Hizmetleri (AWS) hesabı

Bu öğreticide, Azure Active Directory (Azure AD) birden fazla hesap Amazon Web Hizmetleri (AWS) ile tümleştirme öğrenin.

Amazon Web Hizmetleri (AWS) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Amazon Web Hizmetleri (AWS) erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Amazon Web Hizmetleri (AWS için) (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Amazon Web Hizmetleri (AWS) ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD temel veya premium aboneliği
- Amazon Web Hizmetleri (AWS) birden çok çoklu oturum açma hesaplarını etkin

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

4. Arama kutusuna **Amazon Web Hizmetleri (AWS)** seçin **Amazon Web Hizmetleri (AWS)** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Amazon Web Hizmetleri (AWS)](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

5. Uygulama eklendikten sonra Git **özellikleri** sayfası ve kopyalama **nesne kimliği**.

    ![Sonuçlar listesinde Amazon Web Hizmetleri (AWS)](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_properties.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Amazon Web Hizmetleri ("Britta Simon" adlı bir test kullanıcı tabanlı AWS) test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Amazon Web Hizmetleri (AWS) bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Amazon Web Hizmetleri (AWS) arasında bir bağlantı ilişkisi kurulması gerekir.

Amazon Web Hizmetleri (AWS) değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Amazon Web Hizmetleri (AWS) ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Amazon Web Hizmetleri (AWS) uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Amazon Web Hizmetleri (AWS) ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Amazon Web Hizmetleri (AWS)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. Üzerinde **Amazon Web Hizmetleri (AWS) etki alanı ve URL'leri** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiş gibi tüm adımları gerçekleştirin.

    ![Amazon Web Hizmetleri (AWS) etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. Amazon Web Hizmetleri (AWS) yazılım uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.

    ![Çoklu oturum açma özniteliği yapılandırın](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_attribute.png)  

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı  | Öznitelik Değeri | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://aws.amazon.com/SAML/Attributes |
    | Rol            | User.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >AWS konsolundan tüm rolleri getirmek için Azure AD'de kullanıcı sağlamayı yapılandırmanız gerekir. Hazırlama adımları bakın.

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma özniteliği yapılandırın](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. İçinde **Namespace** metin kutusuna, ilgili satır için gösterilen ad alanı değeri yazın.
    
    d. **Tamam**’a tıklayın.

6. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_400.png)

8. Farklı bir tarayıcı penceresinde Amazon Web Hizmetleri (AWS) şirket sitenize yönetici olarak oturum.

9. Tıklatın **AWS giriş**.
   
    ![Çoklu oturum açma giriş yapılandırın][11]

10. Tıklatın **IAM** (kimlik ve erişim yönetimi). 
   
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

18. Geçerli AWS hesabı ve Azure AD ile çoklu oturum açmayı yapılandırın istediğiniz başka bir hesap ile oturum açma oturumu kapatın.

19. Bu hesap için Kurulum istediğiniz birden çok rol oluşturmak için adım-17. adım-9 gerçekleştirin. Lütfen ikiden fazla hesabınız varsa, bunlar için rolleri oluşturmak tüm hesapları için aynı adımları gerçekleştirin.

20. Tüm rolleri hesaplarında oluşturulduktan sonra bunların görünmesini **rolleri** bu hesaplar için liste.

    ![Rol Kurulumu](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_listofroles.png)

21. Tüm rol daha ve güvenilir varlıklar tüm roller için el ile Azure AD uygulaması ile eşlemek için ihtiyacımız tüm hesapları arasında yakalamak gerekir. 

22. Tıklatın kopyalamak için roller üzerinde **rol daha** ve **güvenilir varlıklar** değerleri. Bu değerler, Azure AD'de oluşturmak için gereken tüm rolleri için gerekir.

    ![Rol Kurulumu](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_role_summary.png)
 
23. Tüm hesapları tüm roller için yukarıdaki adımı gerçekleştirmek ve bunların tümünün biçiminde depolayan **rol daha güvenilir varlıklar** bir Not Defteri'nde. 

24. Açık [Azure AD Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede.

    a. Graph Explorer'a sitenin kiracınız için genel yönetici/ortak yönetici kimlik bilgilerini kullanarak oturum açın.

    b. Rolleri oluşturmak için yeterli izinleri olması gerekir. Tıklayın **değiştirme izinleri** gerekli izinleri almak için. 

    ![Grafik explorer iletişim kutusu](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Aşağıdaki izinleri (, bunlar zaten yoksa) listesinden seçin ve "İzinleri değiştir"'i tıklatın 

    ![Grafik explorer iletişim kutusu](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Bu, oturum açmayı yeniden ve onay kabul etmek ister. Onay kabul ettikten sonra Graph Explorer'a yeniden kaydedilir.

    e. Sürüm açılır değiştirme **beta**. Tüm hizmet asıl adı Kiracı getirmek için aşağıdaki sorguyu kullanın:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Birden çok dizin kullanıyorsanız, daha sonra birincil etki alanınızda varsa desen aşağıdaki kullanabilirsiniz `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Grafik explorer iletişim kutusu](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new1.png)
    
    f. Alınan hizmet asıl adı listesinden değiştirmek için gereken bir alın. Ctrl + F, tüm listelenen ServicePrincipals uygulamadan aramak için de kullanabilirsiniz. Sorguyu kullanarak kullanabileceğiniz **nesne kimliği** ilgili hizmet sorumlusu almak için Azure AD özellikler sayfasından kopyalanan.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Grafik explorer iletişim kutusu](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. AppRoles özelliği hizmet asıl nesneden ayıklayın. 

    ![Grafik explorer iletişim kutusu](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new3.png)

    h. Artık uygulamanız için yeni rolleri oluşturmak gerekir. 

    i. JSON appRoles nesnesinin bir örnektir. Uygulamanız için istediğiniz rolleri eklemek için benzer bir nesnesi oluşturun. 

    ```
    {
    "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "4aacf5a4-f38b-4861-b909-bae023e88dde",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Admin,arn:aws:iam::12345:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Auditors,WAAD",
            "displayName": "Auditors,WAAD",
            "id": "bcad6926-67ec-445a-80f8-578032504c09",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Auditors,arn:aws:iam::12345:saml-provider/WAAD"
        }    ]
    }
    ```

    > [!Note]
    > Sonra yeni rolleri yalnızca ekleyebilirsiniz **msiam_access** düzeltme eki işlemi için. Ayrıca, kuruluş gereksiniminizi istediğiniz sayıda rolleri ekleyebilirsiniz. Azure AD gönderecek **değeri** SAML yanıt talep değeri olarak bu rollerin.
    
    j. Graph Explorer'a geri dönün ve yönteminden değiştirin **almak** için **düzeltme eki**. Rolleri appRoles özelliği yukarıdaki örnekte gösterilene benzer güncelleştirerek istenen için hizmet sorumlusu nesnesi düzeltme eki. Tıklatın **sorgu çalıştırma** düzeltme eki işlemi yürütmek için. Bir başarı iletisi Amazon Web Hizmetleri uygulamanız için rolünün oluşturulmasını onaylar.

    ![Grafik explorer iletişim kutusu](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new11.png)

25. Hizmet sorumlusu daha fazla rolleriyle uygulandıktan sonra ilgili rollere kullanıcıları/grupları atayabilirsiniz. Bu Portalı'na gidip Amazon Web Hizmetleri uygulamasına gidip yapılabilir. Tıklayın **kullanıcılar ve gruplar** üst sekmesinde. 

26. Her AWS rolü için yeni gruplar oluşturabilir ve böylece bu grubun belirli rol atayabilirsiniz öneririz. Bu bire bir eşleme için bir rol için bir grup olduğunu unutmayın. Daha sonra bu gruba ait üyeler ekleyebilirsiniz.

27. Grupları oluşturulduktan sonra grubu seçin ve uygulamaya atayın. 

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new5.png)

28. Gruba rol atamak için rolü seçin ve tıklayın **atamak** sayfanın düğmesini.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new6.png)

> [!Note]
> Yeni rolleri görmek için Azure portalında oturumunuzu yenilemek gerektiğini unutmayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Amazon Web Hizmetleri (AWS) parçasında tıkladığınızda, Amazon Web Hizmetleri (AWS) uygulama sayfası rolü seçin seçeneğiyle almanız gerekir.

![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_screen.png)

Ayrıca, talepler olarak geçirilen rolleri görmek için SAML yanıtını doğrulayabilir.

![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_saml.png)

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic7950253.png
[36]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_on.png

