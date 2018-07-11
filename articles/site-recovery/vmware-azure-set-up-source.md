---
title: Azure Site Recovery ile Azure'a çoğaltma VMware için kaynak ortamı ayarlama | Microsoft Docs
description: Bu makalede, azure'da Azure Site Recovery ile VMware Vm'lerini çoğaltmak için şirket içi ortamınızı ayarlama açıklar.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ramamill
ms.openlocfilehash: 1380c1bc820a815fae317a86fcd0ee4f46dd9aa5
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952663"
---
# <a name="set-up-the-source-environment-for-vmware-to-azure-replication"></a>Azure'a çoğaltma için VMware için kaynak ortamı ayarlama

Bu makalede, VMware Vm'lerini Azure'a çoğaltma için kaynak şirket içi ortamı ayarlamak açıklar. Otomatik olarak keşfetme şirket içi Vm'leri ve bir şirket içi makine Site Recovery yapılandırma sunucusu olarak ayarlama, çoğaltma senaryosu seçmeye yönelik adımlar içerir. 

## <a name="prerequisites"></a>Önkoşullar

Sahip olduğunuz varsayılır:
- [Kaynaklarını ayarlama](tutorial-prepare-azure.md) içinde [Azure portalında](http://portal.azure.com).
- [Şirket içi VMware ayarlama](vmware-azure-tutorial-prepare-on-premises.md), otomatik bulma için adanmış bir hesap da dahil olmak üzere.



## <a name="choose-your-protection-goals"></a>Koruma hedeflerinizi seçme

1. Azure portalında Git **kurtarma Hizmetleri** kasası dikey penceresinin ve kasanızı seçin.
2. Kasa kaynak menüsünde Git **Başlarken** > **Site Recovery** > **1. adım: altyapıyı hazırlama**  >  **Koruma hedefi**.

    ![Hedefleri seçme](./media/vmware-azure-set-up-source/choose-goals.png)
3. İçinde **koruma hedefi**seçin **Azure'a**ve **Evet, VMware vSphere Hypervisor ile**. Daha sonra, **Tamam**'a tıklayın.

    ![Hedefleri seçme](./media/vmware-azure-set-up-source/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>Yapılandırma sunucusunu ayarlayın

Yapılandırma sunucusunu bir açık sanallaştırma uygulama (OVA) şablonu aracılığıyla şirket içi VMware VM ayarlayabilirsiniz. [Daha fazla bilgi edinin](concepts-vmware-to-azure-architecture.md) VMware VM'de yüklü bileşenler hakkında.

1. Hakkında bilgi edinin [önkoşulları](vmware-azure-deploy-configuration-server.md#prerequisites) yapılandırma sunucusu dağıtımı için.
2. [Kapasite rakamlarının denetleyin](vmware-azure-deploy-configuration-server.md#capacity-planning) dağıtımı için.
3. [İndirme](vmware-azure-deploy-configuration-server.md#download-the-template) ve [alma](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) OVA şablonu şirket içi VMware yapılandırma sunucusu çalıştıran bir VM kümesi. Şablonu ile sağlanan lisans, bir değerlendirme lisans olduğunu ve 180 gün boyunca geçerli olur. POST bu dönem, tedarik edilen lisans ile windows etkinleştirmek müşteri gerekir.
4. VMware VM kapatma ve [kaydetmek](vmware-azure-deploy-configuration-server.md#register-the-configuration-server-with-azure-site-recovery-services) kurtarma Hizmetleri kasası.


## <a name="add-the-vmware-account-for-automatic-discovery"></a>Otomatik bulma için bir VMware hesabı Ekle

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>VMware sunucusuna

Azure Site Recovery, şirket içi ortamınızda çalışan sanal makineleri bulmak izin vermek için Site Recovery ile VMware vCenter sunucusunun veya vSphere ESXi konakları bağlanmanız gerekir.

Seçin **+ vCenter** bir VMware vCenter server veya VMware vSphere ESXi konağına bağlanmak başlatmak için.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Genel sorunlar
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Sonraki adımlar
[Hedef ortamınızı](./vmware-azure-set-up-target.md) azure'da.
