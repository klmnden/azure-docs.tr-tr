---
title: Azure Önizleme Gözcü çalışmalarıyla araştırma | Microsoft Docs
description: Azure Gözcü çalışmalarıyla araştırmak öğrenmek için bu öğreticiyi kullanın.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: a493cd67-dc70-4163-81b8-04a9bc0232ac
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: 6b3357ec06c89645b9c41e9efdb582a18af40672
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60614657"
---
# <a name="tutorial-investigate-cases-with-azure-sentinel-preview"></a>Öğretici: Azure Önizleme Gözcü çalışmalarıyla araştırın

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu öğretici Azure Gözcü ile tehditleri algılamanıza yardımcı olur.

Çalıştırdıktan sonra [veri kaynaklarınıza bağlı](quickstart-onboard.md) şüpheli bir şey olduğunda size bildirilmesini istiyorsanız Azure Gözcü için. Bunu sağlamak için oluşturduğunuz Azure Gözcü atayabileceğiniz çalışmaları üreten uyarı kuralları ve kullanım anomalileri ve ortamınızdaki tehditlere derin bir şekilde araştırmak için Gelişmiş sağlar. 

> [!div class="checklist"]
> * Çalışmaları oluşturma
> * Durumları inceleme
> * Tehditlere yanıt verme

## <a name="investigate-cases"></a>Durumları inceleme

Bir durum, birden çok uyarı içerebilir. Bu, belirli bir araştırma için tüm kanıt bir toplama olur. Servis talebi, tanımladığınız uyarılar temel alınarak oluşturulur **Analytics** sayfası. Uyarıları önem ve durum gibi ilgili özellikleri büyük/küçük harf düzeyinde ayarlanır. Azure, bildiğiniz aradığınız tehditleri ve bunları nasıl bulacağınıza ilişkin ne tür Gözcü izin sonra durumlarda araştırma tarafından algılanan tehditleri izleyebilirsiniz. 

1. Seçin **çalışmaları**. **Çalışmaları** sayfa, sahip olduğunuz kaç çalışması bilmenizi sağlar, ne kadar kaç ayarladığınız açık olduğunu **sürüyor**, ve kaç kapatılır. Her durumda, gerçekleştiği zaman ve servis talebi durumunu görebilirsiniz. İlk işlemeye karar verirken önem derecesi arayın. İçinde **çalışmaları** sayfasında **uyarılar** olayla ilgili tüm uyarıları görmek için sekmesinde. Durum bölümü görüntülenebilir önceki eşlenen varlıkları **varlıkları** sekmesi.  Durumlarda, örneğin durumu veya önem sırasına göre gerektiği şekilde filtreleyebilirsiniz. Baktığınızda **çalışmaları** sekmesinde tanımlanan algılama kurallarınızı tarafından tetiklenen uyarılar içerir açık durumda görürsünüz **Analytics**. Üst kısmında gördüğünüz etkin servis talepleri, yeni çalışmaları ve ilerleme durumlarda. Önem derecesine göre tüm çalışmalarınızı genel bir bakış da görebilirsiniz.

   ![Uyarı Panosu](./media/tutorial-investigate-cases/cases.png)

2. Bir araştırma başlamak için belirli bir servis talebi üzerinde tıklayın. Sağ tarafta (sizin eşlemesini göre), önem derecesi, Özet ilgili varlıkların sayısı dahil olmak üzere durumu için ayrıntılı bilgileri görebilirsiniz. Her durumda benzersiz bir kimliğe sahiptir. Servis talebi önem durumda dahil en önemli uyarı göre belirlenir.  

1. Bir durumda uyarılar ve varlıklar hakkında daha fazla ayrıntı görüntülemek için tıklayın **tam ayrıntılarını görüntüleme** durumda sayfasında ve durum bilgileri özetleyen ilgili sekmeleri gözden geçirin.  Tam durum görünümü, uyarı, ilişkili uyarı ve varlıkları tüm kanıt birleştirir.

1. İçinde **uyarılar** sekmesinde, uyarının kendisine - tetiklendi zaman ve ne kadar belirlediğiniz eşikleri aşıldı tarafından gözden geçirin. Uyarı: Sorgu ve uyarı üzerinde playbook'ları çalıştırma olanağı başına döndürülen sonuç sayısı uyarıyı tetikleyen sorgu ilgili tüm bilgileri görebilirsiniz. Detaya gitmek için aşağı daha durumuna, isabet sayısına tıklayın. Bu, sonuçları ve uyarının Log analytics'te sonuçlar oluşturulan sorgu açar.

3. İçinde **varlıkları** sekmesinde uyarı kuralı tanımının bir parçası, eşleşen tüm varlıkları görebilirsiniz. 

4. Bir durum etkin olarak araştırdığınızı ise, durumu ayarlamak için iyi bir fikirdir **sürüyor** Siz kapatana kadar. Durum da kapatabilirsiniz burada **çözümlenen kapalı** durumu gösteren bir olay işlendiğini, çalışmaları sırasında **kapatılmış kapalı** işleme gerektirmeyen çalışmaları durumu. Açıklamalar, servis talebi kapatma, mantık açıklayan gereklidir.

5. Durumlarda, belirli bir kullanıcıya atanabilir. Her durum için durum ayarlayarak bir sahip atayabilir **sahibi** alan. Tüm durumlarda başlangıç olarak atanmamış. Örneklerinden gidin ve sahip olduğunuz tüm durumları görmek için ada göre Filtrele. 

5. Tıklayın **Araştır** araştırma Haritası ve düzeltme adımları, ihlal kapsamına görüntülemek için. 



## <a name="respond-to-threats"></a>Tehditlere yanıt verme

Azure Sentinel playbook'ları kullanarak tehditlerine yanıt verme için iki birincil seçenek sunar. Playbook bir uyarı tetiklenir ya da bir uyarıya yanıt olarak bir playbook el ile çalıştırabilirsiniz otomatik olarak çalışacak şekilde ayarlayabilirsiniz.

- Playbook playbook yapılandırdığınızda, bir uyarı tetiklendiğinde otomatik olarak çalışacak şekilde ayarlayabilirsiniz. 

- Gelen bir playbook uyarı içine tıklayarak el ile çalıştırabilirsiniz **playbook'ları görüntüleme** ve sonra çalıştırılacak bir playbook seçerek.




## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure Gözcü kullanarak araştırmaya başlama öğrendiniz. Öğreticiye devam [otomatik playbook'ları kullanarak tehditleri nasıl](tutorial-respond-threats-playbook.md).
> [!div class="nextstepaction"]
> [Tehditleri](tutorial-respond-threats-playbook.md) tehditleri verdiğiniz yanıtları otomatik hale getirmek için.

