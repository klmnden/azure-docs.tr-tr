---
title: Veri bilimi sanal makine havuzları - Azure | Microsoft Docs
description: Bir takım için paylaşılan bir kaynak olarak veri bilimi VM'ler havuzlarını dağıtma
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
ms.openlocfilehash: 0740ff7542d066442146b8e80e188ad5ba49a2b5
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309407"
---
# <a name="create-a-shared-pool-of-data-science-virtual-machines"></a>Paylaşılan bir havuzu veri bilimi sanal makineler oluşturun

Bu makalede, paylaşılan bir havuz, veri bilimi sanal makineleri kullanır (DSVMs) ekibin nasıl oluşturabileceğiniz açıklanmaktadır. Paylaşılan bir havuzu kullanmanın yararları daha iyi kaynak kullanımı, paylaşım ve işbirliği dönüştüğünde, yönetim ve daha etkili yönetim DSVM kaynakların ' dir. 

DSVMs havuzu oluşturmak için birçok yöntem ve teknolojileri kullanabilirsiniz. Bu makalede, toplu işleme ve etkileşimli VM'ler için havuzları odaklanır.

## <a name="batch-processing-pool"></a>Toplu işlem havuzu
Çoğunlukla çevrimdışı bir toplu işlemde işlerini çalıştırmak için DSVMs kuyruğunu ayarlamak istiyorsanız kullanabileceğiniz [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) veya [Azure Batch](https://docs.microsoft.com/azure/batch/) hizmet. Bu makale Azure Batch AI odaklanır.

Azure Batch AI görüntülerinde biri olarak DSVM Ubuntu sürümünde desteklenir. Azure CLI veya Python oluşturduğunuz Azure Batch AI küme, SDK'sı belirtebilirsiniz `image` parametre ve ayarlamak `UbuntuDSVM`. Ne tür bir işlem düğümlerini istediğiniz seçebilirsiniz: GPU tabanlı örnekleri yalnızca CPU örnekler, CPU ve bellek sayısı yerine bir [VM örnekleri geniş seçim](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) Azure üzerinde kullanılabilir. 

GPU tabanlı düğümleriyle toplu AI Ubuntu DSVM görüntüsünü kullandığınızda, tüm gerekli GPU sürücüleri ve çerçeveleri öğrenme derin önceden. Önkurulum toplu işlem düğümleri hazırlamak için önemli ölçüde zaman kazandırır. Aslında, bir Ubuntu DSVM etkileşimli olarak geliştiriyorsanız, toplu AI düğümlerin tam olarak aynı kurulumu ve yapılandırması olduğunu fark edeceksiniz. 

Genellikle toplu AI kümesi oluşturduğunuzda, aynı zamanda tüm düğümler tarafından oluşturulmuş bir dosya paylaşımı oluşturun. Dosya Paylaşımı, girdi ve çıktı veri, yanı toplu iş kodu/komut dosyalarını depolamak için kullanılır. 

Bir toplu AI küme oluşturduktan sonra çalıştırılacak iş göndermek için aynı CLI veya Python SDK'sını kullanabilirsiniz. Yalnızca toplu işlerini çalıştırmak için kullanılan zaman için ödeme yaparsınız. 

Daha fazla bilgi için bkz.
* Kullanarak adım adım [Azure CLI](https://docs.microsoft.com/azure/batch-ai/quickstart-cli) toplu AI yönetmek için
* Kullanarak adım adım [Python](https://docs.microsoft.com/azure/batch-ai/quickstart-python) toplu AI yönetmek için
* [Toplu AI tarif](https://github.com/Azure/BatchAI) çeşitli AI ve toplu AI çerçeveleri öğrenme derin nasıl kullanılacağı gösterilmektedir

## <a name="interactive-vm-pool"></a>Etkileşimli VM havuzu

Tüm AI/veri bilimi ekibi tarafından paylaşılan etkileşimli VM'ler havuzu kullanıcıların her kullanıcı kümesi için adanmış bir örneği olması yerine DSVM kullanılabilir bir örneğini oturum olanak tanır. Bu kurulumu daha iyi kullanılabilirlik ve kaynakları daha etkili kullanımı ile yardımcı olur. 

Etkileşimli bir VM havuzu oluşturmak için kullandığınız teknolojidir [Azure sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). Ölçek kümeleri aynı, yük dengeli bir grup ve otomatik ölçeklendirmeyi VM'ler oluşturmak ve yönetmek için kullanabilirsiniz. 

Kullanıcı ana havuzunun IP veya DNS adresine bağlanır. Ölçek oturum yollar ölçek kümesindeki bir kullanılabilir DSVM için otomatik olarak ayarlayın. Kullanıcıların VM bakılmaksızın benzer bir ortam istediğiniz çünkü bunlar oturum açtığınızdan, Azure dosya paylaşımı veya bir NFS paylaşımına gibi bir paylaşılan ağ sürücüsü VM ölçek kümesindeki tüm örneklerini takma. Kullanıcının paylaşılan çalışma alanı, normalde her örnekleri takılı paylaşılan dosya deposu tutulur. 

Ubuntu DSVM örnekleriyle Ayarla ölçek oluşturur bir örnek Azure Resource Manager şablonu bulabilirsiniz [GitHub](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json). Bir örnek [parametre dosyası](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) Azure Resource Manager için aynı konumda şablonudur. 

Azure Resource Manager şablonu Azure CLI parametre dosyası değerlerini ayarlamak ölçek oluşturabilirsiniz. 

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
Yukarıdaki komutlar, sahip olduğunuz varsayılmaktadır:
* Ölçek Örneğiniz için belirtilen değerlerle parametre dosyanın bir kopyasını ayarlayın.
* VM örneği sayısı.
* Azure dosyaları işaretçiler paylaşır.
* Her VM bağlanacaktır depolama hesabı için kimlik bilgileri. 

Parametre dosyası, komutları yerel olarak başvurulur. Ayrıca, parametreleri satır içi ya da bunlar için komut isteminde geçirebilirsiniz.  

Yukarıdaki şablonu SSH ve ön uç ölçeği Ubuntu DSVMs arka uç havuzuna Ayarla JupyterHub bağlantı noktasını etkinleştirir. Bir kullanıcı olarak, yalnızca JupyterHub veya SSH üzerinde VM normal şekilde oturum açın. VM örnekleri genişletilebilir veya aşağı dinamik olarak herhangi bir durum olması gerekir çünkü bağlı kaydedilen Azure dosyaları paylaşır. Windows DSVMs havuzu oluşturmak için aynı yaklaşımı kullanın. 

[Azure dosya paylaşımı bağladığı betik](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh) da github'da Azure DataScienceVM Havuzda kullanılabilir. Komut parametresi dosyasında belirtilen bağlama noktası Azure dosyaları paylaşıma bağlar. Komut dosyası, ilk kullanıcının giriş dizininde bağlı sürücüye yazılım bağlantılarını da oluşturur. Bağlantılı bir kullanıcıya özgü dizüstü dizin Azure dosya paylaşımı içinde yazılım `$HOME/notebooks/remote` dizin böylece kullanıcılar erişim, çalıştırmak ve bunların Jupyter not defterleri kaydedin. Her kullanıcının Jupyter çalışma Azure dosya paylaşımına işaret edecek şekilde VM üzerinde ek kullanıcılar oluştururken, aynı yöntemi kullanabilirsiniz. 

Sanal makine ölçek destek otomatik ölçeklendirmeyi ayarlar. Ek örnekleri oluşturmak ne zaman ve zaman örnekleri ölçeklendirmek kurallar ayarlayabilir. Örneğin, sanal makineleri hiç kullanılmadığı zaman bulut donanım kullanım maliyetlerinde kaydetmek için sıfır örneklerine ölçeklendirmek. Ayrıntılı adımlar için sanal makine ölçek kümeleri belge sayfalarının sağlamak [otomatik ölçeklendirmeyi](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Sonraki adımlar

* [Ortak bir kimlik ayarlayın](dsvm-common-identity.md)
* [Bulut kaynaklarına erişmek için kimlik bilgilerini güvenli bir şekilde saklayın](dsvm-secure-access-keys.md)















