---
title: Git - Team Data Science Process ile işbirliğine dayalı kodlama
description: Çevik planlama ile Git kullanarak veri bilimi projeleri için işbirliğine dayalı kod geliştirme yapmakla.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/28/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 551d0cd149c4d1555a40ccf0d7baeff97c6809c2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60336375"
---
# <a name="collaborative-coding-with-git"></a>Git ile işbirliği içinde kodlama

Bu makalede, biz paylaşılan kod geliştirme çerçevesi Git kullanarak veri bilimi projeleri için işbirliğine dayalı kod geliştirme yapmak nasıl açıklar. Bu etkinlikler için planlanan çalışmayı kodlama bağlama kapsayan [Çevik Geliştirme](agile-development.md) ve kod incelemesi öğrenin.


## 1. <a name='Linkaworkitemwithagitbranch-1'></a>Git dal ile bir çalışma öğesiyle bağlantılandırmak 

Azure DevOps hizmetleriyle Git dalı ile bir iş öğesi (bir hikaye veya görev) bağlanmak için kullanışlı bir yol sağlar. Bu, doğrudan ilişkili kod hikayesi veya görev bağlamanıza olanak sağlar. 

Bir iş öğesi için yeni bir dal bağlanmak için bir iş öğesi'ne çift tıklayın ve açılır pencerede **yeni dal Oluştur** altında **+ Ekle bağlantısını**.  

![1](./media/collaborative-coding-with-git/1-sprint-board-view.png)

Dal adı temel Git deposu ve dalı gibi bu yeni dal için bilgileri sağlayın. Seçilen Git deposu depo iş öğesinin ait olduğu projenin altında olması gerekir. Ana dal, ana dalı veya mevcut bazı bir dal olabilir.

![2](./media/collaborative-coding-with-git/2-create-a-branch.png)

Her hikayesi iş öğesi için bir Git dal oluşturmak iyi bir uygulamadır. Ardından, her görev çalışma öğesi için hikaye dala göre bir dal oluşturun. Aynı projeye farklı hikayelerdeki çalışan birden çok kişinin kullandığınız ya da aynı hikayeyi farklı görevler üzerinde çalışan birden çok kişinin sahip hikaye görev ilişkilerini karşılık gelen hiyerarşik bu şekilde dalları düzenlemek yararlıdır. Her ekip üyesi farklı bir dal ve her üye bir dal paylaşırken farklı kodlarını veya diğer yapıları üzerinde çalışsa çalışırken çakışmaları en aza indirilebilir. 

Aşağıdaki resimde TDSP için önerilen dallanma stratejisi gösterilmektedir. Özellikle yalnızca biri varsa burada gösterilen şekilde veya aynı proje üzerinde çalışan iki kişinin çoğu dallar veya yalnızca bir kişinin bir hikaye tüm görevler üzerinde çalıştığı gerekmeyebilir. Ancak, ana daldan geliştirme dalına ayırarak her zaman iyi bir uygulamadır. Bu yayın dalı geliştirme etkinlikleri tarafından kesintiye önlemeye yardımcı olabilir. Git dal modeli daha ayrıntılı açıklama bulunabilir [bir başarılı Git dallanma modeli](https://nvie.com/posts/a-successful-git-branching-model/).

![3](./media/collaborative-coding-with-git/3-git-branches.png)

Üzerinde çalışmak istediğiniz dala geçmek için kabuk komut (Windows veya Linux) aşağıdaki komutu çalıştırın. 

    git checkout <branch name>

Değiştirme *< dal adı\>*  için **ana** anahtarları yedeklemek için **ana** dal. Çalışma dalı geçtikten sonra bu iş öğesi üzerinde çalışma, öğesini tamamlamak için gereken kodu veya belgeleri yapıtları geliştirmeye başlayabilirsiniz. 

Ayrıca, bir iş öğesi var olan bir dala bağlayabilirsiniz. İçinde **ayrıntı** tıklamak yerine, bir iş öğesi sayfa **yeni dal Oluştur**, tıkladığınız **+ Bağlantı Ekle**. Ardından, çalışma öğesine bağlamak istediğiniz dalı seçin. 

![4](./media/collaborative-coding-with-git/4-link-to-an-existing-branch.png)

Ayrıca, Git Bash komutları yeni bir dal oluşturabilirsiniz. < Ana dal adı\> eksik, < yeni dal adı\> dayanır _ana_ dal. 
    
    git checkout -b <new branch name> <base branch name>


## 2. <a name='WorkonaBranchandCommittheChanges-2'></a>Bir dalda çalışmak ve değişiklikleri 

Şimdi, bazı değişiklik varsayalım *veri\_alımı* dal için yerel makinenizde dalın bir R dosyası ekleme gibi iş öğesi. Aşağıdaki Git komutlarını kullanarak Git kabuğunuzda dalında Gerçekleştirdiğim olması koşuluyla bu iş öğesi için dala eklenen R dosya onaylayabilirsiniz:

    git status
    git add .
    git commit -m"added a R scripts"
    git push origin data_ingestion

![5](./media/collaborative-coding-with-git/5-sprint-push-to-branch.png)

## 3. <a name='CreateapullrequestonVSTS-3'></a>Azure DevOps Services'de bir çekme isteği oluşturun 

Birkaç işlemeler ve bildirim sonra hazır olduğunuzda geçerli dal, ana dal ile birleştirmek için gönderebilmek için bir **çekme isteği** Azure DevOps Hizmetleri. 

Projenizin ana sayfasına gidin ve tıklayın **kod**. Birleştirilecek dalı ve dal ile birleştirmek istediğiniz Git depo adı seçin. Ardından **çekme istekleri**, tıklayın **yeni çekme isteği** çalışma dalı, ana dala birleştirilmeden önce bir çekme isteği gözden geçirme oluşturmak için.

![6](./media/collaborative-coding-with-git/6-spring-create-pull-request.png)

Bu çekme isteği hakkında bir açıklama girin, gözden geçirenler ekleme ve bunu gönderin.

![7](./media/collaborative-coding-with-git/7-spring-send-pull-request.png)

## 4. <a name='ReviewandMerge-4'></a>Gözden geçirme ve birleştirme 

Çekme isteği oluşturulurken, gözden geçirenler çekme isteklerini gözden geçirmek için bir e-posta bildirimi alın. Gözden geçirenler değişiklikleri veya çalışma ve istekte bulunan taraf ile değişiklikleri mümkünse test etmek gerekir. Gözden geçirenler, kendi değerlendirmesini temel alan, onaylayın veya çekme isteği reddedin. 

![8](./media/collaborative-coding-with-git/8-add_comments.png)

![9](./media/collaborative-coding-with-git/9-spring-approve-pullrequest.png)

Gözden geçirme tamamlandıktan sonra çalışma dalı, ana dala tıklayarak birleştirilir **tam** düğmesi. Birleştirilmiş olmadığı sonra çalışma dalı silmek isteyebilirsiniz. 

![10](./media/collaborative-coding-with-git/10-spring-complete-pullrequest.png)

İstek olarak işaretlenmiş sol üst köşedeki doğrulayın **tamamlandı**. 

![11](./media/collaborative-coding-with-git/11-spring-merge-pullrequest.png)

Ne zaman, geri giderek depoya altında **kod**, ana dala değiştirmiş söyledik.

![12](./media/collaborative-coding-with-git/12-spring-branch-deleted.png)

Ayrıca, çalışma dalınızı kendi ana dalına birleştirmek ve çalışma dalı Birleştirmeden sonra silmek için aşağıdaki Git komutlarını da kullanabilirsiniz:

    git checkout master
    git merge data_ingestion
    git branch -d data_ingestion

![13](./media/collaborative-coding-with-git/13-spring-branch-deleted-commandline.png)


 
## <a name="next-steps"></a>Sonraki adımlar

[Veri bilimi görevleri yürütme](execute-data-science-tasks.md) etkileşimli veri keşfi, veri analizi, raporlama ve modeli oluşturma gibi çeşitli genel veri bilimi görevlerini tamamlamak için yardımcı programlar kullanmayı gösterir.

İşlem için tüm adımları gösteren talimatlara **belirli senaryoları** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) makalesi. Bunlar, bulut, şirket içi araçları ve Hizmetleri, bir iş akışı veya akıllı bir uygulama oluşturmak için işlem hattı birleştirme işlemini göstermektedir. 

