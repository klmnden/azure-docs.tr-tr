---
title: Azure Site Recovery ile Azure çoğaltma için VMware kaynak ortamını ayarlama | Microsoft Docs
description: Bu makalede, Azure Site Recovery ile azure'a VMware sanal makineleri çoğaltmak için şirket içi ortamınızı ayarlayın açıklar.
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: b2c564e8d49e39d9cdc09d3fe168388d579de70e
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
ms.locfileid: "29812659"
---
# <a name="set-up-the-source-environment-for-vmware-to-azure-replication"></a>Azure çoğaltma için VMware kaynak ortamını ayarlama

Bu makalede, VMware Vm'lerini Azure'a çoğaltma için kaynak şirket içi ortamınızı oluşturmak açıklar. Otomatik bulma VM'ler şirket içi ve şirket içi makineyi Site Recovery yapılandırma sunucusu olarak ayarlama, çoğaltma senaryonuzun seçmek için adımları içerir. 

## <a name="prerequisites"></a>Önkoşullar

Makale, zaten sahip olduğunuz varsayılmaktadır:
- [Kaynakları ayarlamak](tutorial-prepare-azure.md) içinde [Azure portal](http://portal.azure.com).
- [Şirket içi VMware ayarlamak](vmware-azure-tutorial-prepare-on-premises.md), otomatik bulma için adanmış bir hesap da dahil olmak üzere.



## <a name="choose-your-protection-goals"></a>Koruma hedeflerinizi seçme

1. Azure portalında Git **kurtarma Hizmetleri** kasa dikey ve kasanızı seçin.
2. Kasa kaynak menüsünde Git **Başlarken** > **Site Recovery** > **1. adım: altyapıyı hazırlama**  >  **Koruma hedefi**.

    ![Hedefleri seçme](./media/vmware-azure-set-up-source/choose-goals.png)
3. İçinde **koruma hedefi**seçin **için Azure**ve seçin **Evet, VMware vSphere hiper yönetici ile**. Daha sonra, **Tamam**'a tıklayın.

    ![Hedefleri seçme](./media/vmware-azure-set-up-source/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>Yapılandırma sunucusu kurma

Yapılandırma sunucusunu bir şirket içi VMware VM ayarlama, açık sanallaştırma biçimi (OVF) şablonunu kullanın. [Daha fazla bilgi edinin](concepts-vmware-to-azure-architecture.md) VMware VM üzerinden yüklenecek bileşenleri hakkında. 

1. Hakkında bilgi edinin [Önkoşullar](vmware-azure-deploy-configuration-server.md#prerequisites) yapılandırma sunucusu dağıtımı için.
2. [Kapasite numaraları denetleyin](vmware-azure-deploy-configuration-server.md#capacity-planning) dağıtımı için.
3. [Karşıdan](vmware-azure-deploy-configuration-server.md#download-the-template) ve [alma](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) şirket içi VMware yapılandırma sunucusu çalıştıran VM ayarlamak için OVF şablonu (how-to-dağıtma-yapılandırma-server.md).
4. VMware VM üzerinden etkinleştirmek ve [kaydetmek](vmware-azure-deploy-configuration-server.md#register-the-configuration-server) kurtarma Hizmetleri kasası.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>Otomatik bulma için VMware hesabı ekleyin

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>VMware sunucuya bağlanın

Azure Site Recovery, şirket içi ortamınızda çalışan sanal makineleri bulmak izin vermek için VMware vCenter Server veya vSphere ESXi konakları Site Recovery ile bağlanmanız gerekir.

Seçin **+ vCenter** bir VMware vCenter sunucusu veya bir VMware vSphere ESXi konağı bağlanma başlatmak için.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Genel sorunlar
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Sonraki adımlar
[Hedef ortamınızı ayarlama](./vmware-azure-set-up-target.md) azure'da.
