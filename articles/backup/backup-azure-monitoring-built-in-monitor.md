---
title: "Azure yedekleme: İzleyici Azure Yedekleme'de korunan iş yükleri"
description: Azure portalını kullanarak Azure Backup iş yüklerini izleme
services: backup
author: pvrk
manager: shivamg
keywords: Azure yedekleme; Uyarıları;
ms.service: backup
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: pullabhk
ms.assetid: 86ebeb03-f5fa-4794-8a5f-aa5cbbf68a81
ms.openlocfilehash: 2513f2e7a2e82a541fa5638b70d0543192c84765
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56885357"
---
# <a name="monitoring-azure-backup-workloads"></a>Azure Backup iş yüklerini izleme

Azure Backup, yedekleme gereksinimi ve altyapı topolojiye bağlı (şirket içinde veya Azure) birden çok yedekleme çözümleri sağlar. Tüm yedekleme kullanıcı/yönetici tüm çözümleri arasında neler olduğunu görmeniz gerekir ve önemli senaryolarda bildirilmesi bekleniyor. Bu makalede Azure Backup hizmeti tarafından sağlanan izleme ve uyarı özellikleri ayrıntılı olarak açıklanmaktadır.

## <a name="backup-jobs-in-recovery-services-vault"></a>Kurtarma Hizmetleri kasasında yedekleme işleri

Azure Backup, yerleşik izleme ve uyarı Azure Backup tarafından korunan iş yükleri için özellikleri sağlar. Kurtarma Hizmetleri kasası ayarlarında 'İzleme' bölüm tarafından oluşturulan işler ve uyarılar sağlar.

![Yerleşik izleme RS kasası](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltmonitoring.png)

İşler, yedekleme, yedekleme işlemi, geri yükleme işlemi, yedekleme vb. silme gibi işlemleri gerçekleştirildiğinde oluşturulur.
Aşağıdaki Azure Backup çözümleri işlerden burada gösterilir.

- Azure VM yedeklemeleri
- Azure dosya yedekleri
- SQL gibi Azure iş yükü yedekleme
- Azure Backup Aracısı'nı (MAB)

System Center veri koruma Yöneticisi'nden (SC-DPM), Microsoft Azure Backup sunucusu (MABS) işler burada görüntülenmez.

> [!NOTE]
> Azure vm'lerde SQL yedekleme gibi Azure iş yükleri bakımından yedekleme işlerinin sayısını çok büyük. Örneğin, günlük yedeklerinin her 15 dakikada bir çalıştırabilirsiniz. Bu nedenle, bu tür DB iş yükleri için tetiklenen yalnızca kullanıcı işlemlerini görüntülenir. Zamanlanmış yedekleme işlemleri görüntülenmez.

## <a name="backup-alerts-in-recovery-services-vault"></a>Kurtarma Hizmetleri kasasında yedekleme uyarıları

Uyarılar, öncelikle müşteri ilgili önlem, bildirim almak için gereken yere senaryolar verilmiştir. 'Yedekleme Uyarılar' bölümünde, Azure Backup hizmeti tarafından oluşturulan uyarılar gösterilir. Bu uyarılar hizmet tarafından tanımlanan ve kullanıcı özel olamaz herhangi bir uyarı oluştur. Aşağıdaki senaryolarda alertable senaryoları hizmeti tarafından tanımlanan

- Yedekleme/geri yükleme hataları
- Yedekleme uyarılarla başarılı oldu
- Veri/durdurmayı silinecek verileri bekleterek korumayı durdurun

Bir hata durumunda bir uyarı ortaya değil, birkaç özel durumlar vardır.

- Kullanıcı, açıkça çalışan işi iptal edildi
- Başka bir yedekleme işi (biz yalnızca önceki işin tamamlanmasını beklemek zorunda olduğundan burada yapacak hiçbir şey) sürüyor olduğundan iş başarısız
- Yedeklenen Azure VM artık mevcut olmadığından sanal makine yedekleme işi başarısız olur.

Yukarıdaki özel durumları (öncelikle tetikleyen kullanıcı) bu işlemlerin sonucu hemen portal/PS/CLI istemcilerde gösterilir anlama gelen tasarlanmıştır. Bu nedenle, kullanıcı hemen farkındadır ve bildirim gerek yoktur.

Aşağıdaki Azure Backup çözümlerinden gelen uyarılar burada gösterilir.

- Azure VM yedeklemeleri
- Azure dosya yedekleri
- SQL gibi Azure iş yükü yedekleme
- Azure Backup Aracısı'nı (MAB)

> [!NOTE]
> System Center veri koruma Yöneticisi'nden (SC-DPM), Microsoft Azure Backup sunucusu (MABS) uyarıları burada görüntülenmez.

Uyarı önem derecesine göre uyarılar üç tür tanımlanabilir:

- **Kritik:** İlkesi (zamanlanan veya tetiklenen kullanıcı) yedekleme veya kurtarma herhangi hatası için bir uyarı oluşturulmasını sunulmasını ve kritik uyarı gösteriliyordu. Ayrıca silme yedekleme gibi zararlı işlemleri de kritik uyarılar oluşturur.
- **Uyarı**: Yedekleme işlemi başarılı oldu ancak bazı uyarılar ile uyarı bildirimleri listelenen değilse.
- **Bilgilendirme**: Bugünden itibaren Azure Backup hizmeti tarafından hiçbir bilgilendirme uyarısı oluşturulur.

### <a name="notification-for-backup-alerts"></a>Bildirim için yedekleme uyarıları

> [!NOTE]
> Bildirim yapılandırması yalnızca Azure portalı üzerinden yapılabilir. PS/CLI/REST API/Azure Resource Manager şablonu desteği henüz sağlanmadı.

Bir uyarı ortaya sonra bildirim almak müşteri gerekir. Azure Backup e-posta yoluyla bir yerleşik bildirim mekanizması sağlar. Bireysel e-posta adresleri veya dağıtım listeleri, bir uyarının oluşturulması olduğunda bilgilendirilmeniz için belirtebilirsiniz. Ayrıca, tek tek her uyarı için bildirim almak için veya içinde bir saatlik Özet gruplandırın ve ardından bildirim alın de seçebilirsiniz.

![RS kasa yerleşik e-posta bildirimi](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltnotification.png)

Bildirimleri yapılandırdıktan sonra Azure Backup bir uyarı ortaya çıktığında bu adreslere e-postaları gönderebilir doğrulayan bir Hoş Geldiniz/tanıtım e-posta alırsınız. Sıklığı için bir saatlik Özet ayarlarsanız ve bir uyarı oluşturulur ve aynı saat içinde çözülen yaklaşan saatlik Özet bir parçası olmaz.

> [!NOTE]
> 'Delete veri korumayı durdurun' gibi zararlı bir işlemi gerçekleştirilirse, bir uyarı tetiklenir ve bildirimleri RS kasası için yapılandırılmamış olsa bile abonelik sahipleri, yöneticileri ve ortak Yöneticiler bir e-posta gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure İzleyicisi'ni kullanarak Azure yedekleme iş yükleri izleme](backup-azure-monitoring-use-azuremonitor.md)