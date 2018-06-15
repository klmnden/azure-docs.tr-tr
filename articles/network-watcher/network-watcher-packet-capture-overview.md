---
title: Paket yakalama Azure Ağ İzleyicisi giriş | Microsoft Docs
description: Bu sayfa Ağ İzleyicisi paket yakalama özelliği genel bir bakış sağlar
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 3a81afaa-ecd9-4004-b68e-69ab56913356
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 152cc8fb61aa6115c7b5863e4d798db9e7aa5b7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23864297"
---
# <a name="introduction-to-variable-packet-capture-in-azure-network-watcher"></a>Azure Ağ İzleyicisi değişken paket yakalama giriş

Ağ İzleyicisi değişken paket yakalama, bir sanal makine gelen ve giden trafiği izlemek için paket yakalama oturumları oluşturmanıza olanak sağlar. Her iki ağ anormallikleri Tepkisel tanılamak için paket Yakalama yardımcı olur ve proactivity. Diğer kullanımlar ağ yetkisiz erişim, istemci-sunucu iletişimleri ve çok daha fazlasını hata ayıklamak için bilgi sağlamasını ağ istatistikleri toplama içerir.

Paket yakalama Ağ İzleyicisi uzaktan başlatılan bir sanal makine uzantısıdır. Bu özellik, bir paket yakalama değerli zaman kazandırır istenen sanal makinesinin üzerinde el ile çalıştırmayı yükünü kolaylaştırır. Paket yakalama portal, PowerShell'i, CLI veya REST API tetiklenebilir. Paket yakalama nasıl tetiklenebilir bir sanal makine uyarılara örnektir. Filtreler sağlamak, izlemek istediğiniz trafiği yakalamak yakalama oturumu için sağlanır. Filtreler, 5-tanımlama grubu üzerinde (protokolü, yerel IP adresi, uzak IP adresi, yerel bağlantı noktası ve uzak bağlantı noktası) dayalı bilgileri. Yakalanan veriler, yerel disk veya bir depolama blob depolanır. Her Abonelikteki bölge başına 10 paket yakalama oturumu sınırı yoktur. Bu sınır yalnızca oturumlar için geçerlidir ve kaydedilmiş paket yakalama dosyalarını VM yerel olarak veya bir depolama hesabı için geçerli değildir.

> [!IMPORTANT]
> Paket yakalama gerektiren bir sanal makine uzantısı `AzureNetworkWatcherExtension`. Bir Windows VM uzantısı yüklemek için ziyaret [Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md) ve Linux VM ziyaret edin: [Linux için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/linux/extensions-nwa.md).

Yalnızca istediğiniz bilgileri yakalamak bilgi azaltmak için aşağıdaki seçenekleri paket yakalama oturum açmak için kullanılabilir:

**Yapılandırma Yakalama**

|Özellik|Açıklama|
|---|---|
|**Paket (bayt) başına en fazla bayt sayısı** | Sayı yakalanan bayt gelen her paket, tüm baytlar boş bırakılırsa yakalanır. Sayı yakalanan bayt gelen her paket, tüm baytlar boş bırakılırsa yakalanır. Yalnızca IPv4 üstbilgisi – gerekirse burada 34 belirtin |
|**Oturum (bayt) başına en fazla bayt sayısı** | Oturum sona erdirilir değere ulaşıldığında, bayt toplam sayısı yakalanır.|
|**Süre (saniye)** | Paket bir zaman kısıtlama kümelerini oturum yakalayın. Varsayılan değer 18000 saniye veya 5 saat olur.|

**(İsteğe bağlı) filtreleme**

|Özellik|Açıklama|
|---|---|
|**Protokol** | Paket yakalama için filtre uygulamak için protokol. Kullanılabilir TCP, UDP ve tüm değerlerdir.|
|**Yerel IP adresi** | Bu değer, yerel IP adresi bu filtre değeri eşleştiği paket için paket yakalama filtreler.|
|**Yerel bağlantı noktası** | Bu değer, yerel bağlantı noktası bu filtre değeri eşleştiği paket için paket yakalama filtreler.|
|**Uzak IP adresi** | Bu değer, uzak IP Bu filtre değeri eşleştiği paket için paket yakalama filtreler.|
|**Uzak bağlantı noktası** | Bu değer, uzak bağlantı noktası bu filtre değeri eşleştiği paket için paket yakalama filtreler.|

### <a name="next-steps"></a>Sonraki adımlar

Paket yakalama Portalı aracılığıyla ziyaret ederek nasıl yönetebileceğiniz öğrenin [paket yakalama Azure portalında yönetmek](network-watcher-packet-capture-manage-portal.md) veya şu adresi ziyaret ederek PowerShell ile [PowerShell ile paket yakalama yönetmek](network-watcher-packet-capture-manage-powershell.md).

Sanal makine uyarılar ziyaret ederek temelinde öngörülü paket yakalamaları oluşturmayı öğrenin [bir uyarı tetiklenen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md)

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













