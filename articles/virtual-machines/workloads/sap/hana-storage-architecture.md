---
title: SAP hana (büyük örnekler) azure'da depolama mimarisi | Microsoft Docs
description: Depolama Mimarisi (büyük örnekler) Azure üzerinde SAP HANA dağıtma.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/05/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a2cfe9dc02e69f3b47c99e01bc70bffc942338fd
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707265"
---
# <a name="sap-hana-large-instances-storage-architecture"></a>SAP HANA (büyük örnekler) depolama mimarisi

Depolama alanı düzenini (büyük örnekler) Azure üzerinde SAP HANA için SAP HANA tarafından önerilen yönergeleri SAP başına Klasik dağıtım modelinde yapılandırılır. Yönergeleri bölümünde belgelendirilen [SAP HANA depolama gereksinimlerini](https://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) teknik incelemesi.

Ben sınıf türünün HANA büyük örneği dört kez bellek toplu depolama birimi olarak gelir. HANA büyük örneği birimleri Type II sınıfı için depolama dört kat daha fazla değildir. Birimleri tasarlanmıştır HANA işlem günlüğü yedeklemeleri depolamak için bir birim gelir. Daha fazla bilgi için [yüklemek ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırma](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Depolama tahsisi açısından aşağıdaki tabloya bakın. Tablo farklı HANA büyük örneği birimleri ile sağlanan farklı birimler için kaba kapasite listeler.

| HANA büyük örneği SKU | hana/veri | hana/günlük | hana ve paylaşılan | hana/logbackups |
| --- | --- | --- | --- | --- |
| S72 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3,328 GB | 768 GB |1,280 GB | 768 GB |
| S96 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| S192 | 4\.608 GB | 1\.024 GB | 1\.536 GB | 1\.024 GB |
| S192m | 11,520 GB | 1\.536 GB | 1,792 GB | 1\.536 GB |
| S192xm |  11,520 GB |  1\.536 GB |  1,792 GB |  1\.536 GB |
| S384 | 11,520 GB | 1\.536 GB | 1,792 GB | 1\.536 GB |
| S384m | 12.000 GB | 2\.050 GB | 2\.050 GB | 2040 GB |
| S384xm | 16.000 BİN GB | 2\.050 GB | 2\.050 GB | 2040 GB |
| S384xxm |  20.000 GB | 3\.100 GB | 2\.050 GB | 3\.100 GB |
| S576m | 20.000 GB | 3\.100 GB | 2\.050 GB | 3\.100 GB |
| S576xm | 31.744 GB | 4\.096 GB | 2\.048 GB | 4\.096 GB |
| S768m | 28,000 GB | 3\.100 GB | 2\.050 GB | 3\.100 GB |
| S768xm | 40.960 GB | 6\.144 GB | 4\.096 GB | 6\.144 GB |
| S960m | 36,000 GB | 4,100 GB | 2\.050 GB | 4,100 GB |


Gerçek dağıtılan birimleri, dağıtım ve birim boyutları göstermek için kullanılan bir aracı bağlı olarak değişebilir.

Bir HANA büyük örneği SKU bölüyorsanız, olası bölme parçaları birkaç örnek gibi görünmelidir:

| GB cinsinden bellek bölümü | hana/veri | hana/günlük | hana ve paylaşılan | hana/log/yedekleme |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| 1,024 | 1,792 GB | 640 GB | 1\.024 GB | 640 GB |
| 1536 | 3,328 GB | 768 GB | 1,280 GB | 768 GB |


Bu boyutları dağıtım ve hacimlerinde aramak için kullanılan araçları biraz göre değişebilir kaba birim sayılardır. Ayrıca var. 2,5 TB gibi diğer bölüm boyutları Önceki bölümleri için kullanılan benzer bir formül ile bu depolama boyutu hesaplanır. "Bölümler" terimi, işletim sistemi, bellek veya CPU kaynaklarının herhangi bir yolla bölümlenir anlamına gelmez. Bu, tek bir HANA büyük örneği birim dağıtmak isteyebilirsiniz farklı HANA örnekleri için depolama bölümleri gösterir. 

Daha fazla depolama alanı gerekebilir. Ek depolama alanı 1 TB birimler satın alarak depolama ekleyebilirsiniz. Bu ek depolama alanı, ek bir birim eklenebilir. Ayrıca, bir veya daha fazla var olan birimler genişletmek için de kullanılabilir. İlk olarak dağıtılan ve genellikle önceki tablolarda tarafından belgelenen şekilde birimlerin boyutunu azaltmak mümkün değildir. Birim adlarını değiştirme veya takma adları mümkün değildir. Daha önce açıklanan depolama birimleri, HANA büyük örneği birimine NFS4 birimleri olarak eklenir.

Depolama anlık görüntüleri yedekleme ve geri yükleme, olağanüstü durum kurtarma amacıyla kullanabilirsiniz. Daha fazla bilgi için [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Başvuru [HLI desteklenen senaryoları](hana-supported-scenario.md) senaryonuz için depolama düzeni Ayrıntılar için.

## <a name="run-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Bir HANA büyük örneği biriminde birden çok SAP HANA örnekleri çalıştırma

HANA büyük örneği birimi birden fazla etkin SAP HANA örneğinde barındırmak mümkündür. Depolama anlık görüntüleri ve olağanüstü durum kurtarma özellikleri sağlamak için bu tür bir yapılandırma örneği başına bir birim gerektirir. Şu anda, HANA büyük örneği birimleri gibi alt bölümlere ayrılabilir:

- **S72 S72m, S96 S144, S192**: En düşük başlangıç birim 256 GB ile 256 GB artışlarla. 256 GB ile 512 GB gibi farklı aralıklarla en büyük bellek birimi birleştirilebilir.
- **S144m ve S192m**: En küçük birim 512 GB ile 256 GB artışlarla. 512 GB ve 768 GB gibi farklı aralıklarla en büyük bellek birimi birleştirilebilir.
- **Türü II sınıfı**: 512 GB'ın katlarıyla artacak en düşük başlangıç birimindeki 2 TB. 512 GB, 1 TB ve 1,5 TB'lık gibi farklı aralıklarla en büyük bellek birimi birleştirilebilir.

Birden çok SAP HANA örnekleri çalışan örneklerde aşağıdaki gibi görünebilir.

| SKU | Bellek boyutu | Depolama boyutu | Birden fazla veritabanıyla, boyutları |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 x 768 GB HANA örneği<br /> ya da 1 x 512 GB örneğine + 1 x 256 GB örneği<br /> veya 3 x 256 GB örnekleri | 
| S72m | 1,5 TB | 6 TB | 3x512GB HANA örnekleri<br />ya da 1 x 512 GB örneğine + 1 x 1 TB örneği<br />veya 6 x 256 GB örnekleri<br />veya 1x1.5 TB örneği | 
| S192m | 4 TB | 16 TB | 8 x 512 GB örnekleri<br />veya 4 x 1 TB örnekleri<br />ya da 4 x 512 GB örnekleri + 2 x 1 TB örnekleri<br />ya da 4 x 768 GB örnekleri + 2 x 512 GB örnekleri<br />ya da 1 x 4 TB örneği |
| S384xm | 8 TB | 22 TB | 4 x 2 TB örnekleri<br />veya 2 x 4 TB örnekleri<br />ya 2 x 3 TB örnekleri + 1 x 2 TB örnekleri<br />veya örnekleri 2x2.5 TB + 1 x 3 TB örnekleri<br />ya da 1 x 8 TB örneği |


Diğer farklılıklar da vardır. 

## <a name="encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi
Disklere depolanan gibi verilerin saydam bir şifrelenmesi HANA büyük örneği için kullanılan depolama alanı sağlar. Bir HANA büyük örneği birim dağıtıldığında, bu tür şifrelemeyi etkinleştirebilirsiniz. Dağıtım gerçekleştirildikten sonra şifrelenmiş birimlere da değiştirebilirsiniz. Şifrelenmemiş taşımak şifrelenmiş birimlere saydamdır ve kapalı kalma süresi gerektirmez. 

Ben türüyle sınıfı SKU, LUN üzerinde depolanan önyükleme birimi şifrelenir. SKU'ları, HANA büyük örneği Type II sınıfı için işletim sistemi yöntemleriyle LUN önyükleme şifrelemeniz gerekir. Daha fazla bilgi için Microsoft Hizmet Yönetimi ekibine başvurun.

## <a name="required-settings-for-larger-hana-instances-on-hana-large-instances"></a>HANA büyük örnekler üzerinde daha büyük bir HANA örnekleri için gerekli ayarları
HANA büyük örnekleri kullanılan depolama alanı dosya boyutu sınırlaması vardır. [Boyut sınırlaması 16 TB'tır](https://docs.netapp.com/ontap-9/index.jsp?topic=%2Fcom.netapp.doc.dot-cm-vsmg%2FGUID-AA1419CF-50AB-41FF-A73C-C401741C847C.html) dosya başına. Farklı dosya boyutu sınırlamaları EXT3 dosya sistemleri, HANA örtük olarak HANA büyük örnekleri depolaması tarafından zorlanan depolama sınırlama farkında değil. Sonuç olarak 16 TB'lık dosya boyutu sınırını ulaşıldığında HANA'ya yeni bir veri dosyası otomatik olarak oluşturmaz. HANA 16 TB ötesinde dosya büyütme girişiminde HANA sonunda, hataları ve dizin sunucusu çöker rapor eder.

> [!IMPORTANT]
> Veri büyür ve HANA büyük Örnek Depolama 16 TB dosya boyutu sınırını aşan çalışılırken HANA önlemek için aşağıdaki parametreleri HANA global.ini yapılandırma dosyasında ayarlamanız gerekir
> 
> - datavolume_striping = true
> - datavolume_striping_size_gb 15000 =
> - Ayrıca SAP bkz Not [#2400005](https://launchpad.support.sap.com/#/notes/2400005)
> - SAP Not unutmayın [#2631285](https://launchpad.support.sap.com/#/notes/2631285)




**Sonraki adımlar**
- Başvuru [HANA büyük örnekler için desteklenen senaryolar](hana-supported-scenario.md)