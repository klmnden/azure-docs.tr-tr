---
title: Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen rol talep yapılandırma | Microsoft Docs
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
ms.date: 05/30/2018
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: 5aa716f91a3155e81ef8dc7c436b4a9a5811238b
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34723261"
---
# <a name="configure-the-role-claim-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen rol talep yapılandırma

Azure Active Directory (Azure AD) kullanarak, talep türü rol talep için yanıt belirteçte uygulama yetkilendirmek sonra aldığınız özelleştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
- Bir dizin Kurulumu Azure AD abonelikle.
- Çoklu oturum etkin açma (SSO) sahip bir abonelik. SSO ile uygulamanızı yapılandırmanız gerekir.

## <a name="when-to-use-this-feature"></a>Bu özelliği kullanmak ne zaman

Uygulamanızı bir SAML yanıtında geçirilecek özel roller görüyorsa, bu özelliği kullanmak gerekir. Uygulamanızı Azure AD'den geçirilecek gerektiği kadar rolleri oluşturabilirsiniz.

## <a name="create-roles-for-an-application"></a>Bir uygulama için roller oluşturma

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory simgesi][1]

2. Seçin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölmesi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    !["Yeni uygulamayı" düğmesi][3]

4. Arama kutusuna uygulamanızın adını yazın ve ardından uygulamanızı sonuç panelinden seçin. Seçin **Ekle** uygulama eklemek için düğmesi.

    ![Sonuçlar listesinde uygulama](./media/active-directory-enterprise-app-role-management/tutorial_app_addfromgallery.png)

5. Uygulama eklendikten sonra Git **özellikleri** sayfasında ve nesne kimliği kopyalayın.

    ![Özellikler sayfası](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png)

6. Açık [Azure AD Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede ve aşağıdaki adımları uygulayın:

    a. Graph Explorer'a siteye kiracınız için genel yönetici veya ortak yönetici kimlik bilgilerini kullanarak oturum açın.

    b. Rolleri oluşturmak için yeterli izinleri gerekir. Seçin **değiştirme izinleri** için izin almak üzere. 

      !["İzinleri değiştirme" düğmesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new9.png)

    c. Aşağıdaki izinleri (, bunlar zaten yoksa) listesi ve select seçin **izinleri değiştir**. 

      ![İzinler ve "İzinleri değiştir" düğmesi listesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new10.png)

    d. Onay kabul edin. Sistem yeniden oturum açtınız.

    e. Sürüm olarak değiştirin **beta**ve aşağıdaki sorguyu kullanarak Kiracı hizmet asıl adı listesi getirilemedi:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
      Birden çok dizin kullanıyorsanız, bu deseni izleyin: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
      ![Hizmet sorumluları getirme için sorguyla grafik Explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
    f. Getirilen hizmet asıl adı listesinden değiştirmek için gereken bir alın. Ctrl + F, tüm listelenen hizmet asıl adı uygulamadan aramak için de kullanabilirsiniz. Öğesinden kopyalanan nesne kimliği Ara **özellikleri** sayfasında ve hizmet sorumlusu almak için aşağıdaki sorguyu kullanın:
    
      `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

      ![Değişiklik yapmanız hizmet sorumlusu almak için sorgu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

    g. Extract **appRoles** hizmet sorumlusu nesnesi özelliğinden. 

      ![AppRoles özelliğinin ayrıntıları](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

      > [!Note]
      > Özel uygulama (Azure Market uygulaması değil) kullanıyorsanız, iki varsayılan rolleri Bkz: kullanıcı ve msiam_access. Market uygulaması için msiam_access yalnızca varsayılan rolüdür. Varsayılan rollerinde herhangi bir değişiklik yapmak gerekmez.

    h. Uygulamanız için yeni rolleri oluşturun. 

      Aşağıdaki JSON örneğidir **appRoles** nesnesi. Uygulamanız için istediğiniz rolleri eklemek için benzer bir nesnesi oluşturun. 

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
      > Düzeltme eki işlemi için msiam_access sonra yeni rolleri yalnızca ekleyebilirsiniz. Ayrıca, kuruluş gereksinimlerinize kadar rolleri ekleyebilirsiniz. Azure AD değeri bu roller, talep değeri SAML yanıt olarak gönderir. GUID üretmek için web araçları gibi yeni rollerinin Kimliğini değerlerini kullanın [bu](https://www.guidgenerator.com/)
    
    i. Graph Explorer'a geri dönün ve yönteminden değiştirin **almak** için **düzeltme eki**. İstenen rollerin güncelleştirerek sağlamak için hizmet sorumlusu nesnesi düzeltme eki **appRoles** özelliği önceki örnekte gösterilene benzer. Seçin **sorgu çalıştırma** düzeltme eki işlemi yürütmek için. Bir başarı iletisi rolünün oluşturulmasını onaylar.

      ![Düzeltme eki işlemi başarı iletisi](./media/active-directory-enterprise-app-role-management/graph-explorer-new11.png)

7. Hizmet sorumlusu daha fazla rolleriyle uygulandıktan sonra kullanıcılar ilgili roller atayabilirsiniz. Kullanıcı Portalı'na gidip uygulama için gözatma atayabilirsiniz. Seçin **kullanıcılar ve gruplar** sekmesi. Bu sekme, tüm kullanıcılar ve uygulamaya zaten atanmış grupları listeler. Yeni roller üzerinde yeni kullanıcılar ekleyebilir. Ayrıca varolan bir kullanıcı seçin ve Seç **Düzenle** rolünü değiştirmek için.

    !["Kullanıcılar ve Gruplar" sekmesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new5.png)

    Herhangi bir kullanıcı rolü atamak için yeni rolünü seçin ve seçin **atamak** sayfanın düğmesinde.

    !["Atama Düzenle" ve "Rol Seç" bölmesinde](./media/active-directory-enterprise-app-role-management/graph-explorer-new6.png)

    > [!Note]
    > Yeni rolleri görmek için Azure portal oturumunuzda yenilemeniz gerekir.

8. Güncelleştirme **öznitelikleri** rol talep özelleştirilmiş bir eşleme tablosunu.

9. İçinde **kullanıcı öznitelikleri** bölümünü **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin.
    
    | Öznitelik adı | Öznitelik değeri |
    | -------------- | ----------------|    
    | Rol adı      | User.assignedrole |

    a. Seçin **Ekle özniteliği** açmak için **özniteliği eklemek** bölmesi.

      !["Öznitelik Ekle" düğmesi](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

      !["Öznitelik Ekle" bölmesi](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. İçinde **adı** gerektiği şekilde öznitelik adı yazın. Bu örnekte **rol adı** talep adı.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** kutusu boş.
    
    e. Seçin **Tamam**.

10. Bir tek bir kimlik sağlayıcısı tarafından başlatılan oturum içinde uygulamanızı test etmek için oturum açın [erişim paneli](https://myapps.microsoft.com) ve uygulama kutucuğunuzun seçin. SAML belirteci atanan tüm rolleri verdiğiniz talep ada sahip kullanıcı için görmeniz gerekir.

## <a name="update-an-existing-role"></a>Mevcut bir rolü güncelleştirir

Mevcut bir rolü güncelleştirmek için aşağıdaki adımları gerçekleştirin:

1. Açık [Azure AD Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer).

2. Graph Explorer'a siteye kiracınız için genel yönetici veya ortak yönetici kimlik bilgilerini kullanarak oturum açın.
    
3. Sürüm olarak değiştirin **beta**ve aşağıdaki sorguyu kullanarak Kiracı hizmet asıl adı listesi getirilemedi:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Birden çok dizin kullanıyorsanız, bu deseni izleyin: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Hizmet sorumluları getirme için sorguyla grafik Explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Getirilen hizmet asıl adı listesinden değiştirmek için gereken bir alın. Ctrl + F, tüm listelenen hizmet asıl adı uygulamadan aramak için de kullanabilirsiniz. Öğesinden kopyalanan nesne kimliği Ara **özellikleri** sayfasında ve hizmet sorumlusu almak için aşağıdaki sorguyu kullanın:
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Değişiklik yapmanız hizmet sorumlusu almak için sorgu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. Extract **appRoles** hizmet sorumlusu nesnesi özelliğinden.
    
    ![AppRoles özelliğinin ayrıntıları](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)
    
6. Mevcut rolünü güncelleştirmek için aşağıdaki adımları kullanın.

    !["Açıklama" ve "vurgulanmış displayname" ile "Düzeltme eki için" istek gövdesindeki](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)
    
    a. Yönteminden değiştirme **almak** için **düzeltme eki**.

    b. Mevcut rollerin kopyalayıp altında **iste gövde**.

    c. Bir rolü değerini rol açıklaması, rolü değeri veya rol görünen adı gerektiği gibi güncelleştirerek güncelleştirin.

    d. Gerekli tüm rolleri güncelleştirdikten sonra Seç **sorgu çalıştırma**.
        
## <a name="delete-an-existing-role"></a>Mevcut bir rolü silme

Mevcut bir rolü silmek için aşağıdaki adımları gerçekleştirin:

1. Açık [Azure AD Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede.

2. Graph Explorer'a siteye kiracınız için genel yönetici veya ortak yönetici kimlik bilgilerini kullanarak oturum açın.

3. Sürüm olarak değiştirin **beta**ve aşağıdaki sorguyu kullanarak Kiracı hizmet asıl adı listesi getirilemedi:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Birden çok dizin kullanıyorsanız, bu deseni izleyin: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Hizmet sorumluları listesini getirme için sorguyla grafik Explorer iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Getirilen hizmet asıl adı listesinden değiştirmek için gereken bir alın. Ctrl + F, tüm listelenen hizmet asıl adı uygulamadan aramak için de kullanabilirsiniz. Öğesinden kopyalanan nesne kimliği Ara **özellikleri** sayfasında ve hizmet sorumlusu almak için aşağıdaki sorguyu kullanın:
     
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Değişiklik yapmanız hizmet sorumlusu almak için sorgu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. Extract **appRoles** hizmet sorumlusu nesnesi özelliğinden.
    
    ![Hizmet sorumlusu nesnesi appRoles özelliğinden ayrıntıları](./media/active-directory-enterprise-app-role-management/graph-explorer-new7.png)

6. Mevcut rolünü silmek için aşağıdaki adımları kullanın.

    !["Düzeltme eki için" false olarak ayarlanmış IsEnabled ile istek gövdesindeki](./media/active-directory-enterprise-app-role-management/graph-explorer-new8.png)

    a. Yönteminden değiştirme **almak** için **düzeltme eki**.

    b. Mevcut rollerin uygulamadan kopyalayıp altında **iste gövde**.
        
    c. Ayarlama **IsEnabled** değeri **false** silmek istediğiniz rolü için.

    d. Seçin **sorgusu**.
    
    > [!NOTE] 
    > Msiam_access rolüne sahip ve oluşturulan rolünde eşleşen kimliği emin olun.
    
7. Rolü devre dışı bırakıldıktan sonra bu rolü bloğundan silme **appRoles** bölümü. Yöntemi olarak tutun **düzeltme eki**seçip **sorgu çalıştırma**.
    
8. Sorgu çalıştırdıktan sonra rol silinir.
    
    > [!NOTE]
    > Rolü kaldırılmadan önce devre dışı bırakılması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

Ek adımlar için bkz: [uygulama belgelerine](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png