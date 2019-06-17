---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: beb08c29587e4ce522131142fd61925b5af45fa9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66114552"
---
Azure portalı veya SMB kullanarak Azure dosya paylaşımına yapılan değişiklikler hemen algılandı ve sunucu uç noktası değişiklikleri gibi çoğaltılır. Azure dosyaları henüz sahip değil değişiklik bildirimleri veya günlüğe kaydetme, vardır, bu nedenle dosyaları değiştiği bir eşitleme oturumu otomatik olarak başlatmak için bir yolu yoktur. Windows Server, Azure dosya eşitleme kullanan [Windows USN günlüğü](https://msdn.microsoft.com/library/windows/desktop/aa363798.aspx) dosyaları değiştiğinde bir eşitleme oturumu otomatik olarak başlatmak için.<br /><br /> Azure dosya paylaşımına değişikliklerini algılamak için Azure dosya eşitleme adında bir zamanlanmış iş sahip bir *algılama işi değiştirmek*. Bir değişiklik algılama iş dosya paylaşımındaki her dosyanın numaralandırır ve söz konusu dosya için eşitleme sürümle karşılaştırır. Değişiklik algılama işi dosyaların değiştiğini belirlediğinde, Azure dosya eşitleme eşitleme oturumu başlatır. Değişiklik algılama işin her 24 saatte başlatılır. Her bir dosyanın Azure dosya paylaşımının numaralandırarak değişiklik algılama işi çalıştığı için daha büyük ad alanlarında daha küçük ad alanlarında değişiklik algılama, daha uzun sürer. Büyük ad alanları için 24 hangi dosyaların değiştiğini belirlemek için saatte uzun sürebilir.<br /><br />
Not, REST kullanarak Azure dosya paylaşımı için yapılan değişiklikler, SMB son değiştirme zamanı güncelleştirmesi yapar ve bir değişiklik eşitleme tarafından görülmez. <br /><br />
Windows Server'da biz birimleri için USN benzer bir Azure dosya paylaşımı için ekleme değişiklik algılama aramaktadır. Gelecekteki geliştirme için bu özellik sayfasından için oy vermeyle belirlememize yardımcı [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).
