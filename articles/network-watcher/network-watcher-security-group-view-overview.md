---
title: Azure Ağ İzleyicisi'nde güvenlik grubu görünümü giriş | Microsoft Docs
description: Bu sayfa Ağ İzleyicisi güvenlik görünümü özelliği genel bir bakış sağlar.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: kumud
ms.openlocfilehash: aae57603dea9b7142956065dd65727e59014dcb7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60684361"
---
# <a name="introduction-to-network-security-group-view-in-azure-network-watcher"></a>Ağ güvenlik grubu Görünümü'nde Azure Ağ İzleyicisi giriş

Ağ güvenlik grupları, bir alt ağ düzeyinde veya bir NIC düzeyinde ilişkilendirilir. Bir alt ağ düzeyinde ilişkilendirilmiş, alt ağdaki tüm VM örnekleri için geçerlidir. Ağ güvenlik grubu görünümünü yapılandırılan Nsg'ler ve yapılandırma hakkında Öngörüler sağlayan bir sanal makine için NIC'te veya alt ağ düzeyinde ilişkilendirilmiş kuralları döndürür. Ayrıca, geçerli güvenlik kuralları, her bir sanal makinede NIC döndürülür. Kullanarak ağ güvenlik grubu görünümü, ağ güvenlik açıklarını açık bağlantı noktaları gibi sanal Makineyi değerlendirebilirsiniz. Siz de doğrulayabilir, ağ güvenlik grubu temel alarak beklendiği gibi çalışıp çalışmadığını bir [yapılandırılmış ve onaylı güvenlik kuralları arasında karşılaştırma](network-watcher-nsg-auditing-powershell.md).

Güvenlik uyumluluk ve denetleme daha uzun bir kullanım örneği kullanılabilir. Kuruluşunuzdaki güvenlik idare modeli olarak Düzenleyici birtakım güvenlik kuralları tanımlayabilirsiniz. Bir dönemsel uyumluluk denetim ağınızdaki sanal makinelerin her biri için öngörücü kuralları geçerli kuralları ile karşılaştırarak programlı bir şekilde uygulanabilir.

Portalda kuralları etkili, alt ağ, ağ arabirimi ve varsayılan ayrılır. Bu, bir sanal makineye uygulanan kurallar basit bir görünüm sağlar. Sekme ne olursa olsun tüm güvenlik kuralları bir CSV dosyasına kolayca indirmek için indir düğmesine sağlanır.

![güvenlik grubu görünümü][1]

Kuralları seçilebilir ve ağ güvenlik grubu ve kaynak ve hedef ön ekleri göstermek için yeni bir dikey pencere açılır. Bu dikey pencereden ağ güvenlik grubu kaynağa doğrudan gidebilirsiniz.

![detayına gitme][2]

### <a name="next-steps"></a>Sonraki adımlar

Ağ güvenlik grubu ayarlarınızı ederek denetim öğrenin [PowerShell ile ağ güvenlik grubu denetim ayarları](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









