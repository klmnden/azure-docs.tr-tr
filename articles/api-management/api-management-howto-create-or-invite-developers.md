---
title: "Nasıl kullanıcı hesaplarını Azure API Management'te yönetme | Microsoft Docs"
description: "Oluşturma ve Azure API Management'te kullanıcıları davet öğrenin"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 078abfa5-1e4f-4c9d-b9c7-a172bd19c1a2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: d3a50f6d22cbf1797f580078bc0d2cc9cefe5064
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Kullanıcı hesapları Azure API Management'te yönetme
API Yönetimi'nde, geliştiriciler API Management kullanarak kullanıma API'leri kullanıcılardır. Bu kılavuz, API'ları ve ürünlerini kullanmak için için nasıl oluşturulacağını ve geliştiricilerin davet gösterir, API Management örneği ile kendileri için kullanılabilir hale. Kullanıcı hesaplarını program aracılığıyla yönetme hakkında daha fazla bilgi için bkz: [kullanıcı varlığı](https://msdn.microsoft.com/library/azure/dn776330.aspx) belgelerinde [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) başvuru.

## <a name="create-developer"></a>Yeni bir geliştirici oluşturma
Yeni bir geliştirici oluşturmak için tıklatın **yayımcı portalına** API Management hizmetiniz için Azure Portalı'nda. Bu sizi API Management yayımcı portalına götürür. Henüz bir API Management hizmeti örneği oluşturmadıysanız, [Azure API Management'i kullanmaya başlama][Get started with Azure API Management] öğreticisinde [API Management hizmet örneği oluşturma][Create an API Management service instance]'ya bakın.

![Yayımcı portalı][api-management-management-console]

Tıklatın **kullanıcılar** gelen **API Management** sol menüsünde ve ardından **kullanıcı ekleme**.

![Geliştirici oluşturma][api-management-create-developer]

Girin **e-posta**, **parola**, ve **adı** tıklatın ve yeni geliştirici için **kaydetmek**.

![Geliştirici oluşturma][api-management-add-new-user]

Varsayılan olarak, yeni oluşturulan Geliştirici hesaplardır **etkin**ve ilişkili **geliştiriciler** grubu.

![Yeni Geliştirici][api-management-new-developer]

Geliştirici hesaplarının bir **etkin** durumu, tüm abonelikleri sahiptirler API'leri erişmek için kullanılabilir. Yeni oluşturulan Geliştirici ek gruplarıyla ilişkilendirmek için bkz: [grupları geliştiricilerle ilişkilendirme][How to associate groups with developers].

## <a name="invite-developer"></a>Geliştirici davet et
Bir geliştirici davet etmek için tıklatın **kullanıcılar** gelen **API Management** sol menüsünde ve ardından **davet kullanıcı**.

![Geliştirici davet et][api-management-invite-developer]

Geliştirici adını ve e-posta adresini girin ve tıklayın **davet**.

![Geliştirici davet et][api-management-invite-developer-window]

Bir onay iletisi görüntülenir, fakat daveti kabul ettikten sonra yeni davet edilen Geliştirici kadar listesinde görünmez. 

![Onay davet et][api-management-invite-developer-confirmation]

Bir geliştirici davet, geliştiriciler için bir e-posta gönderilir. Bu e-posta şablonu kullanılarak oluşturulan ve özelleştirilebilir. Daha fazla bilgi için bkz: [yapılandırma e-posta şablonlarını][Configure email templates].

Daveti kabul edildikten sonra hesap etkin hale gelir.

## <a name="block-developer"></a> Devre dışı bırakın veya bir geliştirici hesabını yeniden etkinleştirme
Varsayılan olarak, yeni oluşturulan veya davet edilen Geliştirici hesaplardır **etkin**. Bir geliştirici hesabını devre dışı bırakmak için tıklatın **blok**. Engellenen Geliştirici hesabı yeniden etkinleştirmek için tıklatın **etkinleştirme**. Engellenen Geliştirici hesabını değil Geliştirici portalına erişmek veya tüm API'leri çağırın. Bir kullanıcı hesabını silmek için tıklatın **silmek**.

![Blok Geliştirici][api-management-new-developer]

## <a name="reset-a-user-password"></a>Kullanıcı parolasını sıfırlama
Bir kullanıcı hesabının parolasını sıfırlamak için hesap adına tıklayın.

![Parola sıfırlama][api-management-view-developer]

Tıklatın **parola sıfırlama** kullanıcının parolasını sıfırlamak için bir bağlantı göndermek için.

![Parola sıfırlama][api-management-reset-password]

Program aracılığıyla kullanıcı hesapları ile çalışmak için bkz: [kullanıcı varlığı](https://msdn.microsoft.com/library/azure/dn776330.aspx) belgelerinde [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) başvuru. Belirli bir değere bir kullanıcı hesabı parolasını sıfırlamak için kullanabileceğiniz [kullanıcıyı güncelleştirmek](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) işlemi ve istenilen parola belirtin.

## <a name="pending-verification"></a>Bekleyen doğrulama
![Bekleyen doğrulama][api-management-pending-verification]

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

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
