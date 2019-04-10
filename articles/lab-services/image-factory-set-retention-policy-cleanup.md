---
title: Azure DevTest Labs'de bir görüntü fabrikası oluşturma | Microsoft Docs
description: Azure DevTest Labs'de bir özel görüntü fabrikası oluşturma hakkında bilgi edinin.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 8190c2043d7d3daae91c93fd3b66126d0941710b
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59361855"
---
# <a name="create-a-custom-image-factory-in-azure-devtest-labs"></a>Azure DevTest Labs'de bir özel görüntü fabrikası oluşturma
Bu makale, bir bekletme ilkesi ayarlamak, Fabrika temizleme ve kuruluşunuzdaki tüm diğer DevTest Labs'den eski görüntüleri devre dışı bırakma kapsar. 

## <a name="prerequisites"></a>Önkoşullar
Devam etmeden önce bu makaleler izlediğinizden emin olun:

- [Görüntü fabrikası oluşturma](image-factory-create.md)
- [Azure DevOps’tan bir görüntü fabrikası çalıştırma](image-factory-set-up-devops-lab.md)
- [Özel görüntüleri birden çok laboratuvara kaydetme ve dağıtma](image-factory-save-distribute-custom-images.md)

Aşağıdaki öğeler zaten koşulların karşılanması:

- Görüntü fabrikasının Azure DevTest labs'deki bir laboratuvara
- Azure DevTest Labs Fabrika altın görüntü burada dağıtacak bir veya daha fazla hedef
- Bir Azure DevOps projesi, görüntü Fabrika otomatik hale getirmek için kullanılır.
- Betikler ve yapılandırmada (Bizim örneğimizde, yukarıda kullanılan aynı DevOps projesi) içeren kaynak kodu konumu
- Azure Powershell görevleri otomatik düzende gerçekleştirmek için bir yapı tanımı
 
## <a name="setting-the-retention-policy"></a>Bekletme İlkesi ayarlama
Temizleme adımları yapılandırmadan önce DevTest Labs'de korumak istediğiniz kaç geçmiş görüntüleri tanımlayın. Ne zaman takip [Azure DevOps bir görüntü fabrikası çalıştırma](image-factory-set-up-devops-lab.md) makalesi, yapılandırdığınız çeşitli değişkenleri oluşturun. Bunlardan biri olan **ImageRetention**. Bu değişkeni ayarlamak `1`, DevTest Labs özel görüntülerin geçmişini sağlamayacak anlamına gelir. Yalnızca son dağıtılmış görüntüleri kullanılabilir. Bu değişkeni değiştirmeniz halinde `2`, en son görüntü dağıtılmış artı öncekilerle korunur. Bu değer, DevTest Labs'de korumak istediğiniz geçmiş görüntü sayısını tanımlamak için ayarlayabilirsiniz.

## <a name="cleaning-up-the-factory"></a>Fabrika temizleme
Fabrika temizleme ilk adımı, görüntü fabrikasından altın görüntü Vm'leri kaldırmaktır. Bizim önceki betikler gibi bu görevi gerçekleştirmek için bir komut yoktur. Başka bir tane eklemek için ilk adımıdır **Azure Powershell** aşağıdaki görüntüde gösterildiği gibi görev için derleme tanımı:

![PowerShell adım](./media/set-retention-policy-cleanup/powershell-step.png)

Listede yeni bir görev oluşturduktan sonra öğeyi seçin ve aşağıdaki görüntüde gösterildiği gibi tüm ayrıntıları girin:

![Eski görüntüleri PowerShell görev Temizle](./media/set-retention-policy-cleanup/configure-powershell-task.png)

Betik parametreleri: `-DevTestLabName $(devTestLabName)`.

## <a name="retire-old-images"></a>Eski görüntüleri devre dışı bırakma 
Bu görevi yalnızca geçmiş eşleşen tutarak herhangi bir eski görüntü kaldırır **ImageRetention** değişkeni oluşturun. Ek bir ekleme **Azure Powershell** bizim derleme tanımı için bir görev oluşturun. Eklendikten sonra görev seçin ve aşağıdaki görüntüde gösterildiği gibi ayrıntıları girin: 

![Eski görüntüleri PowerShell görevi devre dışı bırakma](./media/set-retention-policy-cleanup/retire-old-image-task.png)

Betik parametreleri şunlardır: `-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -SubscriptionId $(SubscriptionId) -DevTestLabName $(devTestLabName) -ImagesToSave $(ImageRetention)`

## <a name="queue-the-build"></a>Derlemeyi kuyruğa al
Derleme tanımı tamamladığınıza göre her şeyin çalıştığından emin olmak için yeni bir yapıyı kuyruğa alın. Derleme hedef Laboratuvardaki yeni özel görüntüleri gösterisini ve görüntü Fabrika Laboratuvar iade başarıyla tamamlandıktan sonra sağlanan VM görürsünüz. Ayrıca, daha fazla derlemeleri sıraya, eski özel görüntülere DevTest Labs uygun yapı değişkenlerinde ayarlanan bekletme değerine devre dışı bırakma, temizleme görevleri konusuna bakın.

> [!NOTE]
> Bu serideki son makalenin sonunda derleme işlem hattı çalıştırıldığında yeni yapıyı kuyruğa önce resim Fabrika laboratuvarda oluşturulan sanal makineleri el ile silin.  Her şeyi ayarlamak ve çalıştığını doğrulayın. elle temizleme adım yalnızca gereklidir.



## <a name="summary"></a>Özet
Artık oluşturabilir ve dağıtabilirsiniz laboratuvarlarınızı isteğe bağlı olarak özel görüntüleri çalışan bir görüntü üreteci var. Bu nokta, onun kendi görüntülerinizi alma adımlarından düzgün bir şekilde ayarlaması ve hedef labs tanımlayan. Önceki makalede belirtildiği gibi **Labs.json** bulunan dosyasını, **yapılandırma** klasörü hangi görüntüleri her hedef labs içinde yapılması gerektiğini belirtir. Diğer DevTest Labs kuruluşunuzda ekledikçe Labs.json yeni Laboratuvar için bir giriş eklemek yeterlidir.

Yeni bir görüntü factory'nize ekleyerek de basit bir işlemdir. Fabrikanızı açtığınız yeni bir görüntü eklemek istediğiniz zaman [Azure portalında](https://portal.azure.com)factory'nize DevTest Labs gidin, bir sanal makine eklemek için düğmeyi seçin ve istenen Market görüntüsü ve yapıtları seçin. Yerne **Oluştur** düğmesini seçin ve yeni VM sağlamak için **görünümü Azure Resource Manager şablonu**"ve şablon içinde herhangi bir .json dosyası olarak kaydedin **GoldenImages** deponuzda klasör. Görüntü fabrikanızı bir sonraki çalıştırmanızda özel görüntü oluşturun.


## <a name="next-steps"></a>Sonraki adımlar
1. [Derleme/yayın zamanlama](/azure/devops/pipelines/build/triggers?view=azure-devops&tabs=designer) görüntü Fabrika düzenli aralıklarla çalışacak. Bu, üretici tarafından oluşturulan görüntüleri düzenli aralıklarla yeniler.
2. Daha fazla altın görüntü fabrikanızın olun. Ayrıca düşünebilirsiniz [yapıtları oluşturma](devtest-lab-artifact-author.md) ek VM Kurulumu görevlerinizi parçalarını betik ve Fabrika görüntülerinizi yapıtları içerir.
4. Oluşturma bir [ayrı derleme/yayın](/azure/devops/pipelines/overview.md?view=azure-devops-2019) çalıştırılacak **DistributeImages** ayrı olarak komut dosyası. Labs.json için değişiklik ve tüm görüntüleri yeniden yeniden oluşturmak zorunda kalmadan hedef Laboratuvarları için kopyalanan görüntü alma, bu komut dosyasını çalıştırabilirsiniz.

