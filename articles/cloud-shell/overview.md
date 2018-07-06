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
ms.date: 02/15/2018
ms.author: juluk
ms.openlocfilehash: 4ee02bc2a1956994da0ba49a24eefabf9608565c
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856469"
---
# <a name="overview-of-azure-cloud-shell"></a>Azure Cloud shell'e genel bakış
Azure Cloud Shell'i Azure kaynaklarını yönetmek için etkileşimli ve tarayıcı erişilebilir bir kabuktur.
Bu, çalışma biçiminize en uygun kabuk deneyimini seçme esnekliği sağlar.
Linux kullanıcıları Bash deneyimini, Windows kullanıcıları ise PowerShell’i tercih edebilir.

Bu düğmeyi kullanarak shell.azure.com deneyin.

[![](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell'i Başlat")](https://shell.azure.com)

Cloud Shell simgesini kullanarak Azure portalından deneyin.

![Portal başlatma](media/overview/portal-launch-icon.png)

## <a name="features"></a>Özellikler
### <a name="browser-based-shell-experience"></a>Tarayıcı tabanlı kabuk deneyimi
Cloud Shell, Azure yönetim görevlerini aklınızda ile oluşturulmuş bir tarayıcı tabanlı komut satırı deneyimi erişim sağlar.
Bir şekilde yalnızca bulut yerel makineden untethered çalışmanız yararlanarak Cloud Shell sağlayabilir.

### <a name="choice-of-preferred-shell-experience"></a>Tercih edilen bir kabuk deneyimi seçimi
Windows kullanıcıları (Önizleme) Cloud Shell'de PowerShell kullanabilirsiniz, ancak Linux kullanıcıları Kabuk açılan menüden Cloud Shell'deki Bash hizmetinde kullanabilirsiniz.

![Cloud Shell'de bash](media/overview/overview-bash-pic.png)

![(Önizleme) Cloud shell'de PowerShell](media/overview/overview-ps-pic.png)

### <a name="authenticated-and-configured-azure-workstation"></a>Kimliği doğrulanmış ve yapılandırılmış Azure iş istasyonu
Cloud Shell'i Microsoft tarafından yönetiliyorsa, popüler komut satırı araçları ve dil desteği ile gelecektir. Cloud Shell'i de güvenli bir şekilde otomatik olarak Azure CLI 2.0 veya Azure PowerShell cmdlet'leri aracılığıyla kaynaklarınıza hızlı erişim için kimlik doğrulaması yapar.

Tamamını görüntülemek [Araçlar listesi.](features.md#tools)

### <a name="multiple-access-points"></a>Birden çok erişim noktası
Cloud Shell'i gelen kullanılabilen esnek bir araçtır:
* [portal.azure.com](https://portal.azure.com)
* [shell.azure.com](https://shell.azure.com)
* [Azure CLI 2.0 belgelerine "Deneyin"](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)
* [Azure mobil uygulaması](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [VS Code Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Microsoft Azure dosya depolama bağlama
Cloud Shell makine geçicidir ve takılamadı için bir Azure dosya paylaşımı gerektiren `clouddrive` dosyalarınızı kalıcı hale getirmek için.

Bir kaynak oluşturmak için cloud Shell ister ilk başlatma sırasında sizin adınıza grubu, depolama hesabı ve Azure dosyaları paylaşın. Bu tek seferlik bir adımdır ve tüm oturumları için otomatik olarak eklenir. Tek bir dosya paylaşımı eşlenebilir ve hem Bash hem PowerShell Cloud shell'de (Önizleme) tarafından kullanılır.

#### <a name="create-new-storage"></a>Yeni depolama oluşturma
![](media/overview/basic-storage.png)

Yerel olarak yedekli depolama (LRS) hesabı ve Azure dosyaları paylaşım sizin adınıza oluşturulabilir. Her ikisini birden kullanmayı tercih ederseniz, Azure dosyaları paylaşım hem Bash hem PowerShell ortamlar için kullanılır. Normal depolama ücretleri.

Sizin adınıza üç kaynak oluşturulacak:
1. Kaynak grubu adı: `cloud-shell-storage-<region>`
2. Adlı depolama hesabı: `cs<uniqueGuid>`
3. Adlı dosya paylaşımı: `cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Cloud Shell'deki bash hizmetinde aynı zamanda bir varsayılan 5 GB'lik bir disk yansımasını oluşturur `$Home`. Bağlı Azure dosya paylaşımınızı depolanan kullanıcı disk görüntünüze $Home dizininizin SSH anahtarları gibi tüm dosyalarda kalıcıdır. Dosyaları, $Home dizininizin ve bağlı Azure dosya paylaşımı kaydedilirken en iyi yöntemleri uygulayın.

#### <a name="use-existing-resources"></a>Var olan kaynakları kullan
![](media/overview/advanced-storage.png)

Cloud Shell mevcut kaynaklara ilişkilendirmek için Gelişmiş bir seçenek sağlanır.
Depolama Kurulum isteminde "Show Gelişmiş ayarları"'a tıklayın. ek seçenekleri görmek için.

> [!Note]
> Açılır menüleri kullanarak, önceden atanmış Cloud Shell bölgesi ve GRS/LRS/ZRS depolama hesapları için filtrelenir.

[Bilgi Cloud Shell depolama hakkında dosyaları Azure dosya paylaşımlarını güncelleştiriliyor ve karşıya yükleme ve indirme.](persisting-shell-storage.md)

## <a name="concepts"></a>Kavramlar
* Cloud Shell'i bir başına-oturum, kullanıcı başına sağlanan geçici bir konak üzerinde çalışır
* Cloud Shell'de etkileşimli bir etkinliği olmayan 20 dakika sonra zaman aşımına uğruyor
* Cloud Shell'i Azure dosya paylaşımını bağlanmasını gerektirir.
* Hem Bash hem PowerShell için cloud Shell aynı Azure dosya paylaşımını kullanır
* Cloud Shell'de bir makine kullanıcı hesabı başına atanır
* Cloud Shell'i kullanarak dosya paylaşımınızdaki tutulan bir 5 GB'lık görüntü $Home devam ettirir.
* Bash normal Linux kullanıcı olarak izinleri ayarlayın

Özellikleri hakkında daha fazla bilgi edinin [Cloud Shell'de Bash](features.md) ve [(Önizleme) Cloud shell'de PowerShell](features-powershell.md).

## <a name="pricing"></a>Fiyatlandırma
Cloud Shell barındıran makine önkoşul bağlı bir Azure dosya paylaşımının ile ücretsiz olarak kullanılabilir. Normal depolama ücretleri.

## <a name="next-steps"></a>Sonraki adımlar
[Bash cloud Shell hızlı başlangıçta](quickstart.md) <br>
[PowerShell Cloud Shell'i (Önizleme) hızlı](quickstart-powershell.md)