---
title: Git - Azure Machine Learning ile işbirliği kodlama | Microsoft Docs
description: Çevik planlama ile Git kullanarak veri bilimi projeleri için işbirliği kod geliştirme nasıl.
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: deguhath
ms.openlocfilehash: abb1c7a3f597804a84f06462b1e50bb5a63fb9b3
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837370"
---
# <a name="collaborative-coding-with-git"></a>Git ile işbirliği içinde kodlama

Bu makalede işbirliği kod geliştirme paylaşılan kod geliştirme framework Git kullanarak veri bilimi projeleri için nasıl açıklanmaktadır. Bu etkinlikler, planlı işin kodlama bağlanma kapsayan [Çevik Geliştirme](agile-development.md) ve nasıl incelemeler kod.


## 1. <a name='Linkaworkitemwithagitbranch-1'></a>İş öğesi Git dal ile bağlantı 

VSTS Git dal ile bir iş öğesi (Öykü veya görev) bağlamak için kolay bir yol sağlar. Bu, doğrudan ilişkili kod hikayesi veya görev bağlamak üzere sağlar. 

Bir iş öğesi için yeni bir dalı bağlanmak için bir iş öğesini çift tıklatın ve açılır penceresinde **yeni bir dal oluşturma** altında **+ Ekle bağlantısına**.  

![1](./media/collaborative-coding-with-git/1-sprint-board-view.png)

Dal adı, temel Git deposu ve şube gibi bu dalı için bilgiler sağlar. Seçilen Git deposu depo iş öğesi ait aynı takım projesi altında olmalıdır. Temel şube ana dala veya başka bir mevcut şube olabilir.

![2](./media/collaborative-coding-with-git/2-create-a-branch.png)

Her hikayesi iş öğesi için bir Git dalı oluşturmak iyi bir uygulamadır. Ardından, her görevin çalışma öğesi için Öykü şubeyi temel alarak bir dal oluşturun. Öykü görev ilişkilerini karşılık gelen hiyerarşik bu şekilde dalları düzenleme aynı proje farklı öyküleri üzerinde çalışan birden çok kişilerin sahip veya aynı Öykü farklı görevler üzerinde çalışan birden çok kişinin sahip yararlıdır. Her ekip üyesi farklı dal ve her üye farklı kodları veya diğer yapıları bir dal paylaşırken çalışırken çalışırken çakışmaları en aza indirgenebilir. 

Aşağıdaki resimde TDSP için önerilen dallanma strateji gösterilmektedir. Burada, özellikle yalnızca biri varsa gösterildiği gibi veya aynı proje üzerinde çalışan iki kişinin çoğu dallandırır ya da konunun tüm görevlerinde yalnızca bir kişinin çalıştığı gibi gerekmeyebilir. Ancak ana daldan geliştirme şube ayırarak her zaman iyi bir uygulamadır. Bu, geliştirme etkinlikler tarafından kesintiye yayın dalı önlemeye yardımcı olabilir. Git şube modeli daha eksiksiz bir açıklaması bulunabilir [bir başarılı Git dallanma modeli](http://nvie.com/posts/a-successful-git-branching-model/).

![3](./media/collaborative-coding-with-git/3-git-branches.png)

Üzerinde çalışmak istediğiniz şube geçmek için bir kabuk komutu (Windows veya Linux) aşağıdaki komutu çalıştırın. 

    git checkout <branch name>

Değiştirme *< şube adı\>*  için **ana** anahtarları yedeklemek için **ana** dal. Çalışma dalı geçiş yaptıktan sonra bu iş öğesi üzerinde çalışan, öğe tamamlamak için gereken kodu veya belge yapıları geliştirmeye başlayabilirsiniz. 

Ayrıca varolan bir dal için bir iş öğesi bağlayabilirsiniz. İçinde **ayrıntı** tıklamak yerine bir iş öğesinin sayfa **yeni bir dal oluşturma**, tıklattığınız **+ Ekle bağlantısına**. Ardından, iş öğesine bağlamak istediğiniz dalı seçin. 

![4](./media/collaborative-coding-with-git/4-link-to-an-existing-branch.png)

Git Bash'te komutları yeni bir şube de oluşturabilirsiniz. Varsa < temel şube adı\> eksik, < yeni şube adı\> dayanır _ana_ dal. 
    
    git checkout -b <new branch name> <base branch name>


## 2. <a name='WorkonaBranchandCommittheChanges-2'></a>Bir dal üzerinde çalışır ve değişiklikleri uygulayın 

Şimdi, bazı değişiklik varsayalım *veri\_alım* yerel makinenizde dalı üzerinde bir R dosyası ekleme gibi iş öğesi için dal. Aşağıdaki Git komutları kullanarak bu dalda, Git Kabuğu'nda sağlanan dal bu iş öğesi için eklenen R dosya tamamlayabilir:

    git status
    git add .
    git commit -m"added a R scripts"
    git push origin data_ingestion

![5](./media/collaborative-coding-with-git/5-sprint-push-to-branch.png)

## 3. <a name='CreateapullrequestonVSTS-3'></a>VSTS üzerinde bir çekme isteği oluşturun 

Birkaç işlemeleri ve itmelerden sonra hazır olduğunuzda, ana dala, geçerli dalı birleştirmek için gönderebilmek için bir **çekme isteği** VSTS sunucusunda. 

Takım projenizin ana sayfasına gidin ve tıklayın **kod**. Birleştirilecek şube ve dala birleştirmek istediğiniz Git deposu adını seçin. Ardından **çekme istekleri**, tıklatın **yeni çekme isteği** iş dalı üzerinde temel dalının birleştirilmiş önce bir çekme isteği gözden geçirmesine oluşturmak için.

![6](./media/collaborative-coding-with-git/6-spring-create-pull-request.png)

Bu çekme isteğiyle ilgili bazı açıklamasını doldurun, gözden geçirenleri ekleme ve bunu göndermek.

![7](./media/collaborative-coding-with-git/7-spring-send-pull-request.png)

## 4. <a name='ReviewandMerge-4'></a>Gözden geçirme ve birleştirme 

Çekme isteği oluştururken, gözden geçirenler çekme istekleri gözden geçirmek için bir e-posta bildirimi alır. Gözden geçirenler değişiklikler veya çalışma ve istek sahibinin değişikliklerle mümkünse test denetlemeniz gerekir. Kendi değerlendirmesine göre gözden geçirenler onaylayın veya çekme isteği reddedin. 

![8](./media/collaborative-coding-with-git/8-add_comments.png)

![9](./media/collaborative-coding-with-git/9-spring-approve-pullrequest.png)

Gözden geçirme yapıldıktan sonra çalışma dalı ana dala tıklayarak birleştirilen **tam** düğmesi. Birleştirilmiş olmadığı sonra çalışma dalı silmek tercih edebilirsiniz. 

![10](./media/collaborative-coding-with-git/10-spring-complete-pullrequest.png)

İstek olarak işaretlenmiş sol üst köşede doğrulayın **tamamlandı**. 

![11](./media/collaborative-coding-with-git/11-spring-merge-pullrequest.png)

Ne zaman, Geri Git deponuza altında **kod**, ana dala değiştirmiş söylediniz.

![12](./media/collaborative-coding-with-git/12-spring-branch-deleted.png)

Ana dala çalışma dalı birleştirme ve birleştirme sonra çalışma dalı silmek için aşağıdaki Git komutlarını da kullanabilirsiniz:

    git checkout master
    git merge data_ingestion
    git branch -d data_ingestion

![13](./media/collaborative-coding-with-git/13-spring-branch-deleted-commandline.png)


 
## <a name="next-steps"></a>Sonraki adımlar

[Veri bilimi görevlerin yürütme](execute-data-science-tasks.md) yardımcı programları etkileşimli veri keşfi, veri analizi, raporlama ve model oluşturma gibi birçok ortak veri bilimi görevleri tamamlamak için nasıl kullanılacağını gösterir.

İşlem için tüm adımları gösteren talimatlara **belirli senaryolar** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) makalesi. Bunlar, bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl koruduğu gösterilmiştir. 

