---
title: Avere vFXT DNS - Azure
description: Bir DNS sunucusunu Avere vFXT ile Azure için hepsini bir kez deneme yük dengelemeyi yapılandırma
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 9fd9eaf1e62d063026e0e656346baaaade87064f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60410144"
---
# <a name="avere-cluster-dns-configuration"></a>Avere kümesi DNS yapılandırması

Bu bölümde, DNS sistemini Yük Dengeleme Avere vFXT kümenizi yapılandırma temellerini açıklar. 

Bu belge *içermemesi* ayarlamaya ve bir DNS sunucusu Azure ortamında yönetmeye yönelik yönergeler. 

Yük Dengeleme için DNS hepsini bir kez deneme kullanmak yerine, azure'da vFXT kümeyi el ile uygulanan yöntemleri bağlı oldukları zaman istemciler arasında eşit şekilde IP adresleri atamak kullanmayı düşünün. Çeşitli yöntemler açıklanmıştır [Avere küme bağlama](avere-vfxt-mount-clients.md). 

Bir DNS sunucusu kullanılıp kullanılmayacağı verirken bunları göz önünde bulundurun: 

* Sisteminizi NFS istemciler tarafından erişilirse, DNS kullanarak gerekli değildir - sayısal IP adresleri kullanarak tüm ağ adresleri belirtmek mümkündür. 

* Sisteminiz SMB (CIFS) erişim destekliyorsa, Active Directory sunucusu için bir DNS etki alanı belirtmeniz gerekir çünkü DNS gereklidir.

* Kerberos kimlik doğrulamasını kullanmak istiyorsanız DNS gereklidir.

## <a name="load-balancing"></a>Yük dengeleme

Genel yük dağıtmak için hepsini bir kez deneme yük dağıtımı için istemciye yönelik IP adreslerini kullanmak için bir DNS etki alanınızı yapılandırın.

## <a name="configuration-details"></a>Yapılandırma ayrıntıları

İstemciler kümeye erişirken RRDNS isteklerinde kullanılabilir tüm arabirimleri arasında otomatik olarak dengeler.

En iyi performans için aşağıdaki diyagramda gösterildiği gibi istemci kullanıma yönelik küme adresleri işlemek için DNS sunucunuzu yapılandırın.

Bir küme vserver sol tarafta gösterilir ve IP adresleri Merkezi'nde ve sağdaki görünür. Her istemci erişim noktası ile bir yapılandırma kayıtları ve gösterildiği gibi işaretçiler.

![Avere kümesi hepsini bir kez deneme DNS diyagramı](media/avere-vfxt-rrdns-diagram.png) 
<!--- separate text description file provided  [diagram text description](avere-vfxt-rrdns-alt-text.md) -->

Her istemci kullanıma yönelik IP adresi, küme tarafından iç kullanım için benzersiz bir ad olmalıdır. (Bu diyagramda, istemci IP'leri vs1 adlandırıldığı-istemci - IP-* anlaşılması için ancak üretimde büyük olasılıkla istemci * gibi daha kısa, bir şey kullanmanız gerekir.)

İstemciler, sunucu bağımsız değişken olarak vserver adıyla kümeye bağlayın. 

DNS sunucunuzun değiştirme ``named.conf`` dosyası için vserver sorgular için döngüsel sırayı ayarlamak için. Bu seçenek, tüm kullanılabilir değerleri arasında geçiş sırasında uygulamaları olmasını sağlar. Aşağıdaki gibi bir deyim ekleyin:

```
options {
    rrset-order {
        class IN A name "vserver1.example.com" order cyclic;
    };
};
```

DNS düzgün yapılandırma örneği nsupdate aşağıdakileri sağlar:

```
update add vserver1.example.com. 86400 A 10.0.0.10
update add vserver1.example.com. 86400 A 10.0.0.11
update add vserver1.example.com. 86400 A 10.0.0.12
update add vs1-client-IP-10.example.com. 86400 A 10.0.0.10
update add vs1-client-IP-11.example.com. 86400 A 10.0.0.11
update add vs1-client-IP-12.example.com. 86400 A 10.0.0.12
update add 10.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-10.example.com
update add 11.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-11.example.com
update add 12.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-12.example.com
```

## <a name="cluster-dns-settings"></a>Küme DNS ayarları

İçinde vFXT kümenin kullandığı DNS sunucusunu belirtmenizi **küme** > **yönetim ağ** Ayarları sayfası. Bu sayfadaki ayarlar şunlardır:

* DNS sunucusu adresi
* DNS etki alanı adı
* DNS arama etki alanları

Okuma [DNS ayarlarını](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_admin_network.html#gui-dns>) bu sayfayı kullanma hakkında daha fazla ayrıntı için Avere küme yapılandırma Kılavuzu'nda.


