---
title: "Azure bulut Kabuğu (Önizleme) genel bakış | Microsoft Docs"
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
ms.date: 11/02/2017
ms.author: juluk
ms.openlocfilehash: 3acea56ea414f0c43333a02274e91226db29d454
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="overview-of-azure-cloud-shell-preview"></a>Azure bulut Kabuğu (Önizleme) genel bakış
Azure bulut Kabuk Azure kaynaklarını yönetmek için etkileşimli, tarayıcı erişilebilir bir kabuk ' dir.
Bu çalışma şeklinize uygun kabuk deneyimi seçme esnekliğini size verir.
Linux kullanıcıları Bash deneyimini, Windows kullanıcıları ise PowerShell’i tercih edebilir.

Bulut Kabuk simgesinden Azure portalı üzerinden başlatın:

![Portal başlatma](media/overview/portal-launch-icon.png)

Bash veya PowerShell Kabuk Seçici açılan listeden yararlanan:

![Bulut Kabuğu'nda bash](media/overview/overview-bash-pic.png)

![PowerShell bulut Kabuğu](media/overview/overview-ps-pic.png)

## <a name="features"></a>Özellikler
### <a name="browser-based-shell-experience"></a>Kabuk tarayıcı tabanlı deneyimi
Bulut Kabuk aklınızda Azure yönetim görevleri ile yerleşik bir tarayıcı tabanlı komut satırı deneyimi erişmesini sağlar.
Bir yalnızca bulut yolla yerel makineden untethered çalışması için Dengeleme bulut Kabuk sağlayabilir.

### <a name="choice-of-preferred-shell-experience"></a>Tercih edilen Kabuğu deneyiminin seçimi
Azure Cloud Shell, çalışma biçiminize en uygun kabuk deneyimini seçme esnekliği sunar.
Linux kullanıcıları Bash deneyimini, Windows kullanıcıları ise PowerShell’i tercih edebilir.

### <a name="pre-configured-azure-workstation"></a>Önceden yapılandırılmış Azure iş istasyonu
Bulut Kabuk popüler komut satırı araçları ile önceden yüklü olarak gelen ve daha hızlı çalışabilmeniz için dil desteği.

Tam araç listesini görüntülemek [Bash deneyimi](features.md#tools) ve [PowerShell deneyimi.](features-powershell.md#tools)

### <a name="automatic-authentication"></a>Otomatik kimlik doğrulama
Bulut Kabuğu güvenli bir şekilde otomatik olarak her oturum için anlık kaynaklarınıza erişmek için Azure CLI 2.0 veya Azure PowerShell cmdlet'leri aracılığıyla üzerinde kimliğini doğrular.

### <a name="connect-your-azure-file-storage"></a>Azure dosya depolama birimini bağlayın
Bulut Kabuk makine geçicidir ve sonuç olarak bir Azure dosyaları paylaşımı olarak bağlanmasını gerektiren `clouddrive` $Home dizininize kalıcı hale getirmek için.
Bir kaynak oluşturmak için bulut Kabuk ister üzerinde ilk kez başlatıldığında, sizin adınıza grup, depolama hesabı ve dosya paylaşımı. Bu tek seferlik bir adımdır ve tüm oturumları için otomatik olarak eklenir. Tek bir dosya paylaşımı eşlenebilir ve Bash ve bulut kabuğunda PowerShell tarafından kullanılır.

#### <a name="create-new-storage"></a>Yeni depolama alanı oluşturma
![](media/overview/basic-storage.png)

Bir yerel olarak yedekli depolama (LRS) hesabı ve Azure dosya paylaşımı sizin adınıza oluşturulabilir. Her ikisi de kullanmayı seçerseniz Azure dosya paylaşımı Bash ve PowerShell ortamları için kullanılır. Normal depolama ücretleri.

Üç kaynakları sizin adınıza oluşturulacak:
1. Kaynak grubu adı:`cloud-shell-storage-<region>`
2. Adlı depolama hesabı:`cs<uniqueGuid>`
3. Adlı dosya paylaşımı:`cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Bulut Kabuk bash'te da bir varsayılan 5 GB disk yansımasını oluşturur `$Home`. SSH anahtarları gibi $Home dizininizdeki tüm dosyalara, bağlı dosya paylaşımında depolanan kullanıcı disk görüntünüzdeki kalıcıdır. Dosyaları, $Home dizin ve bağlı dosya paylaşımı kaydedilirken en iyi yöntemleri uygulayın.

#### <a name="use-existing-resources"></a>Var olan kaynakları kullanın
![](media/overview/advanced-storage.png)

Gelişmiş bir seçenek bulut Kabuğu mevcut kaynaklarla ilişkilendirmek için sağlanır.
"Göster Gelişmiş ayarları" depolama Kurulum isteminde tıklatın ek seçenekleri göstermek için.
Bırakmalar atanmış olan bulut Kabuk bölge ve yerel olarak/genel-yedekli depolama hesapları için filtrelenir.

[Öğrenin bulut Kabuk depolama hakkında dosya paylaşımları güncelleştirme ve karşıya yükleme ve indirme dosyaları.](persisting-shell-storage.md)

## <a name="concepts"></a>Kavramlar
* Bulut Kabuk bir oturuma özgü üzerinde kullanıcı başına sağlanan geçici bir makinede çalıştırır
* Bulut Kabuk etkileşimli etkinliği olmadan 20 dakika sonra zaman aşımına uğradı
* Bulut Kabuk yalnızca bağlı bir dosya paylaşımı ile erişilebilir
* Kabuk kullanan bulut bir Bash ve PowerShell için aynı dosya paylaşımı
* Bulut Kabuk atanmış bir makine her kullanıcı hesabı
* İzinler normal Linux kullanıcı (Bash) ayarlanır

Özellikleri hakkında daha fazla bilgi [bulut Kabuğu'nda Bash](features.md) ve [bulut Kabuk PowerShell'de](features-powershell.md).

## <a name="examples"></a>Örnekler
* Azure yönetim görevlerini otomatikleştirmek için komut dosyası kullanma
* Aynı anda Azure portalı ve Azure komut satırı araçları aracılığıyla Azure kaynaklarını yönetme
* Azure CLI 2.0 veya Azure PowerShell cmdlet'leri dediğini

Hızlı Başlangıç ipuçları için bu örneklerde denemek [bulut Kabuğu'nda Bash](quickstart.md) ve [bulut Kabuk PowerShell'de](quickstart-powershell.md).

## <a name="pricing"></a>Fiyatlandırma
Bulut Kabuk barındıran makine, bir önkoşul bağlı Azure dosya paylaşımı ile ücretsizdir. Normal depolama ücretleri.

## <a name="supported-browsers"></a>Desteklenen tarayıcılar
Bulut Kabuk Chrome, sınır ve Safari için önerilir.
Bulut Kabuk Chrome, Firefox, Safari, IE ve kenar desteklense de, bulut Kabuk belirli tarayıcı ayarları tabi değil.
