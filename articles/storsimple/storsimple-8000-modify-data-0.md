---
title: DATA 0 değişiklik StorSimple 8000 serisi cihaz ayarları | Microsoft Docs
description: StorSimple için Windows PowerShell, StorSimple Cihazınızda DATA 0 ağ arabirimindeki yeniden yapılandırmak için kullanmayı öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: 3cf136c5ddec8f4998d15c597914e1f806453945
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60631592"
---
# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi Cihazınızı DATA 0 ağ arabirimi ayarlarını değiştirme

## <a name="overview"></a>Genel Bakış

Microsoft Azure StorSimple Cihazınızı veri 5 DATA 0'dan altı ağ arabirimi var. DATA 0 arabirimi her zaman Windows PowerShell arabirimi veya seri konsol yapılandırılır ve otomatik olarak bulut etkindir. Azure portalı üzerinden DATA 0 ağ arabirimindeki yapılandıramayacağınızı unutmayın.

DATA 0 arabirimi ilk sırasında Kurulum Sihirbazı aracılığıyla yapılandırılan ilk StorSimple cihazının dağıtım. Cihaz bir işletimsel modunda olduğunda DATA 0'ı yeniden yapılandırmanız gerekebilir ayarları. Bu öğretici, ağ ayarları, her iki ile Windows PowerShell için StorSimple DATA 0 değiştirmek için iki yöntem sunar.

Bu öğreticide okuduktan sonra şunları yapabilir:

* DATA 0 değişiklik Ağ Kurulum Sihirbazı aracılığıyla ayarı
* DATA 0 ağ ayarları'nda değiştirmek `Set-HcsNetInterface` cmdlet'i

## <a name="modify-data-0-network-settings-through-setup-wizard"></a>Kurulum Sihirbazı aracılığıyla veri 0 ağ ayarlarını değiştirme
Veri 0 ağ ayarları, StorSimple cihazınızın Windows PowerShell arabirimine bağlanma ve bir Kurulum Sihirbazı oturumu başlatılırken yeniden yapılandırabilirsiniz. DATA 0 değiştirmek için aşağıdaki adımları gerçekleştirin ayarları:

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a>Kurulum Sihirbazı aracılığıyla veri 0 ağ ayarları değiştirmek için
1. Seri konsol menüsünde seçeneği 1 **tam erişimle oturum açmak**. İstendiğinde sağlamak **cihaz Yöneticisi parolası**. Varsayılan parola `Password1`.
2. Komut istemine şunları yazın:
   
    `Invoke-HcsSetupWizard`
3. DATA 0'ı yapılandırmak için Kurulum Sihirbazı'nı görünür cihazınızın arabirimi. IP adresi, ağ geçidi ve ağ maskesi için yeni değerleri sağlayın.

> [!NOTE]
> Sabit denetleyici IP'lerin aracılığıyla yapılandırılması gerekir **ağ ayarları** Azure portalında StorSimple cihaz dikey penceresinde. Daha fazla bilgi için Git [ağ arabirimleri değiştirme](storsimple-8000-modify-device-config.md#modify-network-interfaces).

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>Set-HcsNetInterface cmdlet'i aracılığıyla veri 0 ağ ayarlarını değiştirme
DATA 0 ağ arabirimi olan kullanımının yeniden yapılandırmak için alternatif bir yolu `Set-HcsNetInterface` cmdlet'i. Cmdlet, StorSimple cihazınızın Windows PowerShell arabiriminden yürütülür. Bu yordamı kullanarak, denetleyici IP'leri sabit de burada yapılandırılabilir. DATA 0 değiştirmek için aşağıdaki adımları gerçekleştirin ayarları: 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a>Set-HcsNetInterface cmdlet'i aracılığıyla veri 0 ağ ayarları değiştirmek için
1. Seri konsol menüsünde seçeneği 1 **tam erişimle oturum açmak**. İstendiğinde cihaz Yöneticisi parolasını verin. Varsayılan parola `Password1`.
2. Komut istemine şunları yazın:
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    Açılı ayraçlar içine DATA 0 için aşağıdaki değerleri yazın:
   
   * IPv4 adresi
   * IPv4 ağ geçidi
   * IPv4 alt ağ maskesi
   * Denetleyici 0 için sabit bir IPv4 adresi
   * Denetleyici 1 için sabit bir IPv4 adresi
     
     Bu cmdlet'in kullanımı hakkında daha fazla bilgi için Git [StorSimple cmdlet başvurusu için Windows PowerShell](https://technet.microsoft.com/library/dn688161.aspx).

## <a name="next-steps"></a>Sonraki adımlar
* DATA 0 dışındaki ağ arabirimlerinin yapılandırmak için kullanabileceğiniz [ağ ayarlarını yapılandırma Azure portalında](storsimple-8000-modify-device-config.md). 
* Ağ Arabirimlerinizden yapılandırırken sorunla karşılaşırsanız başvurmak [dağıtım sorunlarını giderme](storsimple-troubleshoot-deployment.md).

