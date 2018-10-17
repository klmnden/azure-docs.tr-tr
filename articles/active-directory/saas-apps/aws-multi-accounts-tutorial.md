---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Amazon Web Services (birden çok hesaba bağlanmak için AWS) | Microsoft Docs'
description: Azure AD arasında çoklu oturum açmayı yapılandırma hakkında bilgi edinin ve birden çok hesap Amazon Web Services (AWS).
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
ms.date: 10/15/2018
ms.author: jeedes
ms.openlocfilehash: a9acb9539497c85f408ce7417fa5983072ea80b9
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365671"
---
# <a name="tutorial-azure-active-directory-integration-with-multiple-amazon-web-services-aws-accounts"></a>Öğretici: Azure Active Directory Tümleştirme ile birden çok Amazon Web Services (AWS) hesapları

Bu öğreticide, Azure Active Directory (Azure AD) birden çok hesap Amazon Web Services (AWS) ile tümleştirme konusunda bilgi edinin.

Amazon Web Services (AWS) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Amazon Web Services (AWS) erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Amazon Web Services (AWS için) (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

![Sonuç listesinde Amazon Web Services (AWS)](./media/aws-multi-accounts-tutorial/amazonwebservice.png)

>[!NOTE]
>Lütfen tüm AWS hesapları için bir AWS uygulama bağlama bizim önerdiğimiz yaklaşım olmadığını unutmayın. Bunun yerine kullanılacak öneririz [bu](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial) yaklaşım, Azure AD'de AWS hesabı birden çok örneğe AWS uygulamaların birden fazla örneğini yapılandırmak için.

**Aşağıdaki nedenlerden dolayı bu yaklaşımı kullanmak için önermiyoruz olduğunu lütfen unutmayın:**

* Düzeltme eki uygulama için tüm roller için Graph Gezgini yaklaşımı kullanmak zorunda. Bildirim dosyası yaklaşım kullanımı önerilmemektedir.

* ~ 1200 uygulama rolleri için tek bir AWS uygulama ekledikten sonra uygulama üzerinde herhangi bir işlemi hataları özel durum atma başladığını raporlama müşterileri boyutuna ilgili gördük. Uygulama nesnesinde boyutu için sabit bir sınır yoktur.

* Roller, tüm hesapları Değiştir yaklaşım ve değil ekleme ne yazık ki olduğu eklenin olarak rolü el ile güncelleştirmeniz gerekir. Hesaplarınızı sonra bu büyüyor de olur n x n ilişkisi hesapları ve rolleri.

* AWS hesapları aynı Federasyon meta veri XML dosyasında kullanacağınız ve sertifika geçişi zamanında aynı anda tüm AWS hesapları bir sertifikayı güncelleştirmek için bu büyük alıştırma sürücü gerekir.

## <a name="prerequisites"></a>Önkoşullar

Amazon Web Services (AWS) ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD temel veya premium aboneliği
- Amazon Web Services (AWS) birden çok çoklu oturum açma etkin hesaplar

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

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/aws-multi-accounts-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/aws-multi-accounts-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/aws-multi-accounts-tutorial/a_new_app.png)

4. Arama kutusuna **Amazon Web Services (AWS)** seçin **Amazon Web Services (AWS)** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

5. Uygulama eklendikten sonra Git **özellikleri** sayfası ve kopyalama **nesne kimliği**.

    ![Sonuç listesinde Amazon Web Services (AWS)](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_properties.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Amazon Web Services ("Britta Simon" adlı bir test kullanıcı tabanlı AWS) test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Amazon Web Services (AWS) için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Amazon Web Services (AWS) arasında bir bağlantı ilişki kurulması gerekir.

Amazon Web Services (AWS) değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Amazon Web Services (AWS) ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Amazon Web Services (AWS) uygulamanızda çoklu oturum açmayı yapılandırın.

**Amazon Web Services (AWS) ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **Amazon Web Services (AWS)** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/aws-multi-accounts-tutorial/B1_B2_Select_SSO.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/aws-multi-accounts-tutorial/b1_b2_saml_sso.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/aws-multi-accounts-tutorial/b1-domains_and_urlsedit.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde kullanıcısının clonedatabase'i uygulama zaten Azure ile önceden tümleşik olarak herhangi bir adımı gerçekleştirmek.

    ![image](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_url.png)

5. Amazon Web Services (AWS) uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri ve talepler** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri ve talepler** iletişim.

    ![image](./media/aws-multi-accounts-tutorial/i4-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri ve talepler** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Ad  | Kaynak özniteliği  | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://aws.amazon.com/SAML/Attributes |
    | Rol            | User.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "900 saniyenin (15 dakika) arasında bir değer 43200 saniye (12 saat) sağla" |  https://aws.amazon.com/SAML/Attributes |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/aws-multi-accounts-tutorial/i2-attribute.png)

    ![image](./media/aws-multi-accounts-tutorial/i3-attribute.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Girin **Namespace** değeri.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  ve bilgisayarınıza kaydedin.

    ![image](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

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

18. AWS hesabı ve Azure AD ile çoklu oturum açmayı yapılandırmak için istediğiniz başka bir hesap ile oturum açma geçerli oturumu kapatın.

19. Adım 9, Kurulum bu hesap için istediğiniz birden çok rol oluşturmak için adım-17 için gerçekleştirin. Lütfen ikiden fazla hesabınız varsa, bunlar için rolleri oluşturmak tüm hesaplar için de aynı adımları gerçekleştirin.

20. Tüm rolleri hesap oluşturulduktan sonra bunlar gösterilmesi **rolleri** bu hesaplarının listesi.

    ![Rol Kurulumu](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_listofroles.png)

21. Tüm rol ARN ve güvenilir varlıklar, tüm roller için el ile Azure AD uygulaması ile eşlemek için ihtiyacımız tüm hesaplarınızda yakalamak ihtiyacımız var. 

22. Rolleri kopyalamak için tıklayın **rol ARN** ve **güvenilir varlıklar** değerleri. Azure AD'de oluşturmak için ihtiyacınız olan tüm rolleri için bu değerleri gerekir.

    ![Rol Kurulumu](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_role_summary.png)

23. Tüm hesapları tüm roller için yukarıdaki adımı gerçekleştirmek ve bunların tümünü biçiminde depolamak **rol ARN, güvenilir varlıkların** bir not defteri.

24. Açık [Azure AD Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede.

    a. Graph Gezgini sitenin kiracınız için genel yönetici/ortak yönetici kimlik bilgilerini kullanarak oturum açın.

    b. Rolleri oluşturmak için yeterli izinleri vermeniz gerekir. Tıklayarak **değiştirme izinleri** gerekli izinleri almak için.

    ![Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Aşağıdaki izinleri (bunlar önceden yüklü değilse) listeden seçin ve "İzinleri değiştir" tıklayın 

    ![Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Bu, yeniden oturum açmak ve onayı kabul etmek için sorar. Onayı kabul ettikten sonra Graph Explorer'a yeniden kaydedilir.

    e. Sürüm açılan değiştirme **beta**. Tüm hizmet sorumlularını kiracınızdan getirmek için aşağıdaki sorguyu kullanın:

     `https://graph.microsoft.com/beta/servicePrincipals`

    Birden çok dizini kullanıyorsanız, daha sonra birincil etki alanı içindeki deseni, aşağıdaki kullanabilirsiniz `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)

    f. Hizmet sorumluları getirilen listesinden bir değişiklik yapmanız alın. Ctrl + F, listelenen tüm ServicePrincipals uygulamadan aramak için de kullanabilirsiniz. Aşağıdaki sorguyu kullanarak kullanabileceğiniz **nesne kimliği** ilgili hizmet sorumlusuna almak için Azure AD Özellikleri sayfasından kopyalanan.

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. Hizmet sorumlusu nesnesine appRoles özelliği ayıklayın.

    ![Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

    h. Şimdi uygulamanız için yeni rolleri oluşturmak gerekir. 

    i. JSON appRoles nesne örneğidir. Uygulamanız için istediğiniz rolleri eklemek için benzer bir nesne oluşturun.

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
    > Sonra yeni roller yalnızca ekleyebilirsiniz **msiam_access** düzeltme eki işlemi için. Ayrıca, kuruluş gereksinimlerinize istediğiniz sayıda rolleri ekleyebilirsiniz. Azure AD'ye gönderir **değer** talep değerini SAML yanıt olarak bu rollerden.

    j. Graf Gezgininizde geri dönün ve yöntemden değiştirme **alma** için **düzeltme eki**. Yukarıdaki örnekte gösterilene benzer appRoles özelliği güncelleştirerek rolleri istenen hizmet sorumlusu nesne eki. Tıklayın **Sorgu Çalıştır** düzeltme eki işlemi yürütmek için. Başarılı iletisi, Amazon Web Services uygulamanız için rolünün oluşturulmasını onaylar.

    ![Graph Gezgini iletişim kutusu](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

25. Hizmet sorumlusu ile daha fazla rol düzeltme eki sonra ilgili rollere kullanıcılar/gruplar atayabilirsiniz. Bu portala gidip Amazon Web Services uygulamasına gidip yapılabilir. Tıklayarak **kullanıcılar ve gruplar** üst sekmesinde. 

26. Bu gruba belirli rol atayabilmeniz AWS her rol için yeni Grup oluşturmanızı öneririz. Bu bire bir eşleme bir gruba bir rol için olduğunu unutmayın. Daha sonra bu gruba ait olan üyeleri ekleyebilirsiniz.

27. Grup oluşturulduktan sonra grubu seçin ve uygulamayı atayın.

    ![Çoklu oturum açmayı yapılandırma Ekle](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

> [!Note]
> Grupları atarken iç içe geçmiş gruplar desteklenmez.

28. Rol gruba atamak için rolü seçin ve tıklayın **atama** sayfanın düğmesi.

    ![Çoklu oturum açmayı yapılandırma Ekle](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

> [!Note]
> Azure portalında yeni rol görmek için oturumunuzu yenilemek gerektiğini lütfen unutmayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Amazon Web Services (AWS) kutucuğa tıkladığınızda, Amazon Web Services (AWS) uygulama sayfası rolü seçin seçeneğiyle almanız gerekir.

![Çoklu oturum açmayı yapılandırma Ekle](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_screen.png)

Rolleri talepler olarak geçirilen görmek için SAML yanıtını da doğrulayabilirsiniz.

![Çoklu oturum açmayı yapılandırma Ekle](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_saml.png)

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[11]: ./media/aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/aws-multi-accounts-tutorial/ic7950253.png
[36]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/aws-multi-accounts-tutorial/