---
title: "Bir Azure aboneliği veya yönetim grubu - Azure Bul | Microsoft Docs"
description: "Yönetim grupları görmek için birden çok dizin ve abonelikler arasında gezinme"
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: f1b9c1ec2af8240ff71f6907516d8894c36ac9c3
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="find-an-azure-subscription-or-management-group"></a>Bir Azure aboneliği veya yönetim grubu bulma

Azure üzerinde bir abonelik veya yönetim grubu bulma ile ilgili sorunlar yaşıyorsanız, yanlış dizinde arama. Birden çok Azure Active dizinlerde hesabınız varsa, bu durum oluşabilir. Her [active directory bağımsız](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-directory-independence) ve dizinlerde erişim devralınan değil.      

![Anahtar dizini menüsü](media/billing-enterprise-mgmt-groups/mgempty.png)



## <a name="switch-directories"></a>Anahtar dizinler 
Azure portalında dizinleri kolayca geçiş yapabilirsiniz.
1.  [Azure portal](https://portal.azure.com) oturum açın.
2.  Üst adınızı seçin ekranın sağ. 
3.  Menünün altındaki erişiminiz tüm dizinleri listeler.
4.  Erişim için bir dizinin adını seçin. 

![Anahtar dizini menüsü](media/billing-enterprise-mgmt-groups/switch-directory.png)

## <a name="asset-is-unavailable"></a>Varlık kullanılamıyor? 
"Bu varlık kullanılamıyor" hata iletisi alıyorsanız, bir abonelik veya yönetim grubu, sonra erişmeye sahip olmadığında bu öğeyi görüntülemek için doğru rol.  

![varlık olmayan bulundu](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Erişim verilecek abonelik veya yönetim gruplarının Yönet başvurun.  
* Abonelikler için başvuru [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) belge rolü gerekli Yardım için.
* Yönetim grupları için RBAC erişim kullanılabilir değil ve yakında çıkıyor. Enterprise portal'ınızın yönetme erişimi atanan kişi.   

## <a name="improve-your-experience-with-management-groups-and-subscriptions-in-the-same-directory"></a>Yönetim grupları ve abonelikleri aynı dizinde deneyiminizi geliştirmek 
Böylece her şeyi aynı konumda bulunan seçtiğiniz dizine aboneliklerinizi veya yönetim gruplarınızı aktarın.  Abonelikler ve Yönetim grupları aynı dizine birleştirerek yardımcı azaltmak dizinleri geçin ve izin vermek için gereken devralınan için ilkeler.  


### <a name="transfer-your-subscriptions"></a>Aboneliklerinizi Aktarım 
Yönetim grupları ile ilişkili dizini için bir abonelik taşıyabilirsiniz. Bu taşıma aboneliğinizi hedef dizinde bir hesaba transfer kayıt yöneticinize sağlanarak elde edilebilir. Daha fazla bilgi için oturum [Enterprise portal](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription).


 






