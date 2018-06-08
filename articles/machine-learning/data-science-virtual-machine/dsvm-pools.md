---
title: Veri bilimi sanal makine havuzları - Azure | Microsoft Docs
description: Takım için paylaşılan bir kaynak olarak veri bilimi VM havuzlarını dağıtma
keywords: derin öğrenme, AI, veri bilimi araçları, veri bilimi sanal makine, Jeo-uzamsal analytics, takım veri bilimi işlemi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: c7aab0435ecbd0aee57a15008ac0270159ec2eb3
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837091"
---
# <a name="creating-a-shared-pool-of-data-science-virtual-machines"></a>Paylaşılan bir havuz veri bilimi sanal makineler oluşturma

Bu makalede ekibi tarafından nasıl paylaşılan havuz veri bilimi sanal makineler (DSVM) kullanım için oluşturulabilir anlatılmaktadır. Paylaşım ve işbirliği kolaylaştırmanın ve vererek, daha iyi kaynak kullanımı, paylaşılan bir havuzu kullanmanın faydası olduğu DSVM kaynaklarınızı daha etkili bir şekilde yönetmek için BT. 

Birçok yolu ve DSVM havuzu oluşturmak için kullanılan farklı teknolojiler vardır.  Ana senaryolar aşağıda verilmiştir:

* Toplu işleme için havuzu
* Etkileşimli VM'ler için havuzu

## <a name="batch-processing-pool"></a>Toplu işleme havuzu
Çoğunlukla çevrimdışı bir toplu işlemde işlerini çalıştırmak için DSVM kuyruğunu ayarlamak istediğiniz sonra kullanabileceğiniz [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) hizmet veya [Azure Batch](https://docs.microsoft.com/azure/batch/). 

### <a name="azure-batch-ai"></a>Azure Batch AI
Azure Batch AI görüntülerinde biri olarak DSVM Ubuntu sürümünde desteklenir. Azure CLI veya Azure Batch AI küme oluşturacağınız Python SDK belirtebilirsiniz ```image``` parametre ve ayarlamak ```UbuntuDSVM```. Ne tür bir işlem düğümlerini istediğiniz seçebilirsiniz - GPU tabanlı örnekleri vs CPU yalnızca örnekler, CPU, bellek sayısı bir [VM örnekleri geniş seçim](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) Azure üzerinde kullanılabilir. Kullandığınızda, Ubuntu DSVM görüntünün toplu AI GPU tabanlı düğüm ile tüm gerekli GPU sürücüleri ve derin öğrenme çerçeveleri, toplu işlem düğümleri hazırlamak için önemli ölçüde zaman kazandırır önceden yüklenmiş. Aslında, bir Ubuntu DSVM etkileşimli olarak geliştiriyorsanız, toplu AI düğümlerin tam olarak aynı kurulumu ve yapılandırması olduğunu fark edeceksiniz. Bir toplu AI küme oluşturulduğunda genellikle, ayrıca tüm düğümler tarafından oluşturulmuş ve girdi ve çıktı veri yanı toplu iş kodu depolamak için kullanılan bir dosya paylaşımı oluşturmak / komut dosyası. 

Toplu AI kümenizi oluşturulduktan sonra çalıştırılacak iş göndermek için aynı CLI veya Python SDK'sını kullanabilirsiniz. Yalnızca toplu işlerini çalıştırmak için kullanılan zaman için ödeme yaparsınız. 

#### <a name="more-information"></a>Daha fazla bilgi
* Kullanarak adım adım [Azure CLI](https://docs.microsoft.com/azure/batch-ai/quickstart-cli) toplu AI yönetmek için
* Kullanarak adım adım [Python](https://docs.microsoft.com/azure/batch-ai/quickstart-python) toplu AI yönetmek için
* [Toplu AI tarif](https://github.com/Azure/BatchAI) çeşitli AI/derin öğrenme çerçeveleri toplu AI ile kullanmak nasıl kullanılabilir.

## <a name="interactive-vm-pool"></a>Etkileşimli VM havuzu

Tüm AI tarafından paylaşılan etkileşimli DSVMs havuzu / veri bilimi ekibi her kullanıcı kümesi için adanmış bir örneği olması yerine DSVM kullanılabilir bir örneğini oturum açmak kullanıcıların sağlar. Bu, daha iyi kullanılabilirlik ve kaynakları daha etkili kullanımı ile yardımcı olur. 

Etkileşimli bir VM havuzu oluşturmak için kullanılan teknolojidir [Azure sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) (VMSS) olanak sağlayan, aynı grup, Yük Dengeleme ve otomatik ölçeklendirmeyi VM'ler oluşturun ve yönetin. Kullanıcının oturum açtığı ana havuzunun IP veya DNS adresi. Ölçek oturum yollar ölçek kümesindeki bir kullanılabilir DSVM için otomatik olarak ayarlayın. Kullanıcı içine oturum benzer ortamı VM yedeklemiş istediğiniz olduğundan, VM ölçek kümesindeki tüm örneklerini paylaşılan bir ağ sürücüsündeki bir Azure dosyaları veya bir NFS paylaşımına gibi bağlar. Kullanıcının paylaşılan çalışma alanı, normalde her örnekleri takılı paylaşılan dosya deposu tutulur. 

Ubuntu DSVMs örnekleriyle oluşturan bir VM ölçek kümesi örnek Azure Resource Manager şablonu bulunabilir [github](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json). Azure Resource Manager şablonu örneği [parametre dosyası](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) aynı konumda de sağlanır. 

Azure Resource Manager şablonu Azure CLI kullanarak parametre dosyası için uygun değerleri ayarlamak VM ölçek oluşturabilirsiniz. 

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
Yukarıdaki komutlarda VM ölçek kümesi VM örneği sayısı, her bir VM bağlanacaktır depolama hesabı için kimlik bilgileri ile birlikte, Azure dosyalarınızı işaretçiler Örneğiniz için belirtilen değerlerle parametre dosyasının bir kopyası olduğunu varsayın. Parametre dosyasını yerel olarak yukarıdaki komutu başvurulmaktadır. Ayrıca, parametreleri satır içi ya da bunlar için komut isteminde geçirebilirsiniz.  

Yukarıdaki şablonu SSH sağlar ve Ubuntu DSVMs arka uç havuzuna Jupyterhub bağlantı VM ölçek ön uç noktasından ayarlayın.  Bir kullanıcı olarak, yalnızca VM SSH veya JupyterHub normal şekilde oturum açın. VM örnekleri genişletilebilir veya aşağı dinamik olarak herhangi bir durum olması gereken beri bağlı kaydedilen Azure dosyaları paylaşır. Aynı yaklaşımı Windows DSVMs havuzu oluşturmak için kullanılabilir. 

[Azure dosyaları bağlar betik](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh) de Azure DataScienceVM Github'da bulunmaktadır. Azure dosyaları parametre dosyasında belirtilen bağlama noktası oluşturma, ek olarak da ilk kullanıcının giriş dizininde bağlı sürücüye ek yazılım bağlantılarını oluşturur ve bir kullanıcıya özgü dizüstü dizin paylaşılan Azure dosyaları içinde yazılım bağlı ```$HOME/notebooks/remote``` erişim, çalıştırmak ve bunların Jupyter not defterleri kaydetmek kullanıcı etkinleştirme dizin.  Her kullanıcının Jupyter çalışma paylaşılan Azure dosyaları'na işaret etmek için VM üzerinde ek kullanıcılar oluşturduğunuzda, aynı kurala kullanılabilir. 

Azure VM ölçek kuralları ek örnekleri oluşturmak ne zaman ve sanal makineleri hiç kullanılmadığı zaman hangi koşullarda kaydetmek için sıfır örnekler aşağıya doğru getirilmesi dahil olmak üzere örnekleri ölçeğini donanım kullanım maliyetleri bulut altında ayarlayabileceğiniz destek otomatik ölçeklendirmeyi ayarlar. . Ayrıntılı adımlar için VM ölçek kümesi belge sayfalarının sağlamak [otomatik ölçeklendirme](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Sonraki adımlar

* [Ortak kimlik ayarlayın](dsvm-common-identity.md)
* [Bulut kaynaklarına erişmek için kimlik bilgilerini güvenli bir şekilde saklayın](dsvm-secure-access-keys.md)















