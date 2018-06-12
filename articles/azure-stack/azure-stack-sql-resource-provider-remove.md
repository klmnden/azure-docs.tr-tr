---
title: SQL kaynak sağlayıcısı Azure yığında kaldırma | Microsoft Docs
description: Azure yığın dağıtımınızdan SQL kaynak sağlayıcısı nasıl kaldırabileceğiniz öğrenin.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 9f90201cad0f74923460c2f25eff4de98dc6690a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294789"
---
# <a name="removing-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcısı kaldırılıyor  
SQL kaynak sağlayıcısı kaldırmadan önce ilk bağımlılıkları kaldırmak için gereklidir.

## <a name="remove-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcısını kaldırma 

1. Var olan SQL kaynak sağlayıcısı bağımlılıkları kaldırdınız doğrulayın.

  > [!NOTE]
  > Bağımlı kaynaklar kaynak sağlayıcısı şu anda kullanıyorsanız bile SQL kaynak sağlayıcısını kaldırma devam edecek. 
  
2. SQL kaynak sağlayıcısı bağdaştırıcısı bu sürümü için indirdiğiniz özgün dağıtım paketi olduğundan emin olun.
3. Aşağıdaki parametreleri kullanarak dağıtım betiği yeniden çalıştırın:
    - Kullanın parametre kaldırma
    - IP adresi veya ayrıcalıklı uç noktanın DNS adı.
    - Bulut Yöneticisi, ayrıcalıklı endpoint erişmek için gerekli kimlik bilgileri.
    - Azure yığın hizmet yönetici hesabı için kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerinin aynısını kullanın.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Hizmetleri PaaS teklifi](azure-stack-app-service-overview.md)
