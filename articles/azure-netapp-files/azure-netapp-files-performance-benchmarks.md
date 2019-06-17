---
title: Azure NetApp dosyaları için performans kıyaslamalarını | Microsoft Docs
description: Azure NetApp dosyaları birim düzeyinde performans kıyaslama testlerinin sonuçlarını açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: b-juche
ms.openlocfilehash: aca0668fc364518fe45d9fe94d089ee366b25676
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64870890"
---
# <a name="performance-benchmarks-for-azure-netapp-files"></a>Azure NetApp Files için performans kıyaslamaları

Bu makalede Azure NetApp dosyaları birim düzeyinde performans kıyaslama testlerinin sonuçlarını açıklar. 

## <a name="sample-application-used-for-the-tests"></a>Testler için kullanılan örnek uygulama

Performans testleri Azure NetApp dosyaları'nı kullanan bir örnek uygulaması ile çalıştırılamadı. Uygulama aşağıdaki özelliklere sahiptir: 

* Bulut için yapılandırılmış Linux tabanlı bir uygulama
* Eklenen sanal makinelerle (VM) gerektiği gibi bilgi işlem gücü artırmak için doğrusal olarak ölçeklendirebilirsiniz
* Data lake hızlı erişilebilirliğini gerektirir
* Bazen rastgele ve bazen sıralı g/ç düzenleri 
    * Rastgele bir desen, büyük miktarlarda g/ç için düşük gecikme süresi gerektirir. 
    * Bir ardışık düzeni, büyük miktarda bant genişliği gerektirir. 

## <a name="about-the-workload-generator"></a>İş yükü Oluşturucu hakkında

Sonuçları Vdbench Özet dosyalarından gelir. [Vdbench](https://www.oracle.com/technetwork/server-storage/vdbench-downloads-1901681.html) doğrulamak için depolama performansını, disk g/ç iş yükleri oluşturan bir komut satırı yardımcı programıdır. Kullanılan istemci-sunucu yapılandırmasını ölçeklenebilirdir.  Bu, tek bir karma master/istemci ve 14 ayrılmış istemci Vm'leri içerir.

## <a name="about-the-tests"></a>Testler hakkında

Testler, örnek uygulamayı olabilir sınırları ve yanıt süresi, eğriler sınırları belirlemek için tasarlanmıştır.  

Aşağıdaki test çalıştırılamadı: 

* % 100 8 KiB rastgele okuma
* % 100 8 KiB rastgele yazma
* % 100 64 KiB sıralı okuma
* % 100 64 KiB sıralı yazma
* % 50 64 KiB sıralı okuma, % 50 64 KiB sıralı yazma
* 50 8 KiB rastgele okuma %, % 50 8 KiB rastgele yazma

## <a name="bandwidth"></a>Bant genişliği

Birden çok Azure NetApp dosyaları sunar [hizmet düzeyleri](azure-netapp-files-service-levels.md). Her hizmet düzeyi farklı miktarda bir bant genişliği başına sağlanan Kapasite (birim kota) TiB sunar. Bir birim için bant genişliği sınırı, hizmet düzeyi ve birim kota birleşimini temel sağlanır. Bant genişliği sınırını bir faktör gerçek aktarım hızı gerçekleşmiş miktarını belirleme konusunda olduğuna dikkat edin.  

Şu anda 4,500 MiB tek bir birim test karşı bir iş yükü tarafından elde en yüksek aktarım hızıdır.  Premium hizmet düzeyiyle 70.31 TiB birimi kotası, bu aktarım hızı aşağıdaki hesaplama gerçekleştirmek için yeterli bant genişliği sağlayacak: 

![Bant genişliği formülü](../media/azure-netapp-files/azure-netapp-files-bandwidth-formula.png)

![Kota ve hizmet düzeyi](../media/azure-netapp-files/azure-netapp-files-quota-service-level.png)

## <a name="throughput-intensive-workloads"></a>Aktarım hızı açısından yoğun iş yükleri

Aktarım hızı testi Vdbench ve 12xD32s V3 depolama Vm'leri bir bileşimi kullanılır. Örnek birim testinde elde edilen aşağıdaki üretilen iş sayısı:

![Aktarım hızı testi](../media/azure-netapp-files/azure-netapp-files-throughput-test.png)

## <a name="io-intensive-workloads"></a>G/Ç açısından yoğun iş yükleri

G/ç test Vdbench ve 12xD32s V3 depolama Vm'leri bir bileşimi kullanılır. Örnek birim test aşağıdaki g/ç sayıları elde edebilirsiniz:

![G/ç test](../media/azure-netapp-files/azure-netapp-files-io-test.png)

## <a name="latency"></a>Gecikme süresi

Test sanal makineleri ve Azure NetApp dosyaları toplu arasındaki uzaklığı g/ç performansı üzerinde bir etkisi yoktur.  Aşağıdaki grafik, IOPS ve gecikme süresi yanıt eğrileri iki farklı sanal makine kümeleri için karşılaştırır.  Bir VM kümesi Azure NetApp dosyaları ve diğer küme hemen daha fazla.  Daha fazla VM kümesi artan gecikme sürelerini belirli bir düzeyde paralellik derecesi elde IOPS miktarına etkili olduğuna dikkat edin.  Ne olursa olsun, bir birim yapılan okumalar aşağıda gösterildiği gibi 300.000 IOPS aşabilir: 

![Gecikme süresi incelemesi](../media/azure-netapp-files/azure-netapp-files-latency-study.png)

## <a name="summary"></a>Özet

Gecikme süresi açısından duyarlı iş yükleri (veritabanları), tek bir milisaniyeden kısa tepki süresi olabilir. İşlem performansı, üzerinde 300 k IOPS için tek bir birim olabilir.

Aktarım hızı duyarlı uygulamalar (için akış ve görüntüleme) 4.5GiB olabilir / s aktarım hızı.
