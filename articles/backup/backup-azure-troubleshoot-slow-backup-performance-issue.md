---
title: "Dosya ve klasörleri Azure Yedekleme'de yavaş yedeğini sorunlarını giderme | Microsoft Docs"
description: "Azure Backup performans sorunlarının nedeni tanılamanıza yardımcı olması için sorun giderme yönergeleri sunar"
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.assetid: e379180a-db13-4e0c-90e4-28e5dd6f5b14
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 373a98855886cc7be7518c664f82bb6f92ca86f3
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Azure Backup’ta dosya ve klasörlerin yavaş yedekleme sorunlarını giderme
Bu makalede, Azure Backup kullanırken dosyalar ve klasörler için yedekleme performansın nedenini tanılamaya yardımcı olmak için sorun giderme kılavuzu verilmiştir. Dosyaları yedeklemek için Azure Yedekleme aracısı kullandığınızda, yedekleme işlemi beklenenden daha uzun sürebilir. Bu gecikme bir veya daha fazlasını tarafından nedeni olabilir:

* [Yedeklenmekte olan bilgisayarda performans sorunları vardır.](#cause1)
* [Başka bir işlem veya virüsten koruma yazılımı ile Azure yedekleme işlemi engelliyor.](#cause2)
* [Yedekleme aracısını bir Azure sanal makine (VM) üzerinde çalışıyor.](#cause3)  
* [Çok sayıda dosya (milyonlarca) yedekleme yapıyorsanız.](#cause4)

İndirme ve yükleme sorunlarını giderme başlamadan önce öneririz [en son Azure Backup aracısını](http://aka.ms/azurebackup_agent). Çeşitli sorunları giderin, özellikleri ekleyin ve performansı artırmak için Backup Aracısı sık güncelleştirmeler vermiyoruz.

Ayrıca gözden geçirmenizi öneririz [Azure Backup hizmeti ile ilgili SSS](backup-azure-backup-faq.md) , değil yaşıyorsunuz ortak yapılandırma sorunlardan biri emin olmak için.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Neden: Bilgisayardaki performans sorunları
Yedeklenmekte olan bilgisayarda performans sorunlarını gecikmelere neden olabilir. Örneğin, bilgisayarın özelliği okuma veya yazma disk veya ağ üzerinden veri göndermek için kullanılabilir bant genişliği performans sorunlarını neden olabilir.

Windows adlı yerleşik bir araç sağlar [Performans İzleyicisi](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) bu performans sorunlarını algılamak için (Perfmon).

Bazı performans sayaçları ve en iyi yedeklemeler için performans sorunlarını tanılamada yararlı olabilir aralıkları aşağıda verilmiştir.

| Sayaç | Durum |
| --- | --- |
| Mantıksal Disk (Fiziksel Disk)--% boşta |• %100 %50 boşta boşta Sağlıklı =</br>• %49 %20 boşta boşta uyarı veya İzleyici =</br>• % 19 boşta %0 boşta kritik veya dışı Spec = |
| Mantıksal Disk (Fiziksel Disk)--% Ort. Disk sn okuma veya yazma |• 0,001 ms 0.015 ms için sağlıklı =</br>• 0.015 ms 0.025 ms için uyarı veya İzleyici =</br>• 0.026 ms veya kritik veya dışı Spec artık = |
| Mantıksal Disk (Fiziksel Disk)--geçerli Disk Sırası Uzunluğu (tüm örnekler) |6 dakikadan fazla için 80 istek |
| Bellek--olmayan havuzda bayt |• Tüketilen havuzu % 60'den Sağlıklı =<br>• %61 80 tüketilen havuzunun uyarı veya İzleyici =</br>• Tüketilen % 80 havuzu büyük kritik veya dışı Spec = |
| Bellek--disk belleği havuzu bayt sayısı |• Tüketilen havuzu % 60'den Sağlıklı =</br>• %61 80 tüketilen havuzunun uyarı veya İzleyici =</br>• Tüketilen % 80 havuzu büyük kritik veya dışı Spec = |
| Bellek--kullanılabilir megabayt |• kullanılabilir boş bellek % 50 veya daha iyi =</br>• 25 kullanılabilir boş bellek yüzdesi İzleyici =</br>• 10 kullanılabilir boş bellek yüzdesi uyarı =</br>• 100 MB veya kullanılabilir boş bellek % 5'den daha az önemli veya dışı Spec = |
| İşlemci--\%işlemci zamanı (tüm örnekler) |• % 60'den tüketilen Sağlıklı =</br>• 61 tüketilen % 90 İzleyici veya uyarı =</br>• 91 tüketilen % 100 kritik = |

> [!NOTE]
> Altyapı sorunlu olduğunu belirlerseniz, daha iyi performans için düzenli aralıklarla disk birleştirme öneririz.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Neden: Başka işlem veya Azure yedekleme ile engellemesini virüsten koruma yazılımı
Diğer işlemlerin Windows sisteminde Azure Yedekleme aracısı işlemi performansını olumsuz yönde burada etkilenen çeşitli örneklerine gördük. Örneğin, verileri yedeklemek için Azure Backup aracısı ve başka bir program kullanıyorsanız veya virüsten koruma yazılımı çalıştıran ve yedeklenecek dosyaları üzerinde bir kilit varsa, birden çok üzerinde kilitler dosyaları Çekişme neden olabilir. Bu durumda, yedekleme başarısız olabilir veya iş beklenenden daha uzun sürebilir.

En iyi Bu senaryoda Azure Yedekleme aracısı yedekleme zamanını değişiklikler olup olmadığını görmek için diğer yedekleme programınızı kapatmak için önerilir. Genellikle, birden çok yedekleme işleri aynı anda çalışmayan emin birbirine etkilemesini önlemek yeterli olur.

Virüsten koruma programları için konumları ve aşağıdaki dosyaları dışla öneririz:

* C:\Program Files\Microsoft Azure kurtarma Hizmetleri Agent\bin\cbengine.exe bir işlem olarak
* C:\Program Files\Microsoft Azure kurtarma Hizmetleri Agent\ klasörleri
* (Standart konumu değil kullanıyorsanız) konumu boş

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Neden: bir Azure sanal makine üzerinde çalışan yedekleme aracısı
Backup Aracısı VM üzerinde çalıştırıyorsanız, performans, bir fiziksel makine çalıştırdığınızda yavaş olacaktır. Bu IOPS sınırlamaları nedeniyle beklenen bir durumdur.  Ancak, Azure Premium Storage yedeklenmekte veri sürücüleri geçerek performansını iyileştirebilirsiniz. Bu sorunu düzeltmeye çalışıyoruz ve düzeltme gelecekteki bir sürümde kullanılabilir olacaktır.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Neden: Yedekleme dosyaları çok sayıda (milyonlarca)
Büyük miktarda veri taşıma daha küçük bir veri hacmi taşımaktan daha uzun sürer. Bazı durumlarda, yalnızca boyutunu verileri, aynı zamanda dosyaları veya klasörleri sayısı için yedekleme saati ilişkilidir. Bu, özellikle küçük dosyalar (birkaç kilobayt için birkaç bayt) milyonlarca yedekleniyor olduğunda geçerlidir.

Veri yedekleme ve Azure'a taşıma olsa da, Azure dosyalarınızı aynı anda katalog için bu davranış oluşur. Bazı nadir senaryolarda katalog işlemi beklenenden daha uzun sürebilir.

Aşağıdaki göstergeleri performans sorunu anlamanıza ve buna göre sonraki adımları iş yardımcı olabilir:

* **UI veri aktarımı için ilerleme durumunu gösteren**. Veri hala aktarıldığı. Ağ bant genişliğini veya verilerin boyutunu gecikmelere neden.
* **UI ilerleme veri aktarımı için gösteren değil**. C:\Microsoft Azure kurtarma Hizmetleri Agent\Temp bulunan günlükleri'ni açın ve günlükleri FileProvider::EndData girişi için denetleyin. Bu giriş, veri aktarımı tamamlandı ve Katalog işlemi gerçekleştiriliyor belirtir. Yedekleme işleri iptal etme. Bunun yerine, katalog işlemini tamamlamak biraz daha uzun süre bekleyin. Sorun devam ederse, iletişim [Azure Destek](https://portal.azure.com/#create/Microsoft.Support).
