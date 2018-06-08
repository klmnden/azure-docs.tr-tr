---
title: Yapı projeleri takım veri bilimi işlem şablonu ile | Microsoft Docs
description: Azure Machine Learning'de, işbirliği için yapı projeleri takım veri bilimi işlem (TDSP) şablonlarında örneği oluşturmak nasıl
services: machine-learning
documentationcenter: ''
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: bradsev
ms.openlocfilehash: 5b53bd3ec479ba6e096b4d00089f968e37f0135c
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831777"
---
# <a name="structure-projects-with-the-team-data-science-process-template"></a>Takım veri bilimi işlem şablonu ile yapısı projeleri

Bu belge, takım veri bilimi işlem (TDSP) şablonları ile Azure Machine Learning ile veri bilimi projeleri oluşturma hakkında yönergeler sağlar. Bu şablonlar yapısı projelerde işbirliği ve yeniden Üretilebilirlik yardımcı olur. 


## <a name="what-is-the-team-data-science-process"></a>Team Data Science Process nedir?
TDSP yürütme ve Gelişmiş analiz çözümleri teslim etmek için bir Çevik, yinelemeli, veri bilimi işlemidir. İşbirliği ve büyük kuruluşlar, veri bilimi ekipleri verimliliğini artırmak için tasarlanmıştır. Dört anahtar bileşenleri ile bu amaçları destekler:

   * Standart bir [veri bilimi yaşam döngüsü](../team-data-science-process/lifecycle.md) tanımı.
   * Standartlaştırılmış proje yapısını [Proje belgelerine ve raporlama şablonları](https://github.com/Azure/Azure-TDSP-ProjectTemplate).
   * Bir altyapı ve sırasıyla bir işlem ve depolama altyapısı ve kod depoları gibi proje yürütme için kaynaklar.
   * [Araçlar ve yardımcı programlar](https://github.com/Azure/Azure-TDSP-Utilities) veri bilimi için proje görevleri, örneğin:
      - İşbirlikçi sürüm denetimi
      - Kod gözden geçirme
      - Veri keşfi ve modelleme
      - İş planlama

TDSP daha kapsamlı bir açıklama için bkz: [takım veri bilimi işlemine genel bakış](../team-data-science-process/overview.md).

## <a name="why-should-you-use-the-tdsp-structure-and-templates"></a>Neden TDSP yapısı ve şablonları kullanmalısınız?
Standartlaştırma yapısı, yaşam döngüsü ve veri bilimi projeleri belgelerine, veri bilimi ekipleri üzerinde etkili işbirliği kolaylaştırmanın için anahtardır. Machine learning projeleri Eşgüdümlü ekip çalışması için bu tür bir çerçeve sağlamak üzere TDSP şablonu oluşturun.

Daha önce yayımladık bir [TDSP proje yapısını ve şablonları için GitHub depo](https://github.com/Azure/Azure-TDSP-ProjectTemplate) bu hedeflerine ulaşmasına yardımcı olacak. Ancak, veri bilimi araç içinde şablonları ve TDSP yapısı örneği oluşturmak için şimdiye kadar mümkün değildi. Şimdi TDSP yapısı ve belgeleri şablonları başlatır bir machine learning projesi oluşturmak mümkündür. 

## <a name="things-to-note-before-creating-a-new-project"></a>Yeni bir proje oluşturmadan önce dikkat edilecek noktalar
Aşağıdaki öğeleri inceleyin *önce* yeni bir proje oluşturun:
* TDSP Machine Learning gözden [şablon](https://aka.ms/tdspamlgithubrepo).
* (Dışında ne "docs" klasöründe zaten) içeriği az 25 MB boyutunda olması gerekir. Bu liste nota bakın.
* Örnek\_veri klasörüdür ile kodunuzu test etmek veya erken geliştirme başlatmak yalnızca küçük veri dosyaları için (5 MB'den az).
* Word gibi dosyaları ve PowerPoint dosyaları depolamak, "docs" klasörünün boyutunu önemli ölçüde artırabilir. Biz, öneri işbirliğine dayalı bir Wiki bulmanızı [SharePoint](https://products.office.com/en-us/sharepoint/collaboration), ya da bu tür dosyaları depolamak için diğer ortak kaynak.
* Büyük dosyaları ve Machine Learning çıktılarında nasıl ele alınacağını öğrenmek için [değişiklikleri devam ettirmeden ve büyük dosyalarla ilgili](http://aka.ms/aml-largefiles).

> [!NOTE]
> Tüm belgeleri ile ilgili içerik (metin, markdowns, resimleri ve diğer belge dosyaları) *değil* proje yürütme sırasında kullanılan, diğer readme.md dosyasından bulunmalıdır "Belgeler" (tümü küçük harf) adlı klasörde. "Belgeler" klasörü, Machine Learning yürütme tarafından yoksayılarak hedefleri gereksiz yere hesaplamak için bu klasörün içeriğini kopyalanan olmayan özel bir klasördür. Bu klasördeki nesnelerin de 25 MB cap proje boyutu için doğru sayılmaz. "Belgeler" klasörü, örneğin, belgelerinizde gereken büyük görüntü dosyaları depolamak için yerdir. Bu dosyalar hala çalıştırma geçmişi Git tarafından izlenir. 

## <a name="instantiate-the-tdsp-structure-and-templates-from-the-machine-learning-template-gallery"></a>Machine Learning Şablon Galerisi şablonlardan ve TDSP yapısı örneği
TDSP yapısı ve belgeleri şablonları ile yeni bir proje oluşturmak için aşağıdaki yordamları tamamlayın.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma
Yeni bir proje oluşturmak için Azure Machine Learning açın. Altında **projeleri** sol bölmesinde, artı işaretini seçin (**+**) ve ardından **yeni proje**.

![Yeni proje](./media/how-to-use-tdsp-in-azure-ml/instantiation-1.png)


### <a name="create-a-new-tdsp-structured-project"></a>TDSP yapılandırılmış yeni bir proje oluşturun
   1. Parametreler ve bilgileri ilgili kutunun veya listesini belirtin:

      - Proje adı
      - Proje dizini
      - Proje açıklaması
      - Boş bir Git deposu yol
      - Çalışma alanı adı

   2. Ardından **arama** kutusuna **TDSP**. 
   3. Zaman **TDSP sahip bir proje yapısı** seçeneği görünür, bu şablonu seçin. 
   4. Seçin **oluşturma** yeni projeniz TDSP yapısıyla oluşturma düğmesi. Proje (uygun metin kutusuna) oluşturduğunuzda, boş bir Git deposu sağlarsanız, ardından bu depoyu Proje yapısı ve içeriği ile Proje oluşturulduktan sonra dolduracaksınız.

![TDSP projesi oluşturma](./media/how-to-use-tdsp-in-azure-ml/instantiation-2.png)


## <a name="examine-the-tdsp-project-structure"></a>TDSP proje yapısını inceleyin
Yeni projeniz oluşturulduktan sonra yapısını inceleyebilirsiniz (aşağıdaki şekilde sol panelinde bakın). İş anlamak için standartlaştırılmış belgelerine tüm yönlerini içerir. Bu öğeler TDSP yaşam döngüsü, veri konumu, tanımları ve bu belge şablonu mimarisi aşamaları içerir. 

Gösterilen yapı yayınlanan TDSP yapısı türetilir [TDSP Proje yapısı, belgeler ve yapı şablonları](https://github.com/Azure/Azure-TDSP-ProjectTemplate), bazı değişiklikler ile. Örneğin, birkaç belge şablonları bir markdown, yani, birleştirilir [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport). 

### <a name="project-folder-structure"></a>Proje klasör yapısı
TDSP proje şablonu, aşağıdaki üst düzey klasör içerir:
   - **kod**: kodunu içerir.
   - **belgeleri**: Proje (örneğin, markdown dosyaları ve ilgili medya) hakkında gerekli belgeleri içerir.
   - **sample_data**: içeren **(küçük) örnek** erken geliştirme veya test için kullanabileceğiniz veri. Genellikle, bu ayarlar birçok (5) büyük olmayan MB. Bu klasör için tam veya büyük veri kümesi değil.

![Örnek veriler](./media/how-to-use-tdsp-in-azure-ml/instantiation-3.png)


## <a name="use-the-tdsp-structure-and-templates"></a>TDSP yapısı ve şablonları'nı kullanın
Proje özgü bilgileri yapısı ve şablonları eklemeniz gerekir. Bu kodu ve yürütün ve projenizin teslim etmek gerekli bilgileri içeren doldurmak için beklenen. [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) projenize ilgili bilgilerle değiştirmek için gereken bir şablon bir dosyadır. Bir yardımcı soruları kümesiyle birlikte gelen her dört aşaması bilgileri doldurun [TDSP yaşam döngüsü](../team-data-science-process/lifecycle.md).

Proje yapısı nasıl yürütme sırasında veya tamamlama aşağıdaki şekilde sol panelinde gösterilen sonra göründüğünü örneği. Bu proje arasındadır [takım veri bilimi işlemi örnek proje: Azure Machine learning'de ABD census verilerden yüksek sınıflandırmak](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) örnek proje.

![Örnek Proje yapısı](./media/how-to-use-tdsp-in-azure-ml/instantiation-4.png)

## <a name="document-your-project"></a>Projenizi belge
Başvurmak [TDSP belgelerine şablonları](https://github.com/Azure/Azure-TDSP-ProjectTemplate) projenizi belge hakkında bilgi için. Geçerli makine öğrenme TDSP belgelerine şablonunda tüm bilgileri dahil olan öneririz [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) dosya. Bu şablon projenize özgü bilgiler ile doldurulması. 

Ayrıca sağladığımız bir [ProjectLearnings](https://aka.ms/tdspamlgithubrepoprojectlearnings) şablonu. Bu şablon, birincil proje belgede bulunmaz, ancak belge hala yararlı bilgileri eklemek için kullanabilirsiniz. 

### <a name="example-project-report"></a>Örnek Proje raporu
Alabileceğiniz bir [örnek proje raporu](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md). Bu proje rapor için [ABD gelir sınıflandırma örnek proje](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) örneği ve TDSP şablonu için bir veri bilimi projesini nasıl kullanılacağını gösterir.

## <a name="next-steps"></a>Sonraki adımlar
Machine Learning projelerinde TDSP yapısı ve şablonları kullanma konusunda anlama kolaylaştırmak için Machine Learning belgelerindeki birkaç projeyi örnekleri sunuyoruz:

- Machine Learning TDSP proje oluşturmayı gösteren bir örnek için bkz: [takım veri bilimi işlemi örnek proje: Azure Machine Learning BİZE Census verilerden yüksek sınıflandırmak](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome).
- Derin öğrenme doğal dil projede TDSP örneği Machine Learning (NLP) işleme kullanır bir örnek için bkz: [biyografisi TIP varlık tanıma ile derin öğrenme işleme doğal dili kullanarak](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction).

