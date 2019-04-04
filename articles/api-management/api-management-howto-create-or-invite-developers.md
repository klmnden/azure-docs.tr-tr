---
title: Kullanıcı hesapları Azure API Management'te yönetme | Microsoft Docs
description: Oluşturma veya kullanıcıları Azure API Management'ta davet öğrenin
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
ms.openlocfilehash: 6a4ad827c39ec106acdc7b52a5b769ab6e7febf8
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893954"
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Azure API Yönetimi'nde kullanıcı hesaplarını yönetme

API Yönetimi'nde, geliştiricilerin API Management kullanarak kullanıma sunan API'leri kullanıcılarıdır. Bu kılavuz, ürünlerini ve API'leri kullanmak için nasıl oluşturulacağı ve geliştiricilerin davet gösterir. API Management örneğinizin kullanabilecekleri olun. Programlı olarak kullanıcı hesaplarını yönetme hakkında daha fazla bilgi için bkz: [kullanıcı varlığı](https://docs.microsoft.com/en-us/rest/api/apimanagement/user) belgelerinde [API Management REST](/rest/api/apimanagement/) başvuru.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki görevleri tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-developer"> </a>Yeni bir geliştiricinin oluşturma

Yeni bir kullanıcı eklemek için bu bölümdeki adımları izleyin:

1. Seçin **kullanıcılar** ekranın sol tarafındaki için sekmesinde.
2. Tuşuna **+ Ekle**.
3. Kullanıcı için uygun bilgileri girin.
4. **Ekle**’ye basın.

    ![Yeni kullanıcı ekleme](./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png)

Varsayılan olarak, yeni oluşturulan Geliştirici hesaplarıdır **etkin**ve ilişkili **geliştiriciler** grubu. Geliştirici hesaplarının bir **etkin** durumu, tüm abonelikleri sahiptirler API'leri erişmek için kullanılabilir. Yeni oluşturulan Geliştirici ek gruplarıyla ilişkilendirmek için bkz: [grupları geliştiricilerle ilişkilendirme][How to associate groups with developers].

## <a name="invite-developer"> </a>Bir geliştirici davet edin
Bir geliştirici davet etmek için bu bölümdeki adımları izleyin:

1. Seçin **kullanıcılar** ekranın sol tarafındaki için sekmesinde.
2. Tuşuna **+ davet**.

Bir onay iletisi görüntülenir ancak daveti kabul sonra yeni davet edilen Geliştirici kadar listesinde görünmez. 

Bir geliştirici davet, geliştiriciler için bir e-posta gönderilir. Bu e-posta, bir şablon kullanılarak oluşturulur ve özelleştirilebilir. Daha fazla bilgi için [e-posta şablonlarını yapılandırma][Configure email templates].

Daveti kabul edildikten sonra hesap etkin olur.

## <a name="block-developer"> </a> Bir geliştirici hesabı yeniden etkinleştir veya devre dışı bırakma

Varsayılan olarak, yeni oluşturduğunuz veya davet edilen Geliştirici hesaplarıdır **etkin**. Bir geliştirici hesabı devre dışı bırakmak için tıklayın **blok**. Engellenen Geliştirici hesabı yeniden etkinleştirmek için tıklayın **etkinleştirme**. Engellenen bir geliştirici hesabını, geliştirici portalına erişmek veya herhangi bir API çağrısı. Bir kullanıcı hesabı silmek için tıklayın **Sil**.

Bir kullanıcıyı engellemek için aşağıdaki adımları izleyin.

1. Seçin **kullanıcılar** ekranın sol tarafındaki için sekmesinde.
2. Engellemek istediğiniz kullanıcıya tıklayın.
3. Tuşuna **blok**.

## <a name="reset-a-user-password"></a>Kullanıcı parolasını sıfırlama

Program aracılığıyla kullanıcı hesapları ile çalışmak için bkz: [kullanıcı varlığı](https://docs.microsoft.com/en-us/rest/api/apimanagement/user) belgelerinde [API Management REST](/rest/api/apimanagement/) başvuru. Belirli bir değere bir kullanıcı hesabı parolasını sıfırlamak için kullanabilirsiniz [kullanıcıyı güncelleştirmek](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-user-entity#UpdateUser) işlemi ve istediğiniz parolayı belirtin.

## <a name="next-steps"> </a>Sonraki adımlar
Bir geliştirici hesabı oluşturulduktan sonra rolleriyle ilişkilendirmek ve ürünlerini ve API'leri için abone olun. Daha fazla bilgi için [grupları oluşturma ve kullanma konusunda][How to create and use groups].

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
