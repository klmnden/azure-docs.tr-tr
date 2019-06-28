---
title: Microsoft Azure FXT Edge dosyalayıcı küme oluşturma
description: Azure FXT Edge dosyalayıcı ile karma depolama önbellek kümesi oluşturma
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 1bfe8f0efce0a844263fc65df0ad927114886769
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450544"
---
# <a name="tutorial-create-the-azure-fxt-edge-filer-cluster"></a>Öğretici: Azure FXT Edge dosyalayıcı kümesi oluşturma

Yükleyip Azure FXT Edge dosyalayıcı donanım düğümleri başlatmak için önbelleğinizi sonra FXT küme yazılımı, önbellek kümeyi oluşturmak için kullanın. 

Bu öğreticide bir küme olarak donanım düğümlerinizi yapılandırma adımlarında size kılavuzluk eder. 

Bu öğreticide şunları öğreneceksiniz: 

> [!div class="checklist"]
> * Hangi bilgilerin başlatmadan önce kümeyi oluşturmak gereklidir
> * Kümenin yönetim ağı, küme ağı ve istemciye yönelik ağ arasındaki fark
> * Bir küme düğümüne bağlanma 
> * Bir Azure FXT Edge dosyalayıcı düğümü kullanarak bir ilk küme oluşturma
> * Küme Denetim Masası'na küme ayarlarını yapılandırmak için oturum açma

Bu yordamı arasında 15 alır ve 45 dakika IP tanımlamak için gerçekleştirmeniz gereken ne kadar araştırma bağlı olarak adresleri ve ağ kaynakları.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki önkoşulları tamamlayın:

* Veri merkezinizde en az üç Azure FXT Edge dosyalayıcı donanım sistemi yükle 
* Sisteme uygun gücü ve ağ kablolarını bağlama  
* En az bir Azure FXT Edge dosyalayıcı düğüm gücüyle ve [kök parolasını ayarlayın](fxt-node-password.md)

## <a name="gather-information-for-the-cluster"></a>Küme için bilgileri toplayın 

Azure FXT Edge dosyalayıcı kümeyi oluşturmak için aşağıdaki bilgilere ihtiyacınız vardır:

* Küme adı

* Küme için yönetici parolası

* IP adresleri:

  * Küme yönetimi ve ağ maskesi ve yönlendirici yönetim ağı için kullanmak için tek bir IP adresi
  * İlk ve son IP adresleri bitişik IP adresleri (düğümden düğüme) küme iletişimi için. Bkz: [IP adresi dağıtımını](#ip-address-distribution), aşağıdaki Ayrıntılar için. 
  * (İstemci'e dönük IP adresleri, Küme oluşturulduktan sonra ayarlanır.)

* Ağ altyapısı bilgileri:

  * Küme için bir DNS sunucusunun IP adresi
  * Küme için DNS etki alanı adı
  * Adı veya IP adresi için küme NTP sunucuları (bir sunucu veya üç veya daha fazla) 
  * IEEE 802.1ax etkinleştirmek isteyip istemediğinizi-2008 bağlantı kümenin arabirimleri üzerinde toplama
  * Bağlantı toplama etkinleştirirseniz IEEE 802.3ad (LACP) kullanılıp kullanılmayacağı dinamik toplama

Bu ağ altyapı öğeleri, sonra kümeyi oluşturmak, ancak oluşturma sırasında yapmak daha iyi yapılandırabilirsiniz. 

### <a name="ip-address-distribution"></a>IP adresi dağıtım

Azure FXT Edge dosyalayıcı karma depolama önbellek kümesi üç kategoriye IP adreslerini kullanır:

* Yönetim IP: Küme yönetimi için tek bir IP adresi

  Bu adres, Küme Yapılandırması yardımcı programlarını (web tabanlı Denetim Masası'nı veya XML-RPC API) erişmek için giriş noktası olarak görev yapar. Bu adresi otomatik olarak kümedeki birincil düğüme atanır ve birincil düğüm değişip değişmediğini otomatik olarak taşır.

  Denetim Masası erişmek için diğer IP adresleri kullanılabilir, ancak yönetim IP adresi, tek tek düğümler üzerinden başarısız olsa bile erişim sağlamak için tasarlanmıştır.

* Küme ağı: Küme iletişimi için bir dizi IP adresleri

  Küme ağı, küme düğümleri arasında ve (çekirdek filtrelerin) arka uç depolama biriminden dosyaları almak için iletişim için kullanılır.

  **En iyi yöntem:** Her Azure FXT Edge dosyalayıcı düğümde Küme iletişimi için kullanılan fiziksel bağlantı noktası üzerinden her bir IP adresi ayırın. Küme, belirtilen aralıktaki adresler tek tek düğümlere otomatik olarak atar.

* İstemciye yönelik ağ: İstek ve yazma dosyalara istemciler kullanan bir aralık IP adresleri

  İstemci ağ adresleri aracılığıyla küme çekirdek dosyalayıcı verilere erişmek için istemciler tarafından kullanılır. Örneğin, bir NFS İstemcisi bu adreslerden birini bağlama.

  **En iyi yöntem:** Her bir düğümde FXT serisi istemci iletişimi için kullanılan fiziksel bağlantı noktası üzerinden her bir IP adresi ayırın.

  Küme istemciye yönelik IP adresleri, bağlı düğümleri arasında mümkün olduğunca eşit olarak dağıtır.

  Kolaylık olması için istemci isteklerini adres aralığı dağıtmayı kolaylaştırmak için hepsini bir kez deneme DNS (RRDNS) yapılandırmasına sahip tek bir DNS adı birçok Yöneticiler yapılandırın. Bu ayar, ayrıca tüm istemcilerin, kümeye erişmek için aynı bağlama komutu kullanmasını sağlar. Okuma [yapılandırma](fxt-configure-network.md#configure-dns-for-load-balancing) daha fazla bilgi için.

Yeni bir küme oluşturmak için yönetim IP adresi ve bir küme ağ adres aralığı belirtilmelidir. İstemciye yönelik adresleri, Küme oluşturulduktan sonra belirtilir.

## <a name="connect-to-the-first-node"></a>İlk düğümüne bağlanma

Yüklü FXT düğümlerinden herhangi birine bağlanın ve kümeyi oluşturmak için işletim sistemi yazılımını kullanın.

Varsa zaten, güç FXT düğümleri, kümeniz için en az birinde yapmadıysanız ve bir ağ bağlantısı ve bir IP adresi olduğundan emin olun. Düğüm etkinleştirin, böylece adımları yeni bir kök parola ayarlamanız gerekir [donanım parola atama](fxt-node-password.md) zaten yapmadıysanız.

Ağ bağlantısını denetlemek için düğümün ağ bağlantısı LED'lerini ışıklı (ve gerekirse, ağ üzerinde göstergeleri bağlı olduğu için geçiş,) emin olun. Gösterge LED'lerini açıklanmıştır [İzleyici Azure FXT Edge dosyalayıcı donanım durumunu](fxt-monitor.md).

Düğüm önyüklendiğinde, bir IP adresi ister. Bir DHCP sunucusuna bağlı ise, DHCP tarafından sağlanan IP adresini kabul eder. (Bu IP adresi geçicidir. Kümeyi oluşturduğunuzda, farklı olacaktır.)

Bir DHCP sunucusuna bağlı değil veya bir yanıt almaz, düğüm Bonjour yazılım 169.254 biçiminde kendi kendine atanan bir IP adresi ayarlamak için kullanır. \*. \*. Ancak, geçici bir statik IP adresi düğümün ağ kartları birinde bir küme oluşturmak için kullanmadan önce ayarlamanız gerekir. Eski bu belgede yönergeler bulunmaktadır. güncelleştirilmiş bilgiler için Microsoft Service ve Destek ekibine başvurun: [Ek A: Bir FXT düğüm üzerinde bir statik IP adresi ayarlama](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/static_ip.html).

### <a name="find-the-ip-address"></a>IP adresi Bul

IP adresini bulmak için Azure FXT Edge dosyalayıcı düğümüne bağlanın. Bir seri kablo, USB ve VGA bağlantı noktalarına doğrudan bağlantı kullanın veya KVM anahtar aracılığıyla bağlanın. (Bağlantı için bilgi [ilk parola atama](fxt-node-password.md).)

Bağlandıktan sonra kullanıcı adıyla oturum `root` ve düğümü ilk kez önyüklendiğinde, ayarladığınız parolayı.  

Oturum açtıktan sonra düğümün IP adresi belirlemeniz gerekir.

Komutunu `ifconfig` bu sisteme atanan adresi görmek için.

Örneğin, komut `ifconfig | grep -B5 inet` Internet adreslerine sahip bağlantı noktalarını arar ve beş satırlık bir bağlantı noktası tanımlayıcısına göstermek için bağlam sağlar.

İfconfig raporda herhangi bir IP adresi yazın. Bağlantı noktası adları ile listelenen adresler e0a ister ya da e0b iyi seçenekleri. IPMI bağlantı noktaları için bu adları yalnızca kullanılmadığından e7 * adları ile listelenen herhangi bir IP adresi kullanmayın normal olmayan bir ağ bağlantı noktaları.  

## <a name="load-the-cluster-configuration-wizard"></a>Küme Yapılandırma Sihirbazı'nı yükleme

Kümeyi oluşturmak için tarayıcı tabanlı bir küme yapılandırma aracını kullanın. 

Bir web tarayıcısında düğümü için IP adresini girin. Tarayıcı güvenilmeyen site hakkında bir ileti veriyorsa, siteye yine de devam edin. (Tek tek Azure FXT Edge dosyalayıcı düğümleri CA tarafından sağlanan güvenlik sertifikalarını yoktur.)

![Denetim Masası sayfasının tarayıcı penceresinde oturum açma](media/fxt-cluster-create/unconfigured-node-gui.png)

Bırakın **kullanıcıadı** ve **parola** alanları boş. Tıklayın **oturum açma** küme oluşturma sayfası yüklenemedi.

![Tarayıcı tabanlı GUI Denetim Masası'nda yapılandırılmamış bir düğüm için ilk Kurulum Ekranı. Bu yazılım güncelleştirmesi, küme el ile yapılandırmak veya bir kurulum dosyası bir kümeden yapılandırma seçeneklerini gösterir.](media/fxt-cluster-create/setup-first-screen.png)

## <a name="create-the-cluster"></a>Kümeyi oluşturma

Küme Yapılandırması aracını, ekranlar Azure FXT Edge dosyalayıcı kümeyi oluşturmak için bir dizi aracılığıyla size yol gösterir. Olduğundan emin olun [gerekli bilgileri](#gather-information-for-the-cluster) başlatmadan önce hazır. 

### <a name="creation-options"></a>Oluşturma seçenekleri

İlk ekrana üç seçenek sunar. Destek personeli özel yönergeleri olmadığı sürece el ile yapılandırma seçeneğini kullanın.

Tıklayın **kümeyi el ile yapılandırır** yeni küme yapılandırma seçenekleri ekranının yüklenmesine kadar. 

Diğer seçenekleri nadiren kullanılır:

* "Sistem görüntüsünü güncelleştir" küme oluşturmadan önce yeni bir işletim sistemi yazılım yüklemenizi ister. (Ekranın en üstünde şu anda yüklü yazılım sürümü listelenir.) Bir URL ve kullanıcı adı/parola, ya da yazılım paketi dosyası - komutunu sağlamalıdır veya bilgisayarınızı bir dosyadan karşıya yüklemek. 

* Küme kurulum dosyası seçeneği, bazen Microsoft Müşteri Hizmetleri ve desteği tarafından kullanılır. 

## <a name="cluster-options"></a>Küme seçenekleri

Sonraki ekranda yeni küme için seçenekleri yapılandırmak isteyip istemediğinizi sorar.

Sayfa iki ana bölüme ayrılır **temel yapılandırma** ve **ağ yapılandırması**. Ağ yapılandırma bölümü alt bölümleri de vardır: biri **Yönetim** ağ, diğeri de **küme** ağ.

### <a name="basic-configuration"></a>Temel yapılandırma

Üst kısmında, yeni küme için temel bilgileri doldurun.

![Tarayıcı GUI sayfası "Temel yapılandırma" bölümünde ayrıntı. Üç alan gösterir (küme adı, yönetici parolası onaylama parolası)](media/fxt-cluster-create/basic-configuration.png) 

* **Küme adı** -kümesi için benzersiz bir ad girin.

  Küme adı, şu ölçütleri karşılamalıdır:
  
  * 1 ile 16 karakter uzunluğu
  * Harf, sayı ve tire (-) ve alt çizgi (_) karakterleri içerebilir. 
  * Diğer bir noktalama işareti veya özel karakterler içermemelidir
  
  Bu adı daha sonra değiştirebileceğiniz **küme** > **genel kurulumu** yapılandırma sayfası. (Küme ayarları hakkında daha fazla bilgi için okuma [küme yapılandırma Kılavuzu](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/ops_conf_index.html), bu belgeleri kümesinin bir parçası değil.)

  > [!NOTE] 
  > Küme adınız, şirketinizin adını içerecek şekilde yararlıdır izleme veya sorun giderme için desteklenebilmesi için karşıya yüklenen sistem bilgileri tanımlamak için kullanılır.

* **Yönetici parolası** -varsayılan yönetici kullanıcı için parolayı ayarlayın `admin`.
  
  Bireysel kullanıcı hesapları kümesi yöneten her kişi için ayarlamanız gerekir, ancak kullanıcı kaldıramazsınız `admin`. Olarak oturum `admin` ek kullanıcılar oluşturmanız gerekiyorsa.
 
  Parolasını değiştirebileceğiniz `admin` içinde **Yönetim** > **kullanıcılar** kümedeki Denetim Masası Ayarları sayfası. Ayrıntılı bilgi edinmek için **kullanıcılar** belgelerinde [küme yapılandırma Kılavuzu](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_users.html).

<!-- to do: update "legacy" URLs when docs are ported to Microsoft site -->

### <a name="network-configuration"></a>Ağ yapılandırması

**Ağ** bölümü kümenin kullanacağı ağ altyapısını belirtmenizi ister. 

Yapılandırmak için iki ayrı ağa vardır:

* *Yönetim ağı* küme yapılandırma ve izleme için yönetici erişim sağlar. Burada belirtilen IP adresi, Denetim Masası'na veya SSH erişimini bağlanırken kullanılır. 

  Çoğu kümeleri yalnızca tek bir yönetim IP adresi kullanır, ancak arabirim eklemek istiyorsanız, Küme oluşturulduktan sonra bunu yapabilirsiniz.

* *Küme ağı* küme düğümleri arasında ve arka uç depolama (çekirdek filtrelerin) ile küme düğümleri arasındaki iletişim için kullanılır.

İstemciye yönelik ağ, daha sonra Küme oluşturulduktan sonra yapılandırılır.

Bu bölüm her iki ağ tarafından kullanılan DNS ve NTP sunucuları için yapılandırma da içerir.

### <a name="configure-the-management-network"></a>Yönetim ağı yapılandırma

Ayarlarında **Yönetim** bölümü, küme için yönetici erişim sağlayan ağ içindir.

![5 seçenekleri için alanları ve 1Gb yönetim ağı için bir onay kutusu "Yönetim" Bölüm ayrıntısı](media/fxt-cluster-create/management-network-filled.png)

* **Yönetim IP** -Denetim Masası kümeye erişmek için kullanacağınız IP adresini belirtin. Bu adres kümenin birincil düğüm tarafından talep edilir, ancak özgün birincil düğüm kullanılamaz duruma gelirse, otomatik olarak sağlıklı bir düğüme taşır.

  Çoğu kümede, yalnızca bir yönetim IP adresi kullanın. Birden fazla istiyorsanız kullanarak küme oluşturduktan sonra bunları ekleyebilirsiniz **küme** > **yönetim ağ** Ayarları sayfası. Daha fazla bilgi [küme yapılandırma Kılavuzu](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_admin_network.html).

* **Ağ maskesi** -yönetim ağı için ağ maskesi belirtin.

* **Yönlendirici** -yönetim ağı tarafından kullanılan varsayılan ağ geçidi adresini girin.

* **(İsteğe bağlı) VLAN etiket** - VLAN etiketlerini kümenizi kullanıyorsa, yönetim ağı için bir etiket belirtin.

  Ek VLAN ayarlarını yapılandırılmış **küme** > **VLAN** Ayarları sayfası. Okuma [VLAN'ları ile çalışma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/network_overview.html#vlan-overview) ve [küme > VLAN](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_vlan.html) daha fazla bilgi için küme yapılandırma kılavuzu.

* **MTU** - gerekirse, kümenizin yönetim ağı için en fazla iletim birimi (MTU) ayarlayın.

* **Kullanım 1Gb mgmt ağ** -yalnızca yönetim ağına bağlantı noktalarında FXT düğümlerinizi iki 1GbE ağ atamak istiyorsanız bu kutuyu işaretleyin. Bu kutuyu işaretlerseniz yoksa, en yüksek hız ve bağlantı noktası kullanılabilir yönetim ağını kullanır. 

### <a name="configure-the-cluster-network"></a>Küme ağı yapılandırma 

Küme ağ ayarlarını, küme düğümleri arasında ve çekirdek filtreleri ile küme düğümleri arasındaki trafiği için geçerlidir.

!["Küme" bölümünde, altı değerler girmek için alanlar ayrıntısı](media/fxt-cluster-create/cluster-network-filled.png)

* **İlk IP** ve **son IP** -iç küme iletişimi için kullanılacak aralığı tanımlamak IP adreslerini girin. Burada kullanılan IP adresleri, bitişik ve DHCP tarafından atanmış olması gerekir.

  Küme oluşturulduktan sonra daha fazla IP adresi ekleyebilirsiniz. Kullanım **küme** > **küme ağları** Ayarları sayfası ([küme yapılandırma kılavuzu belgeleri](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cluster_networks.html#gui-cluster-networks)).

  Değer **IP'ler sayı aralığındaki** hesaplanır ve otomatik olarak gösterilir.

* **(İsteğe bağlı) olmayan-mgmt ağ maskesi** -küme ağı için ağ maskesi belirtin. 

  Sistem Yönetim ağı için girdiğiniz ağ maskesi değeri otomatik olarak önerir. gerekirse değiştirin.

* **(İsteğe bağlı) küme yönlendirici** -küme ağı tarafından kullanılan varsayılan ağ geçidi adresi belirtin. 

  Sistem Yönetim ağı için girdiğiniz aynı ağ geçidi adresini otomatik olarak önerir.

* **(İsteğe bağlı) VLAN etiket küme** - VLAN etiketlerini kümenizi kullanıyorsa, küme ağı için bir etiket belirtin.

* **(İsteğe bağlı) olmayan-mgmt MTU** - gerekirse, küme ağınızın en fazla iletim birimi (MTU) ayarlayın.

### <a name="configure-cluster-dns-and-ntp"></a>Yapılandırma küme DNS ve NTP 

Aşağıda **küme** bölümü var olan alanları için DNS ve NTP sunucuları belirtme ve bağlantı toplama etkinleştirmek için. Bu ayarlar kümenin kullandığı tüm ağlar için geçerlidir.

![Üç alan DNS sunucuları, DNS etki alanı ve DNS arama alanları, üç alanı NTP sunucuları için DNS/NTP yapılandırması bölümle ayrıntılarını ve bağlantı toplama seçenekleri için bir açılan menü](media/fxt-cluster-create/dns-ntp-filled.png)

* **DNS sunucuları** - bir IP adresi girin veya daha fazla etki alanı adı sistemi (DNS) sunucusu.

  DNS tüm kümeleri için önerilen ve gerekli SMB, AD kullanmak istiyorsanız veya Kerberos. 
  
  Bölümünde anlatıldığı gibi hepsini bir kez deneme Yük Dengeleme için DNS sunucusu kümenin en iyi performans için yapılandırma [Azure FXT Edge dosyalayıcı kümesi için DNS yapılandırma](fxt-configure-network.md#configure-dns-for-load-balancing).

* **DNS etki alanı** -kümenin Ağ etki alanı adı girin.

* **DNS Arama** - isteğe bağlı olarak, DNS sorguları çözümlemek için sistem araması gereken ek etki alanı adlarını girin. Boşluklarla ayrılan en fazla altı etki alanı adları ekleyebilirsiniz.

* **NTP sunucuları** -sağlanan alanlarda bir veya üç ağ zaman Protokolü (NTP) sunucuları belirtin. Ana bilgisayar adlarını veya IP adreslerini kullanabilirsiniz.

* **Bağlantı toplama** -bağlantı toplama ethernet bağlantı noktası FXT küme düğümlerinde çeşitli trafiği nasıl işleneceğini özelleştirmenize olanak sağlar. Daha fazla bilgi edinmek için [bağlantı toplama](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cluster_general_setup.html#link-aggregation) küme yapılandırma kılavuzu.

### <a name="click-the-create-button"></a>Oluştur düğmesine tıklayın

Gerekli tüm ayarları biçiminde girdikten sonra tıklayın **küme oluşturma** düğmesi.

![Oluştur düğmesine sağ alt köşede, imleci ile tamamlanan formun altı](media/fxt-cluster-create/create-cluster.png)

Sistem, kümeyi oluştururken bir ileti görüntüler.

![tarayıcıda küme yapılandırma durum iletisi: "Küme FXT düğüm artık oluşturuyor. Bu işlem birkaç dakika sürer. Küme oluşturulduğunda, yapılandırmayı tamamlamak için bu bağlantıyı ziyaret edin." Köprüyü ile "Bu bağlantıyı ziyaret edin"](media/fxt-cluster-create/creating-message.png)

Birkaç dakika sonra Denetim Masası kümeye gitmek için ileti bağlantıya tıklayabilirsiniz. (Bu bağlantı, belirttiğiniz IP adresi götürür **yönetim IP**.) Oluştur düğmesine tıkladıktan sonra etkin duruma gelmesi 15 saniye olarak bağlantı için bir dakika sürer. Web arabirimi yüklenmezse, birkaç saniye daha bekleyin ve ardından bağlantısına yeniden tıklayın. 

Küme oluşturma veya daha fazla bir dakika sürer, ancak işlem devam ederken Denetim Masası'na oturum açabilirsiniz. Küme oluşturma işlemi tamamlanana kadar uyarıları göstermek Denetim Masası'nın Pano sayfası için normal bir durumdur. 

## <a name="open-the-settings-pages"></a>Ayarları sayfaları açın 

Kümeyi oluşturduktan sonra iş akışı ve ağ yapılandırmasını özelleştirmek gerekir. 

Yeni küme oluşturma için Denetim Masası web arabirimini kullanın. Küme oluşturma durumu ekranından bağlantıyı izleyin veya küme üzerinde ayarladığınız yönetim IP adresine göz atabilirsiniz.

Web arabirimi kullanıcı adıyla oturum `admin` ve ne zaman ayarladığınız parolayı kümesi oluşturma.

![Denetim Masası oturum açma alanlarını gösteren bir web tarayıcısı](media/fxt-cluster-config/admin-login.png)

Denetim Masası açılır ve gösterir **Pano** sayfası. Küme oluşturma bittikçe herhangi bir uyarı iletisi ekrandan temizlemelidir.

Tıklayın **ayarları** kümesini yapılandırmak için sekmesinde.

Üzerinde **ayarları** sekmesinde, sol kenar menüsü yapılandırma sayfalarını gösterir. Sayfalar, kategoriye göre düzenlenir. Tıklayın + veya - üst kategori adının genişletin veya her bir sayfayı gizleyin.

![Ayarlar sekmesinde kümeyle (tarayıcıdaki) Denetim Masası > Genel Kurulum sayfasına yüklendi](media/fxt-cluster-config/settings-tab-populated.png)

## <a name="cluster-setup-steps"></a>Küme kurulum adımları

Bu noktada işlem sırasında kümeniz var, ancak yalnızca bir düğüm, istemci'e dönük IP adresleri ve hiçbir arka uç depolama sahiptir. Yeni oluşturulan küme, iş akışı işlemeye hazır olan bir önbellek sistemine gitmek için ek adımlar uygulamanız gerekir.

### <a name="required-configuration"></a>Gerekli yapılandırma

Bu adımlar, çoğu veya tamamı kümeler için gereklidir. 

* Küme Düğümleri Ekle 

  Üç düğüm standart olması, ancak çoğu üretim kümeleri daha - en çok 24 düğümleri sahip.

  Okuma [küme düğümleri Ekle](fxt-add-nodes.md) kümenize diğer Azure FXT Edge dosyalayıcı birimlerini eklemek ve yüksek kullanılabilirliği etkinleştirme hakkında bilgi edinmek için.

* Arka uç depolama belirtin

  Ekleme *dosyalayıcı çekirdek* kümenin kullanacağı her arka uç depolama sistemi için tanımlar. Okuma [arka uç depolama alanı ekleme ve sanal ad alanı yapılandırma](fxt-add-storage.md#about-back-end-storage) daha fazla bilgi için.

* İstemci erişimi ve sanal ad alanı ayarlama 

  En az bir sanal sunucu (vserver) oluşturmak ve kullanmak istemci makineler için IP adresi aralığı atayabilirsiniz. (Genel Namespace ya da GNS olarak da adlandırılır), küme ad alanı da yapılandırmalısınız olanak tanıyan bir sanal dosya sistemi özelliğine harita arka uç depolama dışarı aktarmaları için sanal yol. Arka uç depolama medyası geçiş olsa bile küme ad alanı istemciler tutarlı ve erişilebilir bir dosya sistemi yapısı sağlar. Ad alanı, Azure Blob kapsayıcıları için bir kullanıcı dostu bir sanal depolama hiyerarşisi veya başka bir desteklenen bulut nesnesi depolama de sağlayabilirsiniz.

  Okuma [ad alanı yapılandırma](fxt-add-storage.md#configure-the-namespace) Ayrıntılar için. Bu adım içerir:
  * Vservers oluşturma
  * İstemci ağ depolama görünümü ve arka uç arasındaki merkezleriyle ayarlama 
  * Tanımlama istemci IP adresleri her vserver tarafından sunulur

  > [!Note] 
  > Önemli planlama, kümenin GNS başlamadan önce önerilir. Okuma [genel Namespace kullanılarak](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gns_overview.html) ve [oluşturmak ve VServers ile çalışmak](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/settings_overview.html#creating-and-working-with-vservers) Yardım için küme yapılandırma kılavuzu olarak bölümlerde.

* [Ağ ayarları](fxt-configure-network.md)

  Doğrulanmış veya yeni bir küme için özelleştirilmiş ağla ilgili birkaç ayar vardır. Okuma [ağ ayarlarını](fxt-configure-network.md) bu öğeler hakkında ayrıntılı bilgi için:

  * DNS ve NTP yapılandırması doğrulanıyor 
  * Dizin Hizmetleri, gerekirse yapılandırma 
  * VLAN'ları ayarlama
  * Proxy sunucularını yapılandırma
  * Küme ağı IP adresleri ekleme
  * Şifreleme sertifikaları depolama

* [Destek izleme işlevini ayarlama](#enable-support)

  Yapılandırma Aracı için gizlilik ilkesini kabul etmeniz gerekir ve aynı anda destek karşıya yükleme ayarlarınızı yapılandırmanız gerekir.

  Küme istatistikleri de dahil olmak üzere ve hata ayıklama dosyaları kümeniz hakkında sorun giderme verilerini otomatik olarak karşıya yükleyebilirsiniz. Bu yüklemeler, Microsoft Müşteri Hizmetleri ve desteği sağlamak en iyi hizmeti sağlar. Hangi izlenir ve isteğe bağlı olarak proaktif destek ve sorun giderme uzak hizmet etkinleştir özelleştirebilirsiniz.  

### <a name="optional-configuration"></a>İsteğe bağlı yapılandırma

Bu adımları tüm kümeler için gerekli değildir. Bunlar, bazı iş akışları veya belirli küme yönetimi stilleri türleri için gereklidir. 

* Düğüm ayarları özelleştirme

  Düğüm adları Ayarla ve düzeyi ya da tek tek bir küme genelinde düğüm IPMI bağlantı noktalarını yapılandırın. Düğümleri kümeye eklemeden önce bu ayarları yapılandırmak, yeni düğümleri bunlar katıldığında ayarlarını otomatik olarak seçebilirsiniz. Seçenekler, eski küme oluşturma belgede açıklanan [düğüm ayarları özelleştirme](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_node.html).

  > [!TIP]
  > Bu ürün için bazı belgelerde Microsoft Azure belgeleri sitesindeki henüz kullanılamaz. Bağlantılar [küme yapılandırma Kılavuzu](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/ops_conf_index.html) ve eski sürümü [küme oluşturma Kılavuzu](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/create_index.html) sizi Github'da barındırılan ayrı bir Web sitesine yönlendirir. 

* SMB yapılandırın

  NFS yanı sıra, küme SMB erişmesine izin vermek istiyorsanız, SMB yapılandırmak ve açın. SMB (CIFS olarak da adlandırılır), genellikle Microsoft Windows istemcileri desteklemek için kullanılır.

  Planlama ve yapılandırma SMB birden fazla Denetim Masası'nda birkaç düğmelere tıklamak içerir. Sistem gereksinimlerine bağlı olarak, SMB çekirdek filtrelerin, oluşturduğunuz kaç vservers nasıl tanımladığınızı etkileyebilir nasıl, merkezleriyle ve ad alanı, erişim izinleri ve diğer ayarları yapılandırın.

  Daha fazla bilgi için küme yapılandırma kılavuzu okuyun [SMB erişimi yapılandırma](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/smb_overview.html) bölümü.

* Ek lisans yükleyin

  Azure Blob dışındaki bulut depolama kullanmak istiyorsanız, bir ek özellik lisansı yüklemeniz gerekir. Bir FlashCloud satın alma hakkında ayrıntılı bilgi için Microsoft temsilcinize başvurun<sup>TM</sup> lisans. İçinde açıklanan ayrıntıları [arka uç depolama alanı ekleme ve sanal ad alanı yapılandırma](fxt-add-storage.md#about-back-end-storage).


### <a name="enable-support"></a>Desteğini etkinleştir

Azure FXT Edge dosyalayıcı küme desteği veri kümeniz hakkında otomatik olarak karşıya yükleyebilirsiniz. Bu yüklemeler sağlamak en iyi olası müşteri hizmetleri personeli olanak tanır.

Destek karşıya ayarlamak için aşağıdaki adımları izleyin.

1. Gidin **küme** > **Destek** Ayarları sayfası. Gizlilik ilkesini kabul edin. 

   ![Gizlilik ilkesini kabul etmek için Denetim Masası ve Onayla düğmesi açılır pencereyi gösteren ekran görüntüsü](media/fxt-cluster-config/fxt-privacy-policy.png)

1. Sol tarafındaki üçgeni **müşteri bilgileri** bölümü genişletin.
1. Tıklayın **Revalidate karşıya yükleme bilgileri** düğmesi.
1. Kümenin destek adı kümesinde **benzersiz küme adı** -benzersiz olarak tanımladığı personel desteklemek için kümenizin emin olun.
1. İçin onay kutularını işaretleyin **istatistikleri izleme**, **genel bilgilerini karşıya**, ve **kilitlenme bilgi karşıya**.
1. **Gönder**'e tıklayın.  

   ![Müşteri bilgileri bölümü destek ayarları sayfasının ekran görüntüsü içeren tamamlandı](media/fxt-cluster-config/fxt-support-info.png)

1. Sol tarafındaki üçgeni **güvenli proaktif destek (SPS)** bölümü genişletin.
1. İçin kutuyu **etkinleştir SPS bağlantısını**.
1. **Gönder**'e tıklayın.

   ![Destek Ayarları sayfasında tamamlanmış güvenli proaktif destek bölüm içeren ekran görüntüsü](media/fxt-cluster-config/fxt-support-sps.png)

## <a name="next-steps"></a>Sonraki adımlar

Temel küme oluşturup gizlilik ilkesini kabul sonra kalan küme düğümlerine ekleyin. 

> [!div class="nextstepaction"]
> [Küme Düğümleri Ekle](fxt-add-nodes.md)
