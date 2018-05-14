---
title: Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen yapılandırma rol talep | Microsoft Docs
description: Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen rol talep yapılandırma konusunda bilgi edinin
services: active-directory
documentationcenter: ''
author: jeevansd
manager: mtillman
editor: ''
ms.assetid: eb2b3741-3cde-45c8-b639-a636f3df3b74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2018
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: 43a4db9114cd47da5bef98ed634847b547589b47
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="configuring-role-claim-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen rol talep yapılandırma

Bu özellik, kullanıcıların Azure AD kullanarak bir uygulama yetkilendirme temel alınan yanıt belirtecinde 'rolleri' talep için talep türü özelleştirme olanak tanır.

## <a name="prerequisites"></a>Önkoşullar
- Bir Azure AD directory Kurulum abonelikle
- Bir çoklu oturum açma abonelik etkin
- SSO ile uygulamanızı yapılandırmanız gerekir

## <a name="when-to-use-this-feature"></a>Bu özelliği kullanmak ne zaman

Uygulamanızı SAML yanıt olarak geçirilecek özel roller görüyorsa, bu özelliği kullanmak gerekir. Bu özellik, uygulamanızı Azure AD'den geçirilecek gerektiği gibi birçok rol oluşturmanıza olanak sağlar.

## <a name="steps-to-use-this-feature"></a>Bu özelliği kullanmak için adımları

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna, uygulamanızın adını yazın, uygulamanızın sonuç panelinden seçin'ı tıklatın **Ekle** uygulama eklemek için düğmesi.

    ![Sonuçlar listesinde uygulama](./media/active-directory-enterprise-app-role-management/tutorial_app_addfromgallery.png)

5. Uygulama eklendikten sonra Git **özellikleri** sayfası ve kopyalama **nesne kimliği**.

<!-- ![Properties Page](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png) Note: Image is missing. -->

6. Açık [Azure AD Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede.

    a. Graph Explorer'a sitenin kiracınız için genel yönetici/ortak yönetici kimlik bilgilerini kullanarak oturum açın.

    b. Rolleri oluşturmak için yeterli izinleri olması gerekir. Tıklayın **değiştirme izinleri** gerekli izinleri almak için. 

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new9.png)

    c. Aşağıdaki izinleri (, bunlar zaten yoksa) listesinden seçin ve "İzinleri değiştir"'i tıklatın 

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new10.png)

    d. Bu, oturum açmayı yeniden ve onay kabul etmek ister. Onay kabul ettikten sonra sisteme yeniden kaydedilir.

    e. Sürüm olarak değiştirin **beta** ve sorguyu kullanarak Kiracı gelen hizmet asıl adı listesi getirilemedi:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Birden çok dizin kullanıyorsanız, bu deseni izlemelidir `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
    f. Alınan hizmet asıl adı listesinden değiştirmek için gereken bir alın. Ctrl + F, tüm listelenen ServicePrincipals uygulamadan aramak için de kullanabilirsiniz. Arama **nesne kimliği**, hangi özellikleri sayfasında kopyaladığınız ve ilgili hizmet sorumlusu almak için sorguyu kullanın.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

    g. AppRoles özelliği hizmet asıl nesneden ayıklayın. 

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

    > [!Note]
    > Özel (Galerisi olmayan) uygulama kullanıyorsanız, iki varsayılan rolleri - kullanıcı ve msiam_access bakın. Galeri uygulama durumunda msiam_access yalnızca varsayılan rolüdür. Varsayılan rollerinde herhangi bir değişiklik yapmanız gerekmez.

    h. Artık uygulamanız için yeni rolleri oluşturmak gerekir. 

    i. JSON appRoles nesnesinin bir örnektir. Uygulamanız için istediğiniz rolleri eklemek için benzer bir nesne oluşturun. 

    ```
    {
       "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "b9632174-c057-4f7e-951b-be3adc52bfe6",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Administrators Only",
            "displayName": "Admin",
            "id": "4f8f8640-f081-492d-97a0-caf24e9bc134",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "Administrator"
        }
    ]
    }
    ```
    > [!Note]
    > Sonra yeni rolleri yalnızca ekleyebilirsiniz **msiam_access** düzeltme eki işlemi için. Ayrıca, kuruluş gereksiniminizi istediğiniz sayıda rolleri ekleyebilirsiniz. Azure AD gönderecek **değeri** SAML yanıt talep değeri olarak bu rollerin.
    
    j. Graph Explorer'a geri dönün ve yönteminden değiştirin **almak** için **düzeltme eki**. Hizmet sorumlusu nesnesi rolleri appRoles özelliği yukarıdaki örnekte gösterilene benzer güncelleştirerek istenen için düzeltme eki. Tıklatın **sorgu çalıştırma** düzeltme eki işlemi yürütmek için. Bir başarı iletisi rolünün oluşturulmasını onaylar.

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new11.png)

7. Hizmet sorumlusu daha fazla rolleriyle uygulandıktan sonra kullanıcılar ilgili roller atayabilirsiniz. Bu Portalı'na gidip ilgili uygulama gezinme yapılabilir. Tıklayın **kullanıcılar ve gruplar** üst sekmesinde. Bu, tüm kullanıcılar ve uygulamanın zaten atanan grupları listeler. Yeni rolü yeni kullanıcı ekleme ve ayrıca varolan bir kullanıcı seçin ve'ı tıklatın **Düzenle** rolünü değiştirmek için.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-enterprise-app-role-management/graph-explorer-new5.png)

     Herhangi bir kullanıcı rolüne atamak için yeni rolünü seçin ve tıklayın **atamak** sayfanın düğmesini.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-enterprise-app-role-management/graph-explorer-new6.png)

    > [!Note]
    > Yeni rolleri görmek için Azure portalında oturumunuzu yenilemek gerektiğini unutmayın.

8. Kullanıcılara roller atama sonra güncelleştirmek ihtiyacımız **öznitelikleri** özelleştirilmiş Eşleme tablosunu **rol** talep.

9. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | -------------- | ----------------|    
    | Rol Adı      | User.assignedrole |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

    ![Çoklu oturum açma özniteliği yapılandırın](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, gerektiği şekilde öznitelik adı yazın. Bu örnekte kullandık **rol adı** adı talebi olarak.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.

10. Çoklu oturum açma IDP'de uygulamanızı test etmek için erişim paneline oturum açma, başlatılan (https://myapps.microsoft.com) , uygulama kutucuğuna tıklayın. SAML belirteci atanan tüm rolleri verdiğiniz talep ada sahip kullanıcı için görmeniz gerekir.

## <a name="update-existing-role"></a>Mevcut bir rolü güncelleştirir

Mevcut bir rolü güncelleştirmek için adımları izleyerek-

1. Açık [Azure AD Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer).

2. Graph Explorer'a sitenin kiracınız için genel yönetici/ortak yönetici kimlik bilgilerini kullanarak oturum açın.
    
3. Sürüm olarak değiştirin **beta** ve sorguyu kullanarak Kiracı gelen hizmet asıl adı listesi getirilemedi:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Birden çok dizin kullanıyorsanız, bu deseni izlemelidir `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Alınan hizmet asıl adı listesinden değiştirmek için gereken bir alın. Ctrl + F, tüm listelenen ServicePrincipals uygulamadan aramak için de kullanabilirsiniz. Arama **nesne kimliği**, hangi özellikleri sayfasında kopyaladığınız ve ilgili hizmet sorumlusu almak için sorguyu kullanın.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. AppRoles özelliği hizmet asıl nesneden ayıklayın.
    
    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)
    
6. Mevcut rolünü güncelleştirmek için aşağıdaki adımları izleyin:

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)
    
    * Yönteminden değiştirme **almak** için **düzeltme eki**.

    * Mevcut rollerin kopyalayıp bunları içinde **iste gövde**.

    * Rol değerini güncelleştirmek **rol açıklaması**, **rolü değeri** veya **rol displayname** gerektiğinde.

    * Gerekli tüm rolleri güncelleştirildikten sonra tıklatın **sorgu çalıştırma**.
        
## <a name="delete-existing-role"></a>Varolan rolünü silme

Mevcut bir rolü silmek için şu adımları gerçekleştirin:

1. Açık [Azure AD Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede.

2. Graph Explorer'a sitenin kiracınız için genel yönetici/ortak yönetici kimlik bilgilerini kullanarak oturum açın.

3. Sürüm olarak değiştirin **beta** ve sorguyu kullanarak Kiracı gelen hizmet asıl adı listesi getirilemedi:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Birden çok dizin kullanıyorsanız, bu deseni izlemelidir `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Alınan hizmet asıl adı listesinden değiştirmek için gereken bir alın. Ctrl + F, tüm listelenen ServicePrincipals uygulamadan aramak için de kullanabilirsiniz. Arama **nesne kimliği**, hangi özellikleri sayfasında kopyaladığınız ve ilgili hizmet sorumlusu almak için sorguyu kullanın.
     
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. AppRoles özelliği hizmet asıl nesneden ayıklayın.
    
    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new7.png)

6. Mevcut rolünü silmek için aşağıdaki adımları izleyin:

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new8.png)

    * Yönteminden değiştirme **almak** için **düzeltme eki**.

    * Mevcut rollerin uygulamadan kopyalayıp bunları **iste gövde**.
        
    * Ayarlama **IsEnabled** değeri **false** silmek istediğiniz rolü

    * Tıklatın **sorgusu**.
    
    > [!NOTE] 
    > Lütfen sahip olduğunuzdan emin olun **msiam_access** kullanıcı rolü ve kimliği oluşturulan rolünde eşleşen.
    
7. Rolü devre dışı bırakıldığında, bu rolü blok appRoles bölümünden silmek, yöntemi olarak tutun **düzeltme eki** tıklatıp **sorgu çalıştırma**.
    
8. Sorgu çalıştırdıktan sonra rol silinir.
    
    > [!NOTE]
    > Rolü kaldırılmadan önce ilk devre dışı bırakılması gerekir. 

## <a name="next-steps"></a>Sonraki Adımlar

Başvuru [uygulama belgeleri ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) ek adımlar için.

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png
