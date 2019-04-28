---
title: Proje akustik Unreal ve Wwise tümleştirme
titlesuffix: Azure Cognitive Services
description: Bu nasıl yapılır ve proje akustik Unreal Wwise eklentileri tümleştirme projenize açıklar.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: how-to
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: c6baa9f8330338c1e5fdc9ee0b5a8cc8b344e871
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61436139"
---
# <a name="project-acoustics-unreal-and-wwise-integration"></a>Proje akustik Unreal ve Wwise tümleştirme
Bu nasıl yapılır mevcut, Unreal ve Wwise game projeye proje akustik eklenti paketi ayrıntılı tümleştirme adımları sağlar. 

Yazılım gereksinimleri:
* [Unreal Engine](https://www.unrealengine.com/) 4.20 veya 4.21
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2018.1.\*
* [Unreal Wwise eklentisi](https://www.audiokinetic.com/library/?source=UE4&id=index.html)
  * Wwise Unreal eklentileri kullanmak yerine doğrudan bir tümleştirme Wwise SDK kullanıyorsanız, proje akustik Unreal eklentisi başvurun ve Wwise API çağrıları ayarlayın.

Proje akustik Wwise dışındaki bir ses altyapısıyla kullanmak istiyorsanız, aracılığıyla Bize Ulaşın [proje akustik forumları](https://social.msdn.microsoft.com/Forums/en-US/home?forum=projectacoustics). Akustik verileri sorgulamak ve ardından altyapınız için API çağrıları yapmak için proje akustik Unreal eklentisi kullanabilirsiniz.

## <a name="download-project-acoustics"></a>Proje akustik indirin
Henüz yüklemediyseniz, indirme [proje akustik Unreal & Wwise eklenti paketi](https://www.microsoft.com/download/details.aspx?id=58090)). 

Pakette bir Unreal Engine eklentisi ve Wwise mixer eklentisi ekledik. Unreal eklentisi Düzenleyicisi ve çalışma zamanı tümleştirme sağlar. Oyun sırasında oyun her nesne için kapatma gibi parametreleri her çerçeve proje akustik Unreal eklentisi hesaplar. Bu parametreleri Wwise API çağrısına çevrilir.

## <a name="review-integration-steps"></a>Tümleştirme adımları gözden geçirin

Paketi yüklemek ve oyununuzda katılımcılığı dağıtmak için bu temel adım vardır.
1. Proje akustik Wwise mixer eklentisini yükleme
2. (Yeniden) oyununuzu Wwise dağıtın. Bu adım, oyun projenize mixer eklentisi yayar.
3. Oyununuzu proje akustik Unreal Eklentisi Ekle
4. Wwise'nın Unreal eklentisi işlevselliğini genişletme
5. Oyun oluşturmak ve Python etkinleştirildiğinden emin olun
6. Proje akustik kullanılacak Wwise projenizi ayarlama
7. Unreal ses Kurulumu

## <a name="1-install-the-project-acoustics-mixer-plugin"></a>1. Proje akustik mixer eklentisini yükleme
* Wwise başlatıcısı, ardından açın **eklentileri** sekmesindeki **yeni eklenti yükleme**seçin **Dizin Ekle**. 

    ![Wwise başlatıcısında bir eklenti yükleme işleminin ekran görüntüsü](media/wwise-install-new-plugin.png)

* Seçin `AcousticsWwisePlugin\ProjectAcoustics` indirdiğiniz paket içerisine dâhil dizin. Wwise mixer eklenti paketi içeriyor.

* Wwise eklentisi yüklenir. Proje akustik artık Wwise yüklü eklentiler listesinde gösterilmesi gerekir.
![Proje akustik yüklemeden sonra eklentiler listesi yüklü Wwise ekran görüntüsü](media/unreal-integration-post-mixer-plugin-install.png)

## <a name="2-redeploy-wwise-into-your-game"></a>2. (Yeniden) Wwise oyununuzu dağıtma
Zaten Wwise entegre ettik bile Wwise oyununuzu yeniden dağıtın. Bu proje akustik Wwise eklentisi seçer.

* **Altyapısı Eklentisi:** Unreal C++ projesinde bir oyun eklentisi olarak yüklenen Wwise varsa, bu adımı atlayın. Unreal projenizi şema yalnızca Wwise dağıtım bizim mixer eklentisiyle örneği daha karmaşık olduğu için bunun yerine bir altyapısı eklentisi olarak yüklenirse. İşlevsiz, boş Unreal C++ projesi oluşturma, Unreal Düzenleyicisi açılırsa kapatın ve bu işlevsiz projeye Wwise dağıtmak için kalan yordamı izleyin. Ardından dağıtılan Wwise eklentisi kopyalayın.
 
* Wwise başlatıcıdan tıklayın **Unreal Engine** sekmesine ve ardından hamburger menüsüne tıklayın **son Unreal Engine projeler** seçip **projesi için Gözat**. Oyununuzun Unreal projesini `.uproject` dosya.

    ![Ekran görüntüsü, Wwise Başlatıcısı'nın Unreal sekmesi](media/wwise-unreal-tab.png)

* Ardından **tümleştirme Wwise projesinde** veya **değiştirme Wwise projesinde**. Bu adım (yeniden) Wwise ikili dosyaları projenize artık proje akustik mixer eklentiyi dahil olmak üzere tümleştirir.

* **Altyapısı Eklentisi:** Wwise bir altyapısı eklentisi kullanıyorsanız ve işlevsiz projeyi yukarıdaki olarak oluşturulan dağıtılan Wwise klasörüne kopyalayın: `[DummyUProject]\Plugins\Wwise` ve üzerine yapıştırın `[UESource]\Engine\Plugins\Wwise`. `[DummyUProject]` boş Unreal C++ proje yolu ve `[UESource]` yüklü Unreal Engine kaynaklarına sahip olduğu olduğu. İşiniz bittiğinde kopyalama işlevsiz projesini silebilirsiniz.

## <a name="3-add-the-project-acoustics-unreal-plugin-to-your-game"></a>3. Oyununuzu proje akustik Unreal Eklentisi Ekle
 
* Kopyalama `Unreal\ProjectAcoustics` eklenti klasöründe paketini ve yeni bir klasör oluşturun `[UProjectDir]\Plugins\ProjectAcoustics`burada `UProjectDir` olduğundan, oyun proje klasörünü içeren `.uproject` dosya.
  * **Altyapısı eklentisi**: Bir altyapı eklentisi Wwise kullanıyorsanız, proje akustik de bir Unreal Engine'i eklentisi kullanmanız gerekir. Yukarıdaki hedef dizin yerine kullanın: `[UESource]\Engine\Plugins\ProjectAcoustics`.

* Gördüğünüz onaylayın bir `Wwise` klasör yanı sıra `ProjectAcoustics` klasör. İkili dosyaları (re-), adım 2'de dağıtılan mixer eklentisi için birlikte Wwise eklentisi içeriyor.

## <a name="4-extend-wwises-unreal-plugin-functionality"></a>4. Wwise'nın Unreal eklentisi işlevselliğini genişletme
* Ek davranış proje akustik Unreal eklenti gerektirir ortaya Wwise Unreal eklentisi API öğesinden [bu yönergeleri](https://www.audiokinetic.com/library/?source=UE4&id=using__initialsetup.html). Düzeltme eki uygulayan yordama otomatikleştirmek için bir toplu iş dosyası ekledik. 
* İçinde `Plugins\ProjectAcoustics\Resources`çalıştırın `PatchWwise.bat`. Aşağıdaki örnek görüntüde AcousticsGame örnek Projemizin kullanır.

    ![Betik yama Wwise sağlanan Windows Gezgini'nin ekran görüntüsü penceresi vurgulama](media/patch-wwise-script.png)

* Yoksa, DirectX SDK'sı yüklü, içinde DXSDK_DIR içeren satırı açıklama satırı yapın gerekir `[UProject]\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`

    ![Kod Düzenleyicisi'ni yorum DXSDK gösteren ekran görüntüsü](media/directx-sdk-comment.png)

## <a name="5-build-game-and-check-python-is-enabled"></a>5. Oyun oluşturmak ve Python etkinleştirildiğinden emin olun

* Oyununuzu derleyin ve doğru bir şekilde derlendiğinden emin olun. Aksi takdirde, devam etmeden önce önceki adımları denetleyin. 
* Projenizi Unreal Düzenleyicisi'nde açın. 
* **Altyapısı Eklentisi:** ProjectAcoustics altyapısı eklentisi kullanıyorsanız, aynı zamanda etkinleştirilmiş olduğunu, altında "yerleşik" eklentileri listelenen emin olun.
* Proje akustik tümleşik gösteren yeni bir mod görmeniz gerekir.

    ![Ekran görüntüsü, akustik modu tam gösteren Unreal](media/acoustics-mode-full.png)

* Python eklentisi için Unreal etkin olduğunu onaylayın. Bu, Düzenleyicisi tümleştirmesinin düzgün çalışması için gereklidir.

    ![Python uzantıları Unreal düzenleyicisindeki etkinleştirme ekran görüntüsü](media/ensure-python.png)

## <a name="6-wwise-project-setup"></a>6. Wwise proje ayarları

Bir örnek Wwise proje örnekleri indirmeye dahil edilir. Bu yönergeler yanı sıra göz atabilirsiniz öneririz. Aşağıdaki ekran görüntüleri, bu projeden alınır.

### <a name="bus-setup"></a>Veri yolu kurulumu
* Bu veri yolundaki ilişkili mixer eklentisi projesi akustik Unreal eklentisi arar ***tam*** adı: `Project Acoustics Bus`. Bu ada sahip yeni bir ses bus oluşturun. Mixer eklentisi çeşitli yapılandırmalarda çalışabilir, ancak şimdilik yalnızca işleme Yankı için kullanılacak olan varsayıyoruz. Bu veri yolu, karma Yankı sinyal akustik kullanan tüm kaynakları için sahip olacaktır. Yukarı Akış yapısı karıştırma yoluna karıştırabilir miyim, bir örnek aşağıda örnek yüklemeye dahil Wwise örnek Projemizin alınan gösterilir.

    ![Proje akustik yol gösteren ekran görüntüsü, Wwise veri yolları](media/acoustics-bus.png)

* Kanal yapılandırmasını yolundaki birine ayarlanması gerekir: `1.0, 2.0, 4.0, 5.1 or 7.1`. Diğer yapılandırmaları bu veri yoluna hiçbir çıktı neden olur.

    ![Proje akustik Service Bus kanal yapılandırma seçeneklerinin ekran görüntüsü](media/acoustics-bus-channel-config.png)

* Proje akustik ayrıntıları veri yolu ve Mixer eklenti sekmesinde görebilirsiniz olun Git

    ![Proje akustik veri yolu için Mixer eklenti sekmesini etkinleştir gösteren Wwise ekran görüntüsü](media/mixer-tab-enable.png)

* Ardından Mixer eklenti sekmesine gidin ve eklenti projesi akustik mixer veri yoluna ekleyin

    ![Proje akustik Mixer eklenti ekleme gösteren, Screenshow Wwise veri yolu](media/add-mixer-plugin.png)

### <a name="actor-mixer-hierarchy-setup"></a>Aktör mixer hiyerarşisi Kurulumu
* Performansla ilgili nedenlerle, proje akustik ses DSP tüm kaynaklarına eşzamanlı olarak uygulanır. Bu, bir mixer eklenti çalışması için eklenti gerektirir. Çıkış yolu genellikle kuru çıkış sinyal taşıyan ancak Wwise mixer eklentileri çıkış yolunda olmasını gerektirir. Proje akustik gerektirir kuru sinyal ıslak sinyal taşınan sırasında yedek veri yolları yönlendirilir `Project Acoustics Bus`. Aşağıdaki işlem bu sinyal akış aşamalı geçişi destekler.

* Varsayalım, var olan bir projeyi geçirmiş, durumlarını ve diğer üst düzey içeren bir aktör mixer hiyerarşisi. Her alt kuru karışımı için karşılık gelen çıkış veri yolu vardır. Akustik kullanılacak geçirmiş geçirmek istediğiniz varsayalım sağlar. İlk alt geçirmiş çıkış yolu bunların kuru submix yürütmek için karşılık gelen bir yedek bus oluşturun. Tam adı önemli değildir ancak örneği için "Kuru" öneki aşağıdaki resimde, bunları düzenlemek için kullandık. Herhangi bir ölçümleri veya geçirmiş veri yoluna sahip etkileri önceki gibi çalışmaya devam eder.

    ![Önerilen Wwise kuru karışımı Kurulum görüntüsü](media/wwise-dry-mix-setup.png)

* Veri yolu çıktı yapısını geçirmiş aktör Mixer'ı şu şekilde değiştirin, proje akustik çıkış yolu ve Dry_Footsteps veri yolu ile bir yedek kullanıcı tanımlı yol ayarlayın.

    ![Önerilen Wwise aktör Mixer Bus Kurulum görüntüsü](media/actor-mixer-bus-settings.png)

* Artık tüm geçirmiş akustik işleme alın ve bunların Yankı proje akustik yolundaki çıktı. Kuru sinyal Dry_Footsteps yönlendirilir ve zamanki spatialized.

* Proje akustik yalnızca dünyada 3B bir konuma sahip ses için geçerlidir. Aşağıdaki [Wwise belgeleri](https://blog.audiokinetic.com/out-with-the-old-in-with-the-new-positioning-revamped-in-wwise-2018.1/), konumlandırma özelliklerini ayarlanmalıdır gösterildiği gibi. "3B Spatialization" ayarı gerektiğinde "Konum" veya "Konum + yön" olabilir.

    ![Önerilen Wwise aktör konumlandırma ayarlarının ekran görüntüsü](media/wwise-positioning.png)

* Çıkış yolu Yukarı Akış içine karıştırır bazı diğer bir yolu olarak ayarlanması **proje akustik Bus** çalışmaz. Bu gereksinimi temel mixer eklentileri Wwise uygular.

* Akustik kullanmayı geçirmiş aktör mixer hiyerarşideki bir alt istiyorsanız, her zaman "üst üzerinde geçersiz" geri çevirmek için kullanabilirsiniz.

* Oyun veya kullanıcı tanımlı gönderir oyun herhangi aktör mixer üzerinde Yankı için kullanıyorsanız, bunları bir bu aktör-Yankı iki kez uygulanmasını önlemek için mixer üzerinde kapatın.

### <a name="spatialization"></a>Spatialization
Varsayılan olarak, proje akustik Wwise mixer eklentisi evrişim Yankı, kaydırma spatialization yapmak için Wwise bırakarak uygular. Proje akustik bu varsayılan yalnızca Yankı yapılandırmada kullanılırken, karıştırın ve eşleştirin proje akustik yankı ile neredeyse tüm spatializer olanak tanıyan kuru, karışımı üzerinde herhangi bir kanal yapılandırmasını ve spatialization yöntemi kullanmak ücretsiz. Seçenekleriniz şunlardır [Ambisonics tabanlı binaural spatializers](https://www.audiokinetic.com/products/ambisonics-in-wwise/) ve [Windows Sonic](https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound).
 
Proje akustik hem nesne tabanlı, yüksek çözünürlüklü HRTF işleme ve kaydırma destekleyen bir isteğe bağlı spatializer içerir. Mixer eklenti ayarları "Spatialization gerçekleştirmek" onay HRTF veya kaydırma arasında seçim yapın ve yukarıda tüm kuru veri yolları için iki kez hem proje akustik mixer eklentisi ve Wwise spatializing önlemek için belirlenen kullanıcı tanımlı aux gönderir devre dışı bırakın. Ses banka anahtarınızın yeniden oluşturulması gerektiğinden spatialization modu gerçek zamanlı olarak değiştirilemez. Unreal yeniden başlatın ardından soundbanks mixer eklentisi yapılandırma değişiklikleri gibi 'Gerçekleştirmek Spatialization' onay kutusunu seçmek için play ulaşmaktan önce yeniden oluştur.

![Ekran Wwise Mixer eklentisi Spatialization ayarları](media/mixer-spatial-settings.png)

Ne yazık ki, diğer nesne tabanlı spatializer eklentileri mixer eklentiler uygulanan ve Wwise şu anda tek bir aktör-mixer atanmış birden çok mixer eklentileri izin şu anda desteklenemiyor.  

## <a name="7-audio-setup-in-unreal"></a>7. Unreal ses Kurulumu
* İlk yerleştirileceği bir akustik varlık oluşturmak için oyun düzeyinizi hazırlama gerekecektir `Content\Acoustics`. Başvurun [Unreal hazırlama Öğreticisi](unreal-baking.md) ve buradan devam edin. Önceden oluşturulan bazı düzeyleri örnek pakete dahil edilir.
* Akustik alanı aktör, sahnede oluşturun. Yalnızca tam düzeyi için akustik temsil ettiğinden bu aktörler birini bir düzeyinde oluşturun. 

    ![Akustik alanı aktör oluşturulmasını gösteren Unreal ekran Düzenleyicisi](media/create-acoustics-space.png)

* Şimdi oluşturulan akustik veri varlığına akustik alanı aktör akustik verilerini yuvada atayın. Sahneniz akustik sunuyor!

    ![Unreal ekran Düzenleyicisi s howing akustik varlık atama](media/acoustics-asset-assign.png)

* Şimdi boş bir aktör ekleyin ve aşağıdakileri yapın:

    ![Boş aktörün akustik bileşen kullanımını gösteren Unreal ekran Düzenleyicisi](media/acoustics-component-usage.png)

1. Akustik ses bileşen Aktöre ekleyin. Bu bileşen Wwise ses bileşeni için proje akustik ile işlevselliği genişletir.
2. Başlangıç kutusunda Play ilişkili Wwise olay düzeyi başlangıçta tetikleyecek varsayılan olarak denetlenir.
3. Ekrandaki yazdırmak için akustik parametreleri göster onay kutusunu kullanın kaynak hakkında bilgi için hata ayıklama.
    ![Etkin hata ayıklama değerlerle ses kaynağında Unreal ekran Düzenleyicisi akustik paneli](media/debug-values.png)
4. Her zamanki Wwise iş akışı başına bir Wwise olay atayın
5. Ses uzamsal kullanma kapalı olduğundan emin olun. Belirli bir ses bileşeni için proje akustik kullanırsanız, şu anda aynı anda Wwise'nın kayma Ses altyapısı için akustik kullanamazsınız.

İşinizi tamamlandı. Görünüm hareket ve akustik etkileri keşfedin!

## <a name="next-steps"></a>Sonraki adımlar
* [Tasarım](unreal-workflow.md) Unreal/Wwise içinde proje akustik Öğreticisi
* [Bakes yapma hakkında bilgi](unreal-baking.md) , oyun sahneler için 
