---
title: Azure Backup kullanarak bant altyapınızı değiştirme
description: Yedekleme ve geri yükleme azure'da verileri olanak veren bant benzeri semantiği Azure Backup'nasıl sağladığını öğrenin
services: backup
author: trinadhk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 1/10/2017
ms.author: saurse
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 59236774f98af927082c78f4b75a1f5880a7cac4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60646977"
---
# <a name="move-your-long-term-storage-from-tape-to-the-azure-cloud"></a>Uzun vadeli depolama alanınızı banttan Azure bulutuna taşıyın.
Azure yedekleme ve System Center Data Protection Manager müşterileri yapabilirsiniz:

* Kuruluşunuzun gereksinimlerine en uygun tablolarındaki verileri yedekleyin.
* Yedekleme verilerini korumak için uzun süre
* Azure, uzun süreli saklama parçası gerekiyor (bant yerine) olun.

Bu makalede, müşteriler, yedekleme ve bekletme ilkeleri nasıl olanak sağlayabileceğiniz açıklanmaktadır. Adres kendi uzun-vadeli koruma için bantları kullanan müşteriler artık bu özelliğin kullanılabilirliği ile güçlü ve uygun bir alternatif olması. Azure Backup'ın en son sürümde özelliği etkinleştirilir (kullanılabildiği [burada](https://aka.ms/azurebackup_agent)). System Center DPM müşteriler güncelleştirmeniz gerekir için en az DPM 2012 R2 DPM ile Azure Backup hizmeti kullanmadan önce UR5.

## <a name="what-is-the-backup-schedule"></a>Yedekleme zamanlaması nedir?
Yedekleme zamanlaması, Yedekleme işleminin sıklığını gösterir. Örneğin, aşağıdaki ekranda ayarlar, yedeklemeler günlük olarak saat 18: 00 ve gece yarısı alınır gösteriyor.

![Günlük Zamanlama](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Müşteriler ayrıca bir haftalık yedekleme zamanlayabilirsiniz. Örneğin, aşağıdaki ekranda ayarları yedeklemeleri her alternatif Pazar & Çarşamba 09:30:00 ve 01: 00'da alınacağını gösterir.

![Haftalık Zamanlama](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-the-retention-policy"></a>Bekletme İlkesi nedir?
Bekletme İlkesi, yedekleme depolanmalıdır süreyi belirtir. Müşteriler, yalnızca tüm yedekleme noktaları için "sabit ilke" belirtmek yerine, yedek zamanını temel alan farklı bekletme ilkeleri belirtebilirsiniz. Örneğin, bir operasyonel kurtarma noktası olarak hizmet veren, günlük, geçen yedekleme noktası 90 gün boyunca korunur. Denetim amacıyla her bir üç aylık dönemin sonunda alınan yedekleme noktası uzun bir süre için korunur.

![Saklama İlkesi](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

"Bu ilkesinde belirtilen bekletme noktalarını" toplam sayısı: (günlük noktalarına) 90 + 40 (her biri Çeyrek 10 yıl) 130 =.

## <a name="example--putting-both-together"></a>Örneğin, her ikisi de bir araya getirilmesi
![Örnek ekran](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Günlük Bekletme İlkesi**: Günlük gerçekleştirilen yedeklemeleri yedi gün boyunca saklanır.
2. **Haftalık Bekletme İlkesi**: Yedeklemeler her gece yarısı ve 18: 00 Cumartesi gece geçen dört hafta için korunur.
3. **Aylık Bekletme İlkesi**: Gece yarısı ve her ayın son Cumartesi günleri 6 pm alınan yedeklemeler 12 ay boyunca korunur.
4. **Yıllık Bekletme İlkesi**: Her Mart Son Cumartesi gece alınan yedeklemeler 10 yıl boyunca korunur.

"Bekletme noktalarını" toplam sayısı (içinden bir müşteri geri yükleyebilir, veri noktaları) önceki diyagramda şu şekilde hesaplanır:

* iki yedi gün = 14 gün başına kurtarma noktalarını
* iki dört hafta = 8 haftalık kurtarma noktalarını
* iki 12 ay = 24 aylık kurtarma noktalarını
* 10 yıl = 10 kurtarma başına yılda bir işaret

Kurtarma noktalarının toplam sayısı, 56 ' dir.

> [!NOTE]
> Azure yedekleme, Kurtarma noktalarının sayısı üzerinde bir kısıtlama yok.
>
>

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma
Tıklayarak **Değiştir** önceki ekranda, koruma zamanlamalarını belirtilirken daha fazla esneklik imajlarını.

![Değiştir](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a>Sonraki Adımlar
Azure yedekleme hakkında daha fazla bilgi için bkz:

* [Azure Backup'a giriş](backup-introduction-to-azure-backup.md)
* [Azure Backup'ı deneyin](backup-try-azure-backup-in-10-mins.md)
