---
title: Azure için Avere vFXT dağıtma
description: Azure'da Avere vFXT kümesi dağıtma adımları
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: v-erkell
ms.openlocfilehash: 7ded66c29f12b8f68746726ca6c126bffbc51f0d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60410330"
---
# <a name="deploy-the-vfxt-cluster"></a>vFXT kümesini dağıtma

Bu yordamı, Azure Marketi'nde kullanılabilir Dağıtım Sihirbazı kullanarak adımları açıklanmaktadır. Sihirbaz, bir Azure Resource Manager şablonu kullanarak küme otomatik olarak dağıtır. Parametreleri girin ve tıklayın sonra **Oluştur**, Azure, otomatik olarak aşağıdaki adımları gerçekleştirir:

* Kümeyi yönetmek ve dağıtmak için gereken yazılımları içeren temel bir VM kümesi denetleyiciyi oluşturur.
* Kaynak grubunu ve sanal ağ altyapısı, yeni öğeler oluşturma gibi ayarlar.
* Küme düğümü sanal makineleri oluşturur ve bunları Avere kümesi olarak yapılandırır.
* İstenirse, yeni bir Azure Blob kapsayıcısı oluşturur ve küme çekirdek dosyalayıcı yapılandırır.

Bu belgedeki yönergeleri uyguladıktan sonra bir sanal ağ, bir alt ağ, bir denetleyici ve aşağıdaki çizimde gösterildiği gibi bir vFXT küme olacaktır. Bu diyagram bir yeni Blob Depolama kapsayıcısında (gösterilmez yeni bir depolama hesabı,) ve alt ağ içinde Microsoft depolama için hizmet uç noktası içeren isteğe bağlı Azure Blob çekirdek dosyalayıcı gösterir. 

![üç Eşmerkezli dikdörtgenler Avere küme bileşenleri gösteren diyagram. Dış dikdörtgen 'Kaynak grubu' ve '(isteğe bağlı) depolama Blob' etiketli bir Altıgene içeriyor. Sonraki dikdörtgende etiketli ' sanal ağ: 10.0.0.0/16' ve herhangi bir benzersiz bileşeni içermiyor. En içteki dikdörtgen 'Subnet:10.0.0.0/24' ve 'Küme denetleyici', 'vFXT düğümleri (vFXT küme)' olarak etiketlenen üç VM ve 'Hizmet uç noktası' etiketli bir Altıgene yığınını etiketli bir sanal makine içeriyor. Hizmet uç noktası (aynı alt ağ içinde) ve (aynı kaynak grubunda bir sanal ağ ve alt ağ dışında) blob depolama bağlama bir ok mevcuttur. Ok, sanal ağ sınırlarını ve alt ağ geçirir.](media/avere-vfxt-deployment.png)  

Oluşturma şablonu kullanmadan önce şu önkoşulların giderdik emin olun:  

1. [Yeni Abonelik](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Abonelik sahibi izinleri](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Kota vFXT kümesi için](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)
1. [(Gerekirse) depolama hizmet uç noktası](avere-vfxt-prereqs.md#create-a-storage-service-endpoint-in-your-virtual-network-if-needed) - için gerekli kullanarak mevcut bir sanal ağ ve blob depolama oluşturma dağıtır

Küme dağıtım adımları ve planlama hakkında daha fazla bilgi için okuma [Avere vFXT sisteminizi planlama](avere-vfxt-deploy-plan.md) ve [dağıtımına genel bakış](avere-vfxt-deploy-overview.md).

## <a name="create-the-avere-vfxt-for-azure"></a>Azure için Avere vFXT oluşturma

Azure portalında oluşturma şablonu Avere için arama ve "Avere vFXT Azure ARM şablonu için" seçerek erişebilirsiniz. 

!["Yeni > Market > her şey" ekmek Azure portalıyla gösteren tarayıcı penceresinde kalbimdeki. Her şeyi sayfasında, arama alanına sahip terimi "avere" ve "Avere vFXT Azure ARM şablonu için" ikinci sonucu özetlenen vurgulamak için kırmızı renkte.](media/avere-vfxt-template-choose.png)

Azure ARM şablonu sayfasının Avere vFXT ayrıntıları okuduktan sonra tıklayın **Oluştur** başlamak için. 

![Dağıtım şablonu gösteren'ın ilk sayfasında ile Azure Market](media/avere-vfxt-deploy-first.png)

Şablon dört adımı - iki bilgi toplama sayfaları ayrıca doğrulama ve onay adımları bölünür. 

* Sayfa bir VM kümesi denetleyicisinin ayarlarını odaklanır. 
* İki sayfa alt ağlar ve depolama gibi ilişkili kaynakları ve küme oluşturmak için parametreler toplar. 
* Sayfa üç ayarları özetlenmekte ve yapılandırmasını doğrular. 
* Sayfa dört yazılım hüküm ve koşullar açıklanır ve küme oluşturma işlemini başlatmanıza olanak tanır. 

## <a name="page-one-parameters---cluster-controller-information"></a>Bir parametre - küme denetleyicisi bilgileri sayfası

Dağıtım Şablonu'nın ilk sayfasında, küme denetleyiciyle ilgili bilgileri toplar. 

![Dağıtım şablonunu, ilk sayfa](media/avere-vfxt-deploy-1.png)

Aşağıdaki bilgileri doldurun:

* **Küme Denetleyici adı** -küme denetleyicisi VM adını ayarlayın.

* **Denetleyici kullanıcı adı** -VM kümesi denetleyicisi için kök kullanıcı adı girin. 

* **Kimlik doğrulama türü** -parola veya denetleyiciye bağlanmak için SSH ortak anahtarı kimlik doğrulaması'nı seçin. SSH ortak anahtar yöntemi önerilir. Okuma [oluşturma ve SSH anahtarlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) yardıma ihtiyacınız varsa.

* **Parola** veya **SSH ortak anahtarı** -seçtiğiniz kimlik doğrulaması türüne bağlı olarak, RSA ortak anahtarını veya sonraki alanları bir parola sağlamalısınız. Bu kimlik bilgisi, daha önce sağlanan kullanıcı adı ile kullanılır.

* **Abonelik** -Avere vFXT için aboneliği seçin. 

* **Kaynak grubu** - Avere vFXT kümesi için mevcut bir boş bir kaynak grubunu seçin veya "Yeni Oluştur" a tıklayın ve yeni bir kaynak grubu adı girin. 

* **Konum** -küme ve kaynaklarınıza Azure konumu seçin.

Tıklayın **Tamam** bittiğinde. 

> [!NOTE]
> Küme denetleyicisinin bir genel kullanıma yönelik IP adresine sahip istiyorsanız, var olan bir ağ seçmek yerine küme için yeni bir sanal ağ oluşturun. İki sayfada ayarıdır.

## <a name="page-two-parameters---vfxt-cluster-information"></a>İki parametre - vFXT küme bilgileri sayfası

Dağıtım şablonu ikinci sayfasında, küme boyutu, düğüm türü, önbellek boyutunu ve diğer ayarları arasında depolama parametrelerini ayarlamanıza olanak sağlar. 

![İkinci sayfasında dağıtım şablonu](media/avere-vfxt-deploy-2.png)

* **Avere vFXT küme düğümü sayısını** -kümede kullanmak için düğüm sayısını seçin. En az üç düğüm olmalıdır ve en fazla on iki. 

* **Küme yönetim parolası** -küme yönetimi için bir parola oluşturun. Bu parola kullanıcı adını kullanılacak ```admin``` küme Denetim Masası'na kümesini izlemek için ve ayarlarını yapılandırmak için oturum açmak için.

* **Avere vFXT küme adı** -küme benzersiz bir ad verin. 

* **Boyutu** -Bu bölümde küme düğümleri için kullanılacak VM türü gösterilmektedir. Yalnızca bir önerilen seçenek olmakla **değiştirme boyutu** bağlantı, bu örnek türü ve fiyatlandırma hesaplayıcısını yönelik bağlantı ayrıntılarını içeren bir tablo açar.  

* **Önbellek boyutu düğüm başına** -toplam önbellek boyutunu Avere vFXT kümenizdeki düğümleri sayı ile çarpılan düğüm başına önbellek boyutu böylece küme önbellek küme düğümleri arasında yayılır. 

  Önerilen yapılandırma, Standard_E32s_v3 düğüm için düğüm başına 4 TB kullanmaktır.

* **Sanal ağ** - küme barındırmak için yeni bir vnet tanımlayın veya açıklanan önkoşulları karşılayan mevcut bir vnet seçin [Avere vFXT sisteminizi planlama](avere-vfxt-deploy-plan.md#resource-group-and-network-infrastructure). 

  > [!NOTE]
  > Yeni bir vnet oluşturun, yeni özel ağa erişebilmesi için küme denetleyicisi bir genel IP adresi gerekir. Mevcut bir sanal ağı seçerseniz, küme denetleyicisi bir genel IP adresi olmadan yapılandırılır. 
  > 
  > Herkese görünür bir IP adresi kümesi denetleyicisinde vFXT kümesine kolay erişim sağlar, ancak küçük bir güvenlik riski oluşturur. 
  >  * Bir genel IP adresi kümesi denetleyicisinde Avere vFXT küme dışında özel alt ağa bağlanmak için bir atlama konağı olarak kullanmanıza olanak sağlar.
  >  * Genel bir IP adresi denetleyicisinde ayarlamazsanız, kümeye erişmek için başka bir atlama konak, bir VPN bağlantısı veya ExpressRoute kullanmanız gerekir. Örneğin, önceden yapılandırılmış bir VPN bağlantısı olan bir sanal ağ içindeki denetleyiciyi oluşturun.
  >  * Genel bir IP adresi ile bir denetleyici oluşturursanız, bir ağ güvenlik grubuyla denetleyicisi VM'SİNİN korumanız gerekir. Varsayılan olarak Avere vFXT Azure dağıtımı için bir ağ güvenlik grubu oluşturur ve yalnızca bağlantı noktası 22 genel IP adresleri ile denetleyicileri için gelen erişimi kısıtlar. Sistem kilitleyerek daha iyi koruyabilirsiniz, IP kaynak adresi aralığı - erişim tuşunu diğer bir deyişle, yalnızca gelen bağlantılara makineler kümeye erişim için kullanmayı düşündüğünüz izin verin.

  Dağıtım şablonu, ayrıca Küme alt ağdan yalnızca IP'ler için kilitli ağ erişim denetimi ile Azure Blob Depolama için depolama hizmet uç noktası ile yeni vnet'in yapılandırır. 

* **Alt ağ** - var olan sanal ağınızdan bir alt ağ seçin veya yeni bir tane oluşturun. 

* **Oluşturma ve blob depolama kullanma** -seçin **true** yeni bir Azure Blob kapsayıcısı oluşturmak ve bunu yeni Avere vFXT küme için arka uç depolama olarak yapılandırmak için. Bu seçenek ayrıca küme ve Küme alt ağı içinde Microsoft Depolama hizmet uç noktası olarak aynı kaynak grubunda yeni bir depolama hesabı oluşturur. 
  
  Bir sanal ağınız sağlarsanız, kümeyi oluşturmadan önce depolama hizmet uç noktası olmalıdır. (Daha fazla bilgi için okuma [Avere vFXT sisteminizi planlama](avere-vfxt-deploy-plan.md).)

  Bu alan kümesine **false** yeni bir kapsayıcı oluşturmak istemiyorsanız. Bu durumda, ekleme ve Küme oluşturulduktan sonra depolama yapılandırmanız gerekir. Okuma [depolamayı yapılandırma](avere-vfxt-add-storage.md) yönergeler için. 

* **(Yeni) Depolama hesabı** : yeni bir Azure Blob kapsayıcısı oluşturma girin, yeni depolama hesabı için bir ad. 

## <a name="validation-and-purchase"></a>Doğrulama ve satın alma

Sayfa üç yapılandırma özetler ve parametrelerini doğrular. Doğrulama başarılı olduktan sonra tıklayın **Tamam** devam etmek için düğmesine. 

![Üçüncü sayfasında, dağıtım şablonu - doğrulama](media/avere-vfxt-deploy-3.png)

Dört sayfasında, gerekli tüm iletişim bilgileri girin ve tıklayın **Oluştur** koşullarını kabul edin ve Azure kümesine için Avere vFXT oluşturma düğmesi. 

![Dağıtım şablonu - hüküm ve koşullar, dördüncü sayfasında düğme oluşturma](media/avere-vfxt-deploy-4.png)

Küme dağıtımı, 15-20 dakika sürer.

## <a name="gather-template-output"></a>Şablon çıktısı toplayın

Avere vFXT şablon kümesi oluşturma tamamlandığında, yeni kümeye hakkında bazı önemli bilgiler çıkarır. 

> [!TIP]
> Yönetim IP adresi şablonu çıktısını kopyaladığınızdan emin olun. Bu adres kümeyi yönetmek için ihtiyacınız.

Bu bilgileri bulmak için bu yordamı izleyin:

1. Küme denetleyiciniz için kaynak grubuna gidin.

1. Sol taraftaki **dağıtımları**, ardından **microsoft avere.vfxt şablon**.

   ![Microsoft-avere.vfxt-şablon dağıtım adı altında bir tablodaki gösteren ve sol seçili dağıtımları olan kaynak grubu portal sayfası](media/avere-vfxt-outputs-deployments.png)

1. Sol taraftaki **çıkışları**. Her alan değerleri kopyalayın. 

   ![etiketleri sağındaki alanları sayfasını gösteren SSHSTRING, RESOURCE_GROUP, konum, NETWORK_RESOURCE_GROUP, ağ, alt ağ, SUBNET_ID, VSERVER_IPs ve MGMT_IP değerleri çıkarır](media/avere-vfxt-outputs-values.png)

## <a name="next-step"></a>Sonraki adım

Kümenin çalıştığından ve yönetim IP adresini bilmeniz göre yapabilecekleriniz [Küme Yapılandırma Aracı'na bağlanma](avere-vfxt-cluster-gui.md) desteğini etkinleştirmek için depolama gerekli ve diğer küme ayarlarını özelleştirme ekleyin.
