---
title: "Bir sanal ağ Azure Batch havuzunda sağlama | Microsoft Docs"
description: "İşlem düğümlerine güvenli bir şekilde ağındaki bir dosya sunucusu gibi diğer VM'ler ile iletişim kurabilmesi için Batch havuzundaki bir sanal ağ oluşturabilirsiniz."
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 02/05/2018
ms.author: danlep
ms.openlocfilehash: 5a06ad5086a42bb00147e085227f3c71c357544e
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="create-an-azure-batch-pool-in-a-virtual-network"></a>Bir sanal ağ içinde bir Azure Batch havuzu oluşturma


Bir Azure Batch havuzu oluşturduğunuzda, bir alt ağdan havuzunda sağlayabilirsiniz bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md) (VNet) belirttiğiniz. Bu makalede, bir VNet Batch havuzunda ayarlama açıklanmaktadır. 



## <a name="why-use-a-vnet"></a>Bir VNet neden kullanılır?


Bir Azure Batch havuzunda işlem düğümlerini birbirleri ile - örneğin iletişim kurmak için çok örnekli görevleri çalıştırmak için izin vermek için ayarlara sahip. Bu ayarları ayrı VNet gerektirmez. Ancak, varsayılan olarak, bir lisans sunucusuna veya bir dosya sunucusu gibi Batch havuzu parçası olmayan sanal makineler ile düğümleri iletişim kuramıyor. Havuzu işlem düğümlerine güvenli bir şekilde diğer sanal makinelerle veya bir şirket içi ağ ile iletişim kurmasına izin vermek için bir Azure sanal alt ağdan havuzunda sağlayabilirsiniz. 



## <a name="prerequisites"></a>Önkoşullar

* **Kimlik Doğrulaması**. Azure sanal ağı kullanmak için Batch istemci API'sinin Azure Active Directory (AD) kimlik doğrulamasını kullanması gerekir. Azure AD için Azure Batch desteği, [Batch hizmeti çözümlerinin kimliğini Active Directory ile doğrulama](batch-aad-auth.md) makalesinde belirtilmiştir. 

* **Bir Azure sanal**. Bir sanal ağ bir veya daha fazla alt ağ ile önceden hazırlamak için Azure portalı, Azure PowerShell, Azure komut satırı arabirimi (CLI) veya başka yöntemler kullanabilirsiniz. Bir Azure Resource Manager tabanlı VNet oluşturmak için bkz: [bir sanal ağ oluşturma](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Klasik bir VNet oluşturmak için bkz: [birden çok alt ağı ile bir sanal ağ (Klasik) oluşturmak](../virtual-network/create-virtual-network-classic.md).

### <a name="vnet-requirements"></a>Sanal ağ gereksinimleri
[!INCLUDE [batch-virtual-network-ports](../../includes/batch-virtual-network-ports.md)]
    
## <a name="create-a-pool-with-a-vnet-in-the-portal"></a>Portalda VNet ile havuz oluşturma

Sanal ağınızı kez oluşturdunuz ve bir alt ağ atanmış, o VNet ile birlikte bir Batch havuzu oluşturabilirsiniz. Azure portalından bir havuzu oluşturmak için aşağıdaki adımları izleyin: 



1. Azure portalında Batch hesabınıza gidin. Bu hesabı aynı abonelikte ve bölgede kullanmayı düşündüğünüz VNet içeren kaynak grubunu şu şekilde olması gerekir. 
2. İçinde **ayarları** penceresinde soldaki select **havuzları** menü öğesi.
3. İçinde **havuzları** penceresinde, seçin **Ekle** komutu.
4. Üzerinde **havuzu Ekle** penceresinde, gelen kullanmayı düşündüğünüz seçeneğini **görüntü türü** açılır. 
5. Doğru seçin **teklif/yayımcı/Sku** özel görüntünüzü için.
6. Gerekli ayarları dahil olmak üzere, kalan belirtin **düğüm boyutu**, **ayrılmış düğümleri hedef**, ve **düşük öncelik düğümleri**, yanı sıra tüm isteğe bağlı ayarlar istenen.
7. İçinde **sanal ağ**, kullanmak istediğiniz alt ağ ve sanal ağ seçin.
  
  ![Sanal ağ ile havuzu ekleme](./media/batch-virtual-network/add-vnet-pool.png)

## <a name="user-defined-routes-for-forced-tunneling"></a>Kullanıcı tanımlı yollar zorlanan tünel

Gereksinimler, kuruluşunuzda yeniden yönlendirme (zorla) Internet'e bağlı trafik alt ağdan geri şirket içi konumunuz denetleme ve günlüğe kaydetme için gerekebilir. Vnet'inizi alt ağlar için zorlamalı tünel etkin. 

Azure Batch havuzu işlem düğümleriniz etkin tünel gerektirdi bir VNet içinde çalıştığından emin olmak için aşağıdakileri ekleyin [kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md) bu alt ağ için:

* Batch hizmeti görevleri zamanlamak için işlem düğümleri havuzu ile iletişim kurması gerekiyor. Bu iletişimi etkinleştirmek için Batch hesabınıza bulunduğu bölgede Batch hizmeti tarafından kullanılan her bir IP adresi için bir kullanıcı tarafından tanımlanan rota ekleyin. Batch hizmeti, IP adreslerinin listesi elde etmek için lütfen Azure desteğine başvurun.

* Azure Storage bu giden trafiği emin olun (özellikle, formun URL'lerini `<account>.table.core.windows.net`, `<account>.queue.core.windows.net`, ve `<account>.blob.core.windows.net`), şirket içi ağ Gereci engellenmez.

Bir kullanıcı tarafından tanımlanan rota eklediğinizde, ilgili her toplu IP adresi ön eki için rota tanımlayın ve ayarlayın **sonraki atlama türü** için **Internet**. Aşağıdaki örneğe bakın:

![Kullanıcı tanımlı yol](./media/batch-virtual-network/user-defined-route.png)

## <a name="next-steps"></a>Sonraki adımlar

- Toplu ayrıntılı bir bakış için bkz: [geliştirme büyük ölçekli paralel işlem çözümleri yığın](batch-api-basics.md).
- Bir kullanıcı tarafından tanımlanan rota oluşturma hakkında daha fazla bilgi için bkz: [Azure portalında bir kullanıcı tarafından tanımlanan rota - oluşturmak](../virtual-network/tutorial-create-route-table-portal.md).
