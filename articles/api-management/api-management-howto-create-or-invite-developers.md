---
title: Nasıl kullanıcı hesaplarını Azure API Management'te yönetme | Microsoft Docs
description: Oluşturma ve Azure API Management'te kullanıcıları davet öğrenin
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: 53ed914a3890aab1dd49536856feeb8975986e00
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Kullanıcı hesapları Azure API Management'te yönetme
API Yönetimi'nde, geliştiriciler API Management kullanarak kullanıma API'leri kullanıcılardır. Bu kılavuz, API'ları ve ürünlerini kullanmak için için nasıl oluşturulacağını ve geliştiricilerin davet gösterir, API Management örneği ile kendileri için kullanılabilir hale. Kullanıcı hesaplarını program aracılığıyla yönetme hakkında daha fazla bilgi için bkz: [kullanıcı varlığı](https://msdn.microsoft.com/library/azure/dn776330.aspx) belgelerinde [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) başvuru.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede görevleri tamamlayın: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-developer"> </a>Yeni bir geliştirici oluşturma

Yeni bir kullanıcı eklemek için bu bölümdeki adımları izleyin:

1. Seçin **kullanıcılar** ekranın sol sekmesine.
2. Tuşuna **+ Ekle**.
3. Kullanıcı için uygun bilgileri girin.
4. **Ekle**’ye basın.

    ![Yeni kullanıcı ekleme](./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png)

Varsayılan olarak, yeni oluşturulan Geliştirici hesaplardır **etkin**ve ilişkili **geliştiriciler** grubu. Geliştirici hesaplarının bir **etkin** durumu, tüm abonelikleri sahiptirler API'leri erişmek için kullanılabilir. Yeni oluşturulan Geliştirici ek gruplarıyla ilişkilendirmek için bkz: [grupları geliştiricilerle ilişkilendirme][How to associate groups with developers].

## <a name="invite-developer"> </a>Bir geliştirici davet et
Bir geliştirici davet etmek için bu bölümdeki adımları izleyin:

1. Seçin **kullanıcılar** ekranın sol sekmesine.
2. Tuşuna **+ davet**.

Bir onay iletisi görüntülenir, fakat daveti kabul ettikten sonra yeni davet edilen Geliştirici kadar listesinde görünmez. 

Bir geliştirici davet, geliştiriciler için bir e-posta gönderilir. Bu e-posta şablonu kullanılarak oluşturulan ve özelleştirilebilir. Daha fazla bilgi için bkz: [yapılandırma e-posta şablonlarını][Configure email templates].

Daveti kabul edildikten sonra hesap etkin hale gelir.

## <a name="block-developer"> </a> Bir geliştirici hesabı yeniden etkinleştirmek veya devre dışı bırakma

Varsayılan olarak, yeni oluşturulan veya davet edilen Geliştirici hesaplardır **etkin**. Bir geliştirici hesabını devre dışı bırakmak için tıklatın **blok**. Engellenen Geliştirici hesabı yeniden etkinleştirmek için tıklatın **etkinleştirme**. Engellenen Geliştirici hesabını Geliştirici portalına erişmek veya herhangi bir API çağrısı. Bir kullanıcı hesabını silmek için tıklatın **silmek**.

Bir kullanıcıyı engellemek için aşağıdaki adımları izleyin.

1. Seçin **kullanıcılar** ekranın sol sekmesine.
2. Engellemek istediğiniz kullanıcıya tıklayın.
3. Tuşuna **blok**.

## <a name="reset-a-user-password"></a>Kullanıcı parolasını sıfırlama

Program aracılığıyla kullanıcı hesapları ile çalışmak için bkz: [kullanıcı varlığı](https://msdn.microsoft.com/library/azure/dn776330.aspx) belgelerinde [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) başvuru. Belirli bir değere bir kullanıcı hesabı parolasını sıfırlamak için kullanabileceğiniz [kullanıcıyı güncelleştirmek](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) işlemi ve istenilen parola belirtin.

## <a name="next-steps"> </a>Sonraki adımlar
Bir geliştirici hesabı oluşturulduktan sonra rolleriyle ilişkilendirmek ve ürünleri ve API'ler için abone olun. Daha fazla bilgi için bkz: [grupları oluşturma ve kullanma konusunda][How to create and use groups].

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
