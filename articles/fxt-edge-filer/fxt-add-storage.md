---
title: Microsoft Azure FXT Edge dosyalayıcı kümeye arka uç depolama ekleme
description: Arka uç depolama ve istemciye yönelik pseudonamespace Azure FXT Edge dosyalayıcı için yapılandırma
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 0a16e654ff92c450438ac91c590b42d22201d015
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450460"
---
# <a name="tutorial-add-back-end-storage-and-configure-the-virtual-namespace"></a>Öğretici: Arka uç depolama alanı ekleme ve sanal ad alanı yapılandırma 

Bu öğretici, önbelleğiniz arka uç depolama alanı nasıl eklenir ve istemciye yönelik sanal dosya sistemi ayarlama açıklanmaktadır. 

Arka uç depolama sistemleri veri istemcilerin istek erişmek ve değişiklikleri kıyasla daha kalıcı olarak depolamak için küme bağlandığı önbellek. 

Arka uç depolama değişen istemci-tarafı iş akışları olmadan takas etmenize olanak tanıyan istemciye yönelik sanal dosya sistemi ad alanıdır. 

Bu öğreticide şunları öğreneceksiniz: 

> [!div class="checklist"]
> * Azure FXT Edge dosyalayıcı kümeye arka uç depolama ekleme 
> * Depolama için istemciye yönelik yolu tanımlama

## <a name="about-back-end-storage"></a>Arka uç depolama hakkında

Azure FXT Edge dosyalayıcı kümesi kullanan bir *dosyalayıcı çekirdek* bir arka uç depolama sistemi FXT kümeye bağlanmak için tanımı.

Azure FXT Edge dosyalayıcı çeşitli popüler NAS donanım sistemler ile uyumludur ve Azure Blob veya diğer bulut depolama boş kapsayıcıları kullanabilirsiniz. 

Bulut depolama kapsayıcıları FXT işletim sistemi tamamen tüm bulut depolama birimindeki verileri yönetebilmeniz için eklendiğinde boş olmalıdır. Küme çekirdeği dosyalayıcı olarak kapsayıcı ekledikten sonra mevcut verilerinizi bulut kapsayıcıya taşıyabilirsiniz.

Bir çekirdek dosyalayıcı sisteminize eklemek için Denetim Masası'nı kullanın.

> [!NOTE]
> 
> Amazon AWS veya Google bulut depolama kullanmak istiyorsanız, bir FlashCloud yüklemelisiniz<sup>TM</sup> özellik lisans. Bir lisans anahtarı için Microsoft temsilcinize başvurun ve ardından eski Yapılandırma Kılavuzu'ndaki yönergeleri izleyin [ekleme veya özellik lisanslarını kaldırma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/install_licenses.html#install-licenses).
> 
> Azure Blob Depolama için destek, Azure FXT Edge dosyalayıcı yazılım lisans dahildir. 

Çekirdek filtrelerin ekleme hakkında daha ayrıntılı bilgi için küme yapılandırma kılavuzu aşağıdaki bölümleri okuyun:

* Daha fazla ilgili seçme ve çekirdek dosyalayıcı eklemek için hazırlanma için okuma [çalışma ile çekirdek filtrelerin](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/core_filer_overview.html#core-filer-overview).
* Ayrıntılı önkoşulları ve adım adım yönergeler için bu makaleleri okuyun:

  * [Yeni bir NAS çekirdek dosyalayıcı ekleme](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_nas.html#create-core-filer-nas)
  * [Yeni bir bulut çekirdek dosyalayıcı ekleme](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_cloud.html#create-core-filer-cloud)

Bir çekirdek dosyalayıcı ekledikten sonra çekirdek dosyalayıcı ayrıntıları Ayarları sayfasında, ayarları güncelleştirebilirsiniz.

## <a name="add-a-core-filer"></a>Bir çekirdek dosyalayıcı Ekle

Tıklayarak bir çekirdek dosyalayıcı tanımlamak **Oluştur** düğmesini **çekirdek dosyalayıcı** > **yönetme çekirdek filtrelerin** Ayarları sayfası.

![Çekirdek filtrelerin Yönet sayfasında Çekirdek filtrelerin listesini yukarıda Oluştur düğmesine tıkladığınızda](media/fxt-cluster-config/create-core-filer-button.png)

**Ekleme yeni çekirdek dosyalayıcı** Sihirbazı yol gösterir; böylece, arka uç depolama bağlanan bir çekirdek dosyalayıcı oluşturma işlemi. Küme yapılandırma Kılavuzu, bulut depolama alanı (yukarıda bağlantılar verilmiştir) için ve NFS/NAS depolama için farklı işlemi adım adım açıklamaları vardır. 

Alt görevler aşağıdakileri içerir:

* Çekirdek dosyalayıcı (NAS veya Bulut) türünü belirtin

  ![Donanım NAS yeni çekirdek dosyalayıcı Sihirbazı'nın ilk sayfasında. "Bulut çekirdek dosyalayıcı" seçeneğini devre dışı bırakılır ve lisans eksik ilgili bir hata iletisi gösterir.](media/fxt-cluster-config/new-nas-1.png)

* Çekirdek dosyalayıcı'nın adını ayarlayın. Küme yöneticileri temsil ettiği hangi depolama sistemi anlamanıza yardımcı olacak bir ad seçin.

* NAS çekirdek filtreleri için tam etki alanı adı (FQDN) veya IP adresi sağlayın. FQDN, tüm çekirdek filtreleri için önerilen ve SMB erişimi için gerekli.

* Önbellek ilkesi seçin - Sihirbazın ikinci sayfasında kullanılabilir önbellek ilkeleri için yeni çekirdek dosyalayıcı listeler. Ayrıntılı bilgi edinmek için [önbelleğe alma ilkeleri bölümüne küme yapılandırma Kılavuzu](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_manage_cache_policies.html). 

  ![Sihirbazın ikinci sayfasına bir donanım NAS yeni çekirdek dosyalayıcı; Önbellek İlkesi açılan menüsü, çeşitli seçenekler ve üç geçerli önbellek ilkesi seçenekleri (önbelleğe alma ve okuma/yazma önbelleği okuma atlama) devre dışı gösteren, açık durumdadır.](media/fxt-cluster-config/new-nas-choose-cache-policy.png)

* Bulut depolama, bulut hizmeti ve erişim kimlik bilgileri, diğer parametreler arasında belirtmeniz gerekir. Ayrıntılı bilgi edinmek için [bulut hizmeti ve Protokolü](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_cloud.html#cloud-service-and-protocol) küme yapılandırma kılavuzu.

  ![Bulut Temel dosyalayıcı bilgi yeni çekirdek dosyalayıcı Sihirbazı](media/fxt-cluster-config/new-core-filer-cloud3.png) <!-- xxx get an Azure version of this screenshot xxx -->
  
  Bulut erişim kimlik bilgileri bu küme için eklediyseniz, bunlar listede görünür. Güncelleştirme ve kimlik bilgilerini eklemek **küme** > **bulut kimlik bilgileri** Ayarları sayfası. 

Tüm gerekli ayarları Sihirbazı'nda doldurduktan sonra tıklatın **ekleme dosyalayıcı** değişikliği gönderme düğmesi.

Birkaç dakika sonra depolama sistemi görünür **Pano** çekirdek filtrelerin listesini ve çekirdek dosyalayıcı ayarları sayfaları erişilebilir.

![Genişletilmiş dosyalayıcı Ayrıntılar görünümü ile çekirdek filtrelerin yönetme Ayarları sayfasında, "NAS Flurry" dosyalayıcı çekirdek](media/fxt-cluster-config/core-filer-in-manage-page.png)

Bu ekran görüntüsünde çekirdek dosyalayıcı bir vserver eksik. Çekirdek dosyalayıcı bağlamak için bir vserver ve istemcilerin depolama erişebilmesi için bir birleşim oluşturmanız gerekir. Bu adımlar aşağıda açıklanan [ad alanı yapılandırma](#configure-the-namespace).

## <a name="configure-the-namespace"></a>Ad alanı yapılandırma

Sanal dosya sistemi adlı bir Azure FXT Edge dosyalayıcı kümeyi oluşturur *ad alanı küme* istemci erişimi için farklı arka uç sistemlerine depolanan verileri basitleştirir. Dosya sanal yolu kullanarak istemcilere isteği olduğundan, depolama sistemleri eklenebilir veya istemci iş akışını değiştirmek zorunda kalmadan değiştirildi. 

Küme ad alanı, Bulut ve NAS depolama sistemleri benzer bir dosya yapısı içinde sunmak da sağlar. 

Kümenin vservers, ad alanı ve istemcilerine içeriği sunan korur. Küme ad alanı oluşturmak için iki adımı vardır: 

1. Bir vserver oluşturma 
1. Arka uç depolama sistemleri ve istemciye yönelik dosya sistemi yolları arasında merkezleriyle ayarlama 

### <a name="create-a-vserver"></a>Bir vserver oluşturma

VServers kümenin çekirdeği filtrelerin ve istemci arasında verilerin nasıl aktığını denetlemek sanal dosya sunucularıdır:

* VServers ana bilgisayar istemci kullanıma yönelik IP adresleri
* VServers ad alanı oluşturma ve arka uç depolama dışarı aktarmaya yönelik istemci sanal dizin yapısı eşleyen merkezleriyle tanımlayın
* Çekirdek dosyalayıcı verme ilkeleri ve kullanıcı kimlik doğrulama sistemleri gibi dosya erişim denetimleri VServers zorla
* VServers SMB altyapısı sağlar.

Bir küme vserver yapılandırmak başlatmadan önce bağlantılı belgeleri okuyun ve Yardım anlama ad alanı ve vservers için Microsoft temsilcinize başvurun. VLAN kullanıyorsanız [oluşturduktan](fxt-configure-network.md#adjust-network-settings) vserver oluşturmadan önce. 

Küme yapılandırma Kılavuzu'nun bu bölümler, FXT vserver ve genel ad alanı özellikleri ile kendinizi alıştırın yardımcı olur:

* [Oluşturma ve VServers ile çalışma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/settings_overview.html#creating-and-working-with-vservers)
* [Genel bir Namespace kullanma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gns_overview.html)
* [Bir VServer oluşturma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_vserver_manage.html#creating-a-vserver)

Kümeniz için en az bir vserver ihtiyacınız var. 

Yeni bir vserver oluşturmak için aşağıdaki bilgiler gereklidir:

* Vserver için ayarlanacak adı

* İstemciye yönelik IP adresleri aralığını vserver işleyecek

  Vserver oluşturduğunuzda, tek bir bitişik IP adres aralığı sağlamanız gerekir. Kullanarak, daha sonra bir daha fazla adres ekleyebilirsiniz **istemci bakan ağ** Ayarları sayfası.

* Ağınızda VLAN'ları, bu vserver için kullanılacak bir VLAN hangi varsa

Kullanım **VServer** > **yönetme VServers** yeni vserver oluşturmak için ayarları sayfası. Ayrıntılar okumak için [bir VServer oluşturma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_vserver_manage.html#creating-a-vserver) küme yapılandırma kılavuzu. 

![Yeni bir vserver oluşturmak için açılır pencere](media/fxt-cluster-config/new-vserver.png)

### <a name="create-a-junction"></a>Bir bağlantı oluşturun

A *birleşim* bir arka uç depolama yolu istemci görünen ad alanına eşler.

Bu sistem, istemci bağlama noktaları kullanılan yolu basitleştirmek ve bir sanal yol birden çok çekirdek filtrelerin depolamadan uyum için kapasite sorunsuz bir şekilde ölçeklendirmek için kullanabilirsiniz.

![Doldurulmuş ayarlarla yeni bir birleşim Sihirbaz Sayfası Ekle](media/fxt-cluster-config/add-junction-full.png)

Başvurmak [ **VServer** > **Namespace** ](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_namespace.html) ad alanı birleşim oluşturma hakkında tam Ayrıntılar için küme yapılandırma Kılavuzu'nda.

![VServer > için bir birleşim ayrıntılarını gösteren Namespace Ayarları sayfası](media/fxt-cluster-config/namespace-populated.png)

## <a name="configure-export-rules"></a>Dışarı aktarma kuralları yapılandırma

Hem bir vserver hem de çekirdek dosyalayıcı sonra verme kurallarını özelleştirme ve istemcilerin çekirdek dosyalayıcı dışarı aktarmaları dosyalarda nasıl erişebileceğini denetlemek ilkeleri ver gerekir.

İlk olarak, **VServer** > **dışarı aktarma kuralları** sayfası varsayılan ilkesini değiştirmek için ya da kendi özel dışarı aktarma ilkesi oluşturmak için yeni kurallar ekleyebilir.

İkinci olarak, kullanın **VServer** > **verme ilkeleri** bu vserver erişildiğinde çekirdek dosyalayıcı'nın dışarı özelleştirilmiş ilkeyi uygulamak için sayfa.

Küme yapılandırma kılavuzu makaleyi okuyun [çekirdek dosyalayıcı dışarı aktarmaları erişimi denetleme](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/export_rules_overview.html) Ayrıntılar için.


## <a name="next-steps"></a>Sonraki adımlar

Depolama alanı eklemeye ve istemciye yönelik ad alanı yapılandırdıktan sonra kümenin ilk Kurulumu tamamlayın: 

> [!div class="nextstepaction"]
> [Küme ağ ayarlarını yapılandırma](fxt-configure-network.md)
