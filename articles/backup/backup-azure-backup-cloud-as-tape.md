---
title: "Bant altyapınızın yerini alması için Azure Yedekleme'yi | Microsoft Docs"
description: "Yedekleme ve geri yükleme verilerini azure'da olanak veren bant benzeri semantiği Azure Backup'nasıl sağladığını öğrenin"
services: backup
documentationcenter: 
author: trinadhk
manager: vijayts
editor: 
ms.assetid: 2e1bb67d-986c-4437-8056-3a63169b4214
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 1/10/2017
ms.author: saurse;trinadhk;markgal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0f3152daf5f91f7c9e540797bf09b21969d2d33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
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

![Bekletme İlkesi](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

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
