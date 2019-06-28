---
title: Microsoft Azure FXT Edge dosyalayıcı küme için ağ ayarları
description: Azure FXT Edge dosyalayıcı Küme oluşturulduktan sonra ağ ayarlarını özelleştirme
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 6b7c3099415aed9529727a1de30cd832189db58d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450383"
---
# <a name="tutorial-configure-the-clusters-network-settings"></a>Öğretici: Küme ağ ayarlarını yapılandırma 

Yeni oluşturulan bir Azure FXT Edge dosyalayıcı küme kullanmadan önce denetleyin ve birkaç ağ ayarları, iş akışını özelleştirme gerekir. 

Bu öğreticide, yeni bir küme için ayarlanacak ihtiyacınız olabilecek ağ ayarlarını açıklar. 

Şunları öğreneceksiniz: 

> [!div class="checklist"]
> * Hangi ağ ayarlarını bir küme oluşturulduktan sonra güncelleştirilmesi gerekebilir
> * Hangi FXT Edge dosyalayıcı kullanım örnekleri bir AD sunucusu ya da bir DNS sunucusu gerektirir. 
> * Hepsini otomatik olarak Yük Dengeleme istemci isteklerini FXT küme için DNS (RRDNS) yapılandırma

Sisteminizde kaç yapılandırma değişiklikleri gerekir, bu adımları tamamlamak için gereken süreyi bağlıdır:

* Öğreticiyi okuyun ve birkaç ayarlarını denetlemek yalnızca ihtiyacınız varsa, bu 10-15 dakika sürer. 
* DNS hepsini bir kez deneme yapılandırmanız gerekiyorsa, bu görev bir saat veya daha fazla sürebilir.

## <a name="adjust-network-settings"></a>Ağ ayarları

Ağ ile ilgili çeşitli görevleri yeni bir Azure FXT Edge dosyalayıcı küme ayarlama bir parçasıdır. Bu listeyi kontrol edin ve hangilerinin sisteminiz için geçerli karar verin.

Küme için ağ ayarları hakkında daha fazla bilgi edinmek için [Ağ Hizmetleri Yapılandırma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/network_overview.html) küme yapılandırma kılavuzu.

* (İsteğe bağlı) istemciye yönelik ağ için hepsini bir kez deneme DNS yapılandırma

  Yük küme trafiğin yükünü dengelemenizi açıklandığı DNS sistem yapılandırarak [FXT Edge dosyalayıcı kümesi için DNS yapılandırma](#configure-dns-for-load-balancing).

* NTP ayarlarını doğrulayın

* Active Directory ve kullanıcı adı/grup adı indirmeler (gerekirse) yapılandırma

  Ağ konaklarınız Active Directory veya başka tür bir dış dizin hizmeti kullanıyorsanız, kümenin Dizin Hizmetleri Yapılandırması nasıl kullanıcı adı ve grup bilgilerini küme indirir ayarlamak için değiştirmeniz gerekir. Okuma **küme** > **Dizin Hizmetleri** Ayrıntılar için küme yapılandırma Kılavuzu'nda.

  SMB desteği istiyorsanız bir AD sunucusu gereklidir. SMB ayarlanacak başlatmadan önce AD yapılandırın.

* VLAN'ları (isteğe bağlı) tanımlama
  
  Kümenizin vservers ve genel ad alanı tanımlamadan önce gereken ek tüm VLAN'ları yapılandırın. Okuma [VLAN'ları ile çalışma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/network_overview.html#vlan-overview) daha fazla bilgi için küme yapılandırma kılavuzu.

* Proxy sunucuları (gerekirse) yapılandırma

  Kümenizi dış adreslerine ulaşmak için bir proxy sunucusu kullanıyorsa, bunu ayarlamak için aşağıdaki adımları izleyin:

  1. Proxy sunucu tanımlama **Proxy Yapılandırması** Ayarları sayfası
  1. İle proxy sunucusu yapılandırmasını uygulamak **küme** > **genel kurulumu** sayfası veya **çekirdek dosyalayıcı ayrıntıları** sayfası.
  
  Daha fazla bilgi için okuma [web proxy'si kullanarak](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/proxy_overview.html) küme yapılandırma kılavuzu.

* Karşıya yükleme [şifreleme sertifikaları](#encryption-certificates) kümenin (isteğe bağlı) kullanmak

### <a name="encryption-certificates"></a>Şifreleme sertifikaları

FXT Edge dosyalayıcı küme, bu işlevler için X.509 sertifikaları kullanır:

* Küme yönetim trafiğini şifrelemek için

* Üçüncü taraf KMIP sunucularına bir istemci adına kimlik doğrulaması yapmak için

* Bulut sağlayıcıları sunucu sertifikalarını doğrulamak için

Küme sertifikalarını yüklemek ihtiyacınız varsa **küme** > **sertifikaları** Ayarları sayfası. Ayrıntılar için bkz [küme > Sertifikalar](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_certificates.html) küme yapılandırma kılavuzu sayfası.

Küme yönetimi iletişimi şifrelemek için **küme** > **genel kurulumu** Ayarları sayfasında, yönetim SSL için kullanmak üzere hangi sertifikanın seçin.

> [!Note] 
> Bulut hizmeti erişim anahtarlarını kullanarak depolanır **bulut kimlik bilgileri** yapılandırma sayfası. [Çekirdek dosyalayıcı ekleme](fxt-add-storage.md#add-a-core-filer) küme yapılandırma kılavuzu okuyun; gösterildiği örnek bölüm [bulut kimlik bilgileri](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cloud_credentials.html) ayrıntıları bölümü. 

## <a name="configure-dns-for-load-balancing"></a>Yük Dengeleme için DNS yapılandırma

Bu bölüm, istemci yükleme FXT Edge dosyalayıcı kümenizdeki tüm istemci kullanıma yönelik IP adresleri arasında dağıtmak için hepsini bir kez deneme DNS (RRDNS) sistemini yapılandırma temellerini açıklar. 

### <a name="decide-whether-or-not-to-use-dns"></a>DNS kullanılıp kullanılmayacağı karar verin

Yük Dengeleme her zaman önerilir, ancak her zaman DNS kullanmak zorunda değilsiniz. Örneğin, bazı türleri istemci iş akışları ile bunlar kümeye bağladığınızda, istemciler arasında eşit şekilde küme IP adresleri atamak için bir betik kullanmak için daha fazla mantıklı olabilir. Bazı yöntemler açıklanmıştır [küme bağlama](fxt-mount-clients.md). 

Bir DNS sunucusu kullanılıp kullanılmayacağı verirken bunları göz önünde bulundurun: 

* Sisteminizi NFS istemciler tarafından erişilirse, DNS gerekli değildir. Sayısal IP adresleri kullanarak tüm ağ adresleri belirtmek mümkündür. 

* Sisteminiz SMB (CIFS) erişim destekliyorsa, Active Directory sunucusu için bir DNS etki alanı belirtmeniz gerekir çünkü DNS gereklidir.

* Kerberos kimlik doğrulamasını kullanmak istiyorsanız DNS gereklidir.

### <a name="round-robin-dns-configuration-details"></a>Hepsini bir kez deneme DNS yapılandırma ayrıntıları

İstemciler kümeye erişirken RRDNS isteklerinde kullanılabilir tüm arabirimleri arasında otomatik olarak dengeler.

En iyi performans için aşağıdaki diyagramda gösterildiği gibi istemci kullanıma yönelik küme adresleri işlemek için DNS sunucunuzu yapılandırın.

Bir küme vserver sol tarafta gösterilir ve IP adresleri Merkezi'nde ve sağdaki görünür. Her istemci erişim noktası ile bir yapılandırma kayıtları ve gösterildiği gibi işaretçiler.

![Küme hepsini bir kez deneme DNS diyagramı - ayrıntılı alternatif metin bağlantı aşağıdaki görüntü](media/fxt-rrdns-diagram.png) 
[ayrıntılı açıklama metni](https://azure.github.io/Avere/legacy/Azure-FXT-EdgeFilerDNSconfiguration-alt-text.html)

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

Aşağıdaki ``nsupdate`` komutları doğru DNS yapılandırma örneği sağlar:

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

### <a name="enable-dns-in-the-cluster"></a>DNS kümede etkinleştir 

İçinde kümenin kullandığı DNS sunucusunu belirtmenizi **küme** > **yönetim ağ** Ayarları sayfası. Bu sayfadaki ayarlar şunlardır:

* DNS sunucusu adresi
* DNS etki alanı adı
* DNS arama etki alanları

Daha fazla bilgi edinmek için [DNS ayarlarını](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_admin_network.html#gui-dns>) küme yapılandırma kılavuzu.

## <a name="next-steps"></a>Sonraki adımlar

Bu Azure FXT Edge dosyalayıcı kümesi için son temel yapılandırma adımdır. 

* Sistemin LED'lerini ve diğer göstergeleri hakkında [donanım durumunu izlemek](fxt-monitor.md).
* İstemciler FXT Edge dosyalayıcı nasıl bağlama hakkında daha fazla küme bilgi [küme bağlama](fxt-mount-clients.md). 
* İşletim ve FXT Edge dosyalayıcı küme yönetme hakkında daha fazla bilgi için bkz. [küme yapılandırma Kılavuzu](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/ops_conf_index.html). 