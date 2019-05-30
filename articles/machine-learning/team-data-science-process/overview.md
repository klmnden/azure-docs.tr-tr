---
title: Team Data Science Process nedir?
description: Tahmine dayalı analiz çözümlerini ve akıllı uygulamalar sunmak için bir veri bilimi metodolojisidir sağlar.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 10/20/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 3b4e8c78d7402c254c91c3e100814e1f3eafc41b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "61429442"
---
# <a name="what-is-the-team-data-science-process"></a>Team Data Science Process nedir?

Team Data Science işlem (TDSP), Tahmine dayalı analiz çözümlerini ve akıllı uygulamaları etkili bir şekilde sunmak için bir Çevik, yinelemeli bir veri bilimi metodolojisidir olur. TDSP, takım işbirliğinin ve öğrenimin geliştirilmesine yardımcı olur. Veri bilimi girişimlerini başarıyla uygulamayı kolaylaştıran, Microsoft’un ve sektördeki diğer kuruluşların en iyi deneyimlerinin ve yapılarının bileşimini içerir. Hedef, şirketlerin kendi analiz programlarının avantajlarını tam olarak hayata geçirmesine yardımcı olmaktır.

Bu makalede TDSP ve ana bileşenlerini genel bir bakış sağlar. Uygulanabilir genel bir açıklama buraya işleminin çeşitli araçlar sunuyoruz. Proje görevlerini ve rolleri işlem yaşam döngüsü içinde ilgili daha ayrıntılı bir açıklama ek bağlantılı konularda sağlanır. Microsoft araçları ve TDSP ekiplerimiz uygulamak için kullandığımız altyapısı belirli bir kümesini kullanarak TDSP uygulama konusunda yönergeler de verilmektedir.

## <a name="key-components-of-the-tdsp"></a>TDSP temel bileşenleri

TDSP anahtar aşağıdaki bileşenlerden oluşur:

- A **veri bilimi yaşam döngüsünü** tanımı
- A **standartlaştırılmış Proje yapısı**
- **Altyapı ve kaynakları** için veri bilimi projeleri
- **Araçlar ve yardımcı programlar** proje yürütme


## <a name="data-science-lifecycle"></a>Veri bilimi yaşam döngüsü

Team Data Science işlem (TDSP), veri bilimi proje geliştirme yapısı için bir yaşam döngüsü sağlar. Yaşam döngüsü, başlangıçtan bitişe kadar bunlar yürütüldüğünde projeleri genellikle izlemeniz adımlarını özetler.

Başka bir veri bilimi yaşam döngüsünü, gibi kullanıyorsanız [NET-DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) veya kuruluşun kendi özel işleminizi, görev tabanlı TDSP bu geliştirme yaşam döngüleri bağlamında kullanmaya devam edebilirsiniz. Yüksek düzeyde, bu farklı yöntemleri çok ortaktır. 

Bu yaşam döngüsünü, akıllı uygulamalar bir parçası olarak gönderilen veri bilimi projeleri için tasarlanmıştır. Bu uygulamalar, Tahmine dayalı analiz için makine öğrenme veya yapay zeka modelleri dağıtın. Bu işlemi kullanarak keşif veri bilimi projeleri veya geçici analiz projeleri de yararlanabilir. Ancak, bu gibi durumlarda, bazı açıklanan adımları gerekmeyebilir.    

Yaşam döngüsü projeleri genellikle genellikle yinelemeli olarak yürütme, önemli aşamalar açıklanmaktadır:

* **İşin gereksinimlerini anlama**
* **Veri edinme ve anlama**
* **Modelleme**
* **Dağıtım**
* **Müşteri kabulü**

Görsel bir temsilini işte **Team Data Science Process yaşam döngüsü**. 

![TDSP Lifecycle2](./media/overview/tdsp-lifecycle2.png) 

Hedefleri, görevleri ve belgeleri yapıtlar TDSP çevriminin her aşaması için açıklanan [Team Data Science Process yaşam döngüsü](lifecycle.md) konu. Bu görevler ve yapıtları proje rolleri ile ilişkilendirilmiş:

- Çözüm Mimarı
- Proje Yöneticisi
- Veri uzmanı
- Proje lideri 

Aşağıdaki diyagramda görevlerinde (mavi) ve bu rolleri (dikey eksende) için yaşam döngüsü (yatay eksende) her aşaması ilişkili yapıları (yeşil içinde) bir kılavuz görünümünü sağlar. 

[![TDSP-roller-ve-görevleri](./media/overview/tdsp-tasks-by-roles.png)](./media/overview/tdsp-tasks-by-roles.png#lightbox)

## <a name="standardized-project-structure"></a>Standart Proje yapısı

Bir dizin yapısına paylaşın ve proje belgelerini şablonlarını kullanma tüm projeleri sahip, onların projeleri hakkında bilgi, takım üyeleri için kolaylaştırır. Tüm kod ve belgelere işbirliğini etkinleştirmek için Git, TFS veya Subversion gibi bir sürüm denetim sistemi (VC) depolanır. İzleme görevleri ve izleme sistemi Jıra, yarışı ve Azure DevOps gibi bir Çevik proje özelliklerinde, tek tek özellikler için kodun yakın izleme sağlar. Bu tür izleme de daha iyi maliyet tahminlerini almak takımların imkan tanır. TDSP, sürüm oluşturma, bilgi güvenliği ve işbirliği için VC'ler her proje için ayrı bir depo oluşturmanızı önerir. Tüm projeler için standartlaştırılmış yapısı kuruluş genelinde kurumsal bilgi yapı yardımcı olur.

Klasör yapısını ve standart olmayan konumlara gerekli belgeleri için şablonlar sağlar. Veri keşfi ve özellik ayıklama kodunu içeren ve bu modeli yinelemeler kayıt dosyaları bu klasör yapısını düzenler. Bu şablonlar, başkaları tarafından yapılan iş anlamak ve takıma yeni üyeler eklemek için takım üyeleri için kolaylaştırır. Belge şablonları markdown biçiminde görebilecek ve kolay bir işlemdir. Anahtar sorular sorun iyi tanımlanmış ve teslim edilebilirler beklenen kaliteyi karşıladığını sağlamak üzere her proje için denetim listeleri sağlamak için şablonları kullanın. Örneklere şunlar dahildir:

- projenin kapsamını ve iş sorununu belge için bir proje kurucu
- Belge yapısı ve istatistikleri ham verilerin veri raporlarını
- belge türetilmiş özellikleri için model raporları
- ROC eğrileri veya MSE gibi performans ölçümlerini modeli


[![TDSP dizinleri](./media/overview/tdsp-dir-structure.png)](./media/overview/tdsp-dir-structure.png#lightbox)

Dizin yapısı dan kopyalanabilir [GitHub](https://github.com/Azure/Azure-TDSP-ProjectTemplate).

## <a name="infrastructure-and-resources-for-data-science-projects"></a>Altyapı ve veri bilimi projeleri için kaynaklar  

TDSP paylaşılan analiz ve depolama altyapısı gibi yönetmek için öneriler sağlar:

- veri kümelerini depolamak için bulut dosya sistemleri 
- veritabanları
- büyük veri (Hadoop veya Spark) kümeleri 
- Machine learning hizmeti 

Analiz ve depolama altyapısı, bulutta veya şirket içinde olabilir. Ham ve işlenen veri kümeleri depolandığı budur. Bu Altyapı yeniden üretilebilen analizi sağlar. Ayrıca, tutarsızlıklar ve gereksiz altyapı maliyetlerinden açabilir çoğaltma önler. Bu kaynaklara güvenli bir şekilde bağlanmak her takım üyesinin izin paylaşılan kaynakları sağlamak ve bunları izlemek için Araçlar sağlanır. Proje üyelerini tutarlı işlem ortamı oluşturmak için de iyi bir uygulamadır. Farklı ekip üyelerinin çoğaltabilir ve denemeleri doğrulayın.

Birden fazla proje üzerinde çalışma ve çeşitli bulut analiz altyapısı bileşenleriyle paylaşımı takım örneği aşağıda verilmiştir.

[![TDSP altyapı](./media/overview/tdsp-analytics-infra.png)](./media/overview/tdsp-analytics-infra.png#lightbox) 


## <a name="tools-and-utilities-for-project-execution"></a>Araçlar ve yardımcı programlar için proje yürütme

Çoğu kuruluş işlemde giriş zordur. Veri bilimi işlemi ve yaşam döngüsü Yardım düşük önündeki engelleri uygulamak ve kendi benimseme tutarlılığını artırmak için sağlanan araçları. TDSP araçları ve betikleri TDSP benimsenmesini ekip içinde hızlı giriş yapmak için başlangıç kümesi sağlar. Ayrıca, veri bilimi yaşam döngüsünü veri keşfi ve modelleme temel gibi ortak görevlerin bazıları otomatikleştirilmesine yardımcı olur. Kişiler, paylaşılan Araçlar ve yardımcı programlar, takımın paylaşılan kod depoda katkıda bulunmak için sağlanan iyi tanımlanmış bir yapı yoktur. Bu kaynaklar, sonra ekip veya kuruluş içindeki diğer projeleri tarafından yararlanılabilir. TDSP araçları ve yardımcı programlar için bütün topluluk Katkıları etkinleştirmek de planları. TDSP yardımcı programları dan kopyalanabilir [GitHub](https://github.com/Azure/Azure-TDSP-Utilities).


## <a name="next-steps"></a>Sonraki adımlar

[Team Data Science Process'i: Rolleri ve görevleri](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/roles-tasks.md) anahtar personel rolleri ve ilişkilendirilen görevlerinin standartlaştırır bu işlemde bir veri bilimi takım için özetler. 
