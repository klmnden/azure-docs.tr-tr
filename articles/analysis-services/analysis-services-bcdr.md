---
title: Azure Analysis Services yüksek kullanılabilirlik | Microsoft Docs
description: Azure Analysis Services yüksek kullanılabilirlik modemlerin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f8252e47224b96d43551ad389148a1ce4349c308
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="analysis-services-high-availability"></a>Analysis Services yüksek kullanılabilirlik
Bu makalede, Azure Analysis Services sunucuları için yüksek kullanılabilirlik modemlerin açıklanmaktadır. 


## <a name="assuring-high-availability-during-a-service-disruption"></a>Yüksek oranda kullanılabilir bir hizmet kesintisi sırasında modemlerin
Ender olsa da, bir Azure veri merkezinde bir kesinti olabilir. Kesinti oluştuğunda birkaç dakika son veya saat boyunca en son bir iş kesintiye neden olur. Yüksek kullanılabilirlik, en sık sunucu artıklık ile elde edilir. Azure Analysis Services ile bir veya daha fazla bölgelerde ek, İkincil sunucuları oluşturarak artıklık elde edebilirsiniz. Veri ve meta verileri bu sunucular üzerinde güvence altına almak için yedek sunucuları oluşturma-sync olduğunda bir bölgede sunucusuyla, çevrimdışı duruma geçti, şunları yapabilirsiniz:

* Modelleri diğer bölgelerdeki yedek sunucular dağıtın. Bu yöntem hem birincil sunucu hem de yedek sunucuları paralel veriler işlenirken gerektirir, tüm sunucuların modemlerin eşitleme.

* [Yedekleme](analysis-services-backup.md) veritabanları birincil sunucu ve yedek sunucuları geri yükleme. Örneğin, Azure depolama gecelik yedekleme otomatikleştirmek ve diğer bölgelerdeki yedekli diğer sunuculara geri yükleyin. 

Her iki durumda da, birincil sunucunuz bir kesinti oluşursa farklı bir bölgesel veri merkezinde sunucusuna bağlanmak için raporlama istemcileri bağlantı dizelerinde değiştirmeniz gerekir. Bu değişiklik son çare değerlendirilmesi ve yalnızca yıkıcı bölgesel veri merkezi kesintisinden oluşur. Ağdaki tüm istemci bağlantıları güncelleştirebilir önce birincil sunucunuzu barındırma veri merkezi kesintisinden tekrar çevrimiçi duruma daha yüksektir. 

Bağlantı dizeleri raporlama istemcilerde değiştirmek zorunda kalmamak için bir sunucu oluşturabilirsiniz [diğer](analysis-services-server-alias.md) birincil sunucunuz için. Birincil sunucu kullanılamaz hale gelirse, başka bir bölgede yedekli sunucusuna işaret etmek için diğer ad değiştirebilirsiniz. Diğer ad, bir uç nokta sistem durumu denetimi birincil sunucudaki kodlayarak sunucu adına otomatik hale getirebilirsiniz. Sistem durumu denetimi başarısız olursa, aynı uç noktası başka bir bölgede yedekli sunucusuna yönlendirebilirsiniz. 

## <a name="related-information"></a>İlgili bilgiler
[Yedekleme ve geri yükleme](analysis-services-backup.md)   
[Azure Analysis Services yönetme](analysis-services-manage.md)   
[Diğer sunucu adları](analysis-services-server-alias.md) 

