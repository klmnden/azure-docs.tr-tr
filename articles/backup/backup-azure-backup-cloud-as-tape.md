---
title: Azure Backup kullanarak bant altyapınızı değiştirme
description: Yedekleme ve geri yükleme verilerini azure'da olanak veren bant benzeri semantiği Azure Backup'nasıl sağladığını öğrenin
services: backup
author: trinadhk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 1/10/2017
ms.author: saurse
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff30dd0e4c7cadabddbeddc38c28a773db68d8ff
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606503"
---
# <a name="move-your-long-term-storage-from-tape-to-the-azure-cloud"></a>Uzun vadeli depolama için Azure bulut banttan taşıma
Azure yedekleme ve System Center Data Protection Manager müşteriler yapabilirsiniz:

* Kuruluşunuzun gereksinimlerine en uygun tablolarındaki verileri yedekleyin.
* Uzun süre yedekleme verilerini korur
* (Bant yerine) kendi uzun vadeli bekletme parçası gereken Azure olun.

Bu makalede, müşteriler yedekleme ve bekletme ilkeleri nasıl etkinleştirebilirsiniz açıklanmaktadır. Kendi uzun-süreli saklama adres için bantları kullanacak müşterilerin, artık bu özelliğin kullanılabilirliği ile güçlü ve uygun bir alternatif sahip. En son sürümünü Azure yedekleme özelliği etkinleştirilmişse (kullanılabilen [burada](http://aka.ms/azurebackup_agent)). System Center DPM müşteriler şekilde güncelleştirilmesi gerekir, en az DPM 2012 R2 DPM Azure Backup hizmeti ile kullanmadan önce UR5.

## <a name="what-is-the-backup-schedule"></a>Yedekleme zamanlaması nedir?
Yedekleme zamanlaması, yedekleme işlemi sıklığını belirtir. Örneğin, aşağıdaki ekran ayarlarında yedeklemeleri 6 pm saatinde ve gece alınır gösterir.

![Günlük Zamanlama](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Müşterileri de haftalık bir yedekleme zamanlayabilirsiniz. Örneğin, aşağıdaki ekran ayarlarında yedeklemeleri her alternatif Pazar & Çarşamba 09:30:00 ve 1: 00'da alınacağını belirtin.

![Haftalık Zamanlama](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-the-retention-policy"></a>Bekletme İlkesi nedir?
Bekletme İlkesi yedeğin depolanması gereken süreyi belirtir. Müşteriler, yalnızca tüm yedekleme noktaları için "sabit ilke" belirtmek yerine, yedeklemenin ne zamana göre farklı bekletme ilkeleri belirtebilirsiniz. Örneğin, bir işletimsel kurtarma noktası olarak hizmet veren, günlük, geçen yedekleme noktası 90 gün boyunca korunur. Denetim amacıyla her üç aylık dönemin sonunda alınan yedekleme noktası uzun bir süre için korunur.

![Saklama İlkesi](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

"Bu ilkede belirtilen bekletme noktalarını" toplam sayısı olan 90 (günlük noktaları) + 40 (her biri 10 Yıl Çeyrek) 130 =.

## <a name="example--putting-both-together"></a>Örnek – hem de bir araya getirme
![Örnek ekran](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Günlük Bekletme İlkesi**: günlük alınan yedeklemeler yedi gün boyunca saklanır.
2. **Haftalık Bekletme İlkesi**: her gün gece ve 6 PM Cumartesi alınan yedeklemeler için dört hafta korunur
3. **Aylık Bekletme İlkesi**: Gece ve her ayın son Cumartesi 6 pm alınan yedeklemeler için 12 ay boyunca korunur
4. **Yıllık Bekletme İlkesi**: her Mart Son Cumartesi gece alınan yedeklemeler, 10 yılı aşkın korunur

"Bekletme noktalarını" toplam sayısı (içinden bir müşteri geri yükleyebilir, veri noktaları) önceki diyagramda aşağıdaki gibi hesaplanır:

* iki yedi gün = 14 gün başına kurtarma noktalarını
* iki haftada bir dört hafta = 8 için kurtarma noktalarını
* iki aylık 12 ay = 24 için kurtarma noktalarını
* bir işaret 10 yıl = 10 kurtarma başına yıllık

Toplam kurtarma noktaları 56 sayısıdır.

> [!NOTE]
> Azure yedekleme kurtarma noktalarının sayısı üzerinde bir kısıtlama yoktur.
>
>

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma
Tıklayarak **Değiştir** önceki ekranında müşteriler bekletme zamanlamaları belirterek daha fazla esneklik bulunur.

![Değiştir](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a>Sonraki Adımlar
Azure yedekleme hakkında daha fazla bilgi için bkz:

* [Azure Backup'a giriş](backup-introduction-to-azure-backup.md)
* [Azure Backup'ı deneyin](backup-try-azure-backup-in-10-mins.md)
