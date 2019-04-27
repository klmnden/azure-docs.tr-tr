---
title: PostgreSQL için Azure veritabanı'nda Query Store kullanım senaryoları
description: Bu makalede, PostgreSQL için Azure veritabanında Query Store için bazı senaryolar açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/26/2018
ms.openlocfilehash: 873462354b70d13e56ca108c3257031ef34873f8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60563197"
---
# <a name="usage-scenarios-for-query-store"></a>Query Store kullanım senaryoları

**Şunlara uygulanır:** 9.6 ve 10 PostgreSQL için Azure veritabanı

Query Store çeşitli izleme ve bakımı öngörülebilir iş yükü performansının kritik olduğu senaryolarda kullanabilirsiniz. Aşağıdaki örnekleri dikkate alın: 
- Tanımlama ve pahalı en sık kullanılan sorgular ayarlama 
- A / B testi 
- Yükseltme sırasında performans kararlı tutma 
- Tanımlama ve geçici iş yükleri geliştirme 

## <a name="identify-and-tune-expensive-queries"></a>Tanımlamak ve pahalı sorgu ayarlama 

### <a name="identify-longest-running-queries"></a>Uzun çalışan sorguları belirleyin 
Kullanım [sorgu performansı İçgörüleri](concepts-query-performance-insight.md) en uzun çalışan sorguların hızlı bir şekilde tanımlamak için Azure portalında görünümü. Bu sorguları genellikle önemli ölçüde kaynakları kullanma eğilimindedir. Uzun süre çalışan sorularınızı en iyi duruma getirme, kaynakları kullanmak için sistem üzerinde çalışan diğer sorgular tarafından azaltarak performansı artırabilir. 

### <a name="target-queries-with-performance-deltas"></a>Performans deltaları hedef sorguları 
Query Store performans verilerini zaman pencereleri halinde böler, böylece zaman içinde bir sorgu performansını izleyebilirsiniz. Bu tam olarak belirlemenize, sorguları katkıda bulunuyorsanız harcanan toplam zamanı arasında bir artış yardımcı olur. Sonuç olarak, hedeflenen iş yükünüz sorun giderme yapabilirsiniz.

### <a name="tuning-expensive-queries"></a>Sorgular pahalı ayarlama 
Bir sorgu ile performansın tespit ederken, sorunun doğasına üzerinde gerçekleştirdiğiniz eylemi bağlıdır: 
- Kullanım [performans önerileri](concepts-performance-recommendations.md) olmadığını belirlemek için dizinleri önerilir. Evet ise, dizin oluşturma ve sonra dizini oluşturduktan sonra sorgu performansını değerlendirmek için Query Store kullanın. 
- İstatistikleri sorgu tarafından kullanılan temel alınan tablolar için güncel olduğundan emin olun.
- Pahalı sorguları yeniden yazmak göz önünde bulundurun. Örneğin, sorgu parametreleştirmesinin avantajlarından yararlanın ve dinamik SQL kullanımını azaltabilirsiniz. Veri filtreleme veritabanı tarafında, uygulama tarafından değil, uygulama gibi verileri okunurken, en iyi mantığını uygular. 


## <a name="ab-testing"></a>A / B testi 
Önce ve sonra bir uygulama değişikliği tanıtmak için planlama iş yükü performansını karşılaştırmak için Query Store kullanın. Query Store ortam veya uygulamanın etkisini değerlendirmek için kullanılan senaryo örnekleri, iş yükü performansı değiştirin: 
- Bir uygulamanın yeni bir sürümü alınıyor. 
- Ek kaynaklar sunucuya ekleme. 
- Pahalı sorgular tarafından başvurulan tablolarda eksik dizinler oluşturuluyor. 
 
Tüm bu senaryolar aşağıdaki iş akışı geçerlidir: 
1. İş yükünüz performans taban çizgisi oluşturmak için önce planlanan değişiklik Query Store ile çalıştırın. 
2. Zaman denetlenen anda uygulama değişiklikler uygulanır. 
3. İş yükü değişiklikten sonra performans sistem görüntüsü oluşturmak için yeteri kadar çalıştırmaya devam edin. 
4. Öğesinden önce ve sonra değişiklik sonuçlarını karşılaştırın. 
5. Değişikliği veya geri alma tutmaya karar verin. 


## <a name="identify-and-improve-ad-hoc-workloads"></a>Tanımlayın ve geçici iş yükleri artırın 
Bazı iş yükleri, genel uygulama performansını artırmak için ayar yapabilirsiniz baskın sorgular yoktur. Bu iş yüklerini göreceli olarak çok sayıda benzersiz sorguları ile bunların sistem kaynaklarının bir kısmını kullanan her biri genellikle belirlenir. Her benzersiz sorgu seyrek, bu nedenle tek çalışma zamanı tüketimi için kritik değildir yürütülür. Öte yandan, uygulama her zaman yeni sorgular oluşturan koşuluyla, sistem kaynaklarının büyük bir kısmı en iyi sorgu derleme üzerinde harcanır. Genellikle, bu durum uygulamanızı sorgular (yerine saklı yordamları ya da parametreli sorgular kullanma) oluşturuyorsa veya varsayılan olarak sorgular oluşturmak, nesne ilişkisel eşleme çerçeveleri dayanıyorsa'olmuyor 
 
Uygulama kodu denetiminde olması durumunda, saklı yordamları ya da parametreli sorgular kullanmayı veri erişim katmanı yeniden yazmayı düşünebilirsiniz. Ancak, bu durum Ayrıca uygulama değişiklikleri tüm veritabanını (tüm sorguları) veya sorguyu şablonları ile aynı sorgu karma sorgu parametreleştirmesinin zorlayarak geliştirilebilir. 

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Query Store kullanmak için en iyi yöntemler](concepts-query-store-best-practices.md)