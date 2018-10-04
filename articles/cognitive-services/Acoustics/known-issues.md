---
title: Akustik eklentisi - Bilişsel hizmetler ile ilgili bilinen sorunlar
description: Tasarımcısı Önizleme için proje akustik kullanırken aşağıdaki bilinen sorunlarla karşılaşabilirsiniz.
services: cognitive-services
author: kylestorck
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kylestorck
ms.openlocfilehash: e42a2f854d5d4fa72e17a0b75ffef55069ccb626
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48267878"
---
# <a name="known-issues"></a>Bilinen sorunlar
Tasarımcısı Önizleme için proje akustik kullanırken aşağıdaki bilinen sorunlarla karşılaşabilirsiniz.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Sahne yeniden adlandırdığınızda akustik parametreleri kaybolur

Sahne yeniden adlandırırsanız, bu görünüme ait akustik parametreleri yeni sahneye otomatik olarak aktarılmaz. Bunlar eski varlık dosyasını ancak varolmaya devam eder. Aranacak **SceneName_AcousticParameters.asset** içinde dosya **Düzenleyicisi** , Sahne dosyasını yanındaki dizin. Yeni bir sahne adını yansıtacak şekilde dosyanızı yeniden adlandırın.

## <a name="runtime-voxels-are-a-different-size-than-scene-preview-voxels"></a>Çalışma zamanı voxels Sahne Önizleme voxels değerinden farklı boyutta olan

Bunu yaparsanız bir **Calculate** üzerinde **araştırmaları** voxels farklı boyutlarda, sekme ve görünüm voxels sonra aynı Sahne için çalışma zamanında bir hazırlama ve görünüm voxels yapın. Önceden hazırlama gösterilen voxels benzetim kullanılan voxels ' dir. Çalışma zamanında gösterilen voxels araştırma noktaları arasında enterpolasyon için kullanılır. Bu portalları açık olmayan çalışma zamanında açık nerede görüneceğini bir tutarsızlık neden olabilir.

## <a name="unity-crashes-when-closing-project"></a>Unity proje kapatıldığında çöküyor

Unity (2018.2 +) en son sürümlerinde Unity projeniz kapattığınızda burada kilitlenir bilinen bir hata yoktur. Bu tarafından izlenen [bu Unity sorunu](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project).

## <a name="trouble-deploying-to-android"></a>Sorun Android'e dağıtmak için
Android'de proje akustik kullanmak için Android, derleme hedefini değiştirin. Unity'nün bazı sürümleri sahip ses eklentileri dağıtma ile bir hata--emin değil kullanmakta olduğunuz etkilenen bir sürüm [bu hatayı](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player).

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Bu 'meta veri dosyası System.Security.dll bulunamadı' bir hata alıyorum

Player ayarları Scripting çalışma zamanı sürümü ayarlandığından emin olun **.NET 4.x eşdeğer**, Unity yeniden başlatın.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Azure'a bağlanırken kimlik doğrulama sorunları yaşıyorum

Sağlayamazsanız doğru kimlik bilgilerini Azure hesabınız için hesabınızı hazırlama istenen düğüm türünü destekler ve Sistem saatiniz doğru olduğunu kullandınız.

## <a name="canceling-a-bake-leaves-the-bake-tab-in-deleting-state"></a>"Silinme durumunda" hazırlama sekmesi ayrıldığında bir hazırlama iptal ediliyor
Proje akustik temizleme tüm Azure kaynakları için bir iş, başarıyla tamamlandığında veya bu 5 dakika kadar sürebilir iptal eder.

## <a name="next-steps"></a>Sonraki adımlar
* [Unity projeniz için akustik tümleştirmesine](getting-started.md) başlayın

