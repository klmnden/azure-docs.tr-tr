---
title: "Veri bilimi projeleri - Azure yürütülmesi | Microsoft Docs"
description: "Bir veri Bilimcisi veri bilimi projesi nasıl trackable, yürütebilir sürüm denetimli ve işbirliği yolu."
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
ms.date: 11/16/2017
ms.author: bradsev;
ms.openlocfilehash: 1015a9f24ca2c175ff367b1748f05bb3e464457f
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="execution-of-data-science-projects"></a>Veri bilimi projeleri yürütülmesi

Bu belgede, nasıl geliştiriciler veri bilimi projesinde bir sistematik, sürüm denetimli ve proje ekibi içinde işbirliği biçimini kullanarak yürütebilirsiniz açıklanmaktadır [takım veri bilimi işlemi](overview.md) (TDSP). TDSP etkinlikleri bulut tabanlı, Tahmine dayalı analiz çözümleri verimli bir şekilde çalıştırmak için yapılandırılmış bir dizi sağlayan Microsoft tarafından geliştirilen bir çerçevedir. Personel rolleri ve bir veri bilimi takım bu işlemi Standartlaştırma tarafından işlenen ilişkilendirilen görevlerinin ana hattı için bkz: [takım veri bilimi işlemi rolleri ve görevleri](roles-tasks.md). 

Bu makale için yönergeler içerir: 

1. yapmak **sprint planlama** bir proje ile ilgili iş öğeleri için.<br> Sprint planlama konusunda bilginiz yoksa ayrıntıları ve genel bilgiler bulabileceğiniz [burada](https://en.wikipedia.org/wiki/Sprint_(software_development) "burada"). 
2. **İş öğesi ekleme** sprint için.
3. **Etkinlikler kodlama ile iş öğelerini bağlama** git tarafından izlenir.
4. yapmak **kod gözden geçirme**. 

> [!NOTE]
> Visual Studio Team Services (VSTS) kullanarak bir TDSP takım ortamını ayarlamak için gerekli olan adımları aşağıdaki yönergeler kümesini özetlenmiştir. Bunlar, Microsoft'ta TDSP uygulamak nasıl olduğundan VSTS bu görevleri gerçekleştirmek nasıl belirtin.  VSTS, öğeleri (3) ve (4) önceki listede kullanmayı tercih ederseniz doğal olarak alma yararlar şunlardır. Grubunuz için başka bir kod barındırma platformu kullanılıyorsa, ekip lideri tarafından genellikle tamamlanması gereken görevler değiştirmeyin. Ancak bu görevleri tamamlamak için yol farklı olacak. Örneğin, öğe bölümünde altı, **git dala sahip bir iş öğesi bağlantı**, VSTS üzerinde olduğu gibi kolay olmayabilir.
>
>

Tipik bir sprint planlama, kodlama aşağıdaki şekilde gösterilmiştir ve veri bilimi projesi uygulama kaynak denetimi iş akışı söz konusu:

![1](./media/project-execution/project-execution-1-project-execute.png)


##  1. <a name='Terminology-1'></a>Terminolojisi 

Planlama framework TDSP sprint, sık kullanılan dört tür **iş öğelerini**: **özelliği**, **kullanıcı hikayesi**, **görev**, ve **Hata**. Her takım projesi tüm iş öğeleri için tek bir biriktirme listesini tutar. Bir takım projesi altında git deposu düzeyinde hiçbir biriktirme listesi yok. Tanımlarını şunlardır:

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

Veri bilimcilerine özellikle ile TDSP yaşam döngüsünün aşamaları hizalar Çevik bir şablonu kullanarak daha rahat açabilir. Aklınızda bir Çevik türetilmiş sprint planlama şablonu, Destanlar vb. hikayeleri TDSP yaşam döngüsünün aşamaları veya substages tarafından nerede değiştirilir oluşturuldu. Çevik bir şablonun nasıl oluşturulacağını belgelerinde bulunabilir [burada](https://msdata.visualstudio.com/AlgorithmsAndDataScience/TDSP/_git/TDSP?path=%2FDocs%2Fteam-data-science-process-agile-template.md&version=GBxibingao&_a=preview).


##  2. <a name='SprintPlanning-2'></a>Sprint planlama 

Sprint planlama proje önceliği ve kaynak planlama ve ayırma için yararlıdır. Birçok veri bilimcilerine her biri tamamlamak için aylar alabilir birden çok proje ile katılan. Projeleri genellikle farklı meleri boşluk devam edin. VSTS sunucuda kolayca oluşturabilir, yönetebilir ve takım projesinde iş öğelerini izlemek ve projelerinizi beklendiği gibi ileri taşıdığınızdan emin olmak planlama sprint yürütün. 

İzleyin [bu bağlantıyı](https://www.visualstudio.com/en-us/docs/work/scrum/sprint-planning) içinde VSTS planlama sprint adım adım yönergeler için. 


##  3. <a name='AddFeature-3'></a>Özellik ekleme  

Takım projesi altındaki proje deponuz oluşturulduktan sonra ekibine gitmek **genel bakış** sayfasında ve tıklayın **çalışmasını yönetmesi**.

![2](./media/project-execution/project-execution-2-sprint-team-overview.png)

Bir özellik biriktirme listesine eklemek için tıklatın **biriktirme listeleri** --> **özellikleri** --> **yeni**, özellik türü **başlık**(genellikle projenizin adına) ve ardından **Ekle** .

![3](./media/project-execution/project-execution-3-sprint-team-add-work.png)

Oluşturduğunuz özelliği çift tıklayın. Açıklamasında doldurun, takım üyeleri bu özellik için atayın ve planlama bu özellik için parametreleri ayarlayın. 

Ayrıca, bu özellik Proje depoya bağlayabilirsiniz. Tıklatın **Bağlantı Ekle** altında **geliştirme** bölümü. Özellik düzenlemeyi bitirdikten sonra tıklatın **Kaydet ve Kapat** çıkmak için.


##  4. <a name='AddStoryunderfeature-4'></a>Özellik altındaki Öykü ekleme 

Özellik altında hikayeleri (özellik) proje tamamlamak için gereken önemli adımlar açıklamak için eklenebilir. Yeni Öykü eklemek için tıklatın  **+**  oturum biriktirme listesi görünümünde özellik solundaki.  

![4](./media/project-execution/project-execution-4-sprint-add-story.png)

Durumu, açıklama, yorumlar, planlama ve açılır pencerede öncelik gibi Öykü ayrıntılarını düzenleyebilirsiniz.

![5](./media/project-execution/project-execution-5-sprint-edit-story.png)

Tıklayarak mevcut bir depoya bu hikayesine bağlayabilirsiniz **+ Ekle bağlantısına** altında **geliştirme**. 

![6](./media/project-execution/project-execution-6-sprint-link-existing-branch.png)


##  5. <a name='AddTaskunderstory-5'></a>Bir hikayesine görev ekleme 

Her hikaye tamamlamak için gereken belirli ayrıntılı adımlar görevlerdir. Konunun tüm görevler tamamlandıktan sonra Öykü çok tamamlanmalıdır. 

Bir hikayesine bir görev eklemek için tıklatın  **+**  select Öykü öğesinin yanındaki oturum **görev**ve ardından bu görevin açılır pencerede ayrıntılı bilgileri doldurun.

![7](./media/project-execution/project-execution-7-sprint-add-task.png)

Özellikler, hikayeler ve görevleri oluşturduktan sonra bunları görüntüleyebilirsiniz **biriktirme listesi** veya **Panosu** durumlarını izlemek için görünümleri.

![8](./media/project-execution/project-execution-8-sprint-backlog-view.png)

![9](./media/project-execution/project-execution-9-link-to-a-new-branch.png)


##  6. <a name='Linkaworkitemwithagitbranch-6'></a>İş öğesi git dal ile bağlantı 

VSTS git dal ile bir iş öğesi (Öykü veya görev) bağlamak için kolay bir yol sağlar. Bu, doğrudan ilişkili kod hikayesi veya görev bağlamak üzere sağlar. 

Bir iş öğesi için yeni bir dalı bağlanmak için bir iş öğesini çift tıklatın ve açılır penceresinde **yeni bir dal oluşturma** altında **+ Ekle bağlantısına**.  

![10](./media/project-execution/project-execution-10-sprint-board-view.png)

Dal adı, temel git deposu ve şube gibi bu dalı için bilgiler sağlar. Seçilen git deposu depo iş öğesi ait aynı takım projesi altında olmalıdır. Temel şube ana dala veya başka bir mevcut şube olabilir.

![11](./media/project-execution/project-execution-11-create-a-branch.png)

Her hikayesi iş öğesi için bir git dalı oluşturmak iyi bir uygulamadır. Ardından, her görevin çalışma öğesi için Öykü şubeyi temel alarak bir dal oluşturun. Öykü görev ilişkilerini karşılık gelen hiyerarşik bu şekilde dalları düzenleme aynı proje farklı öyküleri üzerinde çalışan birden çok kişilerin sahip veya aynı Öykü farklı görevler üzerinde çalışan birden çok kişinin sahip yararlıdır. Her ekip üyesi farklı dal ve her üye farklı kodları veya diğer yapıları bir dal paylaşırken çalışırken çalışırken çakışmaları en aza indirgenebilir. 

Aşağıdaki resimde TDSP için önerilen dallanma strateji gösterilmektedir. Burada, özellikle yalnızca biri varsa gösterildiği gibi veya aynı proje üzerinde çalışan iki kişinin çoğu dallandırır ya da konunun tüm görevlerinde yalnızca bir kişinin çalıştığı gibi gerekmeyebilir. Ancak ana daldan geliştirme şube ayırarak her zaman iyi bir uygulamadır. Bu, geliştirme etkinlikler tarafından kesintiye yayın dalı önlemeye yardımcı olabilir. Git şube modeli daha eksiksiz bir açıklaması bulunabilir [bir başarılı Git dallanma modeli](http://nvie.com/posts/a-successful-git-branching-model/).

![12](./media/project-execution/project-execution-12-git-branches.png)

Üzerinde çalışmak istediğiniz şube geçmek için bir kabuk komutu (Windows veya Linux) aşağıdaki komutu çalıştırın. 

    git checkout <branch name>

Değiştirme *< şube adı\>*  için **ana** anahtarları yedeklemek için **ana** dal. Çalışma dalı geçiş yaptıktan sonra bu iş öğesi üzerinde çalışan, öğe tamamlamak için gereken kodu veya belge yapıları geliştirmeye başlayabilirsiniz. 

Ayrıca varolan bir dal için bir iş öğesi bağlayabilirsiniz. İçinde **ayrıntı** tıklamak yerine bir iş öğesinin sayfa **yeni bir dal oluşturma**, tıklattığınız **+ Ekle bağlantısına**. Ardından, iş öğesine bağlamak istediğiniz dalı seçin. 

![13](./media/project-execution/project-execution-13-link-to-an-existing-branch.png)

Yeni bir dalı git bash komutları de oluşturabilirsiniz. Varsa < temel şube adı\> eksik, < yeni şube adı\> dayanır _ana_ dal. 
    
    git checkout -b <new branch name> <base branch name>


##  7. <a name='WorkonaBranchandCommittheChanges-7'></a>Bir dal üzerinde çalışır ve değişiklikleri uygulayın 

Şimdi, bazı değişiklik varsayalım *veri\_alım* yerel makinenizde dalı üzerinde bir R dosyası ekleme gibi iş öğesi için dal. Aşağıdaki Git komutları kullanarak bu dalda, Git Kabuğu'nda sağlanan dal bu iş öğesi için eklenen R dosya tamamlayabilir:

    git status
    git add .
    git commit -m"added a R scripts"
    git push origin data_ingestion

![14](./media/project-execution/project-execution-14-sprint-push-to-branch.png)

##  8. <a name='CreateapullrequestonVSTS-8'></a>VSTS üzerinde bir çekme isteği oluşturun 

Birkaç işlemeleri ve itmelerden sonra hazır olduğunuzda, ana dala, geçerli dalı birleştirmek için gönderebilmek için bir **çekme isteği** VSTS sunucusunda. 

Takım projenizin ana sayfasına gidin ve tıklayın **kod**. Birleştirilecek şube ve dala birleştirmek istediğiniz git deposu adını seçin. Ardından **çekme istekleri**, tıklatın **yeni çekme isteği** iş dalı üzerinde temel dalının birleştirilmiş önce bir çekme isteği gözden geçirmesine oluşturmak için.

![15](./media/project-execution/project-execution-15-spring-create-pull-request.png)

Bu çekme isteğiyle ilgili bazı açıklamasını doldurun, gözden geçirenleri ekleme ve bunu göndermek.

![16](./media/project-execution/project-execution-16-spring-send-pull-request.png)

##  9. <a name='ReviewandMerge-9'></a>Gözden geçirme ve birleştirme 

Çekme isteği oluştururken, gözden geçirenler çekme istekleri gözden geçirmek için bir e-posta bildirimi alır. Gözden geçirenler değişiklikler veya çalışma ve istek sahibinin değişikliklerle mümkünse test denetlemeniz gerekir. Kendi değerlendirmesine göre gözden geçirenler onaylayın veya çekme isteği reddedin. 

![17](./media/project-execution/project-execution-17-add_comments.png)

![18](./media/project-execution/project-execution-18-spring-approve-pullrequest.png)

Gözden geçirme yapıldıktan sonra çalışma dalı ana dala tıklayarak birleştirilen **tam** düğmesi. Birleştirilmiş olmadığı sonra çalışma dalı silmek tercih edebilirsiniz. 

![19](./media/project-execution/project-execution-19-spring-complete-pullrequest.png)

İstek olarak işaretlenmiş sol üst köşede doğrulayın **tamamlandı**. 

![20](./media/project-execution/project-execution-20-spring-merge-pullrequest.png)

Ne zaman, Geri Git deponuza altında **kod**, ana dala değiştirmiş söylediniz.

![21](./media/project-execution/project-execution-21-spring-branch-deleted.png)

Ana dala çalışma dalı birleştirme ve birleştirme sonra çalışma dalı silmek için aşağıdaki Git komutlarını da kullanabilirsiniz:

    git checkout master
    git merge data_ingestion
    git branch -d data_ingestion

![22](./media/project-execution/project-execution-22-spring-branch-deleted-commandline.png)


##  10. <a name='DataQualityReportUtility-10'></a>Etkileşimli veri keşfi, analiz ve Raporlama (IDEAR) yardımcı programı

R markdown tabanlı kullanılmasından değerlendirmek ve veri kümelerini keşfetmek için esnek ve etkileşimli bir araç sağlar. Kullanıcıların hızlı bir şekilde minimal kodlama ile veri kümesinden raporları da oluşturabilirsiniz. Kullanıcıların etkileşimli aracı araştırması sonuçlarında istemcilere teslim veya sonraki model adımda eklemek için hangi değişkenleri hakkında kararlar almak için kullanılan bir son rapor vermek için düğmelerini tıklatabilirsiniz.

Şu anda aracı yalnızca veri çerçevelerini bellekte üzerinde çalışır. .Yaml dosya incelenecek veri kümesi parametrelerini belirtmek için gereklidir. Daha fazla bilgi için bkz: [TDSP veri bilimi yardımcı programları IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils).


##  11. <a name='ModelingUtility-11'></a>Taban çizgisi modelleme ve yardımcı programı raporlama

Bu yardımcı program modeli oluşturma hyper-yerleştirmez, çok parametresiyle gerçekleştirmek ve bu modelleri doğruluğunu karşılaştırmak için özelleştirilebilir, yarı otomatik bir araç sağlar. 

Model oluşturma müstakil html çıkışını farklı bölümleri aracılığıyla kolay gezinme için içindekiler tablosu ile üretmek için çalıştırabilirsiniz bir R markdown dosyası programıdır. Markdown dosyası (birbirine bağlı) çalıştırıldığında üç algoritmalar çalıştırılır: glmnet kullanarak regularized regresyon paketini, randomForest paketini kullanarak ve xgboost paketini kullanarak ağaçları artırmanın rastgele orman). Bu algoritmalar her bir modeli oluşturur. Bu modeller doğruluğunu karşılaştırılır ve göreli özelliği önem çizimleri bildirilir. Şu anda iki yardımcı programları vardır: biri için bir ikili sınıflandırma görev ve regresyon görev için biridir. Bunları birincil farklarını olduğu şekilde denetim parametrelerini ve doğruluk ölçümleri bu öğrenme görevler için belirtilir. 

Yaml dosyası belirtmek için kullanılır:

- veri girişi (SQL kaynağı veya bir R veri dosyası) 
- verilerin hangi kısmını eğitim ve test etmek için ne bölümü için kullanılır
- çalıştırmak için hangi algoritmaları 
- model iyileştirme denetim parametrelerini Seçimi:
    - Çapraz doğrulama 
    - önyükleme eklemesi
    - Çapraz doğrulama Katlama
- hyper-parametresi için her algoritmasını ayarlar. 

Ayrıca algoritmalar, en iyi duruma getirme için Katlama sayısı, hyper-parametreleri sayısı ve üzerinden sweep için hyper-parametre kümesi sayısı modelleri hızlı bir şekilde çalıştırmak için Yaml dosyasında değiştirilebilir. Örneğin, daha az sayıda MS Katlama, parametre kümeleri daha düşük bir sayı ile çalıştırılabilir. Verilmişse bunlar aynı zamanda daha kapsamlı MS Katlama daha yüksek bir sayı veya çok sayıda parametre kümeleri ile çalıştırılabilir.

Daha fazla bilgi için bkz: [otomatik modelleme ve raporlama yardımcı programı'nda TDSP veri bilimi yardımcı programları](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling).


##  12. <a name='PowerBI-12'></a>Power BI panoları projelerle ilerlemesini izleme

Veri bilimi grup yöneticileri, takım müşteri adayları ve proje müşteri adayları gerek projelerini ilerlemesini izlemek için hangi iş bunlardaki ve kim tarafından yapılır ve yapılacaklar listelerinde üzerinde kalır. VSTS kullanıyorsanız, etkinlikler ve bir Git deposu ile ilişkili iş öğelerini izlemek için Power BI panoları oluşturmak kullanabilirsiniz. Visual Studio Team Services için Power BI bağlanma hakkında daha fazla bilgi için bkz: [Power BI Team Services](https://www.visualstudio.com/en-us/docs/report/powerbi/connect-vso-pbi-vs). 

VSTS veri Power BI bağlandıktan sonra Git deposu etkinliklerinizi ve iş öğelerini izlemek için Power BI panolar ve raporlar oluşturmayı öğrenmek için bkz: [oluşturma Power BI panoları ve raporları](https://www.visualstudio.com/en-us/docs/report/powerbi/report-on-vso-with-power-bi-vs). 

Burada, Git etkinlikleri izlemek ve iş öğeleri için oluşturulmuş iki basit bir örnek panolar bulunmaktadır. İlk örnek Panoda git taahhüt etkinlikler farklı tarihler ve farklı depoları farklı kullanıcılar tarafından listelenir. Kolayca dilim ve ilgilendiğiniz olanları filtrelemek için inin.

![23](./media/project-execution/project-execution-23-powerbi-git.png)

İkinci örnek panosunda, iş öğelerini (hikayeleri ve görevler) farklı yineleme sunulur. Bunlar atananlar ve öncelik düzeyleri göre gruplandırılmış ve duruma göre renkli.

![24](./media/project-execution/project-execution-24-powerbi-workitem.png)

 
## <a name="next-steps"></a>Sonraki adımlar

Tam işlem için tüm adımları gösteren uçtan uca talimatlara **belirli senaryolar** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) makalesi. Bunlar, bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl koruduğu gösterilmiştir. 

Azure Machine Learning Studio'da kullanmak adımları takım veri bilimi işlemde çalışan örnekler için bkz: [ile Azure ML](http://aka.ms/datascienceprocess) öğrenme yolu.