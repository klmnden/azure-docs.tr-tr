---
title: Azure veritabanı geçiş hizmeti önizlemesi genel bakış | Microsoft Docs
description: Azure veri platformlar için birçok veritabanı kaynaktan sorunsuz geçişler sağlayan Azure veritabanı geçiş hizmeti, genel bakış.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.topic: article
ms.date: 04/25/2018
ms.openlocfilehash: b28ea5606e4fae849a2906b0d81a9ed07f265ebf
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="what-is-the-azure-database-migration-service-preview"></a>Azure veritabanı geçiş hizmeti Önizleme nedir?
Azure veritabanı geçiş hizmeti, Azure veri platformları en az kapalı kalma süresi ile birden çok veritabanı kaynakları sorunsuz kümesinden sağlamak için tasarlanmış tam olarak yönetilen bir hizmettir. Hizmet şu anda odaklanan geliştirme çalışmalarını ile genel önizlemede değil:

- Güvenilirlik ve performans.
- Yinelemeli Ayrıca kaynak hedef çifti.
- Uyuşmazlık serbest geçişler sürekli yatırım.

## <a name="use-familiar-tools"></a>Tanıdık araçları kullanın
Azure veritabanı geçiş hizmeti mevcut araçlar ve hizmetlerimizi işlevselliğini bazıları tümleştirir. Bu, müşteri ile kapsamlı ve yüksek oranda kullanılabilir bir çözüm sağlar. Hizmet kullandığı [veri geçiş Yardımcısı](http://aka.ms/dma) bir geçiş işlemi gerçekleştirmeden önce gerekli değişiklikler kılavuzluk için öneri sağlama değerlendirme raporları oluşturmak için. Bu, gerekli herhangi bir düzeltme gerçekleştirmek için hazır. Geçiş işlemine başlamak hazır olduğunuzda, Azure veritabanı geçiş hizmeti ilişkili tüm adımları gerçekleştirir. Yangın ve geçiş projelerinizi işlemi yararlanan Microsoft tarafından belirlendiği şekilde en iyi yöntemlere göz bilmesinin rahatlık unutmayın.

## <a name="regional-availability-during-public-preview"></a>Genel Önizleme sırasında bölgesel kullanılabilirliği
Azure veritabanı geçiş hizmetinin genel Önizleme sürümü aşağıdaki bölgelerde şu anda kullanılabilir:
- Doğu ABD
- Orta ABD
- Orta Güney ABD
- Batı ABD
- Orta Kanada
- Güney Brezilya
- Batı Avrupa
- Kuzey Avrupa
- Güneydoğu Asya
- Batı Hindistan

## <a name="next-steps"></a>Sonraki adımlar
- [Azure portalı kullanarak Azure veritabanı geçiş hizmeti örneğini oluşturmak](quickstart-create-data-migration-service-portal.md).
- [SQL Server Azure SQL veritabanına geçirme](tutorial-sql-server-to-azure-sql.md).
- [Azure veritabanı geçiş hizmeti kullanma önkoşulları genel bakış](pre-reqs.md).
- [Azure veritabanı geçiş hizmeti ile ilgili SSS](faq.md).
