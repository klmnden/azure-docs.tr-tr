---
title: Machine Learning Studio'ya veri içeri aktarma | Microsoft Docs
description: Azure Machine Learning Studio çeşitli veri kaynaklarından veri aktarmak nasıl. Hangi veri türleri ve veri biçimleri desteklenir öğrenin.
keywords: veriler, veri biçimi, veri türleri, veri kaynakları, eğitim verilerini içeri aktarma
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: a5750555802489b41b007831164767beb953ebc4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837472"
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Eğitim verilerinizi çeşitli veri kaynaklarından Azure Machine Learning Studio’ya alma
Geliştirmek ve Tahmine dayalı analiz çözümü eğitmek için Machine Learning Studio'da kendi verilerinizi kullanmak için aşağıdakileri yapabilirsiniz: 

* Verileri yüklemek bir **yerel dosya** çalışma alanınızda bir veri kümesi modülü oluşturmak için sabit sürücünüzden vaktinden
* erişim verileri birinden birkaç **çevrimiçi veri kaynakları** denemenizi kullanarak çalışırken [veri içeri aktarma] [ import-data] Modülü 
* verileri başka bir Azure Machine learning kullanarak **denemeler** bir veri kümesi kaydedildi
* Şirket içi verileri kullanan **SQL Server veritabanı**

Bu seçeneklerin her biri konulardan birine menüsünde açıklanmıştır. Bu konular Machine Learning Studio'da kullanmak için bu çeşitli veri kaynaklarından veri içeri aktarma gösterir. 

[!INCLUDE [import-data-into-aml-studio-selector](../../../includes/machine-learning-import-data-into-aml-studio.md)]

> [!NOTE]
> Eğitim verileri için kullanabileceğiniz bir Machine Learning Studio'da kullanılabilir birçok örnek veri kümesi yok. Bunlar hakkında daha fazla bilgi için bkz: [Azure Machine Learning Studio'daki örnek veri kümelerini kullanan](use-sample-datasets.md)).
> 
> 

Bu giriş konu aynı zamanda veri Machine Learning Studio'da kullanım için hazır hale getirmek nasıl açıklanır ve hangi veri biçimleri ve veri türleri desteklenir açıklar. 

> [!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
> 
> 

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da kullanılmaya hazır veri al
Machine Learning Studio, ayrılmış veya bazı durumlarda dikdörtgen olmayan veri kullanılabilmesine rağmen bir veritabanındaki verileri yapılandırılmış metin verileri gibi dikdörtgen veya tablo verilerle çalışmak üzere tasarlanmıştır.

Verilerinizi görece temiz ise en iyisidir. Diğer bir deyişle, tırnak işareti olmayan dizeler gibi sorunların denemenize verileri karşıya yüklemeden önce ilgilenebilmek istersiniz.

Ancak, yok modülleri Machine Learning Studio'daki, bazı işleme denemenizi içindeki verilerin etkinleştirin. Bağlı olarak makine öğrenimi algoritmaları kullanıyor olmanız, eksik değerleri ve seyrek veri gibi veri yapısal sorunların nasıl ele alacağız karar vermeniz gerekebilir ve ile yardımcı olabilecek modülleri vardır. Bakılacak yer **veri dönüştürme** bu işlevleri gerçekleştirmek modülleri için modül paletinin bölümü.

Denemenizin herhangi bir noktada görüntüleyebilir veya çıkış bağlantı noktasına tıklayarak modülü tarafından üretilen veri indirin. Modül bağlı olarak farklı indirme seçenekleri kullanılabilir olabilir veya Machine Learning Studio'da web tarayıcınızdan görselleştirmek mümkün olabilir.

## <a name="data-formats-and-data-types-supported"></a>Desteklenen veri biçimleri ve veri türleri
Veri türlerinin sayısı denemenize içeri aktarabilirsiniz, mekanizmaya bağlı olarak veri ve burada bu geldiği içeri aktarın:

* Düz metin (.txt)
* Virgülle ayrılmış değerler (CSV bir başlık (.csv) ile veya olmadan) (. nh.csv)
* Sekmeyle ayrılmış değerler (TSV bir başlık (.tsv) ile veya olmadan) (. nh.tsv)
* Excel dosyası
* Azure tablosu
* Hive tablosu
* SQL veritabanı tablosu
* OData değerleri
* SVMLight veri (.svmlight) (bkz [SVMLight tanımı](http://svmlight.joachims.org/) biçimi bilgileri için)
* Öznitelik ilişkisi dosya biçimi'ne (ARFF) veri (.arff) (bkz [ARFF tanımı](http://weka.wikispaces.com/ARFF) biçimi bilgileri için)
* Zip dosyası (.zip)
* R nesne ya da çalışma dosyası (. RData)

Meta verileri içeren ARFF gibi biçiminde veri içe aktarırsanız, Machine Learning Studio bu meta veriler başlık ve her sütunun veri türünü tanımlamak için kullanır.

Bu meta verileri içermeyen TSV veya CSV biçiminde gibi verileri içe aktarırsanız, Machine Learning Studio verileri örnekleyerek her sütun için veri türü oluşturur. Veri sütun başlıkları de yoksa, Machine Learning Studio varsayılan adlarını sağlar.

Açıkça belirtin veya kullanarak sütun başlıkları ve veri türlerini değiştirme [Düzenle meta veri][edit-metadata].

Aşağıdaki **veri türleri** Machine Learning Studio tarafından tanınan:

* Dize
* Tamsayı
* Çift
* Boole
* DateTime
* TimeSpan

Machine Learning Studio adlı bir iç veri türü kullanan ***veri tablosu*** modülleri arasında veri iletmek için. Veri tablosu biçimi kullanarak, verilerinizi açıkça dönüştürebilirsiniz [veri kümesine Dönüştür] [ convert-to-dataset] modülü.

Veri tablosu dışında biçimlerini kabul eden herhangi bir modül verileri veri tablosuna sessizce sonraki modülüne geçirmeden önce dönüştürür.

Gerekirse, veri tablosu biçime geri CSV, TSV, ARFF veya diğer dönüştürme modülleri kullanarak SVMLight biçimine dönüştürebilirsiniz.
Bakılacak yer **veri formatı dönüştürme** bu işlevleri gerçekleştirmek modülleri için modül paletinin bölümü.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
