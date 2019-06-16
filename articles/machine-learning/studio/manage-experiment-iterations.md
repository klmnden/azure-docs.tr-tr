---
title: Deneme yinelemelerini yönetme
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio'da deneme yinelemelerini yönetme konusunda. Önceki çalıştırmaları, denemelerinizi meydan okuyun, yeniden ziyaret, sonuçta onaylayın veya önceki varsayımlar daraltmak için herhangi bir zamanda gözden geçirebilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 03/20/2017
ms.openlocfilehash: 34a72f2e7b6be90654c0f053d5b8978b0283d56c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60860255"
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio’da deneme yinelemelerini yönetme
Tahmine dayalı analiz modeli geliştirmek olduğu bir süreçtir - çeşitli işlevleri ve denemenizi parametrelerini değiştirirken, memnun ana kadar sonuçlarınız yakınsanır eğitilmiş ve verimli bir model vardır. Bu işlem için anahtar deneme parametreleri ve yapılandırmaları çeşitli yinelemeler izlemektir.



Önceki çalıştırmaları, denemelerinizi meydan okuyun, yeniden ziyaret, sonuçta onaylayın veya önceki varsayımlar daraltmak için herhangi bir zamanda gözden geçirebilirsiniz. Machine Learning Studio, bir denemeyi çalıştırırken, veri kümesi, modül ve bağlantı noktası bağlantıları ve parametreleri de dahil olmak üzere çalışma bir geçmişini tutar. Bu geçmiş, ayrıca sonuçları, başlatma ve durdurma zamanları, günlük iletilerini ve yürütme durumu gibi çalışma zamanı bilgileri yakalar. Bu çalıştırmaları birini deneyin ve Ara sonuçlar uygulanmasına geçmişi gözden geçirmek istediğiniz zaman geri bakabilirsiniz. Yoldaki basit, karmaşık veya hatta topluluğu modelleme çözümleri oluşturmak için yeni bir sorgu ve bulma aşaması başlatmak için denemenizi önceki bir çalıştırmada bile kullanabilirsiniz.

> [!NOTE]
> Bir deney önceki bir çalıştırmada görüntülediğinizde, deneme sürümünün kilitli ve düzenlenemez. Ancak, bir kopyasını tıklayarak kaydedebileceğiniz **SAVE AS** ve kopya için yeni bir ad sağlamayı. Daha sonra düzenleyin ve çalıştırmak yeni bir kopyasını Machine Learning Studio'da açılır. Bu kopyasını denemenizi kullanılabilir **DENEMELERİ** birlikte diğer tüm denemelerinizin listesi.
> 
> 

## <a name="viewing-the-prior-run"></a>Önceki çalıştırma görüntüleme
En az bir kez çalıştırdığınız deneme açık olduğunda, tıklayarak denemeyi önceki yürütülmesi görüntüleyebilirsiniz **önceki çalıştırma** Özellikler bölmesinde.

Örneğin, bir deneme oluşturmak ve 11: 23'te bu sürümlerini çalıştıran varsayalım 11:42 ve 11:55. Deneme (11:55)'ın son çalıştırılmasındaki açıp tıklayın **önceki çalıştırma**, 11:42 çalıştırdığınız sürümü açılır.

## <a name="viewing-the-run-history"></a>Çalıştırma geçmişini görüntüleme
Bir deney, tüm önceki çalıştırmaları tıklayarak görüntüleyebilirsiniz **çalıştırma geçmişini görüntüle** açık bir denemede.

Örneğin, ile deneme oluşturduğunuz düşünün [doğrusal regresyon] [ linear-regression] modülü ve değerinin değiştirilmesi etkisini gözlemlemek istediğiniz **öğrenme oranı** üzerinde sonuçları denemeler yapın. Denemeyi birden çok kez bu parametre için farklı değerlerle şu şekilde çalıştırın:

| Öğrenme oranı değeri | Çalıştırma başlangıç zamanı |
| --- | --- |
| 0.1 |9/11/2014 4:18:58 pm |
| 0.2 |9/11/2014 4:24:33 pm |
| 0.4 |9/11/2014 4:28:36 pm |
| 0,5 |9/11/2014 4:33:31 pm |

Tıklarsanız **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE**, bu çalıştırmaları listesi görürsünüz:

![Örneği çalıştırma geçmişi](./media/manage-experiment-iterations/viewrunhistory.jpg)

Herhangi bir anlık görüntü deneyde, çalıştırdığınız zaman görüntülemek için bu çalışır birine tıklayın. Yapılandırma, parametre değerleri, açıklamaları ve sonuçları tüm bu çalıştırmanın denemenizin eksiksiz bir kaydı size korunur.

> [!TIP]
> Yinelemelerinizi deneyde belgelemek için her zaman başlığı değiştirebilirsiniz. Bunu çalıştırmanız, güncelleştirebilirsiniz **özeti** deneyde özellikleri bölmesi ve ekleyebilir veya güncelleştirebilirsiniz kaydetmek için tek tek modüllerinin açıklamaları, değiştirir. Başlık, Özet, modül açıklamaları deneme ile her bir çalıştırmanın kaydedilir.
> 
> 

Alanındaki denemeler listesini **DENEMELERİ** Machine Learning Studio'da sekmesi, her zaman bir denemeyi en son sürümünü görüntüler. Denemeyi önceki bir çalıştırmada açarsanız (kullanarak **önceki çalıştırma** veya **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE**), tıklayarak taslak sürümünde döndürebilir **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE** ve seçme sahip yineleme bir **durumu** , **düzenlenebilir**.

## <a name="iterating-on-a-previous-run"></a>Önceki bir çalıştırmada yineleme
Tıkladığınızda **önceki çalıştırma** veya **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE** ve bir önceki çalıştırma açın, tamamlanmış bir deneme salt okunur modda görüntüleyebilirsiniz.

Denemenizin yapılandırdığınız, bir önceki çalıştırma için yol ile başlayan bir yinelemesini başlamak isterseniz, Farklı Çalıştır'ı açıp tıklatarak bunu yapabilirsiniz **SAVE AS**. Bu yeni bir deneme çalıştırma geçmişi, boş bir yeni başlık oluşturur ve tüm bileşenleri ve parametre değerlerini önceki çalıştırın. Bu yeni bir deneme listelenen **DENEMELERİ** Machine Learning Studio'ya giriş sayfası ve sekmede değiştirebilirsiniz ve bu, yeni bir başlatma run denemenizin bu yineleme için geçmişi. 

Örneğin, denemeyi çalıştırma geçmişi önceki bölümde gösterilen olduğunu varsayalım. Ayarladığınızda neler olduğunu gözlemek istediğiniz **öğrenme oranı** parametresi için farklı değerler deneyin ve 0.4 **eğitim dönemlerinde sayısı** parametresi.

1. Tıklayın **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE** ve 4:28:36 (içinde ayarladığınız parametre değeri için 0.4) pm, çalıştırdığınız deneyde yinelemeyi açın.
2. Tıklayın **Kaydet**.
3. Yeni bir başlık girin ve tıklayın **Tamam** onay işareti. Denemeyi yeni bir kopyası oluşturulur.
4. Değiştirme **eğitim dönemlerinde sayısı** parametresi.
5. Tıklayın **ÇALIŞTIRMA**.

Artık bu sürümünü denemenizi, çalıştırmak ve değiştirmek çalışmanızı kaydetmek için yeni bir çalıştırma geçmişi oluşturmaya devam edebilirsiniz.

<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
