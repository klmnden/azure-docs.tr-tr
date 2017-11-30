---
title: "Veri bilimi projelerin - Azure Machine Learning Çevik Geliştirme | Microsoft Docs"
description: "Nasıl geliştiriciler saat dilimi ve tarih takım veri bilimi işlemi kullanarak veri bilimi projesinde bir sistematik, sürüm denetimli ve proje ekibi içinde işbirliğine dayalı şekilde çalıştırabilirsiniz."
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: bradsev;
ms.openlocfilehash: 686f751b241d49d116948711c683f4b504d5d5f9
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="agile-development-of-data-science-projects"></a>Veri bilimi projelerinin Çevik Geliştirme

Bu belgede, nasıl geliştiriciler veri bilimi projesinde bir sistematik, sürüm denetimli ve proje ekibi içinde işbirliği biçimini kullanarak yürütebilirsiniz açıklanmaktadır [takım veri bilimi işlemi](overview.md) (TDSP). TDSP etkinlikleri bulut tabanlı, Tahmine dayalı analiz çözümleri verimli bir şekilde çalıştırmak için yapılandırılmış bir dizi sağlayan Microsoft tarafından geliştirilen bir çerçevedir. Personel rolleri ve bir veri bilimi takım bu işlemi Standartlaştırma tarafından işlenen ilişkilendirilen görevlerinin ana hattı için bkz: [takım veri bilimi işlemi rolleri ve görevleri](roles-tasks.md). 

Bu makale için yönergeler içerir: 

1. yapmak **sprint planlama** bir proje ile ilgili iş öğeleri için.<br> Sprint planlama konusunda bilginiz yoksa ayrıntıları ve genel bilgiler bulabileceğiniz [burada](https://en.wikipedia.org/wiki/Sprint_(software_development) "burada"). 
2. **İş öğesi ekleme** sprint için. 

> [!NOTE]
> Visual Studio Team Services (VSTS) kullanarak bir TDSP takım ortamını ayarlamak için gerekli olan adımları aşağıdaki yönergeler kümesini özetlenmiştir. Bunlar, Microsoft'ta TDSP uygulamak nasıl olduğundan VSTS bu görevleri gerçekleştirmek nasıl belirtin.  VSTS, öğeleri (3) ve (4) önceki listede kullanmayı tercih ederseniz doğal olarak alma yararlar şunlardır. Grubunuz için başka bir kod barındırma platformu kullanılıyorsa, ekip lideri tarafından genellikle tamamlanması gereken görevler değiştirmeyin. Ancak bu görevleri tamamlamak için yol farklı olacak. Örneğin, öğe bölümünde altı, **Git dala sahip bir iş öğesi bağlantı**, VSTS üzerinde olduğu gibi kolay olmayabilir.
>
>

Tipik bir sprint planlama, kodlama aşağıdaki şekilde gösterilmiştir ve veri bilimi projesi uygulama kaynak denetimi iş akışı söz konusu:

![1](./media/agile-development/1-project-execute.png)


##  1. <a name='Terminology-1'></a>Terminolojisi 

Planlama framework TDSP sprint, sık kullanılan dört tür **iş öğelerini**: **özelliği**, **kullanıcı hikayesi**, **görev**, ve **Hata**. Her takım projesi tüm iş öğeleri için tek bir biriktirme listesini tutar. Bir takım projesi altında Git deposu düzeyinde hiçbir biriktirme listesi yok. Tanımlarını şunlardır:

- **Özellik**: bir özellik için bir proje katılım karşılık gelir. Bir istemci ile farklı katılımlar farklı özellikler olarak kabul edilir. Benzer şekilde, bir projenin farklı aşamalarında farklı özellikleri olan bir istemci dikkate alınması gereken en iyisidir. Bir şema gibi seçerseniz ***ClientName EngagementName*** özelliklerinizi adlandırmak için daha sonra kolayca adları kendilerini proje/engagement'tan bağlamında tanıyabilirsiniz.
- **Öykü**: hikayeleri olan bir özellik (Proje) uçtan uca tamamlamak için gereken farklı iş öğeleri. Öyküleri örnekleri şunlardır:
    - Veri alma 
    - Veri araştırma 
    - Özellikleri oluşturma
    - Yapı modelleri
    - Faaliyete geçirmeye yönelik modelleri 
    - Modelleri yeniden eğitme
- **Görev**: görevlerdir atanabilir kodu veya belge iş öğeleri ya da belirli bir hikayesini tamamlamak için yapılması gereken diğer etkinlikler. Örneğin, yazıdaki görevleri *veri alma* olabilir:
    -  SQL Server'ın kimlik bilgilerini alma 
    -  SQL veri ambarı'na karşıya veri yükleme. 
- **Hata**: hatalar genellikle bir görevi tamamlarken yapılan bir var olan kodu veya belge için gerekli olan düzeltmelere bakın. Aşamaları veya görevleri sırasıyla eksik hataya neden olursa, bir hikayesi veya bir görev olmaya taşıyarak. 

> [!NOTE]
> Kavramlar, özellikleri, hikayeleri, görevler ve yazılım kod Yönetimi veri bilimi kullanılacak (SCM) hatalardan ödünç. Bunlar kendi geleneksel SCM tanımları biraz farklı olabilir.
>
>

> [!NOTE]
> Veri bilimcilerine özellikle ile TDSP yaşam döngüsünün aşamaları hizalar Çevik bir şablonu kullanarak daha rahat açabilir. Aklınızda bir Çevik türetilmiş sprint planlama şablonu, Destanlar vb. hikayeleri TDSP yaşam döngüsünün aşamaları veya substages tarafından nerede değiştirilir oluşturuldu. Çevik şablonu oluşturma hakkında daha fazla yönerge için bkz: [Çevik veri bilimi işleminde Visual Studio Online ayarlama](agile-development.md#set-up-agile-dsp-6).
>
>

## 2. <a name='SprintPlanning-2'></a>Sprint planlama 

Sprint planlama proje önceliği ve kaynak planlama ve ayırma için yararlıdır. Birçok veri bilimcilerine her biri tamamlamak için aylar alabilir birden çok proje ile katılan. Projeleri genellikle farklı meleri boşluk devam edin. VSTS sunucuda kolayca oluşturabilir, yönetebilir ve takım projesinde iş öğelerini izlemek ve projelerinizi beklendiği gibi ileri taşıdığınızdan emin olmak planlama sprint yürütün. 

İzleyin [bu bağlantıyı](https://www.visualstudio.com/en-us/docs/work/scrum/sprint-planning) içinde VSTS planlama sprint adım adım yönergeler için. 


## 3. <a name='AddFeature-3'></a>Özellik ekleme  

Takım projesi altındaki proje deponuz oluşturulduktan sonra ekibine gitmek **genel bakış** sayfasında ve tıklayın **çalışmasını yönetmesi**.

![2](./media/agile-development/2-sprint-team-overview.png)

Bir özellik biriktirme listesine eklemek için tıklatın **biriktirme listeleri** --> **özellikleri** --> **yeni**, özellik türü **başlık**(genellikle projenizin adına) ve ardından **Ekle** .

![3](./media/agile-development/3-sprint-team-add-work.png)

Oluşturduğunuz özelliği çift tıklayın. Açıklamasında doldurun, takım üyeleri bu özellik için atayın ve planlama bu özellik için parametreleri ayarlayın. 

Ayrıca, bu özellik Proje depoya bağlayabilirsiniz. Tıklatın **Bağlantı Ekle** altında **geliştirme** bölümü. Özellik düzenlemeyi bitirdikten sonra tıklatın **Kaydet ve Kapat** çıkmak için.


## 4. <a name='AddStoryunderfeature-4'></a>Öykü altında Özellik Ekle 

Özellik altında hikayeleri (özellik) proje tamamlamak için gereken önemli adımlar açıklamak için eklenebilir. Yeni Öykü eklemek için tıklatın  **+**  oturum biriktirme listesi görünümünde özellik solundaki.  

![4](./media/agile-development/4-sprint-add-story.png)

Durumu, açıklama, yorumlar, planlama ve açılır pencerede öncelik gibi Öykü ayrıntılarını düzenleyebilirsiniz.

![5](./media/agile-development/5-sprint-edit-story.png)

Tıklayarak mevcut bir depoya bu hikayesine bağlayabilirsiniz **+ Ekle bağlantısına** altında **geliştirme**. 

![6](./media/agile-development/6-sprint-link-existing-branch.png)


## 5. <a name='AddTaskunderstory-5'></a>Bir hikayesine görev ekleme 

Her hikaye tamamlamak için gereken belirli ayrıntılı adımlar görevlerdir. Konunun tüm görevler tamamlandıktan sonra Öykü çok tamamlanmalıdır. 

Bir hikayesine bir görev eklemek için tıklatın  **+**  select Öykü öğesinin yanındaki oturum **görev**ve ardından bu görevin açılır pencerede ayrıntılı bilgileri doldurun.

![7](./media/agile-development/7-sprint-add-task.png)

Özellikler, hikayeler ve görevleri oluşturduktan sonra bunları görüntüleyebilirsiniz **biriktirme listesi** veya **Panosu** durumlarını izlemek için görünümleri.

![8](./media/agile-development/8-sprint-backlog-view.png)

![9](./media/agile-development/9-link-to-a-new-branch.png)


## 6. <a name='set-up-agile-dsp-6'></a>Bir Çevik TDSP iş şablonunda Visual Studio Online ayarlama

Bu makalede, TDSP veri bilimi yaşam döngüsünün aşamaları kullanan ve Visual Studio Online (vso) ile çalışma öğelerini izleyen bir Çevik veri bilimi işlem şablonu ayarlanacağı açıklanmaktadır. Veri bilimi özgü Çevik ayarlama örneği aracılığıyla ilerlemesi aşağıdaki adımları işlem şablonu *AgileDataScienceProcess* ve şablona dayalı veri bilimi iş öğelerinin nasıl oluşturulacağını gösterir.

### <a name="agile-data-science-process-template-setup"></a>Çevik veri bilimi işlem şablonu Kurulumu

1. Sunucu giriş sayfasına gitmek **yapılandırma** -> **işlem**.

    ![10](./media/agile-development/10-settings.png) 

2. Gidin **tüm işlemler** -> **işlemleri**altında **Çevik** ve tıklayın **oluşturma işlemi devralınan**. İşlem adı "AgileDataScienceProcess" put sonra tıklatın **işlem oluşturma**.

    ![11](./media/agile-development/11-agileds.png)

3. Altında **AgileDataScienceProcess** -> **iş öğesi türleri** sekmesinde, devre dışı **Epic**, **özelliği**,  **Kullanıcı hikayesi**, ve **görev** iş öğesi türleri tarafından **yapılandırma -> devre dışı bırak**

    ![12](./media/agile-development/12-disable.png)

4. Gidin **AgileDataScienceProcess** -> **biriktirme listesi düzeyleri** sekmesi. "TDSP projelerine" "Destanlar" tıklayarak yeniden adlandırma **yapılandırma** -> **Düzenle/yeniden adlandırma**. Aynı iletişim kutusunda tıklatın **+ yeni iş öğesi türü** "Veri bilimi projesinde" ve değeri ayarlayın **varsayılan iş öğesi türü** "TDSP projesine" 

    ![13](./media/agile-development/13-rename.png)  

5. Benzer şekilde, biriktirme listesi adı "Özellikler" "TDSP aşamaları" değiştirip aşağıdakileri ekleyin **yeni iş öğesi türünü**:

    - İşin Gereksinimlerini Anlama
    - Veri alma
    - Modelleme
    - Dağıtım

6. "Kullanıcı hikayesi", "TDSP yeni oluşturulan"TDSP Substage"türüne ayarlanır Substages" varsayılan iş öğesi türü ile yeniden adlandırın.

7. "Yeni iş öğesi türü"TDSP görev"oluşturulan görevlere" ayarlayın 

8. Bu adımlar, biriktirme listesi düzeyleri aşağıdaki gibi görünmelidir:

    ![14](./media/agile-development/14-template.png)  

 
### <a name="create-data-science-work-items"></a>Veri bilimi iş öğeleri oluşturma

Veri bilimi işlem şablonu oluşturulduktan sonra oluşturabilir ve TDSP yaşam döngüsü için karşılık gelen, veri bilimi iş öğelerini izlemek.

1. Yeni takım projesi oluşturduğunuzda, "Agile\AgileDataScienceProcess" olarak seçin **iş öğesi işlemi**:

    ![15](./media/agile-development/15-newproject.png)

2. Yeni oluşturulan takım projesine gidin ve tıklayın **iş** -> **biriktirme listeleri**.

3. "TDSP projeleri" tıklayarak görülebilmesi **team ayarlarını yapılandır** ; "TDSP projeleri" denetimi'yi ve sonra kaydedin.

    ![16](./media/agile-development/16-enabledsprojects.png)

4. Şimdi veri bilimi özgü iş öğeleri oluşturma başlatabilirsiniz.

    ![17](./media/agile-development/17-dsworkitems.png)

5. Veri bilimi proje çalışma öğeleri nasıl görünmelidir örneği şöyledir:

    ![18](./media/agile-development/18-workitems.png)


## <a name="next-steps"></a>Sonraki adımlar

[Git ile işbirliği kodlama](collaborative-coding-with-git.md) paylaşılan kod geliştirme framework Git kullanarak veri bilimi projeleri için işbirliği kod geliştirme yapmak ve bu etkinlikler ile çevik işlem planlı işin kodlama bağlantı açıklar.

Aşağıda, Çevik işlemleri üzerindeki kaynaklara bağlantılar verilmiştir.

- Çevik işlem [https://www.visualstudio.com/en-us/docs/work/guidance/agile-process](https://www.visualstudio.com/en-us/docs/work/guidance/agile-process)
- Çevik işlem iş öğesi türleri ve iş akışı [https://www.visualstudio.com/en-us/docs/work/guidance/agile-process-workflow](https://www.visualstudio.com/en-us/docs/work/guidance/agile-process-workflow)


İşlem için tüm adımları gösteren talimatlara **belirli senaryolar** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) makalesi. Bunlar, bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl koruduğu gösterilmiştir. 
