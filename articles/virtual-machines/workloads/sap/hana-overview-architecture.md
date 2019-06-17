---
title: SAP HANA (büyük örnekler) azure'da genel bakış | Microsoft Docs
description: SAP HANA (büyük örnekler) azure'da dağıtma genel bakış.
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
ms.openlocfilehash: c177dbad1145dee6eda3202d8076997cc7673dfc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60477809"
---
#  <a name="what-is-sap-hana-on-azure-large-instances"></a>Azure üzerinde SAP HANA (Büyük Örnekler) nedir?

SAP HANA (büyük örnekler) azure'da, Azure için benzersiz bir çözümdür. SAP HANA çalıştırmayı ve dağıtmak için sanal makineler sağlamanın yanı sıra Azure çalıştırın ve size atanmış çıplak sunucularda SAP HANA dağıtma olanağı sunar. SAP HANA (büyük örnekler) Azure çözümü, size atanan konak/sunucu paylaşılmayan çıplak bilgisayar donanım üzerine inşa edilmiştir. Sunucu donanımı işlem/sunucu, ağ ve depolama altyapısı içeren daha büyük damga olarak katıştırılır. Bir birleşimi buna sertifikalı uyarlanmış HANA veri merkezi tümleştirmesi (TDI) var. SAP HANA (büyük örnekler) Azure üzerinde farklı bir sunucuya SKU'ları veya boyutları sunar. Birimleri 36 Intel CPU Çekirdeği ve 768 GB bellek ve en fazla 480 Intel CPU çekirdeği olan birimleri adede kadar ve 24 Git olabilir TB bellekle.

Altyapı damga içindeki müşteri yalıtımı, kiracılar, benzer gerçekleştirilir:

- **Ağ**: Kiracı yalıtımı müşterilerin altyapı yığını aracılığıyla müşteri başına sanal ağ içinde atanır. Bir kiracı, tek bir müşteriye atanır. Bir müşteri, birden çok kiracıya sahip olabilir. Kiracılar aynı müşteriye ait olsa bile, kiracılar ağ yalıtımını altyapı damga düzeyi, kiracılar arasında ağ iletişimi engeller.
- **Depolama bileşenleri**: Atanmış depolama birimleri olan depolama sanal makineler için yalıtımı. Depolama birimleri, yalnızca bir depolama sanal makineye atanabilir. Bir depolama alanı sanal makine yalnızca tek bir kiracı SAP HANA TDI sertifikalı altyapı yığınında atanır. Sonuç olarak, bir depolama alanı sanal makineye atanan depolama birimleri, bir özel ve ilgili kiracıda yalnızca erişilebilir. Bunlar, farklı dağıtılan kiracılar arasında görünmez.
- **Sunucu veya konak**: Sunucu ya da konak birimi müşteriler veya kiracılar arasında paylaşılmaz. Bir sunucu veya dağıtılmış bir müşteri için ana, tek bir kiracı için atanmış bir atomik çıplak işlem birimi değil. *Hayır* bölümleme donanım veya yazılım bölümleme kullanılır, başka bir müşteri bir konak ya da bir sunucu paylaşımı içinde neden. Belirli bir kiracıda depolama sanal makineye atanan depolama birimleri, bu tür bir sunucuya bağlanır. Bir kiracı özel olarak atanan farklı SKU'ları çok sayıda sunucu ölçü birine sahip olabilir.
- İçinde bir SAP HANA (büyük örnekler) Azure altyapı damgası üzerinde birçok farklı kiracıların dağıtılır ve ağ, depolama ve işlem düzeyi Kiracı kavramları aracılığıyla birbirine karşı yalıtılmış. 


Bu tam sunucu birimleri, yalnızca SAP HANA çalıştırmayı desteklenir. SAP uygulama katmanında veya iş yükü donanımlar orta katman sanal makinelerde çalıştırır. SAP HANA (büyük örnekler) Azure birimlerde çalıştıran altyapı Damgalar için Azure Ağ Hizmetleri omurgalarından bağlanır. Bu şekilde, SAP HANA (büyük örnekler) Azure birimleri ve sanal makineler arasında düşük gecikme süreli bağlantı sağlanır.

Bu belgede, SAP HANA (büyük örnekler) azure'da kapsayan birkaç belge biridir. Bu belge, temel mimari, sorumlulukları ve çözüm tarafından sağlanan hizmetleri tanıtır. Çözüme üst düzey özellikleri de ele alınmıştır. Ağ ve bağlantı gibi birçok diğer alanlarda, dört belge detaya gitme bilgileri kapsar. Belgeleri (büyük örnekler) Azure üzerinde SAP hana, SAP NetWeaver yükleme yönlerini veya vm'lerde SAP NetWeaver dağıtımlarını ele alınmamıştır. Azure'da SAP NetWeaver aynı Azure belgeleri kapsayıcıda bulunan ayrı belgelerinde ele alınmıştır. 


Farklı belgeleri HANA büyük örneği kılavuzunun aşağıdaki alanları kapsar:

- [SAP HANA (büyük örnekler) genel bakış ve azure'da mimarisi](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Yükleme ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırma](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (büyük örnekler) sorun giderme ve Azure'da izleme](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [STONITH kullanarak yüksek kullanılabilirlik SUSE ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [Tip II SKU'lara yönelik işletim sistemi yedekleme ve geri yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

**Sonraki adımlar**
- Başvuru [koşulları bildirin](hana-know-terms.md)