---
title: Bilgi Bankası - Microsoft Bilişsel hizmetler geliştirme yaşam döngüsü | Microsoft Docs
titleSuffix: Azure
description: Bilgi Bankası geliştirme yaşam döngüsü
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: 9ecdd2c7823eed145621b214690eff7681065507
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "35356034"
---
# <a name="knowledge-base-lifecycle"></a>Bilgi Bankası yaşam döngüsü
QnA Maker en iyi modeli değişiklikleri, utterance örnekler, yayımlama ve veri toplamayı yinelemeli bir döngüyle uç nokta sorgularından öğrenir. 

![Döngüsü yazma](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>QnA Maker Bilgi Bankası oluşturma
QnA Maker Bilgi Bankası (KB) uç noktası KB içeriğine göre bir kullanıcı sorgu için en iyi eşleşme yanıt sağlar. Bilgi Bankası oluşturma sorular, yanıtlar ve ilişkili meta verileri bir içerik deposu ayarlamak için tek seferlik bir işlemdir. Bilgi Bankası SSS sayfaları, Ürün kılavuzlarının veya yapılandırılmış Q-A çiftleri gibi önceden varolan içerikte tarafından oluşturulabilir. Bilgi edinmek için nasıl [Bilgi Bankası oluşturun](../How-To/create-knowledge-base.md).

## <a name="testing-and-updating-the-knowledge-base"></a>Test etme ve Bilgi Bankası güncelleştiriliyor
Bilgi Bankası düzenleme şeklinde veya otomatik ayıklama aracılığıyla içerikle doldurulduktan sonra test etmek için hazırdır. Aracılığıyla sınama yapılabilir **Test** Masası ortak kullanıcı sorgularının girerek ve döndürülen yanıtlarının beklendiği gibi olduğunu doğrulamak ve yeterli güvenirlik puanı. Düşük güvenilirlik puanları düzeltmek için diğer sorular ekleyebilirsiniz. Bir sorgu "eşleşme KB bulunamadı" dönerse yeni yanıtlar de ekleyebilirsiniz varsayılan yanıt. Sonuçlardan memnun olana kadar testi güncelleştirme sıkı Bu döngü devam eder. Bilgi edinmek için nasıl [, Bilgi Bankası test](../How-To/test-knowledge-base.md).

Büyük KB için test generateAnswer API'leri otomatik olarak yapılabilir. 

## <a name="publish-the-knowledge-base"></a>Bilgi Bankası yayımlama
İşiniz bittiğinde Bilgi Bankası test, onu yayımlayabilirsiniz. En son sürümünü test edilmiş Bilgi Bankası'na adanmış bir Azure Search dizini temsil eden iter yayımlama **yayımlanan** Bilgi Bankası. Ayrıca çağrılabilir bir uç nokta uygulaması ya da sohbet bot oluşturur.

Bu şekilde, bir üretim uygulamasında dinamik olabilecek yayımlanmış bir sürüm Bilgi Bankası test sürümüne yapılan değişiklikleri etkilemez.

Bu Bilgi Bankası herbiri ayrı ayrı test etmek için hedeflenebilir. API'lerini kullanarak, Bilgi Bankası'ile deneme sürümü hedefleyebilirsiniz `isTest=true` generateAnswer çağrısında bayrağı.

Bilgi edinmek için nasıl [, Bilgi Bankası yayımlama](../How-To/publish-knowledge-base.md).

## <a name="monitor-usage"></a>Kullanımı izleme
Hizmetinizin sohbet günlükleri oturum açabilmesi için Application Insights etkinleştirmek gerekir, [QnA Maker hizmetinizi oluşturmak](../How-To/set-up-qnamaker-service-azure.md).

Çeşitli analytics hizmeti kullanım elde edebilirsiniz. Application ınsights almak için nasıl kullanılacağı hakkında daha fazla bilgi [analytics QnA Maker hizmetiniz için](../How-To/get-analytics-knowledge-base.md).

Bağlı olarak, analytics'ten öğrenin, uygun hale [, Bilgi Bankası güncelleştirmeleri](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GÜVENİRLİK puanı](./confidence-score.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Bilgi Bankası](./knowledge-base.md)
[QnA Maker genel bakış](../Overview/overview.md)
