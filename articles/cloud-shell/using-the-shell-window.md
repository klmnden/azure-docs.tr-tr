---
title: Azure Cloud Shell penceresini kullanarak | Microsoft Docs
description: Azure Cloud Shell penceresini kullanma genel bakış.
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: damaerte
ms.openlocfilehash: a02642540e6eb39f35b9cc0d38d187a7afa36b7a
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243457"
---
# <a name="using-the-azure-cloud-shell-window"></a>Azure Cloud Shell penceresini kullanma

Bu belge, Cloud Shell penceresini kullanmayı açıklar.

## <a name="swap-between-bash-and-powershell-environments"></a>Bash ve PowerShell ortamlar arasında değiştirme
![](media/using-the-shell-window/env-selector.png)

Ortam Seçici Cloud Shell araç çubuğundaki Bash ve PowerShell ortamlar arasında takas etmek için kullanın.

## <a name="restart-cloud-shell"></a>Cloud Shell'i yeniden başlat
![](media/using-the-shell-window/restart.png)
> [!WARNING]
> Cloud Shell'i yeniden başlatmak makinenin durumu sıfırlanır ve tüm dosyaları, Azure tarafından kalıcı değil dosya paylaşımı kaybolacak.

* Makine durumunu sıfırlamak için Cloud Shell araç çubuğundaki yeniden simgeye tıklayın.

## <a name="minimize--maximize-cloud-shell-window"></a>Simge Durumuna Küçült & Cloud Shell Ekranı Kapla
![](media/using-the-shell-window/minmax.png)
* Üst simge durumuna küçült simgesine tıklayın gizlemek için pencerenin sağ. Yeniden göstermek için Cloud Shell simgesine tıklayın.
* Yükseklik üst sınırı penceresini ayarlamak için Ekranı Kapla simgesine tıklayın. Pencere önceki durumuna döndürmek için Geri Yükleme'ye tıklayın.

## <a name="concurrent-sessions"></a>Eş zamanlı oturum
Cloud Shell, her bir oturum ayrı bir Bash işlem vererek tarayıcı sekmeler arasında birden fazla eşzamanlı oturum sağlar.
Bir oturumundan çıkılıyor yaparsanız, bunlar aynı makine üzerinde çalıştırmasına rağmen her işlem bağımsız olarak çalışırken her oturum penceresinden çıkmak emin olun.

## <a name="copy-and-paste"></a>Kopyala ve Yapıştır
[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

## <a name="resize-cloud-shell-window"></a>Cloud Shell penceresini yeniden boyutlandırın
* Tıklayın ve yukarı veya aşağı Cloud Shell penceresini yeniden boyutlandırma araç çubuğunun üst kenarı sürükleyin.

## <a name="scrolling-text-display"></a>Kaydırma metin görüntüleme
* Fare veya terminal metni taşımak için dokunmatik kaydırın.

## <a name="changing-the-text-size"></a>Metin boyutunu değiştirme
![](media/using-the-shell-window/text-size.png)
* Pencerenin üst ayarlar simgesine sol tıklayın ardından "Metin boyutu" seçenek üzerine gelin ve istenen metin boyutunuzu seçin. Seçiminizi oturumu arasında kalıcıdır.

## <a name="exit-command"></a>Çıkış komutu
Çalışan `exit` etkin oturum sona erer. Bu davranış, etkileşim olmadan 20 dakika sonra varsayılan olarak gerçekleşir.

## <a name="next-steps"></a>Sonraki adımlar

[Bash cloud Shell hızlı başlangıçta](quickstart.md)
[PowerShell Cloud Shell hızlı başlangıç](quickstart-powershell.md)