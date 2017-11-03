---
title: "Bir Azure günlük analizi çalışma alanı Sil | Microsoft Docs"
description: "Kişisel abonelik oluşturduysanız günlük analizi çalışma alanınız silmeyi öğrenin veya çalışma modelinizi yeniden yapılandırabilirsiniz."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 3d99f9869dfdfbe18f82e873bd367cc85f9414f1
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Azure portal ile bir Azure günlük analizi çalışma alanı silme
Bu konu Azure portalı artık gerektirebilir günlük analizi workpace silmek için nasıl kullanılacağını gösterir. 

## <a name="to-delete-a-workspace"></a>Çalışma alanı silmek için 
Günlük analizi çalışma alanı sildiğinizde, alanınıza ilgili tüm veriler silinir hizmetinden 30 gün içinde.  Olabileceğinden önemli verileri ve hizmet işlemleri olumsuz yönde etkileyebilir yapılandırma, bir çalışma alanı sildiğinizde dikkatli istiyor.  
 
Bir yöneticiyseniz ve çalışma alanıyla ilişkilendirilmiş birden çok kullanıcı varsa bu kullanıcılar ve çalışma alanı arasındaki ilişki kaybolur. Kullanıcılar diğer çalışma alanları ile ilişkili ise, ardından bunlar günlük analizi bu diğer çalışma alanları ile kullanmaya devam edebilirsiniz. Diğer çalışma alanları ile ilişkili olmayan, ancak, ardından günlük analizi kullanmak için bir çalışma alanı oluşturmak ihtiyaç duydukları. 

1. [Azure portal](http://portal.azure.com) oturum açın. 
2. Azure portalında tıklatın **daha fazla hizmet** sol alt köşesindeki üzerinde bulunamadı. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **oturum Analytics**.
3. Günlük analizi abonelikleri bölmesinde, bir çalışma alanını seçin ve ardından **silmek** orta bölmesinde üstünden.<br><br> ![Çalışma alanı özellikleri bölmesinden silme seçeneği](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace.png)<br>  
4. Doğrulama ileti penceresinde çalışma alanını silme işlemini onaylamak için tıklatın isteyen görüntülendiğinde **Evet**.<br><br> ![Çalışma alanının Silmeyi Onayla](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace-confirm.png)

