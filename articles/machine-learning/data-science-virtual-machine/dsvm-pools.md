---
title: Veri bilimi sanal makinesi havuzları - Azure | Microsoft Docs
description: Bir takım için paylaşılan bir kaynak havuzları veri bilimi VM dağıtma
keywords: derin öğrenme yapay ZEKA, veri bilimi araçları, veri bilimi sanal makinesi, Jeo-uzamsal analiz, team data science Process'i
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: acae59922f5a46f059e19db6865491f5186139f7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53103413"
---
# <a name="create-a-shared-pool-of-data-science-virtual-machines"></a>Veri bilimi sanal makineleri paylaşılan havuz oluşturma

Bu makalede, paylaşılan bir havuz, veri bilimi sanal makineleri kullanır (Dsvm'leri) bir takım için nasıl oluşturabileceğiniz açıklanır. Paylaşılan bir havuz kullanmanın avantajlarını daha iyi kaynak kullanımı, paylaşma ve işbirliği dönüştüğünde, yönetim ve DSVM kaynaklarının daha etkin yönetim var. 

Dsvm'leri havuzu oluşturmak için birçok yöntem ve teknolojileri kullanabilirsiniz. Bu makalede, toplu işlem ve etkileşimli VM'ler için havuzları odaklanır.

## <a name="batch-processing-pool"></a>Toplu işlem havuzu
Esas olarak çevrimdışı bir toplu işte işlerini çalıştırmak için bir Dsvm'leri havuz ölçeğini ayarlamak istiyorsanız, kullanabileceğiniz [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) veya [Azure Batch](https://docs.microsoft.com/azure/batch/) hizmeti. Bu makalede, Azure Batch AI üzerinde odaklanır.

Ubuntu DSVM sürümü, Azure Batch AI görüntüleri biri olarak desteklenir. Azure CLI veya Python oluşturduğunuz Azure Batch AI kümesi, SDK'sını belirtebilirsiniz `image` parametresi ve `UbuntuDSVM`. Ne tür bir işlem düğümleri istediğinizi seçebilirsiniz: GPU tabanlı örnekler yalnızca CPU örnekler, CPU ve bellek sayısı yerine bir [VM örneklerinin geniş seçenek](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) Azure üzerinde kullanılabilir. 

Batch AI GPU tabanlı düğümleri Ubuntu DSVM görüntüsü kullandığınızda, gerekli GPU sürücüleri ve derin öğrenme çerçeveleri makineleridir. Önceden batch düğümleri hazırlamak için önemli ölçüde zaman kazandırır. Aslında, bir Ubuntu DSVM üzerinde etkileşimli olarak geliştiriyorsanız, Batch AI düğümlerin tam olarak aynı kurulumu ve ortamın yapılandırması olduğunu fark edeceksiniz. 

Genellikle, bir Batch AI kümesi oluşturduğunuzda, ayrıca tüm düğümler tarafından oluşturulmuş bir dosya paylaşımı oluşturun. Dosya Paylaşımı, giriş ve çıkış verileri, ek olarak toplu iş kod/betikleri depolamak için kullanılır. 

Bir Batch AI kümesi oluşturduktan sonra çalıştırılacak işleri göndermek için aynı CLI veya Python SDK'sını kullanabilirsiniz. Yalnızca batch işlerini çalıştırmak için kullanılan süre için ödeme yaparsınız. 

Daha fazla bilgi için bkz.
* Kullanmaya yönelik adım adım kılavuz [Azure CLI](https://docs.microsoft.com/azure/batch-ai/quickstart-cli) Batch AI'ı yönetmek için
* Kullanmaya yönelik adım adım kılavuz [Python](https://docs.microsoft.com/azure/batch-ai/quickstart-python) Batch AI'ı yönetmek için
* [Batch AI tarifleri](https://github.com/Azure/BatchAI) çeşitli yapay ZEKA ve derin öğrenme çerçeveleri Batch AI ile nasıl kullanılacağı gösterilmektedir

## <a name="interactive-vm-pool"></a>Etkileşimli VM havuzu

Tüm yapay ZEKA/veri bilimi ekibi tarafından paylaşılan etkileşimli VM'lerin bir havuz kullanılabilir her bir kullanıcı kümesi için ayrılmış bir örnek yerine DSVM örneğini oturum açmasına izin verir. Bu kurulum ile daha iyi kullanılabilirlik ve kaynakların daha etkili kullanımını yardımcı olur. 

Etkileşimli bir VM havuzu oluşturmak için kullandığınız teknoloji, [Azure sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). Ölçek kümeleri, bir grup özdeş, yük dengeli ve otomatik ölçeklendirme Vm'leri oluşturmak ve yönetmek için kullanabilirsiniz. 

Kullanıcı ana havuzuna ait IP ya da DNS adresine bağlanır. Ölçek oturum yollar ölçek kümesindeki bir kullanılabilir DSVM otomatik olarak ayarlayın. Kullanıcıların benzer bir ortam VM ne olursa olsun istediğiniz bunlar oturum açtığınızdan, VM ölçek kümesindeki tüm örnekleri bir Azure dosya paylaşımı veya bir NFS paylaşımına gibi bir paylaşılan ağ sürücüsü bağlayın. Kullanıcının paylaşılan çalışma normalde her örnekleri bağlanan paylaşılan dosya depolama tutulur. 

Örnek bir Azure Resource Manager şablonu ölçek üzerinde Ubuntu DSVM örnekleriyle kümesi oluşturan bulabilirsiniz [GitHub](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json). Bir örnek [parametre dosyası](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) aynı konumda için Azure Resource Manager şablonudur. 

Azure Resource Manager şablonu parametre dosyası için değerler Azure CLI'yi belirleyerek ölçek oluşturabilirsiniz. 

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
Önceki komutlarda, sahip olduğunuz varsayılmaktadır:
* Parametre dosyası, Ölçek kümesi örneği için belirtilen değerlerle bir kopyası.
* Sanal makine örneği sayısını.
* Azure dosyaları için işaretçiler paylaşın.
* Her VM'de bağlanacaktır depolama hesabı için kimlik bilgileri. 

Parametre dosyası, komutları yerel olarak başvuruluyor. Parametreleri, satır içi veya komut istemi onlar için de geçirebilirsiniz.  

Yukarıdaki şablonu SSH ve Ubuntu Dsvm'leri arka uç havuzuna bir ön uç ölçek JupyterHub bağlantı noktasını etkinleştirir. Bir kullanıcı olarak, yalnızca VM'ye SSH veya JupyterHub normal şekilde oturum açın. Sanal makine örneklerine ölçeklendirilebilir veya aşağı dinamik olarak herhangi bir durum olması gerekir çünkü bağlı kaydedilmiş Azure dosyaları paylaşın. Windows Dsvm'leri havuzu oluşturmak için aynı yaklaşımı kullanabilirsiniz. 

[Azure dosyaları paylaşımına bağlandığı betik](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh) de GitHub Azure DataScienceVM depoda kullanılabilir. Betik, Azure dosya paylaşımını parametre dosyasında belirtilen bağlama noktası bağlar. Betik, ilk kullanıcının giriş dizininde bağlı sürücü geçici bağlantılar da oluşturur. Bağlı Azure dosya paylaşımı içinde bir kullanıcıya özgü not defteri dizin geçici `$HOME/notebooks/remote` dizin kullanıcıların erişmek, çalıştırın ve kullanıcıların Jupyter not defterlerini kaydedin. Ek kullanıcılar her kullanıcının Jupyter çalışma Azure dosyaları paylaşımına işaret edecek şekilde sanal makine oluşturduğunuzda, aynı yöntemi kullanabilirsiniz. 

Sanal makine ölçek kümeleri, otomatik ölçeklendirmeyi destekler. Ek örnekleri oluşturmak ne zaman ve ne zaman örneğine ölçeklendirme kuralları ayarlayabilirsiniz. Örneğin, sıfır örneklere Vm'leri hiç kullanılmadığı zaman bulut donanım kullanım maliyet tasarrufu için ölçeği azaltabilirsiniz. Ayrıntılı adımlar için sanal makine ölçek kümeleri belgeleri sayfaları sağlamak [otomatik ölçeklendirme](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Sonraki adımlar

* [Ortak bir kimlik ayarlayın](dsvm-common-identity.md)
* [Bulut kaynaklarına erişmek için kimlik bilgilerini güvenli bir şekilde depolayın](dsvm-secure-access-keys.md)















