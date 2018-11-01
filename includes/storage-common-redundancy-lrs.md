---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/24/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a948a40e638b5f6e042c62ab58c2b7b65a49cd4e
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741989"
---
Yerel olarak yedekli depolama (LRS), en az %99,999999999 (11 dokuzlu) sağlayan belirli bir yıl boyunca nesnelerin dayanıklılık. LRS, verilerinizi depolama Ölçek birimine çoğaltarak bu nesne dayanıklılık sağlar. Depolama hesabınızı oluşturduğunuz bölge içinde bulunan bir veri merkezi depolama ölçek birimi barındırır. Tüm çoğaltmalar için verileri yalnızca yazıldıktan sonra bir LRS depolama hesabı için bir yazma isteği başarıyla döndürür. Her çoğaltma ayrı hata etki alanları ve güncelleme etki alanlarının bir depolama ölçek birimi içinde bulunur.

Bir depolama ölçek birimi, raflar depolama düğümünden bir koleksiyonudur. Hata etki alanı (FD) temsil eden bir fiziksel birimi hata düğümleri grubudur. Hata etki alanı aynı fiziksel dolaba Sürgülü ait düğümleri olarak düşünün. Bir yükseltme etki alanını (UD) birlikte (ürün) bir hizmet yükseltme işlemi sırasında yükseltilir düğümleri grubudur. Çoğaltmalar, UD ve Fd'ler bir depolama ölçek birimi içinde yayılır. Bu mimari, tek bir raf üstü bir donanım hatası etkilerse veya düğüm, hizmet yükseltme sırasında yükseltildiğinde verilerinizin kullanılabilir olmasını sağlar.

LRS, düşük maliyetli çoğaltma seçeneği ve diğer seçenekleri karşılaştırıldığında en düşük dayanıklılık sunar. Tüm çoğaltmalar, (örneğin, yangın veya taşmasını)'de bir veri merkezi düzeyinde olağanüstü durum oluşursa, kayıp veya kurtarılamaz olabilir. Bu riski azaltmak için Microsoft bölgesel olarak yedekli depolama (ZRS) veya coğrafi olarak yedekli depolama (GRS) kullanmanızı önerir.

* Uygulamanızın veri kaybı meydana gelirse, kolayca yeniden verilerini depolayan, LRS için kullanmamayı seçebilirsiniz.
* Bazı uygulamalar, veri İdaresi gereksinimleri nedeniyle ülke içinde yalnızca veri çoğaltmak için kısıtlanır. Bazı durumlarda, başka bir ülkede GRS hesapları için çoğaltılan verileri arasında eşleştirilmiş bölgelerin olabilir. Eşleştirilmiş bölgeler hakkında daha fazla bilgi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/).
