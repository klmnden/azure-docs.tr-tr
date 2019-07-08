---
title: Proje akustik eklentisi ile ilgili bilinen sorunlar
titlesuffix: Azure Cognitive Services
description: Tasarımcısı Önizleme için proje akustik kullanırken aşağıdaki bilinen sorunlarla karşılaşabilirsiniz.
services: cognitive-services
author: kylestorck
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kylestorck
ms.openlocfilehash: 02156f6e39c213764c35e67d8af028489d265835
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616342"
---
# <a name="project-acoustics-known-issues"></a>Proje akustik bilinen sorunlar
Tasarımcısı Önizleme için proje akustik kullanırken aşağıdaki bilinen sorunlarla karşılaşabilirsiniz.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Sahne yeniden adlandırdığınızda akustik parametreleri kaybolur

Sahne yeniden adlandırırsanız, tüm bu görünüme ait akustik parametreleri otomatik olarak gerekmez yeni sahneye aktarın. Bunların hala eski varlık dosyasını ancak mevcut. Aranacak **SceneName_AcousticParameters.asset** içinde dosya **Düzenleyicisi** , Sahne dosyasını yanındaki dizin. Yeni bir sahne adını yansıtacak şekilde dosyanızı yeniden adlandırın.

## <a name="unity-crashes-when-closing-project"></a>Unity proje kapatıldığında çöküyor

Unity (2018.2 +) en son sürümlerinde Unity projeniz kapattığınızda burada kilitlenir bilinen bir hata yoktur. Bu tarafından izlenen [bu Unity sorunu](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project).

## <a name="deploying-to-android-from-some-unity-versions"></a>Bazı Unity sürümlerinden Android'e dağıtmak için

Unity'nün bazı sürümleri ile ses eklentileri Android'e dağıtmak için bir hata var. Etkilenen bir sürüm kullanmadığınız emin [bu hatayı](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player).

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Bu 'meta veri dosyası System.Security.dll bulunamadı' bir hata alıyorum

Player ayarları Scripting çalışma zamanı sürümü ayarlandığından emin olun **.NET 4.x eşdeğer**, Unity yeniden başlatın.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Azure'a bağlanırken kimlik doğrulama sorunları yaşıyorum

Sağlayamazsanız doğru kimlik bilgilerini Azure hesabınız için hesabınızı hazırlama istenen düğüm türünü destekler ve Sistem saatiniz doğru olduğunu kullandınız.

## <a name="canceling-a-bake-leaves-the-bake-tab-in-deleting-state"></a>"Silinme durumunda" hazırlama sekmesi ayrıldığında bir hazırlama iptal ediliyor
Proje akustik işi başarıyla tamamlandığında veya iptal için tüm Azure kaynakları temizler. Bu, 5 dakika kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
* Deneyin [Unity](unity-quickstart.md) veya [Unreal](unreal-quickstart.md) örnek içerik

