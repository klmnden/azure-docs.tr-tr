---
title: "Azure Site Recovery kullanarak çoğaltma Azure için Önkoşullar | Microsoft Docs"
description: "Bu makalede Azure Site Recovery hizmetini kullanarak Azure'a sanal makineleri ve fiziksel makineleri çoğaltma için önkoşullar özetlenmektedir."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/01/2017
ms.author: rajanaki
ms.openlocfilehash: fb5b8c9ac96ac44d0112919664a177f33ef392da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
#  <a name="prerequisites-for-replicating-azure-virtual-machines-to-another-region-by-using-azure-site-recovery"></a>Azure Site Recovery kullanarak başka bir bölgeye Azure sanal makineleri çoğaltmak için Önkoşullar

> [!div class="op_single_selector"]
> * [Azure'dan azure'a](site-recovery-azure-to-azure-prereq.md)
> * [Şirket içinden azure'a](site-recovery-prereq.md)

Azure Site Recovery hizmeti, düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur:
* Azure sanal makineleri başka bir Azure bölgesine çoğaltma.
* Şirket içi fiziksel sunucuların ve sanal makineleri Azure'a veya ikincil veri merkezine çoğaltma. 

Kesinti birincil konumunuzda meydana, üzerinden uygulamaları ve iş yüklerini kullanılabilir durumda tutmak için ikincil bir konuma başarısız olabilir. Normal çalışma dönüldüğünde birincil konumunuz başarısız olabilir. Site kurtarma hakkında daha fazla bilgi için bkz: [Site Recovery nedir?](site-recovery-overview.md).

Bu makalede, Azure Site Recovery çoğaltma şirket içi başlamak için gerekli önkoşullar özetlenmektedir.

Makalenin sonundaki yorumları gönderin ya da teknik sorular [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="azure-requirements"></a>Azure gereksinimleri

**Gereksinim** | **Ayrıntılar**
--- | ---
**Azure hesabı** | A [Microsoft Azure](http://azure.microsoft.com/) hesabı.<br/><br/> [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
**Site Recovery hizmeti** | Site Recovery fiyatlandırması hakkında daha fazla bilgi için bkz: [Site Recovery fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/). Bir olağanüstü durum kurtarma konumu olarak kullanmak istediğiniz Azure bölgesini hedef bir kurtarma Hizmetleri kasası oluşturmanızı öneririz. Örneğin, kaynak VM'ler Doğu ABD çalıştıran ve orta ABD çoğaltmak istediğiniz, merkezi ABD'de kasası oluşturma öneririz.|
**Azure kapasite** | Olağanüstü durum kurtarma konumu olarak kullanmak istediğiniz Azure bölgesini hedefi için bir abonelik sanal makineler, depolama hesapları ve ağ bileşenleri için yeterli kapasiteye sahip olması gerekir. Daha fazla Kapasite eklemek için Destek'e başvurabilirsiniz.
**Depolama Kılavuzu** | Uygun olduğundan emin olmak [depolama Kılavuzu](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) kaynağınız Azure için tüm performans sorunlarını önlemek için sanal makineler. Varsayılan ayarları izlerseniz, Site Recovery kaynak yapılandırmasını temel alarak gerekli depolama hesapları oluşturur. Özelleştirme ve kendi ayarlarınızı seçerseniz, uygun olduğundan emin olmak [sanal makine disklerini için ölçeklenebilirlik hedefleri](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Ağ Kılavuzu** | Belirli URL veya IP aralıkları için beyaz liste ile Azure sanal makineden giden bağlantı gerekir. Daha fazla ayrıntı için başvurmak [Azure sanal makineleri çoğaltmak için kılavuz ağ](site-recovery-azure-to-azure-networking-guidance.md) makalesi.
**Azure VM** | Tüm son kök sertifikaları Windows veya Linux VM üzerinde mevcut olduğundan emin olun. VM son kök sertifikaları mevcut değilse, Site kurtarma işlemi güvenlik kısıtlamaları nedeniyle kaydedilemedi.

>[!NOTE]
>Belirli yapılandırmalar için desteği hakkında daha fazla ayrıntı için okuma [destek matrisi](site-recovery-support-matrix-azure-to-azure.md).

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [Azure sanal makineleri çoğaltmak için kılavuz ağ](site-recovery-azure-to-azure-networking-guidance.md).
- İş yükleri tarafından korumaya başlamak [Azure sanal makineleri çoğaltmak](site-recovery-azure-to-azure.md).
