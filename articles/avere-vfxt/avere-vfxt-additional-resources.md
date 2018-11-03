---
title: Azure için Avere vFXT hakkında ek bağlantılar
description: Azure için Avere vFXT hakkında ek bilgi bağlantıları
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 65e764d843f9e87adee4cf94c1d22b02db80eda0
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958851"
---
# <a name="additional-documentation"></a>Diğer belgeler

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

Vfxt.py, bulut küme oluşturma ve yönetim yardımcı programını kullanarak bir tam Kılavuzu, GitHub üzerinde sağlanır: [bulut vfxt.py ile küme yönetimi](https://github.com/Azure/AvereSDK/blob/master/docs/README.md).  
