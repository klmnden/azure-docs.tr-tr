---
title: -Terimler sözlüğü özel görüntü işleme hizmeti
titlesuffix: Azure Cognitive Services
description: Özel görüntü işleme hizmeti için terimler sözlüğü.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: anroth
ms.openlocfilehash: 3a34af77a2806ceb56e939e2b153f2e68bba61cd
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59048945"
---
# <a name="glossary-of-terms-for-custom-vision-service"></a>Özel görüntü işleme hizmeti için terimler sözlüğü

Özel görüntü işleme hizmeti yaygın olarak kullanılan bazı terimleri şunlardır:

## <a name="classifier"></a>Sınıflandırıcısı

Sınıflandırıcı birkaç eğitim resmi kullanarak özel görüntü işleme hizmeti, oluşturduğunuz bir modeldir. Yeni bir sınıflandırıcı eğitimi tamamladıktan sonra uygulamanıza eklediğiniz bir değerlendirme uç noktası (HTTPS) alırsınız. Kendi oluşturduğunuz her bir sınıflandırıcı projedir ve açtıktan sonra tüm projeleri görüntüleyebilir.

## <a name="domain"></a>Domain

Bir proje oluşturduğunuzda, bu proje için "etki alanı" seçin. Etki alanı bir sınıflandırıcı görüntülerinizi nesnesinde belirli bir tür için en iyi duruma getirir. Örneğin, senaryonuz apple pasta havuçlu kek görüntülerini ve görüntülerini arasında sınıflandırmak için ise, ardından "Yemek" etki alanını seçin. Ardından seçmek için hangi etki alanı emin değilseniz, "Genel" etki alanını seçin.

- **Gıda etki alanı.** Bir restoran menüsünde görür çanakları için en iyi duruma getirilmiş. Bunu tek tek Meyve veya et tanınması için iyi hale getirilmiş değil. Genel etki alanı, tek tek MEYVELERİ veya et fotoğraflarını sınıflandırma istiyorsanız, bu amaç için kullanın.
- **Yer işareti etki alanı.** Tanınabilir için yer işareti, doğal ve yapay en iyi duruma getirilmiş. Yer işareti biraz, önündeki taşıyor kişi grubu tarafından engellendiği bile yer işareti fotoğraf açıkça görünür olduğunda, bu etki alanı en iyi şekilde çalışır.
- **Perakende etki alanı.** Sınıflandırma görüntüleri bir alışveriş katalog veya alışveriş Web sitesi için en iyi duruma getirilmiş. Ardından, yüksek duyarlık sınıflandırmak elbiselerini, pants, gömlekler vb. istiyorsanız, perakende etki alanını kullanın.
- **Genel etki alanı.** Çok çeşitli görüntü sınıflandırma görevleri için de uygundur.

Tarafından üretilen modellerini **compact etki alanları** ile yineleme dışarı aktarma işlevi dışarı aktarılabilir. Mobil cihazlarda gerçek zamanlı sınıflandırma kısıtlamaları için iyileştirilmiştir. Sıkıştırılmış bir etki alanı ile oluşturulan sınıflandırıcılar biraz daha az doğru eğitim verilerini aynı miktarda sahip standart bir etki alanı. Artırabilen bunlar gerçek zamanlı yerel olarak çalıştırılacak küçük olmasıdır. 

## <a name="evaluation"></a>Değerlendirme

Sınıflandırıcınızı eğitim almış sonra herhangi bir görüntü değerlendirme için otomatik olarak oluşturulan bir https uç noktasını kullanarak gönderebilirsiniz. Sınıflandırıcınızı güvenle sırasına göre tahmin edilen etiketler, bir dizi döndürür.

## <a name="iteration"></a>Yineleme

Her Train zaman veya sınıflandırıcınızı yeniden eğitme, modelinizin yeni bir yineleme oluşturun. İlerleme durumunuzu zamanla karşılaştırmak olanak tanımak için birçok geçmiş yinelemeler saklarız. Artık yararlı bulduğunuz herhangi bir yinelemesini silebilirsiniz. Yineleme silinmesi kalıcı bir işlemdir ve tüm görüntüler veya bu yineleme için benzersiz değişiklikler de silmeniz gerektiğini unutmayın. 

## <a name="precision"></a>Duyarlık

Ne zaman, nasıl büyük olasılıkla bir görüntü sınıflandırma sınıflandırıcınızı görüntünün doğru şekilde sınıflandırmak için mi? Sınıflandırıcı (köpek ve ponies) eğitmek için kullanılan tüm görüntüleri dışında ne kadarlık bir model doğru mu? 100 görüntüleri 99 doğru etiketler % 99 duyarlığını sağlar.

## <a name="predictions"></a>Tahminler

Sınıflandırıcınızı sınıflandırmak için yeni görüntüleri kabul gibi sizin için görüntülerini depolar. Bu görüntüler, doğru yanlış tahmin edilen görüntüleri etiketleyerek sınıflandırıcınızı duyarlılığını artırmak için kullanabilirsiniz. Ardından bu yeni görüntülere sınıflandırıcınızı yeniden eğitmek için de kullanabilirsiniz.

## <a name="recall"></a>Geri çekme

Doğru sınıflandırılmış tüm görüntüleri dışında kaç sınıflandırıcınızı doğru tanımlamak? Geri çağırma işlemi % 100 anlamına gelir, bu görüntüleri sınıflandırıcı eğitmek için kullanılan 38 köpek görüntüleri varsa, 38 köpekler sınıflandırıcı tarafından bulunamadı.

## <a name="settings"></a>Ayarlar

Ayarları, proje düzeyi ayarları ve kullanıcı düzeyi ayarları iki türü vardır.

- Proje düzeyi ayarları:
  
  Proje düzeyi ayarları bir proje veya sınıflandırıcı geçerlidir. Bunlara aşağıdakiler dahildir:

   - Proje etki alanı
   - Proje Adı
   - Proje açıklaması
   - Kullanım:
      - Eğitim resimlerinin sayısı
      - Oluşturulan etiket sayısı
      - Oluşturulan yineleme sayısı

- Kullanıcı düzeyi ayarları: 
   - Abonelik anahtarları: biri için bir değerlendirme/tahmin için eğitim.
   - Kullanım:
      - Oluşturulan proje sayısı
      - Yapılan değerlendirme/tahmin API çağrılarının sayısı.

## <a name="tags"></a>Etiketler

Eğitim görüntülerinizi nesneleri etiketlemek için etiketler kullanın. Köpek ve ponies tanımlamak için bir sınıflandırıcı oluşturuyorsanız görüntülerindeki köpekler, "pony" etiket "köpek"'hem ve ponies, içeren görüntülerindeki ve hem bir köpek hem de bir pony içeren görüntülerindeki "pony" etiket içeren bir "köpek" etiketi koyabilirsiniz.

## <a name="training-image"></a>Eğitim resmi

Yüksek duyarlılık sınıflandırıcı oluşturmak için çeşitli eğitim resmi Custom Vision Service'e gerekir. Eğitim sınıflandırmak için Custom Vision Service'e istediğiniz görüntüyü hello'nun görüntüsüdür. Örneğin, portakallar sınıflandırmak için birkaç görüntülerini portakallar hizmetinin portakallar tanıyabileceğiniz bir sınıflandırıcı oluşturmasına izin vermek için özel görüntü işleme hizmeti için karşıya yükleme gerekecektir. Etiket başına en az 30 görüntüleri öneririz.

## <a name="workspace"></a>Çalışma alanı

Çalışma alanınızdaki tüm eğitim görüntüleri içerir ve kaldırıldı veya görüntüleri eklenen gibi en son yineleme tüm değişiklikleri yansıtır. Sınıflandırıcınızı eğittiğinizde, çalışma alanınızda mevcut görüntüleri kullanarak, sınıflandırıcı yeni bir yineleme oluşturun.