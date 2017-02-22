---
title: "Azure Site Recovery için çoğaltma ayarları oluşturma | Microsoft Docs"
description: "Site Recovery&quot;nin, VMM bulutlarındaki Hyper-V sanal makinelerinden Azure&quot;a yönelik çoğaltma, yük devretme ve kurtarma işlemlerini gerçekleştirmek üzere nasıl dağıtılacağını açıklar."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/19/2017
ms.author: sutalasi
translationtype: Human Translation
ms.sourcegitcommit: 74d51269cdd55e39eaa2963fec2b409decb47d0a
ms.openlocfilehash: a279fdc35c62c76e6f0858d52c64240032669f15


---
# <a name="manage-replication-policy-for-vmware-to-azure"></a>VMware’den Azure’a çoğaltma ilkesini yönetme


## <a name="create-a-new-replication-policy"></a>Yeni bir çoğaltma ilkesi oluşturma

1. Sol taraftaki menüde 'Yönet-> Site Recovery Altyapısı'na tıklayın. 
2. 'VMware ve Fiziksel makineler' bölümü altındaki 'Çoğaltma ilkeleri' öğesini seçin.
3. Üst kısımdaki '+Çoğaltma ilkesi' öğesine tıklayın.

    ![Çoğaltma ilkesi oluşturma](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. İlke adını girin.

5. RPO eşiğinde RPO limitini belirtin. Sürekli çoğaltma bu limiti aşarsa uyarılar oluşturulur.
6. Kurtarma noktası bekletme bölümünde, her kurtarma noktası için bekletme süresinin ne kadar olacağını saat cinsinden belirtin. Korumalı makineler, bu süre içindeki herhangi bir noktaya kurtarılabilir. 

    > [!NOTE] 
    > Premium depolamaya çoğaltılan makineler için en fazla 24, standart depolamaya çoğaltılan makineler için en fazla 72 saat bekletme desteklenir.
    
    > [!NOTE] 
    > Yeniden çalışma için çoğaltma ilkesi otomatik olarak oluşturulur.

7. Uygulamayla tutarlı anlık görüntü sıklığı kısmında, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktasının hangi sıklıkta oluşturulacağını (dakika cinsinden) belirtin.

8. ‘Tamam’’a tıklayın. İlke yaklaşık 30 saniye ila 1 dakika içinde oluşturulmalıdır.

![Çoğaltma ilkesi oluşturma](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-configuration-server-with-replication-policy"></a>Yapılandırma sunucusunu çoğaltma ilkesi ile ilişkilendirme
1. Yapılandırma sunucusunu ilişkilendirmek istediğiniz çoğaltma ilkesine tıklayın.
2. Üst kısımdaki 'İlişkilendir' öğesine tıklayın.
![Çoğaltma ilkesi oluşturma](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Sunucu listesinden 'Yapılandırma sunucusu' seçin.
4. Tamam'a tıklayın. Yapılandırma sunucusu yaklaşık 1-2 dakika içinde ilişkilendirilir.

![Çoğaltma ilkesi oluşturma](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-replication-policy"></a>Çoğaltma ilkesini düzenleme
1. Çoğaltma ayarlarını düzenlemek istediğiniz çoğaltma ilkesine tıklayın.
![Çoğaltma ilkesi oluşturma](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. Üst kısımdaki 'Ayarları Düzenle' öğesine tıklayın.
![Çoğaltma ilkesi oluşturma](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Gereksinimlerinize göre ayarları değiştirin.
4. Üst kısımdaki 'Kaydet' öğesine tıklayın. İlke bu çoğaltma ilkesini kullanan VM sayısına bağlı olarak 2 ila 5 dakika içinde kaydedilir.

![Çoğaltma ilkesi oluşturma](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-configuration-server-from-replication-policy"></a>Yapılandırma sunucusunun çoğaltma ilkesi ile ilişkisini kaldırma
1. Yapılandırma sunucusunu ilişkilendirmek istediğiniz çoğaltma ilkesine tıklayın.
2. Üst kısımdaki 'İlişkiyi Kaldır' öğesine tıklayın.
3. Sunucu listesinden 'Yapılandırma sunucusu' seçin.
4. Tamam'a tıklayın. Yapılandırma sunucusunun ilişkisi yaklaşık 1-2 dakika içinde kaldırılır.
    
    > [!NOTE] 
    > İlkeyi kullanan en az bir çoğaltılmış öğe varsa Yapılandırma sunucusunun ilişkisini kaldıramazsınız. Yapılandırma sunucusunun ilişkisini kaldırmadan önce çoğaltılmış bir öğe bulunmadığından emin olun.

## <a name="delete-replication-policy"></a>Çoğaltma ilkesini silme 

1. Silmek istediğiniz çoğaltma ilkesine tıklayın.
2. Sil'e tıklayın. İlke yaklaşık 30 saniye ila 1 dakika içinde silinir.

    > [!NOTE] 
    > Kendisine ilişkili en az 1 yapılandırma sunucusu olan bir çoğaltma ilkesini silemezsiniz. İlkeyi silmeden önce ilkeyi kullanan çoğaltılmış öğelerin bulunmadığından emin olun ve tüm ilişkili yapılandırma sunucularını silin.


<!--HONumber=Jan17_HO4-->


