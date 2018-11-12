---
title: Azure için Avere vFXT dağıtma
description: Azure'da Avere vFXT kümesi dağıtma adımları
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: eb0f5a4a4219c63334e0a5be3ea4378c3c317bec
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51288110"
---
# <a name="deploy-the-vfxt-cluster"></a>vFXT kümesini dağıtma

Azure'da vFXT küme oluşturmak için en kolay yolu, bir küme denetleyicisi kullanmaktır. Küme Denetleyicisi gerekli betikler, şablonları ve yazılım altyapı oluşturmak ve vFXT kümeyi yönetmek için içeren bir vm'dir.

Yeni bir vFXT Küme dağıtımı, aşağıdaki adımları içerir:

1. [Küme denetleyici oluşturma](#create-the-cluster-controller-vm).
1. Azure Blob Depolama kullanıyorsanız [depolama uç noktası oluşturma](#create-a-storage-endpoint-if-using-azure-blob) sanal ağınızda.
1. [Küme denetleyiciyi bağlama](#access-the-controller). Bu adımın geri kalan küme denetleyicisi VM'SİNİN gerçekleştirilir. 
1. [Erişim rolünü oluşturma](#create-the-cluster-node-access-role) küme düğümleri için. Bir prototip sağlanır.
1. [Küme oluşturma betiği özelleştirmek](#edit-the-deployment-script) vFXT küme oluşturmak istediğiniz türü.
1. [Küme oluşturma betiği yürütün](#run-the-script).

Küme dağıtım adımları ve planlama hakkında daha fazla bilgi için okuma [Avere vFXT sisteminizi planlama](avere-vfxt-deploy-plan.md) ve [dağıtımına genel bakış](avere-vfxt-deploy-overview.md). 

Bu belgedeki yönergeleri uyguladıktan sonra bir sanal ağ, bir alt ağ, bir denetleyici ve aşağıdaki çizimde gösterildiği gibi bir vFXT küme olacaktır:

![İsteğe bağlı bir blob depolama ve üç içeren bir alt ağ içeren sanal ağ gösteren diyagram vFXT düğümleri/vFXT kümesi ve bir VM etiketli küme denetleyicisi etiketlenmiş Vm'lere gruplandırılmış](media/avere-vfxt-deployment.png)

Başlamadan önce şu önkoşulların giderdik emin olun:  

1. [Yeni Abonelik](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Abonelik sahibi izinleri](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Kota vFXT kümesi için](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)

Küme düğümü rol oluşturma isteğe bağlı olarak, [önce](avere-vfxt-pre-role.md) küme denetleyici, ancak oluşturma daha sonra yapmak daha basit.

## <a name="create-the-cluster-controller-vm"></a>Küme denetleyiciyi VM oluşturma

İlk adım, oluşturma ve yapılandırma vFXT küme düğümleri sanal makineye oluşturmaktır. 

Bir Linux VM Avere vFXT küme oluşturma yazılımı ve önceden yüklenen betikleri ile küme denetleyicisidir. Düşük maliyetli seçenekler seçebilmeniz önemli işlem yüküyle güç veya depolama alanı, gerek yoktur. Bu VM'yi bazen vFXT kümenin kullanım ömrü boyunca kullanılır.

Küme Denetleyicisi VM oluşturmak için iki yöntem vardır. Bir [Azure Resource Manager şablonu](#create-controller---arm-template) sağlanan [aşağıda](#create-controller---arm-template) işlemi, ancak basitleştirmek için de denetleyicisinden oluşturabilirsiniz [Azure Market görüntüsü](#create-controller---azure-marketplace-image). 

Denetleyici oluşturma işleminin parçası olarak yeni bir kaynak grubu oluşturabilirsiniz.

> [!TIP]
>
> Genel bir IP adresi küme denetleyicisinde kullanılıp kullanılmayacağı karar verin. Genel bir IP adresi vFXT kümesine kolay erişim sağlar, ancak küçük bir güvenlik riski oluşturur.
>
>  * Bir genel IP adresi kümesi denetleyicisinde Avere vFXT küme dışında özel alt ağa bağlanmak için bir atlama konağı olarak kullanmanıza olanak sağlar.
>  * Genel bir IP adresi denetleyicisinde ayarlamazsanız, kümeye erişmek için başka bir atlama konak, bir VPN bağlantısı veya ExpressRoute kullanmanız gerekir. Örneğin, yapılandırılmış bir VPN bağlantısı olan bir sanal ağ içindeki denetleyiciyi oluşturun.
>  * Genel bir IP adresi ile bir denetleyici oluşturursanız, bir ağ güvenlik grubuyla denetleyicisi VM'SİNİN korumanız gerekir. Erişim yalnızca bağlantı noktası üzerinden internet erişimi sağlamak için 22 sağlar.

### <a name="create-controller---resource-manager-template"></a>Denetleyici - Resource Manager şablonu oluşturma

Portaldan düğüm denetleyicisi oluşturmak için aşağıdaki "Azure'a dağıtın" düğmesine tıklayın. Bu dağıtım şablonu oluşturma ve yönetme Avere vFXT küme sanal Makineyi oluşturur.

[![Düğme denetleyicisi oluşturmak için](media/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fvfxt%2Fazuredeploy.json)

Aşağıdaki bilgileri sağlayın.

İçinde **temel** bölümü:  

* **Abonelik** küme için
* **Kaynak grubu** küme için 
* **Konum** 

İçinde **ayarları** bölümü:

* Yeni bir sanal ağ oluşturma gerekip gerekmediğini

  * Yeni bir vnet oluşturun, böylece size ulaşabildiğimizden küme denetleyici bir genel IP adresi atanır. Bir ağ güvenlik grubu için yalnızca bağlantı noktası 22 için gelen trafiği sınırlayan bu sanal ağ da oluşturulur.
  * ExpressRoute veya VPN, küme denetleyiciye bağlanmak için kullanmak istiyorsanız, bu değeri ayarlamak `false` ve mevcut bir vnet'i kalan alanları belirtin. Küme Denetleyicisi, sanal ağa ağ iletişimi için kullanır. 

* Sanal ağ kaynak grubu adını ve alt ağ adı - var olan kaynakların adlarını (mevcut bir vnet'i kullanıyorsanız) yazın veya yeni bir sanal ağ oluşturuyorsanız yeni adlarını yazın
* **Denetleyici adı** -VM denetleyici için bir ad ayarlayın
* Denetleyici yönetici kullanıcı adı - varsayılan değer `azureuser`
* SSH anahtarı - yönetici kullanıcı adıyla ilişkilendirmek için ortak anahtarı Yapıştır. Okuma [oluşturma ve SSH anahtarlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) yardıma ihtiyacınız varsa.

Altında **hüküm ve koşullar**: 

* Hizmet koşullarını okuyun ve bunları kabul etmek için onay kutusuna tıklayın. 

  > [!NOTE] 
  > Abonelik sahibi değilseniz, önkoşul izleyerek adımları için koşulları kabul etmek bir sahip [yazılım önceden koşulları kabul](avere-vfxt-prereqs.md#accept-software-terms-in-advance). 

Tıklayın **satın alma** bittiğinde. Beş veya altı dakika sonra çalışır duruma denetleyicisi düğümünüzü olacaktır.

Kümeyi oluşturmak için gereken denetleyicisi bilgileri toplamak için çıktıların sayfasını ziyaret edin. Okuma [kümeyi oluşturmak gerekli bilgiler](#information-needed-to-create-the-cluster) daha fazla bilgi için.

### <a name="create-controller---azure-marketplace-image"></a>Denetleyici - Azure Market görüntüsü oluşturma

Azure Marketi'nde adı için arama yaparak denetleyicisi şablonu bulun ``Avere``. Seçin **Avere vFXT Azure denetleyicisi** şablonu.

Zaten yapmadıysanız, koşulları kabul etmek ve "programlı olarak dağıtmak istiyorsanız?" tıklayarak Market görüntüsü için programlı erişimi etkinleştir altında bağlantı **Oluştur** düğmesi.

![Ekran görüntüsü oluştur düğmesi programlı erişim bağlantısı](media/avere-vfxt-deploy-programmatically.png)

Tıklayın **etkinleştirme** düğmesine tıklayın ve bu ayarı kaydedin.

![Programlı erişim sağlamak için ekran gösteren fare tıklatın](media/avere-vfxt-enable-program.png)

Ana sayfasına dönün **Avere vFXT Azure denetleyicisi** şablonu ve tıklatın **Oluştur**. 

İlk panelinde doldurun veya bu temel seçenekler onaylayın:

* **Abonelik**
* **Kaynak grubu** (yeni bir grup oluşturmak istiyorsanız yeni bir ad girin.)
* **Sanal makine adı** -denetleyicinin adı
* **Bölge**
* **Kullanılabilirlik seçeneklerini** -yedeklilik gerekli değil
* **Görüntü** -Avere vFXT denetleyicisi düğümünün görüntüsü
* **Boyutu** - varsayılan değeri bırakın veya başka bir ucuz türü seçin
* **Yönetici hesabı** -küme denetleyiciye oturum açma ayarlayın: 
  * Kullanıcı adı/parola veya SSH ortak anahtarı (önerilen) seçin.
  
    > [!TIP] 
    > Bir SSH anahtarı daha güvenlidir. Okuma [oluşturma ve SSH anahtarlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) yardıma ihtiyacınız varsa. 
  * Kullanıcı adı belirtin 
  * SSH anahtarını yapıştırın veya girin ve parolayı onaylayın
* **Gelen bağlantı noktası kuralları** - genel IP adresi, açık bağlantı noktası 22'yi (SSH) kullanarak

Tıklayın **sonraki** disk seçeneklerini ayarlamak için:

* **İşletim sistemi disk türü** -varsayılan değer olan HDD yeterlidir
* **Yönetilmeyen diskleri kullanmaya** - gerekli değil
* **Veri diskleri** -kullanmayın

Tıklayın **sonraki** ağ seçenekleri için:

* **Sanal ağ** - denetleyici için bir vnet seçin veya yeni bir vnet oluşturmak için bir ad girin. Denetleyici üzerinde bir genel IP adresi kullanmak istemiyorsanız, ExpressRoute ya da başka bir erişim yöntemi zaten ayarlanmış olan bir vnet içinde bulma göz önünde bulundurun.
* **Alt ağ** -(isteğe bağlı) sanal ağ içindeki bir alt ağ seçin. Yeni bir vnet oluşturun, aynı anda yeni bir alt ağ oluşturabilirsiniz.
* **Genel IP** -bir genel IP adresi kullanmak istiyorsanız, bunu burada belirtebilirsiniz. 
* **Ağ güvenlik grubu** -varsayılan ayarı koruyun (**temel**) 
* **Ortak gelen bağlantı noktası** - bir genel IP adresini kullanarak kullanırsanız, bu denetim erişim gelen SSH trafiğine izin vermek için. 
* **Accelerated Networking** bu VM için kullanılamaz.

Tıklayın **sonraki** yönetim seçeneklerini ayarlamak için:

* **Önyükleme tanılaması** -değiştirmek **kapalı**
* **İşletim sistemi Konuk tanılama** -devre dışı bırakın
* **Tanılama depolama hesabı** - isteğe bağlı olarak seçin veya tanılama bilgileri tutmak için yeni bir hesap belirtin.
* **Yönetilen hizmet kimliği** -değiştirmek için bu seçeneği **üzerinde**, küme denetleyici için bir Azure AD hizmet ilkesi oluşturur.
* **Otomatik kapatma** -devre dışı bırakın 

Bu noktada, tıklayabilirsiniz **gözden geçir + Oluştur** örneği etiketleri kullanmak istemiyorsanız. ' A tıklayıp **sonraki** iki kez atlamak için **Konuk config** sayfasında ve etiketleri sayfasına alın. Var. bittiğinde tıklatın **gözden geçir + Oluştur**. 

Seçimlerinizi doğrulandıktan sonra tıklayın **Oluştur** düğmesi.  

Oluşturulması, beş veya altı dakika sürer.

## <a name="create-a-storage-endpoint-if-using-azure-blob"></a>Depolama uç noktası (Azure Blob kullanıyorsanız) oluşturma

Arka uç veri depolama için Azure Blob Depolama kullanıyorsanız, sanal ağınızda bulunan bir depolama hizmet uç noktası oluşturmanız gerekir. Bu [hizmet uç noktası](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) Azure Blob trafik internet üzerinden yönlendirmek yerine yerel tutar getirin.

1. Portalda, **sanal ağlar** soldaki.
1. Sanal ağ denetleyiciniz için seçin. 
1. Tıklayın **hizmet uç noktalarını** soldaki.
1. Tıklayın **Ekle** en üstünde.
1. Hizmet olarak bırakın ``Microsoft.Storage`` ve denetleyicinin alt ağ seçin.
1. Alt kısmında tıklayın **Ekle**.

  ![Hizmet uç noktası oluşturma adımları için ek açıklamalar ile Azure portal ekran görüntüsü](media/avere-vfxt-service-endpoint.png)

## <a name="information-needed-to-create-the-cluster"></a>Kümeyi oluşturmak için gereken bilgileri

Küme denetleyici oluşturduktan sonra sonraki adımlar için ihtiyacınız olan bilgileri olduğundan emin olun. 

Denetleyiciye bağlanmak için gereken bilgileri: 

* Denetleyici kullanıcı adı ve SSH anahtarı (veya parola)
* Denetleyici için IP adresi veya VM denetleyiciye bağlanmak için başka bir yöntemi

Küme için gereken bilgileri: 

* Kaynak grubu adı
* Azure konumu 
* Sanal ağ adı
* Alt ağ adı
* Küme düğümü rol adı - bu adın açıklanan rol oluşturduğunuzda ayarlandığından [aşağıda](#create-the-cluster-node-access-role)
* Bir Blob kapsayıcısı oluşturma, depolama hesabı adı

Resource Manager şablonu kullanarak denetleyici düğüm oluşturduysanız, bilgi edinebilirsiniz [şablon çıkış](#find-template-output). 

Denetleyicisi oluşturmak için Azure Market görüntüsü kullandıysanız, bu öğelerin çoğu, doğrudan sağladı. 

Denetleyici VM bilgi sayfasına göz atarak eksik öğeleri bulun. Örneğin, **tüm kaynakları** ve Denetleyici adı için arama yapın ve ardından ayrıntılarını görmek için denetleyicinin adını tıklatın.

### <a name="find-template-output"></a>Şablon çıkış Bul

Şablon çıkış Kaynak Yöneticisi'nden bu bilgileri bulmak için bu yordamı izleyin:

1. Küme denetleyiciniz için kaynak grubuna gidin.

1. Sol taraftaki **dağıtımları**, ardından **Microsoft.Template**.

   ![Dağıtımları ve dağıtım adı altında bir tablodaki Microsoft.Template gösteren seçili olan kaynak grubunu portal sayfası](media/avere-vfxt-deployment-template.png)

1. Sol taraftaki **çıkışları**. Her alan değerleri kopyalayın. 

   ![Sayfa SSHSTRING, RESOURCE_GROUP, konum, ağ, alt ağ ve alanları etiketleri sağında gösterilen SUBNET_ID ile çıkarır](media/avere-vfxt-template-outputs.png)

## <a name="access-the-controller"></a>Denetleyiciye Access

Dağıtım adımlarına geri kalanını yapmak için küme denetleyiciye bağlanmak için gerekir.

1. Küme denetleyicinize bağlanmak için yöntem kurulumunuzu temel bağlıdır.

   * Denetleyici bir genel IP adresi SSH denetleyicisinin IP yönetici kullanıcı adı olarak varsa, ayarladığınız (örneğin, ``ssh azureuser@40.117.136.91``).
   * Denetleyici bir genel IP yoksa, bir VPN kullanın veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) , sanal ağa bağlantı.

1. Çalıştırarak denetleyicinize oturum açtıktan sonra kimlik doğrulaması yapmak `az login`. Kabuğu'nda sağlanan doğrulama kodu kopyalayın ve sonra yüklemek için bir web tarayıcısı kullanın [ https://microsoft.com/devicelogin ](https://microsoft.com/devicelogin) ve Microsoft sistemi ile kimlik doğrulaması. Onay Kabuk dönün.

   ![Tarayıcı bağlantısı ve kimlik doğrulama kodu görüntüleme 'AZ login' komutunun komut satırı çıkışı](media/avere-vfxt-azlogin.png)

1. Aboneliğiniz, abonelik Kimliğinizi kullanarak şu komutu çalıştırarak ekleyin:  ```az account set --subscription YOUR_SUBSCRIPTION_ID```

## <a name="create-the-cluster-node-access-role"></a>Küme düğümü erişim rolü oluşturma

> [!NOTE] 
> Abonelik sahibi değildir ve rol zaten oluşturulmadı, yordamı kullanın veya bu adımları bir abonelik sahibi olan [Avere vFXT küme çalışma zamanı erişim rol olmadan bir denetleyici oluşturma](avere-vfxt-pre-role.md).

[Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) vFXT küme düğümlerine gerekli görevler gerçekleştirme yetkisi verir.  

Normal vFXT küme işlemi bir parçası olarak bireysel vFXT düğümleri gibi şeyleri yapmanıza gerek Azure kaynak özelliklerini okumak, depolama alanını yönet ve diğer düğümlere ağ arabirimi ayarları denetler. 

1. Denetleyicide açın ``/avere-cluster.json`` dosya bir düzenleyicide.

   ![bir liste komut ve ardından "olduğu gibi vi /avere-cluster.json" gösteren konsol](media/avere-vfxt-open-role.png)

1. Abonelik Kimliğinizi ekleyin ve yukarıdaki satırı silmek için dosyayı düzenleyin. Dosyayı Farklı Kaydet ``avere-cluster.json``.

   ![Metin düzenleyici, abonelik kimliği ve "remove bu satırı" silinmek üzere seçilen gösteren konsol](media/avere-vfxt-edit-role.png)

1. Rolü oluşturmak için bu komutu kullanın:  

   ```bash
   az role definition create --role-definition /avere-cluster.json
   ```

Sonraki adımda küme oluşturma komut rolün adını geçirir. 

## <a name="create-nodes-and-configure-the-cluster"></a>Düğümleri oluşturur ve küme yapılandırma

Avere vFXT kümeyi oluşturmak için denetleyicide dahil örnek betikler birini düzenleyin ve var. çalıştırın. Örnek betikler, kök dizinde bulunur (`/`) küme denetleyicisinde.

* Avere vFXT'ın arka uç depolama sistemi olarak kullanın, için Blob kapsayıcısı oluşturmak istiyorsanız ``create-cloudbacked-cluster`` betiği.

* Daha sonra depolama ekleyeceksiniz kullanırsanız ``create-minimal-cluster`` betiği.

> [!TIP]
> Düğüm ekleme ve vFXT kümeyi yok etme için prototip betikler de dahil edilecek `/` küme denetleyicisi VM'SİNİN dizin.

### <a name="edit-the-deployment-script"></a>Dağıtım betiğini Düzenle

Örnek betik bir düzenleyicide açın. Özelleştirilmiş betik özgün örnek üzerine yazılmasını önlemek için farklı bir adla kaydetmek isteyebilirsiniz.

Bu komut dosyası değişkenleri için değerleri girin.

* Kaynak grubu adı

  * Farklı kaynak gruplarındadır ağ veya depolama bileşenleri kullanıyorsanız, değişkenleri açıklamasını kaldırın ve bu adları da sağlayın. 

```python
# Resource groups
# At a minimum specify the resource group.  If the network resources live in a
# different group, specify the network resource group.  Likewise for the storage
# account resource group.
RESOURCE_GROUP=
#NETWORK_RESOURCE_GROUP=
#STORAGE_RESOURCE_GROUP=
```

* Konum adı
* Sanal ağ adı
* Alt ağ adı
* Örnekte izlediyseniz azure AD çalışma zamanı rol adı - [küme düğümü erişim rolünü oluşturma](#create-the-cluster-node-access-role), kullanın ``avere-cluster``. 
* Depolama hesabı adı (yeni bir Blob kapsayıcı oluşturuyorsanız)
* Küme adı - aynı kaynak grubunda aynı ada sahip iki vFXT küme sahip olamaz. 
* Yönetici parolasını - izleme ve kümeyi yönetmek için güvenli bir parola seçin. Bu parolayı kullanıcıya atanmış ``admin``. 
* Düğüm örneği türü - bkz [vFXT düğümü boyutları](avere-vfxt-deploy-plan.md#vfxt-node-sizes) bilgi
* Düğüm önbellek boyutu - bkz [vFXT düğümü boyutları](avere-vfxt-deploy-plan.md#vfxt-node-sizes) bilgi

Çıkış ve dosyayı kaydedin.

### <a name="run-the-script"></a>Betiği çalıştırın

Oluşturduğunuz dosya adını yazarak betiği çalıştırın. (Örnek: `./create-cloudbacked-cluster-west1`)  

> [!TIP]
> Bu komut içinde çalıştırmayı göz önünde bulundurun bir [terminal çoğaltıcı](http://linuxcommand.org/lc3_adv_termmux.php) gibi `screen` veya `tmux` bağlantınızı kaybetmeniz durumunda.  

Çıkış ayrıca oturum `~/vfxt.log`.

Betik tamamlandığında, küme yönetim için gereken yönetim IP adresi kopyalayın.

![Yönetim IP adresi sonlarında görüntüleme betiğin komut satırı çıkışı](media/avere-vfxt-mgmt-ip.png)

## <a name="next-step"></a>Sonraki adım

Kümenin çalıştığından ve yönetim IP adresini bilmeniz göre yapabilecekleriniz [Küme Yapılandırma Aracı'na bağlanma](avere-vfxt-cluster-gui.md) desteğini etkinleştir ve gerekirse depolama eklemek için.
