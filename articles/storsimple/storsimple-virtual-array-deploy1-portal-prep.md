---
title: StorSimple Virtual Array için Portal hazırlığı | Microsoft Docs
description: StorSimple sanal dizisi dağıtmak için ilk öğreticide, Azure portal'ı hazırlama içerir
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 68a4cfd3-94c9-46cb-805c-46217290ce02
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/11/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7c6f0a6371b38f0271237db0f7d80b831ecc145c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62127149"
---
# <a name="deploy-storsimple-virtual-array---prepare-the-azure-portal"></a>StorSimple sanal Dizini'ni dağıtma - Azure portal'ı hazırlama

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a>Genel Bakış

Serideki ilk makaleyi dağıtım öğretici tamamen sanal dizininiz bir dosya sunucusu veya Resource Manager modelini kullanarak bir iSCSI sunucusu olarak dağıtmak için gereken budur. Bu makalede, oluşturma ve bir sanal dizin sağlama önce StorSimple cihaz Yöneticisi hizmetinize yapılandırmak için gereken hazırlık açıklanır. Bu makalede ayrıca kullanıma bir dağıtım yapılandırma denetim listesi ve yapılandırma için önkoşulları bağlantılarını içerir.

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Başlamadan önce dağıtım yapılandırma denetim listesini gözden geçirmenizi öneririz. Portal hazırlığı 10 dakikadan kısa sürer.

Bu makalede yayımlanmış bilgiler, StorSimple sanal dizisi dağıtıma Azure portalı ve Microsoft Azure kamu Bulutu geçerlidir.

### <a name="get-started"></a>başlarken
Portalı hazırlama, sanallaştırılmış ortamınızdaki bir sanal dizin sağlama ve Kurulumu Tamamlanıyor dağıtımı iş akışı oluşur. StorSimple sanal dizisi dağıtımına bir dosya sunucusu veya bir iSCSI sunucusu olarak çalışmaya başlamak için aşağıdaki tabulated kaynaklara başvurmanız gerekir.

#### <a name="deployment-articles"></a>Dağıtım makaleleri

StorSimple Virtual Array'iniz dağıtmak için önceden belirlenmiş bir sırada aşağıdaki makalelere bakın.

| **#** | **Bu adımda** | **Bunu...** | **Ve bu belgeleri kullanın.** |
| --- | --- | --- | --- |
| 1. |**Azure portal'ı ayarlama** |Oluşturun ve StorSimple sanal dizisi sağlama önce StorSimple cihaz Yöneticisi hizmetinize yapılandırın. |[Portalı hazırlama](storsimple-virtual-array-deploy1-portal-prep.md) |
| 2. |**Sanal dizin sağlayın** |Hyper-v, sağlama ve StorSimple sanal dizisi Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 üzerinde Hyper-V çalıştıran bir konak sisteminde bağlanın. <br></br> <br></br> VMware için sağlayın ve VMware ESXi 5.0, 5.5, 6.0 veya 6.5 çalıştıran bir konak sisteminde bir StorSimple Virtual Array için bağlanın.<br></br> |[Hyper-V'de bir sanal dizin sağlayın](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [Bir sanal dizin vmware'de sağlama](storsimple-virtual-array-deploy2-provision-vmware.md) |
| 3. |**Sanal dizi Kurulumu ayarlayın** |Dosya sunucunuz için ilk kurulum gerçekleştirmek, StorSimple dosya sunucunuzu kaydedin ve cihaz kurulumunu tamamlayın. Ardından SMB paylaşımlarını sağlayabilirsiniz. <br></br> <br></br> İSCSI sunucunuz için ilk kurulum gerçekleştirmek, StorSimple iSCSI sunucunuzu kaydedin ve cihaz kurulumunu tamamlayın. Ardından, iSCSI birimleri sağlayabilirsiniz. |[Sanal dizi Kurulumu dosya sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[Sanal dizi Kurulumu iSCSI sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-iscsi-setup.md) |

Artık Azure portalını ayarlamaya başlayabilirsiniz.

## <a name="configuration-checklist"></a>Yapılandırma denetim listesi

Yapılandırma denetim listesi, StorSimple sanal dizisi yazılım yapılandırmadan önce toplamak için gereken bilgileri açıklar. Bu bilgileri önceden hazırlama, ortamınızda StorSimple cihazını dağıtma işlemini kolaylaştırmaya yardımcı olur. StorSimple Virtual Array'iniz bir dosya sunucusu veya bir iSCSI sunucusu dağıtılmış temel bağlı olarak, aşağıdaki denetim listelerini biri gerekir.

* İndirme [StorSimple sanal dizisi dosya sunucusu yapılandırma denetim listesini](https://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).
* İndirme [StorSimple Virtual Array iSCSI sunucusunu yapılandırma denetim listesi](https://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Önkoşullar

Burada, StorSimple cihaz Yöneticisi hizmetiniz ve StorSimple Virtual Array'iniz veri merkezi ağı için yapılandırma önkoşulları bulabilirsiniz.

### <a name="for-the-storsimple-device-manager-service"></a>StorSimple Cihaz Yöneticisi hizmeti için

Başlamadan önce aşağıdakilerden emin olun:

* Erişim kimlik bilgilerine sahip bir Microsoft hesabınız var.
* Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.
* Microsoft Azure aboneliğiniz StorSimple cihaz Yöneticisi hizmeti için etkinleştirilmesi gerekir.

### <a name="for-the-storsimple-virtual-array"></a>StorSimple sanal dizisi için

Sanal dizi dağıtmadan önce emin olun:

* Bir Windows Server 2008 R2 veya sonraki sürümlerde Hyper-V çalıştıran bir konak sistemi ya da olabilir (ESXi 5.0, 5.5, 6.0 veya 6.5) VMware erişiminiz bir sağlamak için kullanılan bir cihaz.
* Konak sisteminin, sanal diziyi sağlamak için aşağıdaki kaynakları ayırmanız olanağına sahip:
  
  * En az 4 çekirdek.
  * En az 8 GB RAM. Sanal dizi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 2 milyon dosyalarını destekler. 16 GB RAM, 2-4 milyon dosyalarını desteklemek için ihtiyacınız vardır.
  * Bir ağ arabirimi.
  * Sistem verileri için 500 GB sanal disk.

### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

* Veri Merkezi ağında StorSimple cihazınız için ağ gereksinimlerine uygun olarak yapılandırılır. Daha fazla bilgi için [StorSimple sanal dizini sistem gereksinimleri](storsimple-ova-system-requirements.md).
* StorSimple Virtual Array'iniz ayrılmış 5 MB/sn Internet bant genişliği (veya daha fazla) olduğundan her zaman kullanılabilir. Bu bant genişliği başka hiçbir uygulamayla paylaşılmamalıdır.

## <a name="step-by-step-preparation"></a>Adım adım hazırlama

Portalınız için StorSimple cihaz Yöneticisi hizmeti hazırlamak için aşağıdaki adım adım yönergeleri kullanın.

## <a name="step-1-create-a-new-service"></a>1\. adım: Yeni hizmet oluşturma

Birden çok StorSimple sanal dizisi, StorSimple cihaz Yöneticisi hizmetinin tek bir örneğini yönetebilir. StorSimple Cihaz Yöneticisi hizmetinin bir örneğini oluşturmak için aşağıdaki adımları gerçekleştirin. Sanal diziler yönetmek için mevcut bir StorSimple cihaz Yöneticisi hizmeti varsa, bu adımı atlayın ve Git [2. adım: Hizmet kayıt anahtarı alma](#step-2-get-the-service-registration-key).

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> Hizmetinizle birlikte bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirmediyseniz, bir hizmeti başarıyla oluşturduktan sonra en az bir depolama hesabı oluşturmanız gerekir.
> 
> * Otomatik olarak bir depolama hesabı oluşturmadıysanız, ayrıntılı yönergeler için [Hizmet için yeni bir depolama hesabı yapılandırma](#optional-step-configure-a-new-storage-account-for-the-service) bölümüne gidin.
> * Bir depolama hesabı otomatik olarak oluşturulmasını etkinleştirdiyseniz, Git [2. adım: Hizmet kayıt anahtarı alma](#step-2-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-the-service-registration-key"></a>2\. adım: Hizmet kayıt anahtarı alma

StorSimple Cihaz Yöneticisi hizmeti çalışır duruma geldikten sonra, hizmet kayıt anahtarını almanız gerekir. Bu anahtar StorSimple cihazınızı kaydetmek ve hizmete bağlamak için kullanılır.

[Azure portalında](https://portal.azure.com/) aşağıdaki adımları izleyin.

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> Hizmet kayıt anahtarı, StorSimple cihaz Yöneticisi hizmetiyle kaydedilmesi gereken tüm StorSimple cihaz Yöneticisi cihazları kaydetmek için kullanılır.
> 
> 

## <a name="step-3-download-the-virtual-array-image"></a>3\. adım: Sanal dizi görüntüsünü indir

Hizmet kayıt anahtarını aldıktan sonra ana bilgisayar sisteminizin üzerinde bir sanal diziyi sağlamak için uygun sanal dizi görüntüsünü indir gerekecektir. Sanal dizi görüntüleri, belirli işletim sistemi ve Azure portalında hızlı başlangıç sayfasından indirilebilir.

> [!IMPORTANT]
> StorSimple sanal dizisi üzerinde çalışan yazılımı yalnızca StorSimple cihaz Yöneticisi hizmeti ile kullanılabilir.
> 
> 

[Azure portalında](https://portal.azure.com/) aşağıdaki adımları izleyin.

#### <a name="to-get-the-virtual-array-image"></a>Sanal dizi görüntüsünü almak için

1. [Azure portal](https://portal.azure.com/) oturum açın. 
2. Azure portalında **Gözat > StorSimple cihaz yöneticileri**.
3. StorSimple cihaz Yöneticisi hizmetini seçin. İçinde **StorSimple cihaz Yöneticisi** dikey penceresinde tıklayın **Hızlı Başlangıç**. 
4. Microsoft Download Center'dan gelen indirmek istediğiniz görüntüyü karşılık gelen bağlantıya tıklayın. Görüntü dosyaları yaklaşık 4,8 GB'tır.
   
   * Hyper-V Windows Server 2012 ve üzeri için VHDX
   * Hyper-V Windows Server 2008 R2 ve üzeri için VHD
   * VMWare ESXi 5.0, 5.5, 6.0 veya 6.5 için VMDK
5. Dosyayı yerel sürücüye indirin ve sıkıştırmasını açın. Sıkıştırması açılan dosyanın konumunu not alın.

## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>İsteğe bağlı adım: Hizmet için yeni bir depolama hesabı yapılandırın

Bu adım isteğe bağlıdır ve yalnızca, bir depolama hesabı otomatik olarak oluşturulmasını hizmetinizle etkinleştirmediyseniz gerçekleştirilmelidir.

Farklı bir bölgede bir Azure depolama hesabı oluşturmak ihtiyacınız varsa bkz [bir depolama hesabının nasıl oluşturulacağını](../storage/common/storage-quickstart-create-account.md) adım adım yönergeler için.

Aşağıdaki adımlarda gerçekleştirmek [Azure portalında](https://ms.portal.azure.com/) mevcut bir Microsoft Azure depolama hesabını eklemek için StorSimple cihaz Yöneticisi hizmet sayfasında.

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a>Cihaz Yöneticisi hizmetiyle aynı Azure aboneliği olan bir depolama hesabı kimlik bilgisi eklemek için

1. Cihaz Yöneticisi hizmetinize select gidin ve çift tıklayın. Bu açılır **genel bakış** dikey penceresi.
2. Seçin **depolama hesabı kimlik bilgileri** içinde **yapılandırma** bölümü.
3. **Ekle**'yi tıklatın.
4. İçinde **bir depolama hesabı ekleme** dikey penceresinde aşağıdakileri yapın:
   
   1. İçin **abonelik**seçin **geçerli**.
   
   2. Azure depolama hesabınızın adını sağlayın.
   
   3. Seçin **etkinleştirme** StorSimple cihazınız ve bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak için. Seçin **devre dışı** yalnızca, bir özel bulutta işlem yapıyorsanız temizleyin.
   
   4. **Ekle**'yi tıklatın. Depolama hesabı başarıyla oluşturulduktan sonra size bildirilir.<br></br>
   
      ![Mevcut bir depolama hesabı kimlik bilgisi Ekle](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a>Sonraki adım

StorSimple Virtual Array'iniz için bir sanal makine sağlamak için sonraki adımdır bakın. Konak işletim sisteminize bağlı olarak, şuradaki ayrıntılı yönergelere bakın:

* [StorSimple sanal dizisi Hyper-V'de sağlama](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [StorSimple sanal dizisi vmware'de sağlama](storsimple-virtual-array-deploy2-provision-vmware.md)

