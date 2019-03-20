---
title: Proje akustik genel bakış
titlesuffix: Azure Cognitive Services
description: Proje akustik etkileşimli bir tasarımı denetimleri ile bir akustik desteklenmiş altyapısı tümleştirme 3B etkileşimli deneyimleri için wave fizik benzetimi ' dir.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 3d99ea5767c7b2e62f7228440201b4a9b6593b02
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58136609"
---
# <a name="what-is-project-acoustics"></a>Project Acoustics nedir?
Proje akustik bir 3B etkileşimli deneyimleri için wave akustik altyapısıdır. Karmaşık sahneler diffraction portaling ve yankı efektleri gibi wave etkileri, el ile bölgeye biçimlendirme gerek kalmadan modelleri. Ayrıca, oyun altyapısı ve ses ara yazılım tümleştirme de içerir. Proje akustik felsefesi statik Aydınlatmanın benzerdir: fiziksel temel sağlamak için çevrimdışı ayrıntılı fizik hazırlama ve basit bir çalışma zamanı ifadesel tasarım denetimlerle Artistik hedeflerinize ulaşmak için kullanabilirsiniz.

![Tasarım Görünümü](media/gears-with-voxels.jpg)

## <a name="using-wave-physics-for-interactive-acoustics"></a>Etkileşimli akustik için Wave fizik kullanma
Ray tabanlı akustik yöntemleri için tek kaynak dinleyici ray atama kullanarak kapatma denetleyin veya birkaç ışınlarındaki yerel Sahne birimle tahmin ederek Yankı sürücü. Ancak bir pebble bir boulder artacak occludes çünkü bu teknikler güvenilir olabilir. Işınlarındaki nesneleri, şekilde ses kıvrımla diffraction bilinen bir olguya hesabı yok. Proje akustik benzetimi wave tabanlı benzetimini kullanarak bu etkileri yakalar. , Daha öngörülebilir ve güvenilir sonucudur.

Geleneksel ses tasarım kavramları akustik benzetimi birkaç proje akustik anahtar yenilik sağlamaktır. Kapatma, portaling ve yankı için geleneksel ses DSP parametrelerine simülasyon sonuçlarını çevirir. Bu çeviri işlemi üzerinde denetimleri Tasarımcı kullanır. Proje akustik arkasında çekirdek teknolojiler hakkında daha fazla bilgi için ziyaret [araştırma proje sayfası](https://www.microsoft.com/en-us/research/project/project-triton/).

![Tasarım Görünümü](media/wave-simulation.gif)

## <a name="setup"></a>Kurulum
[Proje akustik Unity tümleştirmesi](unity-integration.md) sürükle ve bırak ve Unity Ses altyapısı eklentisi içerir. Bir proje akustik ekleyerek Unity ses kaynağından denetimleri büyütmek C# ses her nesne için denetimleri bileşeni.

[Proje akustik Unreal tümleştirme](unreal-integration.md) Unreal ve Wwise mixer eklentisi için düzenleyici ve oyun eklentileri içerir. Özel bir ses bileşen tasarım denetimleri Canlı akustik ile Unreal içinde tanıdık Wwise işlevselliği genişletir. Tasarım denetimleri mixer eklentisine Wwise içinde kullanıma sunulur.

## <a name="workflow"></a>İş akışı
* **Önceden hazırlama:** Hangi geometri seçerek hazırlama ayarı ile başlangıç akustik için örneğin ışık Miller yoksayılıyor yanıt verir. Ardından, otomatik malzeme atamaları ve gezinti alanları Kılavuzu dinleyicisine örnekleme seçerek düzenleyin. Yankı/portal/odası bölgeler için el ile biçimlendirme yok yoktur.
* **Hazırlama:** Bir analiz adım voxelization ve yukarıdaki seçimlerine göre Sahne geometrik diğer çözümleme yapar, yerel olarak çalıştırılır. Sonuçları Sahne kurulumunu doğrulama düzenleyicisinde görselleştirilmiştir. Hazırlama gönderimi voxel verileri Azure'a gönderilen devre dışı ve akustik oyun varlık ulaşırsınız.
* **Çalışma zamanı:** Varlık düzeyinizi yüklemek ve sizin düzeyinizde içinde akustik dinlemek hazırsınız. Kaynak başına ayrıntılı denetimlerini kullanarak düzenleyicide Canlı Tasarım akustik. Denetimleri de düzeyi komut temelli.

## <a name="platforms"></a>Platformlar
Proje akustik çalışma zamanı eklentileri, şu anda aşağıdaki platformlara dağıtılabilir:
* Windows
* Android
* Xbox One

## <a name="download"></a>İndirme
* [Proje akustik eklentileri ve örnekleri](https://www.microsoft.com/en-us/download/details.aspx?id=57346)
  * Kaydolma formu aşağıdaki Xbox ikili dosyalar ve destek için bize ulaşın
* [Proje akustik forumları](https://social.msdn.microsoft.com/Forums/en-US/home?forum=projectacoustics)
* [Proje akustik güncelleştirmeleri almak için kaydolun](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u)

## <a name="next-steps"></a>Sonraki adımlar
* Deneyin bir [Unity proje akustik hızlı](unity-quickstart.md) veya [Unreal](unreal-quickstart.md)
* Keşfedin [ses proje akustik, tasarımı felsefesi](design-process.md)

