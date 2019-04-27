---
title: Sınıf Laboratuvarlarını eğitimleri - Azure Lab Services'i kullanın. | Microsoft Docs
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
ms.date: 02/22/2019
ms.author: spelluru
ms.openlocfilehash: 4d2ba11181977f1976b5ae933e8b93a92424fa96
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60695288"
---
# <a name="use-classroom-labs-for-trainings"></a>Sınıf Laboratuvarlarını eğitimleri için kullanın.
Bir laboratuvar eğitimleri için ayarlayabilirsiniz. Azure Lab Services'ın classroom Labs burada her Yardımcısı aynı ve yalıtılmış ortamlar için eğitim kullanır, eğitim için bir laboratuvar oluşturmanıza imkan tanır. Yalnızca bunlar ihtiyacınız ve eğitim için gereken sanal makineler gibi-yeterli kaynak - içeren eğitim ortamları her Yardımcısı için kullanılabilir olmasını sağlamak için ilkeler uygulayabilirsiniz. 

![Classroom lab](../media/classroom-labs-scenarios/classroom.png)

Sınıf Laboratuvarlarını herhangi bir sanal ortam eğitim gerçekleştirmek için gerekli aşağıdaki gereksinimleri karşılar: 

- Yardımıyla eğitim ortamlarını hızla sağlayabilir
- Her bir eğitim makinede aynı olmalıdır.
- Diğer yardımıyla tarafından oluşturulan sanal makineler yardımıyla göremiyorum
- Yardımıyla bunları kullanmıyorsanız, eğitim ve kapatma sanal makineleri için Ayrıca bunlar gereksinim duyduğunuzdan daha fazla VM alınamıyor sağlayarak maliyet denetimi
- Eğitim Laboratuvar her Yardımcısı ile kolayca paylaşın
- Eğitim Laboratuvar tekrar tekrar tekrar

Bu makalede, eğitim için laboratuvar ayarlamak için izleyebileceğiniz ayrıntılı adımlar ve daha önce açıklandığı gibi eğitim gereksinimlerinizi karşılamak için kullanılabilecek çeşitli Azure Lab Services özellikleri hakkında bilgi edinin.  

## <a name="create-the-lab-account-as-a-lab-account-administrator"></a>Bir laboratuvar hesabı yönetici olarak bir laboratuvar hesabı oluşturma
Azure Lab Services'i kullanmanın ilk adımı, Azure portalında bir laboratuvar hesabı oluşturmaktır. Bir laboratuvar hesabı yöneticisi laboratuar hesabı oluşturduktan sonra Yönetim için Laboratuarları oluşturmak isteyen kullanıcıların ekler **Laboratuvar oluşturan** rol. Eğitmenler, Öğrenciler, bunlar Ders kursa alıştırmaları yapmak için sanal makinelerle laboratuvarlar oluşturun. Ayrıntılar için bkz [oluşturun ve Laboratuvar hesabını yönetme](how-to-manage-lab-accounts.md).

## <a name="create-and-manage-classroom-labs"></a>Oluşturma ve sınıf laboratuvarlarını yönetme
Bir laboratuvar hesabı Laboratuvar oluşturan rolünün bir üyesi olan bir eğitmen bir veya daha fazla labs Laboratuvar hesabı oluşturabilirsiniz. Oluşturur ve kursunuzun alıştırmalarda yapmak için gerekli tüm yazılım ile bir VM şablonu yapılandırma. Bir sınıf laboratuvarı oluşturmak için kullanılabilir görüntülerden kullanıma hazır bir görüntü seçin ve sonra Laboratuvar için gerekli yazılımı yükleyerek özelleştirin. Ayrıntılar için bkz [oluştur ve sınıf laboratuvarlarını yönetme](how-to-manage-classroom-labs.md).

## <a name="configure-usage-settings-and-policies"></a>Kullanım ayarları ve ilkeleri yapılandırma
Laboratuvar oluşturan eklemek veya kaldırmak Laboratuvar kullanıcılara, kullanıcı, tek tek kotalar ayarını güncelleştirme Laboratuvar ve daha fazla sanal makine sayısı gibi ilkeleri ayarlama, Laboratuvar kullanıcılara göndermek için kayıt bağlantısını alın. Ayrıntılar için bkz [kullanım ayarları ve ilkeleri yapılandırma](how-to-configure-student-usage.md).

## <a name="create-and-manage-schedules"></a>Zamanlamaları oluşturma ve yönetme
Zamanlamaları bir sınıf laboratuvarına Vm'leri Laboratuvardaki otomatik olarak başlatın ve belirli bir zamanda kapatma şekilde yapılandırmanıza olanak sağlar. Tek seferlik zamanlama veya yinelenen bir zamanlama tanımlayabilirsiniz. Ayrıntılar için bkz [oluştur ve zamanlamalar için sınıf laboratuvarlarını yönetme](how-to-create-schedules.md).

## <a name="set-up-and-publish-a-template-vm"></a>Ayarlama ve VM şablon yayımlama
Laboratuvardaki şablon, tüm kullanıcıların sanal makinelerinin oluşturulduğu bir temel sanal makine görüntüsüdür. VM şablonu, tam olarak eğitim katılımcılarına sağlamak istediğiniz ile yapılandırıldığı şekilde ayarlayın. Laboratuvar kullanıcılarının görebileceği bir ad ve açıklama belirtebilirsiniz. Ardından, VM şablonu örneklerini Laboratuvar kullanıcılarınız için kullanılabilir hale getirmek için şablonu yayımlayın. Bir şablonu yayımladığınızda Azure Lab Services, şablonu kullanarak laboratuvarda sanal makineler oluşturur. Bu işlemde oluşturulan sanal makine sayısı, laboratuvarda izin verilen maksimum kullanıcı sayısıyla aynıdır. Laboratuvarın kullanım ilkesinde bu maksimum değeri ayarlayabilirsiniz. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir. Ayrıntılar için bkz [kümesi ayarlama ve sanal makine şablonu yayımlama](how-to-create-manage-template.md). 

## <a name="use-vms-in-the-classroom-lab"></a>Vm'leri classroom laboratuvarda kullanma
Bir öğrenci veya eğitim katılımcı laboratuvara kaydeder ve kursa alıştırmaları yapmak için VM bağlanır. Ayrıntılar için bkz [nasıl bir sınıf laboratuvarına erişim](how-to-use-classroom-lab.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makaledeki yönergeleri izleyerek Classroom Labs'de bir laboratuvar hesabı oluşturmayla başlatın: [Öğretici: Azure Lab Services ile bir laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md).