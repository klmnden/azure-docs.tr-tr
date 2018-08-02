---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 58d81bd4c1ce4b3af91a335039f62df08b340576
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39400000"
---
Yerel olarak yedekli depolama (LRS), en az %99,999999999 sağlamak için tasarlanmıştır (11 9) çoğaltma depolama ölçek birimi içinde verilerinizin belirli bir yıl boyunca nesnelerin dayanıklılık. Bir depolama ölçek birimi, depolama hesabınızı oluşturduğunuz bölge'deki veri merkezinde barındırılır. Tüm çoğaltmalar için verileri yalnızca yazıldıktan sonra bir LRS depolama hesabı için bir yazma isteği başarıyla döndürür. Bu çoğaltmaların her bulunan ayrı hata etki alanları ve güncelleme etki alanları içindeki bir depolama ölçek birimi.

Bir depolama ölçek birimi, raflar depolama düğümünden bir koleksiyonudur. Hata etki alanı (FD), bir fiziksel hata birimini temsil eder ve düğümleri aynı fiziksel dolaba Sürgülü ait olarak kabul edilebilir düğümleri grubudur. Bir yükseltme etki alanını (UD) birlikte (ürün) bir hizmet yükseltme işlemi sırasında yükseltilir düğümleri grubudur. Çoğaltmalar, UD ve Fd'ler bir depolama ölçek birimi içinde yayılır. Bu mimari, verilerinizi tek bir raf üstü bir donanım hatası etkiler veya bir güncelleştirme kullanıma sunulduğunda düğümleri yükselttiğinizde kullanılabilir olmasını sağlar.

LRS, düşük maliyet çoğaltma seçeneği ve diğer seçenekleri karşılaştırıldığında en düşük dayanıklılık sunar. Tüm çoğaltmalar, (örneğin, yangın veya taşmasını)'de bir veri merkezi düzeyinde olağanüstü durum oluşursa, kayıp veya kurtarılamaz olabilir. Bu riski azaltmak için Microsoft bölgesel olarak yedekli depolama (ZRS) veya coğrafi olarak yedekli depolama (GRS) kullanmanızı önerir.

* Uygulamanızın veri kaybı meydana gelirse, kolayca yeniden verilerini depolayan, LRS için kullanmamayı seçebilirsiniz.
* Bazı uygulamalar, veri İdaresi gereksinimleri nedeniyle ülke içinde yalnızca veri çoğaltmak için kısıtlanır. Bazı durumlarda, başka bir ülkede GRS hesapları için çoğaltılan verileri arasında eşleştirilmiş bölgelerin olabilir. Eşleştirilmiş bölgeler hakkında daha fazla bilgi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/).
