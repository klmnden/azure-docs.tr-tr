---
title: Veri bilimi projeleri - Team Data Science Process, Çevik Geliştirme
description: Geliştiriciler bir sistematik, sürüm denetimli bir veri bilimi proje nasıl yürütebilir ve işbirliğine dayalı bir şekilde Team Data Science Process kullanarak bir proje ekibi içinde.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/28/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: adf713fc3f875168f99b302b0a9affef88e8414f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60327906"
---
# <a name="agile-development-of-data-science-projects"></a>Veri bilimi projeleri, Çevik Geliştirme

Bu belgede, nasıl geliştiricilerin veri bilimi proje bir sistematik olarak, sürüm denetimli ve işbirliğine dayalı bir şekilde bir proje ekibi içinde kullanarak yürütebilirsiniz açıklanmaktadır [Team Data Science Process](overview.md) (TDSP). TDSP, etkinlikleri, bulut tabanlı ve Tahmine dayalı analiz çözümleri verimli bir şekilde yürütmek için yapılandırılmış bir dizi sağlayan Microsoft tarafından geliştirilen bir çerçevedir. Personel roller ve bir veri bilimi takım bu işlemle ilgili hale getirerek işlenen ilişkilendirilen görevlerinin ana hat için bkz. [Team Data Science Process rolleri ve görevleri](roles-tasks.md). 

Bu makale için yönergeler içerir: 

1. yapmak **sprint planlamayı** bir projesinde iş öğeleri için.<br> Sprint planlama ile alışkın değilseniz, ayrıntıları ve genel bilgileri bulabilirsiniz [burada](https://en.wikipedia.org/wiki/Sprint_(software_development) "burada"). 
2. **İş öğelerini eklemek** sprint için. 

> [!NOTE]
> Azure DevOps Hizmetleri'ni kullanarak TDSP takım ortamını ayarlamak için gerekli olan adımları aşağıdaki yönergeler kümesini özetlenmiştir. Bunlar, Microsoft'ta TDSP uygulama olduğundan, Azure DevOps hizmetleriyle bu görevleri gerçekleştirmek üzere nasıl belirtin.  Azure DevOps Hizmetleri, öğeleri (3) ve (4) önceki listede kullanmayı tercih ederseniz doğal olarak aldığınız avantajları var. Grubunuz için başka bir kod barındırma platformu kullanılıyorsa, ekip lideri tarafından genellikle tamamlanması gereken görevler değiştirmeyin. Ancak bu görevleri tamamlamak için yol farklı zordur. Örneğin, öğe bölümünde altı, **Git dal ile bir çalışma öğesiyle bağlantılandırmak**, Azure DevOps hizmetlerinde olduğu gibi kolay olmayabilir.
>
>

Tipik bir sprint planlama, kodlama aşağıdaki şekilde gösterilmiştir ve uygulama bir veri bilimi proje kaynak denetimi iş akışı söz konusu:

![1](./media/agile-development/1-project-execute.png)


##  1. <a name='Terminology-1'></a>Terminolojisi 

TDSP sprint planlama framework, sık kullanılan dört tür **iş öğelerini**: **Özellik**, **kullanıcı hikayesi**, **görev**, ve **hata**. Tüm iş öğeleri için tek bir biriktirme listesi her proje tutar. Git deposu düzeyinde bir projesi altındaki hiçbir biriktirme listesi yoktur. Tanımlarını şunlardır:

- **Özellik**: Bir özellik için bir proje engagement karşılık gelir. İstemcisi ile farklı angajmanları farklı özellikleri olarak kabul edilir. Benzer şekilde, farklı bir proje aşamasının farklı özelliklere sahip bir istemci dikkate alınması gereken en iyisidir. Bir şema gibi seçerseniz ***ClientName EngagementName*** özelliklerinizi adlandırmak için ardından, kolayca adları proje/engagement'tan bağlamında tanıyabilir.
- **Hikaye**: Hikayeler bir özellik (Proje) için uçtan uca tamamlamak için gereken farklı iş öğelerdir. Hikayeler örnekleri şunlardır:
    - Veri alma 
    - Verileri Araştırma 
    - Özellikleri oluşturma
    - Yapı modelleri
    - Faaliyete geçirmeye yönelik model 
    - Modelleri yeniden eğitme
- **Görev**: Atanabilir kod veya belge iş öğeleri ya da belirli bir hikayeyi tamamlamak üzere gerçekleştirilmesi gereken diğer etkinlikleri görevlerdir. Örneğin, öyküyü görevleri *veri alma* olabilir:
    -  SQL Server'ın kimlik bilgilerini alma 
    -  SQL Data Warehouse'a veri yükleme. 
- **Hata**: Hataları, genellikle bir görevi tamamlanırken yapılan bir var olan kod veya belge için gerekli düzeltmeleri için bakın. Aşamalar veya görevleri sırasıyla eksik hataya neden oluyorsa, bir hikayesi veya bir görev olarak ilerletebilirler. 

> [!NOTE]
> Kavramları, özellikleri, hikayeleri, görevleri ve yazılım kodu Yönetimi (SCM) veri bilimi kullanılacak hatalardan ödünç. Bunlar, geleneksel SCM tanımlarından biraz farklı olabilir.
>
>

> [!NOTE]
> Veri bilimcileri, özellikle TDSP yaşam döngüsünün aşamaları ile eşleşen bir Çevik şablonu kullanarak daha iyi çalışır. Göz önünde bir Çevik türetilmiş sprint planlama şablonu, Epic'ler, hikayeleri vb. TDSP yaşam döngüsünün aşamaları veya substages burada değiştirilir oluşturuldu. Bir Çevik şablonu oluşturma hakkında yönergeler için bkz: [Visual Studio Online'da Çevik data science Process'i ayarlama](agile-development.md#set-up-agile-dsp-6).
>
>

## 2. <a name='SprintPlanning-2'></a>Sprint planlama 

Sprint planlama, proje önceliği ve kaynak planlama ve ayırma için kullanışlıdır. Birçok veri uzmanları, her biri tamamlanması aylar sürebilir, birden fazla projeyle katılan. Projeleri, genellikle farklı oşluklar devam edin. Azure DevOps Services'ta kolayca oluşturabilir, yönetebilir ve projenizde iş öğelerini izlemek ve sprint projelerinizi beklendiği gibi ileri taşıdığınızdan emin olmak planlama yapın. 

İzleyin [bu bağlantıyı](https://www.visualstudio.com/en-us/docs/work/scrum/sprint-planning) sprint planlama Azure DevOps Hizmetleri'nde adım adım yönergeler için. 


## 3. <a name='AddFeature-3'></a>Özellik Ekle  

İçin takım projesi deponuzu altında bir projeye oluşturulduktan sonra Git **genel bakış** sayfasında ve tıklayın **çalışmasını yönetmesi**.

![2](./media/agile-development/2-sprint-team-overview.png)

Biriktirme listesinde bir özellik eklemek için tıklatın **biriktirme listelerini** --> **özellikleri** --> **yeni**, özellik türü **başlık**(genelde projenizin adına) ve ardından **Ekle** .

![3](./media/agile-development/3-sprint-team-add-work.png)

Oluşturduğunuz özellik çift tıklayın. Bağlantısındaki açıklamalara doldurun, bu özellik için takım üyeleri atamak ve planlama parametreleri için bu özelliği ayarlayın. 

Bu özellik Proje depoya da bağlayabilirsiniz. Tıklayın **Ekle bağlantısını** altında **geliştirme** bölümü. Özellik düzenleme tamamladıktan sonra tıklayın **Kaydet ve Kapat** çıkmak için.


## 4. <a name='AddStoryunderfeature-4'></a>Hikaye altında özelliği Ekle 

Özelliği altında (özellik) proje işleminin tamamlanması için gereken önemli adımlar açıklamak için hikayeleri eklenebilir. Yeni bir hikaye eklemek için tıklatın **+** sol tarafındaki özellik biriktirme listesi görünümü'nde oturum açın.  

![4](./media/agile-development/4-sprint-add-story.png)

Yazının durumu, açıklama, yorumlar, planlama ve açılır pencerede öncelik gibi ayrıntılarını düzenleyebilirsiniz.

![5](./media/agile-development/5-sprint-edit-story.png)

Bu hikaye tıklayarak mevcut bir depoya bağlayabilirsiniz **+ Ekle bağlantısını** altında **geliştirme**. 

![6](./media/agile-development/6-sprint-link-existing-branch.png)


## 5. <a name='AddTaskunderstory-5'></a>Bir hikaye için bir görev ekleyin 

Her hikayesini tamamlamak için gereken belirli ayrıntılı adımlar görevlerdir. Hikayeyi çok konunun tüm görevler tamamlandıktan sonra tamamlanmalıdır. 

Bir hikaye için bir görev eklemek için tıklatın **+** oturum select hikayesi öğesinin yanındaki **görev**ve ardından bu görevin açılır pencerede ayrıntılı bilgileri doldurun.

![7](./media/agile-development/7-sprint-add-task.png)

Özellikler, hikayeler ve görevler oluşturduktan sonra bunları görüntüleyebilirsiniz **biriktirme listesi** veya **Panosu** durumlarını izlemek için görünümler.

![8](./media/agile-development/8-sprint-backlog-view.png)

![9](./media/agile-development/9-link-to-a-new-branch.png)


## 6. <a name='set-up-agile-dsp-6'></a> Bir Visual Studio Online'da Çevik TDSP iş şablonu ayarlama

Bu makalede, TDSP veri bilimi yaşam döngüsünün aşamaları kullanan ve Visual Studio Online (vso) ile çalışma öğelerini izleyen bir Çevik veri bilimi işlem şablonu ayarlama açıklanmaktadır. Veri bilimi özgü Çevik ayarlama örneği kılavuzda aşağıdaki adımları işlem şablonuna *AgileDataScienceProcess* ve şablonu temel alan veri bilimi iş öğelerini oluşturma işlemi gösterilmektedir.

### <a name="agile-data-science-process-template-setup"></a>Çevik veri bilimi işlem şablonu Kurulumu

1. Sunucu giriş sayfasına gidin **yapılandırma** -> **işlem**.

    ![10](./media/agile-development/10-settings.png) 

2. Gidin **tüm işlemler** -> **işlemleri**altında **Çevik** tıklayın **devralınan işlem oluştur**. Ardından, işlem adı "AgileDataScienceProcess" put ve tıklayın **işlem oluşturma**.

    ![11](./media/agile-development/11-agileds.png)

3. Altında **AgileDataScienceProcess** -> **iş öğesi türleri** sekmesinde, devre dışı **Epic**, **özellik**,  **Kullanıcı hikayesi**, ve **görev** iş öğesi türleri tarafından **yapılandırma -> devre dışı bırak**

    ![12](./media/agile-development/12-disable.png)

4. Gidin **AgileDataScienceProcess** -> **biriktirme listesi düzeyleri** sekmesi. Tıklayarak "TDSP projelerine" "Epic" Yeniden Adlandır **yapılandırma** -> **düzenleme/yeniden adlandırma**. Aynı iletişim kutusuna tıklayın **+ yeni iş öğesi türü** "Veri bilimi proje" de ve değerini ayarlama **varsayılan iş öğesi türü** "TDSP projesi" 

    ![13](./media/agile-development/13-rename.png)  

5. Benzer şekilde, biriktirme listesi adı "Özellikler" "TDSP aşamalara" değiştirip ekleyin **yeni iş öğesi türünü**:

    - İşin Gereksinimlerini Anlama
    - Veri alma
    - Modelleme
    - Dağıtım

6. "Kullanıcı hikayesi", "TDSP yeni oluşturulan"TDSP Substage"türüne ayarlanır Substages" varsayılan iş öğesi türü ile yeniden adlandırın.

7. "Yeni iş öğesi türü"TDSP görevi"oluşturulan görevlere" ayarlayın 

8. Bu adımlardan sonra kapsam düzeyleri şu şekilde görünmelidir:

    ![14](./media/agile-development/14-template.png)  

 
### <a name="create-data-science-work-items"></a>Veri bilimi iş öğeleri oluşturma

Veri bilimi işlem şablonu oluşturulduktan sonra oluşturun ve TDSP yaşam döngüsü için karşılık gelen veri bilimi iş öğelerinizi izleyin.

1. Yeni bir proje oluşturduğunuzda, "Agile\AgileDataScienceProcess" olarak seçin **iş öğesi işlemi**:

    ![15](./media/agile-development/15-newproject.png)

2. Yeni oluşturulan projeye gidin ve tıklayarak **iş** -> **biriktirme listelerini**.

3. Tıklayarak "TDSP projeleri" görünür hale getirin **team ayarlarını yapılandır** ve "TDSP projeleri" denetleyin; ardından kaydedin.

    ![16](./media/agile-development/16-enabledsprojects.png)

4. Artık veri bilimi özgü iş öğelerini oluşturmaya başlayabilirsiniz.

    ![17](./media/agile-development/17-dsworkitems.png)

5. Veri bilimi proje çalışma öğeleri nasıl görünmelidir örneği aşağıda verilmiştir:

    ![18](./media/agile-development/18-workitems.png)


## <a name="next-steps"></a>Sonraki adımlar

[Git ile işbirliğine dayalı kodlama](collaborative-coding-with-git.md) paylaşılan kod geliştirme çerçevesi Git kullanarak veri bilimi projeleri için işbirliğine dayalı kod geliştirme yapmak ve çevik işlem ile planlanan çalışmayı etkinlikleri kodlama bağlantı açıklanır.

Çevik süreçlerin şirket kaynaklarına ek bağlantılar aşağıda verilmiştir.

- Çevik işlem   [https://www.visualstudio.com/en-us/docs/work/guidance/agile-process](https://www.visualstudio.com/en-us/docs/work/guidance/agile-process)
- Çevik işlem iş öğesi türleri ve iş akışı   [https://www.visualstudio.com/en-us/docs/work/guidance/agile-process-workflow](https://www.visualstudio.com/en-us/docs/work/guidance/agile-process-workflow)


İşlem için tüm adımları gösteren talimatlara **belirli senaryoları** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) makalesi. Bunlar, bulut, şirket içi araçları ve Hizmetleri, bir iş akışı veya akıllı bir uygulama oluşturmak için işlem hattı birleştirme işlemini göstermektedir. 
