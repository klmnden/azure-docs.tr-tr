---
title: "Azure Analysis Services yüksek kullanılabilirlik | Microsoft Docs"
description: "Azure Analysis Services yüksek kullanılabilirlik modemlerin."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 554c5e6e3e3cfa2742ef27a3c1510176184b6bd0
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="analysis-services-high-availability"></a>Analysis Services yüksek kullanılabilirlik
Bu makalede, Azure Analysis Services sunucuları için yüksek kullanılabilirlik modemlerin açıklanmaktadır. 


## <a name="assuring-high-availability-during-a-service-disruption"></a>Yüksek oranda kullanılabilir bir hizmet kesintisi sırasında modemlerin
Ender olsa da, bir Azure veri merkezinde bir kesinti olabilir. Kesinti oluştuğunda birkaç dakika son veya saat boyunca en son bir iş kesintiye neden olur. Yüksek kullanılabilirlik, en sık sunucu artıklık ile elde edilir. Azure Analysis Services ile bir veya daha fazla bölgelerde ek, İkincil sunucuları oluşturarak artıklık elde edebilirsiniz. Veri ve meta verileri bu sunucular üzerinde güvence altına almak için yedek sunucuları oluşturma-sync olduğunda bir bölgede sunucusuyla, çevrimdışı duruma geçti, şunları yapabilirsiniz:

* Modelleri diğer bölgelerdeki yedek sunucular dağıtın. Bu yöntem hem birincil sunucu hem de yedek sunucuları paralel veriler işlenirken gerektirir, tüm sunucuların modemlerin eşitleme.

* Veritabanlarını, birincil sunucu ve geri yükleme yedekli sunucularda yedekleyin. Örneğin, Azure depolama gecelik yedekleme otomatikleştirmek ve diğer bölgelerdeki yedekli diğer sunuculara geri yükleyin. 

Her iki durumda da, birincil sunucunuz bir kesinti oluşursa farklı bir bölgesel veri merkezinde sunucusuna bağlanmak için raporlama istemcileri bağlantı dizelerinde değiştirmeniz gerekir. Bu değişiklik son çare değerlendirilmesi ve yalnızca yıkıcı bölgesel veri merkezi kesintisinden oluşur. Ağdaki tüm istemci bağlantıları güncelleştirebilir önce birincil sunucunuzu barındırma veri merkezi kesintisinden tekrar çevrimiçi duruma daha yüksektir. 



## <a name="related-information"></a>İlgili bilgiler
[Yedekleme ve geri yükleme](analysis-services-backup.md)   
[Azure Analysis Services yönetme](analysis-services-manage.md) 

