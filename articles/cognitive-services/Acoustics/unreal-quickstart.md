---
title: Proje akustik hızlı Unreal ile
titlesuffix: Azure Cognitive Services
description: Örnek içeriği kullanma, Unreal ve Wwise tasarım denetimlerinde proje akustik ile denemeler yapın ve Windows Masaüstü için dağıtma.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: a78df2d4d84487399da10ca722550639a92e71bf
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798135"
---
# <a name="project-acoustics-unrealwwise-quickstart"></a>Proje akustik Unreal/Wwise hızlı başlangıç
Bu hızlı başlangıçta, tasarım denetimleri Wwise ve Unreal Engine için sağlanan örnek içerik kullanarak proje akustik ile denemeler.

Yazılım gereksinimleri:
* [Unreal Engine](https://www.unrealengine.com/) 4.21
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2018.1.6

## <a name="download-the-sample-package"></a>Örnek paketi indirin
İndirme [proje akustik Unreal + Wwise örnek paket](https://www.microsoft.com/download/details.aspx?id=58090). Örnek paketi, Unreal Engine projesinde, Unreal proje ve proje akustik Wwise eklentisi Wwise proje içerir.

## <a name="set-up-the-project-acoustics-sample-project"></a>Proje akustik örnek projesi kurun
Proje akustik Unreal/Wwise örnek projeyi'kurmak için proje akustik eklentisi Wwise yüklemeniz gerekir. Unreal projeye Wwise ikili dosyaları dağıttıktan sonra Wwise'nın Unreal eklentisi projesi akustik destekleyecek şekilde ayarlayın.

### <a name="install-the-project-acoustics-wwise-plugin"></a>Proje akustik Wwise eklentisini yükleme
Wwise başlatıcısı, ardından açın **eklentileri** sekmesindeki **yeni eklenti yükleme**seçin **Dizin Ekle**. Seçin `AcousticsWwisePlugin\ProjectAcoustics` indirdiğiniz paket içerisine dâhil dizin.

![Ekran görüntüsü, Wwise Wwise eklentisi yükleme seçeneğini gösteren Başlatıcısı](media/wwise-install-new-plugin.png)

### <a name="add-wwise-binaries-to-the-project-acoustics-unreal-sample-project"></a>Proje akustik Unreal örnek projeye Wwise ikilileri Ekle
Wwise başlatıcıdan tıklayın **Unreal Engine** sekmesine ve ardından hamburger menüsüne tıklayın **son Unreal Engine projeler** seçip **projesi için Gözat**. Örnek Unreal projesini `.uproject` paketteki dosyası `AcousticsSample\AcousticsGame\AcousticsGame.uproject`.

![Wwise başlatıcısı, ekran Unreal sekmesi](media/wwise-unreal-tab.png)

Proje akustik örnek proje yanındaki'a tıklayarak **tümleştirme Wwise projesinde**.

![Ekran görüntüsü, Wwise akustik oyun Unreal projeyi gösteren Başlatıcısı](media/wwise-acoustics-game-project.png)

### <a name="extend-wwises-unreal-plugin-functionality"></a>Wwise'nın Unreal eklentisi işlevselliğini genişletme
Ek davranış proje akustik Unreal eklenti gerektirir Wwise Unreal eklentisini API maruz bırakılmamalıdır. Bu değişiklikleri otomatik hale getirmek için proje akustik Unreal eklentisi ile sağlanan toplu iş dosyasını çalıştırın:
* İçinde `AcousticsGame\Plugins\ProjectAcoustics\Resources`çalıştırın `PatchWwise.bat`.

    ![Düzeltme eki Wwise projesine betik gösteren Windows Gezgini'nin ekran görüntüsü penceresi](media/patch-wwise-script.png)

* DirectX SDK'sı, kullanmakta olduğunuz, Wwise sürümüne bağlı olarak yüklü yoksa içeren satırı açıklama gerekebilir `DXSDK_DIR` içinde `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`:

    ![Kod Düzenleyicisi'ni yorum DXSDK gösteren ekran görüntüsü](media/directx-sdk-comment.png)

### <a name="open-the-unreal-project"></a>Unreal projeyi açın. 
Bu modüller yeniden istenir; Evet'e tıklayın.

>Derleme hataları üzerinde proje açma başarısız olursa, proje akustik Wwise eklentisi proje akustik örnek projesinde kullanılan Wwise aynı sürümünü yüklediğinizi denetleyin.

>Değil kullanıyorsanız [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2018.1.6, önce ses örnek projesinde yürütülecek ses bankalar yeniden gerekir.

## <a name="experiment-with-project-acoustics-design-controls"></a>Proje akustik tasarım denetimleri ile denemeler yapın
Unreal Düzenleyicisi denetimindeki yürütme düğmesine tıklayarak Sahne nasıl ses için dinleyin. Masaüstünde kullanın W, A, S, D ve fareyi hareket etmek için. Diğer denetimlerin klavye kısayollarını görmek için **F1** tuşuna basın. Denemek için bazı tasarım etkinlikler şunlardır:

### <a name="modify-occlusion-and-transmission"></a>Kapatma ve iletim değiştirme
Kaynak başına proje akustik tasarım denetimleri Unreal her ses aktör vardır:

![Unreal Düzenleyicisi ekran akustik tasarım denetimleri](media/demo-scene-sound-source-design-controls.png)

Varsa **kapatma** çarpanı (varsayılan değer 1) 1'den büyük olduğundan, kapatma exaggerated. 1'den küçük yapar ayarlama kapatma efekt daha hafif.

Wall ile aktarımını etkinleştirmek için taşıma **iletim (dB)** kaydırıcı, en düşük düzey kapalı. 

### <a name="modify-wetness-for-a-source"></a>Bir kaynak için wetness değiştirme
Nasıl hızlı bir şekilde wetness uzaklığı ile değişiklikleri değiştirmek için kullanın **Algısal uzaklık Warp**. Proje akustik düzgün uzaklığı ile değişir ve Algısal uzaklık ipuçlarını sağlama ıslak düzeylerine benzetimi alanından boyunca hesaplar. Uzaklık warp artırılması, bu etkiyi uzaklık ilgili ıslak düzeyleri artırarak arttırdığınızda. 1 değerinden çarpıtma değerleri değiştirme uzaklığı tabanlı reverberation daha hafif olun. Bu etkiyi ayrıca daha ayrıntılı ayrıntılı olarak ayarlayarak ayarlanabilir **Wetness (dB)** .

Ayarlayarak boşluğu boyunca decay süresini artırmak **Decay zaman ölçeği**. Benzetim sonucu 1.5 decay süresini olduğu bir durum düşünün s. Ayarı **Decay zaman ölçeği** 3 ' kaynağı için uygulanan bir decay saat 2'ye sonuçlanır s.

### <a name="modify-distance-based-attenuation"></a>Uzaklık tabanlı zayıflama değiştirme
Proje akustik Wwise mixer eklentisi Wwise içinde oluşturulan kaynak başına uzaklık tabanlı zayıflama dikkate alır. Bu eğri değiştirme kuru yolu düzeyini değiştirecek. Proje akustik eklenti benzetimi ve tasarım denetimleri tarafından belirtilen ıslak kuru karışımı korumak için ıslak düzeyini ayarlar.

![Benzetim sınır önce sıfır gidip zayıflama ekran görüntüsü, Wwise zayıflama eğri paneli](media/demo-sounds-attenuation.png)

Proje akustik "benzetimi bölge" kutusundaki her sanal player konum etrafındaki ortalanmış bir hesaplama gerçekleştirir. Örnek paketi akustik varlıkları 45 m bir simülasyon bölge RADIUS ile desteklenmiş ve attenuations 0 45 dk önce kalan için tasarlanmıştır. Bu azalma kesin bir gereklilik değildir; ancak yalnızca geometrisi dinleyicisinin 45 m içinde sesleri occlude uyarı taşır.

## <a name="next-steps"></a>Sonraki adımlar
* [Proje akustik tümleştirme](unreal-integration.md) Unreal projenize eklentisi
* Kendiniz içerik hazırlamak için [bir Azure hesabı oluşturun](create-azure-account.md)


