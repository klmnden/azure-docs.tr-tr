---
title: StorSimple sanal dizisi için Portal hazırlığı | Microsoft Docs
description: StorSimple sanal dizinin dağıtmak için ilk öğreticide Azure portal hazırlanmasını içerir
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
ms.date: 11/14/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6685c5ab7768176a0c8e7084c8512d5345732d9a
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
ms.locfileid: "24526562"
---
# <a name="deploy-storsimple-virtual-array---prepare-the-azure-portal"></a>StorSimple sanal dizinin dağıtma - Azure portalında hazırlama

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a>Genel Bakış

Serideki ilk makalesi dağıtım öğreticileri tamamen sanal dizinizi bir dosya sunucusu veya Resource Manager modelini kullanarak bir iSCSI sunucu olarak dağıtmak için gerekli olan budur. Bu makalede oluşturmak ve sanal bir dizi sağlama önce StorSimple cihaz Yöneticisi hizmetini yapılandırmak için gereken hazırlık açıklanmaktadır. Bu makalede ayrıca çıkışı bir dağıtım yapılandırma denetim listesi ve yapılandırma için önkoşulları bağlar.

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Başlamadan önce dağıtım yapılandırma denetim listesini gözden geçirmenizi öneririz. Portal hazırlık 10 dakikadan daha kısa sürer.

Bu makalede yayımlanmış bilgiler Azure portalı ve Microsoft Azure kamu bulut StorSimple sanal diziler dağıtımda için geçerlidir.

### <a name="get-started"></a>Kullanmaya başlayın
Dağıtım iş akışı portal hazırlama, sanallaştırılmış ortamınızdaki sanal bir dizi sağlama ve Kurulumu Tamamlanıyor oluşur. Bir dosya sunucusu veya bir iSCSI sunucusu olarak StorSimple sanal dizinin dağıtım ile çalışmaya başlamak için aşağıdaki tabulated kaynaklara başvurmanız gerekir.

#### <a name="deployment-articles"></a>Dağıtım makaleleri

StorSimple sanal dizinizi dağıtmak için belirtilen sırada aşağıdaki makalelere bakın.

| **#** | **Bu adımda** | **Bunu...** | **Ve bu belgeleri kullanın.** |
| --- | --- | --- | --- |
| 1. |**Azure Portalı'nı ayarlama** |Oluşturun ve StorSimple sanal dizinin sağlama önce StorSimple cihaz Yöneticisi hizmetini yapılandırın. |[Portal hazırlama](storsimple-virtual-array-deploy1-portal-prep.md) |
| 2. |**Sanal dizinin sağlama** |Hyper-v, sağlamak ve bir StorSimple sanal dizisi Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 üzerinde Hyper-V çalıştıran bir konak sisteminde bağlanın. <br></br> <br></br> VMware için sağlamak ve VMware ESXi 5.0, 5.5 veya 6.0 çalıştıran konak sisteminde bir StorSimple sanal diziye bağlanın.<br></br> |[Hyper-V sanal bir dizide sağlama](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [VMware sanal bir dizide sağlama](storsimple-virtual-array-deploy2-provision-vmware.md) |
| 3. |**Sanal dizinin Kurulumu ayarlayın** |Dosya sunucunuz için ilk kurulum gerçekleştirmek, StorSimple dosya sunucunuzu kaydetmek ve cihaz kurulumunu tamamlayın. Ardından, SMB paylaşımları sağlayabilirsiniz. <br></br> <br></br> İSCSI sunucunuz için ilk kurulum gerçekleştirmek, StorSimple iSCSI sunucuyu kaydetmek ve cihaz kurulumunu tamamlayın. Ardından, iSCSI birimleri sağlayabilirsiniz. |[Sanal dizinin Kurulumu dosya sunucusu olarak ayarlayın](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[Sanal dizinin Kurulumu iSCSI sunucusu olarak ayarla](storsimple-virtual-array-deploy3-iscsi-setup.md) |

Azure Portalı'nı ayarlama şimdi başlayabilirsiniz.

## <a name="configuration-checklist"></a>Yapılandırma denetim listesi

Yapılandırma denetim listesinde, StorSimple sanal dizisinde yazılım yapılandırmadan önce toplamanız gereken bilgiler açıklanmıştır. Bu bilgileri önceden hazırlama, ortamınızda StorSimple cihazı dağıtma işlemini kolaylaştırmaya yardımcı olur. StorSimple sanal dizinizi bir dosya sunucusu veya bir iSCSI sunucu olarak dağıtılan bağlı bağlı olarak, aşağıdaki denetim listelerini biri gerekir.

* Karşıdan [StorSimple sanal dizinin dosya sunucusu yapılandırma denetim listesi](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).
* Karşıdan [StorSimple sanal dizinin iSCSI sunucu yapılandırma denetim listesi](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Önkoşullar

Burada, StorSimple cihaz Yöneticisi hizmetiniz, StorSimple sanal dizinizi ve veri merkezi ağ için yapılandırma önkoşulları öğrenin.

### <a name="for-the-storsimple-device-manager-service"></a>StorSimple Cihaz Yöneticisi hizmeti için

Başlamadan önce aşağıdakilerden emin olun:

* Erişim kimlik bilgilerine sahip bir Microsoft hesabınız var.
* Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.
* Microsoft Azure aboneliğiniz StorSimple cihaz Yöneticisi hizmeti için etkinleştirilmiş olmalıdır.

### <a name="for-the-storsimple-virtual-array"></a>StorSimple sanal dizi için

Sanal bir dizi dağıtmadan önce emin olun:

* Windows Server 2008 R2 veya daha sonra Hyper-V çalıştıran ana bilgisayar sistemi ya da olabilir (ESXi 5.0, 5.5 veya 6.0) VMware erişimi sağlamak için kullanılan bir cihaz.
* Ana bilgisayar sistemi sanal dizinizi sağlamak için aşağıdaki kaynaklara ayrılması yapabiliyor:
  
  * 4 çekirdek en az.
  * En az 8 GB RAM. Dosya sunucusu olarak sanal dizinin yapılandırmayı planlıyorsanız, 8 GB 2 milyon dosyalarını destekler. 16 GB RAM, 2-4 milyon dosyaları desteklemek için gerekir.
  * Bir ağ arabirimi.
  * Sistem verileri için 500 GB sanal disk.

### <a name="for-the-datacenter-network"></a>Veri Merkezi ağ için

Başlamadan önce aşağıdakilerden emin olun:

* Veri merkezinizdeki ağ StorSimple cihazınız için ağ gereksinimlerine uygun yapılandırılır. Daha fazla bilgi için bkz: [StorSimple sanal dizinin sistem gereksinimleri](storsimple-ova-system-requirements.md).
* StorSimple sanal dizinizi ayrılmış 5 MB/sn Internet bant genişliği (veya daha fazla) olduğundan her zaman kullanılabilir. Bu bant genişliği herhangi bir uygulama ile Paylaşılmaması gerekiyor.

## <a name="step-by-step-preparation"></a>Adım adım hazırlama

StorSimple cihaz Yöneticisi hizmeti için portalınızı hazırlamak için aşağıdaki adım adım yönergeleri kullanın.

## <a name="step-1-create-a-new-service"></a>1. Adım: Yeni bir hizmet oluşturun

StorSimple cihaz Yöneticisi hizmeti tek bir örneği birden çok StorSimple sanal diziler yönetebilirsiniz. StorSimple Cihaz Yöneticisi hizmetinin bir örneğini oluşturmak için aşağıdaki adımları gerçekleştirin. Sanal dizileriniz yönetmek için mevcut bir StorSimple cihaz Yöneticisi hizmetiniz varsa, bu adımı atlayın ve Git [2. adım: Hizmet kayıt anahtarını alın](#step-2-get-the-service-registration-key).

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> Hizmetinizle birlikte bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirmediyseniz, bir hizmeti başarıyla oluşturduktan sonra en az bir depolama hesabı oluşturmanız gerekir.
> 
> * Otomatik olarak bir depolama hesabı oluşturmadıysanız, ayrıntılı yönergeler için [Hizmet için yeni bir depolama hesabı yapılandırma](#optional-step-configure-a-new-storage-account-for-the-service) bölümüne gidin.
> * Bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirdiyseniz, [2. Adım: Hizmet kayıt anahtarını alın](#step-2-get-the-service-registration-key) bölümüne gidin.
> 
> 

## <a name="step-2-get-the-service-registration-key"></a>2. Adım: Hizmet kayıt anahtarını alın

StorSimple Cihaz Yöneticisi hizmeti çalışır duruma geldikten sonra, hizmet kayıt anahtarını almanız gerekir. Bu anahtar StorSimple cihazınızı kaydetmek ve hizmete bağlamak için kullanılır.

Aşağıdaki adımlarda gerçekleştirmek [Azure portal](https://portal.azure.com/).

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> Hizmet kayıt anahtarı, StorSimple cihaz Yöneticisi hizmetiniz ile kaydetmek için gereken tüm StorSimple Aygıt Yöneticisi'ni cihazları kaydetmek için kullanılır.
> 
> 

## <a name="step-3-download-the-virtual-array-image"></a>3. adım: sanal dizinin görüntüsünü karşıdan yükle

Hizmet kayıt anahtarını oluşturduktan sonra sanal bir dizi ana bilgisayar sistemine sağlamak için uygun sanal dizinin görüntüsünü karşıdan yüklemek gerekecektir. Sanal dizinin görüntüleri, işletim sisteminin belirli ve Azure portalında hızlı başlangıç sayfasından indirilebilir.

> [!IMPORTANT]
> StorSimple sanal dizi çalışan yazılımın yalnızca StorSimple cihaz Yöneticisi hizmeti ile kullanılabilir.
> 
> 

Aşağıdaki adımlarda gerçekleştirmek [Azure portal](https://portal.azure.com/).

#### <a name="to-get-the-virtual-array-image"></a>Sanal dizinin görüntü almak için

1. [Azure portal](https://portal.azure.com/) oturum açın. 
2. Azure portalında tıklatın **Gözat > StorSimple cihaz yöneticileri**.
3. Var olan bir StorSimple cihaz Yöneticisi hizmetini seçin. İçinde **StorSimple Aygıt Yöneticisi'ni** dikey penceresinde tıklatın **Hızlı Başlangıç**. 
4. Microsoft Download Center'dan gelen yüklemek istediğiniz görüntünün karşılık gelen bağlantıya tıklayın. Yaklaşık 4.8 GB görüntü dosyalarıdır.
   
   * Hyper-V Windows Server 2012 ve sonraki sürümler için VHDX
   * VHD için Hyper-V Windows Server 2008 R2 ve sonraki sürümler
   * VMWare ESXi 5.0, 5.5 veya 6.0 için VMDK
5. Karşıdan yükleyip sıkıştırması açılmış dosyasının bulunduğu bir not yaparak bir yerel sürücüye dosyanın sıkıştırmasını açın.

## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>İsteğe bağlı adım: hizmet için yeni bir depolama hesabı yapılandırma

Bu adım isteğe bağlıdır ve yalnızca, bir depolama hesabı otomatik olarak oluşturulmasını hizmetiniz ile etkinleştirmediyseniz gerçekleştirilmelidir.

Farklı bir bölgede bir Azure depolama hesabı oluşturmanız gerekiyorsa, bkz: [bir depolama hesabı oluşturmak nasıl](../storage/common/storage-create-storage-account.md#create-a-storage-account) adım adım yönergeler için.

Aşağıdaki adımlarda gerçekleştirmek [Azure portal](https://ms.portal.azure.com/) var olan bir Microsoft Azure depolama hesabı eklemek için StorSimple cihaz Yöneticisi hizmet sayfasında.

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a>Aygıt Yöneticisi hizmeti aynı Azure abonelik sahip bir depolama hesabı kimlik bilgilerini eklemek için

1. Aygıt Yöneticisi hizmetinize select gidin ve çift tıklatın. Bu açılır **genel bakış** dikey.
2. Seçin **depolama hesabının kimlik bilgilerini** içinde **yapılandırma** bölümü.
3. **Ekle**'ye tıklayın.
4. İçinde **depolama hesabı ekleme** dikey penceresinde aşağıdakileri yapın:
   
    1. İçin **abonelik**seçin **geçerli**.
   
    2. Azure depolama hesabınızın adını sağlayın.
   
    3. Seçin **etkinleştirmek** StorSimple cihazınız ve bulut arasındaki ağ iletişimi için güvenli bir kanal oluşturmak için. Seçin **devre dışı** yalnızca özel bir bulutta işlem yapıyorsanız.
   
    4. **Ekle**'ye tıklayın. Depolama hesabı başarıyla oluşturulduktan sonra size bildirilir.<br></br>
   
     ![Varolan bir depolama hesabı kimlik bilgileri Ekle](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a>Sonraki adım

Sonraki adım, StorSimple sanal dizisi için bir sanal makine sağlamaya yöneliktir. Ana bilgisayar işletim sistemine bağlı olarak ayrıntılı yönergelere bakın:

* [Hyper-V StorSimple sanal dizisinde sağlama](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [VMware StorSimple sanal dizisinde sağlama](storsimple-virtual-array-deploy2-provision-vmware.md)

