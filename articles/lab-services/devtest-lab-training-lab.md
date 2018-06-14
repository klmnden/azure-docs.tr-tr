---
title: Azure DevTest Labs için eğitim kullanın | Microsoft Docs
description: Eğitim senaryoları için Azure DevTest Labs kullanmayı öğrenin.
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
ms.openlocfilehash: 85eddaaf101c3e85eca7514b04660163d23c1c80
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788000"
---
# <a name="use-azure-devtest-labs-for-training"></a>Azure DevTest Labs için eğitim kullanın
Azure DevTest Labs, geliştirme ve test ek olarak birçok temel senaryolar uygulamak için kullanılabilir. Bu senaryolardan biri, eğitim için bir laboratuvar ayarlamaktır. Azure DevTest Labs, burada her Yardımcısı Eğitim aynı ve yalıtılmış ortamlar oluşturmak için kullanabileceğiniz özel şablonlar sağlayabilir Laboratuvar oluşturmanıza olanak sağlar. Yalnızca bunları ihtiyaç duydukları ve sanal makineleri için eğitim gerekli gibi-yeterli kaynak - içeren zaman eğitim ortamlar için her Yardımcısı kullanılabilir olduğundan emin olmak için ilkeleri uygulayabilirsiniz. Son olarak, tek bir tıklatmayla erişebilirsiniz trainees ile kolayca Laboratuvar paylaşabilirsiniz.

![DevTest Labs için eğitim kullanın](./media/devtest-lab-training-lab/devtest-lab-training.png)

Azure DevTest Labs, herhangi bir sanal ortam eğitim yürütmek için gerekli aşağıdaki gereksinimleri karşılayan: 

* Trainees diğer trainees tarafından oluşturulan VM'ler göremiyorum
* Her eğitim makine özdeş olmalıdır
* Trainees eğitim ortamlarının hızlı bir şekilde sağlayabilirsiniz
* Bunları kullanmıyorsanız eğitim ve kapatma VM'ler için de gereksinim olandan daha fazla VMs trainees alınamıyor sağlayarak maliyet denetimi
* Eğitim Laboratuvar her Yardımcısı ile kolayca paylaşın
* Eğitim Laboratuvar yeniden yeniden

Bu makalede, eğitim için laboratuvarı ayarlamanız için izleyebileceğiniz ayrıntılı adımlar ve daha önce açıklanan eğitim gereksinimlerini karşılamak için kullanılabilecek çeşitli Azure DevTest Labs özellikleri hakkında bilgi edinin.  

## <a name="implementing-training-with-azure-devtest-labs"></a>Azure DevTest Labs eğitime uygulama
1. **Laboratuvar oluşturma** 
   
    Labs Azure DevTest Labs başlangıç noktasıdır. Bir laboratuvar oluşturduktan sonra görevleri gerçekleştirebilirsiniz gibi kullanıcıların (trainees) Laboratuvar için ekledikçe maliyetlerini denetlemenize, hızlı bir şekilde oluşturabilirsiniz VM görüntüleri ve daha fazlasını tanımlamak için ilkeler ayarlama.   
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'de Laboratuvar oluşturma](devtest-lab-create-lab.md) |Azure portalında Azure DevTest Labs'de Laboratuvar oluşturma öğrenin. |
2. **Eğitim VM'ler hazır Market görüntülerini ve özel görüntüleri kullanarak dakikalar içinde oluşturma** 
   
    Azure Marketi'nde görüntüler çok geniş bir yelpazedeki hazır görüntülerden seçer ve laboratuvara trainees için kullanılabilir duruma getirmek. Hazır görüntüleri gereksinimlerinizi karşılamıyorsa, Laboratuvar Azure Laboratuvar özel görüntü olarak VM kaydetme ve eğitim için gereken tüm yazılım yükleme Marketi'nden hazır bir görüntü kullanarak VM oluşturarak özel bir görüntü oluşturabilirsiniz. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure Market görüntülerini yapılandırın](devtest-lab-configure-marketplace-images.md) |Beyaz liste Azure Market görüntülerini öğrenin; yalnızca görüntüleri seçim için kullanılabilir hale getirme için eğitim istiyor. |
   | [Özel bir görüntü oluşturun](devtest-lab-create-template.md) |Özel bir görüntü trainees hızlı bir şekilde özel görüntü kullanarak bir VM'i oluşturabilmeniz için eğitim gereken yazılımı yüklenerek oluşturun. |
3. **Eğitim makineler için yeniden kullanılabilir şablonlar oluşturma** 
   
    Azure DevTest Labs formülde bir VM oluşturmak için kullanılan varsayılan özellik değerleri listesidir. Görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağı seçerek laboratuvarda bir formül oluşturabilirsiniz. Her Yardımcısı Laboratuvar formülde görebilir ve bir VM oluşturmak için kullanın. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Sanal makineleri oluşturmak için DevTest Labs formüller yönetme](devtest-lab-manage-formulas.md) |Görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağ seçerek bir formül nasıl oluşturabileceğinizi öğrenin. |
4. **Denetim maliyetleri**
   
    Azure DevTest Labs laboratuvara Yardımcısı tarafından oluşturulan VM'ler en büyük sayısını belirtmek için laboratuvarda bir ilke ayarlamanıza olanak sağlar. 
   
    Birden çok gün eğitim gerçekleştirme ve tüm sanal makineleri günün belirli bir zamanda durdurun ve ardından bunları sonraki gün otomatik olarak yeniden istiyorsanız kolayca otomatik kapatma ayarlayarak bunu yapmaya ve otomatik başlatma Laboratuvar ilkeleri. 
   
    Eğitim tamamlandığında, son olarak, tüm sanal makineleri aynı anda tek bir PowerShell Betiği çalıştırarak silebilirsiniz. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md) |Laboratuar ortamında ilkeleri ayarlayarak maliyetleri denetler. |
   | [Tüm Laboratuvar bir PowerShell komut dosyası kullanarak sanal makineleri silin](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Eğitim tamamlandığında, tek bir işlemde tüm labs silin. |
5. **Laboratuvar her Yardımcısı ile paylaşma**
   
    Labs, trainees ile paylaştığınız bir bağlantıyı kullanarak doğrudan erişilebilir. Sahip oldukları sürece, trainees bile bir Azure hesabınızın olması gerekmez bir [Microsoft hesabı](devtest-lab-faq.md#what-is-a-microsoft-account). Trainees diğer trainees tarafından oluşturulan VM'ler göremezsiniz.  
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs laboratuarda bir Yardımcısı ekleyin](devtest-lab-add-devtest-user.md) |Eğitim Laboratuvarınızı trainees eklemek için Azure Portalı'nı kullanın. |
   | [Bir PowerShell Betiği kullanılarak Laboratuvar trainees Ekle](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Eğitim laboratuvarınız için ekleme trainees otomatikleştirmek için PowerShell kullanın. |
   | [Laboratuvar bağlantısını Al](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Nasıl bir laboratuvar doğrudan bir köprü erişilebilen öğrenin. |
6. **Laboratuvar yeniden yeniden** 
   
    Laboratuvar oluşturma, özel ayarlar bir Resource Manager şablonu oluşturma ve aynı labs yeniden oluşturmak için kullanma dahil olmak üzere otomatik hale getirebilirsiniz. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Resource Manager şablonu kullanarak bir laboratuvar oluşturma](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Labs Resource Manager şablonları kullanarak Azure DevTest Labs içinde oluşturun. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

