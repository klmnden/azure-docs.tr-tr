---
title: Azure SQL Data Warehouse yeni nesil yükseltin | Microsoft Docs
description: Yeni nesil Azure donanım ve depolama mimarisi için Azure SQL veri ambarını yükseltmek için adımlar.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.services: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/02/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 42b716274e655bf91f72c1b3ab207b8a5f1ccee0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="upgrade-to-latest-generation-of-azure-sql-data-warehouse-in-the-azure-portal"></a>Azure SQL Data Warehouse yeni nesil Azure portalında yükseltme

Azure donanım ve depolama mimarisi yeni nesil kullanmak için Azure SQL veri ambarını yükseltmek için Azure portalını kullanın. Yükselterek, daha hızlı performans, daha yüksek ölçeklenebilirlik ve columnstore dizinleri için sınırsız depolama yararlanabilirsiniz.  

## <a name="applies-to"></a>Uygulandığı öğe:
Bu yükseltme esneklik performans katmanı için iyileştirilmiş veri ambarlarında uygular.  Yönergeler için işlem performans katmanı için iyileştirilmiş esneklik performans katmanı için iyileştirilmiş bir veri ambarı yükseltme. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

1. Yükseltilecek esneklik veri ambarı için iyileştirilmiş duraklatıldığında [veri ambarı sürdürmek](pause-and-resume-compute-portal.md).
2. Kapalı kalma süresi birkaç dakika için hazır olun. 
3. Yükseltme işlemi, tüm oturumları sonlandırır ve tüm bağlantıları bırakır. Yükseltmeden önce sorgularınızı tamamladığınızdan emin olun. Yükseltme devam eden işlemleri ile başlatırsanız, geri alma süresi kapsamlı olabilir. 

## <a name="start-the-upgrade"></a>Yükseltme işlemini başlatmak

1. Azure portalında, veri ambarı açın ve tıklayın **'e yükseltmek için iyileştirilmiş işlem**.
2. İşlem performans katmanı seçenekleri için iyileştirilmiş dikkat edin. Varsayılan seçim, yükseltmeden önce geçerli düzeye karşılaştırılabilir.
3. Bir performans katmanı seçin. İşlem performans katmanı için iyileştirilmiş bedelinin Önizleme dönemi boyunca şu anda yarı-kapalıdır.
4. Tıklatın **yükseltme**.
5. Azure portalında durumunu kontrol edin.
6. Veri ambarı çevrimiçi değiştirmek bekleyin.

## <a name="rebuild-columnstore-indexes"></a>Columnstore dizinleri yeniden oluştur

Veri ambarı çevrimiçi olduktan sonra verileri yüklemek ve sorgular çalıştırın. Bununla birlikte, performans olabilir çünkü bir arka plan işlemi verileri yeni donanıma geçirme ilk başta yavaş. 

Olabildiğince çabuk geçirmek için veri zorlamak için columnstore dizinleri yeniden oluşturma öneririz. Bunu yapmak için yönergeler için bkz: [segment kalitesini artırmak için columnstore dizinleri yeniden oluşturma](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality). 

## <a name="next-steps"></a>Sonraki adımlar
Veri ambarınız çevrimiçidir. Yeni performans özellikleri kullanmak için bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md).
 