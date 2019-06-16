---
title: Depolama sınırlamaları önlemek ve gecikme süresi girdi ve çıktı dizinlerle deneyin
description: Bu makalede, deneme giriş dosyalarınızı kaydedileceği yeri ve depolama sınırlama hataları önlemek ve gecikme süresi deneme için çıkış dosyalarının yazılacağı öğrenin.
services: machine-learning
author: rastala
ms.author: roastala
manager: danielsc
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 05/28/2019
ms.openlocfilehash: 28d8c47db8ea9c8a51bc8e9deb0a689eb0b20177
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66392897"
---
# <a name="where-to-save-and-write-files-for-azure-machine-learning-experiments"></a>Kaydet ve yazmak için dosyaları Burada, Azure Machine Learning denemeleri

Bu makalede, giriş dosyaları kaydetmek istediğiniz konumu ve çıkış dosyaları depolama önlemek denemelerinizi hataları sınırlandırmak ve gecikme süresi deneme yazılacağı öğrenin.

Eğitim başlatılırken zaman çalıştığı bir [hedef işlem](how-to-set-up-training-targets.md), dış ortamlardan yalıtılır. Bu tasarım amacı yeniden üretilebilirliğini ve deneme taşınabilirliğinin sağlamaktır. Aynı betiği iki kez çalıştırırsanız, hedef, aynı veya başka bir işlem aynı sonuçları alırsınız. Bu tasarımla, durum bilgisi olmayan hesaplama kaynakları, her hiçbir benzeşim tamamladıktan sonra çalışmakta olan işlere sahip olarak işlem hedefleri davranabilirsiniz.

## <a name="where-to-save-input-files"></a>Giriş dosyalarının kaydedileceği yeri

İşlem Hedefi veya yerel makinenize bir deneme başlatabilir önce gerekli dosyaları için bağımlılık dosyaları ve veri dosyalarını kodunuzu çalıştırmak için gereken gibi işlem hedefleyen kullanılabilir emin olmanız gerekir.

Azure Machine Learning eğitim betikleriniz, hedef işlem bağlamına tüm script klasörüne kopyalayarak çalıştırır ve ardından anlık görüntüsünü alır. Deneme anlık görüntüleri için depolama sınırı 300 MB ve/veya 2000 dosyalarını ' dir.

Bu nedenle, öneririz:

* **Bir Azure Machine Learning'de dosyalarınızı depolamak [veri deposu](https://docs.microsoft.com/python/api/azureml-core/azureml.data?view=azure-ml-py).** Bu deneme gecikme sorunlarını engeller ve Azure Machine Learning hizmeti tarafından yönetilen kimlik doğrulaması ve bağlama anlamına gelir. bir uzak işlem hedefin veri erişimi avantajları vardır. Kaynak dizin bir veri deposu belirtmek için veri deposunda karşıya dosya yükleme hakkında daha fazla bilgi edinin [erişim verileri, veri depoları](how-to-access-data.md) makalesi.

* **Yalnızca birkaç veri dosyalarını gerekir ve bağımlılık komut dosyası ve bir veri deposu kullanamazsınız** dosyaları eğitim betiğinizi aynı klasör dizine yerleştirin. Bu klasör olarak belirtmeniz, `source_directory` eğitim betiğiniz içine doğrudan veya eğitim betiğinizi çağıran kodu.

<a name="limits"></a>

### <a name="storage-limits-of-experiment-snapshots"></a>Deneme anlık görüntülerini depolama sınırları

Azure Machine Learning, denemeler için kodunuzu çalıştırma yapılandırdığınızda önerdiğiniz dizinine dayanan bir deneme anlık görüntüsünü otomatik olarak yapar. Bu, 300 MB ve/veya 2000 dosyalarının toplam limiti vardır. Bu sınırı aşarsanız, aşağıdaki hatayı görürsünüz:

```Python
While attempting to take snapshot of .
Your total snapshot size exceeds the limit of 300.0 MB
```

Bu hatayı gidermek için bir veri deposu deneme dosyalarınızı depolayın. Bir veri deposu kullanamazsanız, aşağıdaki tabloda olası alternatif çözümler sunar.

Deneme&nbsp;açıklaması|Depolama sınırı çözümü
---|---
2000'den az dosyaları ve bir veri deposu kullanamazsınız| Anlık görüntü boyutu sınırı ile geçersiz kıl <br> `azureml._restclient.snapshots_client.SNAPSHOT_MAX_SIZE_BYTES = 'insert_desired_size'`<br> Bu işlem sayısına ve dosyaların boyutuna bağlı olarak birkaç dakika sürebilir.
Belirli bir komut dosyası dizini kullanmanız gerekir| Olun bir `.amlignore` dosyaları kaynak kodunun bir parçası değildir, deneme anlık görüntüden dışlanacak dosya. Dosya ekleme `.amlignore` dosya ve eğitim betiğinizi aynı dizine koyun. `.amlignore` Dosyasını kullanır aynı [söz dizimi ve desenleri](https://git-scm.com/docs/gitignore) olarak bir `.gitignore` dosya.
İşlem hattı|Her adım için farklı bir alt kullanın
Jupyter notebooks| Oluşturma bir `.amlignore` dosya veya not defterinizin yeni, boş bir alt taşıyın ve kodunuzu yeniden çalıştırın.

## <a name="where-to-write-files"></a>Dosyaları yazılacağı

Eğitim denemeleri yalıtımının nedeniyle çalıştırma sırasında gerçekleşen dosyalarda yapılan değişiklikler, ortamınızın dışında sürdürülmeyen. Komut dosyanızı işlem yerel dosyaları değiştirirse, çalıştırmak, sonraki denemenize için değişiklikler kalıcı değildir ve bunlar istemci makineye otomatik olarak yayılır değil. Bu nedenle, ilk deneme sırasında çalışma yapılan değişiklikler yok ve bu ikinci Etkilenme.

Değişiklikleri yazarken, dosyaları bir Azure Machine Learning veri deposuna yazma öneririz. Bkz: [erişim verileri, veri depoları](how-to-access-data.md).

Bir veri deposu gerektirmeyen, dosyalara yazma `./outputs` ve/veya `./logs` klasör.

>[!Important]
> İki klasör *çıkarır* ve *günlükleri*, Azure Machine Learning tarafından özel olarak değerlendirilmesi alırsınız. Dosyaları yazdığınızda eğitim sırasında`./outputs` ve`./logs` klasörleri, dosyaları otomatik olarak karşıya yükleme, çalıştırma geçmişi için böylece çalıştırmanız tamamlandıktan sonra onlara yönelik erişimi olur.

* **Durum iletileri veya Puanlama sonuçları gibi çıktı** dosyalara yazma `./outputs` çalıştırma geçmişinde yapıt olarak kalıcı şekilde klasörü. Çalıştırma geçmişi içeriği karşıya yüklendiğinde gecikme oluşabilir gibi sayısı ve bu klasöre yazılan dosyaların boyutu oluşturduğunu unutmayın. Dosyaları bir veri deposuna yazma gecikme süresi önemliyse önerilir.

* **Günlükler çalıştırma geçmişinde olarak yazılmış dosyayı kaydetmeye** dosyalara yazma `./logs` klasör. Bu yöntem Canlı güncelleştirmeleri uzak bir akış için uygun olacak şekilde günlükler gerçek zamanlı olarak karşıya yüklenir.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [verilerine erişme, veri depoları](how-to-access-data.md).

* Daha fazla bilgi edinin [eğitim hedefleri ayarlamak için nasıl](how-to-set-up-training-targets.md).
