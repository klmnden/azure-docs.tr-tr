---
title: Azure Site Recovery ile VMware çoğaltma için çoğaltma ilkelerini yapılandırmanıza ve yönetmenize | Microsoft Docs
description: Azure Site Recovery ile Azure'a VMware çoğaltması için çoğaltma ayarlarını yapılandırmak açıklar.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: sutalasi
ms.openlocfilehash: 115f4e76630a0ecd659fdd3aec0c6aa67d503817
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920496"
---
# <a name="configure-and-manage-replication-policies-for-vmware-replication"></a>Yapılandırma ve Vmware'e çoğaltma için çoğaltma ilkeleri yönetme
Bu makalede, Azure'da VMware Vm'lerini olduğunuz çoğalttığınızda, çoğaltma ilkesi yapılandırma kullanarak [Azure Site Recovery](site-recovery-overview.md).


## <a name="create-a-policy"></a>İlke oluştur

1. **Yönet** > **Site Recovery Altyapısı**’nı seçin.
2. İçinde **VMware ve fiziksel makineler**seçin **çoğaltma ilkeleri**. 
3. Tıklayın **+ Çoğaltma İlkesi**, ilke adı belirtin.
5. **RPO eşiği** bölümünde RPO limitini belirtin. Sürekli çoğaltma Bu limiti aşarsa uyarılar oluşturulur.
6. **Kurtarma noktası bekletme** kısmında, her kurtarma noktası için bekletme süresini saat cinsinden belirtin. Korumalı makineler, bekletme penceresi içindeki herhangi bir noktaya kurtarılabilir. Premium depolama alanına çoğaltılan makineler için 24 saate kadar bekletme desteklenir. 72 saat için standart depolama desteklenir.
7. **Uygulamayla tutarlı anlık görüntü sıklığı** kısmında, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının hangi sıklıkta oluşturulacağını (dakika cinsinden) belirtin.
8. **Tamam**’a tıklayın. İlke, 30 ila 60 saniye içinde oluşturulur.

Bir çoğaltma ilkesi oluşturduğunuzda, eşleşen bir yeniden çalışma çoğaltma ilkesi otomatik olarak, "yeniden çalışma" soneki ile oluşturulur. İlkeyi oluşturduktan sonra bunu seçerek düzenleyebilirsiniz > **ayarlarını Düzenle**.

## <a name="associate-a-configuration-server"></a>Yapılandırma sunucusunu ilişkilendirme 

Çoğaltma İlkesi, şirket içi yapılandırma sunucusu ile ilişkilendirin.

1. Tıklayın **ilişkilendirmek**ve yapılandırma sunucusunu seçin.

    ![Yapılandırma sunucusunu ilişkilendirme](./media/vmware-azure-set-up-replication/associate1.png)

2. **Tamam**’a tıklayın. Yapılandırma sunucusu, bir ila iki dakika içinde ilişkilendirilir.

    ![Yapılandırma sunucusu ilişkilendirme](./media/vmware-azure-set-up-replication/associate2.png)


## <a name="disassociate-or-delete-a-replication-policy"></a>İlişkisini kaldırın veya bir çoğaltma ilkesini silme
1. Çoğaltma ilkesini seçin.
    a. Yapılandırma sunucusundan ilkesinin ilişkisini kaldırmak çoğaltılan hiçbir Makine ilkesi kullandığınızdan emin olun. ' A tıklayarak **Bulutlarından**.
    b. İlkeyi silmek için onu bir yapılandırma sunucusu ile ilişkili olmayan emin olun. ' A tıklayarak **Sil**. Bu, silmek için 30-60 saniye sürer.
2. **Tamam**’a tıklayın.
