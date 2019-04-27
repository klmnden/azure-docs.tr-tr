---
title: Akustik Unity tümleştirme ve dağıtım projesi
titlesuffix: Azure Cognitive Services
description: Bu nasıl yapılır Unity projeniz, proje akustik Unity eklentinin tümleştirmeye açıklar.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: how-to
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: aa7c5c513d65310bf9bffab29c1d18e7e7a85b49
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60389517"
---
# <a name="project-acoustics-unity-integration"></a>Project akustik Unity tümleştirmesi
Bu nasıl yapılır Unity projeniz, proje akustik Unity eklentinin tümleştirmeye açıklar.

Yazılım gereksinimleri:
* [Unity 2018.2 +](http://unity3d.com) Windows için
* [Proje akustik Unity paketini](https://www.microsoft.com/download/details.aspx?id=57346)

## <a name="import-the-plugin"></a>Eklenti Al
Akustik UnityPackage projenize içeri aktarın. 
* Unity içinde Git **varlıklar > paketi İçeri Aktar > özel paket...**

    ![Unity içeri aktarma paketini ekran menü](media/import-package.png)  

* Seçin **ProjectAcoustics.unitypackage**

Varolan bir projeye eklentisini içe aktarıyorsanız, projenize zaten olabilir bir **mcs.rsp** dosyasında proje kök dizinini, C# derleyici seçenekleri belirtir. Proje akustik eklentisi ile birlikte gelen mcs.rsp dosyayı bu dosyayla içeriğini birleştirmek gerekir.

## <a name="enable-the-plugin"></a>Eklentisini etkinleştirme
Akustik Araç Seti hazırlama kısmı .NET 4.x komut dosyası çalışma zamanı sürümünü gerektirir. Paketi içeri aktarma, Unity oynatıcı ayarlarını güncelleştirir. Etkili olması için bu ayar için Unity yeniden başlatın.

![Unity Player ayarları ekran paneli](media/player-settings.png)

![.NET 4. 5'in seçili paneliyle Unity Player ayarları ekran görüntüsü](media/net45.png)

## <a name="set-up-audio-dsp"></a>Ses DSP ayarlayın
Proje akustik ses çalışma zamanı Unity Ses altyapısı spatializer altyapısına tümleştirilen DSP içerir. Bu, hem HRTF tabanlı ve kaydırma spatialization içerir. Unity ses ayarlarını kullanarak proje akustik DSP etkinleştirmek **Düzenle > Proje Ayarları > Ses**, ardından seçerek **proje akustik** olarak **Spatializereklentisi** projeniz için. Emin **DSP arabellek boyutu** en iyi performans için ayarlanmıştır.

![Unity proje ayarları ekran paneli](media/project-settings.png)  

![Seçilen proje akustik spatializer ile Unity Spatializer ekran ayarlar paneli](media/choose-spatializer.png)

Ses Mixer'ı açın (**penceresi > ses Mixer**). Bir grup ile en az bir Mixer olduğundan emin olun. Sağındaki '+' düğmesine tıklarsanız yoksa **Karıştırıcılar**. Kanal Şerit etkileri bölümünde alt sağ tıklayın ve Ekle **proje akustik Mixer** efekt. Bir kerede yalnızca bir proje akustik Mixer desteklenip desteklenmediğini unutmayın.

![Ekran görüntüsü, Unity ses karıştırıcı proje akustik barındırma Mixer](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Ses kaynaklarında akustik etkinleştir
Bir ses kaynağı oluşturun. Bildiren AudioSource'nın denetçisi panelinin altındaki onay kutusuna tıklayın **Spatialize**. Emin **uzamsal Blend** tam 3B'ye ayarlanır.  

![Unity ses kaynağı ekran paneli](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Akustik tasarım etkinleştir
Betik ekleme **AcousticsAdjust** ses kaynağına tıklayarak ek kaynak tasarım parametreleri etkinleştirmek için sahnedeki **Bileşen Ekle** seçip **betikleri > akustik Ayarla** :

![Unity AcousticsAdjust ekran komut dosyası](media/acoustics-adjust.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Sahneniz için Unity proje akustik ile hazırlama](unity-baking.md)
* [Azure Batch hesabı oluşturma](create-azure-account.md) buluttaki sahneniz hazırlama için
* Keşfedin [akustik Unity proje tasarım işlemi](unity-workflow.md).

