---
title: Konuşma öğrenen nedir? -Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Konuşma öğrenen ve nasıl çalıştığı hakkında bilgi edinin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 9cbf0e60382ef17d68aab47cf5f24ea9b8434f13
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353992"
---
# <a name="what-is-conversation-learner"></a>Konuşma öğrenen nedir?

Konuşma öğrenen oluşturmanıza ve örnek etkileşimlerini öğrenin konuşma arabirimleri öğretmek olanak sağlar. 

Geleneksel yaklaşım, konuşma öğrenen yanıtları geliştirmek ve daha ilgi çekici kullanıcı deneyimleri için bir iletişim kutusu uçtan uca bağlamında göz önünde bulundurur. Göreve dayalı çok sayıda kapsayıcı kullanım örnekleri, konuşma öğrenen makine öğrenme arka planda bot ve kullanıcıları rahatsız, ek müşteri hizmeti ücrete neden olasılığı daha akıllı aracıları yapma ve daha sezgisel ile etkileşim kurmak için geçerlidir.

Başlamak için geliştirici taklit istediği Prototipik iletişim kutuları girer. Daha fazla iletişim kutuları girerken, model sürekli olarak güncelleştirilir ve geliştirici ne kadar iyi modeli genelleme görebilirsiniz. Model iyi çalışan sonra bot son kullanıcılara dağıtılabilir. Konuşma öğrenen görüşmeleri kullanıcılarla kaydeder ve geliştirici gözden geçirebilirsiniz. Hataları anlaþýlmaz, geliştirici üzerinde--nokta düzeltme yapabilirsiniz ve hemen retrained ve kullanılabilir modelidir.

Bu yaklaşım, el ile iletişim kutusu denetim mantığını kodlama azaltır ve işletme sahipleri veya etki alanı uzmanlar bir konuşma arabirimi olmadan önceki makineyi bilgi öğrenme katkıda bulunmak etkinleştirir. Bot, akıllı aygıt ya da akıllı Aracısı bir parçası olarak dağıtılan olup olmadığını konuşma öğrenen hızlı bir şekilde yeni yetenekler, davranışları veya yetkinlikleri yinelemek ve hızlı bir şekilde kendi kalitesini artırmak. 

Konuşma öğrenen geliştiriciler Microsoft Bot Framework ya da kendi altyapısını kullanarak tek başına birden çok konuşma kanallardan hızı Pazar ve sürücü başarılı bitirmenize arttırırsınız güçlendirir.

Özet ve vurgular:

- Konuşma öğrenen görev odaklı aracılarını oluşturmanın bir AI ilk yoludur.

- Bir uçtan uca yinelenen sinir ağı üzerinde (LSTM) kullanır ve sohbetleri doğrudan çok Aç örnekleri öğrenir. 

- Tasarımcılar, geliştiriciler, işletme kullanıcılarının ve Arama Merkezi çalışanları oluşturmasına ve aracılarını korumasına olanak sağlar. 

- İş kuralları ve sağduyunuzu kodda express olanağı sağlar.

- Oturumları eğitme sırasında sinir ağı modelinin beklenen Eylemler sonraki kümesini konuşmada Puanlama amacıyla kullanılır. Bot Geliştirici sonra doğru bir eylem seçin ve düzgün yanıt sağlamak için ağ eğitmek.
 
- Eğitim tamamlandıktan sonra Geliştirici bot yanıtları düzeltmeler yapmak ve model yeniden eğitme kullanıcı etkileşimleri günlük iletişim kutularını kullanabilirsiniz. 

- Görevleri tamamlamak için etki alanına özgü ve üçüncü taraf API'leri çağırabilirsiniz.

