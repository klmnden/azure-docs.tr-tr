---
title: Sürüm Notları - özel görüntü işleme hizmeti
titlesuffix: Azure Cognitive Services
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 08/28/2018
ms.author: anroth
ms.openlocfilehash: 8b4ba1601a7c5a9f523f7f8145a032861aac5d8c
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901337"
---
# <a name="custom-vision-service-release-notes"></a>Özel görüntü işleme hizmeti sürüm notları

## <a name="october-9-2018"></a>9 Ekim 2018
- Nesne algılama Ücretli Önizleme girer. Bu gibi durumlarda, nesne algılama projeleri artık bir Azure kaynağı ile oluşturabilirsiniz.
- Bir Azure'a bağlamak için sınırlı deneme sürümünde projeyi yükseltmesine daha kolay hale getirmek için Web sitesine, "Azure Taşı" özelliği eklendi. Kaynak bağlı projesi (F0 veya S0.) Bu ürün için Ayarlar sayfasında bulabilirsiniz.  
- Windows ML Windows 2018 Ekim güncelleştirme sürümünü destekleyecek şekilde ONNX 1.2 aktarma eklendi.
Hata düzeltmeleri için ONNX dahil olmak üzere, özel karakterler ile dışarı aktarın. 


## <a name="august-14-2018"></a>14 Ağustos 2018
- Eklenen "Başlarken" pencere öğesi customvision.ai sitesine proje eğitimi aracılığıyla kullanıcı kılavuzu. 
- Daha fazla multilabel projeleri (yeni kaybı katman) yararlanmak için makine öğrenimi işlem hattı iyileştirmeleri.

## <a name="june-28-2018"></a>28 Haziran 2018'e
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

