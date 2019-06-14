---
title: Azure için Avere vFXT hakkında ek bağlantılar
description: Azure için Avere vFXT hakkında ek bilgi bağlantıları
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: v-erkell
ms.openlocfilehash: 2efbe7ddc39b8bde76ee4a135f3f44af0864a374
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515592"
---
# <a name="additional-documentation"></a>Diğer belgeler

Bu makalede, Denetim Masası'nı Avere Yönetimi arabirimi ve ilgili konular hakkında ilave belgeler için bağlantılar içerir. 

## <a name="avere-cluster-documentation"></a>Avere küme belgeleri

Ek Avere küme belgelerine Web sitesinde bulunabilir <https://azure.github.io/Avere/>. Bu belgeler, kümenin yeteneklerini ve yapılandırma ayarlarını anlamanıza yardımcı olabilir. 

* [FXT küme oluşturma Kılavuzu](<https://azure.github.io/Avere/#fxt_cluster>) tasarlanmış kümeler fiziksel donanım düğümlerinin oluşur ancak bazı bilgiler belgedeki vFXT kümeler için de geçerlidir. Özellikle, yeni vFXT küme yöneticileri, bu bölümler okumasını yararlanabilirsiniz:
  * [Destek ve izleme ayarlarını özelleştirme](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_support.html#config-support>) destek karşıya yükleme ayarlarını özelleştirin ve Uzaktan izleme etkinleştirme açıklanmaktadır. 
  * [VServers ve genel Namespace yapılandırma](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_vserver.html#config-vserver>) istemciye yönelik ad alanı oluşturma hakkında bilgi içeriyor.
  * [Avere küme için DNS'yi yapılandırma](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_network.html#dns-overview>) hepsini bir kez deneme DNS yapılandırma açıklanmaktadır.
  * [Arka uç depolama alanı eklemeye](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_core_filer.html#add-core-filer>) çekirdek filtrelerin ekleme belgeleri.

* [Küme yapılandırma Kılavuzu](<https://azure.github.io/Avere/#operations>) ayarları ve seçenekleri Avere kümesi için tam başvurudur. Bu seçenekler kümesini vFXT kümesi kullanır, ancak aynı yapılandırma sayfaların çoğu uygulama.

* [Pano Kılavuzu](<https://azure.github.io/Avere/#operations>) kümenin izleme özellikleri Avere Denetim Masası'nın nasıl kullanıldığını açıklar.

## <a name="vfxt-creation-and-management-documentation"></a>vFXT oluşturulması ve Yönetimi belgeleri

Github'da vfxt.py, bulut küme oluşturma ve yönetim yardımcı programını kullanmak için tam bir kılavuz sağlanır: [Bulut vfxt.py ile küme yönetimi](https://github.com/Azure/AvereSDK/blob/master/docs/README.md).  
