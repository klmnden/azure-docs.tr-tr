---
title: Azure DevTest Labs altyapı İdaresi
description: Bu makalede, Azure DevTest Labs altyapısının İdaresi için yönergeler sağlar.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: e02400ef940efdf42370fbdc1da75bdc7062a8ef
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127370"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---company-policy-and-compliance"></a>Azure DevTest Labs altyapı - şirket ilke ve uyumluluk İdaresi
Bu makalede, şirket ilke ve uyumluluk için Azure DevTest Labs altyapı yöneten hakkında rehberlik sağlanır. 

## <a name="public-vs-private-artifact-repository"></a>Ortak ve özel yapıt deposu karşılaştırması

### <a name="question"></a>Soru
Ne zaman bir kuruluş, genel yapıt deposuna DevTest Labs özel yapıt deposunda karşılaştırması kullanmalıdır?

### <a name="answer"></a>Yanıt
[Genel yapıt deposuna](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) en yaygın olarak kullanılan yazılım paketleri başlangıç kümesi sağlar. Bu hızlı dağıtım ile zaman yatırım yapmanıza gerek kalmadan yaygın geliştirici araçları ve eklentileri oluşturmaya yardımcı olur. Kendi özel depoya dağıtmayı tercih edebilirsiniz. Genel ve özel bir depo paralel kullanabilirsiniz. Genel deponun devre dışı bırakmak seçebilirsiniz. Özel bir depo dağıtmak için ölçütleri hakkında önemli noktalar ve aşağıdaki soruları dikkate alınmalıdır:

- Kuruluşun kurumsal lisanslı yazılımı kendi DevTest Labs teklifi kapsamında olan bir gereksinimi var mı? Yanıt Evet ise, özel bir depo oluşturulmalıdır.
- Kuruluş genel sağlama sürecinin bir parçası olarak gerekli olan belirli bir işlemi sağlayan özel yazılım geliştirmek mu? Yanıt Evet ise, özel bir depo dağıtılmalıdır.
- Kuruluşunuzun idare İlkesi yalıtım gerektirir ve dış depolardaki kuruluş doğrudan yapılandırma yönetimi altında olmayan, bir özel yapıt deposu dağıtılmalıdır. Bu işlemin bir parçası, genel deponun bir başlangıç kopyasını kopyalanır ve özel depoya ile tümleşiktir. Böylece kimse kuruluş içinde artık erişebilir sonra genel deponun devre dışı bırakılabilir. Bu yaklaşım, kuruluş kapsamındaki tüm kullanıcılara yalnızca bir kuruluş tarafından onaylanan tek depo olmasını zorlar ve yapılandırma değişikliklerini en aza indirin.

### <a name="single-repository-or-multiple-repositories"></a>Tek depoda veya birden çok deposu 

### <a name="question"></a>Soru
Bir kuruluş için tek bir depoda plan veya birden çok deposu izin?

### <a name="answer"></a>Yanıt
Kuruluşunuzun genel idare ve yapılandırma yönetimi stratejisi kapsamında, merkezi bir havuz kullanmanızı öneririz. Birden çok deposu kullandığınızda, yönetilmeyen yazılım siloları zamanla durdurabilir. Merkezi bir depo ile birden çok takımı yapıtlar bu depodan kendi projeleri için kullanabilir. Standartlaştırma, güvenlik, yönetim kolaylığı zorlar ve çalışmalarınızı çoğaltma ortadan kaldırır. Merkezi bir parçası olarak, aşağıdaki eylemleri uygulamalar için uzun vadeli yönetimi ve sürdürülebilirlik önerilir:

- Azure aboneliği kimlik doğrulama ve yetkilendirme için kullandığı aynı Azure Active Directory kiracısı ile Azure depoları ilişkilendirin.
- Adlı bir grup oluşturun **tüm DevTest Labs geliştiriciler** merkezi olarak yönetilen Azure Active Directory'de. Yapıt katkıda herhangi bir geliştirici bu gruba yerleştirilmelidir.
- Aynı Azure Active Directory grubu, Azure depoları depoya ve Laboratuvar erişim sağlamak için kullanılabilir.
- Azure depolarda dallanma veya çatal ayrı bir de geliştirme depoya birincil üretim depodan kullanılmalıdır. İçerik yalnızca ana dala bir çekme isteği ile bir uygun kod gözden geçirdikten sonra eklenir. Kod Gözden Geçiren, değişikliği onayladığında, ana dalın bakım için sorumlu olan, müşteri adayı geliştirici, güncelleştirilmiş kod birleştirir. 

## <a name="corporate-security-policies"></a>Kurumsal güvenlik ilkeleri

### <a name="question"></a>Soru
Bir kuruluş Kurumsal güvenlik ilkeleri karşılandığından nasıl sağlayabilirsiniz?

### <a name="answer"></a>Yanıt
Bir kuruluş, aşağıdaki eylemleri gerçekleştirerek elde edebilirsiniz:

1. Geliştirme ve kapsamlı güvenlik ilkesi yayımlama. Kuralları kullanmayla ilişkili kabul edilebilir kullanım ilkesi korumadaki yazılımı, bulut varlıklar. Ayrıca, hangi açıkça ilkeyi ihlal tanımlar. 
2. Özel bir görüntü, özel yapıtlar ve active directory ile tanımlanan güvenlik bölgesi içinde düzenleme izin veren bir dağıtım işlemi geliştirin. Bu yaklaşım şirket sınırında zorlar ve ortak bir ortam denetimleri kümesini belirler. Geliştirin ve bunların genel işleminin bir parçası güvenli geliştirme yaşam döngüsü izleyin bu denetimleri ortama yönelik bir geliştirici olarak düşünebilirsiniz. Hedefi de aşırı kısıtlayıcı olmayan bir ortamda, Mayıs sağlamaktır geliştirme ancak makul bir dizi denetimi azaltabilir. Laboratuvar sanal makinesi içeren kuruluş biriminde (OU) grup ilkeleri, üretimde bulunan toplam grup ilkelerinin bir alt kümesi olabilir. Alternatif olarak, bunlar düzgün şekilde tanımlanan tüm riskleri azaltmak için ek bir kümesi olabilir.

## <a name="data-integrity"></a>Veri bütünlüğü

### <a name="question"></a>Soru
Bir kuruluş, uzak geliştiriciler kodu kaldıramaz veya kötü amaçlı yazılımlardan veya onaylanmamış tanıtmak emin olmak için veri bütünlüğü nasıl sağlayabilirsiniz?

### <a name="answer"></a>Yanıt
Denetimin dış danışmanların, yükleniciler ya da DevTest Labs'de işbirliği yapmak için uzaktan iletişim'de olan çalışanlar tehdidi azaltmak için birden fazla katman vardır. 

İlk adım, daha önce belirtildiği gibi birisi ilkeyi ihlal ettiğinde açıkça sonuçları özetler drafted ve tanımlanmış bir kabul edilebilir kullanım ilkesi olması gerekir. 

Uzaktan erişim için denetimlerin ilk katman laboratuvara doğrudan bağlı olmayan bir VPN bağlantısı üzerinden uzaktan erişim ilkesi uygulamaktır. 

İkinci Katman denetimlerin kopyalama önlemek ve Uzak Masaüstü aracılığıyla yapıştırın, Grup İlkesi nesneleri kümesini uygulamaktır. Ağ İlkesi, FTP gibi ortamından giden Hizmetleri ve ortamın dışında RDP Hizmetleri izin vermeyecek şekilde uygulanabilir. Kullanıcı tanımlı yönlendirme tüm Azure ağ trafiğini yeniden şirket içine zorlayabilir, ancak üretim içerik ve oturumları taramak için bir ara sunucu denetlenen sürece veri yüklemeye izin verebilir tüm URL'ler için hesaba değil. Genel IP'ler destekleyen bir dış ağ kaynağı köprüleme izin vermeyecek şekilde DevTest Labs sanal ağ içinde kısıtlı olabilir.

Sonuç olarak, aynı türde kısıtlamaları da çıkarılabilir medya içeriğinin bir gönderi kabul edebilecek dış URL'ler veya tüm olası yöntemlerin için hesap zorunda kuruluş çapında uygulanması gerekiyor. Gözden geçirin ve bir güvenlik ilkesi uygulamak için profesyonel güvenlik ile başvurun. Daha fazla öneri için bkz. [Microsoft siber güvenlik](https://www.microsoft.com/security/default.aspx?&WT.srch=1&wt.mc_id=AID623240_SEM_sNYnsZDs).


## <a name="next-steps"></a>Sonraki adımlar
Bkz: [uygulama geçiş ve tümleştirme](devtest-lab-guidance-governance-application-migration-integration.md).
