---
title: "Azure Data Lake Store için sık sorulan sorular | Microsoft Belgeleri"
description: "Azure Data Lake Store ile ilgili sorunları giderme veya azaltma kılavuzu"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf7fd555-3e30-43ce-b28c-c3ad0a241fdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: a3629845014cb401df96d2d8bf7b9801a0664150
ms.openlocfilehash: 2f184f5289b9394572023fe9d1aec2d28a73c4f7


---
# <a name="frequently-asked-questions-for-azure-data-lake-store"></a>Azure Data Lake Store için sık sorulan sorular
Bu makalede Azure Data Lake Store ile ilgili SSS hakkında bilgi edineceksiniz.

## <a name="how-can-i-further-protect-my-data-from-region-wide-disasters-or-accidental-deletions"></a>Verilerimi bölge çapındaki afetlerden veya yanlışlıkla silinmekten nasıl daha iyi koruyabilirim?
Azure Data Lake Store hesabınızdaki veriler, otomatik çoğaltmalar sayesinde bir bölge içindeki geçici donanım hatalarına karşı dayanıklıdır. Dayanıklılık ve yüksek kullanılabilirlik sağlayan bu olanak, Azure Data Lake Store SLA’sını da karşılar. Verilerinizi nadiren gerçekleşen bölge çapında kesintilerden ya da yanlışlıkla silinmekten daha iyi korumanız için bazı yönergeler aşağıda verilmiştir.

### <a name="disaster-recovery-guidance"></a>Olağanüstü durum kurtarma kılavuzu
Her müşterinin kendi olağanüstü durum kurtarma planını hazırlaması kritik öneme sahiptir. Kendi olağanüstü durum kurtarma planınızı oluşturmak için lütfen aşağıdaki Azure belgelerine bakın. Kendi planınızı oluşturmanıza yardımcı olabilecek bazı kaynaklar aşağıda verilmiştir.

* [Azure uygulamaları için olağanüstü durum kurtarma ve yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Azure dayanıklılık teknik kılavuzu](../resiliency/resiliency-technical-guidance.md)

#### <a name="best-practices"></a>En iyi uygulamalar
Kritik verilerinizi kendi olağanüstü durum kurtarma planınızın gereksinimlerine uygun bir sıklıkta başka bir bölgede bulunan başka bir Data Lake Store hesabına kopyalamanızı öneririz. Verileri kopyalamak için [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) veya [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) dahil olmak üzere çeşitli yöntemler mevcuttur. Azure Data Factory, yinelenen bir düzende veri taşıma işlem hatları oluşturmak ve dağıtmak için kullanışlı bir hizmettir.

Bölgesel bir kesinti gerçekleşmesi durumunda, verilerinizi kopyaladığınız bölgede verilerinize erişebilirsiniz. Küresel olarak Azure hizmet durumunu öğrenmek için [Azure Hizmet Durumu Panosu](https://azure.microsoft.com/status/)’nu kullanabilirsiniz.

### <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Verilerin bozulması veya yanlışlıkla silinmesi durumunda kurtarma kılavuzu
Azure Data Lake Store otomatik çoğaltmalar aracılığıyla veri dayanıklılığı sağlasa da bu önlem uygulamanızın (veya geliştiricilerin/kullanıcıların) verileri bozmasına ya da yanlışlıkla silmesine engel olmaz.

#### <a name="best-practices"></a>En iyi uygulamalar
Verilerin yanlışlıkla silinmesini engellemek için önce Data Lake Store hesabınız için doğru erişim ilkelerini ayarlamanızı öneririz.  Buna önemli kaynakları kilitlemek için [Azure kaynak kilitlerinin](../azure-resource-manager/resource-group-lock-resources.md) uygulanmasının yanı sıra kullanılabilir [Data Lake Store güvenlik özellikleri](data-lake-store-security-overview.md) kullanılarak hesap ve dosya düzeyinde erişimin uygulanması dahildir. Ayrıca, [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) veya [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)’yi kullanarak başka bir Data Lake Store hesabında, klasörde veya Azure aboneliğinde düzenli olarak kritik verilerinizin kopyalarını oluşturmanızı da öneririz.  Bu yöntem, verilerin bozulması veya silinmesi durumunda kurtarılması için de kullanılabilir. Azure Data Factory, yinelenen bir düzende veri taşıma işlem hatları oluşturmak ve dağıtmak için kullanışlı bir hizmettir.

Kurumlar, bir dosyanın kim tarafından silinmiş veya güncelleştirilmiş olabileceği hakkında bilgi sağlayan veri erişimi denetim kayıtlarını toplamak amacıyla Azure Data Lake Store hesapları için [tanılama günlük kaydını](data-lake-store-diagnostic-logs.md) da etkinleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Store ile Çalışmaya Başlama](data-lake-store-get-started-portal.md)
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)




<!--HONumber=Feb17_HO2-->


