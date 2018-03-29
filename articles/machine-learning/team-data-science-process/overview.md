---
title: Team Data Science Process nedir? | Microsoft Docs
description: Tahmine dayalı analiz çözümleri ve akıllı uygulamaları için bir veri bilimi yöntemler sağlar.
services: machine-learning
documentationcenter: ''
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: b1f677bb-eef5-4acb-9b3b-8a5819fb0e78
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: bradsev
ms.openlocfilehash: f7c081dcd74164f4b1f054f5a65f2ff6aaabebd7
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="what-is-the-team-data-science-process"></a>Team Data Science Process nedir?

Takım veri bilimi işlem (TDSP) Tahmine dayalı analiz çözümleri ve akıllı uygulamalar verimli bir şekilde teslim etmek için Çevik, yinelemeli veri bilimi Metodoloji ' dir. TDSP, takım işbirliğinin ve öğrenimin geliştirilmesine yardımcı olur. Veri bilimi girişimlerini başarıyla uygulamayı kolaylaştıran, Microsoft’un ve sektördeki diğer kuruluşların en iyi deneyimlerinin ve yapılarının bileşimini içerir. Hedef, şirketlerin kendi analiz programlarının avantajlarını tam olarak hayata geçirmesine yardımcı olmaktır.

Bu makalede TDSP ve ana bileşenlerini genel bir bakış sağlar. Çeşitli araçları ile genel açıklamasını buraya işleminin uygulanabilir sunuyoruz. Ek bağlantılı konulardaki proje görevleri ve rolleri işlem yaşam döngüsündeki ilgili daha ayrıntılı bir açıklaması sağlanır. Microsoft araçları ve TDSP bizim ekiplerde uygulamak için kullanırız altyapısı belirli bir dizi kullanılarak TDSP gerçekleştirme hakkında yönergeler de sağlanır.

## <a name="key-components-of-the-tdsp"></a>TDSP anahtar bileşenleri

TDSP aşağıdaki anahtar bileşenlerden oluşur:

- A **veri bilimi yaşam döngüsü** tanımı
- A **standartlaştırılmış Proje yapısı**
- **Altyapı ve kaynakları** veri bilimi projeleri için
- **Araçlar ve yardımcı programlar** proje yürütme için


## <a name="data-science-lifecycle"></a>Veri bilimi yaşam döngüsü

Takım veri bilimi işlem (TDSP) veri bilimi projelerinizi geliştirme yapısı için bir yaşam döngüsü sağlar. Yaşam döngüsü başlangıçtan bitişe kadar bunlar çalıştırıldığında projeleri genellikle izlemeniz gereken adımları özetler.

Başka bir veri bilimi yaşam döngüsü, gibi kullanıyorsanız [NET-DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) veya, kuruluşun kendi özel işlem görev tabanlı TDSP bu geliştirme yaşam döngüleri bağlamında kullanmaya devam edebilirsiniz. Yüksek düzeyde, bu farklı yöntemlerini çok ortaktır. 

Bu yaşam döngüsü, akıllı uygulamaların bir parçası gönderilen veri bilimi projeleri için tasarlanmıştır. Bu uygulamalar, Tahmine dayalı analiz için makine öğrenme veya yapay zeka modelleri dağıtın. Ayrıca keşif veri bilimi projeleri veya geçici analytics projeleri bu işlemi kullanmak yararlı olabilir. Ancak bu gibi durumlarda açıklanan adımları bazıları gerekli.    

Yaşam döngüsü projeleri genellikle genellikle tekrarlayarak yürütme, önemli aşamaları ana hatlarıyla gösterilir:

* **İş anlama**
* **Veri alımı ve anlama**
* **Modelleme**
* **Dağıtım**
* **Müşteri kabulü**

Görsel gösterimi işte **takım veri bilimi işlemi yaşam döngüsü**. 

![TDSP Lifecycle2](./media/overview/tdsp-lifecycle2.png) 

Hedefler, görevler ve belgeleri yapıları TDSP çevriminin her aşaması için açıklanan [takım veri bilimi işlemi yaşam döngüsü](lifecycle.md) konu. Bu görevler ve yapıları proje rolleri ile ilişkilidir:

- Çözümü Mimarı
- Proje Yöneticisi
- Veri uzmanı
- Proje lideri 

Aşağıdaki diyagramda görevlerinde (mavi) ve bu rolleri (dikey eksende) yaşam döngüsü (yatay eksende) her aşaması ile ilişkili yapıları (yeşil içinde) bir kılavuz görünümünü sağlar. 

![TDSP-roller-ve-görevleri](./media/overview/tdsp-tasks-by-roles.png)

## <a name="standardized-project-structure"></a>Standartlaştırılmış Proje yapısı

Bir dizin yapısına paylaşabilir ve şablonları proje belgelerini kullanın. tüm projeleri sahip ekip üyelerinin kendi projelerine hakkında bilgi bulmak kolaylaştırır. Tüm kodu ve belgeler gibi Git, TFS veya alt sürüme takım işbirliği etkinleştirmek için sürüm denetimi sistemini (VC) depolanır. İzleme görevleri ve izleme sistemi Jira gibi bir Çevik proje özelliklerinde, yarışı, Visual Studio Team Services sağlayan ayrı özellikler için kodun yakın izleme. Böyle izleme daha iyi tahminler maliyet almak ekipler de sağlar. Sürüm oluşturma, bilgi güvenliği ve işbirliği için VC her proje için ayrı bir havuz oluşturma TDSP önerir. Tüm projeleri için standartlaştırılmış yapısı kuruluş genelinde kurumsal bilgi yapı yardımcı olur.

Klasör yapısı ve standart konumlarda gerekli belgeler için şablonlar sağlar. Bu klasör yapısını veri keşfi ve özelliği ayıklama kodunu içeren ve bu modeli yineleme kayıt dosyaları düzenler. Bu şablonlar, takım üyeleri başkaları tarafından yapılan iş anlamak ve takımlara yeni üye eklemek için kolaylaştırır. Görüntüleyin ve markdown biçiminde belge şablonlarını güncelleştirmek kolaydır. Anahtar sorular sorun iyi tanımlanmış olduğunu ve sonuçlara beklenen kalite karşıladığını güvence altına almaya her proje için denetim listeleri sağlamak için şablon kullanın. Örneklere şunlar dahildir:

- projenin kapsamını ve iş sorununu belgelemek için bir proje kurucu
- Belge yapısı ve ham verileri istatistikleri verileri raporları
- belge türetilmiş özellikleri için model raporları
- Model performans ölçümleri ROC Eğriler veya MSE gibi


![TDSP dizinleri](./media/overview/tdsp-dir-structure.png)

Dizin yapısı gelen kopyalanıp [Github](https://github.com/Azure/Azure-TDSP-ProjectTemplate).

## <a name="infrastructure-and-resources-for-data-science-projects"></a>Altyapı ve veri bilimi projeleri için kaynakları  

TDSP paylaşılan analizi ve depolama altyapısı gibi yönetme ile ilgili öneriler sağlar:

- veri kümelerini depolamak için bulut dosya sistemleri 
- veritabanları
- büyük veri kümeleri (Hadoop veya Spark) 
- Machine learning Hizmetleri. 

Analytics ve depolama altyapısı Bulut veya şirket içi olabilir. Ham ve işlenen veri kümeleri depolandığı budur. Bu Altyapı yeniden üretilebilir çözümleme sağlar. Ayrıca, tutarsızlıklar ve gereksiz altyapı maliyetlerini artırabilir çoğaltma önler. Araçlar, paylaşılan kaynakları sağlamak, bunları izlemek ve bu kaynaklara güvenli bir şekilde bağlanmak her ekip üyesi izin vermek için sağlanır. Ayrıca, iyi bir uygulama tutarlı bilgi işlem ortamı oluşturmak proje üyeleri sahip olabilir. Farklı ekip üyelerinin çoğaltabilir ve denemeler doğrulayın.

Birden fazla proje üzerinde çalıştığı ve çeşitli bulut analytics altyapı bileşenleri paylaşma bir ekibin bir örneği burada verilmiştir.

![TDSP altyapısı](./media/overview/tdsp-analytics-infra.png)


## <a name="tools-and-utilities-for-project-execution"></a>Araçlar ve yardımcı programlar proje yürütme için

Çoğu kuruluş işlemlerde Tanıtımı zordur. Veri bilimi işlemi ve yaşam döngüsü Yardım düşük önündeki engelleri uygulamak ve kendi benimseme tutarlılığını artırmak için sağlanan araçları. TDSP ilk birtakım Araçlar ve TDSP benimseme ekip içinde hızla başlatmak için komut dosyaları sağlar. Ayrıca, bazı veri bilimi yaşam döngüsü veri keşfi ve temel modelleme gibi genel görevleri otomatikleştirmek yardımcı olur. Paylaşılan Araçlar ve yardımcı programlar kendi ekibin paylaşılan kod havuzunda katkıda bulunan kişiler için sağlanan iyi tanımlanmış bir yapı yoktur. Bu kaynaklar, ekip veya kuruluş içindeki diğer projeler tarafından sonra yararlanılabilir. TDSP Araçlar ve yardımcı programlarına tamamını topluluk katkılarına etkinleştirmek de planlar. TDSP yardımcı programları gelen kopyalanıp [Github](https://github.com/Azure/Azure-TDSP-Utilities).


## <a name="next-steps"></a>Sonraki adımlar

[Takım veri bilimi işlemi: Roller ve görevler](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/roles-tasks.md) anahtar personel rolleri ve bu işlemi standartlaştıran veri bilimi ekibin ilişkilendirilen görevlerinin özetlenmektedir. 
