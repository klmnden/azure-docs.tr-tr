---
title: Vmware'den azure'a Azure Site Recovery ile olağanüstü durum kurtarma için çoğaltma ilkelerini yapılandırmanıza ve yönetmenize | Microsoft Docs
description: Azure Site Recovery ile azure'a VMware olağanüstü durum kurtarma için çoğaltma ayarlarını yapılandırma açıklar.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: sutalasi
ms.openlocfilehash: b60d8a8fb9b9300a6914ad33b2f760fb5adde3b4
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59278233"
---
# <a name="configure-and-manage-replication-policies-for-vmware-disaster-recovery-to-azure"></a>Yapılandırma ve Azure'da VMware olağanüstü durum kurtarma için çoğaltma ilkeleri yönetme
Bu makalede, Azure'da VMware Vm'lerini olduğunuz çoğalttığınızda, çoğaltma ilkesi yapılandırma kullanarak [Azure Site Recovery](site-recovery-overview.md).

## <a name="create-a-policy"></a>İlke oluştur

1. **Yönet** > **Site Recovery Altyapısı**’nı seçin.
2. İçinde **VMware ve fiziksel makineler**seçin **çoğaltma ilkeleri**.
3. Tıklayın **+ Çoğaltma İlkesi**, ilke adı belirtin.
4. **RPO eşiği** bölümünde RPO limitini belirtin. Sürekli çoğaltma Bu limiti aşarsa uyarılar oluşturulur.
5. **Kurtarma noktası bekletme** kısmında, her kurtarma noktası için bekletme süresini saat cinsinden belirtin. Korumalı makineler, bekletme penceresi içindeki herhangi bir noktaya kurtarılabilir. Premium depolama alanına çoğaltılan makineler için 24 saate kadar bekletme desteklenir. 72 saat için standart depolama desteklenir.
6. İçinde **uygulamayla tutarlı anlık görüntü sıklığı**, seçin, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının ne sıklıkta (saat cinsinden) açılan listeden oluşturulmalıdır. "Kapalı" değerine açılır uygulama tutarlılık noktası oluşturmayı devre dışı bırak istiyorsanız seçin.
7. **Tamam** düğmesine tıklayın. İlke, 30 ila 60 saniye içinde oluşturulur.

Bir çoğaltma ilkesi oluşturduğunuzda, eşleşen bir yeniden çalışma çoğaltma ilkesi otomatik olarak, "yeniden çalışma" soneki ile oluşturulur. İlkeyi oluşturduktan sonra bunu seçerek düzenleyebilirsiniz > **ayarlarını Düzenle**.

## <a name="associate-a-configuration-server"></a>Yapılandırma sunucusunu ilişkilendirme

Çoğaltma İlkesi, şirket içi yapılandırma sunucusu ile ilişkilendirin.

1. Tıklayın **ilişkilendirmek**ve yapılandırma sunucusunu seçin.

    ![Yapılandırma sunucusunu ilişkilendirme](./media/vmware-azure-set-up-replication/associate1.png)
2. **Tamam** düğmesine tıklayın. Yapılandırma sunucusu, bir ila iki dakika içinde ilişkilendirilir.

    ![Yapılandırma sunucusu ilişkilendirme](./media/vmware-azure-set-up-replication/associate2.png)

## <a name="edit-a-policy"></a>Bir ilkeyi Düzenle

1. Seçin **yönetme** > **Site Recovery altyapısı** > **çoğaltma ilkeleri**.
2. Değiştirmek istediğiniz çoğaltma ilkesini seçin.
3. Tıklayın **ayarlarını Düzenle**ve RPO eşik/kurtarma noktası bekletme saat/uygulama ile tutarlı anlık görüntü sıklığı alanları gerektiği gibi güncelleştirin.
4. "Kapalı" alanının açılan değer uygulama tutarlılık noktası oluşturmayı devre dışı bırak istiyorsanız seçin **uygulamayla tutarlı anlık görüntü sıklığı**.
5. **Kaydet**’e tıklayın. İlke, 30 ila 60 saniye içinde güncelleştirilmelidir.

## <a name="disassociate-or-delete-a-replication-policy"></a>İlişkisini kaldırın veya bir çoğaltma ilkesini silme

1. Çoğaltma ilkesini seçin.
    a. Yapılandırma sunucusundan ilkesinin ilişkisini kaldırmak çoğaltılan hiçbir Makine ilkesi kullandığınızdan emin olun. ' A tıklayarak **Bulutlarından**.
    b. İlkeyi silmek için onu bir yapılandırma sunucusu ile ilişkili olmayan emin olun. ' A tıklayarak **Sil**. Bu, silmek için 30-60 saniye sürer.
2. **Tamam** düğmesine tıklayın.
