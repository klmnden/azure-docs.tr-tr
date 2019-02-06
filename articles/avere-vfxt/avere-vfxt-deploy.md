---
title: Azure için Avere vFXT dağıtma
description: Azure'da Avere vFXT kümesi dağıtma adımları
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: 972ba937ad15fa9a6d2eb74e3e4c9e6e8f3923a4
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745444"
---
# <a name="deploy-the-vfxt-cluster"></a>vFXT kümesini dağıtma

Bu yordamı, Azure Marketi'nde kullanılabilir Dağıtım Sihirbazı kullanarak adımları açıklanmaktadır. Sihirbaz, bir Azure Resource Manager şablonu kullanarak küme otomatik olarak dağıtır. Parametreleri girin ve tıklayın sonra **Oluştur**, Azure, otomatik olarak aşağıdaki adımları gerçekleştirir: 

* Kümeyi yönetmek ve dağıtmak için gereken yazılımları içeren temel bir VM kümesi denetleyiciyi oluşturun.
* Kaynak grubunu ve gerekirse yeni öğeler oluşturma dahil, sanal ağ altyapısı ayarlayın.
* Küme düğümü sanal makineleri oluşturun ve bunları Avere kümesi olarak yapılandırın.
* İstenirse, yeni bir Azure Blob kapsayıcısı oluşturmak ve bir küme çekirdek dosyalayıcı yapılandırın.

Bu belgedeki yönergeleri uyguladıktan sonra bir sanal ağ, bir alt ağ, bir denetleyici ve aşağıdaki çizimde gösterildiği gibi bir vFXT küme olacaktır:

![İsteğe bağlı bir blob depolama ve üç içeren bir alt ağ içeren sanal ağ gösteren diyagram vFXT düğümleri/vFXT kümesi ve bir VM etiketli küme denetleyicisi etiketlenmiş Vm'lere gruplandırılmış](media/avere-vfxt-deployment.png)

Küme oluşturulduktan sonra yapmanız gerekenler [depolama uç noktası oluşturma](#create-a-storage-endpoint-if-using-azure-blob) Blob Depolama kullanıyorsanız, sanal ağınızda. 

Oluşturma şablonu kullanmadan önce şu önkoşulların giderdik emin olun:  

1. [Yeni Abonelik](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Abonelik sahibi izinleri](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Kota vFXT kümesi için](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)
1. [Özel erişim rolleri](avere-vfxt-prereqs.md#create-access-roles) -küme düğümlerine atamak için rol tabanlı erişim denetimine rol oluşturmanız gerekir. Ayrıca küme denetleyicisi için özel erişim rol oluşturma seçeneğiniz vardır, ancak çoğu kullanıcı için bir kaynak grubu sahibi karşılık gelen denetleyicisi ayrıcalıklar verir varsayılan sahip rolü götürür. Okuma [Azure kaynakları için yerleşik roller](../role-based-access-control/built-in-roles.md#owner) daha fazla ayrıntı için.

Küme dağıtım adımları ve planlama hakkında daha fazla bilgi için okuma [Avere vFXT sisteminizi planlama](avere-vfxt-deploy-plan.md) ve [dağıtımına genel bakış](avere-vfxt-deploy-overview.md).

## <a name="create-the-avere-vfxt-for-azure"></a>Azure için Avere vFXT oluşturma

Oluşturma şablonu Azure portalında erişim için Avere arama ve seçme "Avere vFXT ARM dağıtım". 

!["Yeni > Market > her şey" ekmek Azure portalıyla gösteren tarayıcı penceresinde kalbimdeki. İçinde her şey sayfa, arama alanına "avere" terimi ve ikinci sonuç yok "Avere vFXT ARM dağıtım" vurgulamak için kırmızı renkle.](media/avere-vfxt-template-choose.png)

Sayfadaki Avere vFXT ARM dağıtım ayrıntıları okuduktan sonra tıklayın **Oluştur** başlamak için. 

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

* **Rol Kimliği Avere küme oluşturma** -küme denetleyici için erişim denetimi rolü belirtmek için bu alanı kullanın. Varsayılan değer: yerleşik rolü [sahibi](../role-based-access-control/built-in-roles.md#owner). Küme kaynağı grubuna küme denetleyici için sahip ayrıcalıkları kısıtlanır. 

  Rolüne karşılık gelen genel benzersiz tanımlayıcısını kullanmanız gerekir. Varsayılan değer (sahibi) 8e3af657-a8ff-443 c-a75c-2fe8c4bcb635 bir GUID'dir. Özel bir rol için GUID'i bulmak için bu komutu kullanın: 

  ```azurecli
  az role definition list --query '[*].{roleName:roleName, name:name}' -o table --name 'YOUR ROLE NAME'
  ```

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

* **Avere küme işlemleri rolünü** -küme düğümleri için erişim denetimi rolü adını belirtin. Bu gerekli bir adım olarak oluşturulan özel bir roldür. 

  Örnekte açıklandığı [küme düğümü erişim rolünü oluşturma](avere-vfxt-prereqs.md#create-the-cluster-node-access-role) dosyası olarak kaydeder ```avere-operator.json``` ve karşılık gelen rol adı ```avere-operator```.

* **Avere vFXT küme adı** -küme benzersiz bir ad verin. 

* **Boyutu** -küme düğümleri oluşturulurken kullanılacak VM türü belirtin. 

* **Önbellek boyutu düğüm başına** -toplam önbellek boyutunu Avere vFXT kümenizdeki düğümleri sayı ile çarpılan düğüm başına önbellek boyutu böylece küme önbellek küme düğümleri arasında yayılır. 

  Önerilen yapılandırma Standard_D16s_v3 küme düğümleri kullanıyorsanız, düğüm başına 1 TB kullanın ve Standard_E32s_v3 düğümleri kullanarak, düğüm başına 4 TB kullanılacak sağlamaktır.

* **Sanal ağ** - küme barındırmak için var olan bir vnet seçin veya yeni bir vnet oluşturmak için tanımlayın. 

  > [!NOTE]
  > Yeni bir vnet oluşturun, yeni özel ağa erişebilmesi için küme denetleyicisi bir genel IP adresi gerekir. Mevcut bir sanal ağı seçerseniz, küme denetleyicisi bir genel IP adresi olmadan yapılandırılır. 
  > 
  > Herkese görünür bir IP adresi kümesi denetleyicisinde vFXT kümesine kolay erişim sağlar, ancak küçük bir güvenlik riski oluşturur. 
  >  * Bir genel IP adresi kümesi denetleyicisinde Avere vFXT küme dışında özel alt ağa bağlanmak için bir atlama konağı olarak kullanmanıza olanak sağlar.
  >  * Genel bir IP adresi denetleyicisinde ayarlamazsanız, kümeye erişmek için başka bir atlama konak, bir VPN bağlantısı veya ExpressRoute kullanmanız gerekir. Örneğin, önceden yapılandırılmış bir VPN bağlantısı olan bir sanal ağ içindeki denetleyiciyi oluşturun.
  >  * Genel bir IP adresi ile bir denetleyici oluşturursanız, bir ağ güvenlik grubuyla denetleyicisi VM'SİNİN korumanız gerekir. Varsayılan olarak Avere vFXT Azure dağıtımı için bir ağ güvenlik grubu oluşturur ve yalnızca bağlantı noktası 22 genel IP adresleri ile denetleyicileri için gelen erişimi kısıtlar. Sistem kilitleyerek daha iyi koruyabilirsiniz, IP kaynak adresi aralığı - erişim tuşunu diğer bir deyişle, yalnızca gelen bağlantılara makineler kümeye erişim için kullanmayı düşündüğünüz izin verin.

* **Alt ağ** - var olan sanal ağınızdan bir alt ağ seçin veya yeni bir tane oluşturun. 

* **BLOB Depolama kullanma** -seçin **true** yeni bir Azure Blob kapsayıcısı oluşturmak ve bunu yeni Avere vFXT küme için arka uç depolama olarak yapılandırmak için. Bu seçenek ayrıca küme ile aynı kaynak grubunda yeni bir depolama hesabı oluşturur. 

  Bu alan kümesine **false** yeni bir kapsayıcı oluşturmak istemiyorsanız. Bu durumda, ekleme ve Küme oluşturulduktan sonra depolama yapılandırmanız gerekir. Okuma [depolamayı yapılandırma](avere-vfxt-add-storage.md) yönergeler için. 

* **Depolama hesabı** : yeni bir Azure Blob kapsayıcısı oluşturma girin, yeni depolama hesabı için bir ad. 

## <a name="validation-and-purchase"></a>Doğrulama ve satın alma

Sayfa üç yapılandırma özetini sunar ve parametrelerini doğrular. Doğrulama başarılı olduktan sonra tıklayın **Tamam** devam etmek için düğmesine. 

![Üçüncü sayfasında, dağıtım şablonu - doğrulama](media/avere-vfxt-deploy-3.png)

Dört sayfasında tıklayın **Oluştur** koşullarını kabul edin ve Azure kümesine için Avere vFXT oluşturma düğmesi. 

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


## <a name="create-a-storage-endpoint-if-using-azure-blob"></a>Depolama uç noktası (Azure Blob kullanıyorsanız) oluşturma

Arka uç veri depolama için Azure Blob Depolama kullanıyorsanız, sanal ağınızda bulunan bir depolama hizmet uç noktası oluşturmanız gerekir. Bu [hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) Azure Blob trafiği sanal ağ dışında yönlendirme yerine yerel tutar getirin.

1. Portalda, **sanal ağlar** soldaki.
1. Sanal ağ denetleyiciniz için seçin. 
1. Tıklayın **hizmet uç noktalarını** soldaki.
1. Tıklayın **Ekle** en üstünde.
1. Hizmet olarak bırakın ``Microsoft.Storage`` ve denetleyicinin alt ağ seçin.
1. Alt kısmında tıklayın **Ekle**.

  ![Hizmet uç noktası oluşturma adımları için ek açıklamalar ile Azure portal ekran görüntüsü](media/avere-vfxt-service-endpoint.png)

## <a name="next-step"></a>Sonraki adım

Kümenin çalıştığından ve yönetim IP adresini bilmeniz göre yapabilecekleriniz [Küme Yapılandırma Aracı'na bağlanma](avere-vfxt-cluster-gui.md) desteğini etkinleştirmek için depolama gerekli ve diğer küme ayarlarını özelleştirme ekleyin.
