---
title: "Azure yedekleme: İzleyici Azure Yedekleme'de korunan iş yükleri"
description: Azure portalını kullanarak Azure Backup iş yüklerini izleme
services: backup
author: pvrk
manager: shivamg
keywords: Azure yedekleme; Uyarıları;
ms.service: backup
ms.topic: conceptual
ms.date: 03/05/2019
ms.author: pullabhk
ms.assetid: 86ebeb03-f5fa-4794-8a5f-aa5cbbf68a81
ms.openlocfilehash: 8d3e3257f16fe4e0f846c2268bfefc2771387de6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60809034"
---
# <a name="monitoring-azure-backup-workloads"></a>Azure Backup iş yüklerini izleme

Azure Backup, yedekleme gereksinimi ve altyapı topolojiye bağlı (şirket içinde veya Azure) birden çok yedekleme çözümleri sağlar. Herhangi bir yedekleme kullanıcı veya yönetici tüm çözümleri arasında neler olduğunu görmeniz gerekir ve önemli senaryolarda bildirilmesi bekleniyor. Bu makalede Azure Backup hizmeti tarafından sağlanan izleme ve uyarı özellikleri ayrıntılı olarak açıklanmaktadır.

## <a name="backup-jobs-in-recovery-services-vault"></a>Kurtarma Hizmetleri kasasında yedekleme işleri

Azure Backup, yerleşik izleme ve uyarı Azure Backup tarafından korunan iş yükleri için özellikleri sağlar. Kurtarma Hizmetleri kasası ayarları **izleme** bölüm, yerleşik işler ve uyarılar sağlar.

![Yerleşik izleme RS kasası](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltmonitoring.png)

İşler, yedekleme, yedekleme, geri yükleme, silme yedekleme ve benzeri yapılandırma gibi işlemleri gerçekleştirildiğinde oluşturulur.

Aşağıdaki Azure Backup çözümleri işlerden burada gösterilir:

  - Azure VM yedeklemesi
  - Azure dosya yedekleme
  - SQL gibi Azure iş yükü yedekleme
  - Azure Backup Aracısı'nı (MAB)

System Center veri koruma Yöneticisi'nden (SC-DPM), Microsoft Azure Backup sunucusu (MABS) işleri görüntülenmez.

> [!NOTE]
> Azure vm'lerde SQL yedekleme gibi Azure iş yükleri, büyük yedekleme işlerinin sayısını vardır. Örneğin, günlük yedeklerinin her 15 dakikada bir çalıştırabilirsiniz. Bu nedenle, bu tür DB iş yükleri için tetiklenen yalnızca kullanıcı işlemlerini görüntülenir. Zamanlanmış yedekleme işlemleri görüntülenmez.

## <a name="backup-alerts-in-recovery-services-vault"></a>Kurtarma Hizmetleri kasasında yedekleme uyarıları

Uyarılar, öncelikli olarak kullanıcılar, ilgili eylem yararlanabilmeniz burada bildirilir senaryolar verilmiştir. **Yedekleme uyarıları** bölümde, Azure Backup hizmeti tarafından oluşturulan uyarılar gösterilir. Bu uyarılar hizmet tarafından tanımlanan ve kullanıcı özel olamaz herhangi bir uyarı oluştur.

### <a name="alert-scenarios"></a>Uyarı senaryoları
Aşağıdaki senaryolarda alertable senaryoları hizmeti tarafından tanımlanır.

  - Yedekleme/geri yükleme hataları
  - Yedekleme uyarılarla başarılı oldu
  - Veri/durdurmayı silinecek verileri bekleterek korumayı durdurun

### <a name="exceptions-when-an-alert-is-not-raised"></a>Bir uyarı olduğunda özel durumlar
Bir hata durumunda bir uyarı ortaya değil, birkaç özel durum olan, bunlar:

  - Kullanıcı, açıkça çalışan işi iptal edildi
  - Başka bir yedekleme işi (biz yalnızca önceki işin tamamlanmasını beklemek zorunda olduğundan burada yapacak hiçbir şey) sürüyor olduğundan iş başarısız
  - Yedeklenen Azure VM artık mevcut olmadığından sanal makine yedekleme işi başarısız olur.

Yukarıdaki özel durumları (öncelikle tetikleyen kullanıcı) bu işlemlerin sonucu hemen portal/PS/CLI istemcilerde gösterilir anlama gelen tasarlanmıştır. Bu nedenle, kullanıcı hemen farkındadır ve bildirim gerek yoktur.

### <a name="alerts-from-the-following-azure-backup-solutions-are-shown-here"></a>Aşağıdaki Azure Backup çözümlerinden gelen uyarılar burada gösterilir:

  - Azure VM yedeklemeleri
  - Azure dosya yedekleri
  - SQL gibi Azure iş yükü yedekleme
  - Azure Backup Aracısı'nı (MAB)

> [!NOTE]
> System Center veri koruma Yöneticisi'nden (SC-DPM), Microsoft Azure Backup sunucusu (MABS) uyarıları burada görüntülenmez.

### <a name="alert-types"></a>Uyarı türleri
Uyarı önem derecesine göre uyarılar üç tür tanımlanabilir:

  - **Kritik**: İlke, yedekleme veya Kurtarma (zamanlanan veya tetiklenen kullanıcı) hatası için bir uyarı oluşturulmasını sunulmasını ve kritik uyarı ve silme gibi zararlı işlemler olarak yedekleme gösteriliyordu.
  - **Uyarı**: Yedekleme işlemi başarılı olursa ancak birkaç uyarı bunlar uyarı bildirimleri listelenir.
  - **Bilgilendirme**: Bugünden itibaren Azure Backup hizmeti tarafından hiçbir bilgilendirme uyarısı oluşturulur.

## <a name="notification-for-backup-alerts"></a>Bildirim için yedekleme uyarıları

> [!NOTE]
> Bildirim yapılandırması yalnızca Azure portalı üzerinden yapılabilir. PS/CLI/REST API/Azure Resource Manager şablonu destek desteklenmiyor.

Kullanıcılara bir uyarı ortaya bir kez bildirilir. Azure Backup e-posta yoluyla bir yerleşik bildirim mekanizması sağlar. Bireysel e-posta adresleri veya dağıtım listeleri, bir uyarı oluşturulduğunda bildirilmesi için belirtebilirsiniz. Ayrıca, tek tek her uyarı için bildirim almak için veya içinde bir saatlik Özet gruplandırın ve ardından bildirim alın de seçebilirsiniz.

![RS kasa yerleşik e-posta bildirimi](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltnotification.png)

Bildirim yapılandırıldığında, Hoş Geldiniz veya tanıtım e-posta alırsınız. Bu, Azure Backup bir uyarı ortaya çıktığında bu adreslere e-postaları gönderebilir doğrular.<br>

Bir uyarı oluşturulur ve bir saat içinde çözülen sıklığı için bir saatlik Özet ayarlandı ve yaklaşan saatlik Özet bir parçası olmayacaktır.

> [!NOTE]
> 
> * Zararlı gibi **silinecek verileri korumasını Durdur** olduğu gerçekleştirilirse, bir uyarı tetiklenir ve bildirimleri için kurtarma hizmeti yapılandırılmamış olsa bile abonelik sahipleri, yöneticileri ve ortak Yöneticiler bir e-posta gönderilir Kasa.
> * Bildirim başarılı işler kullanmak için yapılandırmak üzere [Log Analytics](backup-azure-monitoring-use-azuremonitor.md#using-log-analytics-workspace).

## <a name="next-steps"></a>Sonraki adımlar

[Azure İzleyicisi'ni kullanarak Azure yedekleme iş yükleri izleme](backup-azure-monitoring-use-azuremonitor.md)
