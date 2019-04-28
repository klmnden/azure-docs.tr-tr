---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 55e46e058bddca717929df61b2bc766b89e0f885
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485350"
---
![Tek başına bulut hizmetinde sanal makineler](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Bir sanal ağda sanal makinelerinizi yerleştirirseniz, kaç bulut Hizmetleri için kullanmak istediğiniz yük dengeleme ve kullanılabilirlik kümeleri karar verebilirsiniz. Ayrıca, şirket içi ağ ve sanal ağ, şirket içi ağınıza bağlanmak ağlardaki sanal makineler aynı şekilde düzenleyebilirsiniz. Bir örneği aşağıda verilmiştir:

![Bir sanal ağdaki sanal makineler](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Sanal ağlar, azure'da sanal makineleri bağlamak için önerilen yoldur. Uygulamanızın her bir katman içinde ayrı bulut hizmeti yapılandırmak için en iyi yöntem olacaktır. Ancak, en yüksek abonelik başına 200 bulut Hizmetleri içinde kalması için aynı bulut hizmeti bazı sanal makinelerden farklı uygulama katmanları birleştirin gerekebilir. Bu ve diğer sınırlamaları gözden geçirmek için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Sanal makineleri bir sanal ağ bağlama
Bir sanal ağdaki sanal makinelere bağlanmak için:

1. Sanal ağ oluşturma [Azure portalında](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) 'Klasik dağıtım' belirtin.
2. Dağıtımınızın tasarımınızı kullanılabilirlik kümeleri için yansıtır ve Yük Dengeleme bulut Hizmetleri kümesi oluşturun. Azure portalında **kaynak Oluştur > işlem > bulut hizmeti** her bulut hizmeti için.

   Bulut hizmeti ayrıntıları doldururken aynı seçin _kaynak grubu_ sanal ağ ile kullanılır.

3. Her yeni bir sanal makine oluşturmak için tıklayın **kaynak Oluştur > işlem**, uygun sanal makine görüntüsünden seçip **öne çıkan uygulamalar**.

   VM'de **Temelleri** dikey penceresinde aynı seçin _kaynak grubu_ sanal ağ ile kullanılır.

   ![Bir sanal ağ kullanılırken VM temel bilgiler dikey penceresi](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. VM'i doldururken **ayarları**, doğru seçin _bulut hizmeti_ veya _sanal ağ_ VM için.

   Azure, seçiminize göre bir öğe seçer.

   ![Bir sanal ağ kullanılırken VM ayarlar dikey penceresi](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a>Vm'leri bir tek başına bulut hizmetine bağlama
Bir tek başına bulut hizmeti sanal makinelere bağlanmak için:

1. Bulut hizmeti oluşturma [Azure portalında](http://portal.azure.com). Tıklayın **yeni > işlem > bulut hizmeti**. Veya ilk sanal makinenizi oluşturduğunuzda, bulut hizmeti dağıtımınız için oluşturabilirsiniz.
2. Sanal makine oluşturduğunuzda, bulut hizmetiyle birlikte kullanılan kaynak grubunu seçin.

   ![Var olan bir bulut hizmeti için bir sanal makine ekleyin](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3. Sanal makine ayrıntıları bölümünü doldurun gibi bulut hizmeti ilk adımda oluşturduğunuz adını seçin.

   ![Bir bulut hizmeti için bir sanal makine seçme](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
