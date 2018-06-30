---
title: Bir Azure günlük analizi çalışma alanı Sil | Microsoft Docs
description: Kişisel abonelik oluşturduysanız günlük analizi çalışma alanınız silmeyi öğrenin veya çalışma modelinizi yeniden yapılandırabilirsiniz.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 54f2af60751ed0d9c64e71efad6fa9aa3ef06589
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129124"
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Azure portal ile bir Azure günlük analizi çalışma alanı silme
Bu makalede Azure portalı artık gerektirebilecek günlük analizi çalışma alanını silmek için nasıl kullanılacağı gösterilmektedir. 

## <a name="to-delete-a-workspace"></a>Çalışma alanı silmek için 
Günlük analizi çalışma alanı sildiğinizde, alanınıza ilgili tüm veriler silinir hizmetinden 30 gün içinde.  Olabileceğinden önemli verileri ve hizmet işlemleri olumsuz yönde etkileyebilir yapılandırma, bir çalışma alanı sildiğinizde dikkatli istiyor. Diğer Azure Hizmetleri ve verilerini günlük analizleri gibi depolama kaynakları göz önünde bulundurun:

* Application Insights
* Azure Güvenlik Merkezi
* Azure Otomasyonu
* Windows ve Linux sanal makinelerde çalışan aracıları
* Windows ve Linux bilgisayarlar, ortamınızda çalışan aracıları
* System Center Operations Manager
* Yönetim çözümleri 

Tüm aracıları ve çalışma alanına rapor için yapılandırılan System Center Operations Manager yönetim gruplarına yalnız bırakılmış bir durumda devam eder.  Hangi aracıları, çözümler, stok ve devam etmeden önce diğer Azure hizmetleriyle çalışma alanıyla tümleşiktir.   
 
Bir yöneticiyseniz ve çalışma alanıyla ilişkilendirilmiş birden çok kullanıcı varsa bu kullanıcılar ve çalışma alanı arasındaki ilişki kaybolur. Kullanıcılar başka çalışma alanlarıyla ilişkilendirilmişse bu diğer çalışma alanlarıyla Log Analytics'i kullanmaya devam edebilirler. Diğer çalışma alanları ile ilişkili olmayan, ancak, ardından günlük analizi kullanmak için bir çalışma alanı oluşturmak ihtiyaç duydukları. 

1. [Azure portal](http://portal.azure.com) oturum açın. 
2. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
3. Günlük analizi abonelikleri bölmesinde, bir çalışma alanını seçin ve ardından **silmek** orta bölmesinde üstünden.<br><br> ![Çalışma alanı özellikleri bölmesinden silme seçeneği](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace.png)<br>  
4. Doğrulama ileti penceresinde çalışma alanını silme işlemini onaylamak için tıklatın isteyen görüntülendiğinde **Evet**.<br><br> ![Çalışma alanının Silmeyi Onayla](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace-confirm.png)

