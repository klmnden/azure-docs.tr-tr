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
ms.openlocfilehash: 4f455e26f078360f17f8118f8b5b1db5bd75d473
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Azure portal ile bir Azure günlük analizi çalışma alanı silme
Bu konu Azure portalı artık gerektirebilir günlük analizi workpace silmek için nasıl kullanılacağını gösterir. 

## <a name="to-delete-a-workspace"></a>Çalışma alanı silmek için 
Günlük analizi çalışma alanı sildiğinizde, alanınıza ilgili tüm veriler silinir hizmetinden 30 gün içinde.  Olabileceğinden önemli verileri ve hizmet işlemleri olumsuz yönde etkileyebilir yapılandırma, bir çalışma alanı sildiğinizde dikkatli istiyor.  
 
Bir yöneticiyseniz ve çalışma alanıyla ilişkilendirilmiş birden çok kullanıcı varsa bu kullanıcılar ve çalışma alanı arasındaki ilişki kaybolur. Kullanıcılar diğer çalışma alanları ile ilişkili ise, ardından bunlar günlük analizi bu diğer çalışma alanları ile kullanmaya devam edebilirsiniz. Diğer çalışma alanları ile ilişkili olmayan, ancak, ardından günlük analizi kullanmak için bir çalışma alanı oluşturmak ihtiyaç duydukları. 

1. [Azure portal](http://portal.azure.com) oturum açın. 
2. Azure portalında tıklatın **tüm hizmetleri**. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
3. Günlük analizi abonelikleri bölmesinde, bir çalışma alanını seçin ve ardından **silmek** orta bölmesinde üstünden.<br><br> ![Çalışma alanı özellikleri bölmesinden silme seçeneği](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace.png)<br>  
4. Doğrulama ileti penceresinde çalışma alanını silme işlemini onaylamak için tıklatın isteyen görüntülendiğinde **Evet**.<br><br> ![Çalışma alanının Silmeyi Onayla](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace-confirm.png)

