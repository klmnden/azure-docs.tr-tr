---
title: Azure Ağ İzleyicisi paket yakalama giriş | Microsoft Docs
description: Bu sayfa Ağ İzleyicisi paket yakalama özelliği genel bir bakış sağlar.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 3a81afaa-ecd9-4004-b68e-69ab56913356
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: 8ef9da86377ac6f1b012cb0ebfd9d6866bc0c620
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061580"
---
# <a name="introduction-to-variable-packet-capture-in-azure-network-watcher"></a>Değişken paket yakalama Azure Ağ İzleyicisi'nde giriş

Ağ İzleyicisi değişken paket yakalama, bir sanal makineye gelen ve giden trafiği izlemek için paket yakalama oturumu oluşturmanıza olanak sağlar. Paket yakalama ağ anomalileri öngörülebiliyorsa ve proaktif tanılamaya yardımcı olur. Diğer kullanımlar ağ izinsiz girişi, istemci-sunucu iletişimleri ve daha fazlasını hata ayıklamak için bilgi elde etme, ağ istatistikleri toplama içerir.

Paket yakalaması Uzaktan Ağ İzleyicisi başlatılan bir sanal makine uzantısıdır. Bu özellik, bir paket yakalama değerli zaman kazandırır istenen sanal makineye üzerinde el ile çalıştırmayı yükünü kolaylaştırır. Paket yakalama, portal, PowerShell, CLI veya REST API tetiklenebilir. Paket yakalaması nasıl tetiklenebilir bir sanal makine uyarılara örnektir. Filtreler, izlemek istediğiniz trafik yakalama emin olmak yakalama oturumu için sağlanır. Filtreler 5 veri grubu üzerinde (protokol, yerel IP adresi, uzak IP adresi, yerel bağlantı noktası ve uzak bağlantı noktası) göre bilgi. Yakalanan veriler, yerel disk veya depolama blobu depolanır. Her Abonelikteki bölge başına 10 paket yakalama oturumu sınırı yoktur. Bu sınırı yalnızca oturumları için geçerlidir ve kaydedilen paket yakalama dosyalarını VM üzerinde yerel olarak veya bir depolama hesabı için geçerli değildir.

> [!IMPORTANT]
> Paket yakalaması gerektiren bir sanal makine uzantısı `AzureNetworkWatcherExtension`. Bir Windows VM'de uzantıyı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md) ve Linux VM ziyaret [LinuxiçinAzureAğİzleyicisiAracısısanalmakineuzantısı](../virtual-machines/linux/extensions-nwa.md).

Yalnızca istediğiniz bilgileri yakalamak bilgi miktarını azaltmak için aşağıdaki seçenekler için bir paket yakalama oturumu kullanılabilir:

**Yapılandırma Yakala**

|Özellik|Açıklama|
|---|---|
|**Paket (bayt) başına en fazla bayt** | Sayı yakalanan bayt her paket, tüm baytları boş bırakılırsa yakalanır. Sayı yakalanan bayt her paket, tüm baytları boş bırakılırsa yakalanır. IPv4 üst bilgisi yalnızca – gerekiyorsa 34 burada belirtin. |
|**Oturum (bayt) başına en fazla bayt** | Toplam sayısı, bayt değeri oturumu sonra erene ulaşıldıktan sonra yakalanır.|
|**Süre (saniye)** | Bir zaman kısıtlaması paket yakalama oturumu ayarlar. Varsayılan değer 18000 saniye veya 5 saat ' dir.|

**(İsteğe bağlı) filtreleme**

|Özellik|Açıklama|
|---|---|
|**Protokolü** | Paket yakalaması için filtre uygulamak için protokol. TCP, UDP ve tüm değerleri kullanılabilir.|
|**Yerel IP adresi** | Bu değer, yerel IP adresi bu filtre değeri eşleştiği paketleri paket yakalaması filtreler.|
|**Yerel bağlantı noktası** | Bu değer, yerel bağlantı noktası bu filtre değeri eşleştiği paketleri paket yakalaması filtreler.|
|**Uzak IP adresi** | Bu değer, paket için paket yakalaması uzak IP Bu filtre değeri eşleştiği filtreler.|
|**Uzak bağlantı noktası** | Bu değer, uzak bağlantı noktası bu filtre değeri eşleştiği paketleri paket yakalaması filtreler.|

### <a name="next-steps"></a>Sonraki adımlar

Portal üzerinden paket yakalamaları ederek nasıl yönetebileceğinizi öğrenin [Azure portalında paket yakalamayı yönetme](network-watcher-packet-capture-manage-portal.md) veya PowerShell ederek [PowerShell ile yönetme paket yakalama](network-watcher-packet-capture-manage-powershell.md).

Sanal makine uyarılara göre ziyaret ederek proaktif paket yakalamaları oluşturmayı [uyarı tetiklendi paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













