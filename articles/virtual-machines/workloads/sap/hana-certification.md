---
title: SAP hana (büyük örnekler) azure'da sertifika | Microsoft Docs
description: SAP HANA (büyük örnekler) azure'da sertifika.
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
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 15de566d756d6b0f7719eabf74ee9c7ac66659d6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60794000"
---
# <a name="certification"></a>Sertifika

NetWeaver sertifika yanı sıra, SAP, SAP hana, SAP HANA Azure Iaas gibi belirli altyapıları desteklemek özel bir sertifika gerektirir.

Temel, derece SAP HANA sertifika ve NetWeaver'lu SAP notuna [SAP notu #1928533-azure'da SAP uygulamaları: Desteklenen Ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533).

SAP HANA (büyük örnekler) Azure birimlerdeki sertifika kayıtlarını bulunabilir [SAP HANA sertifikalı ve Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) site. 

SAP HANA (büyük örnekler) Azure türleri, SAP HANA sertifikalı Iaas platformları site içinde başvurulan Microsoft sağlar ve müşteriler SAP büyük SAP Business Suite, SAP BW, s/4 HANA, BW/4HANA veya diğer SAP HANA iş yüklerini azure'da dağıtma yeteneği. Çözüm, SAP HANA sertifikalı adanmış donanım damgada bağlıdır ([SAP HANA veri merkezi tümleştirmesi – TDI uyarlanmış](https://scn.sap.com/docs/DOC-63140)). SAP HANA TDI yapılandırılmış bir çözüm çalıştırırsanız, tüm SAP HANA tabanlı uygulamalar (örneğin, SAP hana'da SAP Business Suite, SAP BW on HANA SAP S4/HANA'ya ve BW4/HANA) donanım altyapısında çalışır.

SAP HANA Vm'lerinde çalışan için karşılaştırıldığında, bu çözümü bir avantajı vardır. Bu, çok daha yüksek bellek hacimleri için sağlar. Bu çözümü etkinleştirmek için aşağıdaki önemli noktalar anlamanız gerekir:

- SAP uygulama katmanı ve SAP olmayan uygulamalar her zamanki Azure donanım damga olarak barındırılan sanal makineleri çalıştırın.
- Müşteri şirket altyapısı, veri merkezleri ve uygulama dağıtımları (önerilen) ExpressRoute aracılığıyla bulut platformu veya sanal özel ağ (VPN) bağlı. Ayrıca Active Directory ve DNS Azure'a genişletilir.
- SAP HANA veritabanı örneği HANA iş yükü için SAP HANA (büyük örnekler) Azure üzerinde çalışır. Vm'lerde çalışan yazılım HANA büyük örneği çalıştıran HANA örneği ile etkileşim kurabilmesi büyük örneği damgasında Azure ağı ile bağlı.
- Donanım (büyük örnekler) Azure üzerinde SAP HANA'ın bir Iaas ile SUSE Linux Enterprise Server ya da Red Hat Enterprise Linux makineleridir sağlanan adanmış donanım ' dir. Sanal makineler gibi daha fazla güncelleştirmeler ve Bakım işletim sistemi için olduğundan sizin sorumluluğunuzdadır.
- HANA veya SAP HANA HANA büyük örneği birimlerde çalıştırmak için gereken tüm ek bileşenleri sizin sorumluluğunuzdur. Tüm ilgili devam eden işlemler ve Azure üzerinde SAP HANA yönetimini de sizin sorumluluğunuzdadır.
- Burada açıklanan çözümlerinin yanı sıra, Azure aboneliğinizdeki SAP HANA (büyük örnekler) azure'da bağlandığı diğer bileşenleri yükleyebilirsiniz. İletişimi ile veya doğrudan SAP HANA etkinleştiren bileşenleri, atlama sunucuları gibi RDP sunucuları, SAP HANA Studio SAP BI senaryoları için SAP Data Services veritabanı ya da ağ izleme çözümleri verilebilir.
- Azure olduğu gibi HANA büyük örneği, yüksek kullanılabilirlik ve olağanüstü durum kurtarma işlevleri için destek sunar.

**Sonraki adımlar**
- Başvuru [HLI için kullanılabilir SKU'ları](hana-available-skus.md) 