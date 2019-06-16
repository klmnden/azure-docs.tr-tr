---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: jeking
ms.custom: include file
ms.openlocfilehash: efa593d0ff0043d81574b67192deed30933e1e40
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66114876"
---
Coğrafi olarak yedekli depolama (GRS), en az % 99,99999999999999'si sağlamak için tasarlanmıştır (16 9) mil birincil bölgeden yüzlerce ikincil bir bölgeye veri çoğaltma ile belirli bir yıl boyunca nesnelerin dayanıklılık. Etkin GRS depolama hesabınızın sahip, verilerinizi tam bölgesel bir kesinti veya birincil bölgeye kurtarılamaz bir olağanüstü durum olması durumunda bile kalıcı olur.

GRS için kullanmayı seçerseniz, aralarından seçim yapabileceğiniz ilgili iki seçeneğiniz vardır:

* Veriler yalnızca Microsoft ikincil bölgeye birincil bir yük devretme başlatır, okumak kullanılabilir ancak bu GRS, verilerinizi ikincil bir bölgede başka bir veri merkezine çoğaltır. 
* GRS okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) temel alır. RA-GRS, verilerinizi ikincil bir bölgede başka bir veri merkezine çoğaltılır ve ayrıca ikincil bölgeden okumak için seçeneği sağlar. RA-GRS ile mi Microsoft ikincil bölgeye birincil bir yük devretme başlatır bağımsız olarak ikincil bölgeye okuma yapabilirsiniz. 

GRS veya RA-GRS etkin olan bir depolama hesabı için tüm verileri ilk kez çoğaltılır yerel olarak yedekli depolama (LRS). Bir güncelleştirme, öncelikle birincil konuma taahhüt eder ve LRS kullanılarak çoğaltılır. Güncelleştirmeyi zaman uyumsuz olarak GRS kullanarak ikincil bir bölgeye çoğaltılır. İkincil konuma veri yazıldığında de LRS kullanarak içinde bu konuma çoğaltılır. 

Birincil ve ikincil bölgeler, çoğaltmalar ayrı hata etki alanları arasında yönetme ve yükseltme etki alanları içindeki bir depolama ölçek birimi. Depolama ölçek birimi, veri merkezi içinde temel çoğaltma birimidir. Bu düzeyde çoğaltma LRS tarafından sağlanır; Daha fazla bilgi için [yerel olarak yedekli depolama (LRS): Azure depolama için düşük maliyetli veri yedekliği](../articles/storage/common/storage-redundancy-lrs.md).

Hangi çoğaltma seçeneği kullanmak için karar verirken, aşağıdaki noktaları göz önünde bulundurun:

* Bölgesel olarak yedekli depolama (ZRS), yüksek oranda kullanılabilirlik ile zaman uyumlu çoğaltma sağlar ve bazı senaryolar için GRS veya RA-GRS daha iyi bir seçim olabilir. ZRS hakkında daha fazla bilgi için bkz. [ZRS](../articles/storage/common/storage-redundancy-zrs.md).
* Zaman uyumsuz çoğaltma, ikincil bölgeye çoğaltılır, birincil bölgeye yazılır veri andan itibaren bir gecikme içerir. Birincil bölgeden verilerin kurtarılamaması durumunda bölgesel bir olağanüstü durum yaşandığında ikincil bölgeye henüz çoğaltılan henüz değişiklikler kaybolabilir.
* GRS ile Microsoft ikincil bölgeye yük devretme başlatır sürece çoğaltma okuma veya yazma erişimi için kullanılabilir değildir. Bir yük devretme durumunda okuma ve yük devretme sonrasında bu verilere yazma erişimi tamamlandı. Daha fazla bilgi için lütfen bkz [olağanüstü durum kurtarma Kılavuzu](../articles/storage/common/storage-disaster-recovery-guidance.md).
* İkincil bölgeden okumak uygulamanız gerekiyorsa, RA-GRS etkinleştirin.