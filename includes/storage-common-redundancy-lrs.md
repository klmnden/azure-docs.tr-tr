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
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30928718"
---
Yerel olarak yedekli depolama (LRS), en az %99.999999999 sağlamak için tasarlanmıştır (11 9'in) dayanıklılık verilerinizi bir depolama ölçek birimi içinde çoğaltma tarafından verilen bir yıl içinde nesne. Bir depolama ölçek birimi, depolama hesabınız oluşturuldu bölgede bir veri merkezinde barındırılır. Tüm çoğaltmalar için verileri yalnızca yazıldıktan sonra Yazma isteği LRS depolama hesabı başarıyla döndürür. Her bulunan bu çoğaltmalar hata etki alanlarını ayırmak ve bir depolama ölçek birimi etki alanlarını güncelleştirin.

Bir depolama ölçek birimi depolama düğümleri raflarının koleksiyonudur. Hata etki alanı (FD) hata fiziksel bir birimi temsil eder ve aynı fiziksel raf ait düğümleri olarak kabul düğümlerinin bir gruptur. Bir yükseltme etki alanına (UD), hizmet yükseltmesi (sunum) işlemi sırasında birlikte yükseltilir düğümleri grubudur. Çoğaltmaları UDs ve FDs içinde bir depolama ölçek birimi yayılır. Bu mimari, verilerinizi bir donanım hatası tek bir rafa etkiliyorsa veya düğümler piyasaya sürme sırasında yükseltildiğinde kullanılabilir olmasını sağlar.

LRS, en düşük maliyeti çoğaltma seçeneği ve diğer seçenekleri karşılaştırıldığında en az düzeyde dayanıklılık sunar. (Örneğin, yangın veya taşmasını)'de bir veri merkezi düzeyi olağanüstü durum oluşursa, tüm çoğaltmaları kayıp veya kurtarılamaz olabilir. Bu riski azaltmak için Microsoft bölge olarak yedekli depolama (ZRS) veya coğrafi olarak yedekli depolama (GRS) kullanılmasını önerir.

* Uygulamanızın veri kaybı meydana gelirse, kolayca canlandırılabilir veri depoluyorsa, LRS için tercih edebilirsiniz.
* Bazı uygulamalar, yalnızca veri idare gereksinimleri nedeniyle bir ülke içinde veri çoğaltmak için kısıtlanır. Bazı durumlarda, başka bir ülkede GRS hesapları için çoğaltılan veriler arasında eşleştirilmiş bölgeler olabilir. Eşleştirilmiş bölgeler hakkında daha fazla bilgi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/).
