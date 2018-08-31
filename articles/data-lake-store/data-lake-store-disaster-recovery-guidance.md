---
title: Azure Data Lake Store için olağanüstü durum kurtarma Kılavuzu | Microsoft Docs
description: Azure Data Lake Store için olağanüstü durum kurtarma Kılavuzu
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: b51f0c1e0c6ef713bf8d3ff0a124300f446a9373
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306817"
---
# <a name="disaster-recovery-guidance-for-data-in-data-lake-store"></a>Data Lake Store, veriler için olağanüstü durum kurtarma Kılavuzu

Azure Data Lake Store, yerel olarak yedekli depolama (LRS) sağlar. Bu nedenle, hesabınızdaki veriler, Azure Data Lake Store otomatik çoğaltmalar sayesinde bir veri merkezinde geçici donanım hatalarına dayanıklı. Dayanıklılık ve yüksek kullanılabilirlik sağlayan bu olanak, Azure Data Lake Store SLA’sını da karşılar. Bu makalede, verilerinizi nadir bölge çapında kesintilerden ya da yanlışlıkla silinmekten daha iyi korumak nasıl hakkında yönergeler sağlanır.

## <a name="disaster-recovery-guidance"></a>Olağanüstü durum kurtarma kılavuzu
Her müşterinin kendi olağanüstü durum kurtarma planını hazırlaması kritik öneme sahiptir. Olağanüstü durum kurtarma planınızı oluşturmak için bu makaledeki bilgileri okuyun. Kendi planınızı oluşturmanıza yardımcı olabilecek bazı kaynaklar aşağıda verilmiştir.

* [Azure uygulamaları için olağanüstü durum kurtarma ve yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Azure dayanıklılık teknik kılavuzu](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practices"></a>En iyi uygulamalar
Kritik verilerinizi kendi olağanüstü durum kurtarma planınızın gereksinimlerine uygun bir sıklıkta başka bir bölgede bulunan başka bir Data Lake Store hesabına kopyalamanızı öneririz. Verileri kopyalamak için [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) veya [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) dahil olmak üzere çeşitli yöntemler mevcuttur. Azure Data Factory, yinelenen bir düzende veri taşıma işlem hatları oluşturmak ve dağıtmak için kullanışlı bir hizmettir.

Bölgesel bir kesinti gerçekleşmesi durumunda, verilerinizi kopyaladığınız bölgede verilerinize erişebilirsiniz. Küresel olarak Azure hizmet durumunu öğrenmek için [Azure Hizmet Durumu Panosu](https://azure.microsoft.com/status/)’nu kullanabilirsiniz.

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Verilerin bozulması veya yanlışlıkla silinmesi durumunda kurtarma kılavuzu
Azure Data Lake Store otomatik çoğaltmalar aracılığıyla veri dayanıklılığı sağlasa da bu önlem uygulamanızın (veya geliştiricilerin/kullanıcıların) verileri bozmasına ya da yanlışlıkla silmesine engel olmaz.

### <a name="best-practices"></a>En iyi uygulamalar
Verilerin yanlışlıkla silinmesini engellemek için önce Data Lake Store hesabınız için doğru erişim ilkelerini ayarlamanızı öneririz.  Buna önemli kaynakları kilitlemek için [Azure kaynak kilitlerinin](../azure-resource-manager/resource-group-lock-resources.md) uygulanmasının yanı sıra kullanılabilir [Data Lake Store güvenlik özellikleri](data-lake-store-security-overview.md) kullanılarak hesap ve dosya düzeyinde erişimin uygulanması dahildir. Ayrıca, [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) veya [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)’yi kullanarak başka bir Data Lake Store hesabında, klasörde veya Azure aboneliğinde düzenli olarak kritik verilerinizin kopyalarını oluşturmanızı da öneririz.  Bu yöntem, verilerin bozulması veya silinmesi durumunda kurtarılması için de kullanılabilir. Azure Data Factory, yinelenen bir düzende veri taşıma işlem hatları oluşturmak ve dağıtmak için kullanışlı bir hizmettir.

Kurumlar, bir dosyanın kim tarafından silinmiş veya güncelleştirilmiş olabileceği hakkında bilgi sağlayan veri erişimi denetim kayıtlarını toplamak amacıyla Azure Data Lake Store hesapları için [tanılama günlük kaydını](data-lake-store-diagnostic-logs.md) da etkinleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Store ile Çalışmaya Başlama](data-lake-store-get-started-portal.md)
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)

