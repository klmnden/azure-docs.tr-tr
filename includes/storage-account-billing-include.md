---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/19/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ed7bfca6095dbb03042efd14456f34556f74a843
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66115663"
---
Azure depolama, depolama hesabı kullanımınıza göre faturalandırılırsınız. Depolama hesabındaki tüm nesneler bir grup halinde faturalandırılır. 

Depolama maliyetleri şu etkenlere göre hesaplanır: bölge/konum, hesap türü, erişim katmanı, depolama kapasitesi, çoğaltma düzeni, depolama işlemleri ve veri çıkışı.

* **Bölge** hesabınızı dayanır coğrafi bölgeye başvurur.
* **Hesap türü** kullandığınız depolama hesabı türünü belirtir. 
* **Erişim katmanı** genel amaçlı v2 veya Blob Depolama hesabı için belirttiğiniz veri kullanım deseni ifade eder.
* Depolama **kapasite** ne kadar depolama hesabı payınızın olduğunuz anlamına gelir verilerini depolamak için kullanma.
* **Çoğaltma** tek seferde ve konumu, verilerinizin kaç kopyasının tutulduğu belirtir.
* **İşlem** tüm okuma ve yazma işlemleri Azure depolama birimine başvurun.
* **Veri çıkışı** bir Azure bölgesinin dışına aktarılan tüm verileri ifade eder. Depolama hesabınızdaki verilere aynı bölgede çalıştırılmayan bir uygulama eriştiğinde veri çıkışı için ücretlendirilirsiniz. Veri ve hizmetlerinizi aynı bölgede çıkış ücretlerini sınırlamak için Grup için kaynak gruplarını kullanma hakkında daha fazla bilgi için bkz: [bir Azure kaynak grubu nedir?](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group). 

[Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası hesap türüne, depolama kapasitesine, çoğaltmaya ve işlemlere bağlı olarak ayrıntılı fiyatladırma bilgileri sağlar. [Veri Aktarımları Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/data-transfers/), veri çıkışı için ayrıntılı fiyatlandırma bilgileri sağlar. Maliyetlerinizi tahmin etmeye yardımcı olması için [Azure Storage Fiyatlandırması Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?scenario=data-management)’nı kullanabilirsiniz.

