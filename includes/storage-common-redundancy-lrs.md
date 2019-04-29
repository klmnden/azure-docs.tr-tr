---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 02/12/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 32032f729283cb3f2a786412b563fdee88ba4c8a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60729656"
---
Yerel olarak yedekli depolama (LRS), en az %99,999999999 (11 dokuzlu) sağlayan belirli bir yıl boyunca nesnelerin dayanıklılık. LRS, verilerinizi depolama Ölçek birimine çoğaltarak bu nesne dayanıklılık sağlar. Depolama hesabınızı oluşturduğunuz bölge içinde bulunan bir veri merkezi depolama ölçek birimi barındırır. Tüm çoğaltmalar için verileri yalnızca yazıldıktan sonra bir LRS depolama hesabı için bir yazma isteği başarıyla döndürür. Her çoğaltma ayrı hata etki alanları ve yükseltme etki alanlarının bir depolama ölçek birimi içinde bulunur.

Bir depolama ölçek birimi, raflar depolama düğümünden bir koleksiyonudur. Hata etki alanı (FD) temsil eden bir fiziksel birimi hata düğümleri grubudur. Hata etki alanı aynı fiziksel dolaba Sürgülü ait düğümleri olarak düşünün. Bir yükseltme etki alanını (UD) birlikte (ürün) bir hizmet yükseltme işlemi sırasında yükseltilir düğümleri grubudur. Çoğaltmalar, UD ve Fd'ler bir depolama ölçek birimi içinde yayılır. Bu mimari, tek bir raf üstü bir donanım hatası etkilerse veya düğüm, hizmet yükseltme sırasında yükseltildiğinde verilerinizin kullanılabilir olmasını sağlar.

LRS, düşük maliyetli çoğaltma seçeneği ve diğer seçenekleri karşılaştırıldığında en düşük dayanıklılık sunar. Tüm çoğaltmalar, (örneğin, yangın veya taşmasını)'de bir veri merkezi düzeyinde olağanüstü durum oluşursa, kayıp veya kurtarılamaz olabilir. Bu riski azaltmak için Microsoft bölgesel olarak yedekli depolama (ZRS) veya coğrafi olarak yedekli depolama (GRS) kullanmanızı önerir.

* Uygulamanızın veri kaybı meydana gelirse, kolayca yeniden verilerini depolayan, LRS için kullanmamayı seçebilirsiniz.
* Bazı uygulamalar, veri İdaresi gereksinimleri nedeniyle ülke içinde yalnızca veri çoğaltmak için kısıtlanır. Bazı durumlarda, başka bir ülkede GRS hesapları için çoğaltılan verileri arasında eşleştirilmiş bölgelerin olabilir. Eşleştirilmiş bölgeler hakkında daha fazla bilgi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/).
