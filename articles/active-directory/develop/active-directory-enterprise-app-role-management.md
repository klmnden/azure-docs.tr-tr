---
title: Azure AD'de kurumsal uygulamalar için SAML belirtecinde verilen rol talep yapılandırma | Microsoft Docs
description: Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen rol talep yapılandırma hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: jeevansd
manager: CelesteDG
editor: ''
ms.assetid: eb2b3741-3cde-45c8-b639-a636f3df3b74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2019
ms.author: jeedes
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15165bce70a9bc2fbf3eb840ca8bce4fd5073280
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544622"
---
# <a name="how-to-configure-the-role-claim-issued-in-the-saml-token-for-enterprise-applications"></a>Nasıl yapılır: Kurumsal uygulamalar için SAML belirtecinde verilen rol talep yapılandırma

Azure Active Directory (Azure AD) kullanarak, talep türü rol talep için yanıt belirteci bir uygulamayı Yetkilendir sonra aldığınız özelleştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure AD aboneliğinin dizin kurulumu ile.
- Çoklu oturum etkin açma (SSO) sahip bir abonelik. SSO ile uygulamanızı yapılandırmanız gerekir.

## <a name="when-to-use-this-feature"></a>Bu özelliği kullanmak ne zaman

Uygulamanız SAML yanıt olarak geçirilecek özel roller görüyorsa, bu özelliği kullanmanız gerekir. Uygulamanızı Azure AD'den geçirilecek gerektiği kadar rolleri oluşturabilir.

## <a name="create-roles-for-an-application"></a>Bir uygulama rolleri oluşturma

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi.

    ![Azure Active Directory simgesi][1]

2. Seçin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölmesi][2]

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna, uygulamanızın adını yazın ve ardından sonuç panelinden uygulamanızı seçin. Seçin **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Uygulama sonuçları listesi](./media/active-directory-enterprise-app-role-management/tutorial_app_addfromgallery.png)

5. Uygulama eklendikten sonra Git **özellikleri** sayfasında ve nesne kimliği kopyalayın.

    ![Özellikler sayfası](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png)

6. Açık [Azure AD Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede ve aşağıdaki adımları uygulayın:

    a. Graph Gezgini siteye kiracınız için genel yönetici veya coadmin kimlik bilgilerini kullanarak oturum açın.

    b. Rolleri oluşturmak için yeterli izinlere ihtiyacınız var. Seçin **değiştirme izinleri** izin almak için.

      !["İzinlerini değiştirme" düğmesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new9.png)

    c. (Bunlar önceden yüklü değilse) listesi ve seçin, aşağıdaki izinleri seçin **değiştirme izinlerini**.

      ![İzinler ve "İzinleri değiştir" düğmesine listesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new10.png)

    > [!Note]
    > Dizin okuma ve yazma için genel yönetici izinlerine gerek duyduğunuz bulut uygulaması Yöneticisi ve uygulama yöneticisi rolü Bu senaryoda çalışmaz.

    d. Onay kabul edin. Sisteme tekrar oturum açtınız.

    e. Sürüm olarak değiştirin **beta**ve aşağıdaki sorguyu kullanarak hizmet sorumluları listesi kiracınızdan getirin:

     `https://graph.microsoft.com/beta/servicePrincipals`

      Birden çok dizini kullanıyorsanız, bu düzen izleyin: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

      ![Hizmet sorumlularını alma için sorgu ile Graph Gezgini iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

      > [!Note]
      > Müşteriler, hizmette bazı kesinti yaşanmasını görebilirsiniz API'leri yükseltme işlemindeyse çoktan BENİMSEDİ.

    f. Getirilen hizmet sorumluları listesinden değiştirmeniz gerekir. bir tane alın. Ctrl + F, tüm listelenen hizmet sorumlularını uygulamadan aramak için de kullanabilirsiniz. Öğesinden kopyalanan nesne kimliği için arama **özellikleri** sayfasında ve hizmet sorumlusuna almak için aşağıdaki sorguyu kullanın:

      `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

      ![Değiştirmek için gereken hizmet sorumlusu almaya yönelik sorgu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

    g. Ayıklama **appRoles** hizmet sorumlusu nesnesi özelliği.

      ![AppRoles özellik ayrıntıları](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

      > [!Note]
      > Özel bir uygulama (Azure Marketi'nde uygulama değil) kullanıyorsanız, iki varsayılan rolleri görürsünüz: kullanıcı ve msiam_access. Market uygulaması için msiam_access yalnızca varsayılan roldür. Varsayılan roller herhangi bir değişiklik yapmanız gerekmez.

    h. Uygulamanız için yeni roller oluşturun.

      Aşağıdaki JSON örneğidir **appRoles** nesne. Uygulamanız için istediğiniz rolleri eklemek için benzer bir nesne oluşturun.

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
      > Ayrıca, düzeltme eki işlemi için msiam_access sonra yalnızca yeni rolleri ekleyebilirsiniz. Ayrıca, kuruluş gereksinimlerinize kadar rolleri ekleyebilirsiniz. Azure AD, bu rolleri değerini SAML yanıtını talep değeri olarak gönderir. GUID üretmek için için yeni rol kimliği gibi web araçları değerlerinde [bu](https://www.guidgenerator.com/)

    i. Graph Explorer'a geri dönün ve yöntemden değiştirme **alma** için **düzeltme eki**. Düzeltme eki güncelleştirerek istenen rolleri için hizmet sorumlusu nesnesi **appRoles** özelliği önceki örnekte gösterilene benzer. Seçin **Sorgu Çalıştır** düzeltme eki işlemi yürütmek için. Başarılı iletisi rolünün oluşturulmasını onaylar.

      ![Başarılı iletisi ile düzeltme eki işlemi](./media/active-directory-enterprise-app-role-management/graph-explorer-new11.png)

7. Hizmet sorumlusu ile daha fazla rol düzeltme eki sonra kullanıcılar ilgili rollerine atayabilirsiniz. Kullanıcı Portalı'na gidip bir uygulama için gözatma atayabilirsiniz. Seçin **kullanıcılar ve gruplar** sekmesi. Bu sekme, tüm kullanıcılar ve uygulamaya zaten atanmış olan grupları listeler. Yeni roller üzerinde yeni kullanıcıları ekleyebilirsiniz. Ayrıca var olan bir kullanıcı seçin ve seçin **Düzenle** rolünü değiştirmek için.

    !["Kullanıcılar ve Gruplar" sekmesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new5.png)

    Herhangi bir kullanıcıya rol atamak için yeni rolünü seçin ve seçin **atama** sayfanın alt kısmındaki düğmesi.

    !["Atamasını Düzenle" bölmesi ve "Rol Seç" bölmesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new6.png)

    > [!Note]
    > Azure portalında yeni rol görmek için oturumunuzu yenilemek gerekir.

8. Güncelleştirme **öznitelikleri** rol talep özelleştirilmiş bir eşleme tablosunu.

9. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:

    | Öznitelik adı | Öznitelik değeri |
    | -------------- | ----------------|
    | Rol adı  | User.assignedroles |

    >[!NOTE]
    >Rol talep değeri null ise, ardından Azure AD bu değer belirtece göndermez ve tasarım göre bu varsayılandır.

    a. tıklayın **Düzenle** açmak için simgeyi **kullanıcı öznitelikleri ve talepler** iletişim.

      !["Öznitelik Ekle" düğmesi](./media/active-directory-enterprise-app-role-management/editattribute.png)

    b. İçinde **yönetmek, kullanıcı talepleri** iletişim kutusunda, tıklayarak SAML belirteci öznitelik Ekle **Ekle yeni talep**.

      !["Öznitelik Ekle" düğmesi](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

      !["Öznitelik Ekle" bölmesi](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    c. İçinde **adı** gerektiği şekilde öznitelik adı yazın. Bu örnekte **rol adı** talep adı.

    d. Bırakın **Namespace** kutusunu boş.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. **Kaydet**’i seçin.

10. Bir tek bir kimlik sağlayıcısı tarafından başlatılan oturum, uygulamanızı test etmek için oturum açın [erişim paneli](https://myapps.microsoft.com) ve, uygulama kutucuğunu seçin. SAML belirtecinde talep adıyla verdiğiniz kullanıcı için tüm atanan roller görmeniz gerekir.

## <a name="update-an-existing-role"></a>Mevcut bir rolü güncelleştirir

Mevcut bir rolü güncelleştirmek için aşağıdaki adımları gerçekleştirin:

1. Açık [Azure AD Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer).

2. Graph Gezgini siteye kiracınız için genel yönetici veya coadmin kimlik bilgilerini kullanarak oturum açın.

3. Sürüm olarak değiştirin **beta**ve aşağıdaki sorguyu kullanarak hizmet sorumluları listesi kiracınızdan getirin:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Birden çok dizini kullanıyorsanız, bu düzen izleyin: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Hizmet sorumlularını alma için sorgu ile Graph Gezgini iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

4. Getirilen hizmet sorumluları listesinden değiştirmeniz gerekir. bir tane alın. Ctrl + F, tüm listelenen hizmet sorumlularını uygulamadan aramak için de kullanabilirsiniz. Öğesinden kopyalanan nesne kimliği için arama **özellikleri** sayfasında ve hizmet sorumlusuna almak için aşağıdaki sorguyu kullanın:

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Değiştirmek için gereken hizmet sorumlusu almaya yönelik sorgu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

5. Ayıklama **appRoles** hizmet sorumlusu nesnesi özelliği.

    ![AppRoles özellik ayrıntıları](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

6. Mevcut rol güncelleştirmek için aşağıdaki adımları kullanın.

    !["Description" ve "vurgulanmış displayname" ile "Düzeltme eki için" istek gövdesi](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)

    a. Yöntemi Değiştir **alma** için **düzeltme eki**.

    b. Var olan rolleri kopyalayıp altında **istek gövdesi**.

    c. Rol açıklaması, rol değeri veya rol görünen adı gerektiği gibi güncelleştirerek rol değerini güncelleştirin.

    d. Tüm gerekli rollerini güncelleştirdikten sonra seçin **Sorgu Çalıştır**.

## <a name="delete-an-existing-role"></a>Mevcut bir rolü siler

Mevcut bir rolü silmek için aşağıdaki adımları gerçekleştirin:

1. Açık [Azure AD Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer) başka bir pencerede.

2. Graph Gezgini siteye kiracınız için genel yönetici veya coadmin kimlik bilgilerini kullanarak oturum açın.

3. Sürüm olarak değiştirin **beta**ve aşağıdaki sorguyu kullanarak hizmet sorumluları listesi kiracınızdan getirin:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Birden çok dizini kullanıyorsanız, bu düzen izleyin: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Hizmet sorumluları listesi getirilirken için sorgu ile Graph Gezgini iletişim kutusu](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

4. Getirilen hizmet sorumluları listesinden değiştirmeniz gerekir. bir tane alın. Ctrl + F, tüm listelenen hizmet sorumlularını uygulamadan aramak için de kullanabilirsiniz. Öğesinden kopyalanan nesne kimliği için arama **özellikleri** sayfasında ve hizmet sorumlusuna almak için aşağıdaki sorguyu kullanın:

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Değiştirmek için gereken hizmet sorumlusu almaya yönelik sorgu](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

5. Ayıklama **appRoles** hizmet sorumlusu nesnesi özelliği.

    ![Hizmet sorumlusu nesnesi appRoles özelliğinden ayrıntıları](./media/active-directory-enterprise-app-role-management/graph-explorer-new7.png)

6. Mevcut bir rolü silmek için aşağıdaki adımları kullanın.

    !["Düzeltme eki için" false olarak ayarlanmış IsEnabled ile istek gövdesi](./media/active-directory-enterprise-app-role-management/graph-explorer-new8.png)

    a. Yöntemi Değiştir **alma** için **düzeltme eki**.

    b. Var olan rolleri uygulamadan kopyalayıp altında **istek gövdesi**.

    c. Ayarlama **IsEnabled** değerini **false** silmek istediğiniz rolü için.

    d. Seçin **sorgusu**.

    > [!NOTE]
    > Msiam_access rolüne sahip ve oluşturulan rolünde eşleşen kimliği emin olun.

7. Rol devre dışı bırakıldıktan sonra bu rolü bloğundan Sil **appRoles** bölümü. Yöntemi olarak tutmak **düzeltme eki**seçip **Sorgu Çalıştır**.

8. Rol, sorgu çalıştırdıktan sonra silinir.

    > [!NOTE]
    > Rol kaldırılmadan önce devre dışı bırakılması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Ek adımlar için bkz: [app belgeleri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png
