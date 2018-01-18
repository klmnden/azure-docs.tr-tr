---
title: "Azure bulut kabuğuna genel bakış | Microsoft Docs"
description: "Azure bulut Kabuk genel bakış."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: juluk
ms.openlocfilehash: b710c324f72fa56a2ebad0d1b35052639611d30d
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="overview-of-azure-cloud-shell"></a>Azure bulut Kabuk genel bakış
Azure bulut Kabuk Azure kaynaklarını yönetmek için etkileşimli, tarayıcı erişilebilir bir kabuk ' dir.
Çalışma şeklinize uygun kabuk deneyimi seçme esnekliğini size sağlar.
Linux kullanıcıları Bash deneyimini, Windows kullanıcıları ise PowerShell’i tercih edebilir.

Bu düğme kullanarak shell.azure.com deneyin.

[![](https://shell.azure.com/images/launchcloudshell.png "Azure bulut Kabuğu'nu başlatın")](https://shell.azure.com)

Bulut Kabuk simgesini kullanarak Azure portalından deneyin.

![Portal başlatma](media/overview/portal-launch-icon.png)

## <a name="features"></a>Özellikler
### <a name="browser-based-shell-experience"></a>Kabuk tarayıcı tabanlı deneyimi
Bulut Kabuk aklınızda Azure yönetim görevleri ile yerleşik bir tarayıcı tabanlı komut satırı deneyimi erişmesini sağlar.
Bir yalnızca bulut yolla yerel makineden untethered çalışması için Dengeleme bulut Kabuk sağlayabilir.

### <a name="choice-of-preferred-shell-experience"></a>Tercih edilen Kabuğu deneyiminin seçimi
Windows kullanıcıları bulut Kabuğu'nda (Önizleme) PowerShell kullanabilirsiniz, ancak Linux kullanıcıları Kabuk aşağı açılır listeden bulut Kabuk Bash'te kullanabilirsiniz.

![Bulut Kabuğu'nda bash](media/overview/overview-bash-pic.png)

![PowerShell bulut Kabuğu (Önizleme)](media/overview/overview-ps-pic.png)

### <a name="authenticated-and-configured-azure-workstation"></a>Kimliği doğrulanmış ve yapılandırılmış Azure iş istasyonu
Yaygın komut satırı araçları ve dil desteği gelecektir bulut Kabuk Microsoft tarafından yönetilir. Bulut Kabuk de güvenli bir şekilde otomatik olarak anlık kaynaklarınıza erişmek için Azure CLI 2.0 veya Azure PowerShell cmdlet'leri aracılığıyla için kimliğini doğrular.

Tam araç listesini görüntülemek [Bash deneyimi](features.md#tools) ve [PowerShell (Önizleme) deneyimi.](features-powershell.md#tools)

### <a name="multiple-access-points"></a>Birden çok erişim noktası
Bulut Kabuk alanından kullanılabilen esnek bir araçtır:
* [portal.azure.com](https://portal.azure.com)
* [shell.azure.com](https://shell.azure.com)
* [Azure CLI 2.0 "Deneyin" belgeleri](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest)
* [Azure mobil uygulaması](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [VS Code Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Microsoft Azure dosyaları depolama birimini bağlayın
Bulut Kabuk makine geçicidir ve bir Azure dosyaları paylaşımı olarak bağlanmasını gerektiren `clouddrive` dosyalarınızı kalıcı hale getirmek için.

Bir kaynak oluşturmak için bulut Kabuk ister ilk başlatılırken grubu, depolama hesabı ve Azure dosyaları sizin adınıza paylaşır. Bu tek seferlik bir adımdır ve tüm oturumları için otomatik olarak eklenir. Tek bir dosya paylaşımı eşlenebilir ve Bash ve PowerShell bulut Kabuğu (Önizleme) tarafından kullanılır.

#### <a name="create-new-storage"></a>Yeni depolama alanı oluşturma
![](media/overview/basic-storage.png)

Bir yerel olarak yedekli depolama (LRS) hesabı ve Azure dosya paylaşımı sizin adınıza oluşturulabilir. Her ikisi de kullanmayı seçerseniz Azure dosya paylaşımı Bash ve PowerShell ortamları için kullanılır. Normal depolama ücretleri.

Üç kaynakları sizin adınıza oluşturulacak:
1. Kaynak grubu adı:`cloud-shell-storage-<region>`
2. Adlı depolama hesabı:`cs<uniqueGuid>`
3. Adlı dosya paylaşımı:`cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Bulut Kabuk bash'te da bir varsayılan 5 GB disk yansımasını oluşturur `$Home`. SSH anahtarları gibi $Home dizininizdeki tüm dosyalara, bağlı Azure dosya paylaşımında depolanan kullanıcı disk görüntünüzdeki kalıcıdır. Dosyaları, $Home dizin ve bağlı Azure dosya paylaşımı kaydedilirken en iyi yöntemleri uygulayın.

#### <a name="use-existing-resources"></a>Var olan kaynakları kullanın
![](media/overview/advanced-storage.png)

Gelişmiş bir seçenek bulut Kabuğu mevcut kaynaklarla ilişkilendirmek için sağlanır.
"Göster Gelişmiş ayarları" depolama Kurulum isteminde tıklatın ek seçenekleri göstermek için.

> [!Note]
> Bırakmalar önceden atanmış bulut Kabuk bölge ve LRS/GRS depolama hesapları için filtrelenir.

[Öğrenin bulut Kabuk depolama hakkında Azure dosya paylaşımları güncelleştirme ve karşıya yükleme ve indirme dosyaları.](persisting-shell-storage.md)

## <a name="concepts"></a>Kavramlar
* Bir oturum başına üzerinde kullanıcı başına sağlanan geçici bir ana bilgisayarda bulut Kabuk çalıştırır
* Bulut Kabuk etkileşimli etkinliği olmadan 20 dakika sonra zaman aşımına uğradı
* Bulut Kabuk takılması Azure dosya paylaşımının gerektirir
* Bulut Kabuk aynı Azure dosya paylaşımı için Bash ve PowerShell kullanır
* Bulut Kabuk atanmış bir makine her kullanıcı hesabı
* Dosya paylaşımınızı tutulan 5GB görüntüsünü kullanarak $Home bash devam ettirir
* İzinler normal Linux kullanıcı Bash olarak ayarlanır

Özellikleri hakkında daha fazla bilgi [bulut Kabuğu'nda Bash](features.md) ve [PowerShell bulut Kabuğu (Önizleme)](features-powershell.md).

## <a name="pricing"></a>Fiyatlandırma
Bulut Kabuk barındıran makine, bir önkoşul bağlı Azure dosya paylaşımı ile ücretsizdir. Normal depolama ücretleri.

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Kabuk hızlı başlangıcı bash](quickstart.md) <br>
[PowerShell içinde bulut Kabuğu (Önizleme) hızlı başlangıç](quickstart-powershell.md)