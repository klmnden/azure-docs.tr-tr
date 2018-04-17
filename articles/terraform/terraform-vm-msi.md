---
title: Yönetilen hizmet kimliği ile Terraform Linux sanal makine oluşturmak için Azure Market görüntü kullanın
description: Market görüntüsü kolayca kaynakları Azure'a dağıtmak için Yönetilen hizmet kimliği ve uzak durum yönetimi ile Terraform Linux sanal makine oluşturmak için kullanın.
keywords: terraform, devops, MSI, sanal makine, uzak durumu, azure
author: VaijanathB
manager: rloutlaw
ms.author: tarcher
ms.date: 3/12/2018
ms.topic: article
ms.openlocfilehash: 5f0ee2904c1072a5ad8c5f7ae1c90e649cc4813c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-an-azure-marketplace-image-to-create-a-terraform-linux-virtual-machine-with-managed-service-identity"></a>Yönetilen hizmet kimliği ile Terraform Linux sanal makine oluşturmak için Azure Market görüntü kullanın

Bu makalede nasıl kullanılacağı gösterilmektedir bir [Terraform Market görüntüsü](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) bir Ubuntu Linux VM oluşturmak için (16.04 LTS) en son [Terraform](https://www.terraform.io/intro/index.html) sürümü yüklü ve kullanılarak yapılandırılan [yönetilen Hizmet kimliği (MSI)](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview). Bu görüntü da etkinleştirmek için uzak bir arka uç yapılandırır [uzak durumu](https://www.terraform.io/docs/state/remote.html) Terraform kullanarak yönetimi. 

Terraform Market görüntüsü Terraform Azure üzerinde yüklemek ve Terraform el ile yapılandırmak zorunda kalmadan kullanmaya başlamak kolaylaştırır. 

Bu Terraform VM görüntüsü için yazılım harcamanız yok. Sağlanan sanal makine boyutuna göre uygunluk Azure donanım kullanım ücretleri ödersiniz. İşlem ücretleri hakkında daha fazla bilgi için bkz: [Linux sanal makineleri fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Önkoşullar
Bir Linux Terraform sanal makine oluşturmadan önce bir Azure aboneliğinizin olması gerekir. Zaten yoksa, bkz: [ücretsiz Azure hesabınızı bugün oluşturmak](https://azure.microsoft.com/free/).  

## <a name="create-your-terraform-virtual-machine"></a>Terraform sanal makine oluşturma 

Bir Linux Terraform sanal makine örneği oluşturmak için adımlar şunlardır: 

1. Azure portalında Git [kaynak oluşturma](https://ms.portal.azure.com/#create/hub) listesi.

2. İçinde **Market arama** arama çubuğu, arama **Terraform**. Seçin **Terraform** şablonu. 

3. Sağ alt köşesinde Terraform Ayrıntılar sekmesinde seçin **oluşturma** düğmesi.

    ![Terraform sanal makine oluşturma](media\terraformmsi.png)

4. Aşağıdaki bölümler girişleri her Terraform Linux sanal makine oluşturmak için sihirbazın adımlarını sağlar. Aşağıdaki bölümde bu adımların her biri yapılandırmak için gereken girişleri listelenmektedir.

## <a name="details-on-the-create-terraform-tab"></a>Oluşturma Terraform sekmesindeki Ayrıntılar

Aşağıdaki ayrıntıları girin **oluşturma Terraform** sekmesi:

1. **Temel Bilgiler**
    
   * **Ad**: Terraform sanal makinenin adı.
   * **Kullanıcı adı**: ilk hesap oturum açma kimliği.
   * **Parola**: ilk hesap parolası. (Parola yerine bir SSH ortak anahtarı kullanabilirsiniz.)
   * **Abonelik**: Makine olduğu oluşturulur ve fatura için abonelik. Bu abonelik için kaynak oluşturma ayrıcalıkları olmalıdır.
   * **Kaynak grubu**: yeni veya var olan kaynak grubu.
   * **Konum**: en uygun olan veri merkezi. Genellikle verilerinizden en iyi veya en yakın fiziksel konumunuza en hızlı ağ erişimi için bir veri merkezi olur.

2. **Ek ayarları**

   * **Boyutu**: sanal makine boyutu. 
   * **VM disk türü**: SSD veya HDD.

3. **Özet Terraform**

   * Girdiğiniz tüm bilgilerin doğru olduğunu doğrulayın. 

4. **Satın alma**

   * Sağlama işlemini başlatmak için **satın**. Bağlantı işlem koşullarını sağlanır. VM boyutu adımda seçtiğiniz sunucu boyutu işlem ötesinde herhangi bir ek ücret yok.

Terraform VM görüntüsü aşağıdaki adımları gerçekleştirir:

* Ubuntu 16.04 LTS görüntüyü temel alarak sistem atanan kimliğe sahip bir VM oluşturur.
* OAuth belirteçleri için Azure kaynaklarını verilmesine izin vermek için VM'de MSI uzantısı yükler.
* Kaynak grubu için sahiplik hakları verme yönetilen kimliğine RBAC izinleri atar.
* Bir Terraform şablon klasörü (tfTemplate) oluşturur.
* Azure yedekleme Terraform uzak durumuyla önceden yapılandırır son.

## <a name="access-and-configure-a-linux-terraform-virtual-machine"></a>Erişim ve Linux Terraform sanal makine yapılandırma

VM oluşturduktan sonra kendisine SSH kullanarak oturum açabilirsiniz. Adım 3 metin kabuk arabirimi için "Temel" bölümünde oluşturduğunuz hesap kimlik bilgilerini kullanın. Windows, bir SSH istemcisi aracı gibi indirebilirsiniz [Putty](http://www.putty.org/).

Sanal makineye bağlanmak için SSH kullandıktan sonra yönetilen hizmet kimliği için sanal makinedeki tüm abonelik katkıda bulunan izinleri vermeniz gerekir. 

Katkıda bulunan izni Terraform VM kaynak grubu dışındaki kaynaklar oluşturmak üzere kullanmak için VM'de MSI yardımcı olur. Bu eylem bir komut dosyası bir kez çalıştırarak kolayca elde edebilirsiniz. Aşağıdaki komutu kullanın:

`. ~/tfEnv.sh`

Önceki komut dosyası kullanan [AZ CLI v 2.0 etkileşimli oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest#interactive-log-in) Azure kimlik doğrulaması ve sanal makine yönetilen hizmet kimliği katkıda bulunan tüm abonelik izinlerini atamak için bir mekanizma. 

 VM Terraform uzak durumu arka uç vardır. Terraform dağıtımınızı etkinleştirmek için remoteState.tf dosyasını tfTemplate dizininden Terraform betikleri kök dizinine kopyalayın.  

 `cp  ~/tfTemplate/remoteState.tf .`

 Uzak durum yönetimi hakkında daha fazla bilgi için bkz: [bu sayfa Terraform uzak durumu hakkında](https://www.terraform.io/docs/state/remote.html). Depolama erişim tuşu bu dosyada sunulur ve teslim etme Terraform yapılandırma dosyalarını önce kaynak denetimine hariç tutulacak gerekiyor.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure üzerinde Terraform Linux sanal makine ayarlama öğrendiniz. Azure üzerinde Terraform hakkında daha fazla bilgi edinmenize yardımcı olması için bazı ek kaynaklar aşağıda verilmiştir: 

 [Microsoft.com Terraform Hub](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure sağlayıcı belgeleri](http://aka.ms/terraform)  
 [Terraform Azure sağlayıcı kaynağı](http://aka.ms/tfgit)  
 [Terraform Azure modülleri](http://aka.ms/tfmodules)
 

















