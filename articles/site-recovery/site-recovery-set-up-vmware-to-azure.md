---
title: "Kaynak ortamı (VMware Azure) ayarlama | Microsoft Docs"
description: "Bu makalede, VMware sanal makinelerini Azure'a çoğaltma başlatmak için şirket içi ortamınızı ayarlayın açıklar."
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 02/18/2018
ms.author: anoopkv
ms.openlocfilehash: ff927a4846ba63d3f00d0e81b8cb818af1441449
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a>Kaynak ortamı (VMware Azure) ayarlama
> [!div class="op_single_selector"]
> * [Vmware’den Azure’a](./site-recovery-set-up-vmware-to-azure.md)
> * [Azure için fiziksel](./site-recovery-set-up-physical-to-azure.md)

Bu makalede, Azure'da VMware üzerinden çalışan sanal makineleri çoğaltmak için kaynağını, şirket içi ortamına Ayarla açıklar. Otomatik bulma VM'ler şirket içi ve şirket içi makineyi Site Recovery yapılandırma sunucusu olarak ayarlama, çoğaltma senaryonuzun seçmek için adımları içerir. 

## <a name="prerequisites"></a>Önkoşullar

Makale, zaten sahip olduğunuz varsayılmaktadır:
- [Kaynakları ayarlamak](tutorial-prepare-azure.md) içinde [Azure portal](http://portal.azure.com).
- [Şirket içi VMware ayarlamak](tutorial-prepare-on-premises-vmware.md), otomatik bulma için adanmış bir hesap da dahil olmak üzere.



## <a name="choose-your-protection-goals"></a>Koruma hedeflerinizi seçme

1. Azure portalında Git **kurtarma Hizmetleri** kasa dikey ve kasanızı seçin.
2. Kasa kaynak menüsünde Git **Başlarken** > **Site Recovery** > **1. adım: altyapıyı hazırlama**  >  **Koruma hedefi**.

    ![Hedefleri seçme](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. İçinde **koruma hedefi**seçin **için Azure**ve seçin **Evet, VMware vSphere hiper yönetici ile**. Daha sonra, **Tamam**'a tıklayın.

    ![Hedefleri seçme](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>Yapılandırma sunucusu kurma

Yapılandırma sunucusunu bir şirket içi VMware VM ayarlama, açık sanallaştırma biçimi (OVF) şablonunu kullanın. [Daha fazla bilgi edinin](concepts-vmware-to-azure-architecture.md) VMware VM üzerinden yüklenecek bileşenleri hakkında. 

1. Hakkında bilgi edinin [Önkoşullar](how-to-deploy-configuration-server.md#prerequisites) yapılandırma sunucusu dağıtımı için. [Kapasite numaraları denetleyin](how-to-deploy-configuration-server.md#capacity-planning) dağıtımı için.
2. [Karşıdan](how-to-deploy-configuration-server.md#download-the-template) ve [alma](how-to-deploy-configuration-server.md#import-the-template-in-vmware) şirket içi VMware yapılandırma sunucusu çalıştıran VM ayarlamak için OVF şablonu (how-to-dağıtma-yapılandırma-server.md).
3. VMware VM üzerinden etkinleştirmek ve [kaydetmek](how-to-deploy-configuration-server.md#register-the-configuration-server) kurtarma Hizmetleri kasası.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>Otomatik bulma için VMware hesabı ekleyin

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>VMware sunucuya bağlanın

Azure Site Recovery, şirket içi ortamınızda çalışan sanal makineleri bulmak izin vermek için VMware vCenter Server veya vSphere ESXi konakları Site Recovery ile bağlanmanız gerekir.

Seçin **+ vCenter** bir VMware vCenter sunucusu veya bir VMware vSphere ESXi konağı bağlanma başlatmak için.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Genel sorunlar
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Sonraki adımlar
[Hedef ortamınızı ayarlama](./site-recovery-prepare-target-vmware-to-azure.md) azure'da.
