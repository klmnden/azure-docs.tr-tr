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
ms.openlocfilehash: 88a9f5988d1fe3f4de4fe10da23a5f713e3f3370
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
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

5. Uygulama eklendikten sonra Git **özellikleri** sayfası ve kopyalama **nesne kimliği**

    ![Özellikler sayfası](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png)

6. Açık [Azure AD Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede.

    a. Graph Explorer'a sitenin kiracınız için genel yönetici/ortak yönetici kimlik bilgilerini kullanarak oturum açın.

    b. Sürüm olarak değiştirin **beta** ve sorguyu kullanarak Kiracı gelen hizmet asıl adı listesi getirilemedi:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Birden çok dizin kullanıyorsanız, bu deseni izlemelidir `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    c. Alınan hizmet asıl adı listesinden değiştirmek için gereken bir alın. Ctrl + F, tüm listelenen ServicePrincipals uygulamadan aramak için de kullanabilirsiniz. Arama **nesne kimliği**, hangi özellikleri sayfasında kopyaladığınız ve ilgili hizmet sorumlusu almak için sorguyu kullanın.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    d. AppRoles özelliği hizmet asıl nesneden ayıklayın.

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)

    e. Artık, uygulamanız için yeni rolleri oluşturmak gerekir. Azure AD rol Oluşturucu indirebilirsiniz gelen [burada](https://app.box.com/s/jw6m9p9ehmf4ut5jx8xhcw87cu09ml3y).

    f. Azure AD Oluşturucu açın ve adımları gerçekleştirin-

    ![Azure AD Oluşturucusu](./media/active-directory-enterprise-app-role-management/azure_ad_role_generator.png)
    
    Girin **rol adı**, **rol açıklaması**, ve **rolü değeri**. Tıklatın **Ekle** rolü eklemek için
    
    Gerekli tüm rolleri ekledikten sonra tıklatın **oluştur**
    
    Tıklatarak içeriği Kopyala **kopyalama içeriği**

    > [!NOTE] 
    > Lütfen sahip olduğunuzdan emin olun **msiam_access** kullanıcı rolü ve kimliği oluşturulan rolünde eşleşen.

    g. Graph Explorer'a geri dönün. Yönteminden değiştirme **almak** için **düzeltme eki**. Hizmet sorumlusu nesnesi appRoles kopyalanan değerlerle appRoles özelliği güncelleştirerek istenen için düzeltme eki. Tıklatın **sorgusu**.

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-patch.png)

    > [!NOTE]
    > Aşağıdaki appRoles nesnesinin bir örnektir. 
    > 
    >   ```
    > {
    > "appRoles": [
    > {
    >   "allowedMemberTypes": [
    >   "User"
    >   ],
    >   "description": "msiam_access",
    >   "displayName": "msiam_access",
    >   "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
    >   "isEnabled": true,
    >   "origin": "Application",
    >   "value": null
    > },
    > {
    >   "allowedMemberTypes": [
    >   "User"
    >   ],
    >   "description": "teacher",
    >   "displayName": "teacher",
    >   "id": "6478ffd2-5dbd-4584-b2ce-137390b09b60",
    >   "isEnabled": ,
    >   "origin": "ServicePrincipal",
    >   "value": "teacher"
    > }
    > ] 
    > }
    >
    >   ```

7. Hizmet sorumlusu daha fazla rolleriyle uygulandıktan sonra kullanıcılar ilgili roller atayabilirsiniz. Bu Portalı'na gidip ilgili uygulama gezinme yapılabilir. Ardından, tıklayarak **kullanıcılar ve gruplar** üst sekmesinde. Bu işlem tüm kullanıcıları veya grupları listeler.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-enterprise-app-role-management/userrole.png)

    a. Bir rol için herhangi bir kullanıcı atamak için yalnızca belirli kullanıcı/grup seçin ve tıklayın **atamak** sayfanın alt kısmında düğmesi.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-enterprise-app-role-management/userandgroups.png)

    b. Tıklamak bir rolü asıl ilgili hizmet için tanımlanan farklı roller seçmek için açılır pencere getirir.

    c. ' I tıklatın ve gerekli rol gönderme seçin.

8. Kullanıcılara roller atama sonra güncelleştirmek ihtiyacımız **öznitelikleri** özelleştirilmiş Eşleme tablosunu **rol** talep.

9. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | -------------- | ----------------|    
    | Rol Adı      | User.assignedrole |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

    ![Çoklu oturum açma özniteliği yapılandırın](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.

10. Çoklu oturum açma IDP'de uygulamanızı test etmek için erişim paneline oturum açma, başlatılan (https://myapps.microsoft.com) , uygulama kutucuğuna tıklayın. SAML belirtecinde atanan tüm rolleri verdiğiniz talep ada sahip kullanıcı için görmeniz gerekir.

## <a name="update-existing-role"></a>Mevcut bir rolü güncelleştirir

1. Mevcut bir rolü güncelleştirmek için adımları izleyerek-

    a. Açık [Azure AD Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede.

    b. Graph Explorer'a sitenin kiracınız için genel yönetici/ortak yönetici kimlik bilgilerini kullanarak oturum açın.
    
    c. Sürüm olarak değiştirin **beta** ve sorguyu kullanarak Kiracı gelen hizmet asıl adı listesi getirilemedi:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
        
    Birden çok dizin kullanıyorsanız, bu deseni izlemelidir `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    d. Alınan hizmet asıl adı listesinden değiştirmek için gereken bir alın. Ctrl + F, tüm listelenen ServicePrincipals uygulamadan aramak için de kullanabilirsiniz. Arama **nesne kimliği**, hangi özellikleri sayfasında kopyaladığınız ve ilgili hizmet sorumlusu almak için sorguyu kullanın.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.
    
    e. AppRoles özelliği hizmet asıl nesneden ayıklayın.
    
    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)
    
    f. Mevcut rolünü güncelleştirmek için aşağıdaki adımları izleyin:

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)
    
    * Yönteminden değiştirme **almak** için **düzeltme eki**.

    * Mevcut rollerin uygulamadan kopyalayıp bunları içinde **iste gövde**.
    
    * Rol değerini değiştirerek güncelleştirme **rol açıklaması**, **rolü değeri**, ve **rol displayname** kuruluş gereksiniminden göredir.
    
    * Gerekli tüm rolleri güncelleştirildikten sonra tıklatın **sorgu çalıştırma**.
        
## <a name="delete-existing-role"></a>Varolan rolünü silme

1. Mevcut bir rolü silmek için adımları izleyerek-

    a. Açık [Azure AD Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede.

    b. Graph Explorer'a sitenin kiracınız için genel yönetici/ortak yönetici kimlik bilgilerini kullanarak oturum açın.

    c. Sürüm olarak değiştirin **beta** ve sorguyu kullanarak Kiracı gelen hizmet asıl adı listesi getirilemedi:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Birden çok dizin kullanıyorsanız, bu deseni izlemelidir `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    d. Alınan hizmet asıl adı listesinden değiştirmek için gereken bir alın. Ctrl + F, tüm listelenen ServicePrincipals uygulamadan aramak için de kullanabilirsiniz. Arama **nesne kimliği**, hangi özellikleri sayfasında kopyaladığınız ve ilgili hizmet sorumlusu almak için sorguyu kullanın.
     
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.
    
    e. AppRoles özelliği hizmet asıl nesneden ayıklayın.
    
    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)

    f. Mevcut rolünü silmek için aşağıdaki adımları izleyin:

    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-patchdelete.png)

    Yönteminden değiştirme **almak** için **düzeltme eki**.

    Mevcut rollerin uygulamadan kopyalayıp bunları **iste gövde**.
    
    Ayarlama **IsEnabled** değeri **false** silmek istediğiniz rolü

    Tıklatın **sorgusu**.
    
    > [!NOTE] 
    > Lütfen sahip olduğunuzdan emin olun **msiam_access** kullanıcı rolü ve kimliği oluşturulan rolünde eşleşen.
    
    g. Yukarıdaki işlem yaptıktan sonra yöntemi olarak tutun **düzeltme eki** ve içeriği remianing rol yapıştırma **iste gövde** tıklatıp **sorgu çalıştırma**.
    
    ![Grafik explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-patchfinal.png)

    h. Sorgu çalıştırdıktan sonra rol silinir.
    
    > [!NOTE]
    > Rolü kaldırılmadan önce ilk devre dışı bırakılması gerekir. 

## <a name="next-steps"></a>Sonraki Adımlar

Başvuru [uygulama belgeleri ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-tutorial-list) ek adımlar için.

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png
