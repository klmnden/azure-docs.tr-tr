---
title: Dağıtıma genel bakış - Azure için Avere vFXT
description: Azure için Avere vFXT dağıtımı genel bakış
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: aa5737d67ea2c9cb8cc7c7098764ae67fc91137d
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634469"
---
# <a name="avere-vfxt-for-azure---deployment-overview"></a>Azure - dağıtıma genel bakış için Avere vFXT

Bu makalede, Azure kümesi ayarlama ve çalıştırma için Avere vFXT erişmek için gerekli adımlara genel bir bakış sağlar.

İlk kez bir Avere vFXT sistemini dağıttığınızda çoğu diğer Azure araçlarını dağıtma değerinden daha fazla adım içerdiğini fark edebilirsiniz. Başlangıç ve bitiş işleminin NET bir fikir gereken çaba kapsam yardımcı olur. Sistem çalışır duruma geldikten sonra bulut tabanlı bilgi işlem görevlerini hızlandırmak için gücünü çabalara yapar.

## <a name="deployment-steps"></a>Dağıtım adımları

Sonra [sisteminizi planlama](avere-vfxt-deploy-plan.md), Avere vFXT kümenizi oluşturmak başlayabilirsiniz. 

Bir küme denetleyicisi vFXT kümeyi oluşturmak için kullanılan sanal makine oluşturarak başlayın.

VFXT küme çalışır duruma geldikten sonra istemcilerin kendisine bağlanıp nasıl kattığını bilmek isteyecektir ve gerekirse, yeni Blob Depolama kapsayıcısına verilerinizi taşıma.  

Tüm adımlar bir genel bakış aşağıda verilmiştir.

1. Önkoşulları yapılandırma 

   VM oluşturmadan önce Avere vFXT proje için yeni bir abonelik oluşturun, aboneliğin sahipliğini yapılandırmalı, kotalar denetleyin ve gereken ve Avere vFXT yazılımlarının kullanımı için koşulları kabul artışı isteyebilir. Okuma [Avere vFXT oluşturmak için hazırlık](avere-vfxt-prereqs.md) ayrıntılı yönergeler için.

1. Küme denetleyiciyi oluşturun

   *Küme denetleyicisi* Avere vFXT kümenin aynı sanal ağda bulunan basit bir vm'dir. Denetleyici vFXT düğümleri ve formları kümeyi oluşturur ve ayrıca bir yaşam süresi boyunca kümeyi yönetmek için bir komut satırı arabirimi sağlar.

   Bir genel IP adresiyle denetleyicinizi yapılandırmak, küme sanal ağ dışında bağlamak için ayrıca bir atlama konağı olarak görebilir.

   Tüm yazılımların vFXT kümeyi oluşturmak ve düğümlerini yönetmek için gerekli önceden küme denetleyicisinde.

   Okuma [VM kümesi denetleyicisi oluşturmak](avere-vfxt-deploy.md#create-the-cluster-controller-vm) Ayrıntılar için.

1. Küme düğümleri için bir çalışma zamanı rolü oluşturun 

   Azure kullanan [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/) küme düğümü sanal makinelerin belirli görevleri gerçekleştirmek için yetkilendirmek üzere (RBAC). Örneğin, küme düğümleri için atama yapabilir veya diğer küme düğümleri için IP adreslerini yeniden atamak gerekir. Kümeyi oluşturmadan önce bunları yeterli izin veren bir rol tanımlamanız gerekir.

   Küme denetleyicinin önceden yüklenmiş yazılım özelleştirebileceğiniz bir prototip rolü içerir. Okuma [küme düğümü erişim rolünü oluşturma](avere-vfxt-deploy.md#create-the-cluster-node-access-role) yönergeler için.

1. Avere vFXT kümesi oluşturma 

   Denetleyici üzerinde uygun küme oluşturma betiği düzenleyin ve kümeyi oluşturmak için yürütün. [Dağıtım betiği Düzenle](avere-vfxt-deploy.md#edit-the-deployment-script) ayrıntılı yönergeler içerir. 

1. Kümeyi yapılandırma 

   Avere vFXT yapılandırma arabiriminin (Avere küme ayarlarını özelleştirmek için Denetim Masası) bağlanın. İzleme desteği için iyileştirilmiş ve bir şirket içi veri merkezi kullanıyorsanız, depolama sistemi ekleyin.

   * [Erişim vFXT küme](avere-vfxt-cluster-gui.md)
   * [Desteğini etkinleştir](avere-vfxt-enable-support.md)
   * [Depolamayı yapılandırma](avere-vfxt-add-storage.md) (gerekirse)

1. İstemciler bağlama

   Bölümündeki yönergeleri uygulayın [Avere vFXT küme bağlama](avere-vfxt-mount-clients.md) Yük Dengeleme ve istemci makineleri kümenin nasıl bağlama öğrenin.

1. (Gerekirse) veri ekleme

   Avere vFXT ölçeklenebilir çok istemci önbelleğini olduğundan, verileri yeni bir arka uç depolama kapsayıcısına taşımak için en iyi yolu çok istemci, birden çok iş parçacıklı bir dosya kopyalama ile stratejisidir. Okuma [vFXT kümeye veri taşıma](avere-vfxt-data-ingest.md) Ayrıntılar için.

## <a name="next-steps"></a>Sonraki adımlar

Devam [Avere vFXT oluşturmak için hazırlık](avere-vfxt-prereqs.md) Avere vFXT dağıtmak için Azure başlangıç görevleri tamamlamak için. 