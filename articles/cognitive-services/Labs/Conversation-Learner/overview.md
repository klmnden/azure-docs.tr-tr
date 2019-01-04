---
title: Konuşma Öğrenici nedir? -Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici ve nasıl çalıştığı hakkında bilgi edinin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 0d2cca335206ee59b922a31ed3cf3ba24ef4ecd7
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790784"
---
# <a name="what-is-conversation-learner"></a>Konuşma Öğrenici nedir?

Konuşma Öğrenici örnek etkileşimlerinden öğrenin damıtarak konuşma bağlamında kullanılabilen arabirimleri öğretmek ve oluşturmanıza olanak sağlar. 

Geleneksel yaklaşım, konuşma Öğrenici yanıtları artırmak ve daha ilgi çekici kullanıcı deneyimleri için bir iletişim kutusu uçtan uca bağlamında göz önünde bulundurur. Kapsayan çok sayıda görev odaklı kullanım örnekleri, konuşma Öğrenici robotlar ve akıllı aracıları kullanıcıları rahatsız, ek müşteri hizmeti ücrete neden olma olasılığını hale getirmek için arka planda machine learning uygulanır ve daha fazla spur daha sezgisel etkileşimler.

Geliştiriciler, girdiklerini taklit ederek istedikleri Prototipik iletişim kutuları girerek başlayın. Daha fazla iletişim kutuları girildiği gibi Model öğrenir. Bot, modeli de çalışmaya başladığında, son kullanıcılara dağıtılabilir. Konuşma Öğrenici konuşmaları kullanıcılarla kaydeder ve geliştirici gözden geçirebilirsiniz. Hataları anlaþýlmaz, geliştirici üzerinde--nokta düzeltme yapabilir ve hemen retrained ve kullanılabilir modelidir.

Bu yaklaşım, el ile iletişim kutusu denetim mantığının kodlama azaltan ve işletme sahipleri veya etki alanı uzmanları damıtarak konuşma bağlamında kullanılabilen bir arabirim önceki makine öğrenimi bilgi katkıda bulunmak sağlar. Bot, akıllı cihaz veya akıllı Aracısı bir parçası olarak dağıtılan olsun, konuşma Öğrenici hızla yeni beceriler, davranışları ve yetkinlikleri yineleme ve bunların kalitesinin hızlı bir şekilde geliştirin. 

Konuşma Öğrenici geliştiricilerin Microsoft Bot Framework ya da kendi altyapınızı kullanarak tek başına birden çok damıtarak konuşma bağlamında kullanılabilen kanallarından hızlı pazara açılma ve sürücü başarılı bitirmenize arttırırsınız güçlendirir.

Summary ve vurgular:

- Konuşma Öğrenici görev odaklı botlar oluşturmanın bir yapay ZEKA ilk yoludur.

- Bir uçtan uca yinelenen sinir ağı üzerinde (LSTM) kullanır ve konuşmaları doğrudan çok Aç örnekleri öğrenir. 

- Tasarımcılar, geliştiriciler, iş kullanıcıları ve çağrı merkezi çalışanları oluşturmasına ve botlar korumasına olanak tanır. 

- İş kuralları ve kod genel anlamda express olanağı sağlar.

- Oturumlarının eğitiminde sırasında sinir ağı modelinin sonraki beklenen eylemleri kümesini konuşmada puanlamak için kullanılır. Bot Geliştirici sonra doğru eylemi seçin ve düzgün yanıt sağlamak için ağ eğitin.
 
- Alıştırma tamamlandıktan sonra Geliştirici bot yanıtları düzeltmeler yapmak ve modeli yeniden eğitme Kullanıcı etkileşimlerine günlük iletişim kutularını kullanabilirsiniz. 

- Görevleri tamamlamak için etki alanına özgü ve üçüncü taraf API'leri çağırabilirsiniz.

