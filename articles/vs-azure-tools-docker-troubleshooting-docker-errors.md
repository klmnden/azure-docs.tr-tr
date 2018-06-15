---
title: Visual Studio kullanarak Windows Docker istemci hatalarında sorun giderme | Microsoft Docs
description: Visual Studio oluşturmak ve Visual Studio 2017 kullanarak web uygulamaları Docker Windows dağıtmak için kullanırken karşılaştığınız sorunları giderin.
services: azure-container-service
documentationcenter: na
author: devinb
manager: douge
editor: ''
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 10/13/2017
ms.author: devinb
ms.openlocfilehash: 90dd5df4a607568e2f3a60791da2948af7ce4e50
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
ms.locfileid: "24002939"
---
# <a name="troubleshoot-visual-studio-2017-development-with-docker"></a>Visual Studio 2017 geliştirme Docker ile ilgili sorunları giderme

Docker için Visual Studio Araçları ile çalışırken, oluşturma veya uygulamanızın hatalarını ayıklama sırasında sorunlarla karşılaşabilirsiniz. Aşağıda bazı genel sorun giderme adımları şunlardır.

## <a name="volume-sharing-is-not-enabled-enable-volume-sharing-in-the-docker-ce-for-windows-settings--linux-containers-only"></a>Birimi paylaşan etkin değil. Windows CE Docker ayarları (yalnızca Linux kapsayıcıları) paylaşımı birimi etkinleştirilemedi

Bu sorunu gidermek için:

1. Sağ **Windows için Docker** bildirim alanına ve ardından **ayarları**.
1. Seçin **paylaşılan sürücüleri** ve projeyi bulunduğu sürücü yanı sıra sistem sürücüsünün paylaşın.

> [!NOTE]
> Dosyaları paylaşılan görünüyorsa, birimi paylaşan yeniden etkinleştirmek için iletişim kutusunun altındaki "sıfırlama kimlik..." bağlantısını tıklatın gerekebilir.

![Paylaşılan sürücüler](./media/vs-azure-tools-docker-troubleshooting-docker-errors/shareddrives.png)

## <a name="unable-to-start-debugging"></a>Hata ayıklama başlatılamıyor

Nedenlerinden biri, kullanıcı profili klasörünüzde eski hata ayıklama bileşenleri zorunda ilgili olabilir. Böylece en son hata ayıklama bileşenleri sonraki hata ayıklama oturumunda indirilir bu klasörleri kaldırmak için aşağıdaki komutları yürütün.

- DEL %userprofile%\vsdbg
- DEL %userprofile%\onecoremsvsmon

## <a name="errors-specific-to-networking-when-debugging-your-application"></a>Uygulamanızı hata ayıklama sırasında ağa belirli hataları

Dan indirilebilir komut dosyası yürütme deneyin [temizleme kapsayıcı konak ağ](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/master/windows-server-container-tools/CleanupContainerHostNetworking), hangi ana makinenizde ağla ilgili bileşenler Yenile.


## <a name="microsoftdockertools-github-repo"></a>Microsoft/DockerTools GitHub depo

Karşılaştığınız diğer herhangi bir sorun için bkz: [Microsoft/DockerTools](https://github.com/microsoft/dockertools/issues) sorunları.