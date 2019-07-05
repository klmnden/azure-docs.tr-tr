---
title: Nasıl bir Microsoft Azure FXT Edge dosyalayıcı birim Kapat
description: Başlangıç ve bir Azure FXT Edge dosyalayıcı düğümünün güvenli kapatma için yordamlar
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: how-to
ms.date: 07/01/2019
ms.author: v-erkell
ms.openlocfilehash: 8d779547040da9855409d3408827af2e4acd17a7
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542871"
---
# <a name="how-to-safely-power-off-azure-fxt-edge-filer-hardware"></a>Nasıl güvenli bir şekilde Azure FXT Edge dosyalayıcı donanım kapatmasına

Tek bir düğümde geçiş yapmak için fiziksel bir güç düğmesini kullanabilirsiniz, ancak, bu normal koşullar altında birim kapatmaya kullanmamanız gerekir.

Bir kümenin parçası olarak Azure FXT Edge dosyalayıcı düğüm olduktan sonra donanım kapatmak için küme Denetim Masası yazılımı kullanmanız gerekir. 

> [!NOTE] 
> Olası veri kaybı veya bozulması önlemek için her zaman bir Azure FXT Edge dosyalayıcı kapatmak için Denetim Masası yazılımı kullanın. Microsoft Müşteri Hizmetleri ve desteği tarafından Bunu yapmak için başlatmamanız sürece fiziksel güç düğmesine kapatma için kullanmayın.
> 
> Elektrik Acil güç kablosu kesmek veya verilerinizi center'ın elektrik mekanizması bağlantısını kesin.

## <a name="shut-down-a-node-from-the-control-panel"></a>Denetim Masası'ndan bir düğüm kapatma

Güvenli bir şekilde bir Azure FXT Edge dosyalayıcı düğümü devre dışı güç için bu yönergeleri izleyin:

1. Denetim Masası kümeye oturum açın. (Yönde [ayarları sayfaları](fxt-cluster-create.md#open-the-settings-pages))
1. Tıklayın **ayarları** sekmesine ve ardından Yük **küme** > **FXT düğümleri** sayfası.
1. Kapatmak istediğiniz bir küme düğümlerinin listesinde bulun. Tıklayın **kapatın** düğmesine kendi **eylemleri** sütun. 
1. Birkaç dakika bekleyin. Düğümü Kapat ve kendisini gücünün kapatılmasını.

## <a name="next-steps"></a>Sonraki adımlar

* Durum LED'lerini ve diğer göstergeleri hakkında [İzleyici Azure FXT Edge dosyalayıcı donanım durumunu](fxt-monitor.md).
* Azure FXT Edge dosyalayıcı power hakkında daha fazla bilgi sağlayan içinde [power kabloları bağlayın](fxt-network-power.md#connect-power-cables).
