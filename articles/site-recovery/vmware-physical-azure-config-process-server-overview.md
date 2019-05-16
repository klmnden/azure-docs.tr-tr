---
title: Azure Site Recovery yapılandırması, işlem ve ana hedef sunucuları | Microsoft Docs
description: Bu makalede yapılandırma, işlem ve ana hedef sunucuları Azure Site Recovery ile şirket içi VMware vm'lerinin olağanüstü durum kurtarmayı ayarlarken kullanarak genel bir bakış sağlar.
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: raynew
ms.openlocfilehash: 6f501e251f0b006bbbb4f64090cac5c3d61b7bf2
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523556"
---
# <a name="about-site-recovery-components-configuration-process-master-target"></a>Site Recovery bileşenleri hakkında (yapılandırma, işlem, ana hedef)

Bu makalede yapılandırma, işlem ve ana hedef sunucuları VMware Vm'lerini ve fiziksel sunucuları azure'a çoğaltırken kullanılan [Site Recovery](site-recovery-overview.md) hizmeti.

## <a name="configuration-server"></a>Yapılandırma sunucusu

Şirket içi VMware Vm'leri ve fiziksel sunucuları olağanüstü durum kurtarma için bir Site kurtarması gereken şirket içi yapılandırma sunucusu dağıttınız.

**Ayar** | **Ayrıntılar** | **Bağlantılar**
--- | --- | ---
**Bileşenleri**  | Configuration server makinesi yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucusu içeren tüm şirket içi Site Recovery bileşenlerini çalıştırır.<br/><br/> Yapılandırma sunucusunu ayarlayın, tüm bileşenleri otomatik olarak yüklenir. | [Okuma](vmware-azure-common-questions.md#configuration-server) yapılandırma sunucusu ile ilgili SSS.
**Rol** | Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir. | Mimarisi hakkında daha fazla bilgi [VMware](vmware-azure-architecture.md) ve [fiziksel sunucu](physical-azure-architecture.md) azure'a olağanüstü durum kurtarma.
**VMware gereksinimlerini** | Şirket içi VMware vm'lerinin olağanüstü durum kurtarma için yükleme ve yapılandırma sunucusuna bir şirket içi, yüksek oranda kullanılabilir bir VMware sanal makinesi çalıştırın. | [Hakkında bilgi edinin](vmware-azure-deploy-configuration-server.md#prerequisites) önkoşulları.
**VMware dağıtım** | İndirilen OVA şablon kullanarak yapılandırma sunucusu dağıtmanızı öneririz. Bu yöntem sağlayan bir yalnızca tüm gereksinimleri ve önkoşullar uyumlu bir yapılandırma sunucusu kurma olanağı.<br/><br/> OVA şablonu kullanarak bir VMware VM dağıtmak için herhangi bir nedenden dolayı yapılandırma sunucusu makineleri fiziksel makine olağanüstü durum kurtarma için aşağıda açıklandığı gibi el ile ayarlayabilirsiniz. | [Dağıtma](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template) OVA şablonuyla.
**Fiziksel sunucu gereksinimleri** | Şirket içi fiziksel sunucularda olağanüstü durum kurtarma için yapılandırma sunucusunu el ile dağıtın. | [Hakkında bilgi edinin](physical-azure-set-up-source.md#prerequisites) önkoşulları.
**Fiziksel sunucu dağıtımı** | VMware VM olarak yüklenemez, bir fiziksel sunucuya yükleyebilirsiniz. | [Dağıtma](physical-azure-set-up-source.md#set-up-the-source-environment) yapılandırma sunucusunu el ile.


## <a name="process-server"></a>İşlem sunucusu

**Ayar** | **Ayrıntılar** | **Bağlantılar**
--- | --- | ---
**Dağıtım**  | Olağanüstü durum kurtarma ve şirket içi VMware Vm'leri ve fiziksel sunucuları çoğaltma için bir işlem sunucusu gerek şirket içinde. Dağıtım sırasında varsayılan olarak, işlem sunucusu yapılandırma sunucusuna yüklenir. | [Daha fazla bilgi edinin](vmware-azure-architecture.md?#architectural-components).
**(Şirket içi rolü** | -Çoğaltma için etkinleştirilmiş makinelerden çoğaltma verilerini alır.<br/> -Çoğaltma verilerini önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.<br/> -Şirket içi VMware Vm'leri ve fiziksel sunucuları çoğaltmak istediğiniz Site Recovery Mobility hizmetinin göndererek yükleme gerçekleştirir.<br/> -Şirket içi makineleri otomatik olarak bulunmasını gerçekleştirir. | [Daha fazla bilgi edinin](vmware-physical-azure-config-process-server-overview.md#process-server). 
**Rol (azure'dan yeniden çalışma)** | Şirket içi sitenizden yük devretme işleminden sonra bir işlem sunucusu, azure'da bir Azure VM'i olarak şirket içi konumunuza geri dönme işlemek için ayarlayın.<br/><br/> Azure'da işlem sunucusu geçicidir. Azure VM, yeniden çalışma gerçekleştirdikten sonra silinebilir. | [Daha fazla bilgi edinin](vmware-azure-set-up-process-server-azure.md).
**Ölçeklendirme** | Daha büyük dağıtımlar için ek, genişleme işlem sunucusu kurabilirsiniz şirket içi. Makineler ve daha büyük çoğaltma trafiği hacimlerini çoğaltmak daha büyük sayılar işleme göre kapasite ölçeğinizi ek sunucular.<br/><br/> Makineleri çoğaltma trafiğin yükünü dengelemek için işlem, iki sunucu arasında taşıyabilirsiniz. | [Daha fazla bilgi edinin](vmware-azure-set-up-process-server-scale.md),


## <a name="master-target-server"></a>Ana hedef sunucusu

Ana hedef sunucu, Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

- Varsayılan olarak yapılandırma sunucusuna yüklenir.
- Büyük dağıtımlar için yeniden çalışma için bir ek, ayrı bir ana hedef sunucusu ekleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
- Gözden geçirme [mimarisi](vmware-azure-architecture.md) VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma için.
- Gözden geçirme [gereksinimler ve Önkoşullar](vmware-physical-azure-support-matrix.md) VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma için. 
