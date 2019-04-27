---
title: Azure Market görüntüsü kullanarak yönetilen kimlik özelliğine sahip bir Terraform Linux sanal makinesi oluşturma
description: Bir Market görüntüsü kullanarak yönetilen kimlik ve Uzak Durum Yönetimi özelliklerine sahip bir Terraform Linux sanal makinesi oluşturabilir ve Azure'a kolayca kaynak dağıtımı gerçekleştirebilirsiniz.
services: terraform
ms.service: azure
keywords: terraform, devops, MSI, sanal makine, uzak durum, azure
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 3/12/2018
ms.openlocfilehash: a1a980e1f8b004c4a3dba53e4f83367022074c7c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60884498"
---
# <a name="use-an-azure-marketplace-image-to-create-a-terraform-linux-virtual-machine-with-managed-identities-for-azure-resources"></a>Azure Market görüntüsü kullanarak Azure kaynakları için yönetilen kimliğe sahip bir Terraform Linux sanal makinesi oluşturma

Bu makalede bir [Terraform Market görüntüsünü](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) kullanarak [Azure kaynakları için yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) ile yüklenmiş ve yapılandırılmış en son [Terraform](https://www.terraform.io/intro/index.html) sürümüne sahip bir Ubuntu Linux VM (16.04 LTS) oluşturmayı öğreneceksiniz. Bu görüntü ayrıca Terraform ile [uzak durum](https://www.terraform.io/docs/state/remote.html) yönetimi sağlamak için bir uzak arka uç da yapılandıracaktır. 

Terraform Market görüntüsü, Terraform'u el ile yükleme ve yapılandırmanıza gerek kalmadan Terraform'u Azure'da kullanmaya başlamanızı kolaylaştırır. 

Bu Terraform VM görüntüsü için yazılım ücreti alınmaz. Yalnızca sağlanan sanal makinenin boyutuna göre belirlenen Azure donanımı kullanım ücretlerini ödersiniz. İşlem ücretleri hakkında daha fazla bilgi için bkz. [Linux sanal makineleri fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Önkoşullar
Linux Terraform sanal makinesi oluşturmak için bir Azure aboneliğine sahip olmanız gerekir. Henüz yoksa [ücretsiz Azure hesabınızı hemen oluşturun](https://azure.microsoft.com/free/).  

## <a name="create-your-terraform-virtual-machine"></a>Terraform sanal makinenizi oluşturma 

Linux Terraform sanal makinesi örneği oluşturma adımları aşağıda verilmiştir: 

1. Azure portalda [Kaynak oluştur](https://ms.portal.azure.com/#create/hub) bölümüne gidin.

2. **Market içinde ara** arama çubuğundan **Terraform** araması yapın. **Terraform** şablonunu seçin. 

3. Terraform ayrıntıları sekmesinin sağ alt bölümünde **Oluştur** düğmesini seçin.

    ![Terraform sanal makinesi oluşturma](media/terraformmsi.png)

4. Aşağıdaki bölümlerde Terraform Linux sanal makinesini oluşturmak için sihirbaza girmeniz gereken değerler verilmiştir. Aşağıdaki bölümde bu adımları yapılandırmak için gerekli olan değerler belirtilmiştir.

## <a name="details-on-the-create-terraform-tab"></a>Terraform Oluştur sekmesindeki ayrıntılar

**Terraform Oluştur** sekmesine aşağıdaki bilgileri girin:

1. **Temel Bilgiler**
    
   * **Ad**: Terraform sanal makinenizin adı.
   * **Kullanıcı adı**: İlk hesap oturum açma kimliği
   * **Parola**: İlk hesap parolası. (Parola yerine SSH ortak anahtarı kullanabilirsiniz.)
   * **Abonelik**: Makine oluşturulması ve fatura olduğu abonelik. Bu abonelikte kaynak oluşturma ayrıcalıklarına sahip olmanız gerekir.
   * **Kaynak grubu**: Yeni veya mevcut bir kaynak grubu.
   * **Konum**: En uygun veri merkezi. Bu genellikle verilerinizin çoğunun bulunduğu veya en hızlı ağ erişimi için fiziksel konumunuza en yakın olan veri merkezidir.

2. **Ek ayarlar**

   * **Boyutu**: Sanal makine boyutu. 
   * **VM disk türü**: SSD veya HDD.

3. **Terraform Özeti**

   * Girdiğiniz tüm bilgilerin doğru olduğunu onaylayın. 

4. **Satın Al**

   * Sağlama işlemini başlatmak için **Satın Al**'ı seçin. İşlemin koşullarının bağlantısı sunulur. Boyut adımında seçtiğiniz sunucu boyutu için işlem ücretlerinin dışında VM için herhangi bir ücret alınmaz.

Terraform VM görüntüsü aşağıdaki adımları gerçekleştirir:

* Ubuntu 16.04 LTS görüntüsünü temel alan ve sistem tarafından atanmış olan kimliğe sahip olan bir VM oluşturur.
* Azure kaynakları için OAuth belirteçlerinin oluşturulmasına izin verme amacıyla VM üzerine MSI uzantısını yükler.
* Yönetilen kimliğe RBAC izinleri atayarak kaynak grubunda sahip hakları verir.
* Bir Terraform şablon klasörü (tfTemplate) oluşturur.
* Bir Terraform uzak durumu için Azure arka ucuyla ön yapılandırma gerçekleştirir.

## <a name="access-and-configure-a-linux-terraform-virtual-machine"></a>Linux Terraform sanal makinesi erişim ve yapılandırma adımları

VM'yi oluşturduktan sonra SSH kullanarak oturum açabilirsiniz. Metin kabuk arabirimi için 3. adımın "Temel Bilgiler" bölümünde oluşturduğunuz hesap kimlik bilgilerini kullanın. Windows'da [Putty](https://www.putty.org/) gibi bir SSH istemcisi aracı indirebilirsiniz.

SSH kullanarak sanal makineye bağlandıktan sonra sanal makine üzerindeki Azure kaynakları için yönetilen kimliklere aboneliğin tamamında katkıda bulunan izni vermeniz gerekir. 

Katkıda bulunan izinleri sayesinde VM üzerindeki MSI, Terraform'u kullanarak VM kaynak grubu dışında kaynak oluşturabilirsiniz. Bu işlemi gerçekleştirmek için aşağıdaki betiği bir kez çalıştırmanız yeterlidir. Aşağıdaki komutu kullanın:

`. ~/tfEnv.sh`

Yukarıdaki betikte [AZ CLI v 2.0 etkileşimli oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) mekanizması kullanılarak Azure kimlik doğrulaması gerçekleştirilir ve sanal makine Yönetilen Kimliğe aboneliğin tamamında katkıda bulunan izni atanır. 

 VM, Terraform uzak durum arka ucuna sahiptir. Bunu Terraform dağıtımınızda etkinleştirmek için tfTemplate dizinindeki remoteState.tf dosyasını Terraform betiklerinin kök dizinine kopyalayın.  

 `cp  ~/tfTemplate/remoteState.tf .`

 Uzak Durum Yönetimi hakkında daha fazla bilgi için [bu Terraform uzak durum sayfasını inceleyin](https://www.terraform.io/docs/state/remote.html). Depolama erişim anahtarı bu dosyada gösterilmektedir ve Terraform yapılandırma dosyalarını kaynak denetimine göndermeden önce kaldırılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Azure'da bir Terraform Linux sanal makinesi oluşturmayı öğrendiniz. Aşağıdaki kaynaklardan Azure'da Terraform kullanımı hakkında daha fazla bilgi edinebilirsiniz: 

 [Microsoft.com Terraform Hub'ı](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure sağlayıcı belgeleri](https://aka.ms/terraform)  
 [Terraform Azure sağlayıcı kaynağı](https://aka.ms/tfgit)  
 [Terraform Azure modülleri](https://aka.ms/tfmodules)
 

















