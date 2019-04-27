---
title: Eğitim için Azure DevTest Labs kullanma | Microsoft Docs
description: Eğitim senaryoları için Azure DevTest Labs'i kullanmayı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 0a7ce1640636c6fba246584d098043a91990b9a0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622856"
---
# <a name="use-azure-devtest-labs-for-training"></a>Eğitim için Azure DevTest Labs kullanma
Azure DevTest Labs, geliştirme/test yanı sıra birçok önemli senaryolar uygulamak için kullanılabilir. Bu senaryolardan biri, eğitim için laboratuvar ayarlamaktır. Azure DevTest Labs, burada her Yardımcısı Eğitim özdeş ve yalıtılmış ortamlar oluşturmak için kullanabileceğiniz özel şablonları sağlayabilir Laboratuvar oluşturmanıza olanak sağlar. Yalnızca bunlar ihtiyacınız ve eğitim için gereken sanal makineler gibi-yeterli kaynak - içeren eğitim ortamları her Yardımcısı için kullanılabilir olmasını sağlamak için ilkeler uygulayabilirsiniz. Son olarak, Laboratuvar, tek bir tıklamayla erişebilirsiniz yardımıyla ile kolayca paylaşabilirsiniz.

![Eğitim için DevTest Labs kullanma](./media/devtest-lab-training-lab/devtest-lab-training.png)

Azure DevTest Labs herhangi bir sanal ortam eğitim gerçekleştirmek için gerekli aşağıdaki gereksinimleri karşılar: 

* Diğer yardımıyla tarafından oluşturulan sanal makineler yardımıyla göremiyorum
* Her bir eğitim makinede aynı olmalıdır.
* Yardımıyla eğitim ortamlarını hızla sağlayabilir
* Yardımıyla bunları kullanmıyorsanız, eğitim ve kapatma sanal makineleri için Ayrıca bunlar gereksinim duyduğunuzdan daha fazla VM alınamıyor sağlayarak maliyet denetimi
* Eğitim Laboratuvar her Yardımcısı ile kolayca paylaşın
* Eğitim Laboratuvar tekrar tekrar tekrar

Bu makalede, eğitim için laboratuvar ayarlamak için izleyebileceğiniz ayrıntılı adımlar ve daha önce açıklandığı gibi eğitim gereksinimlerinizi karşılamak için kullanılabilecek çeşitli Azure DevTest Labs özellikler hakkında bilgi edinin.  

## <a name="implementing-training-with-azure-devtest-labs"></a>Azure DevTest Labs ile eğitim uygulama
1. **Laboratuvar oluşturma** 
   
    Labs Azure DevTest labs'deki başlangıç noktasıdır. Bir laboratuvarı oluşturduktan sonra görevleri gerçekleştirebileceğiniz gibi kullanıcı (yardımıyla) laboratuvara ekleme gibi hızlı bir şekilde oluşturabileceğiniz VM görüntüleri ve daha fazlasını tanımlayın, maliyetleri denetim yönelik ilkeler ayarlama.   
   
    Aşağıdaki tabloda bağlantıları tıklayarak daha fazla bilgi:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'de Laboratuvar oluşturma](devtest-lab-create-lab.md) |Azure portalında Azure DevTest Labs'de Laboratuvar oluşturma konusunda bilgi edinin. |
2. **Kullanıma hazır Market görüntüleri hem de özel görüntüleri kullanarak dakikalar içinde eğitim VM oluşturma** 
   
    Azure Market'te çok çeşitli görüntüleri hazır görüntülerini çekme ve Laboratuvardaki yardımıyla için kullanılabilir duruma getirmek. Kullanıma hazır görüntüler gereksinimlerinizi karşılamıyorsa, Laboratuvardaki özel görüntü olarak VM kaydetme ve eğitim için gereken tüm yazılım yükleme, Azure Market'te kullanıma hazır bir görüntüyü kullanarak VM'yi bir laboratuvara oluşturarak, özel bir görüntü oluşturabilirsiniz. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak daha fazla bilgi:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) |Beyaz liste Azure Market görüntüleri öğrenin; yalnızca görüntü seçimi için kullanılabilir hale getirme için eğitim istersiniz. |
   | [Özel görüntü oluşturma](devtest-lab-create-template.md) |Yardımıyla özel görüntüyü kullanarak VM'yi hızla oluşturabilmeniz için eğitim ihtiyacınız olan yazılımları önceden yükleyerek özel bir görüntü oluşturun. |
3. **Eğitim makineler için yeniden kullanılabilir şablonları oluşturma** 
   
    Azure DevTest labs'deki bir formül, bir VM oluşturmak için kullanılan varsayılan özellik değerleri listesidir. Görüntü, bir VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağ'ı seçerek, laboratuvarda bir formül oluşturabilirsiniz. Her Yardımcısı Laboratuvar formülü görebilir ve bir VM oluşturmak için bunu kullanın. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak daha fazla bilgi:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Sanal makineler oluşturmak için DevTest Labs formülleri yönetme](devtest-lab-manage-formulas.md) |Görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağ seçerek bir formülü nasıl oluşturacağınızı öğrenin. |
4. **Maliyetleri**
   
    Azure DevTest Labs Laboratuvardaki Laboratuvardaki bir Yardımcısı tarafından oluşturulan VM'ler en fazla sayısını belirtmek için bir ilke ayarlamanıza olanak tanır. 
   
    Birden çok günlük eğitim yürütmek ve günün belirli bir zamanda tüm sanal makineleri durdurmak ve ardından bunları sonraki gün otomatik olarak yeniden istiyor, kolayca otomatik kapatma ayarlayarak bunu yapmaya ve Laboratuvar ilkelerini otomatik olarak başlat. 
   
    Eğitim tamamlandığında Son olarak, tüm VM'ler aynı anda tek bir PowerShell betiğini çalıştırarak silebilirsiniz. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak daha fazla bilgi:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md) |Laboratuvar ilkeleri ayarlayarak maliyetleri denetleyin. |
   | [Tüm Laboratuvar bir PowerShell Betiği kullanarak Vm'leri Sil](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Alıştırma tamamlandıktan sonra tek bir işlemde tüm labs silin. |
5. **Laboratuvar her Yardımcısı ile paylaşma**
   
    Laboratuvarlar, yardımıyla ile paylaştığınız bir bağlantıyı kullanarak doğrudan erişilebilir. Sahip oldukları sürece, yardımıyla bile bir Azure hesabınızın olması gerekmez bir [Microsoft hesabı](devtest-lab-faq.md#what-is-a-microsoft-account). Yardımıyla diğer yardımıyla tarafından oluşturulan sanal makineler göremez.  
   
    Aşağıdaki tabloda bağlantıları tıklayarak daha fazla bilgi:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest labs'deki bir laboratuvara bir Yardımcısı ekleyin](devtest-lab-add-devtest-user.md) |Eğitim Laboratuvarınızı yardımıyla eklemek için Azure portalını kullanın. |
   | [Bir PowerShell betiğini kullanarak laboratuvara yardımıyla ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Eğitim laboratuvarınız için ekleme yardımıyla otomatikleştirmek için PowerShell kullanın. |
   | [Laboratuvar için bir bağlantı alma](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Nasıl bir laboratuvar doğrudan köprü erişilebilen öğrenin. |
6. **Laboratuvar tekrar tekrar tekrar** 
   
    Laboratuvar oluşturma, Resource Manager şablonu oluşturarak ve onu tekrar tekrar aynı Laboratuarları oluşturmak için kullanılarak özel ayarları da dahil olmak üzere otomatik hale getirebilirsiniz. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak daha fazla bilgi:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Resource Manager şablonu kullanarak Laboratuvar oluşturma](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Resource Manager şablonlarını kullanarak Azure DevTest Labs laboratuvarlar oluşturun. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

