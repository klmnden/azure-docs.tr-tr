---
title: Azure için Avere vFXT hakkında ek bağlantılar
description: Azure için Avere vFXT hakkında ek bilgi bağlantıları
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: afba39d1af700650cfbf7226dff36729a76a0bda
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634458"
---
# <a name="additional-documentation"></a>Ek belgeleri

Bu makalede, Denetim Masası'nı Avere Yönetimi arabirimi ve ilgili konular hakkında ilave belgeler için bağlantılar içerir. 

## <a name="avere-cluster-documentation"></a>Avere küme belgeleri

Ek Avere küme belgelerine Web sitesinde bulunabilir <http://library.averesystems.com/>. Bu belgeler, kümenin yeteneklerini ve yapılandırma ayarlarını anlamanıza yardımcı olabilir. 

* [FXT küme oluşturma Kılavuzu](<http://library.averesystems.com/#fxt_cluster>) tasarlanmış kümeler fiziksel donanım düğümlerinin oluşur ancak bazı bilgiler belgedeki vFXT kümeler için de geçerlidir. Özellikle, yeni vFXT küme yöneticileri, bu bölümler okumasını yararlanabilirsiniz:
  * [Destek ve izleme ayarlarını özelleştirme](<http://library.averesystems.com/create_cluster/4_8/html/config_support.html#config-support>) destek karşıya yükleme ayarlarını özelleştirin ve Uzaktan izleme etkinleştirme açıklanmaktadır. 
  * [VServers ve genel Namespace yapılandırma](<http://library.averesystems.com/create_cluster/4_8/html/config_vserver.html#config-vserver>) istemciye yönelik ad alanı oluşturma hakkında bilgi içeriyor.
  * [Avere küme için DNS'yi yapılandırma](<http://library.averesystems.com/create_cluster/4_8/html/config_network.html#dns-overview>) hepsini bir kez deneme DNS yapılandırma açıklanmaktadır.
  * [Arka uç depolama alanı eklemeye](<http://library.averesystems.com/create_cluster/4_8/html/config_core_filer.html#add-core-filer>) çekirdek filtrelerin ekleme belgeleri.

* [Küme yapılandırma Kılavuzu](<http://library.averesystems.com/#operations>) ayarları ve seçenekleri Avere kümesi için tam başvurudur. Bu seçenekler kümesini vFXT kümesi kullanır, ancak aynı yapılandırma sayfaların çoğu uygulama.

* [Pano Kılavuzu](<http://library.averesystems.com/#operations>) kümenin izleme özellikleri Avere Denetim Masası'nın nasıl kullanıldığını açıklar.

## <a name="vfxt-creation-and-management-documentation"></a>vFXT oluşturulması ve Yönetimi belgeleri

Vfxt.py, bulut küme oluşturma ve yönetim yardımcı programını kullanarak bir tam Kılavuzu sağlanan <https://github.com/AvereSystems/vFXT.py/blob/master/docs/README.md>.  
