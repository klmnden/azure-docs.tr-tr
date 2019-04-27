---
title: SAP HANA (büyük örnekler) azure'da ekleme gereksinimleri | Microsoft Docs
description: SAP HANA (büyük örnekler) azure'da ekleme gereksinimleri.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/31/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 252c84bce2b70f6931593fe9410abe6cc146b5bb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60679060"
---
# <a name="onboarding-requirements"></a>Ekleme gereksinimleri

Bu liste, SAP HANA (büyük örnekler) Azure üzerinde çalıştırmak için gereksinimler birleştirir.

**Microsoft Azure**

- SAP hana (büyük örnekler) azure'da bağlı bir Azure aboneliği.
- Microsoft Premier Destek Sözleşmesi. Azure'da SAP çalıştırma ile ilgili ayrıntılı bilgi için bkz: [SAP destek Not #2015553 – Microsoft Azure üzerinde SAP: Destek önkoşulları](https://launchpad.support.sap.com/#/notes/2015553). 384 ve daha fazla CPU HANA büyük örneği birimleri kullanmayı da Premier genişletmek gerekirse Azure hızlı yanıt'ı dahil etmek için destek sözleşmesi.
- Farkındalık, HANA büyük örneği SAP boyutlandırma alıştırmaya gerçekleştirdikten sonra ihtiyacınız SKU.

**Ağ bağlantısı**

- Şirket içi arasında Azure ExpressRoute: Şirket içi veri merkezinizi Azure'a bağlanmak için en az 1 GB/sn bağlantı ISS'niz tarafından sipariş emin olun. HANA büyük örneği birimleri ve Azure arasında bağlantı, ExpressRoute teknolojisini kullanıyor. Bu ExpressRoute bağlantısı HANA büyük örneği birimleri ve Azure arasında bu belirli ExpressRoute bağlantı hattı için tüm veri giriş ve çıkış ücretleri dahil olmak üzere, HANA büyük örneği birimlerinin fiyatı dahildir. Bu nedenle, müşteri olarak değil karşılaştığınız ek maliyetler ExpressRoute bağlantınız şirket içi ile Azure arasında ötesinde.

**İşletim sistemi**

- Lisanslar SAP uygulamaları için SUSE Linux Enterprise Server 12.

   > [!NOTE] 
   > Microsoft tarafından sunulan işletim sistemi SUSE ile kayıtlı değil. Bu abonelik yönetimini örneğine bağlı değil.

- SUSE Linux abonelik yönetimi bir VM'de azure'da dağıtılan aracı. Bu araç, kayıtlı ve sırasıyla SUSE tarafından güncelleştirildi (büyük örnekler) Azure üzerinde SAP HANA için yeteneği sağlar. (HANA büyük örnek veri merkezinde internet erişimi yoktur.) 
- Red Hat Enterprise Linux 6.7 veya SAP HANA için 7.x için lisans.

   > [!NOTE]
   > Microsoft tarafından sunulan işletim sistemi, Red Hat ile kayıtlı değil. Bu, bir Red Hat Abonelik Yöneticisi örneğine bağlı değil.

- Red Hat Abonelik Yöneticisi azure'da bir VM üzerinde dağıtılabilir. Red Hat Abonelik Yöneticisi, kayıtlı ve Red Hat sırasıyla güncelleştirildi (büyük örnekler) Azure üzerinde SAP HANA için yeteneği sağlar. (Hiçbir doğrudan internet erişimini Azure büyük örnek damgası üzerinde dağıtılan kiracıda yoktur.)
- SAP destek sahip olmanızı gerektirir de Linux sağlayıcınız ile sözleşme. Bu gereksinim, HANA büyük örneği ya da Linux Azure üzerinde çalışmasına olgu çözüm kaldırılmaz. Bazı Linux Azure galeri görüntüleri hizmet ücretinin benzemez *değil* HANA büyük örneği çözüm teklife dahil edilen. Bunu SAP Linux dağıtımcısı gerekliliklerini destek sözleşmeleri ile ilgili sizin sorumluluğunuzdur. 
   - SUSE Linux için destek sözleşmeleri gereksinimlerini arayın [SAP notu #1984787 - SUSE Linux Enterprise Server 12: Yükleme notları](https://launchpad.support.sap.com/#/notes/1984787) ve [SAP notu #1056161 - SUSE öncelikli destek için SAP uygulamaları](https://launchpad.support.sap.com/#/notes/1056161).
   - Red Hat Linux için destek içerir ve hizmet güncelleştirmeleri işletim sistemlerini HANA büyük örneği için doğru abonelik düzeylerini olması gerekir. Red Hat, SAP çözüm için Red Hat Enterprise Linux abonelik önerir. Başvuru https://access.redhat.com/solutions/3082481. 

Farklı SAP HANA sürümleriyle farklı Linux sürümleri için destek matrisi bkz [SAP notu #2235581](https://launchpad.support.sap.com/#/notes/2235581).

Başvurmak için uyumluluk matrisi HLI bellenim/DRIVER sürümleri ve işletim sistemi [HLI için işletim sistemi yükseltme](os-upgrade-hana-large-instance.md).


> [!IMPORTANT] 
> Bu noktada Type II birimleri yalnızca SLES 12 SP2 işletim sistemi sürümü desteklenmiyor. 


**Veritabanı**

- Lisanslar ve yazılım yükleme bileşenleri için SAP HANA (platform veya enterprise edition).

**Uygulamalar**

- Lisanslar ve yazılım yükleme bileşenleri ilgili SAP ve SAP HANA'ya bağlanan tüm SAP uygulamaları için sözleşmeler destekler.
- Lisansları ve herhangi bir SAP olmayan uygulamalar için yazılım yükleme bileşenleri ile SAP HANA (büyük örnekler) Azure ortamlarında kullanılan ve destek sözleşmeleri ilgili.

**Beceriler**

- Deneyiminizi ve Azure Iaas ve bileşenlerinin bilgisine sahip.
- Deneyiminizi ve bir SAP iş yükünü azure'a dağıtma konusunda bilgi.
- SAP HANA yüklemesi kişisel sertifikalı.
- SAP HANA geçici olarak yüksek kullanılabilirlik ve olağanüstü durum kurtarma tasarım için Mimarı becerileri SAP.

**SAP**

- Beklenir SAP müşterisi olduğunuz ve bir destek sözleşmesi ile SAP.
- İçin özellikle uygulamaları HANA büyük örneği SKU'ları Type II sınıfının SAP ile SAP HANA ve büyük ölçekli ölçek büyütme donanımda nihai yapılandırmalar sürümlerinde başvurun.

**Sonraki adımlar**
- Başvuru [azure'da SAP HANA (büyük örnekler) mimarisi](hana-architecture.md)