---
title: "Güvenlik grubu Azure Ağ İzleyicisi görünümünde giriş | Microsoft Docs"
description: "Bu sayfa Ağ İzleyicisi güvenlik görünüm yetenek genel bir bakış sağlar"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: jdial
ms.openlocfilehash: f4175875b68c52e68588b8d0debd003ab73427ec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-network-security-group-view-in-azure-network-watcher"></a>Ağ güvenlik grubu Azure Ağ İzleyicisi görünümünde giriş

Ağ güvenlik grupları, bir alt ağ düzeyinde veya bir NIC düzeyi ilişkilendirilir. Bir alt düzeyde ilişkili olduğunda, alt ağdaki tüm VM örnekleri için geçerlidir. Ağ güvenlik grubu görünümü yapılandırılan Nsg'ler ve yapılandırma hakkında bilgi sağlayan bir sanal makine için bir NIC ve alt ağ düzeyinde ilişkili kurallarını döndürür. Ayrıca, her bir VM NIC'ler için etkili güvenlik kuralları döndürülür. Ağ güvenlik grubu kullanarak görünüm açık bağlantı noktaları gibi ağ güvenlik açıkları için bir VM değerlendirebilirsiniz. De doğrulamak, ağ güvenlik grubu temel alarak beklendiği gibi çalışıp çalışmadığını bir [yapılandırılmış ve etkin güvenlik kuralları arasında karşılaştırma](network-watcher-nsg-auditing-powershell.md).

Daha uzun bir kullanım örneği, güvenlik uyumluluğunu ve denetim gerçekleştirilir. Kuruluşunuzdaki güvenlik idare modeli olarak güvenlik kuralları Düzenleyici kümesi tanımlayabilirsiniz. Bir dönemsel uyumluluk denetimi için ağınızda VM'lerin her birinde etkili kurallarla düzenleyici kuralları karşılaştırarak programlı bir şekilde uygulanabilir.

Portalda kuralları etkili, alt ağ, ağ arabirimi ve varsayılan ayrılır. Bu kuralı bir sanal makineye uygulanan basit bir görünüm sağlar. Karşıdan Yükle düğmesi kolayca sekmesini olursa olsun tüm güvenlik kuralları bir CSV dosyasına indirmek için sağlanır.

![Güvenlik grubu görünümü][1]

Kuralları seçilebilir ve ağ güvenlik grubu ve kaynak ve hedef öneklerinin göstermek için yeni bir dikey pencere açılır. Bu dikey pencereden doğrudan ağ güvenlik grubu kaynak gidebilirsiniz.

![ayrıntıya gitme][2]

### <a name="next-steps"></a>Sonraki adımlar

Ağ güvenlik grubu ayarlarınızı ziyaret ederek denetim öğrenin [PowerShell ile ağ güvenlik grubu denetim ayarları](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









