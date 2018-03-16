---
title: "Yönetilen hizmet kimliği ile Terraform Linux sanal makine oluşturmak için Azure Market görüntü kullanın"
description: "Market görüntüsü kolayca kaynakları Azure'a dağıtmak için Yönetilen hizmet kimliği ve Uzaktan durumunu yönetimi ile Terraform Linux sanal makine oluşturmak için kullanın."
keywords: terraform, devops, MSI, sanal makine, uzak durumu, azure
author: VaijanathB
manager: rloutlaw
ms.author: tarcher
ms.date: 3/12/2018
ms.topic: article
ms.openlocfilehash: ce8a3e8b813bf4224a1fd77d60ec30f2f8e080a7
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="use-an-azure-marketplace-image-to-create-a-terraform-linux-virtual-machine-with-managed-service-identity"></a>Yönetilen hizmet kimliği ile Terraform Linux sanal makine oluşturmak için Azure Market görüntü kullanın

Bu makalede nasıl kullanılacağını gösterir bir [Terraform Market görüntüsü](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) oluşturmak için bir `Ubuntu Linux VM (16.04 LTS)` son ile [Terraform](https://www.terraform.io/intro/index.html) sürümü yüklü ve kullanılarak yapılandırılan [yönetilen hizmet kimliği () MSI)](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview). Bu görüntü da etkinleştirmek için uzak bir arka uç yapılandırır [uzaktan durumunu](https://www.terraform.io/docs/state/remote.html) Terraform kullanarak yönetimi. Terraform Market görüntüsü Terraform Azure üzerinde dakika cinsinden Terraform yüklemek ve kimlik doğrulama el ile yapılandırmak zorunda kalmadan kullanmaya başlamak kolaylaştırır. 

Bu Terraform VM görüntüsü için yazılım harcamanız yok. Sağlanan sanal makine boyutuna göre uygunluk Azure donanım kullanım ücretleri ödersiniz. İşlem ücretleri hakkında daha fazla ayrıntı bulunabilir [Linux sanal makineleri Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Önkoşullar
Linux Terraform sanal makine oluşturmadan önce bir Azure aboneliğinizin olması gerekir. Zaten bir yoksa, bkz: [ücretsiz Azure hesabınızı bugün oluşturmak](https://azure.microsoft.com/free/).  

## <a name="create-your-terraform-virtual-machine"></a>Terraform sanal makine oluşturma 

Örnek Linux Terraform sanal makine oluşturmak için adımlar şunlardır. 

1. Gidin [kaynak oluşturma](https://ms.portal.azure.com/#create/hub) Azure Portal'da listesi.
2. Arama `Terraform` içinde `Search the Marketplace` arama çubuğu. Seçin `Terraform` şablonu. Seçin **oluşturma** alt sağ tarafında Terraform Ayrıntılar sekmesinde düğmesini.
![Alternatif metin](media\terraformmsi.png)
3. Aşağıdaki bölümlerde her Sihirbazı'ndaki adımları girişleri yapın (**sağ tarafta numaralandırılan**) Terraform Linux sanal makine oluşturmak için.  Bu adımların her biri yapılandırmak için gereken girdiler şunlardır

## <a name="details-in-create-terraform-tab"></a>Ayrıntılar Terraform sekmesi oluştur

Burada, Terraform oluşturmanız sekmesindedir girilmesi gerekir Ayrıntılar verilmiştir.

a. **Temel Bilgiler**
    
* **Ad**: Terraform sanal makinenin adı.
* **Kullanıcı adı**: ilk hesap oturum açma kimliği.
* **Parola**: ilk hesap parolası (kullanabilirsiniz SSH ortak anahtarı parola yerine).
* **Abonelik**: birden fazla aboneliğiniz varsa, bir makine olduğu oluşturulur ve fatura için seçin. Bu abonelik için kaynak oluşturma ayrıcalıkları olmalıdır.
* **Kaynak grubu**: yeni bir tane oluşturun veya varolan bir grubu kullanın.
* **Konum**: en uygun olan veri merkezi seçin. Genellikle verilerinizden en iyi olan ya da fiziksel konumunuza en hızlı ağ erişimi için en yakın veri merkezinin olur.

b. **Ek ayarları**

* Boyutu: Sanal makine boyutu.
* VM disk türü: SSD ve HDD arasında seçim yapma

c. **Özet Terraform**

* Girdiğiniz tüm bilgilerin doğru olduğunu doğrulayın. 

d. **Satın alma**

* Sağlama başlatmak için Satın Al'ı tıklatın. Bağlantı işlem koşullarını sağlanır. VM boyutu adımda seçtiğiniz sunucu boyutu işlem ötesinde herhangi bir ek ücret yok.

Terraform VM görüntüsü aşağıdaki adımları gerçekleştirir

* Ubuntu 16.04 LTS görüntüyü temel alarak kimliği atanır sistemiyle VM oluşturur
* OAuth belirteçleri için Azure kaynaklarını verilmesine izin vermek için VM'de MSI uzantısı yükler
* Kaynak grubu için sahiplik hakları verme yönetilen kimlik, RBAC izinler atama
* Bir Terraform şablon klasörü (tfTemplate) oluşturur
* Azure arka uzak durumuyla Terraform önceden yapılandırır

## <a name="how-to-access-and-configure-linux-terraform-virtual-machine"></a>Erişim ve Linux Terraform sanal makine yapılandırma

VM oluşturulduktan sonra kendisine SSH kullanarak oturum açabilirsiniz. Adım 3 metin kabuk arabirimi için temel kavramları bölümünde oluşturduğunuz hesap kimlik bilgilerini kullanın. Windows, bir SSH istemcisi aracı gibi indirebilirsiniz [Putty](http://www.putty.org/)

Kullandığınız sonra `SSH` sanal makineye bağlanmak için Yönetilen hizmet kimliği sanal makinedeki tüm abonelik katkıda bulunan izinleri vermeniz gerekir. Katkıda bulunan izni Terraform VM kaynak grubu dışındaki kaynaklar oluşturmak üzere kullanmak için VM'de MSI yardımcı olur. Bu eylem bir komut dosyası bir kez yürüterek kolayca elde edebilirsiniz. Burada, yapmak için komut verilmiştir.

`. ~/tfEnv.sh`

Önceki komut dosyası kullanan [AZ CLI v 2.0 etkileşimli oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest#interactive-log-in) Azure kimlik doğrulaması ve tüm abonelik sanal makine yönetilen hizmet kimliği katkıda bulunan izni atamak için mekanizması. 

 VM oluşturulan Terraform uzaktan durumunu arka uç vardır ve Terraform dağıtımınızı etkinleştirmek için remoteState.tf dosya tfTemplate dizininden Terraform betikleri kök dizinine kopyalayın. gerekir.  

 `cp  ~/tfTemplate/remoteState.tf .`

 Daha fazla bilgiyi uzak durum yönetimi hakkında [burada](https://www.terraform.io/docs/state/remote.html). Depolama erişim tuşu bu dosyada sunulur ve dikkatle kaynak denetimine iade edilmesi gerekir.  

## <a name="next-steps"></a>Sonraki Adımlar
Bu makalede yukarı Terraform Linux sanal makine azure'da ayarlamak öğrendiniz. Azure üzerinde Terraform hakkında daha fazla bilgi için bazı ek kaynaklar aşağıda verilmiştir. 

 [Microsoft.com Terraform Hub](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure sağlayıcı belgeleri](http://aka.ms/terraform)  
 [Terraform Azure sağlayıcı kaynağı](http://aka.ms/tfgit)  
 [Terraform Azure modülleri](http://aka.ms/tfmodules)
 

















