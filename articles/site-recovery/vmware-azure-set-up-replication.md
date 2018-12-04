---
title: Vmware'den azure'a Azure Site Recovery ile olağanüstü durum kurtarma için çoğaltma ilkelerini yapılandırmanıza ve yönetmenize | Microsoft Docs
description: Azure Site Recovery ile azure'a VMware olağanüstü durum kurtarma için çoğaltma ayarlarını yapılandırma açıklar.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: sutalasi
ms.openlocfilehash: e83b15b5e53a203a3583c02565ea9c316b7c481c
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52832390"
---
# <a name="configure-and-manage-replication-policies-for-vmware-disaster-recovery-to-azure"></a>Yapılandırma ve Azure'da VMware olağanüstü durum kurtarma için çoğaltma ilkeleri yönetme
Bu makalede, Azure'da VMware Vm'lerini olduğunuz çoğalttığınızda, çoğaltma ilkesi yapılandırma kullanarak [Azure Site Recovery](site-recovery-overview.md).


## <a name="create-a-policy"></a>İlke oluştur

1. **Yönet** > **Site Recovery Altyapısı**’nı seçin.
1. İçinde **VMware ve fiziksel makineler**seçin **çoğaltma ilkeleri**. 
1. Tıklayın **+ Çoğaltma İlkesi**, ilke adı belirtin.
1. **RPO eşiği** bölümünde RPO limitini belirtin. Sürekli çoğaltma Bu limiti aşarsa uyarılar oluşturulur.
1. **Kurtarma noktası bekletme** kısmında, her kurtarma noktası için bekletme süresini saat cinsinden belirtin. Korumalı makineler, bekletme penceresi içindeki herhangi bir noktaya kurtarılabilir. Premium depolama alanına çoğaltılan makineler için 24 saate kadar bekletme desteklenir. 72 saat için standart depolama desteklenir.
1. **Uygulamayla tutarlı anlık görüntü sıklığı** kısmında, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının hangi sıklıkta oluşturulacağını (dakika cinsinden) belirtin.
1. **Tamam** düğmesine tıklayın. İlke, 30 ila 60 saniye içinde oluşturulur.

Bir çoğaltma ilkesi oluşturduğunuzda, eşleşen bir yeniden çalışma çoğaltma ilkesi otomatik olarak, "yeniden çalışma" soneki ile oluşturulur. İlkeyi oluşturduktan sonra bunu seçerek düzenleyebilirsiniz > **ayarlarını Düzenle**.

## <a name="associate-a-configuration-server"></a>Yapılandırma sunucusunu ilişkilendirme 

Çoğaltma İlkesi, şirket içi yapılandırma sunucusu ile ilişkilendirin.

1. Tıklayın **ilişkilendirmek**ve yapılandırma sunucusunu seçin.

    ![Yapılandırma sunucusunu ilişkilendirme](./media/vmware-azure-set-up-replication/associate1.png)

1. **Tamam** düğmesine tıklayın. Yapılandırma sunucusu, bir ila iki dakika içinde ilişkilendirilir.

    ![Yapılandırma sunucusu ilişkilendirme](./media/vmware-azure-set-up-replication/associate2.png)


## <a name="disassociate-or-delete-a-replication-policy"></a>İlişkisini kaldırın veya bir çoğaltma ilkesini silme
1. Çoğaltma ilkesini seçin.
    a. Yapılandırma sunucusundan ilkesinin ilişkisini kaldırmak çoğaltılan hiçbir Makine ilkesi kullandığınızdan emin olun. ' A tıklayarak **Bulutlarından**.
    b. İlkeyi silmek için onu bir yapılandırma sunucusu ile ilişkili olmayan emin olun. ' A tıklayarak **Sil**. Bu, silmek için 30-60 saniye sürer.
1. **Tamam** düğmesine tıklayın.
