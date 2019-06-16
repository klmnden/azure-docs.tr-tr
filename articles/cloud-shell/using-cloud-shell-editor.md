---
title: Azure Cloud Shell Düzenleyicisi'ni kullanarak | Microsoft Docs
description: Azure Cloud Shell Düzenleyicisi'ni nasıl genel bakış.
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
ms.date: 07/24/2018
ms.author: damaerte
ms.openlocfilehash: 7f597bb5cba1a12bdb93325fe2b877ffc644e3e4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60199231"
---
# <a name="using-the-azure-cloud-shell-editor"></a>Azure Cloud Shell Düzenleyicisi'ni kullanarak

Azure Cloud Shell içeren açık kaynaklı yerleşik bir Tümleşik Dosya Düzenleyicisi [Monaco düzenleyicisine](https://github.com/Microsoft/monaco-editor). Cloud Shell Düzenleyicisi dil vurgulamasına gibi özellikler, komut paletini ve dosya Gezgini destekler.

![Cloud Shell Düzenleyicisi](media/using-cloud-shell-editor/open-editor.png)

## <a name="opening-the-editor"></a>Düzenleyicisini açma

Basit dosya oluşturma ve düzenleme için Düzenleyicisi'ni çalıştırarak başlatma `code .` Cloud Shell Terminal. Bu eylem, terminalde ayarlamak active directory'nizle çalışma Düzenleyicisi açılır.

Doğrudan hızlı düzenleme için bir dosya açmak için çalıştırın `code <filename>` dosya Gezgini olmadan düzenleyiciyi açın.

UI düğmesiyle Düzenleyicisi'ni açmak için `{}` araç çubuğundan Düzenleyicisi simgesi. Bu Düzenleyicisi'ni açın ve dosya Gezgini'ne varsayılan `/home/<user>` dizin.

## <a name="closing-the-editor"></a>Düzenleyici kapatma

Düzenleyiciyi kapatmak için açık `...` eylem panelinde üst sağında seçin ve düzenleyici `Close editor`.

![Düzenleyiciyi Kapat](media/using-cloud-shell-editor/close-editor.png)

## <a name="command-palette"></a>Komut paleti

Komut paletini başlatmak için `F1` odak düzenleyicide olarak ayarlandığında anahtar. Komut paletini açma eylem panelinde yapılabilir.

![Komut paleti](media/using-cloud-shell-editor/cmd-palette.png)

## <a name="contributing-to-the-monaco-editor"></a>Monaco düzenleyicisine katkıda bulunan

Cloud Shell düzenleyicisinde dil vurgulama desteği, Yukarı Akış işlevleri aracılığıyla desteklenir [Monaco düzenleyicisine](https://github.com/Microsoft/monaco-editor)kullanıcının Monarch söz dizimi tanımları kullanın. Katkı öğrenmek için okuyun [Monaco katkıda bulunan Kılavuzu'ndaki](https://github.com/Microsoft/monaco-editor/blob/master/CONTRIBUTING.md).

## <a name="next-steps"></a>Sonraki adımlar
[Cloud shell'de Bash için hızlı başlangıç kılavuzundan](quickstart.md)
[tümleşik Cloud Shell araçları tam listesini görüntüleyin](features.md)