---
title: Machine Learning Studio'ya - Azure veri aktarma | Microsoft Docs
description: Verilerinizi çeşitli veri kaynaklarından Azure Machine Learning Studio'ya içeri aktarma. Hangi veri türlerini ve veri biçimleri desteklendiğini öğrenin.
keywords: verileri, veri biçimi, veri türleri, veri kaynakları, eğitim verilerini içeri aktarma
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: 2f8c1eb43fddb21a59d4f00fd86b08d3fb3608f4
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269966"
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Eğitim verilerinizi çeşitli veri kaynaklarından Azure Machine Learning Studio’ya alma

Geliştirmek ve Tahmine dayalı analiz çözümü eğitmek için Machine Learning Studio'da kendi verilerinizi kullanmak için veri kullanabilirsiniz: 

* A [ **yerel dosya** ](import-data-from-local-file.md) -çalışma alanınızda bir veri kümesi modülü oluşturmak için sabit sürücünüzden önceden yerel veri yükleme
* [**Çevrimiçi veri kaynakları** ](import-data-from-online-sources.md) -kullanım [verileri içeri aktarma] [ import-data] denemenizi çalışırken çeşitli çevrimiçi kaynaklardan birinden modülü verilere erişme
* [**Machine Learning Studio denemesine** ](import-data-from-an-experiment.md) -Machine Learning Studio'da bir veri kümesi olarak kaydedilmiş olan veri kullanın
* [**Şirket içi SQL Server veritabanı** ](use-data-from-an-on-premises-sql-server.md) -verileri el ile kopyalamak zorunda kalmadan bir şirket içi SQL Server veritabanından veri kullanan

> [!NOTE]
> Kullanılabilir Machine Learning Studio'da eğitim verilerini için kullanabileceğiniz birçok örnek veri kümesi yok. Bunlar hakkında daha fazla bilgi için bkz: [Azure Machine Learning Studio'da örnek veri kümelerini kullanan](use-sample-datasets.md)).
> 
> 

Giriş niteliğindeki bu makalede, aynı zamanda Machine Learning Studio'da kullanıma hazır verileri alma açıklar ve hangi veri biçimleri ve veri türleri desteklenir açıklar.

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da kullanıma hazır veri alma
Machine Learning Studio, ayrılmış veya bazı durumlarda dikdörtgen olmayan veri kullanılabilmesine rağmen bir veritabanından veri yapılandırılmış olan metin veriler gibi dikdörtgen ya da tablolu verileri ile çalışacak şekilde tasarlanmıştır.

Verilerinizi görece temiz ise en iyisidir. Diğer bir deyişle, tırnak işareti olmayan dizeler gibi sorunların denemenize verileri karşıya yüklemeden önce dikkatli olmanız gerekir.

Ancak, kullanılabilir modülleri Machine Learning Studio'da, bazı düzenleme deneyiminizi içinde veri etkinleştirin. Yapılandırmanıza bağlı olarak makine öğrenimi algoritmaları kullanacaksınız, eksik değerleri ve seyrek veri gibi veri yapısal sorunlar ele alacağız nasıl karar gerekebilir ve yardımcı olan ile modüller vardır. Konum **veri dönüştürme** bu işlevleri gerçekleştirmek modüller için modül paletinin bölümü.

Denemenizin herhangi bir noktada görüntüleyebilir veya çıkış bağlantı noktasına tıklayarak modülü tarafından üretilen veri indirin. Modül bağlı olarak farklı indirme seçenekleri kullanılabilir olabilir veya Machine Learning Studio'da, web tarayıcınızdan verileri görselleştirmek mümkün olabilir.

## <a name="data-formats-and-data-types-supported"></a>Desteklenen veri biçimlerini ve veri türleri
Denemenize birkaç veri türleri içeri aktarabilirsiniz, ne mekanizması bağlı olarak, veri ve burada işleminin yapıldığı içeri aktarın:

* Düz metin (.txt)
* Virgülle ayrılmış değerler (CSV üst bilgisi (.csv) ile veya olmadan) (. nh.csv)
* Sekmeyle ayrılmış değerler (TSV üst bilgisi (.tsv) ile veya olmadan) (. nh.tsv)
* Excel dosyası
* Azure tablosu
* Hive tablosu
* SQL veritabanı tablosu
* OData değerleri
* SVMLight veri (.svmlight) (bkz [SVMLight tanımı](http://svmlight.joachims.org/) biçim bilgilerini için)
* İlişki dosyası biçimi'ne (ARFF) veri (.arff) özniteliği (bkz [ARFF'ye tanımı](http://weka.wikispaces.com/ARFF) biçim bilgilerini için)
* Zip dosyası (.zip)
* R nesne veya çalışma alanı dosyası (. RData)

Meta veriler içeren ARFF'ye gibi bir biçim verileri içe aktarırsanız, Machine Learning Studio başlık ve her bir sütunun veri türünü tanımlamak için bu metaveriyi kullanır.

Bu meta veriler içermeyen TSV veya CSV biçiminde gibi verileri içe aktarırsanız, Machine Learning Studio, her bir sütunun veri türünü verileri yeniden örnekleyerek çıkarır. Machine Learning Studio, verileri de sütun başlıklarını yoksa, varsayılan adları sağlar.

Açıkça belirtebilir veya sütunların kullanarak başlıklar ve veri türlerini değiştirme [meta verileri Düzenle][edit-metadata].

Aşağıdaki **veri türleri** Machine Learning Studio tarafından tanınan:

* Dize
* Tamsayı
* çift
* Boole
* DateTime
* Zaman aralığı

Machine Learning Studio'da adlı bir iç veri türü kullanan ***veri tablosu*** modülleri arasında veri iletmek için. Verilerinizi biçimini kullanarak veri tablosuna açıkça dönüştürebilir [veri kümesine Dönüştür] [ convert-to-dataset] modülü.

Veri tablosu dışında biçimlerini kabul eden herhangi bir modülü verileri veri tablosuna sessizce sonraki modülüne iletmeden önce dönüştürür.

Gerekirse, CSV, TSV, ARFF'ye geri verileri tablo biçiminde veya diğer dönüştürme modüllerini kullanarak SVMLight biçimine dönüştürebilirsiniz.
Konum **veri biçim dönüştürmelerini** bu işlevleri gerçekleştirmek modüller için modül paletinin bölümü.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
