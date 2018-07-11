---
title: Yük devretme Azure hataları için sorun giderme | Microsoft Docs
description: Bu makalede, azure'a yük devretme, sık karşılaşılan sorunları giderme yolları açıklanır.
services: site-recovery
documentationcenter: ''
author: ponatara
manager: abhemraj
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: ponatara
ms.openlocfilehash: ad8b69bfe6f3261f00cd33846efc86ce3b198954
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919701"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Bir sanal makinenin azure'a yük devri sırasında karşılaşılan sorunları giderme

Azure'da bir sanal makine yük devretmesi yaparken aşağıdaki hatalardan birini alabilirsiniz. Sorunu gidermek için her bir hata koşulu için açıklanan adımları kullanın.

## <a name="failover-failed-with-error-id-28031"></a>Hata kimliği 28031 yük devretme başarısız oldu

Site kurtarma, başarısız bir yük devredilen Azure sanal makine oluşturmak ulaşamadı. Aşağıdaki nedenlerden biri nedeniyle oluşabilir:

* Sanal makine oluşturmak kullanılabilir yeterli kotası yoktur: kullanılabilir kota aboneliğine giderek denetleyebilirsiniz -> kullanım ve kotalar. Açabileceğiniz bir [yeni destek isteği](http://aka.ms/getazuresupport) Kotayı artırmak için.

* Yük devretme sanal makineler aynı kullanılabilirlik kümesindeki farklı boyutta ailelerinin deniyorsunuz. Aynı kullanılabilirlik kümesindeki tüm sanal makineler için aynı boyut ailesi seçtiğinizden emin olun. Sanal makinenin işlem ve ağ ayarlarına giderek boyutunu değiştirin ve ardından yük devretmeyi yeniden deneyin.

* Bir sanal makine oluşturulmasını önleyen bir abonelikte bir ilke yoktur. Sanal makine oluşturmaya izin ver ve ardından yük devretmeyi yeniden deneyin ilkesini değiştirin.

## <a name="failover-failed-with-error-id-28092"></a>Hata kimliği 28092 yük devretme başarısız oldu

Site kurtarma, başarısız bir ağ arabirimi oluşturmak mümkün değildi yük devredilen sanal makine. Aboneliğindeki ağ arabirimlerini oluşturmak kullanılabilir yeterli kotası olduğundan emin olun. Kullanılabilir kota aboneliğine giderek denetleyebilirsiniz -> kullanım ve kotalar. Açabileceğiniz bir [yeni destek isteği](http://aka.ms/getazuresupport) Kotayı artırmak için. Yeterli kotanız sonra bu bir aralıklı olabilir gönderme, işlemi yeniden deneyin. Ardından denemelere sorun devam ederse, bu belgenin sonunda bir yorum bırakın.  

## <a name="failover-failed-with-error-id-70038"></a>Hata kimliği 70038 yük devretme başarısız oldu

Site Recovery Azure'da Klasik sanal makine üzerinde başarısız oluşturmak mümkün değildi. Nedeniyle oluşabilir:

* Oluşturulacak sanal makine için gerekli olan bir sanal ağ gibi kaynaklardan biri mevcut değil. Sanal makinenin işlem ve ağ ayarlarında sağlanan sanal ağ oluşturma veya bir sanal ağ zaten var ve ardından yük devretmeyi yeniden deneyin ayarını değiştirin.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Bağlama/RDP veya SSH ile başarısız kurulamıyor sanal makinede bağlantı düğmesi gri nedeniyle sanal makine üzerinde

Bağlan düğmesi gri renkte ve Azure'a bir Express Route veya siteden siteye VPN bağlantısı ardından bağlanmamış

1. Git **sanal makine** > **ağ**, gerekli ağ arabiriminin adına tıklayın.  ![Ağ arabirimi](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Gidin **IP yapılandırmaları**, gerekli IP yapılandırması ad alanında bulunan'ye tıklayın. ![IP yapılandırmaları](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Genel IP adresi etkinleştirmek için tıklayın **etkinleştirme**. ![IP etkinleştir](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Tıklayarak **gerekli ayarları Yapılandır** > **Yeni Oluştur**. ![Yeni Oluştur](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Ortak adres adını girin, için varsayılan seçenekleri seçin **SKU** ve **atama**, ardından **Tamam**.
6. Şimdi, yaptığınız değişiklikleri kaydetmek için tıklatın **Kaydet**.
7. Paneller kapatın ve gidin **genel bakış** sanal makineye bağlanma/RDP bölümü.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-even-though-connect-button-is-available-not-grayed-out-on-the-virtual-machine"></a>Bağlama/RDP veya SSH ile başarısız kurulamıyor üzerinden sanal olsa bile bağlan düğmesi kullanılabilir (gri değil) sanal makine üzerinde makine

Denetleme **önyükleme tanılaması** sanal makineye yükleyip bu makalesinde listelenen hataları denetleyin.

1. Sanal makine başlatılmamışsa daha eski bir kurtarma noktasına yük devretmeyi deneyin.
2. Sanal makinenin içindeki uygulama devretmeyi deneyin bir uygulama ile tutarlı kurtarma noktasına değilse.
3. Sanal Makine etki alanına katılmış ise, etki alanı denetleyicisinin doğru şekilde çalıştığından emin olun. Bu takip ederek yapılabilir aşağıdaki adımlar verilir.
    a. aynı ağda yeni bir sanal makine oluşturma

    b.  aynı etki alanına katılacak şekilde tutabileceğinden emin başarısız yük devredilen sanal makinenin görünmesi beklenen.

    c. Etki alanı denetleyicisi ise **değil** doğru bir şekilde çalışmasını daha sonra deneyin başarısız'nda oturum açtıktan sonra bir yerel yönetici hesabı kullanarak sanal makine üzerinde
4. Özel bir DNS sunucusu kullanıyorsanız erişilebilir olduğundan emin olun. Bu takip ederek yapılabilir aşağıdaki adımlar verilir.
    a. Yeni bir sanal makine aynı ağ ve b oluşturun. sanal makineyi özel DNS sunucusunu kullanarak ad çözümlemesi mümkün olup olmadığını denetleyin

>[!Note]
>Önyükleme tanılama dışındaki herhangi bir ayarı etkinleştirerek Azure VM Aracısı, yük devretmeden önce sanal makinede yüklü olmasını gerektirir

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla yardıma ihtiyacınız olursa, ardından sorgunuza gönderin [Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) veya bu belgenin sonunda bir yorum yazın. Gereken yardımcı olması etkin bir topluluk sahibiz.
