---
title: Dağıtıma genel bakış - Azure için Avere vFXT
description: Azure için Avere vFXT dağıtımı genel bakış
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 88305e5d877f5bc3cc7ebb116f69b0f49f162b79
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59268390"
---
# <a name="avere-vfxt-for-azure---deployment-overview"></a>Azure - dağıtıma genel bakış için Avere vFXT

Bu makalede, Azure kümesi ayarlama ve çalıştırma için Avere vFXT erişmek için gerekli adımlara genel bir bakış sağlar.

Çeşitli görevleri önce ve Azure Marketi'nde vFXT küme oluşturduktan sonra gereklidir. Başlangıç ve bitiş işleminin NET bir fikir gereken çaba kapsam yardımcı olur. 

## <a name="deployment-steps"></a>Dağıtım adımları

Sonra [sisteminizi planlama](avere-vfxt-deploy-plan.md), Avere vFXT kümenizi oluşturmak başlayabilirsiniz. 

Azure Marketi'nde bir Azure Resource Manager şablonu, gerekli bilgileri toplar ve tüm küme otomatik olarak dağıtır. 

VFXT küme çalışır duruma geldikten sonra istemcilerin kendisine bağlanıp nasıl kattığını bilmek isteyecektir ve gerekirse, yeni Blob Depolama kapsayıcısına verilerinizi taşıma.  

Tüm adımlar bir genel bakış aşağıda verilmiştir.

1. Önkoşulları yapılandırma 

   VM oluşturmadan önce Avere vFXT proje için yeni bir abonelik oluşturun, aboneliğin sahipliğini yapılandırmalı, kotalar denetleyin ve gereken ve Avere vFXT yazılımlarının kullanımı için koşulları kabul artışı isteyebilir. Okuma [Avere vFXT oluşturmak için hazırlık](avere-vfxt-prereqs.md) ayrıntılı yönergeler için.

1. Avere vFXT kümesi oluşturma 

   Azure marketi, Azure kümesine için Avere vFXT oluşturmak için kullanın. Bir şablon için gerekli bilgileri toplar ve son ürün oluşturmak için komut dosyalarını çalıştırır.

   Küme oluşturma, tüm Market şablonu tarafından gerçekleştirilen adımları içerir: 

   * Gerekirse, altyapı ve kaynak grupları, yeni ağ oluşturma
   * Oluşturma bir *küme denetleyicisi*  

     Avere vFXT kümenin aynı sanal ağda bulunan basit bir VM kümesi denetleyicisidir ve özel yazılım küme oluşturma ve yönetme için gerekli olan. Denetleyici vFXT düğümleri ve formları kümeyi oluşturur ve ayrıca bir yaşam süresi boyunca kümeyi yönetmek için bir komut satırı arabirimi sağlar.

     Dağıtım sırasında yeni bir vnet oluşturun, denetleyiciniz bir genel IP adresi gerekir. Bu denetleyici küme sanal ağ dışında bağlamak için atlama konak olarak kullanılabileceği anlamına gelir.

   * Küme düğümü sanal makineleri oluşturma

   * Küme düğümünü kümeyi oluşturmak için Vm'leri yapılandırma

   * İsteğe bağlı olarak, yeni bir Blob kapsayıcı oluşturma ve arka uç depolama kümesi için yapılandırma

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