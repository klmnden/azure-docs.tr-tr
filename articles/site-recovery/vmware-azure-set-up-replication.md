---
title: Azure Site Recovery ile VMware çoğaltma için çoğaltma ilkelerini yapılandırmanıza ve yönetmenize | Microsoft Docs
description: Azure Site Recovery ile Azure'da VMware çoğaltma işlemini çoğaltma ayarlarını yapılandırmak açıklar.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: sutalasi
ms.openlocfilehash: 6a8e6e7c6fbdbcbf58557a0896e976a608164041
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
ms.locfileid: "29812673"
---
# <a name="configure-and-manage-replication-policies-for-vmware-replication"></a>Yapılandırma ve VMware çoğaltma için çoğaltma ilkeleri yönetme
Bu makalede, Azure'da VMware Vm'lerini olduğunuz çoğalttığınızda, bir çoğaltma ilkesi yapılandırmak nasıl kullanarak [Azure Site Recovery](site-recovery-overview.md).


## <a name="create-a-policy"></a>Bir ilke oluşturun

1. **Yönet** > **Site Recovery Altyapısı**’nı seçin.
2. İçinde **için VMware ve fiziksel makineleri**seçin **çoğaltma ilkeleri**. 
3. Tıklatın **+ Çoğaltma İlkesi**, ilke adını belirtin.
5. **RPO eşiği** bölümünde RPO limitini belirtin. Sürekli çoğaltma bu sınırı aşarsa uyarıları üretilir.
6. **Kurtarma noktası bekletme** kısmında, her kurtarma noktası için bekletme süresini saat cinsinden belirtin. Korumalı makineler, bekletme penceresi içindeki herhangi bir noktaya kurtarılabilir. Premium depolama alanına çoğaltılan makineler için 24 saate kadar bekletme desteklenir. 72 saat kadar için standart depolama desteklenir.
7. **Uygulamayla tutarlı anlık görüntü sıklığı** kısmında, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının hangi sıklıkta oluşturulacağını (dakika cinsinden) belirtin.
8. **Tamam**’a tıklayın. İlke, 30 ila 60 saniye içinde oluşturulur.

Bir çoğaltma ilkesi oluşturduğunuzda, eşleşen bir yeniden çalışma çoğaltma ilkesi otomatik olarak, "geri dönme" soneki ile oluşturulur. İlke oluşturduktan sonra bunu seçerek düzenleyebilirsiniz > **ayarlarını Düzenle**.

## <a name="associate-a-configuration-server"></a>Yapılandırma sunucusu ilişkilendirme 

Çoğaltma İlkesi, şirket içi yapılandırma sunucusu ile ilişkilendirin.

1. Tıklatın **ilişkilendirmek**ve yapılandırma sunucusu seçin.

    ![Yapılandırma sunucusu ilişkilendirme](./media/vmware-azure-set-up-replication/associate1.png)

2. **Tamam**’a tıklayın. Yapılandırma sunucusu, bir ila iki dakika içinde ilişkilendirilir.

    ![Yapılandırma sunucusu ilişkilendirme](./media/vmware-azure-set-up-replication/associate2.png)


## <a name="disassociate-or-delete-a-replication-policy"></a>İlişkisini kaldırın veya bir çoğaltma ilkesi silme
1. Çoğaltma İlkesi seçin.
    a. Yapılandırma sunucusu ilkesinden ilişkilendirmesini kaldırmak çoğaltılan bir makine İlkesi kullandığınızdan emin olun. Ardından **ilişkiyi**.
    b. İlkesini silmek için bir yapılandırma sunucusu ile ilişkili olmayan emin olun. Ardından **silmek**. Silmek için 30-60 saniye sürer.
2. **Tamam**’a tıklayın.
