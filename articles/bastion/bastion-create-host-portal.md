---
title: Bir Azure Burcu ana bilgisayarı oluşturma | Microsoft Docs
description: Bu makalede, bir Azure Burcu ana bilgisayarı oluşturmayı öğrenin
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: cherylmc
ms.openlocfilehash: 4a52383e6ab24c6ae1e2be0b67293d65dfa04466
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477873"
---
# <a name="create-an-azure-bastion-host-preview"></a>Bir Azure Burcu ana bilgisayarı (Önizleme) oluşturma

Bu makalede bir Azure savunma ana bilgisayarını nasıl oluşturabileceğinizi gösterir. Sanal ağınızda Azure savunma hizmet sağladıktan sonra RDP/SSH'yi sorunsuzca aynı sanal ağdaki tüm Vm'leriniz için kullanılabilir. Bu dağıtım, abonelik/hesap veya sanal makine değil sanal ağ başına yapılır.

Savunma ana bilgisayar kaynağı oluşturmanın iki yolu vardır:

* Azure portalını kullanarak bir savunma kaynağı oluşturun.
* Bir savunma kaynağı, Azure portalında mevcut VM ayarlarını kullanarak oluşturun.

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki Azure genel bölgeler ile sınırlı genel önizleme:

[!INCLUDE [available regions](../../includes/bastion-regions-include.md)]

## <a name="createhost"></a>Kale ana bilgisayarı oluşturma

Bu bölümde, Azure portalından yeni Azure savunma kaynak oluşturmanıza yardımcı olur.

1. İçinde giriş sayfasından [Azure portalı - savunma Önizleme](https://aka.ms/BastionHost), tıklayın **+ kaynak Oluştur**. Bu önizleme, normal Azure portalın değil için portala erişmek için sağlanan bağlantıyı kullandığınızdan emin olun.

1. Üzerinde **yeni** sayfasında *markette Ara* alanına **savunma**, ardından **Enter** arama sonuçlarını almak için.

1. Sonuçlardan tıklayın **savunma (Önizleme)** . Yayımcı olduğundan emin olun *Microsoft* ve kategori *ağ*.

1. Üzerinde **savunma (Önizleme)** sayfasında **Oluştur** açmak için **bir savunma oluşturma** sayfası.

1. Üzerinde **bir savunma oluşturma** sayfasında, yeni bir savunma kaynağı yapılandırın. Savunma kaynağınız için yapılandırma ayarlarını belirtin.

    ![bir savunma oluşturma](./media/bastion-create-host-portal/settings.png)

    * **Abonelik**: Yeni bir savunma kaynak oluşturmak için kullanmak istediğiniz Azure aboneliği.
    * **Kaynak grubu**: Azure kaynak grubu içinde yeni savunma kaynak içinde oluşturulur. Mevcut bir kaynak grubu yoksa, yeni bir tane oluşturabilirsiniz.
    * **Ad**: Yeni savunma kaynağın adı
    * **Bölge**: Kaynağın oluşturulduğu Azure genel bölge.
    * **Sanal ağ**: Sanal ağ içinde savunma kaynak içinde oluşturulur. Durumda yoksa veya mevcut bir sanal ağ kullanmak istemiyorsanız, bu işlem sırasında portalda yeni bir sanal ağ oluşturabilirsiniz. Bir sanal ağınız kullanıyorsanız, var olan sanal ağ'ı savunma alt ağ gereksinimlerini karşılamak için yeterli boş adres alanı olduğundan emin olun.
    * **Alt ağ**: Sanal ağınızda yeni savunma ana kaynak alt dağıtılır. Ad değeri kullanarak bir alt ağ oluşturmanız gerekir **AzureBastionSubnet**. Bu değer, Azure alt ağı için savunma kaynakları dağıtmak için bilmeniz olanak tanır. Bu, bir ağ geçidi alt ağı farklıdır. En az/27 veya daha büyük alt ağı kullanmanız önerilir (/ 27, / 26 vb.). Oluşturma **AzureBastionSubnet** herhangi bir ağ güvenliği grupları olmadan, tablolar veya temsilciler yönlendirebilirsiniz.
    * **Genel IP adresi**: RDP/SSH (bağlantı noktası 443 üzerinden) erişilecek savunma kaynağın genel IP. Yeni bir genel IP oluşturun veya var olanı kullanın. Genel IP adresini, oluşturmakta olduğunuz savunma kaynak ile aynı bölgede olması gerekir.
    * **Genel IP adresi adı**: Genel IP adresi kaynağı adı.
    * **Genel IP adresi SKU**: Varsayılan olarak önceden doldurulmuş **standart**.
    * **Atama**: Varsayılan olarak önceden doldurulmuş **statik**.

1. Ayarları belirttikten tıklayın **gözden geçir + Oluştur**. Bu değerleri doğrular. Doğrulama başarılı olduktan sonra oluşturma işlemine başlayabilirsiniz.
1. Savunma sayfası oluşturma üzerinde tıklayın **Oluştur**.
1. Dağıtımınızı devam ettiği olduğunu bildiğiniz bir ileti görüntülenir. Kaynaklar oluşturulurken durum bu sayfada görüntülenir. Savunma kaynağının oluşturulup dağıtıldığında yaklaşık 5 dakika sürer.

## <a name="createvmset"></a>VM ayarlarını kullanarak bir savunma konağı oluşturma

Varolan bir VM'yi kullanarak portalda oluşturduğunuz Burcu ana bilgisayarı, çeşitli ayarlar otomatik olarak, sanal makine ve/veya sanal ağ için karşılık gelen varsayılan olarak atar.

1. İçinde [Azure portalı - savunma Önizleme](https://aka.ms/BastionHost), sanal makinenize gidin ve ardından tıklayın **Connect**.

    ![VM'ye bağlanın](./media/bastion-create-host-portal/vmsettings.png)

1. Sağ Kenar çubuğunda tıklatın **savunma**, ardından **kullanım savunma**.

    ![Bastion](./media/bastion-create-host-portal/vmbastion.png)

1. Savunma sayfasında aşağıdaki ayarları alanları doldurun:

    * **Ad**: Oluşturmak istediğiniz savunma ana bilgisayar adı.
    * **Alt ağ**: Kaynak için hangi savunma dağıtılacak sanal ağınız içindeki alt ağ. Alt ağ adı ile oluşturulmalıdır **AzureBastionSubnet**. Bu alt ağı savunma kaynağa dağıtmak için bilmeniz Azure sağlar. Bu, bir ağ geçidi alt ağı farklıdır. Tıklayın **Yönet alt ağ yapılandırması** Azure savunma alt ağı oluşturmak için. En az/27 veya daha büyük alt ağı kullanmanız önerilir (/ 27, / 26, vb..). Oluşturma **AzureBastionSubnet** herhangi bir ağ güvenliği grupları olmadan, tablolar veya temsilciler yönlendirebilirsiniz. Tıklayın **Oluştur** alt ağ oluşturmak için ardından sonraki ayarları ile devam edin.

      ![Bastion](./media/bastion-create-host-portal/subnet.png)
      
    * **Genel IP adresi**: RDP/SSH (bağlantı noktası 443 üzerinden) erişilecek savunma kaynağın genel IP. Yeni bir genel IP oluşturun veya var olanı kullanın. Genel IP adresini, oluşturmakta olduğunuz savunma kaynak ile aynı bölgede olması gerekir.
    * **Genel IP adresi adı**: Genel IP adresi kaynağı adı.
1. Doğrulama ekrana tıklayın **Oluştur**. Savunma kaynağının oluşturulup dağıtıldığında yaklaşık 5 dakika bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Okuma [savunma SSS](bastion-faq.md)
