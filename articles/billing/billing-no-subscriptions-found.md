---
title: Abonelik bulunamadı hatası olduğunda Azure portalında veya Azure hesap merkezi oturum açmayı deneyin | Microsoft Docs
description: Hayır abonelikleri bulunduğu hata oluşursa bir sorun için çözüm sağlar, Azure portalında veya Azure hesap merkezi oturum.
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: cwatson
ms.openlocfilehash: 669e597424ae936dda59564ad072ac7d77107321
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276168"
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a>Azure portalında veya Azure hesap merkezi portalında abonelik hata bulunamadı

Oturum açmaya çalıştığınızda "abonelik bulunamadı" hata iletisini alabilirsiniz [Azure portalında](https://portal.azure.com/) veya [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions). Bu makalede, bu sorun için bir çözüm sağlar.

## <a name="symptom"></a>Belirti

Oturum açmaya çalıştığınızda [Azure portalında](https://portal.azure.com/) veya [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions), aşağıdaki hata iletisini alıyorsunuz: "abonelik bulunamadı".

## <a name="cause"></a>Nedeni

Yanlış dizin seçtiyseniz veya hesabınızın yeterli izinlere sahip değilse bu sorun oluşur. 

## <a name="solution"></a>Çözüm

### <a name="scenario-1-error-message-is-received-in-the-azure-portalhttpsportalazurecom"></a>Senaryo 1: Hata iletisi e-posta [Azure portalı](https://portal.azure.com)

Bu sorunu gidermek için:

* Doğru Azure directory sağ üst köşedeki hesabınızı tıklayarak seçili olduğundan emin olun.

  ![Üst dizini seçin Azure portalının sağ](./media/billing-no-subscriptions-found/directory-switch.png)
* Doğru Azure directory seçilir, ancak hata iletisini almaya devam [hesabınıza sahip rolü atamak](../role-based-access-control/role-assignments-portal.md).

### <a name="scenario-2-error-message-is-received-in-the-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a>Senaryo 2: Hata iletisi e-posta [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions)

Kullandığınız hesabın hesap yöneticisi olup olmadığını denetleyin. Hesap Yöneticisi olan doğrulamak için aşağıdaki adımları izleyin:

1. Oturum [Azure portalında abonelikleri görmek](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Kontrol edin ve ardından altına bakın istediğiniz aboneliği seçin **ayarları**.
1. Seçin **özellikleri**. Aboneliğin Hesap Yöneticisi görüntülenen **Hesap Yöneticisi** kutusu.  

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma ihtiyacınız varsa [desteğe](https://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) sorununuzun hızlıca çözülebilmesi için. 
