---
title: Abonelik bulunamadı hatası Azure portalında veya Azure hesap merkezi oturum açmaya çalıştığınızda | Microsoft Docs
description: Hayır abonelik bulunamadı hata oluşur. sorunu için çözüm sağlar Azure portalında veya Azure hesap merkezi ne zaman oturum açın.
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 475a4ad72a1c2fc2ebf99387e193713797cc2586
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34070626"
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a>Abonelik Azure portalında veya Azure hesap merkezi hata bulunamadı

Oturum açmaya çalıştığınızda "abonelik bulunamadı" hata iletisini alabilirsiniz [Azure portal](https://portal.azure.com/) veya [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions). Bu makalede, bu sorun için bir çözüm sağlar.

## <a name="symptom"></a>Belirti

Oturum açmaya çalıştığınızda [Azure portal](https://portal.azure.com/) veya [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions), aşağıdaki hata iletisini alıyorsunuz: "abonelik bulunamadı".

## <a name="cause"></a>Nedeni

Yanlış dizininde seçtiyseniz veya yeterli izinlere hesabınız yoksa, bu sorun oluşur. 

## <a name="solution"></a>Çözüm

### <a name="scenario-1-error-message-is-received-in-the-azure-portalhttpsportalazurecom"></a>Senaryo 1: Hata iletisi alındığında [Azure portalı](https://portal.azure.com)

Bu sorunu gidermek için:

* Hesabınızı sağ üst köşedeki tıklayarak doğru Azure directory seçildiğinden emin olun.

  ![Üst dizini seçin Azure portalının sağ](./media/billing-no-subscriptions-found/directory-switch.png)
* Sağ Azure directory seçilir, ancak hata iletisini almaya devam [sahibi olarak eklenen hesabınızın](billing-add-change-azure-subscription-administrator.md).

### <a name="scenario-2-error-message-is-received-in-the-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a>Senaryo 2: Hata iletisi alındığında [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions)

Kullandığınız hesabın hesap yöneticisi olup olmadığını denetleyin. Hesap Yöneticisi olan doğrulamak için aşağıdaki adımları izleyin:

1. Oturum [abonelikleri görmek Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Denetleyin ve altında aramak istediğiniz aboneliği seçin **ayarları**.
1. Seçin **özellikleri**. Aboneliğin Hesap Yöneticisi görüntülenen **Hesap Yöneticisi** kutusu.  

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma gereksiniminiz varsa [desteğine başvurun](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) hızla çözümlenen sorunu almak için. 
