---
title: Microsoft Azure üzerinde Oracle çözümleri | Microsoft Docs
description: Oracle uygulamaları ve çözümleri tamamen Azure altyapısı üzerinde çalışan veya Oracle bulut altyapısı (OCI) Bulutlar arası bağlantı kullanarak da dahil olmak üzere Microsoft Azure üzerinde dağıtmak için seçenekler hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/04/2019
ms.author: rogirdh
ms.openlocfilehash: b8bfa0dfa82f73cad010a608150eac48c7f3d4c8
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707442"
---
# <a name="overview-of-oracle-applications-and-solutions-on-azure"></a>Oracle uygulamaları ve çözümleri azure'da genel bakış

Bu makalede, Azure altyapısı kullanılarak Oracle çözümleri çalıştırmak için özellikler sunar. Ayrıntılı tanıtımlar kullanılabilir için Ayrıca bkz: [Oracle VM görüntüsü](oracle-vm-solutions.md) Azure marketi, önizleme özelliği [Azure Oracle bulut altyapısı (OCI) ile bağlantı](oracle-oci-overview.md).

## <a name="oracle-databases-on-azure-infrastructure"></a>Oracle veritabanlarının Azure altyapı

Oracle veritabanları Azure Market'teki Linux görüntüleri kullanılabilir kullanarak Azure altyapısı üzerinde çalıştırın:

* Oracle veritabanı 12,1 12.2 ve 18.3 Kurumsal sürüm 

* Oracle veritabanı 12,1 12.2 ve 18.3 standart sürümü 

Ayrıca, azure'da sıfırdan oluşturun veya şirket içi ortamınızdan özel görüntü karşıya özel bir görüntü için bir çözüm temel de seçebilirsiniz.

İsteğe bağlı olarak birden fazla bağlı diskleri olan yapılandırmak ve veritabanı performansını Oracle otomatik Depolama Yönetimi (ASM) yükleyerek.

## <a name="applications-on-oracle-linux-and-weblogic-server"></a>Oracle Linux ve WebLogic Server uygulamaları

Kurumsal uygulamaları Azure'da desteklenen Oracle işletim sistemlerinde çalışır. Aşağıdaki görüntüleri Azure Marketi'nde mevcuttur:

* Oracle WebLogic Server 12.1.2

* Oracle Linux (UEK) 6,8, 6.9, 6.10, 7.3, 7.4, 7.5 ve 7.6

## <a name="high-availability-and-disaster-recovery-options"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma seçenekleri

* İle birlikte Azure altyapısı üzerinde Oracle Data Guard, etkin Data Guard veya Goldengate'i yapılandırma [kullanılabilirlik](../../../availability-zones/az-overview.md) yüksek kullanılabilirlik için.

* Kullanım [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md) düzenlemek ve Oracle Linux Vm'lerinizi Azure ve şirket içinde veya fiziksel sunucular için olağanüstü durum kurtarma yönetimi için. 

* Oracle gerçek uygulama kümeleri (RAC) kullanarak azure'da etkinleştirme [FlashGrid SkyCluster](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="integration-of-azure-with-oci-preview"></a>Azure OCI (Önizleme) ile tümleştirme

Arka uç veritabanları Oracle bulut altyapısı (OCI) bağlı Azure altyapısı, Oracle uygulamaları çalışır. Bu çözüm, aşağıdaki özellikleri kullanır: 

* **Bulutlar arası ağlar** -kullanım doğrudan bağlantı uygulama ve veritabanı katmanı arasında yüksek bant genişlikli, özel ve düşük gecikmeli bağlantılar kurmak için Azure ExpressRoute ve Oracle FastConnect arasında kullanılabilir.
* **Tümleşik kimlik** -Azure AD arasında federasyon kimlik ayarlamak ve Oracle çözümleri için bir tek kimlik kaynağı IDCS. Tek OCI ve Azure'da kaynakları yönetmek için oturum açmayı etkinleştirin.

### <a name="deploy-oracle-applications-on-azure"></a>Azure'da Oracle uygulamaları dağıtma

Terraform şablonları Azure altyapısını kurma ve Bulutlar arası yapılandırmasında çalıştırılacak doğrulandı ve desteklenen Oracle uygulamaları yüklemek için kullanın:

* E-Business Suite
* JD Edwards EnterpriseOne
* PeopleSoft
* Oracle perakende uygulamaları
* Oracle Hyperion Finans Yönetimi

Ayrıca OCI ve diğer Azure Hizmetleri ile bağlanan özel uygulamaları azure'da dağıtın.

### <a name="set-up-oracle-databases-in-oci"></a>Oracle veritabanlarında OCI ayarlama

Oracle veritabanı bulut Hizmetleri (otonom veritabanı, RAC, Exadata, DBaaS, tek düğümlü), Azure'da çalıştırılan Oracle uygulamaları ile birlikte kullanın. Daha fazla bilgi edinin [OCI veritabanı seçenekleri](https://docs.cloud.oracle.com/iaas/Content/Database/Concepts/databaseoverview.htm). 
 

## <a name="licensing"></a>Lisanslama

Azure'da Oracle uygulamalarının dağıtımını, bir "kendi lisansını getir" modelini temel alıyor. Oracle yazılımı ve Oracle ile yerinde geçerli bir destek sözleşmesi olması kullanmak için düzgün şekilde lisanslandığından varsayılır. Oracle lisans taşınabilirliği şirket içinden Azure'a garantisi. Oracle Azure bkz [SSS](https://www.oracle.com/cloud/technologies/oracle-azure-faq.html).

## <a name="next-steps"></a>Sonraki adımlar

* Dağıtma hakkında daha fazla bilgi [Oracle VM görüntüsü](oracle-vm-solutions.md) Azure altyapı.

* Kullanma hakkında daha fazla bilgi edinin [OCI Azure'la bağlantı](oracle-oci-overview.md).