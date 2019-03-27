---
title: Sürüm Notları - özel görüntü işleme hizmeti
titlesuffix: Azure Cognitive Services
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: ce48a240c67c6077027210ffb0ca85db93610782
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58472965"
---
# <a name="custom-vision-service-release-notes"></a>Özel görüntü işleme hizmeti sürüm notları

## <a name="march-26-2019"></a>26 Mart 2019
- Özel görüntü işleme hizmeti, Azure üzerinde genel kullanılabilirlik geçtiğini! 
- Yeni bir makine öğrenme, özellikle zorlu veri kümeleri ve hassas sınıflandırma üzerinde daha iyi performans için arka uç ile gelişmiş bir eğitim özelliği eklendi. Gelişmiş eğitimle, eğitim ve özel işleme için işlem saati bütçe experimentally en iyi eğitim ve genişletmeyi ayarları tanımlayacak belirtebilirsiniz. Hızlı yinelemeler için var olan Hızlı eğitim kullanmaya devam edebilirsiniz. 
- 3.0 sunulan API'ler. Önümüzdeki öncesi 3.0 API'leri 1 Ekim 2019 tarihinde duyurulana. Hızlı Başlangıçlar belgelerine bakın için [.Net](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/csharp-tutorial), [Python](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/python-tutorial), [düğüm](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/node-tutorial), [Java](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/java-tutorial), veya [Git](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/go-tutorial) örnekler nasıl kullanmaya başlamak. 
- "Yinelemeler yayımlama/yayımdan ile 3. 0'varsayılan" yerine API'leri. 
- Yeni model dışarı aktarma hedefler sürümüne eklenmiştir. Dockerfile dışarı aktarma, Raspberry Pi 3'için ARM destekleyecek şekilde yükseltildi. Dışarı aktarma desteği eklendi [işleme yapay ZEKA Geliştirme Seti.](https://visionaidevkit.com/) 
- S0 katmanı için 500 proje başına artan sınırı etiket. S0 katmanı için 100.000 projeye başına artan sınırı görüntüleri.
- Yetişkinlere yönelik etki alanı kaldırıldı. Genel etki alanı yapmanız olur.
- Duyurulan [fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/custom-vision-service/) genel kullanılabilirlik.  


## <a name="february-25-2019"></a>25 Şubat 2019
- Sınırlı deneme sürümünde projeleri (Projeler) bir Azure kaynağı ile ilişkili olmayan, sonuna duyurulan Azure genel önizlemesi için geçiş tamamlanmasından Custom Vision yaklaştığında. 25 Mart 2019'den itibaren CustomVision.ai site yalnızca ücretsiz özel görüntü işleme kaynak gibi bir Azure kaynağı ile ilişkili izleme projeleri destekler. 1 Ekim 2019, hala mevcut sınırlı deneme projelerinize özel görüntü işleme API'leri aracılığıyla erişmesini mümkün olacaktır. Size zaman bu, özel görüntü ile yazdığınız tüm uygulamalar için API anahtarları güncelleştirme olanağı sunar. 1 Ekim 2019'dan sonra Azure'a taşıdık henüz tüm sınırlı deneme projelerin silinir.

## <a name="january-22-2019"></a>22 Ocak 2019
- Yeni Azure bölgeleri için eklenen destek: Batı ABD 2, Doğu ABD, Doğu ABD 2, Batı Avrupa, Kuzey Avrupa, Güneydoğu Asya, Avustralya Doğu, Orta Hindistan, UK Güney, Japonya Doğu ve Kuzey Orta ABD. Destek, Güney Orta ABD için devam eder. 

## <a name="december-12-2018"></a>12 Aralık 2018'e
- Dışarı aktarma nesne algılama modelleri (sunulan nesne algılama Compact etki alanı) için destek.
- Birkaç geliştirilmiş ekran okuyucu ve klavye gezintisi desteği için erişilebilirlik sorun düzeltildi. 
- Resim Görüntüleyici ve daha hızlı etiketleme deneyimi etiketleme geliştirilmiş nesne algılama işlemleri için UX güncelleştirmeler.  
- Daha iyi kalite nesne algılama için nesne algılama etki alanı için güncelleştirilmiş temel modeli. 
- Hata düzeltmeleri.

## <a name="november-6-2018"></a>6 Kasım 2018
- Nesne algılama logosu etki alanı için destek eklendi.

## <a name="october-9-2018"></a>9 Ekim 2018
- Nesne algılama Ücretli Önizleme girer. Bu gibi durumlarda, nesne algılama projeleri artık bir Azure kaynağı ile oluşturabilirsiniz.
- Bir Azure'a bağlamak için sınırlı deneme sürümünde projeyi yükseltmesine daha kolay hale getirmek için Web sitesine, "Azure Taşı" özelliği eklendi. Kaynak bağlı projesi (F0 veya S0.) Bu ürün için Ayarlar sayfasında bulabilirsiniz.  
- Windows ML Windows 2018 Ekim güncelleştirme sürümünü destekleyecek şekilde ONNX 1.2 aktarma eklendi.
Hata düzeltmeleri için ONNX dahil olmak üzere, özel karakterler ile dışarı aktarın. 

## <a name="august-14-2018"></a>14 Ağustos 2018
- Eklenen "Başlarken" pencere öğesi customvision.ai sitesine proje eğitimi aracılığıyla kullanıcı kılavuzu. 
- Daha fazla multilabel projeleri (yeni kaybı katman) yararlanmak için makine öğrenimi işlem hattı iyileştirmeleri.

## <a name="june-28-2018"></a>28 Haziran 2018
- Hata düzeltmeleri ve arka uç geliştirmeleri.
- Görüntüleri tam olarak bir etiketi bulunduğu projeler için çok sınıflı sınıflandırma etkin. Çok sınıflı modu için Öngörüler içinde olasılıklar birine nüfuslarını toplayacağız (tüm görüntüleri belirtilen etiketlerinizi arasında sınıflandırılan).

## <a name="june-13-2018"></a>13 Haziran 2018
- UX yenileme, kullanım kolaylığı ve erişilebilirlik üzerinde odaklanır. 
- Çok sayıda etiket multilabel projeleriyle yararlanmak için makine öğrenimi işlem hattı iyileştirmeleri.
- TensorFlow dışarı aktarma hata düzeltildi. Dışarı aktarılan modeli sürüm oluşturma, yinelemeler birden çok kez dışarı aktarılabilir için etkin. 

## <a name="may-7-2018"></a>7 Mayıs 2018
- Sınırlı Deneme Sürümü projeleri için önizleme sürümündeki Nesne Algılama özelliği eklendi.
- 2.0 API'lere yükseltme yapıldı
- S0 katmanı 250 etiket ve 50.000 görüntüye kadar genişletildi. 
- Görüntü sınıflandırma projeleri için makine öğrenmesi işlem hattında önemli arka uç geliştirmeleri yapıldı. 27 Nisan 2018 sonrasında eğitilen projeler bu güncelleştirmeden faydalanabilecek.
- Windows ML ile kullanılmak üzere modelleri ONNX biçiminde dışarı aktarma özelliği eklendi.
- Dockerfile biçiminde model dışarı aktarma özelliği eklendi. Bu özellik yapıtları indirerek DockerFile, TensorFlow modeli ve hizmet kodu dahil olmak üzere kendi Windows veya Linux kapsayıcılarınızı oluşturmanızı sağlar. 
- Genel (CD) ve önemli yer (CD) etki alanları, TensorFlow dışarı modelleri için yeni eğitim [ortalama değerler artık (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample), tüm projeler genelinde tutarlılık sağlamak için. 

## <a name="march-1-2018"></a>1 Mart 2018
- Ücretli önizleme dönemine geçildi ve Azure portala eklendi. Projeler artık F0 (Ücretsiz) veya S0 (Standard) katmandaki Azure kaynaklarına eklenebilir. 100 etikete ve 25.000 görüntüye izin veren S0 katmanı projeleri eklendi. 
- Makine öğrenmesi işlem hattı/normalleştirme parametresi için arka uç değişiklikleri. Bu sayede müşteriler Olasılık Eşiğini ayarlarken hassasiyet-geri çağırma durumlarını daha iyi denetleyebilecek. Bu değişikliklerle bağlantılı olarak CustomVision.ai portalındaki Olasılık Eşiği %50 olarak ayarlandı.

## <a name="december-19-2017"></a>19 Aralık 2017

- Önceden yayımlanmış iOS'a aktarma (CoreML) özelliğine dahil olarak Android'e aktarma (TensorFlow) seçeneği eklendi. Bu sayede eğitilen sıkıştırılmış model dışarı aktarılarak bir uygulamada çevrimdışı olarak çalıştırılabilir.
- Perakende ve Yer İşareti "sıkıştırılmış" etki alanları eklendi ve bu etki alanları için model dışarı aktarma özellikleri etkinleştirildi.
- [1.2 Eğitim API'si](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) ile [1.1 Tahmin API'si](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164) yayımlandı. API'ler model dışarı aktarma seçeneği, görüntüleri "Tahminlere" kaydetmeyen yeni Tahmin işlemi ile güncelleştirildi ve Eğitim API'sine toplu işlem eklendi.
- Yineleme eğitimi için kullanılan etki alanını görme dahil olmak üzere kullanıcı deneyimi ayarlamaları yapıldı.
- [C# SDK'sı ve örneği](https://github.com/Microsoft/Cognitive-CustomVision-Windows) güncelleştirildi.

