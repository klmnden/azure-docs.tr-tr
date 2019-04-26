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
ms.date: 04/15/2019
ms.author: damaerte
ms.openlocfilehash: 2511f2c8fb706e232cde9ee4c02c7f8114bd3a2b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60200709"
---
# <a name="using-the-azure-cloud-shell-window"></a>Azure Cloud Shell penceresini kullanma

Bu belge, Cloud Shell penceresini kullanmayı açıklar.

## <a name="swap-between-bash-and-powershell-environments"></a>Bash ve PowerShell ortamlar arasında değiştirme

Ortam Seçici Cloud Shell araç çubuğundaki Bash ve PowerShell ortamlar arasında takas etmek için kullanın.  
![Ortam seçin](media/using-the-shell-window/env-selector.png)

## <a name="restart-cloud-shell"></a>Cloud Shell'i yeniden başlat
Makine durumunu sıfırlamak için Cloud Shell araç çubuğundaki yeniden simgeye tıklayın.  
![Cloud Shell'i yeniden başlat](media/using-the-shell-window/restart.png)
> [!WARNING]
> Cloud Shell'i yeniden başlatmak makinenin durumu sıfırlanır ve tüm dosyaları, Azure tarafından kalıcı değil dosya paylaşımı kaybolacak.

## <a name="change-the-text-size"></a>Metin boyutunu değiştirme
Pencerenin üst ayarlar simgesine sol tıklayın ardından "Metin boyutu" seçenek üzerine gelin ve istenen metin boyutunuzu seçin. Seçiminizi oturumu arasında kalıcıdır.
![Metin boyutu](media/using-the-shell-window/text-size.png)

## <a name="change-the-font"></a>Yazı tipini değiştirme
Pencerenin üst ayarlar simgesine sol tıklayın ardından "Yazı tipi" seçeneği gelin ve istenen yazı tipini seçin.  Seçiminizi oturumu arasında kalıcıdır.
![Yazı tipi](media/using-the-shell-window/text-font.png)

## <a name="upload-and-download-files"></a>Dosyaları yükleme ve indirme
Pencerenin üst kısmında karşıya yükleme/indirme dosyaları simgesine sol tıklayın, karşıya yükleme veya indirme ardından seçin.  
![Dosyaları karşıya yükleme/indirme](media/using-the-shell-window/uploaddownload.png)
* Dosyayı yerel bilgisayarınıza göz atın, istediğiniz dosyayı seçin ve "Aç" düğmesine tıklayın, açılır dosya yükleme için kullanın.  İçine dosyasının karşıya yükleneceğini `/home/user` dizin.
* Dosyaları yükleme, açılan pencereye tam dosya yolunu girin ve "İndir" düğmesini seçin.  
> [!NOTE] 
> Dosyalar ve dosya yolları Cloud Shell'de büyük küçük harfe duyarlı. Double, büyük/küçük harf, dosya yolunda denetleyin.

## <a name="open-another-cloud-shell-window"></a>Başka bir Cloud Shell penceresini açın
Cloud Shell, her bir oturum ayrı bir işlem sağlayarak, tarayıcı sekmelerde birden fazla eşzamanlı oturum sağlar.
Bir oturumundan çıkılıyor yaparsanız, bunlar aynı makine üzerinde çalıştırmasına rağmen her işlem bağımsız olarak çalışırken her oturum penceresinden çıkmak emin olun.  
Pencerenin üst kısmında yeni oturum aç simgesine sol tıklayın. Var olan kapsayıcıya bağlı başka bir oturum ile yeni bir sekmede açılır.
![Yeni oturum aç](media/using-the-shell-window/newsession.png)

## <a name="cloud-shell-editor"></a>Cloud Shell Düzenleyicisi
* Başvurmak [Azure Cloud Shell Düzenleyicisi'ni kullanarak](using-cloud-shell-editor.md) sayfası.

## <a name="web-preview"></a>Web önizlemesi
Üst kısmında web Önizleme simgesini pencerenin sol, "Yapılandır"'ı seçin, açmak istediğiniz bağlantı noktasını belirtin.  Seçin ya da "açık bağlantı noktası" yalnızca bağlantı noktasını açın veya "açın ve gidin" bağlantı noktasını açın ve bağlantı noktasını yeni bir sekmede önizlemek için.  
![Web önizlemesi](media/using-the-shell-window/preview.png)  
<br>
![Bağlantı noktası yapılandırma](media/using-the-shell-window/preview-configure.png)  
Pencerenin üst kısmında web önizlemesini sol tıklayın seçin "bağlantı noktası Preview..." açık bir bağlantı noktasına yeni bir sekmede önizlemek için. Pencerenin üst kısmında web önizlemesini sol tıklayın seçin "bağlantı noktasını Kapat …" açık bağlantı noktasını kapatmak için.  
![Bağlantı noktası Önizleme/Kapat](media/using-the-shell-window/preview-options.png)

## <a name="minimize--maximize-cloud-shell-window"></a>Simge Durumuna Küçült & Cloud Shell Ekranı Kapla
Üst simge durumuna küçült simgesine tıklayın gizlemek için pencerenin sağ. Yeniden göstermek için Cloud Shell simgesine tıklayın.
Yükseklik üst sınırı penceresini ayarlamak için Ekranı Kapla simgesine tıklayın. Pencere önceki durumuna döndürmek için Geri Yükleme'ye tıklayın.  
![Simge durumuna küçült ya da Ekranı Kapla](media/using-the-shell-window/minmax.png)

## <a name="copy-and-paste"></a>Kopyala ve Yapıştır
[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

## <a name="resize-cloud-shell-window"></a>Cloud Shell penceresini yeniden boyutlandırın
Tıklayın ve yukarı veya aşağı Cloud Shell penceresini yeniden boyutlandırma araç çubuğunun üst kenarı sürükleyin.

## <a name="scrolling-text-display"></a>Kaydırma metin görüntüleme
Fare veya terminal metni taşımak için dokunmatik kaydırın.

## <a name="exit-command"></a>Çıkış komutu
Çalışan `exit` etkin oturum sona erer. Bu davranış, etkileşim olmadan 20 dakika sonra varsayılan olarak gerçekleşir.

## <a name="next-steps"></a>Sonraki adımlar

[Bash cloud Shell hızlı başlangıçta](quickstart.md) <br>
[PowerShell Cloud Shell hızlı başlangıç](quickstart-powershell.md)