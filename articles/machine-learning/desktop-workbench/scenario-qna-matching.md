---
title: Soru- cevap Azure Machine Learning Workbench'i kullanarak bir eşleşen | Microsoft Docs
description: Açık eşleştirmek için çeşitli etkili machine learning yöntemlerini kullanmayı önceden var olan sorguları sona erdi nasıl SSS soru/çiftleri sorularınızın yanıtları.
services: machine-learning
documentationcenter: ''
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 33d95e0c17e8b9b18313cb0854532337ec76cfd1
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42057453"
---
#  <a name="q--a-matching-using-azure-machine-learning-workbench"></a>Soru- cevap bir eşleşen Azure Machine Learning workbench'i kullanma
Açık bir bitiş soruyu yanıtlayarak zordur ve genellikle el ile emeğinin konu uzmanları (SMEs) gerektirir. İç SMEs taleplerini azaltmak için şirketler genellikle kullanıcılara yardım bir yol sık sorulan sorular (SSS) listesi oluşturun. Bu örnekte, SSS soru/yanıt çifti önceden mevcut açık bitişi sorguları eşleştirmek için çeşitli etkin machine learning yöntemleri gösterir. Bu örnekte, Azure Machine Learning Workbench'i kullanarak çözüm oluşturmaya yönelik bir kolayca geliştirme işlemi gösterilmektedir. 

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın
[https://github.com/Azure/MachineLearningSamples-QnAMatching](https://github.com/Azure/MachineLearningSamples-QnAMatching)

## <a name="overview"></a>Genel Bakış

Bu örnek ile ilgili sık sorulan sorular (diğer bir deyişle, bir SSS) listesi veya soru genellikle sağlanan soru ve yanıt (soru- cevap) çifti önceden varolan kullanıcı sorular eşleme sorunu giderir ve Web sitelerinde mevcut bir çiftleri ister [yığını Overflow](https://stackoverflow.com/). En çok benzer sorusuna yanıt bulma gibi doğru yanıtı soru eşleştirmek için birçok yaklaşım vardır. Her yanıt SSS, birden çok anlamsal olarak eşdeğer soruları yanıtlayabilirsiniz varsayarak ancak bu örnekte açık bitişi sorular tarafından daha önce sorulan sorulara eşleştirilir.

Bu çözüm sunmak için gerekli temel adımlar aşağıdaki gibidir:

1. Temiz ve metin verilerini işleyebilirsiniz.
2. Değerinden bağımsız olarak kabul zaman dizisindeki görüntülendiğinde daha fazla bilgi sağlayan birden çok sözcük dizileri bilgilendirici tümcecikleri öğrenin.
3. Özellikleri metin verileri ayıklayın.
4. Metin sınıflandırma modellerini eğitin ve model performansını değerlendirme.


## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmak için gereken önkoşullar aşağıdaki gibidir:

1. Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz denemeler kullanılabilir).
2. Yüklü bir kopyasını [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu](../service/quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturun.
3. Bu örnek, her işlem bağlam üzerinde çalıştırılabilir. Ancak, çalıştırmak için çok çekirdekli makine ile en az önerilir 16GB bellek ve 5GB disk alanı.

## <a name="create-a-new-workbench-project"></a>Workbench yeni bir proje oluşturun

Bu örnekte, şablon olarak kullanarak yeni bir proje oluşturun:
1.  Açık bir Azure Machine Learning Workbench'i
2.  Üzerinde **projeleri** sayfasında **+** açıp seçmek **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri girin
4.  İçinde **proje şablonlarında Ara** arama kutusuna "Q & A eşleşen" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın

## <a name="data-description"></a>Veri açıklaması

Bu örnekte kullanılan veri kümesinin bir yığın Exchange verilerini depolandığı döküm olduğu [archive.org](https://archive.org/details/stackexchange). Bu verileri tüm kullanıcı tarafından katkıda bulunulan içeriğin anonim bir döküm açıktır [Stack Exchange ağ](https://stackexchange.com/). Her sitede ağ üzerinden XML dosyasından oluşan ayrı bir arşiv daraltılmış olarak biçimlendirilmiş 7-zip bzıp2 sıkıştırın. Her site arşiv gönderileri, kullanıcılar, oy, yorumlar, PostHistory ve PostLinks içerir. 

### <a name="data-source"></a>Veri kaynağı

Bu örnekte [gönderir (10 GB) veri](https://archive.org/download/stackexchange/stackoverflow.com-Posts.7z) ve [PostLinks (515 MB) veri](https://archive.org/download/stackexchange/stackoverflow.com-PostLinks.7z) Stack Overflow sitesinde. Tam şeması için bilgi [readme.txt](https://ia800500.us.archive.org/22/items/stackexchange/readme.txt). 

`PostTypeId` Gönderileri veri alanına bir gönderi olup olmadığını gösterir bir `Question` veya `Answer`. `PostLinkTypeId` PostLinks veri alanında iki gönderileri bağlantılı veya yinelenen gösterir. Soru gönderileri, genellikle bazı etiketler, diğer benzer/yinelenen sorularınız soru kategorilere anahtar sözcükler içerir. Bazı etiket vardır ile yüksek frekanslı gibi `javascript`, `java`, `c#`, `php` vs. soru gönderilerin daha fazla sayıda oluşur. Bu örnek, bir alt kümesini Q & bir çiftiyle `javascript` etiketi ayıklanır.

Additionionally, bir soru gönderisi birden çok yanıt gönderileri veya yinelenen soru gönderileri ilgili olabilir. SSS listesinden bu iki veri kümesi oluşturmak için bazı veri toplama ölçütü olarak kabul edilir. Bu örnekte yer almayan bir SQL betiğini kullanarak derlenmiş verilerin üç adet seçildi. Sonuçta elde edilen veri açıklaması aşağıdaki gibidir:

- `Original Questions (Q)`: Soru gönderileri sorular ve Stack Overflow sitesinde yanıt verdi.
- `Duplications (D)`: Soru gönderileri yinelenen önceden mevcut olan diğer sorular (`PostLinkTypeId = 3`), özgün sorular şunlardır. Yinelenen özgün sorusuna verilen yanıt ayrıca yeni yinelenen soruyu yanıtlar algılama özgün sorulara anlamsal olarak eşdeğer olarak kabul edilir.
- `Answers (A)`: Birden fazla yanıt gönderimleri her özgün soru ve kendi yinelenen bağlanabilir. Bu örnek yalnızca özgün yazar tarafından ya da kabul yanıt post seçer (göre filtrelenmiş `AcceptedAnswerId`) veya en yüksek puanı (göre sıralanmış `Score`) özgün soruları içinde. 

Bu üç veri kümeleri birleşimi burada bir yanıt (A) birden çok yinelenen soru (D) ve özgün soru (Q) tarafından aşağıdaki veri diyagramda gösterildiği gibi eşlenmiş soru- cevap çiftlerini oluşturur:

<table><tr><td><img id ="data_diagram" src="media/scenario-qna-matching/data_diagram.png"></td></tr></table>

### <a name="data-structure"></a>Veri yapısı

Doğrudan indirme bağlantıları üç veri kümesi ve veri şeması aşağıdaki tabloda bulunabilir:

| Veri kümesi | Alan | Tür | Açıklama
| ----------|------------|------------|--------
| [Sorular](https://bostondata.blob.core.windows.net/stackoverflow/orig-q.tsv.gz) | Kimlik | Dize | Benzersiz soru kimliği (birincil anahtar)
|  | AnswerId | Dize | Soru başına benzersiz yanıt kimliği
|  | Text0 | Dize | Sorunun başlık ve gövde içeren ham metin verileri
|  | CreationDate | Zaman damgası | Sorunun ne zaman istendi, zaman damgası
| [yinelenenler](https://bostondata.blob.core.windows.net/stackoverflow/dup-q.tsv.gz) | Kimlik | Dize | Benzersiz çoğaltma kimliği (birincil anahtar)
|  | AnswerId | Dize | Çoğaltma ile ilişkili yanıt kimliği
|  | Text0 | Dize | Çoğaltma'nın başlık ve gövde içeren ham metin verileri
|  | CreationDate | Zaman damgası | Çoğaltma zaman istendi, zaman damgası
| [yanıtlar](https://bostondata.blob.core.windows.net/stackoverflow/ans.tsv.gz)  | Kimlik | Dize | Benzersiz yanıt kimliği (birincil anahtar)
|  | text0 | Dize | Ham metin verileri yanıt


## <a name="scenario-structure"></a>Senaryo yapısı

Soru- cevap eşleştirme örnek üç tür dosyası sunulur. İlk tür, iş akışının tamamını adım adım açıklamaları Göster Jupyter Notebook dizisidir. Python dosyaları içeren özel bir Python modüllerini ifade öğrenme ve özellik ayıklama için ikinci türüdür. Bu örnek aynı zamanda diğer kullanım örnekleri yalnızca hizmet için genel bu Python modüllerdir. Hyper-parametreleriyle ayarlama ve Azure Machine Learning Workbench'i kullanarak model performansını izlemek için Python dosyaları üçüncü türüdür.

Bu örnekte dosyalar gibi düzenlenir.

| Dosya Adı | Tür | Açıklama
| ----------|------------|--------
| `Image` | Klasör | Görüntüler için Benioku dosyasını kaydetmek için kullanılan klasörü
| `notebooks` | Klasör | Jupyter not defterleri klasörü
| `modules` | Klasör | Python modüllerini klasörü
| `scripts` | Klasör | Python dosyaları klasörü
| `notebooks/Part_1_Data_Preparation.ipynb` | Jupyter Notebook | Örnek verilere erişmek, metin ön işlemden ve eğitim hazırlama ve test veri kümeleri
| `notebooks/Part_2_Phrase_Learning.ipynb` | Jupyter Notebook | Bilgilendirici tümcecikleri öğrenin ve metin sözcükleri paketi (BOWs) gösterimlerini simgeleştirin
| `notebooks/Part_3_Model_Training_and_Evaluation.ipynb` | Jupyter Notebook | Özellikleri ayıklayın, model performansını değerlendirme ve metin sınıflandırma modellerini eğitin
| `modules/__init__.py` | Soubor Pythonu | Python paket init dosyası
| `modules/phrase_learning.py` | Soubor Pythonu | Ham verileri dönüştürün ve bilgilendirici tümcecikleri öğrenmek için kullanılan Python modüllerini
| `modules/feature_extractor.py` | Soubor Pythonu | Python modüllerini modeli eğitimi için özellikler ayıklayın
| `scripts/naive_bayes.py` | Soubor Pythonu | Hyper-parametreleriyle Naive Bayes modelinde ayarlamak için Python
| `scripts/random_forest.py` | Soubor Pythonu | Hyper-parametreleriyle rastgele orman modeli ayarlamak için Python
| `README.md` | Markdown dosyası | Benioku markdown dosyası

> [!NOTE]
> Not defterlerini dizi Python 3.5 altında oluşturulur.
> 

### <a name="data-ingestion-and-transformation"></a>Veri alımı ve dönüştürme

Üç derlenmiş veri kümeleri, Blob Depolama alanında depolanır ve alınır `Part_1_Data_Preparation.ipynb` dizüstü bilgisayar.  

Metin sınıflandırma modellerini eğitim önce soruları metinde temizlenir ve kod parçacıkları hariç tutmak için önceden işlenmiş. Bir ifade Denetimsiz öğrenme bilgilendirici birden çok sözcük dizileri öğrenmek için eğitim malzemesi uygulanır. Bu ifadeler, metin sınıflandırma modellerini tarafından kullanılan akış sözcükleri paketi (BOWs) özellik kazandırma sayesinde biriminde tek bir bileşik sözcük olarak temsil edilir.

Ayrıntılı adım adım açıklamaları metin ön işleme ve tümcecik öğrenme not defterlerinde bulunabilir `Part_1_Data_Preparation.ipynb` ve `Part_2_Phrase_Learning.ipynb`sırasıyla.

### <a name="modeling"></a>Modelleme

Bu örnek, yeni sorular karşı önceden mevcut olan soru- cevap çiftlerini bir eğitim metin sınıflandırma modellerini burada her önceden mevcut olan soru- cevap çifti benzersiz bir sınıf, yinelenen soruların soru- cevap her çifti için bir alt kümesi olarak kullanılabilir puanlamak için tasarlanmıştır eğitim malzemesi. Örnekte, özgün sorular ve yinelenen soruların %75 eğitim korunur ve en yakın zamanda gönderilen % 25'ini yinelenen sorular tutulan Değerlendirme verileri genişletme.

Sınıflandırma modeli de dahil olmak üzere üç temel sınıflandırıcılar toplamak için bir topluluğu yöntemini kullanır. **Naive Bayes**, **destekli vektör makinesi**, ve **rastgele orman**. Her bir temel sınıflandırıcı içinde `AnswerId` sınıfı etiket ve BOWs gösterimleri kullanılan gibi özellikleri kullanılır.

Model eğitimi işlem gösterilmiştir `Part_3_Model_Training_and_Evaluation.ipynb`.

### <a name="evaluation"></a>Değerlendirme

İki farklı değerlendirme ölçümleri, performansı değerlendirmek için kullanılır. 
1. `Average Rank (AR)`: ortalama (dışı 103 yanıt sınıfları tam kümesini) alınan soru- cevap çiftlerini listesinde doğru yanıtı bulunduğu konumu belirtir. 
2. `Top 3 Percentage`: test yüzdesi sorularını doğru yanıtı döndürülen ilk üç seçenek listesi sıralanmış alınabilir olduğunu gösterir. 

Değerlendirme gösterilmiştir `Part_3_Model_Training_and_Evaluation.ipynb`.


## <a name="conclusion"></a>Sonuç

Bu örnek iyi bilinen metin analizi teknikleri, tümcecik öğrenme ve metin sınıflandırma gibi güçlü bir modeli oluşturmak için nasıl kullanılacağını vurgular. Ayrıca, Azure Machine Learning Workbench ile etkileşimli çözüm geliştirme ve izleme model performansını nasıl yardımcı olabileceğini gösterir. 

Bu örnekte anahtar bazı önemli noktalar şunlardır:

- Etkili bir şekilde ifade öğrenme ve metin sınıflandırma modellerini ile soru ve yanıt eşleşen sorun çözülebilir.
- Azure Machine Learning Workbench ve Jupyter not defteri ile etkileşimli model geliştirme gösterimi.
- Azure Machine Learning Workbench'i çalıştırma geçmişi ve karşılaştırma amaçları için değerlendirme ölçümlerin günlük öğrenilen modellerini yönetir. Bu özellikler, hızlı hyper parametresi ayarlama sağlar ve en iyi performansa sahip Modellerinizi belirlemeye yardımcı olur.


## <a name="references"></a>Başvurular

Timothy J. Hazen, Fred Uludağ [ _geliştirilmiş damıtarak konuşma bağlamında kullanılabilen konuşma konu modelleme için kısıtlanmış bir ifade ağacı Multiword tümcecikleri modelleme_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Konuşulan dili teknoloji Workshop (SLT) 2012 IEEE. IEEE, 2012.

Timothy J. Hazen [ _MCE eğitim teknikleri konuşulan ses belgelerin konu tanımlayıcısı için_ ](http://ieeexplore.ieee.org/abstract/document/5742980/) IEEE işlemlerde, ses, konuşma ve dil, 19 Transactions Hayır işleme. 8, ss. 2451-2460, Kasım 2011.
