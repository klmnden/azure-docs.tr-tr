---
title: Unity ile proje akustik hızlı
titlesuffix: Azure Cognitive Services
description: Örnek içeriği kullanma, tasarım denetimleri Unity proje akustik ile denemeler yapın ve Windows Masaüstü için dağıtın.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 1c790e0fa726c719d5b888d42b5f59739777566b
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64917112"
---
# <a name="project-acoustics-unity-quickstart"></a>Proje akustik Unity hızlı başlangıç
Kullanım proje akustik içeriğin benzetim destekli tasarım denetimleriyle denemek Unity için örnek.

Yazılım gereksinimleri:
* [Unity 2018.2 +](https://unity3d.com) Windows için
* [Proje akustik örnek içerik paketi](https://www.microsoft.com/download/details.aspx?id=57346)

Ne örnek paketine dahildir?
* Unity Sahne geometri, ses kaynakları ve oyun denetimleri
* Proje akustik eklentisi 
* Sahne için oluşturulan akustik varlıklar

## <a name="import-the-sample-package"></a>Örnek paketi içeri aktarma
Örnek paketi için yeni bir Unity proje içeri aktarın. 
* Unity içinde Git **varlıklar > paketi İçeri Aktar > özel paket...**

    ![Ekran görüntüsü, Unity paketini İçeri Aktar seçenekleri](media/import-package.png)  

* Seçin **ProjectAcoustics.unitypackage**

Varolan bir projeye paketi içe aktarıyorsanız, bkz. [Unity tümleştirmesi](unity-integration.md) için ek adımlar ve notlar.

## <a name="restart-unity"></a>Unity yeniden başlatın
Akustik Araç Seti hazırlama kısmı .NET 4.x komut dosyası çalışma zamanı sürümünü gerektirir. Paketi içeri aktarma, Unity oynatıcı ayarlarını güncelleştirir. Etkili olması için bu ayar için Unity yeniden başlatın.

Bu ayar etkin açarak sürdü doğrulayabilirsiniz **Player ayarları**:

![Unity Player ayarları ekran paneli](media/player-settings.png)

![.NET 4. 5'in seçili paneliyle Unity Player ayarları ekran görüntüsü](media/net45.png)

## <a name="experiment-with-design-controls"></a>Tasarım denetimleri ile denemeler yapın
Örnek sahnede açın **ProjectAcousticsSample** klasörü ve Unity Düzenleyicisi'nde Oynat düğmesini tıklatın. Kullanım W, A, S, D ve fareyi hareket etmek için. Sahne ve akustik olmadan nasıl sesleri Karşılaştırılacak basın **R** katmana metin kırmızıya döner ve diyor kadar düğmesi "Akustik: Devre dışı bırakıldı." Diğer denetimlerin klavye kısayollarını görmek için **F1** tuşuna basın. Ayrıca gerçekleştirmek için bir eylem seçmek için sağ tıklayarak sonra eylemi gerçekleştirmek için sol tıklayarak kalmayacak denetimlerdir.

Betik **AcousticsAdjust** kaynak başına tasarım parametrelerini sağlayan örnek görünümde ses kaynaklarına bağlı. 

![Unity AcousticsAdjust ekran komut dosyası](media/acoustics-adjust.png)

Aşağıda bazı sağlanan denetimleriyle üretilen efektleri keşfediyor. Her denetimi ile ilgili ayrıntılı bilgi için bkz. [proje akustik Unity tasarım öğretici](unreal-workflow.md).

### <a name="modify-distance-based-attenuation"></a>Uzaklık tabanlı zayıflama değiştirme
Ses DSP sağladığı **proje akustik** Unity spatializer eklentisinin Unity düzenleyicide yerleşik olarak bulunan kaynak başına uzaklık tabanlı zayıflama dikkate alır. Denetimler için uzaklık tabanlı zayıflama bulunduğunuz **ses kaynak** bileşeni bulunan **denetçisi** altında ses panelinden kaynakları **3B ses ayarları**:

![Ekran görüntüsü, Unity uzaklık zayıflama Seçenekleri paneli](media/distance-attenuation.png)

Proje akustik player konum etrafındaki ortalanmış bir "benzetimi bölge" kutusu hesaplama gerçekleştirir. Örnek paketinin akustik varlıkları 45 player çevreleyen milyon simülasyonu bölge boyutuna sahip desteklenmiş olduğundan, ses zayıflama 0 olarak yaklaşık 45 m kalan şekilde tasarlanmalıdır.

### <a name="modify-occlusion-and-transmission"></a>Kapatma ve iletim değiştirme
* Varsa **kapatma** çarpanı (varsayılan değer 1) 1'den büyük olduğundan, kapatma exaggerated. 1'den küçük yapar ayarlama kapatma efekt daha hafif.

* Wall ile aktarımını etkinleştirmek için taşıma **iletim (dB)** kaydırıcı, en düşük düzey kapalı. 

### <a name="modify-wetness-for-a-source"></a>Bir kaynak için wetness değiştirme
* Nasıl hızlı bir şekilde wetness uzaklığı ile değişiklikleri değiştirmek için kullanın **Algısal uzaklık Warp**. **Proje akustik** ıslak düzgün uzaklığı ile değişir ve Algısal uzaklık ipuçlarını sağlama düzeylerine benzetimi alanından boyunca hesaplar. Uzaklık warp artırılması, bu etkiyi uzaklık ilgili ıslak düzeyleri artırarak arttırdığınızda. 1 değerinden çarpıtma değerleri değiştirme uzaklığı tabanlı reverberation daha hafif olun. Bu etkiyi ayrıca daha ayrıntılı ayrıntılı olarak ayarlayarak ayarlanabilir **Wetness (dB)**.

* Ayarlayarak boşluğu boyunca decay süresini artırmak **Decay zaman ölçeği**. Belirli kaynak dinleyici konumu çifti için benzetim sonucu 1.5s, decay saati ise ve **Decay zaman ölçeği** ayarlı 2'ye kaynağa decay 3s zamandır.

## <a name="next-steps"></a>Sonraki adımlar
* Tüm ayrıntıları okuyun [Unity tabanlı proje akustik denetimleri tasarlama](unity-workflow.md)
* Kavramları daha iyi keşfedilebilmesi [tasarım işlemi](design-process.md)
* [Bir Azure hesabı oluşturun](create-azure-account.md) ön hazırlama keşfedin ve hazırlama işlemleri

