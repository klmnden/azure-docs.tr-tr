---
title: Azure Cloud shell'e genel bakış | Microsoft Docs
description: Azure Cloud shell'e genel bakış.
services: ''
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
ms.date: 09/04/2018
ms.author: damaerte
ms.openlocfilehash: 5608b3e0f9b98db62d22245de5a864f757f48799
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60199690"
---
# <a name="overview-of-azure-cloud-shell"></a>Azure Cloud shell'e genel bakış
Azure Cloud Shell'i Azure kaynaklarını yönetmek için etkileşimli ve tarayıcı erişilebilir bir kabuktur.
Bu, çalışma biçiminize en uygun kabuk deneyimini seçme esnekliği sağlar.
Linux kullanıcıları Bash deneyimini, Windows kullanıcıları ise PowerShell’i tercih edebilir.

Aşağıda tıklayarak Shell.Azure.com deneyin.

[![Ekleme başlatma](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell'i Başlat")](https://shell.azure.com)

Cloud Shell simgesini kullanarak Azure portalından deneyin.

![Portal başlatma](media/overview/portal-launch-icon.png)

## <a name="features"></a>Özellikler

### <a name="browser-based-shell-experience"></a>Tarayıcı tabanlı kabuk deneyimi
Cloud Shell, Azure yönetim görevlerini aklınızda ile oluşturulmuş bir tarayıcı tabanlı komut satırı deneyimi erişim sağlar.
Bir şekilde yalnızca bulut yerel makineden untethered çalışmanız yararlanarak Cloud Shell sağlayabilir.

### <a name="choice-of-preferred-shell-experience"></a>Tercih edilen bir kabuk deneyimi seçimi
Kullanıcılar, Bash veya PowerShell arasında Kabuk açılan listeden seçebilirsiniz.

![Cloud Shell'de bash](media/overview/overview-bash-pic.png)

![Cloud Shell’de PowerShell](media/overview/overview-ps-pic.png)

### <a name="authenticated-and-configured-azure-workstation"></a>Kimliği doğrulanmış ve yapılandırılmış Azure iş istasyonu
Cloud Shell'i Microsoft tarafından yönetiliyorsa, popüler komut satırı araçları ve dil desteği ile gelecektir. Cloud Shell'i de güvenli bir şekilde otomatik olarak Azure CLI veya Azure PowerShell cmdlet'leri aracılığıyla kaynaklarınıza hızlı erişim için kimlik doğrulaması yapar.

Tamamını görüntülemek [Cloud Shell'de yüklü Araçlar listesi.](features.md#tools)

### <a name="integrated-cloud-shell-editor"></a>Tümleşik Cloud Shell Düzenleyicisi
Cloud Shell'i üzerinde açık kaynak Monaco düzenleyicisine alan bir Tümleşik Grafik metin düzenleyici sunar. Yalnızca oluşturup çalıştırarak yapılandırma dosyalarını düzenleyebilirsiniz `code .` için Azure CLI veya Azure PowerShell ile sorunsuz dağıtım.

[Cloud Shell Düzenleyicisi hakkında daha fazla bilgi](using-cloud-shell-editor.md).

### <a name="integrated-with-docsmicrosoftcom"></a>Docs.microsoft.com ile tümleşik

Cloud Shell doğrudan barındırılan belgelerinde kullanabileceğiniz [docs.microsoft.com](https://docs.microsoft.com). İçinde tümleşik [Microsoft Learn](https://docs.microsoft.com/learn/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) ve [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure) -bir kod parçacığında, sürükleyici Kabuğu'nu açmak için "Try It" düğmesine tıklayın deneyimi. 

### <a name="multiple-access-points"></a>Birden çok erişim noktası
Cloud Shell'i gelen kullanılabilen esnek bir araçtır:
* [portal.azure.com](https://portal.azure.com)
* [shell.azure.com](https://shell.azure.com)
* [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure)
* [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/overview)
* [Azure mobil uygulaması](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [Visual Studio Code Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Microsoft Azure dosya depolama bağlama
Cloud Shell makine geçicidir ve takılamadı için bir yeni veya var olan Azure dosya paylaşımı gerektiren `clouddrive` dosyalarınızı kalıcı hale getirmek için.

Bir kaynak oluşturmak için cloud Shell ister ilk başlatma sırasında sizin adınıza grubu, depolama hesabı ve Azure dosyaları paylaşın. Bu tek seferlik bir adımdır ve tüm oturumları için otomatik olarak eklenir. Tek bir dosya paylaşımı eşlenebilir ve hem Bash hem PowerShell Cloud shell'de tarafından kullanılır.

Bağlama hakkında bilgi edinmek için daha fazla bilgi edinin bir [yeni veya mevcut bir depolama hesabı](persisting-shell-storage.md).

## <a name="concepts"></a>Kavramlar
* Cloud Shell'i bir başına-oturum, kullanıcı başına sağlanan geçici bir konak üzerinde çalışır
* Cloud Shell'de etkileşimli bir etkinliği olmayan 20 dakika sonra zaman aşımına uğruyor
* Cloud Shell'i Azure dosya paylaşımını bağlanmasını gerektirir.
* Hem Bash hem PowerShell için cloud Shell aynı Azure dosya paylaşımını kullanır
* Cloud Shell'de bir makine kullanıcı hesabı başına atanır
* Cloud Shell'i kullanarak dosya paylaşımınızdaki tutulan bir 5 GB'lık görüntü $HOME devam ettirir.
* Bash normal Linux kullanıcı olarak izinleri ayarlayın

Özellikleri hakkında daha fazla bilgi edinin [Cloud Shell'de Bash](features.md) ve [Cloud Shell'deki PowerShell hizmetinde](features-powershell.md).

## <a name="pricing"></a>Fiyatlandırma
Cloud Shell barındıran makine önkoşul bağlı bir Azure dosya paylaşımının ile ücretsiz olarak kullanılabilir. Normal depolama ücretleri.

## <a name="next-steps"></a>Sonraki adımlar
[Bash cloud Shell hızlı başlangıçta](quickstart.md) <br>
[PowerShell Cloud Shell hızlı](quickstart-powershell.md)
