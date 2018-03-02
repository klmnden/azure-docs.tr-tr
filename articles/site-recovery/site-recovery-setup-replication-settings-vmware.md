---
title: "Azure Site Recovery için çoğaltma ayarları oluşturma | Microsoft Docs"
description: "Site Recovery'nin, VMM bulutlarındaki Hyper-V sanal makinelerinden Azure'a yönelik çoğaltma, yük devretme ve kurtarma işlemlerini gerçekleştirmek üzere nasıl dağıtılacağını açıklar."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/22/2018
ms.author: sutalasi
ms.openlocfilehash: 0a567f31bf1991d4c2a95468d2abc31c51a878f3
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="manage-replication-policy-for-vmware-to-azure"></a>VMware’den Azure’a çoğaltma ilkesini yönetme


## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. **Yönet** > **Site Recovery Altyapısı**’nı seçin.
2. **VMware ve Fiziksel makineler için** altındaki **Çoğaltma ilkeleri**’ni seçin.
3. **+Çoğaltma ilkesi**’ni seçin.

    ![Çoğaltma ilkesi oluşturma](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. İlke adını girin.

5. **RPO eşiği** bölümünde RPO limitini belirtin. Sürekli çoğaltma bu limiti aşarsa uyarılar oluşturulur.
6. **Kurtarma noktası bekletme** kısmında, her kurtarma noktası için bekletme süresini saat cinsinden belirtin. Korumalı makineler, bekletme penceresi içindeki herhangi bir noktaya kurtarılabilir.

    > [!NOTE]
    > Premium depolama alanına çoğaltılan makineler için 24 saate kadar bekletme desteklenir. Standart depolama alanına çoğaltılan makineler için 72 saate kadar bekletme desteklenir.

    > [!NOTE]
    > Yeniden çalışmaya yönelik bir çoğaltma ilkesi otomatik olarak oluşturulur.

7. **Uygulamayla tutarlı anlık görüntü sıklığı** kısmında, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının hangi sıklıkta oluşturulacağını (dakika cinsinden) belirtin.

8. **Tamam**’a tıklayın. İlke, 30 ila 60 saniye içinde oluşturulur.

![Çoğaltma ilkesi üretme](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-a-configuration-server-with-a-replication-policy"></a>Yapılandırma sunucusunu çoğaltma ilkesi ile ilişkilendirme
1. Yapılandırma sunucusunu ilişkilendirmek istediğiniz çoğaltma ilkesini seçin.
2. **İlişkilendir**’e tıklayın.
![Yapılandırma sunucusunu ilişkilendirme](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Sunucu listesinden yapılandırma sunucusunu seçin.
4. **Tamam**’a tıklayın. Yapılandırma sunucusu, bir ila iki dakika içinde ilişkilendirilir.

![Yapılandırma sunucusu ilişkilendirme](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-a-replication-policy"></a>Çoğaltma ilkesini düzenleme
1. Çoğaltma ayarlarını düzenlemek istediğiniz çoğaltma ilkesini seçin.
![Çoğaltma ilkesini düzenleme](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. **Ayarları Düzenle**’ye tıklayın.
![Çoğaltma ilkesi ayarlarını düzenleme](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Gereksinimlerinize göre ayarları değiştirin.
4. **Kaydet**’e tıklayın. İlke, bu çoğaltma ilkesini kullanan VM sayısına bağlı olarak iki ila beş dakika içinde kaydedilir.

![Çoğaltma ilkesini kaydetme](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-a-configuration-server-from-a-replication-policy"></a>Yapılandırma sunucusunun çoğaltma ilkesi ile ilişkisini kaldırma
1. Yapılandırma sunucusunu ilişkilendirmek istediğiniz çoğaltma ilkesini seçin.
2. **İlişkilendirmeyi Kaldır**’a tıklayın.
3. Sunucu listesinden yapılandırma sunucusunu seçin.
4. **Tamam**’a tıklayın. Yapılandırma sunucusunun ilişkisi, bir ila iki dakika içinde kaldırılır.

    > [!NOTE]
    > İlkeyi kullanan en az bir çoğaltılmış öğe varsa yapılandırma sunucusunun ilişkisini kaldıramazsınız. Yapılandırma sunucusunun ilişkisini kaldırmadan önce ilkeyi kullanan çoğaltılmış bir öğe bulunmadığından emin olun.

## <a name="delete-a-replication-policy"></a>Çoğaltma ilkesini silme

1. Silmek istediğiniz çoğaltma ilkesini seçin.
2. **Sil**'e tıklayın. İlke, 30 ila 60 saniye içinde silinir.

    > [!NOTE]
    > Kendisiyle ilişkili en az bir yapılandırma sunucusu varsa çoğaltma ilkesini silemezsiniz. İlkeyi silmeden önce, ilkeyi kullanan çoğaltılmış öğe bulunmadığından emin olun ve tüm ilişkili yapılandırma sunucularını silin.
