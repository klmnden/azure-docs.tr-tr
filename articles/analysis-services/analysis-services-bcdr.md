---
title: Azure Analysis Services yüksek kullanılabilirlik | Microsoft Docs
description: Azure Analysis Services yüksek kullanılabilirlik işlemlerini.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 51a0f560a0e4b6ff791d5ed3f9f221eb2eeb9b4d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61036087"
---
# <a name="analysis-services-high-availability"></a>Analysis Services yüksek kullanılabilirlik

Bu makalede, Azure Analysis Services sunucuları için yüksek kullanılabilirlik işlemlerini açıklar. 

## <a name="assuring-high-availability-during-a-service-disruption"></a>Yüksek oranda kullanılabilir bir hizmet kesintisi sırasında işlemlerini

Ender olsa da bir Azure veri merkezinde bir kesinti olabilir. Bir kesinti oluştuğunda, bu işlem birkaç dakika sürebilecek veya saatler alacak neden olur. Yüksek kullanılabilirlik çoğunlukla server yedekliliği ile sağlanır. Azure Analysis Services ile ek, İkincil sunucuları bir veya daha fazla bölgede oluşturarak yedeklilik elde edebilirsiniz. Verileri ve meta verileri bu sunucular üzerinde yapıldığından emin olmak için yedekli sunucuları, oluşturma eşitlenmiş olduğunda bir bölgede sunucusuyla, çevrimdışı duruma geçti, şunları yapabilirsiniz:

* Diğer bölgelerde yedekli sunuculara modelleri dağıtın. Bu yöntem, hem birincil sunucu hem de yedekli sunucular-paralel veri işleme gerektirir, tüm sunucuların işlemlerini eşitlenmiş.

* [Yedekleme](analysis-services-backup.md) veritabanları birincil sunucu ve yedekli sunuculara geri yükleme. Örneğin, Azure depolama gecelik yedeklemelerini otomatikleştirmek ve diğer bölgelerde yedekli diğer sunuculara geri yükleyin. 

Her iki durumda da, birincil sunucu, bir kesinti oluşursa raporlama istemcilerinin farklı bir bölgesel veri merkezi sunucuya bağlanmak için bağlantı dizelerini değiştirmeniz gerekir. Bu değişiklik son çare değerlendirilmesi ve yalnızca bir yıkıcı bölgesel veri merkezi kesintisi durumunda oluşur. Tüm istemcilerin bağlantıları güncelleştirebilir önce birincil sunucusunu barındıran bir veri merkezi arızasına yeniden çevrimiçi gelecektir daha yüksektir. 

Raporlama istemcilerde bağlantı dizelerini değiştirmek zorunda kalmamak için bir sunucu oluşturabilirsiniz [diğer](analysis-services-server-alias.md) birincil sunucunuzun. Birincil sunucunun arıza yaparsa, diğer ad başka bir bölgede yedekli bir sunucuya işaret edecek şekilde değiştirebilirsiniz. Diğer ad, birincil sunucuda bir uç nokta sistem durumu denetimi kodlayarak sunucu adı için otomatik hale getirebilirsiniz. Sistem durumu denetimi başarısız olursa aynı uç noktasına başka bir bölgede yedekli sunucusuna yönlendirebilirsiniz. 

## <a name="related-information"></a>İlgili bilgiler

[Yedekleme ve geri yükleme](analysis-services-backup.md)   
[Azure Analysis Services'ı yönetme](analysis-services-manage.md)   
[Diğer sunucu adları](analysis-services-server-alias.md) 

