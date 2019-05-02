---
title: Deneysel bir hazır içerik algılayan kodlama için - Azure | Microsoft Docs
description: Bu makalede içeriğe duyarlı Azure Media Services encoding ele alınmaktadır.
services: media-services
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/05/2019
ms.author: sethm
ms.custom: ''
ms.openlocfilehash: b0b5a74a6ca0085f945075a8896c05a724ff062c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64717952"
---
# <a name="experimental-preset-for-content-aware-encoding"></a>Deneysel içeriğe duyarlı kodlama Önayarı

İçerik teslimi için hazırlamak amacıyla [bit hızı Uyarlamalı akış](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), video Çoklu bit hızlarında-(düşük, yüksek) kodlanması gerekiyor. Kalite, normal performansında sağlamak amacıyla hızı azaltıldığı kadar görüntü çözünürlüğünü aynıdır. Bu, bir sözde kodlama Merdiveni içinde – çözünürlüklerine ve bit hızlarına dönüştürme tablosu sonuçlanır; Media Services bkz [yerleşik kodlama Önayarları](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset).

## <a name="overview"></a>Genel Bakış

Bir tek-hazır-uygun-all-videoları yaklaşım taşımada ilgi artırılmış Netflix yayımladıktan sonra kendi [blog](https://medium.com/netflix-techblog/per-title-encode-optimization-7e99442b62a2) aralık 2015'te. O zamandan bu yana, içeriğe duyarlı kodlaması için birden çok çözümü Market'te yayımlanan; bkz: [bu makalede](https://www.streamingmedia.com/Articles/Editorial/Featured-Articles/Buyers-Guide-to-Per-Title-Encoding-130676.aspx) genel bakış. Özelleştirme veya kodlama Merdiveni tek video karmaşıklığını için ayarlanacak içeriği dikkat edilmesi gereken olur. Her bir çözünürlükte, ötesinde herhangi bir artış kalite perceptive değil – Kodlayıcı bu en iyi hızı değerinde çalışır bir bit hızı yoktur. İleri iyileştirme düzeyini içeriğine göre çözümler seçmektir: Örneğin, bir PowerPoint sunusu videosunun 720 p aşağıda gitmesini fayda. Daha fazla devam, kodlayıcı video içinde her görüntüsü ayarlarını en iyi duruma getirme görevli. Netflix açıklanan [bu tür bir yaklaşım](https://medium.com/netflix-techblog/optimized-shot-based-encodes-now-streaming-4b9464204830) 2018'de.

Microsoft erken 2017'de yayınlanan [Uyarlamalı akış](autogen-bitrate-ladder.md) kalitesinde değişkenliği sorun ve çözümü için kaynak videoları ele almak için hazır. Müşterilerimizin içeriği, bir noktada 1080 p, diğerleri 720 p ve SD ve daha düşük çözünürlükler birkaç farklı bir karışımını içeriyordu. Ayrıca, tüm kaynak içerik, yüksek kaliteli mezzanines film veya TV stüdyosu arasına oluştu. Bit hızı Merdiveni hiçbir zaman çözüm ya da giriş mezzanine, ortalama hızı aştığını sağlayarak bu sorunları hazır adresleri akış Bağdaşık.

Deneysel içeriğe duyarlı kodlama Önayarı Bu mekanizma, en iyi hızı değerin belirli bir çözüm için ancak kapsamlı hesaplama analiz gerektirmeden arama Kodlayıcı sağlayan özel mantığı ekleyerek genişletir. Bu yeni hazır sahip daha düşük bit hızı Uyarlamalı akış ön ayarı daha, ancak daha yüksek kalitede bir çıktı üretir net sonucudur. Kaliteli ölçümler gibi kullanarak karşılaştırmayı gösteren aşağıdaki örnek grafiklerini görebilirsiniz [PSNR](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio) ve [VMAF](https://en.wikipedia.org/wiki/Video_Multimethod_Assessment_Fusion). Kaynak filmler yüksek karmaşıklık görüntüleri kısa klipleri birleştirerek oluşturuldu ve TV programları, kodlayıcı stres testi uygulamak hedeflenen. Tanımı gereği, gelen içerik için içerik – değişiklik bu hazır üretir sonuçları, ayrıca içerikler için olmayabilir, bit hızı önemli azalmaya veya kalite gelişme anlamına gelir.

![PSNR kullanarak oranı-bozulmanın (RD) eğrisi](media/cae-experimental/msrv1.png)

**Şekil 1: Yüksek karmaşıklık kaynağı PSNR ölçüm kullanarak oranı-bozulmanın (RD) eğrisi**

![VMAF kullanarak oranı-bozulmanın (RD) eğrisi](media/cae-experimental/msrv2.png)

**Şekil 2: Yüksek karmaşıklık kaynağı VMAF ölçüm kullanarak oranı-bozulmanın (RD) eğrisi**

Hazır şu anda yüksek karmaşıklık için ayarlanmış yüksek kaliteli kaynak videoları (filmler, TV programlarına). İş (örneğin, PowerPoint sunuları), düşük karmaşıklığa içerik için uyum yanı sıra düşük kaliteli videolar devam ediyor. Uyarlamalı akış hazır olarak bu hazır çözümler aynı kümesini de kullanır. Microsoft çözümleri içeriğine göre en az sayıda seçmek için yöntemleri üzerinde çalışıyor. Kaynak içerik, başka bir kategori için sonuçları Kodlayıcı giriş düşük kaliteli (birçok sıkıştırma yapıtları nedeniyle düşük hızı) olduğunu belirlemek mümkün olduğu gibidir. Deneysel ile önceden unutmayın, böylece çoğu istemci bekletilen olmadan akış yürütmeye yaratmayacağı hızı anda yalnızca bir çıkış katmanı – üretmek Kodlayıcı verdi.

![RD eğri PSNR kullanma](media/cae-experimental/msrv3.png)

**Şekil 3: RD eğri (1080 p) en düşük kaliteli giriş PSNR kullanma**

![RD eğri VMAF kullanma](media/cae-experimental/msrv4.png)

**Şekil 4: RD eğri (1080 p) en düşük kaliteli giriş VMAF kullanma**

## <a name="use-the-experimental-preset"></a>Deneysel hazır kullanın

Bu gibi önceden dönüştürmeler oluşturabilirsiniz. Bir öğretici kullanıyorsanız [bunun gibi](stream-files-tutorial-with-api.md), kod şu şekilde güncelleştirebilirsiniz:

```csharp
TransformOutput[] output = new TransformOutput[]
{
   new TransformOutput
   {
      // The preset for the Transform is set to one of Media Services built-in sample presets.
      // You can customize the encoding settings by changing this to use "StandardEncoderPreset" class.
      Preset = new BuiltInStandardEncoderPreset()
      {
         // This sample uses the new experimental preset for content-aware encoding
         PresetName = EncoderNamedPreset.ContentAwareEncodingExperimental
      }
   }
};
```

> [!NOTE]
> "Deneysel" ön eki, temel alınan algoritmalar hala artmaktadır göstermek için burada kullanılır. Değişiklikler üzerinde sağlam ve çok çeşitli giriş koşullar uyum sağlayan bir algoritma yakınsamaya amacı ile bit hızı ladders oluşturmak için kullanılan mantıksal zaman içinde olacaktır ve orada kullanabilirsiniz. Kodlama işleri yine de bu hazır olacak kullanarak faturalandırılan bağlı olarak çıkış dakikası olmalı ve çıktı varlığına DASH ve HLS gibi protokoller, bizim akış uç noktalarından teslim edilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Videolarınızı en iyi duruma getirme, bu yeni seçenek hakkında bilgi edindiniz, denemek için davet ediyoruz. Bu makalenin sonunda bağlantıları kullanarak görüşlerinizi bize gönderin veya bize daha doğrudan etkileşim kurun <amsved@microsoft.com>.
