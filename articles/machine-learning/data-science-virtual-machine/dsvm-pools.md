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
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2018
ms.author: gokuma
ms.openlocfilehash: 5cce7f691204a0fd116627fadde1076a4505fcb2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60502291"
---
# <a name="create-a-shared-pool-of-data-science-virtual-machines"></a>Veri bilimi sanal makineleri paylaşılan havuz oluşturma

Bu makalede, paylaşılan bir havuz, veri bilimi sanal makineleri kullanır (Dsvm'leri) bir takım için nasıl oluşturabileceğiniz açıklanır. Paylaşılan bir havuz kullanmanın avantajlarını daha iyi kaynak kullanımı, paylaşma ve işbirliği dönüştüğünde, yönetim ve DSVM kaynaklarının daha etkin yönetim var. 

Dsvm'leri havuzu oluşturmak için birçok yöntem ve teknolojileri kullanabilirsiniz. Bu makalede, etkileşimli VM'ler için havuzlarında odaklanır. Azure Machine Learning işlem bir alternatif yönetilen bilgi işlem altyapısıdır. Bkz: [işlem hedefleri kümesi](../service/how-to-set-up-training-targets.md#amlcompute) daha fazla bilgi için.

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















