---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı ZenDesk yapılandırma | Microsoft Docs"
description: "Otomatik olarak sağlamak ve kullanıcı hesaplarına ZenDesk sağlanmasını için Azure Active Directory yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: d9dcc7f81b32498802d52e864c834d785386985c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a>Öğretici: ZenDesk otomatik kullanıcı sağlamayı için yapılandırma


Bu öğreticinin amacı ZenDesk ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den ZenDesk sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı
*   ZenDesk Kiracı ile [Kurumsal planı](https://www.zendesk.com/product/pricing/) veya daha iyi etkin 
*   ZenDesk yönetici izinlerine sahip bir kullanıcı hesabı 

> [!NOTE]
> Tümleştirme sağlama Azure AD dayanan [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), daha iyi veya temel plan üzerindeki ZenDesk ekipleri için kullanılabilir.

## <a name="assigning-users-to-zendesk"></a>Kullanıcılar için ZenDesk atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir. 

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları ZenDesk uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar ZenDesk uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Kullanıcılar için ZenDesk atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için ZenDesk atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için ZenDesk atarken ya da seçmelisiniz **kullanıcı** rol ya da başka bir geçerli uygulamaya özgü rolü (varsa) atama iletişim. **Varsayılan erişim** rol sağlamak için çalışmaz ve bu kullanıcılar atlandı.

> [!NOTE]
> Eklenen bir özellik olarak sağlama hizmeti Zendesk içinde tanımlanan herhangi bir özel rolü okur ve bunları Azure AD rolü Seç iletişim kutusunda bunlar burada seçilebilir aktarır. Bu rolleri sağlama hizmetinin etkin ve bir eşitleme döngüsü tamamlandıktan sonra Azure Portalı'nda görünür.

## <a name="configuring-user-provisioning-to-zendesk"></a>Kullanıcı için ZenDesk sağlama yapılandırma 

Bu bölümde Azure AD ZenDesk'ın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama göre ZenDesk atanan kullanıcı hesaplarında devre dışı bırakın.

> [!TIP] 
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için ZenDesk, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.


### <a name="configure-automatic-user-account-provisioning-to-zendesk-in-azure-ad"></a>Otomatik olarak bir kullanıcı hesabı için ZenDesk Azure AD'de sağlamayı Yapılandır


1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için ZenDesk zaten yapılandırdıysanız arama alanı kullanarak ZenDesk Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **ZenDesk** uygulama galerisinde. Arama sonuçlarından ZenDesk seçin ve uygulamaları listenize ekleyin.

3. ZenDesk örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![ZenDesk sağlama](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. Altında **yönetici kimlik bilgileri** bölümü, giriş **yönetici kullanıcı adı, tokenkey & etki alanı** ZenDesk'ın hesap tarafından oluşturulan (belirteç hesabınızın altında bulabilirsiniz: **yönetici** > **API** > **ayarları**). 

    ![ZenDesk sağlama](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD ZenDesk uygulamanıza bağlanabilir. Bağlantı başarısız olursa, ZenDesk hesabınıza yönetici izinlerine sahip olduğundan emin olun ve 5. adım yeniden deneyin.

7. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alanına ve "bir hata oluştuğunda e-posta bildirimi gönder." onay kutusunu işaretleyin

8. **Kaydet** düğmesine tıklayın. 

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları ZenDesk**.

10. İçinde **öznitelik eşlemelerini** bölümünde, ZenDesk için Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri ZenDesk kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. ZenDesk hizmet sağlama Azure AD etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

12. **Kaydet** düğmesine tıklayın. 

Bu işlem, herhangi bir kullanıcı ve/veya grupları kullanıcıları ve grupları bölümünde ZenDesk atanan ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 20 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemler anlatılmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](active-directory-saas-provisioning-reporting.md)
