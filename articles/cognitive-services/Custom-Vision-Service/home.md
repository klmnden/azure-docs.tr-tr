---
title: Özel Görüntü İşleme Hizmeti makine öğrenmesine genel bakış - Azure Bilişsel Hizmetler | Microsoft Docs
description: Özel Görüntü İşleme Hizmeti, Azure platformunda özel görüntü sınıflandırıcılar oluşturmanızı sağlayan bir Microsoft Bilişsel Hizmet bileşenidir.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: overview
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: de45fc399470a806fb7456cbbe936cecf659ee7c
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37087804"
---
# <a name="what-is-the-custom-vision-service"></a>Özel Görüntü İşleme Hizmeti nedir?

Özel Görüntü İşleme Hizmeti, özel görüntü sınıflandırıcılar oluşturmanızı sağlayan bir Microsoft Bilişsel Hizmet bileşenidir. Görüntü sınıflandırıcı oluşturma, dağıtma ve geliştirme işlemlerini oldukça kolay ve hızlı hale getirir. Özel Görüntü İşleme Hizmeti, görüntülerinizi yüklemek ve sınıflandırıcıyı eğitmek için kullanabileceğiniz bir REST API'si ve web arabirimi sunar.

## <a name="what-does-custom-vision-service-do-well"></a>Özel Görüntü İşleme Hizmeti hangi konularda iyidir?

Özel Görüntü İşleme Hizmeti, en iyi performansı sınıflandırmaya çalıştığınız öğe görüntüde baskın olduğunda sunar. 

Sınıflandırıcı veya algılayıcı oluşturmak için birkaç görüntü kullanılması gerekir. Prototipinizi başlatmak için sınıf başına 50 görüntü yeterli olacaktır. Özel Görüntü İşleme Hizmetinin kullandığı metotlar, değişikliklerden fazla etkilenmez ve bu sayede az miktarda veriyle prototip çalışmalarına başlamanızı sağlar. Bu da Özel Görüntü İşleme Hizmetinin küçük farkları algılamak istediğiniz senaryolar için pek uygun olmadığı anlamına gelir. Buna örnek olarak kalite denetimi senaryolarındaki küçük çatlaklar veya göçükler verilebilir.

## <a name="release-notes"></a>Sürüm Notları

### <a name="may-7-2018"></a>7 Mayıs 2018
- Sınırlı Deneme Sürümü projeleri için önizleme sürümündeki Nesne Algılama özelliği eklendi.
- 2.0 API'lere yükseltme yapıldı
- S0 katmanı 250 etiket ve 50.000 görüntüye kadar genişletildi. 
- Görüntü sınıflandırma projeleri için makine öğrenmesi işlem hattında önemli arka uç geliştirmeleri yapıldı. 27 Nisan 2018 sonrasında eğitilen projeler bu güncelleştirmeden faydalanabilecek.
- Windows ML ile kullanılmak üzere modelleri ONNX biçiminde dışarı aktarma özelliği eklendi.
- Dockerfile biçiminde model dışarı aktarma özelliği eklendi. Bu özellik yapıtları indirerek DockerFile, TensorFlow modeli ve hizmet kodu dahil olmak üzere kendi Windows veya Linux kapsayıcılarınızı oluşturmanızı sağlar. 
- Yeni eğitilen ve Genel (Sıkıştırılmış) ve Yer İşareti (Sıkıştırılmış) alanlarda TensorFlow'a aktarılan modeller için tüm projelerde tutarlılığı sağlama amacıyla [Ortalama Değerler artık (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample). 

### <a name="march-1-2018"></a>1 Mart 2018
- Ücretli önizleme dönemine geçildi ve Azure portala eklendi. Projeler artık F0 (Ücretsiz) veya S0 (Standard) katmandaki Azure kaynaklarına eklenebilir. 100 etikete ve 25.000 görüntüye izin veren S0 katmanı projeleri eklendi. 
- Makine öğrenmesi işlem hattı/normalleştirme parametresi için arka uç değişiklikleri. Bu sayede müşteriler Olasılık Eşiğini ayarlarken hassasiyet-geri çağırma durumlarını daha iyi denetleyebilecek. Bu değişikliklerle bağlantılı olarak CustomVision.ai portalındaki Olasılık Eşiği %50 olarak ayarlandı.

### <a name="december-19-2017"></a>19 Aralık 2017

- Önceden yayımlanmış iOS'a aktarma (CoreML) özelliğine dahil olarak Android'e aktarma (TensorFlow) seçeneği eklendi. Bu sayede eğitilen sıkıştırılmış model dışarı aktarılarak bir uygulamada çevrimdışı olarak çalıştırılabilir.
- Perakende ve Yer İşareti "sıkıştırılmış" etki alanları eklendi ve bu etki alanları için model dışarı aktarma özellikleri etkinleştirildi.
- [1.2 Eğitim API'si](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) ile [1.1 Tahmin API'si](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164) yayımlandı. API'ler model dışarı aktarma seçeneği, görüntüleri "Tahminlere" kaydetmeyen yeni Tahmin işlemi ile güncelleştirildi ve Eğitim API'sine toplu işlem eklendi.
- Yineleme eğitimi için kullanılan etki alanını görme dahil olmak üzere kullanıcı deneyimi ayarlamaları yapıldı.
- [C# SDK'sı ve örneği](https://github.com/Microsoft/Cognitive-CustomVision-Windows) güncelleştirildi.

## <a name="next-steps"></a>Sonraki adımlar

[Sınıflandırıcı oluşturmayı öğrenin](getting-started-build-a-classifier.md)
