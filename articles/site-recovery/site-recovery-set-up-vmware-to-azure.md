---
title: "Kaynak ortamı (VMware Azure) ayarlama | Microsoft Docs"
description: "Bu makalede, VMware sanal makinelerini Azure'a çoğaltma başlatmak için şirket içi ortamınızı ayarlayın açıklar."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/23/2017
ms.author: anoopkv
ms.openlocfilehash: 32a3f7498d3c8891178818436e33221f91ae2f8f
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a>Kaynak ortamı (VMware Azure) ayarlama
> [!div class="op_single_selector"]
> * [Vmware’den Azure’a](./site-recovery-set-up-vmware-to-azure.md)
> * [Azure için fiziksel](./site-recovery-set-up-physical-to-azure.md)

Bu makalede, Azure'da VMware üzerinden çalışan sanal makineleri çoğaltma işlemi başlatma için şirket içi ortamınızı ayarlayın açıklar.

## <a name="prerequisites"></a>Ön koşullar

Makale, zaten oluşturduğunuzu varsayar:
- Bir kurtarma Hizmetleri kasasına [Azure portal](http://portal.azure.com "Azure portal").
- Adanmış bir hesap için kullanılan, VMware vcenter [otomatik bulma](./site-recovery-vmware-to-azure.md).
- Bir sanal makine yapılandırma sunucusuna yüklemek için.

## <a name="configuration-server-minimum-requirements"></a>Yapılandırma sunucusunun en düşük gereksinimleri
Aşağıdaki tabloda, en düşük donanım, yazılım ve yapılandırma sunucusu için ağ gereksinimleri listelenmektedir.

> [!IMPORTANT]
> VMware sanal makineleri korumak için bir yapılandırma sunucusu dağıtımı sırasında olarak dağıtmanızı öneririz bir **yüksek oranda kullanılabilir (HA)** sanal makine.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> HTTPS tabanlı proxy sunucuları yapılandırma sunucusu tarafından desteklenmez.

## <a name="choose-your-protection-goals"></a>Koruma hedeflerinizi seçme

1. Azure portalında Git **kurtarma Hizmetleri** kasa dikey ve kasanızı seçin.
2. Kasa kaynak menüsünde Git **Başlarken** > **Site Recovery** > **1. adım: altyapıyı hazırlama**  >  **Koruma hedefi**.

    ![Hedefleri seçme](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. İçinde **koruma hedefi**seçin **için Azure**ve seçin **Evet, VMware vSphere hiper yönetici ile**. Daha sonra, **Tamam**'a tıklayın.

    ![Hedefleri seçme](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama
Kaynak ortamını ayarlama iki ana etkinlik içerir:

- Yükleme ve yapılandırma sunucusu Site Kurtarma ile kaydedin.
- Şirket içi sanal makinelerinizi şirket içi VMware vCenter veya vSphere EXSi konaklarınızın için Site Recovery bağlanarak bulur.

### <a name="step-1-install-and-register-a-configuration-server"></a>Adım 1: Yükleme ve yapılandırma sunucusuna kaydedin

1. Tıklatın **1. adım: altyapıyı hazırlama** > **kaynak**. İçinde **kaynağı**,'ı tıklatın, bir yapılandırma sunucusu yoksa **+ yapılandırma sunucusu** eklemesini.

    ![Kaynağı ayarlama](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. Üzerinde **Sunucu Ekle** dikey penceresinde denetleyin **yapılandırma sunucusu** görünür **sunucu türü**.
4. Site Recovery birleşik Kurulumu yükleme dosyasını indirin.
5. Kasa kayıt anahtarını indir Birleşik Kurulum'u çalıştırdığınızda, kayıt anahtarı gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Kaynağı ayarlama](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. Yapılandırma sunucusu olarak kullandığınız makine üzerinde çalışan **Azure Site Recovery birleşik Kurulumu** yapılandırma sunucusuna işlem sunucusu ve ana hedef sunucusu yüklemek için.

#### <a name="run-azure-site-recovery-unified-setup"></a>Kurulumu çalıştırma Azure Site Recovery birleşik

> [!TIP]
> Bilgisayarınızın sistem saati beş dakikadan tarafından yerel saatten farklıysa yapılandırma sunucusu kaydı başarısız olur. Sistem saatinizi bir [saat sunucusu](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) yüklemeyi başlatmadan önce.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Yapılandırma sunucusu komut satırı üzerinden yüklenebilir. Daha fazla bilgi için bkz: [komut satırı araçlarını kullanarak yapılandırma sunucusu yükleme](http://aka.ms/installconfigsrv).

#### <a name="add-the-vmware-account-for-automatic-discovery"></a>Otomatik bulma için VMware hesabı ekleyin

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a>2. adım: bir vCenter ekleme
Azure Site Recovery, şirket içi ortamınızda çalışan sanal makineleri bulmak izin vermek için VMware vCenter Server veya vSphere ESXi konakları Site Recovery ile bağlanmanız gerekir.

Seçin **+ vCenter** bir VMware vCenter sunucusu veya bir VMware vSphere ESXi konağı bağlanma başlatmak için.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Genel sorunlar
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Sonraki adımlar
[Hedef ortamınızı ayarlama](./site-recovery-prepare-target-vmware-to-azure.md) azure'da.
