---
title: "Azure Analysis Services yüksek kullanılabilirlik | Microsoft Docs"
description: "Azure Analysis Services yüksek kullanılabilirlik modemlerin."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 84b4c59bac1feeb8611b3a8d783d093ba073e532
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
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

