---
title: Terimler sözlüğü özel Vision hizmeti - Azure Bilişsel hizmetler | Microsoft Docs
description: Özel görme hizmeti için terimler sözlüğü.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/08/2017
ms.author: anroth
ms.openlocfilehash: 871617ce3c1c5a84df746c0c7d87c113b3a6f354
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352888"
---
# <a name="glossary-of-terms-for-custom-vision-service"></a>Özel görme hizmeti için terimler sözlüğü

Özel görme hizmeti ve anlamları kullanılan bazı şartları şunlardır:

## <a name="classifier"></a>Sınıflandırıcı

Bir sınıflandırıcı birkaç eğitim görüntüleri kullanarak özel görme hizmeti, yapı bir modeldir. Yeni bir sınıflandırıcı eğitim tamamladıktan sonra uygulamanıza ekleme değerlendirme uç (HTTPS) alırsınız. Oluşturduğunuz her bir sınıflandırıcı kendi projede olduğunu ve oturum açtıktan sonra tüm projeleri görüntüleyebilir.

## <a name="domain"></a>Etki alanı

Bir proje oluşturduğunuzda, bu proje için "etki alanı" seçin. Etki alanı bir sınıflandırıcı görüntülerinizi nesnesinde belirli bir tür için en iyi duruma getirir. Örneğin, senaryonuza apple pasta havuçlu kek görüntülerini karşı görüntülerin arasında sınıflandırmak için ise, ardından "Yemek" etki alanını seçin. Seçmek için hangi etki alanını emin değilseniz, "Genel" etki alanı seçin.

- **Yemek etki alanı.** Bir restoran menüsünde görür tabaklar için en iyi duruma getirilmiş. Bu tek tek ulaşılacak durumlardır veya et tanımayı için optimize değil. Tek tek fruits veya et fotoğraflarını sınıflandırmak istiyorsanız, bu amaçla genel etki alanı kullanın.
- **Yer işareti etki alanı.** Tanınabilir işaretleri için doğal ve yapay en iyi duruma getirilmiş. Yer işareti biraz bir grup kişi, önündeki taşıyor tarafından engellendiği olsa bile yer işareti fotoğraf içinde açıkça görünür olduğunda bu etki alanında en iyi şekilde çalışır.
- **Perakende etki alanı.** Sınıflandırma görüntüleri bir alışveriş katalog veya alışveriş Web sitesi için en iyi duruma getirilmiş. Sınıflandırma zaman elbiselerini, pantalonların, gömlekler, vb. Yüksek duyarlılık istiyorsanız, perakende etki alanını kullanın.
- **Yetişkin etki alanı.** Yetişkin olmayan içerik yetişkinlere yönelik içeriğe arasında daha iyi tanımlamak için en iyi duruma getirilmiş. Örneğin, görüntüleri bathing Setleri kişilerin engellemek istiyorsanız, bu etki alanı, bunu yapmak için özel bir sınıflandırıcı oluşturmanıza olanak sağlar.
- **Genel etki alanı.** Çok çeşitli görüntü sınıflandırma görevleri için de uygundur.

Tarafından oluşturulan modelleri **compact etki alanları** yineleme verme işlevsellikle aktarılabilir. Bunlar, mobil cihazlarda gerçek zamanlı sınıflandırma kısıtlamalar için getirilmiştir. Sıkıştırılmış bir etki alanı ile oluşturulmuş sınıflandırıcı biraz daha az doğru eğitim verileri aynı miktarda sahip standart bir etki alanı. Kolaylığını gerçek zamanlı olarak yerel olarak çalıştırılacak küçük ' dir. 

## <a name="training-image"></a>Eğitim resmi

Yüksek duyarlılık sınıflandırıcı oluşturmak için çeşitli eğitim görüntüleri özel görme hizmeti gerekiyor. Özel görme hizmeti tarafından sınıflandırmak istediğiniz resim fotoğrafı eğitim görüntüdür. Örneğin, portakallar sınıflandırmak için birkaç görüntülerini portakallar portakallar tanıyabilmesi için bir sınıflandırıcı Oluşturma Hizmeti'nin izin vermek için özel görme hizmetine yüklemek gerekir. Etiket başına en az 30 görüntüleri öneririz.

## <a name="iteration"></a>Yineleme

Her tren saat veya, sınıflandırıcı yeniden eğitme, yeni bir yineleme modelinizin oluşturun. Biz, zaman içinde ilerleme durumunuzu karşılaştırmanızı sağlar için birkaç geçmişteki yineleme tutun. Artık yararlı bulduğunuz herhangi bir yineleme silebilirsiniz. Yineleme Silme kalıcıdır ve herhangi bir görüntü veya bu yinelemeye benzersiz değişiklikleri de Sil unutmayın. 

## <a name="workspace"></a>Çalışma alanı

Tüm eğitim görüntüleri çalışma alanınızı içeren ve kaldırılmış veya görüntüleri eklenen gibi en son yineleme tüm değişiklikleri yansıtır. Sınıflandırıcı eğitmek, çalışma alanınızda mevcut görüntüleri kullanarak, sınıflandırıcı yeni bir yinelemesini oluşturun.

## <a name="tags"></a>Etiketler

Eğitim görüntülerinizi nesneleri etiketlemek için etiketler kullanın. Köpekler ve ponies tanımlamak için bir sınıflandırıcı oluşturuyorsanız, köpekler, "köpek"'hem ve ponies, içeren resimler "midilli" etiketinde hem bir köpek ve bir midilli içeren resimler "midilli" etiketinde içeren görüntülerinde "köpek" etiketi koyabilirsiniz.

## <a name="evaluation"></a>Değerlendirme

Sınıflandırıcı eğitilmiş sonra değerlendirme için herhangi bir görüntü otomatik olarak oluşturulan https uç noktasını kullanarak gönderebilirsiniz. Sınıflandırıcı güvenirlik sırayla tahmin edilen etiketler, bir dizi döndürür.

## <a name="predictions"></a>Tahminler

Sınıflandırmak için yeni görüntüleri, sınıflandırıcı kabul gibi sizin için görüntüleri depolar. Bu görüntüler, doğru yanlış tahmin edilen görüntüleri etiketleme tarafından sınıflandırıcı kesinliğini artırmak için kullanabilirsiniz. Ardından bu yeni görüntüler, sınıflandırıcı yeniden eğitmek için de kullanabilirsiniz.

## <a name="precision"></a>Duyarlılık

Ne zaman nasıl büyük olasılıkla bir görüntü sınıflandırmak görüntünün doğru sınıflandırmak için sınıflandırıcı mi? (Köpekler ve ponies) sınıflandırıcı eğitmek için kullanılan tüm görüntüleri dışında yüzde model doğru mı aldınız? 100 görüntüleri dışında 99 doğru etiketleri % 99 kesinliğini sağlar.

## <a name="recall"></a>Geri çekme

Doğru sınıflandırılmış tüm görüntüleri dışında kaç, sınıflandırıcı doğru tanımlamak? Geri çağırma işlemi % 100 anlamına gelir, bu sınıflandırıcı eğitmek için kullanılan görüntüleri 38 köpek görüntüleri olsaydı, 38 köpekler tarafından sınıflandırıcı bulunamadı.

## <a name="settings"></a>Ayarlar

İki tür ayarları, proje düzeyi ayarlarla ve kullanıcı düzeyinde ayarları vardır.

- Proje düzeyi ayarları: 
  
  Proje düzeyi ayarlarla, bir proje veya sınıflandırıcı için geçerlidir. Bunlar:

   - Proje etki alanı
   - Proje Adı
   - Proje açıklaması
   - Kullanım:
      - Eğitim resimlerinin sayısı
      - Oluşturulan etiket sayısı
      - Oluşturulan yineleme sayısı

- Kullanıcı düzeyinde ayarları: 
   - Abonelik anahtarları: biri için bir değerlendirme/tahmin için eğitim.
   - Kullanım:
      - Oluşturulan projeleri sayısı
      - Yapılan değerlendirme/tahmin API çağrılarının sayısı.
