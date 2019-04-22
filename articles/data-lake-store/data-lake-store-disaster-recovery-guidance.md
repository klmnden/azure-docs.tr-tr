---
title: Azure Data Lake depolama Gen1 için olağanüstü durum kurtarma Kılavuzu | Microsoft Docs
description: Azure Data Lake depolama Gen1 için olağanüstü durum kurtarma Kılavuzu
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: twooley
ms.openlocfilehash: b3f1888a73baf2b7f9efa9f5e7cdb3305aa9f90d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58878120"
---
# <a name="disaster-recovery-guidance-for-data-in-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 veriler için olağanüstü durum kurtarma Kılavuzu

Azure Data Lake depolama Gen1 yerel olarak yedekli depolama (LRS) sağlar. Bu nedenle, Data Lake depolama Gen1 hesabınızda otomatik çoğaltmalar sayesinde bir veri merkezinde geçici donanım hatalarına dayanıklı verilerdir. Bu, dayanıklılık ve yüksek kullanılabilirlik, Data Lake depolama Gen1 SLA'sı toplantı sağlar. Bu makalede, verilerinizi nadir bölge çapında kesintilerden ya da yanlışlıkla silinmekten daha iyi korumak nasıl hakkında yönergeler sağlanır.

## <a name="disaster-recovery-guidance"></a>Olağanüstü durum kurtarma kılavuzu
Her müşterinin kendi olağanüstü durum kurtarma planını hazırlaması kritik öneme sahiptir. Olağanüstü durum kurtarma planınızı oluşturmak için bu makaledeki bilgileri okuyun. Kendi planınızı oluşturmanıza yardımcı olabilecek bazı kaynaklar aşağıda verilmiştir.

* [Azure uygulamaları için olağanüstü durum kurtarma ve yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Azure dayanıklılık teknik kılavuzu](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practices"></a>En iyi uygulamalar
Kritik verilerinizi başka bir bölgede başka bir Data Lake depolama Gen1 hesabına ile olağanüstü durum kurtarma planınızın gereksinimlerine uygun bir sıklıkta kopyalamanız önerilir. Verileri kopyalamak için [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) veya [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) dahil olmak üzere çeşitli yöntemler mevcuttur. Azure Data Factory, yinelenen bir düzende veri taşıma işlem hatları oluşturmak ve dağıtmak için kullanışlı bir hizmettir.

Bölgesel bir kesinti oluşursa verilerin kopyalandığı bölgede daha sonra erişebilirsiniz. İzleyeceğiniz [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/) dünyanın dört bir yanındaki Azure hizmet durumunu belirlemek için.

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Verilerin bozulması veya yanlışlıkla silinmesi durumunda kurtarma kılavuzu
Data Lake depolama Gen1 otomatik çoğaltmalar aracılığıyla veri dayanıklılığı sağlasa da bu, uygulama (veya geliştiricilerin/kullanıcıların) verileri bozmasına ya da yanlışlıkla silmesine engel olmaz.

### <a name="best-practices"></a>En iyi uygulamalar
VHD'nin yanlışlıkla silinmesini engellemek için Data Lake depolama Gen1 hesabınız için bir doğru erişim ilkelerini ayarlamanızı öneririz.  Bu, uygulama içerir [Azure kaynak kilitlerinin](../azure-resource-manager/resource-group-lock-resources.md) kullanılabilir kullanarak olarak uygulanan hesap ve dosya düzeyinde erişim denetimini de önemli kaynakları kilitlemek için [Data Lake depolama Gen1 güvenlik özellikleri](data-lake-store-security-overview.md). Ayrıca kopyalarını kullanarak önemli verileri düzenli olarak oluşturmanızı öneririz [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) veya [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) başka bir Data Lake depolama Gen1 içinde Hesap, klasör veya Azure aboneliği.  Bu yöntem, verilerin bozulması veya silinmesi durumunda kurtarılması için de kullanılabilir. Azure Data Factory, yinelenen bir düzende veri taşıma işlem hatları oluşturmak ve dağıtmak için kullanışlı bir hizmettir.

Kuruluşlar da etkinleştirebilir [tanılama günlüğüne kaydetme](data-lake-store-diagnostic-logs.md) kimin silinmiş veya güncelleştirilmiş bir dosya hakkında bilgi sağlayan veri erişimi denetim izleri toplamak Data Lake depolama Gen1 hesabı.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)

