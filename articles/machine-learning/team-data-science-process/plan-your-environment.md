---
title: Senaryoları tanımlamak ve Azure - Analytics'i işleminizi planlama | Microsoft Docs
description: Gelişmiş analitikler için bir dizi anahtar soru dikkate alarak planlayın.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 421520dd-7728-4d29-889c-ebe6a0a6fb07
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: 16cc7c5841708b8b27cff4fcc7c93cdbb2fe0fa4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34838342"
---
# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Senaryoları tanımlama ve gelişmiş analiz verileri işlemeyi planlama
Hangi kaynaklara Gelişmiş analitik veri kümesi üzerinde işleme yapmak için bir ortamını ayarlama yaparken dikkate alınacak planlama yapmalıyım? Bu makalede bir dizi yardımcı sorulacak sorular öneren görevler ve kaynaklarla ilgili senaryonuz tanımlayın. Tahmine dayalı analiz için üst düzey adımlar dizisi kısmında özetlenen [takım veri bilimi işlem (TDSP) nedir?](overview.md). Bu adımların her biri, belirli bir senaryo ile ilgili görevleri için belirli kaynaklar gerektirir. Senaryonuz tanımlamak için önemli sorular veri Lojistik, özellikleri, veri kümeleri ve araçları ve analiz yapmayı tercih dilleri kalitesini ilgilendiren.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Lojistik sorular: veri konumları ve taşıma
Lojistik soruları konumunu ilgilendiren **veri kaynağı**, **hedef** Azure ve verilerin taşınması için gereksinimler, zamanlama, tutar ve ilgili kaynaklar dahil olmak üzere. Veri analizi işlemi sırasında birkaç kez taşınması gerekebilir. Azure üzerinde ardından Machine Learning Studio uygulamasına yerel veri depolama bazı forma taşıma ortak bir senaryodur.

1. **Veri kaynağınızı nedir?** Yerel veya bulutta mı? Örneğin:
   
   * Veriler bir HTTP adresi genel olarak kullanılabilir.
   * Verileri bir yerel/ağ dosya konumunda yer alıyor.
   * Verileri bir SQL Server veritabanında mevcut.
   * Veriler bir Azure depolama kapsayıcısında depolanır
2. **Azure hedef nedir?** Burada işleme veya model için olması gerekiyor mu? Örneğin:
   
   * Azure Blob Depolama
   * SQL Azure veritabanı
   * Azure VM’lerde SQL Server
   * Hdınsight (Hadoop Azure üzerinde) veya Hive tabloları
   * Azure Machine Learning
   * Edilebilme Azure sanal sabit diskler.
3. **Nasıl verileri taşımak mısınız?** Yordamlar ve alma veya birçok farklı depolama ve ortamlar işleme veri yüklemek kullanılabilir kaynakları aşağıdaki makalelerde özetlenmiştir:
   
   * [Analiz için depolama ortamlara veri yükleme](ingest-data.md)
   * [Çeşitli veri kaynaklarından Azure Machine Learning Studio'da eğitim verilerinizi almak](../studio/import-data.md).
4. **Verileri düzenli bir zamanlamaya göre taşınamaz veya geçiş sırasında değiştirilmiş gerekiyor mu?** Veri sürekli geçirilmesi, hem şirket içi erişir ve bulut kaynaklarına eklendiyse veya veri işlem yapılan işlem ya da değiştirilmiş veya iş mantığı kendisine geçirilen esnasında eklenmiş olması gerekiyor özellikle karma bir senaryoda, gerektiğinde Azure veri fabrikası (ADF) kullanmayı düşünün. Daha fazla bilgi için bkz: [veri taşıma bir şirket içi SQL Server'dan Azure Data Factory ile SQL Azure](move-sql-azure-adf.md)
5. **Verilerin ne kadar Azure'a taşınacak mi?** Son derece büyük veri kümeleri belirli ortamları depolama kapasitesini aşabilir. Bir örnek için boyutu sınırları hakkında bilgi için Machine Learning Studio'da sonraki bölüme bakın. Bu gibi durumlarda bir örnek veri çözümleme sırasında kullanılabilir. Aşağı örnek bir veri kümesi çeşitli Azure ortamlarda konusunda ayrıntılı bilgi için bkz [örnek veri takım veri bilimi işleminde](sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Veri özellikleri soruları: türünü, biçimi ve boyutu
Bu sorular depolama planlama ve her biri için çeşitli veri türlerine uygun ve her biri bazı kısıtlamalar sahip ortamlarda, işleme için anahtar vardır.

1. **Veri türleri nelerdir?** Örneğin:
   
   * Sayısal
   * Kategorik
   * Dizeler
   * İkili
2. **Verilerinizin nasıl biçimlendirilmiş?** Örneğin:
   
   * Virgülle ayrılmış (CSV) veya sekmeyle ayrılmış (TSV) düz dosyalar
   * Sıkıştırılmış veya sıkıştırılmamış
   * Azure BLOB'ları
   * Hadoop Hive tabloları
   * SQL Server tabloları
3. **Verilerinizi büyüklüğü nedir?**
   
   * Küçük: 2 GB'tan daha az
   * Orta: 2 GB ve 10 GB'değerinden büyük
   * Büyük: 10 GB daha büyük

Azure Machine Learning Studio ortamı örneğin alın:

* Veri biçimleri ve Azure Machine Learning Studio tarafından desteklenen türleri listesi için bkz: [veri biçimleri ve desteklenen veri türlerini](../studio/import-data.md#data-formats-and-data-types-supported) bölümü.
* Azure Machine Learning Studio için veri sınırlamaları hakkında daha fazla bilgi için bkz: **ne kadar büyük veri kümesi modüllerim da olabilir?** bölümünü [alma ve Machine Learning için verileri dışarı aktarma](../studio/faq.md#machine-learning-studio-questions)

Analytics işleminde kullanılan diğer Azure hizmetleriyle sınırlamaları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Veri Kalitesi soruları: araştırması ve ön işleme
1. **Verileriniz hakkında ne biliyor musunuz?** Temel özelliklerinden bir anlayın kazanmak için verileri keşfedin. Hangi desenleri veya onu sergiler, eğilimleri var. hangi aykırı değerlerini veya kaç değerleri eksik. Bu adım, en uygun özellikleri önermek veya analizini yazın varsayımlar formulating ve ek veri toplama için planları formulating gerekli ön işleme kapsamını belirlemek için önemlidir. Açıklayıcı istatistikleri hesaplama ve görselleştirmeleri çizme veri İnceleme için yararlı tekniklerle aynıdır. Bir veri kümesi çeşitli Azure ortamlarda keşfetmek nasıl Ayrıntılar için bkz [takım veri bilimi işlemi keşfedin](explore-data.md).
2. **Verilerin önceden işlenmesi veya temizleme gerektiriyor mu?**
   Ön işleme ve veri temizleme dataset machine learning için etkili bir şekilde kullanılabilmesi için önce genellikle gerçekleştirilmesi gereken önemli görevlerdir. Ham verileri gürültülü ve güvenilmeyen görülür ve değerleri eksik olabilir. Bu tür veriler için modelleme kullanarak yanıltıcı sonuçlara yol açabilir. Bir açıklama için bkz: [veri Gelişmiş için hazırlamak üzere görevleri makine öğrenme](prepare-data.md).

## <a name="tools-and-languages-questions"></a>Araçlar ve dilleri soruları
Çok sayıda veya bağlı olarak hangi dilleri ve geliştirme ortamları veya Araçlar, gereken en conformable kullanarak burada seçenekleri vardır.

1. **Hangi dilleri analiz için kullanmayı tercih ediyorsunuz?**  
   
   * R
   * Python
   * SQL
2. **Hangi Araçlar veri analizi için kullanmalısınız?**
   
   * [Microsoft Azure Powershell](/powershell/azure/overview) -komut dosyası dili Azure kaynaklarınızı yönetmek için kullanılan bir komut dosyası dili.
   * [Azure Machine Learning Studio](../studio/what-is-ml-studio.md)
   * [Devir analizi](http://www.revolutionanalytics.com/revolution-r-open)
   * [Rstudio'dan](http://www.rstudio.com)
   * [Visual Studio için Python Araçları](http://microsoft.github.io/PTVS/)
   * [Anaconda](https://www.continuum.io/why-anaconda)
   * [Jupyter Not Defterleri](http://jupyter.org/)
   * [Microsoft Power BI](http://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Gelişmiş analizler senaryonuz tanımlayın
Önceki bölümde sorulara verdiğiniz yanıtlara sonra en iyi hangi senaryonun durumunuza uygun olduğunu belirlemek hazırsınız. Örnek senaryolar özetlenen [senaryoları Azure Machine Learning, gelişmiş analizler için](plan-sample-scenarios.md).

