---
title: Yük devretme Azure hataları için sorun giderme | Microsoft Docs
description: Bu makalede Azure yapabilmesini içinde sık karşılaşılan sorunları giderme yolları
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
ms.date: 03/09/2018
ms.author: ponatara
ms.openlocfilehash: 838eac510fc17d56f808f541f4e205a279f63c56
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318900"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Bir sanal makineye Azure üzerinden başarısız olduğunda hatalarında sorun giderme

Aşağıdaki hatalardan biri, Azure için sanal makinenin yük devretme yaparken alabilirsiniz. Sorunu gidermek için her bir hata koşulu için açıklanan adımları kullanın.

## <a name="failover-failed-with-error-id-28031"></a>Yük devretme 28031 hata Koduyla başarısız oldu

Site Recovery azure'da sanal makine üzerinde başarısız oluşturmak mümkün değildi. Aşağıdaki nedenlerden biri nedeniyle oluşabilir:

* Sanal makine oluşturmak için kullanılabilir yeterli kotası yok: aboneliğine giderek kullanılabilir kota denetleyebilirsiniz -> kullanım + kotalar. Açabilirsiniz bir [yeni destek isteği](http://aka.ms/getazuresupport) Kotayı artırmak için.

* Yük devretme sanal makineler aynı kullanılabilirlik kümesinde farklı boyutu ailelerinin deniyorsunuz. Aynı kullanılabilirlik kümesinde tüm sanal makineler için aynı boyutu ailesi seçtiğinizden emin olun. Sanal makinenin işlem ve ağ ayarlarına giderek boyutunu değiştirin ve sonra Yük devretme işlemini yeniden deneyin.

* Bir sanal makineye oluşturulmasını engeller abonelikte bir ilke yoktur. Bir sanal makine oluşturulmasına izin ve yük devretme yeniden denemek için ilkeyi değiştirin.

## <a name="failover-failed-with-error-id-28092"></a>Yük devretme 28092 hata Koduyla başarısız oldu

Site Recovery birleştiremedi başarısız için bir ağ arabirimi oluşturmak üzere sanal makine üzerinde. Aboneliğindeki ağ arabirimlerini oluşturmak için kullanılabilir yeterli kotası olduğundan emin olun. Aboneliği giderek kullanılabilir kota denetleyebilirsiniz -> kullanım + kotalar. Açabilirsiniz bir [yeni destek isteği](http://aka.ms/getazuresupport) Kotayı artırmak için. Yeterli kotası olması durumunda bu bir aralıklı olabilir vermek, işlemi yeniden deneyin. Denemelere sorun devam ederse, bu belgenin sonuna bir yorum bırakın.  

## <a name="failover-failed-with-error-id-70038"></a>Yük devretme 70038 hata Koduyla başarısız oldu

Site Recovery, azure'da Klasik sanal makine üzerinde başarısız oluşturmak bırakamıyor. Nedeniyle oluşabilir:

* Oluşturulacak sanal makine için gerekli olan bir sanal ağ gibi kaynaklardan biri yok. Sanal makinenin işlem ve ağ ayarlarının altında sağlanan sanal ağ oluşturun veya zaten var olduğundan ve yük devretme yeniden sanal bir ağa ayarı değiştirin.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Bağlan/RDP/SSH devredilen kurulamıyor sanal makineye Bağlan düğmesi gri nedeniyle sanal makine üzerinde

Bağlan düğmesi gri gösterilir ve Azure'a bir Express Route veya siteden siteye VPN bağlantısı üzerinden, ardından bağlanmamış

1. Git **sanal makine** > **ağ**, gerekli ağ arabirimi adına tıklayın.  ![Ağ arabirimi](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Gidin **IP yapılandırmaları**, gerekli IP yapılandırması ad alanında bulunan'ye tıklayın. ![Ipconfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Genel IP adresi etkinleştirmek için tıklayın **etkinleştirmek**. ![IP etkinleştir](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Tıklayın **gerekli ayarları Yapılandır** > **Yeni Oluştur**. ![Yeni Oluştur](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Ortak adres adını girin, varsayılan seçenekleri seçin **SKU** ve **atama**, ardından **Tamam**.
6. Şimdi, yapılan değişiklikleri kaydetmek için tıklatın **kaydetmek**.
7. Paneller kapatın ve gidin **genel bakış** sanal makineye bağlanma RDP bölümü.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-even-though-connect-button-is-available-not-grayed-out-on-the-virtual-machine"></a>Bağlan/RDP/SSH devredilen kurulamıyor üzerinden sanal bile bağlan düğmesi bulunur (gri değil) sanal makinede makine

Denetleme **önyükleme tanılama** sanal makine ve bu makalesinde listelenen hataları denetleyin.

1. Sanal makine başlatılmamışsa, daha eski bir kurtarma noktasına devretmek deneyin.
2. Sanal makine içinde uygulama yukarı, uygulamayla tutarlı kurtarma noktasına deneyin yapabilmesini değilse.
3. Ardından sanal makine etki alanına katılmış ise, bu etki alanı denetleyicisi doğru şekilde çalıştığından emin olun. Bu izleyerek yapılabilir aşağıdaki adımları verilen.
    a. aynı ağdaki yeni bir sanal makine oluşturun

    b.  aynı etki alanına mümkün olduğundan emin olun başarısız sanal makine gündeme beklenir.

    c. Etki alanı denetleyicisi ise **değil** doğru bir şekilde çalışmasını sonra oturum açmayı başarısız deneyin üzerinden bir yerel yönetici hesabı kullanarak sanal makine
4. Özel bir DNS sunucusu kullanıyorsanız, erişilebilir olduğundan emin olun. Bu izleyerek yapılabilir aşağıdaki adımları verilen.
    a. Yeni bir sanal makine aynı ağ ve b oluşturun. sanal makine adı özel DNS sunucusu kullanılarak çözümleme mümkün olup olmadığını denetleyin

>[!Note]
>Önyükleme tanılaması dışındaki herhangi bir ayarı etkinleştirerek Azure VM Aracısı Yük devretmeden önce sanal makinede yüklü olmasını gerektirir

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla yardıma gereksinim duyarsanız, ardından sorgunuza ileti [Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) veya bu belgenin sonuna bir yorum bırakın. Size yardımcı olmak üzere saptayabilmelisiniz etkin bir topluluk sunuyoruz.
