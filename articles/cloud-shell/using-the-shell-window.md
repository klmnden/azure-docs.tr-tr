---
title: Azure bulut Kabuğu penceresini kullanarak | Microsoft Docs
description: Azure bulut Kabuk penceresinin nasıl kullanılacağını genel bakış.
services: azure
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: juluk
ms.openlocfilehash: 43da2bf5b66ff7db03a6fb5c2e1ceaebe322bcbb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
ms.locfileid: "28920003"
---
# <a name="using-the-azure-cloud-shell-window"></a>Azure bulut Kabuğu penceresini kullanma

Bu belge bulut Kabuk penceresinin nasıl kullanılacağını açıklar.

## <a name="swap-between-bash-and-powershell-environments"></a>Bash ve PowerShell ortamlar arasında değiştirme
![](media/using-the-shell-window/env-selector.png)

Bash ve PowerShell ortamlar arasında takas için bulut Kabuk araç çubuğunda ortamı seçiciyi kullanın.

## <a name="restart-cloud-shell"></a>Cloud Shell'i yeniden başlat
![](media/using-the-shell-window/restart.png)
> [!WARNING]
> Bulut Kabuk yeniden makine durumunu sıfırlamak ve tüm dosyalar, Azure tarafından kalıcı olmayan dosya paylaşımı kaybolacak.

* Makine durumunu sıfırlamak için bulut Kabuk araç çubuğunda yeniden başlatma simgesine tıklayın.

## <a name="minimize--maximize-cloud-shell-window"></a>Simge Durumuna Küçült & Bulut Kabuk penceresinin ekranı kaplamasını sağlayın
![](media/using-the-shell-window/minmax.png)
* Üst simge durumuna küçült simgesine gizlemek için pencerenin sağ. Yeniden göstermek için bulut Kabuk simgesine tıklayın.
* Max yüksekliğini penceresini ayarlamak için Ekranı Kapla simgesine tıklayın. Pencere önceki durumuna döndürmek için Geri Yükleme'yi tıklatın.

## <a name="concurrent-sessions"></a>Eşzamanlı oturum
Bulut Kabuk ayrı bir Bash işlem olarak bulunmasını her oturum vererek tarayıcı sekmelerde birden fazla eşzamanlı oturum sağlar.
Bir oturum çıkmadan, her işlem aynı makine üzerinde çalışsa bağımsız olarak çalışan her oturum penceresinden çıkmak emin olun.

## <a name="copy-and-paste"></a>Kopyala ve Yapıştır
[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

## <a name="resize-cloud-shell-window"></a>Bulut Kabuğu penceresini yeniden boyutlandırın
* ' I tıklatın ve araç üst kenarı yukarı veya aşağı boyutlandırma bulut Kabuğu penceresini sürükleyin.

## <a name="scrolling-text-display"></a>Kayan metin görüntüleme
* Fare veya terminal metni taşımak için dokunmatik ile kaydırın.

## <a name="changing-the-text-size"></a>Metin boyutunu değiştirme
![](media/using-the-shell-window/text-size.png)
* Pencerenin üst ayarlar simgesine sol tıklayın sonra "Metin boyutu" seçenek üzerine getirin ve istenen metin boyutu seçin. Seçiminiz oturumlarında kalıcı.

## <a name="exit-command"></a>Çıkış komutu
Çalışan `exit` etkin oturum sona erer. Bu davranışı varsayılan olarak etkileşimi olmadan 20 dakika sonra gerçekleşir.

## <a name="next-steps"></a>Sonraki adımlar

[Bulut Kabuk hızlı başlangıcı bash](quickstart.md)
[PowerShell içinde bulut Kabuk hızlı başlangıç](quickstart-powershell.md)