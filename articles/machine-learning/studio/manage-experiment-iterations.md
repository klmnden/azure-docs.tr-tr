---
title: Machine Learning Studio'da deneme yinelemelerini yönetme | Microsoft Docs
description: Azure Machine Learning Studio'da deneme yinelemelerini yönetme
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 6a53530f-20d5-40ae-9b49-7b499ccb44b7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: c5419eed1de50c29cf6e5bcaf7070c48d7a335ae
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio’da deneme yinelemelerini yönetme
Tahmine dayalı analiz modeli geliştirmek olduğu yinelemeli süreç - memnun olana kadar sonuçlarınız yakınsanır çeşitli işlevleri ve denemenizi parametrelerini değiştirirken, eğitilmiş ve verimli bir model sahip. Bu işlem anahtarına deneme parametreleri ve yapılandırmaları çeşitli yinelemelerini izlemektir.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

Sınama, yeniden ziyaret ve sonuçta onaylayın ya da önceki varsayımlar iyileştirmek için herhangi bir zamanda denemelerinizi önceki çalışmalarını gözden geçirebilirsiniz. Bir deneme çalıştırdığınızda, Machine Learning Studio veri kümesi, modül ve bağlantı noktası bağlantıları ve parametreleri de dahil olmak üzere çalışma geçmişini tutar. Bu geçmiş ayrıca sonuçları, başlatma ve durdurma sürelerini, günlük iletilerini ve yürütme durumu gibi çalışma zamanı bilgileri yakalar. Bu çalıştırır hiçbirini uygulanmasına geçmişi deneme ve Ara sonuçlarını gözden geçirmek için herhangi bir zamanda geri bakabilirsiniz. Denemenizi, önceki bir çalışmasında bile yolunda basit, karmaşık veya hatta ensemble modelleme çözümleri oluşturmak için yeni bir soru ve bulma aşaması başlatmak için de kullanabilirsiniz.

> [!NOTE]
> Önceki bir çalışmasında bir deneme görüntülediğinizde, bu deneme sürümünün kilitli ve düzenlenemez. Ancak, bir kopyasını tıklayarak kaydedebileceğiniz **SAVE AS** ve kopya için yeni bir ad sağlar. Machine Learning Studio'da daha sonra Düzenle ve çalıştırmak yeni bir kopyası açılır. Bu kopyası denemenizi kullanılabilir **DENEMELER** diğer tüm denemelerinizi birlikte listesi.
> 
> 

## <a name="viewing-the-prior-run"></a>Önceki çalışma görüntüleme
En az bir kez çalıştır bir deneme açık olduğunda tıklayarak denemeyi önceki yürütülmesi görüntüleyebilirsiniz **önceki Çalıştır** Özellikler bölmesinde.

Örneğin, bir deneme oluşturmak ve 11: 23'te bu sürümlerini çalıştıran varsayalım 11:42 ve 11:55. Deneme (11:55)'ın son çalıştırılmasındaki açın ve'ı tıklatırsanız **önceki Çalıştır**, 11:42 çalıştırdığınız sürümü açılır.

## <a name="viewing-the-run-history"></a>Çalıştırma geçmişini görüntüleme
Bir deneme önceki tüm çalışmalarınızı tıklayarak görüntüleyebilirsiniz **çalıştırma geçmişini görüntüle** açık bir deney olarak.

Örneğin, bir deneme ile oluşturduğunuz varsayalım [doğrusal regresyon] [ linear-regression] modülü ve değerinin değiştirilmesi etkisini izlemek istediğinizde **öğrenme oranı** üzerinde Deneme sonuçları. Denemeyi birden çok kez bu parametre için farklı değerler ile aşağıdaki gibi çalıştırın:

| Öğrenme hızı değeri | Çalıştırma başlangıç zamanı |
| --- | --- |
| 0.1 |9/11/2014 4:18:58 pm |
| 0.2 |11/9/2014 4:24:33 pm |
| 0.4 |11/9/2014 4:28:36 pm |
| 0.5 |11/9/2014 4:33:31 pm |

Tıklatırsanız **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE**, bu çalıştırır listesi Bkz:

![Örnek çalıştırma geçmişi][runhistory]

Herhangi bir anlık görüntüsünü çalıştırdığınızda, deneme görüntülemek için bu çalıştırır birini tıklatın. Yapılandırma, parametre değerleri, açıklamalar ve sonuçları tüm denemenizi bu yürütülmesi tamamlandı kaydını vermek için korunur.

> [!TIP]
> Deneme yinelemelerini belgelemek için her zaman başlığını değiştirebilirsiniz. Bunu çalıştırmanız, güncelleştirebilirsiniz **Özet** deneme özelliklerinde, bölmesinde ve ekleyebilir ya da güncelleştirebilirsiniz kaydetmek için tek tek modülleri yorumları, değiştirir. Başlık, özetini ve modül açıklamaları her denemeyi çalıştırma ile kaydedilir.
> 
> 

Alanındaki denemeler listesi **DENEMELER** sekmesi Machine Learning Studio'da her zaman en son bir deneme sürümünü görüntüler. Denemeyi, önceki bir çalışmasında açarsanız (kullanarak **önceki Çalıştır** veya **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE**), tıklatarak taslak sürüm dönebilirsiniz **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE** ve seçme sahip yineleme bir **durumu** , **düzenlenebilir**.

## <a name="iterating-on-a-previous-run"></a>Önceki bir çalışmasında üzerinde yineleme
Tıkladığınızda **önceki Çalıştır** veya **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE** ve önceki bir çalışmasında açın, tamamlanmış bir deneme salt okunur modunda görüntüleyebilirsiniz.

Denemenizi, yapılandırma, bir önceki çalıştırma için yönteminiz ile başlayarak, bir yinelemeye başlamak isterseniz, Çalıştır açıp tıklatarak bunu yapabilirsiniz **SAVE AS**. Bu yeni bir deneme çalıştırma geçmişi, boş bir yeni başlık oluşturur ve tüm bileşenleri ve parametre değerlerini önceki çalıştırın. Bu yeni bir deneme listelenen **DENEMELERİNİ** Machine Learning Studio giriş sayfası ve sekmede değiştirebilirsiniz ve bu, yeni bir başlatma run denemenizi bu yinelemesi geçmişini. 

Örneğin, önceki bölümde gösterilen geçmişi çalıştırmak deneme olduğunu varsayalım. Ayarladığınız zaman neler gözlemlemek istediğiniz **öğrenme oranı** parametresi 0.4 ve için farklı değerler deneyin **eğitim dönemlerinde sayısı** parametresi.

1. Tıklatın **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE** ve 4:28:36 (hangi parametre değerine ayarlamanız için 0.4) pm adresindeki çalıştırdığınız deneme yinelemesi açın.
2. Tıklatın **SAVE AS**.
3. Yeni bir başlık girin ve tıklayın **Tamam** onay işareti. Yeni bir deneme kopyası oluşturulur.
4. Değiştirme **eğitim dönemlerinde sayısı** parametresi.
5. Tıklatın **çalıştırmak**.

Şimdi bu sürümü, denemeyi çalıştırmak ve değiştirmek, çalışmanızı kaydetmek için yeni bir çalıştırma geçmişi oluşturmaya devam edebilirsiniz.

<!-- Images -->
[runhistory]:./media/manage-experiment-iterations/viewrunhistory.jpg


<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
