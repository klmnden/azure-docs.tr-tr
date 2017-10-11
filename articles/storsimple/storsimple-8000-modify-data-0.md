---
title: "Veri 0 değiştirme StorSimple 8000 serisi cihazın ayarlarının | Microsoft Docs"
description: "StorSimple için Windows PowerShell, StorSimple Cihazınızda DATA 0 ağ arabirimindeki yeniden yapılandırmak için nasıl kullanılacağını öğrenin."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: 90df43e22f17fd32fe642514df098b72700e77af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi aygıtınızda veri 0 ağ arabirimi ayarlarını değiştirme

## <a name="overview"></a>Genel Bakış

Microsoft Azure StorSimple Cihazınızı veri 5 0 verilerden altı ağ arabirimi bulunur. DATA 0 arabirimi her zaman Windows PowerShell arabirimi veya seri Konsolu yapılandırılır ve otomatik olarak bulut etkindir. Azure Portalı aracılığıyla DATA 0 ağ arabirimindeki yapılandıramayacağınızı unutmayın.

Arabirim ilk sırasında Kurulum Sihirbazı ile yapılandırılmış veri 0 StorSimple cihaz dağıtımına başlangıç. Cihaz bir işlemsel modunda olduğunda veri 0 yeniden yapılandırmanız gerekebilir ayarlar. Bu öğreticide, veri 0 değiştirmek için iki yöntem ağ ayarlarını, StorSimple için ile hem de Windows PowerShell sağlar.

Bu öğretici okuduktan sonra aşağıdakileri gerçekleştirebilirsiniz:

* Veri 0 değiştirme Ağ Kurulum Sihirbazı'nı ayarlama
* Veri 0 ağ ayarları'nda değiştirme `Set-HcsNetInterface` cmdlet'i

## <a name="modify-data-0-network-settings-through-setup-wizard"></a>Kurulum Sihirbazı aracılığıyla veri 0 ağ ayarlarını değiştirme
StorSimple Cihazınızı Windows PowerShell arabirimine bağlanılırken ve Kurulum Sihirbazı'nı oturumunu başlatmasını veri 0 ağ ayarlarını yapılandırabilirsiniz. Veri 0 değiştirmek için aşağıdaki adımları gerçekleştirin ayarları:

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a>Kurulum Sihirbazı aracılığıyla veri 0 ağ ayarlarını değiştirmek için
1. Seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**. İstendiğinde sağlamak **cihaz Yöneticisi parolası**. Varsayılan parola `Password1`.
2. Komut istemine yazın:
   
    `Invoke-HcsSetupWizard`
3. Veri 0 yapılandırmak için Kurulum Sihirbazı görünür aygıtınızın arabirimi. IP adresi, ağ geçidi ve ağ maskesi için yeni değerleri sağlayın.

> [!NOTE]
> Sabit IP'leri denetleyicileri aracılığıyla yapılandırılması gerekir **ağ ayarlarını** StorSimple cihazı Azure portalında dikey. Daha fazla bilgi için Git [ağ arabirimleri değiştirme](storsimple-8000-modify-device-config.md#modify-network-interfaces).

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>Set-HcsNetInterface cmdlet'i aracılığıyla veri 0 ağ ayarlarını değiştirme
DATA 0 ağ arabirimi olan kullanılarak yeniden yapılandırmak için alternatif bir yolu `Set-HcsNetInterface` cmdlet'i. Cmdlet StorSimple Cihazınızı Windows PowerShell arabiriminden yürütülür. Bu yordamı kullanırken, denetleyici IP'leri sabit de burada yapılandırılabilir. Veri 0 değiştirmek için aşağıdaki adımları gerçekleştirin ayarları: 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a>Set-HcsNetInterface cmdlet'i aracılığıyla veri 0 ağ ayarlarını değiştirmek için
1. Seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**. İstendiğinde cihaz Yöneticisi parolasını verin. Varsayılan parola `Password1`.
2. Komut istemine yazın:
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    Açılı ayraç içinde veri 0 için aşağıdaki değerleri yazın:
   
   * IPv4 adresi
   * IPv4 ağ geçidi
   * IPv4 alt ağ maskesi
   * Denetleyici 0 sabit IPv4 adresi
   * Denetleyici 1 için sabit IPv4 adresi
     
     Bu cmdlet kullanımı hakkında daha fazla bilgi için Git [StorSimple cmdlet başvurusu için Windows PowerShell](https://technet.microsoft.com/library/dn688161.aspx).

## <a name="next-steps"></a>Sonraki adımlar
* Ağ arabirimleri veri 0 dışında yapılandırmak için kullanabileceğiniz [Azure portalında ağ ayarlarını yapılandırma](storsimple-8000-modify-device-config.md). 
* Ağ arabirimleri yapılandırırken herhangi bir sorunla karşılaşırsanız, başvurmak [dağıtım sorunlarını giderme](storsimple-troubleshoot-deployment.md).

