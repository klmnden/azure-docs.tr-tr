---
title: Team Data Science Process şablon projeleriyle yapısı | Microsoft Docs
description: Team Data Science işlem (TDSP) şablonları Azure Machine learning'de, işbirliği için yapısını projeleri oluşturmak nasıl
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 05cb2a62cf0f001012f5faa022de233d7cbdce97
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42055856"
---
# <a name="structure-projects-with-the-team-data-science-process-template"></a>Team Data Science Process şablonu ile projeleri yapılandırma

Bu belge, veri bilimi projeleri Azure Machine Learning ile Team Data Science işlem (TDSP) şablonları oluşturma hakkında yönergeler açıklanmaktadır. Bu şablonlar, işbirliği ve yeniden üretilebilirliğini yapısı projelerine yardımcı olur. 


## <a name="what-is-the-team-data-science-process"></a>Team Data Science Process nedir?
TDSP yürütme ile Gelişmiş analiz çözümleri sunmak için bir Çevik, yinelemeli, veri bilimi işlemidir. İşbirliği ve kurumsal kuruluşlar, veri bilimi ekipleri verimliliğini artırmak için tasarlanmıştır. Bu, bu hedefleri ile başlıca dört bileşenden destekler:

   * Standart [veri bilimi yaşam döngüsünü](../team-data-science-process/lifecycle.md) tanımı.
   * Bir standart Proje yapısı [proje belgeleri ve şablonları raporlama](https://github.com/Azure/Azure-TDSP-ProjectTemplate).
   * Bir altyapı ve gibi sırasıyla bir işlem ve depolama altyapısı ve kod depoları, proje yürütme için kaynaklar.
   * [Araçlar ve yardımcı programlar](https://github.com/Azure/Azure-TDSP-Utilities) için veri bilimi görevleri gibi proje:
      - İşbirliğine dayalı sürüm denetimi
      - Kod gözden geçirme
      - Veri keşfi ve modelleme
      - İş planlama

TDSP daha eksiksiz bir açıklaması için [Team Data Science Process genel bakış](../team-data-science-process/overview.md).

## <a name="why-should-you-use-the-tdsp-structure-and-templates"></a>TDSP yapısı ve şablonları neden kullanmalısınız?
Yapısı, yaşam döngüsü ve veri bilimi projeleri belgelerine Standartlaştırma, veri bilimi ekipleri üzerinde etkili işbirliği etkinleştirme için anahtardır. TDSP şablonunda Eşgüdümlü ekip çalışması için bu tür bir çerçeve sağlamak için Machine learning projeleri oluşturun.

Daha önce yayımlanmış bir [TDSP Proje yapısı ve şablonlar için GitHub deposu](https://github.com/Azure/Azure-TDSP-ProjectTemplate) bu hedeflerine ulaşmaya yardımcı olmak için. Ancak, bir veri bilimi aracını şablonlarında ve TDSP yapısı örneği oluşturmak için şimdiye kadar mümkün değildi. Artık TDSP yapısı ve belgeler şablonları örnekleyen bir machine learning projesi oluşturmak mümkündür. 

## <a name="things-to-note-before-creating-a-new-project"></a>Yeni bir proje oluşturmadan önce dikkat edilecek noktalar
Aşağıdaki öğeleri inceleyin *önce* yeni bir proje oluşturun:
* TDSP Machine Learning gözden [şablon](https://aka.ms/tdspamlgithubrepo).
* ("Belgeler" klasöründe mevcut olduğunu) dışındaki içeriği boyutu 25 MB'tan küçük olması gerekir. Bu liste aşağıdaki nota bakın.
* Örnek\_veri klasördür ile kodunuzu test veya geliştirme başlatmak yalnızca küçük veri dosyaları için (5 MB'tan az).
* Word gibi dosyalar ve PowerPoint dosyaları depolamak, "Belgeler" klasörünün boyutunu önemli ölçüde artırabilir. Biz, öneri, işbirliğine dayalı bir Wiki bulmanızı [SharePoint](https://products.office.com/en-us/sharepoint/collaboration), veya bu tür dosyaları depolamak için işbirliğine dayalı diğer kaynak.
* Büyük dosyaları ve çıkış Machine learning'de nasıl ele alınacağını öğrenmek için [değişiklikleri kalıcı hale getirmeniz ve büyük dosyalarla ilgili](http://aka.ms/aml-largefiles).

> [!NOTE]
> Tüm belgeleri ile ilgili içerik (metin, markdowns, resimler ve diğer belge dosyaları) *değil* proje yürütme sırasında kullanılan Benioku.MD dosyası dışında bulunmalıdır "Belgeler" (tamamı küçük harf) adlı bir klasörde. "Belgeler" klasörü, böylece bu klasördeki içeriği hedefleri gereksiz yere işlem kopyalanmasını yoksa Machine Learning yürütme tarafından göz ardı özel bir klasördür. Bu klasördeki nesneleri ayrıca 25 MB cap proje boyutu için doğru sayılmaz. "Belgeler" klasöründe, örneğin, belgelerinizde gereken büyük görüntü dosyaları depolamak için yerdir. Bu dosyalar, hala çalıştırma geçmişini Git tarafından izlenir. 

## <a name="instantiate-the-tdsp-structure-and-templates-from-the-machine-learning-template-gallery"></a>TDSP yapısı ve Machine Learning Şablon Galerisi şablonlardan örneği
TDSP yapısını ve belgelerini şablonları ile yeni bir proje oluşturmak için aşağıdaki yordamları tamamlayın.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma
Yeni bir proje oluşturmak için Azure Machine Learning açın. Altında **projeleri** üst sol bölmesinde, artı işaretini seçin (**+**) ve ardından **yeni proje**.

![Yeni proje](./media/how-to-use-tdsp-in-azure-ml/instantiation-1.png)


### <a name="create-a-new-tdsp-structured-project"></a>TDSP yapılandırılmış yeni bir proje oluşturun
   1. Parametreler ve bilgileri, ilgili kutunun veya listedeki belirtin:

      - Proje adı
      - Proje dizini
      - Proje açıklaması
      - Boş bir Git depo yolu
      - Çalışma alanı adı

   2. Ardından **arama** kutusuna **TDSP**. 
   3. Zaman **bir TDSP ile proje** seçeneği görüntülenirse o şablonu seçin. 
   4. Seçin **Oluştur** TDSP yapısıyla yeni projenizi oluşturmak için düğme. (Uygun metin kutusundaki) proje oluşturduğunuzda, boş bir Git deposu sağlarsanız, ardından bu depoyu Proje yapısı ve içeriği ile Proje oluşturulduktan sonra doldurur.

![TDSP projesi oluşturma](./media/how-to-use-tdsp-in-azure-ml/instantiation-2.png)


## <a name="examine-the-tdsp-project-structure"></a>TDSP proje yapısını inceleyin
Yeni projeniz oluşturulduktan sonra yapısını inceleyin (sol bölmede bulunan aşağıdaki şekilde bakın). Bu iş anlamak için standartlaştırılmış belgeleri tüm özelliklerini içerir. Bu öğeler TDSP yaşam döngüsü, veri konumu, tanımlarını ve bu belgeleri şablon mimarisini aşamaları içerir. 

Yayınlanan TDSP yapısı gösterilmektedir yapı türetilir [TDSP Proje yapısı, belge ve yapı şablonları](https://github.com/Azure/Azure-TDSP-ProjectTemplate), bazı değişiklikler ile. Örneğin, birkaç belge şablonları bir markdown, yani birleştirilir [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport). 

### <a name="project-folder-structure"></a>Proje klasör yapısı
TDSP proje şablonu, aşağıdaki üst düzey klasör içerir:
   - **kod**: kodunu içerir.
   - **docs**: Proje (örneğin, markdown dosyaları ve ilgili medya) hakkında gerekli belgelerini içerir.
   - **sample_data**: içeren **(küçük) örnek** erken geliştirme veya test amaçlı kullanabileceğiniz veriler. Genellikle, bu ayarlar birçok (5) büyük olmayan MB. Bu klasör için tam ya da büyük veri kümelerini değil.

![Örnek veriler](./media/how-to-use-tdsp-in-azure-ml/instantiation-3.png)


## <a name="use-the-tdsp-structure-and-templates"></a>TDSP yapısı ve şablonları
Projeye özgü bilgileri ekleyin yapısı ve şablonları gerekir. Bu kod ve yürütün ve projenize sunmak için gereken bilgileri ile doldurmak için beklenen. [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) dosyasıdır şablondan projenizle ilgili bilgilerle değiştirmeniz gerekir. Yardımcı sorular bir dizi gelen her dört aşaması için bilgileri doldurun [TDSP yaşam döngüsü](../team-data-science-process/lifecycle.md).

Yürütme sırasında veya sonra tamamlama, sol bölmede bulunan aşağıdaki şekilde gösterildiği gibi bir proje yapısı örneği arar. Bu proje dandır [Team Data Science Process kodunuzla: Azure Machine learning'de ABD görselleştirmenizdeki verilerden içi gelirleri sınıflandırma](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) örnek proje.

![Örnek Proje yapısı](./media/how-to-use-tdsp-in-azure-ml/instantiation-4.png)

## <a name="document-your-project"></a>Projenizi belge
Başvurmak [TDSP belgeleri şablonları](https://github.com/Azure/Azure-TDSP-ProjectTemplate) projenizi belge hakkında bilgi için. Geçerli makine öğrenimi TDSP belgeleri şablonda, tüm bilgileri dahil olan öneririz [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) dosya. Bu şablon, projenize özel bilgiler ile doldurulmalıdır. 

Ayrıca, bildirimde bir [ProjectLearnings](https://aka.ms/tdspamlgithubrepoprojectlearnings) şablonu. Projenin birincil belgede bulunmamaktadır, ancak belge rağmen kullanışlı olan bilgileri eklemek için bu şablonu kullanabilirsiniz. 

### <a name="example-project-report"></a>Örnek Proje raporu
Alabileceğiniz bir [örnek proje raporu](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md). Bu Proje raporu için [ABD gelir sınıflandırma kodunuzla](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) TDSP şablon için bir veri bilimi proje oluşturulacağı ve kullanılacağı gösterilmektedir.

## <a name="next-steps"></a>Sonraki adımlar
Machine Learning projelerinde TDSP yapısı ve şablonlar kullanma konusunda Anlayışınızı kolaylaştırmak için Machine Learning belgeleri birçok projeyi örneklerde sağlıyoruz:

- Machine Learning'de TDSP projesi oluşturma işlemini gösteren bir örnek için bkz: [Team Data Science Process kodunuzla: Azure Machine learning'de BİZE Görselleştirmenizdeki verilerden içi gelirleri sınıflandırma](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome).
- Doğal dil TDSP örneği projede Machine Learning (NLP) işleme, derin öğrenme kullanan bir örnek için bkz. [kullanarak doğal dil işleme ile ayrıntılı öğrenme biyografi TIP varlık tanıma](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction).

