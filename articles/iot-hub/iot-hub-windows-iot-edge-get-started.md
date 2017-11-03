---
title: "Azure IOT kenar (Windows) ile çalışmaya başlama | Microsoft Docs"
description: "Nasıl bir Windows makinesinde bir Azure IOT sınır ağ geçidi oluşturmak ve modülleri ve JSON yapılandırma dosyaları gibi Azure IOT edge'de temel kavramları hakkında bilgi edinin."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 9aff3724-5e4e-40ec-b95a-d00df4f590c5
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/29/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 262963250921ceb6f1a2fed0f68c36a9f052b47b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>Windows Azure IOT kenar mimarisi keşfedin

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="run-the-sample"></a>Örnek çalıştırın

**Build.cmd** komut dosyası oluşturur, çıktı **yapı** yerel kopyasını klasöründe **IOT kenar** deposu. Bu çıktı, birçok dosyaları içerir, ancak bu örnek üç üzerinde odaklanır:
- İki IOT kenar modülleri: **yapı\\modülleri\\Günlükçü\\hata ayıklama\\logger.dll** ve **yapı\\modülleri\\hello_world\\ Hata ayıklama\\hello\_world.dll**. 
- Bir yürütülebilir dosya: **yapı\\örnekleri\\hello\_world\\hata ayıklama\\hello\_world\_sample.exe**. Bu işlem, bir komut satırı bağımsız değişkeni bir JSON yapılandırma dosyası alır.

Bu örnekte kullandığınız dördüncü dosya oluşturma klasöründe değil, ancak IOT kenar havuzu klonlanmış zaman dahil:
- Bir JSON yapılandırma dosyası: **örnekleri\\hello\_world\\src\\hello\_world\_win.json**. Bu dosya iki modülleri yolları içerir. Ayrıca, logger.dll çıktısını için nereye yazdığını bildirir. Varsayılan değer **log.txt** , geçerli çalışma dizininde. 

   >[!NOTE]
   >Örnek modülleri taşımak veya test için kendi ekleme, güncelleştirme **module.path** eşleşecek şekilde yapılandırma dosyasındaki değerleri. Dizine göre modülü yollardır nerede **hello\_world\_sample.exe** bulunur. 

Örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Gidin **yapı** yerel kopyasını kök klasöründe **IOT kenar** deposu.

1. Şu komutu çalıştırın:

   ```cmd
   samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
   ```

1. Aşağıdaki çıktı örneği başarıyla çalıştığı anlamına gelir:

   ```cmd
   gateway successfully created from JSON
   gateway shall run until ENTER is pressed
   ```
  
1. Tuşuna **Enter** işlemi durdurmak için anahtar.


[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
