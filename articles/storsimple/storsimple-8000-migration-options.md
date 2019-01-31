---
title: 5000-7000 Serisi Storsimple'dan verileri geçirmek için ilgili seçenekleri değerlendirmede | Microsoft Docs
description: StorSimple 5000-7000 serisinden verileri geçirmeye seçeneklerine genel bakış sağlar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/30/2019
ms.author: alkohli
ms.openlocfilehash: bbf1eb623232332ed025fa0806e942e638c39974
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456060"
---
# <a name="options-to-migrate-data-from-storsimple-5000-7000-series"></a>5000-7000 Serisi Storsimple'dan verileri geçirmeye seçenekleri 

> [!IMPORTANT]
> 31 Temmuz 2019, StorSimple 5000/7000 Serisi (EOS) destek durumu sonuna ulaşacak. StorSimple 5000/7000 Serisi müşteriler belgede açıklanan alternatifleri birine geçiş öneririz.

StorSimple 5000-7000 Serisi ulaşmasını [destek sonu](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series) Temmuz 2019 içinde. StorSimple 5000-7000 Serisi için diğer Azure yükseltme seçeneğini çalıştıran müşteriler, karma Hizmetleri birinci taraf. Bu makalede, Azure hibrit veri geçirmek için kullanılabilir seçenekleri açıklanır. 

## <a name="migration-options"></a>Geçiş seçenekleri

StorSimple 5000-7000 Serisi kullanan müşteriler aşağıdaki seçenekleriniz vardır:

- **Azure seçenekleri**:

    - **StorSimple 8000 serisi için yükseltme** : yükseltme için StorSimple 8000 serisi ve bu nedenle StorSimple platformunda devam edin.  Bu yükseltme yolunu bir 8000 serisi, 5000-7000 Serisi cihazlar yerine müşterilerinize gerektirir. Veriler, 5000-7000 Serisi CİHAZDAN geçiş aracını kullanarak geçirilir. Taşıma başarıyla tamamlandıktan sonra StorSimple 8000 serisi cihazlar katman verileri Azure Blob Depolama'ya devam eder. 

    StorSimple 8000 serisi kullanarak verileri geçirme hakkında daha fazla bilgi için Git [veri Storsimple'dan 5000-7000 Serisi için 8000 serisi cihaz geçirme](storsimple-8000-migrate-from-5000-7000.md).

    - **Azure dosya eşitleme için geçirme** – bu yeni müşteriler kendi kuruluşunuzun dosya paylaşımlarını Azure dosyaları depolamak geçiş seçeneği sağlar. Bu dosya paylaşımlarını Azure dosya eşitleme (AFS) kullanarak şirket içi erişim için ardından toplanmıştır. Windows Server ana bilgisayarında AFS dağıtılabilir. Gerçek veri geçişinin ardından konak olarak gerçekleştirilir kopyalama veya Taşıma Aracı'nı kullanma.

    Verileri geçirmek için Azure dosya eşitleme hakkında daha fazla bilgi için Git [veri Storsimple'dan 5000-7000 Serisi için Azure dosya eşitleme taşıma](https://aka.ms/StorSimpleMigrationAFS).

- **Üçüncü taraf seçenekleri**:

    - **Geçirmek için Panzura özgürlük NAS** -StorSimple 5000-7000 müşterilerin kendi verilerini Azure'da korumak için Panzura özgürlük NAS geçirmek için seçebilirsiniz. Panzura özgürlük çözüm yayılan veri merkezleri, ofisleri, genel ve özel bulutların bir NAS çözümü sağlar. Çözüm, NFS, SMB ve mobil istemciler için yerel, karma ve bulut data iş akışları sağlar. Bu geçiş Panzura tarafından desteklenir ve geçiş desteği'nden isteyerek müşteriler başlayabilirsiniz [Panzura Web sitesi](https://panzura.com/storsimple-migration/).

    - **Geçiş için Cohesity** -Cohesity veri Cohesity veri platformu Azure üzerinde geçerli StorSimple 5000-7000 geçirilecek olanak sağlar. Cohesity veri platformu dosyaları, yedeklemeler, nesneleri ve Vm'leri tek bir yerel bulut çözümünün birleştiren bir yazılım tanımlı web ölçeğindeki çözümüdür. Veri platformuna geçişten sonra yönetmek, koruma ve veri ve bulut uygulamaları üzerinden tek bir cam bölmeyle core'a sağlayın. En az üç düğüm olarak Cohesity ile başlayın. Daha fazla bilgi [Cohesity DataPlatform geçiş](https://info.cohesity.com/migrate-from-storsimple-to-cohesity.html).

## <a name="migration---frequently-asked-questions"></a>Geçişi - sık sorulan sorular

### <a name="q-when-do-the-storsimple-5000-and-7000-series-devices-reach-end-of-service"></a>S. Ne zaman StorSimple 5000 ve 7000 Serisi cihazlar hizmeti sonuna ulaşmak? 

A. StorSimple 5000-7000 Serisi ulaşmak [hizmet sonuna](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series) Temmuz 2019 içinde. Hizmet sonuna Microsoft artık Temmuz 2019 sonra donanım ve yazılım bu cihaz için destek sağlamak mümkün olacağını gösterir. Verileri cihazlarınızdan artık taşımak için bir plan formulating Başlat öneririz.

### <a name="q-what-happens-to-the-data-i-have-stored-in-azure"></a>S. Azure'da depolanan verilere ne olur?  

A. Daha yeni bir hizmete geçirdikten sonra verileri Azure'da kullanmaya devam edebilirsiniz. 


### <a name="q-what-happens-to-the-data-i-have-stored-locally-on-my-storsimple-device"></a>S. StorSimple cihazımda depolanan yerel olarak verileri ne olur? 

A. Geçiş belgelerinde açıklanan şekilde yeni hizmet yerel cihazda veri kopyalanabilir.

### <a name="q-what-happens-if-i-want-to-keep-my-storsimple-50007000-series-appliance"></a>S. My StorSimple 5000/7000 Serisi Gereci tutmak istiyorsanız ne olacak? 

A. Hizmetlerin çalışmaya devam edebilir ancak Microsoft artık donanım ve yazılım destek sağlamak mümkün olacaktır. Geçiş için iş sürekliliği önemle tavsiye edilir.

### <a name="q-what-options-are-available-to-migrate-data-from-storsimple-5000-7000-series-devices"></a>S. 5000-7000 Serisi cihazlar Storsimple'dan verileri geçirmek hangi seçenekler kullanılabilir? 

A. Kendi senaryoya bağlı olarak, StorSimple 5000-7000 Serisi kullanıcılar aşağıdaki geçiş seçenekleri de mevcuttur. 

 - **8000 serisi için yükseltme**: StorSimple platformunda devam etmek istediğinizde bu seçeneği kullanın. 
 - **Azure dosya eşitleme geçirme**: Azure yerel biçime geçiş yapmak istediğinizde bu seçeneği kullanın. Azure dosya eşitleme dosya paylaşımları Merkezi Yönetim için kullanabilirsiniz. 

Burada listelenmeyen geçiş seçenekler hakkında konuşmak amacıyla Microsoft Support başvurabilirsiniz.

### <a name="q-is-migration-to-other-storage-solutions-supported"></a>S. Desteklenen diğer depolama çözümleri için geçiş mi?

A. Evet. Konak veri kopyası ile diğer depolama çözümleri geçiş desteklenir.

### <a name="q-is-migration-supported-by-microsoft"></a>S. Geçiş, Microsoft tarafından desteklenir? 

A. 5000 veya 7000 serisinden geçiş tam olarak desteklenen bir işlemdir. Aslında, Microsoft, Geçişe başlamadan önce Destek birimine ulaşmanızı önerir. Geçişi şu anda destekli bir işlemdir. 5000-7000 Serisi bir cihaza, Storsimple'dan verileri geçirmeyi düşünüyorsanız [bir destek bileti açın](storsimple-8000-contact-microsoft-support.md).

### <a name="q-what-is-the-pricing-model-for-both-the-migration-options"></a>S. Geçiş seçenekleri için fiyatlandırma modeli nedir?

A. Geçiş maliyeti, belirlediğiniz seçeneğe bağlı olarak değişir. Bir StorSimple 8000 serisi için yükseltmeye karar verirseniz geçiş kendisi ücretsizdir ancak donanım maliyeti olmayacaktır. 

Benzer şekilde, Azure dosya eşitleme kullanırken, hizmeti için Abonelik ücretleri geçerli olabilir. Her durumda, Müşteriler ayrıca sürekli depolama ücret ödemeniz gerekir. Tahmin için aşağıdakilere bakın: 
- [StorSimple'nın fiyatlandırma](https://azure.microsoft.com/pricing/details/storsimple/)  
- [Fiyatlandırma AFS]( https://azure.microsoft.com/pricing/details/storage/files/)

### <a name="q--how-long-does-it-take-to-complete-a-migration"></a>S.  Ne kadar bir geçiş işlemini tamamlamak için sürer?

A. Veri geçiş süresini, veriler ve yükseltme seçeneği belirlenmiş miktarına bağlıdır. 

### <a name="q-what-is-the-end-of-support-date-for-storsimple-8000-series"></a>S. StorSimple 8000 serisi için desteği son tarih nedir?

A. StorSimple 8000 serisi için desteği bitiş tarihi yayımlanan [burada](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series).


## <a name="next-steps"></a>Sonraki adımlar
 - [Verileri bir Storsimple'dan 5000-7000 Serisi 8000 serisinin cihazına geçirmek](storsimple-8000-migrate-from-5000-7000.md).
 - [Azure dosya eşitleme için StorSimple 5000-7000 serisinden veri geçirme](storsimple-5000-7000-afs-migration.md)
