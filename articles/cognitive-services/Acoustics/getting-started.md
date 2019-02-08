---
title: Proje akustik ile çalışmaya başlama
titlesuffix: Azure Cognitive Services
description: Bu Hızlı Başlangıç Kılavuzu, nasıl içinde Unity projenizde eklenti tümleştirme, hazırlama, Sahne ve akustik ses kaynakları için geçerlidir gösterilmektedir.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: a9cc8c7b4cdcc05d38bc0d68561fc9d86305b0cb
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55879923"
---
# <a name="getting-started-with-project-acoustics"></a>Proje akustik ile çalışmaya başlama
Bu Hızlı Başlangıç Kılavuzu, nasıl içinde Unity projenizde eklenti tümleştirme, hazırlama, Sahne ve akustik ses kaynakları için geçerlidir gösterilmektedir. Bu hızlı başlangıçta ilk oluşturmanız gerekecek bir [Azure batch hesabı](create-azure-account.md). Bu kılavuz, Unity bazı bilindiğini varsayar.

## <a name="download-the-plugin"></a>Eklentiyi indirin
Kayıt [burada](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u) Tasarımcısı önizlemeye katılmak için.

## <a name="supported-platforms-for-quickstart"></a>Hızlı Başlangıç için desteklenen platformlar
* [Unity 2018.2 +](http://www.unity3d.com)
  * Projenizi ayarlamak **.NET 4.x eşdeğer** scripting çalışma zamanı sürümü 
  * Windows tabanlı Unity editor gerektirir

## <a name="import-the-plugin"></a>Eklenti Al
Akustik UnityPackage projenize içeri aktarın. 
* Unity içinde Git **varlıklar > paketi İçeri Aktar > özel paket...**

![Paketi İçeri Aktar](media/ImportPackage.png)  

* Seçin **MicrosoftAcoustics.unitypackage**

Varolan bir projeye eklentisini içe aktarıyorsanız, projenize zaten olabilir bir **mcs.rsp** dosyasında proje kök dizinini, C# derleyici seçenekleri belirtir. Proje akustik eklentisi ile birlikte gelen mcs.rsp dosyayı bu dosyayla içeriğini birleştirmek gerekir.

## <a name="enable-the-plugin"></a>Eklentisini etkinleştirme
Akustik Araç Seti hazırlama kısmı .NET 4.x komut dosyası çalışma zamanı sürümünü gerektirir. Paketi içeri aktarma, Unity oynatıcı ayarlarını güncelleştirir. Etkili olması için bu ayar için Unity yeniden başlatın.

![Player ayarları](media/PlayerSettings.png)

![.NET 4.5](media/Net45.png)

## <a name="create-a-navigation-mesh"></a>Bir gezinti ağı oluşturun
Standart [Unity iş akışı](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html) projeniz için bir gezinti kafes oluşturmak için. Kendi gezinme kafesleri kullanma hakkında daha fazla bilgi için bkz: [UI izlenecek yol hazırlama](bake-ui-walkthrough.md).

## <a name="mark-static-meshes-for-acoustics"></a>Statik kafesleri akustik için işaretleyin
Akustik kullanarak penceresi Getir **penceresi > akustik** Unity. Bu pencere yanındaki denetçisi yerleştirilmiş olabilir.

![Açık akustik penceresi](media/WindowAcoustics.png)

Unity'nın hiyerarşi penceresinde, Seçili öğelerin seçimini. Akustik içinde **nesne** sekmesinde terrains akustik geometri olarak, görünümde ve kafesleri tümünü İşaretle "Akustik geometri" onay kutusuna tıklayın.

Üzerinde **malzemeleri** sekmesinde, malzemeler, sahnede kullanılan akustik malzemeleri atayın. **Varsayılan** malzeme içe alma için somut eşdeğer sahiptir. Kendi malzemeleri özelliklerini belirtme hakkında daha fazla bilgi için bkz. [tasarım işlemi sayfa](design-process.md).

![Malzemeleri sekmesi](media/MaterialsTab.png)

## <a name="preview-the-probes"></a>Araştırmaları Önizleme
Üzerinde **araştırmaları** sekmesinde **Calculate**. Bu hesaplama, Sahne boyutuna bağlı olarak birkaç dakika sürebilir. Hesaplama tamamlandığında, Sahne görünümünde "noktaları yoklama" olarak adlandırılan akustik benzetimi için konumları işaretlemek Küreler kayan görürsünüz. Sahne penceresinde bir nesneye yakındır alırsanız, Sahne voxelization de görebilirsiniz. Yeşil voxels geometri işaretlenmiş nesneleri ile hizalamak. Üst şeyler menüde voxel görüntüler ve araştırma noktaları değiştirilebilir Sahne görünümünde, sağ.

![GizmosPreview](media/BakePreviewWithGizmos.png)

## <a name="bake-the-scene"></a>Sahne hazırlama
İçinde **hazırlama** sekmesinde, Azure kimlik bilgilerinizi girin ve tıklayın **hazırlama**. Azure Batch hesabı yoksa bkz [bu kılavuzda, önerilen hesap kurulumu](create-azure-account.md).
Hazırlama tamamlandığında, veri dosyasına otomatik olarak indirilecek **varlıklar/AcousticsData** projenizdeki dizin.

## <a name="set-up-audio-runtime-dsp"></a>DSP ses çalışma zamanını oluşturan ayarlayın
Biz, ses DSP runtime akustik için Unity'nın spatializer framework ekleme ve HRTF tabanlı spatialization ile tümleştirin. Akustik işlemeyi etkinleştirmek için geçiş **Microsoft Acoustics** giderek spatializer **Düzenle > Proje Ayarları > Ses**seçip **Microsoft Acoustics** olarak **Spatializer eklentisi** projeniz için. Ayrıca, emin **DSP arabellek boyutu** en iyi performans için ayarlanmıştır.

![Proje ayarları](media/ProjectSettings.png)  

![Proje akustik Spatializer](media/ChooseSpatializer.png)

Ses Mixer açın (**penceresi > ses Mixer**). Bir grup ile en az bir Mixer olduğundan emin olun. Sağındaki '+' düğmesine tıklarsanız yoksa **Karıştırıcılar**. Kanal Şerit etkileri bölümünde alt sağ tıklayın ve Ekle **Microsoft akustik Mixer** efekt. Bir kerede yalnızca bir proje akustik Mixer desteklenip desteklenmediğini unutmayın.

![Ses Mixer](media/AudioMixer.png)

## <a name="set-up-the-acoustics-lookup-table"></a>Akustik arama tablosu ayarlama
Sürükle ve bırak **Microsoft Acoustics** sahneniz içine Proje panelinden prefab:

![Akustik Prefab](media/AcousticsPrefab.png)

Tıklayarak **ProjectAcoustics** Game nesne ve denetçisi panelini Git. Hazırlama sonuç (. konumunu belirtin ACE dosyası **varlıklar/AcousticsData**) sürükleme ve bırakma tarafından akustik Manager betiğe veya metin kutusunun yanındaki daireye düğmesine tıklayarak.

![Akustik Yöneticisi](media/AcousticsManager.png)  

## <a name="apply-acoustics-to-sound-sources"></a>Akustik ses kaynakları için geçerlidir
Bir ses kaynağı oluşturun. Bildiren AudioSource'nın denetçisi panelinin altındaki onay kutusuna tıklayın **Spatialize**. Emin **uzamsal Blend** tam 3B'ye ayarlanır.  

![Ses kaynağından](media/AudioSource.png)

## <a name="apply-post-bake-design"></a>Hazırlama sonrası Tasarım Uygula
Betiği ekleyebilirsiniz **AcousticsAdjust** ses kaynağına tıklayarak ek kaynak tasarım parametreleri etkinleştirmek için sahnedeki **Bileşen Ekle** seçip **betikleri > akustik Ayarla**:

![AcousticsAdjust](media/AcousticsAdjust.png)

Ayrıca parametre yok üzerinde **Microsoft akustik Mixer**. Hazırlama sonrası tasarımı hakkında daha fazla bilgi için bkz. [tasarım parametreleri](design-process.md).

## <a name="next-steps"></a>Sonraki adımlar
* Deneyin [örnek Sahne](sample-walkthrough.md)
* Eksiksiz bir listesi hakkında bilgi edinin [özellikleri hazırlama](bake-ui-walkthrough.md)
* Daha ayrıntılı incelemek [parametreleri tasarlama](design-process.md)

