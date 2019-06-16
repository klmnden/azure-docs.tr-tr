---
title: Azure DevTest Labs'de olarak bir-hizmet Platform (PaaS) kullanmanıza | Microsoft Docs
description: Hizmet olarak Platform-a-(geçiş) hizmetlerini Azure DevTest Labs'de nasıl kullanacağınızı öğrenin.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2019
ms.author: spelluru
ms.openlocfilehash: 865ae0b3f7a7965698a67183a4c820ba71f49cd8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65833922"
---
# <a name="use-platform-as-a-service-paas-services-in-azure-devtest-labs"></a>Azure DevTest Labs'de olarak bir-hizmet Platform (PaaS) Hizmetleri kullanın
PaaS, DevTest Labs'de ortamları özelliği aracılığıyla desteklenir. DevTest Labs ortamlarda bir Git deposunda önceden yapılandırılmış Azure Resource Manager şablonları tarafından desteklenir. Ortamlar hem PaaS ve Iaas kaynakları içerebilir. Sanal makineler, veritabanları, sanal ağlar gibi Azure kaynaklarını içeren karmaşık sistemleri ve birlikte çalışmak için özelleştirilmiş Web uygulamaları oluşturmanıza olanak sağlar. Bu şablonlar, tutarlı dağıtım ve kaynak kodu denetimi kullanarak ortamlar geliştirilmiş yönetimi sağlar. 

Bir sistemi düzgün bir şekilde ayarlamak, aşağıdaki senaryolar sağlar: 

- Geliştiricilerin bağımsız ve birden çok ortama sahip
- Farklı yapılandırmalarda zaman uyumsuz olarak test etme
- Hazırlama ve üretim işlem hatları şablonu herhangi bir değişiklik yapmadan tümleştirme
- Hem geliştirme makineleri hem de aynı Laboratuvar ortamlarında olması, yönetim ve maliyet raporlama kolaylığı artırır.  

Bu yüzden PaaS kaynaklarına kullanımını etkinleştirmek için laboratuvar kullanıcıya verilecek hiçbir ek izinler gerekir DevTest Labs kaynak sağlayıcısı, Laboratuvar kullanıcı adına kaynakları oluşturur. Aşağıdaki görüntüde, laboratuvar ortamında olarak bir Service Fabric kümesi gösterilmektedir.

![Bir ortamı olarak Service Fabric kümesi](./media/create-environment-service-fabric-cluster/cluster-created.png)

Ortamları ayarlama hakkında daha fazla bilgi için bkz [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md). DevTest Labs kendiniz bir dış GitHub kaynağına bağlanmak zorunda kalmadan ortamlar oluşturmak için kullanabileceğiniz bir Azure Resource Manager şablonları, ortak bir depoya sahip. Ortak ortamlardaki hakkında daha fazla bilgi için bkz. [Azure DevTest labs'deki yapılandırma ve kullanım ortak ortamlardaki](devtest-lab-configure-use-public-environments.md).

Büyük kuruluşlarda, geliştirme takımları, genellikle özelleştirilmiş ve yalıtılmış bir test ortamları gibi ortamları sağlar. Bir iş birimi veya bölüm içindeki tüm takımlar tarafından kullanılacak olan ortamlar olabilir. BT grubu, kuruluş içindeki tüm takımlar tarafından kullanılabilecek kendi ortamlarını sağlamak isteyebilirsiniz.  

## <a name="customizations"></a>Özelleştirmeleri

#### <a name="sandbox"></a>Korumalı alan 
Laboratuvar sahibi kullanıcı rolünden değiştirmek için laboratuvar ortamlarını özelleştirebilirsiniz **okuyucu** için **katkıda bulunan** kaynak grubu içinde. Bu özellik bulunduğu **Laboratuvar ayarları** altındaki **yapılandırması ve ilkelerini** laboratuvarı. Bu değişikliğe rol ortamdaki kaynak ekleme veya kaldırma izin verir. Erişimi daha da kısıtlamak istiyorsanız, Azure ilkelerini kullanın. Bu işlev kaynakları veya abonelik düzeyinde erişimi olmayan yapılandırma özelleştirmenizi sağlar.

#### <a name="custom-tokens"></a>Özel belirteçler
Kaynak grubu dışında ve şablona erişebilmesi ortamlar için belirli bazı özel Laboratuvar bilgisi yoktur. Bunlardan bazıları şunlardır: 

- Laboratuvar Ağ Kimliği
- Location
- Resource Manager Şablonları dosyalarının depolandığı depolama hesabı. 
 
#### <a name="lab-virtual-network"></a>Laboratuvar sanal ağ
[Ortamlarını Laboratuvar sanal ağa bağlama](connect-environment-lab-virtual-network.md) makalede kullanmak için Resource Manager şablonu nasıl değiştireceğiniz `$(LabSubnetId)` belirteci. Bir ortam oluşturulduğunda `$(LabSubnetId)` belirteci ilk alt ağ işareti yerine burada **sanal makinesinde kullanımı oluşturma** seçeneği **true**. Daha önce oluşturulan ağlarının ortamımızda kullanmayı sağlar. Hazırlama ve üretim aynı Resource Manager şablonları test ortamlarında kullanmak istiyorsanız, kullanın `$(LabSubnetId)` Resource Manager şablon parametresi varsayılan değer olarak. 

#### <a name="environment-storage-account"></a>Ortamı depolama hesabı
DevTest Labs kullanılmasını desteklediği [Resource Manager şablonları iç içe geçmiş](../azure-resource-manager/resource-group-linked-templates.md). [[Dağıtmak için test ortamlarında iç içe geçmiş Azure Resource Manager şablonları](deploy-nested-template-environments.md) makalesi, nasıl kullanılacağını açıklar `_artifactsLocation` ve `_artifactsLocationSasToken` belirteçleri aynı klasörde veya iç içe bir Resource Manager şablonu için bir URI oluşturmak için ana şablon klasörü. Bu iki belirteçleri hakkında daha fazla bilgi için bkz: **dağıtım yapıtları** bölümünü [Azure Resource Manager – en iyi uygulamalar Kılavuzu](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md).

## <a name="user-experience"></a>Kullanıcı deneyimi

## <a name="developer"></a>Geliştirici
Geliştiriciler belirli bir ortam oluşturmak için bir VM oluşturmak için aynı iş akışını kullanın. Bunlar makine görüntüsü ve bir ortam seçin ve şablon tarafından gereken bilgileri girin. Her geliştirici bir ortama sahip dağıtımı değişikliklerini ve geliştirilmiş iç döngü hata ayıklama için izin verir. Ortam en son şablonu kullanarak herhangi bir zamanda oluşturulabilir.  Bu özelliği yok ve el ile sistem oluşturma veya hata testing'e kurtarma gerek kalmamasını kapalı kalma süresinin azaltılmasına yardımcı olmak için yeniden için ortamları sağlar.  

### <a name="testing"></a>Test Etme
DevTest Labs ortamları bağımsız belirli kod ve yapılandırmaları zaman uyumsuz olarak test etmek için izin verin. Çekme isteğinden kodla ortamı ayarlama ve otomatik bir testi başlatmak için yaygın uygulamadır bakın. Otomatikleştirilmiş test çalıştırılıp tamamlandıktan sonra herhangi bir el ile test ortamınıza karşı gerçekleştirilebilir. Bu işlem genellikle CI/CD işlem hattının bir parçası yapılır. 

## <a name="management-experience"></a>Yönetim deneyimi

### <a name="cost-tracking"></a>Maliyet İzlemesi
Maliyet izleme özelliği, genel bir parçası olarak farklı ortamlar içinde Azure kaynaklarını içerir maliyet eğilimi. Kaynaklara göre maliyeti ortamda farklı kaynaklar dışarı kesmeyecektir ancak ortam tek bir maliyet görüntüler.

### <a name="security"></a>Güvenlik
DevTest Labs ile düzgün bir şekilde yapılandırılmış bir Azure aboneliği için [yalnızca Laboratuvar aracılığıyla Azure kaynaklarına erişimi](devtest-lab-add-devtest-user.md). Ortamlar ile Laboratuvar sahibi kullanıcıların diğer tüm Azure kaynakları için erişim vermeden onaylı yapılandırmaları ile PaaS kaynaklarına erişmesine izin verebilirsiniz. Laboratuvar kullanıcıları ortamlar nerede özelleştirme senaryosunda, Laboratuvar sahibi, katkıda bulunan erişimine izin verebilirsiniz. Katkıda bulunan erişimi Laboratuvar kullanıcının yönetilen kaynak grubu içinde yalnızca Azure kaynak Ekle veya Kaldır izin verir. Kolay izleme için sağlar ve karşı yönetim abonelik kullanıcı katkıda bulunan erişimi sağlar.

### <a name="automation"></a>Otomasyon
Otomasyon, büyük bir ölçekte, etkin olarak ekosistemine için temel bir bileşenidir. Otomasyon yönetme ya da birden fazla ortam arasında abonelikler ve Laboratuvarları izleme işlemek gereklidir.

### <a name="cicd-pipeline"></a>CI/CD işlem hattı
DevTest Labs PaaS Hizmetleri, erişim Laboratuvar tarafından denetlenen burada dağıtımları odaklanmış tarafından CI/CD işlem hattı artırmaya yardımcı olabilir.

## <a name="next-steps"></a>Sonraki adımlar
Ortamlar hakkında ayrıntılı bilgi için aşağıdaki makalelere bakın: 

- 
- [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md)
- [Yapılandırma ve Azure DevTest Labs'de ortak ortamlardaki kullanma](devtest-lab-configure-use-public-environments.md)
- [Azure DevTest labs'deki müstakil Service Fabric kümesi ile bir ortam oluşturun](create-environment-service-fabric-cluster.md)
- [Azure DevTest Labs'de sanal ağ, Laboratuvar için bir ortama bağlanın](connect-environment-lab-virtual-network.md)
- [Ortamları, Azure DevOps CI/CD işlem hatlarıyla tümleştirin](integrate-environments-devops-pipeline.md)
 





