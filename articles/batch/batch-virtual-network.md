---
title: Bir sanal ağ - Azure Batch havuzunda sağlama | Microsoft Docs
description: İşlem düğümleri ağdaki bir dosya sunucusu gibi diğer vm'lerle güvenli bir şekilde iletişim kurabilmesi için bir Azure sanal ağında bir Batch havuzu oluşturma
services: batch
author: laurenhughes
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 04/10/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: ea048c6adbb4e00ae8543810f1dc571376038c62
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67436241"
---
# <a name="create-an-azure-batch-pool-in-a-virtual-network"></a>Sanal ağ içinde bir Azure Batch havuzu oluşturma

Azure Batch havuzu oluşturduğunuzda, bir alt ağdan havuza sağlayabileceğiniz bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md) (VNet), belirttiğiniz. Bu makalede, bir sanal ağda Batch havuzu oluşturulacağı açıklanmaktadır. 

## <a name="why-use-a-vnet"></a>Bir VNet neden kullanmalısınız?

Azure Batch havuzu birbirleri ile - örneğin iletişim kurmak için çok örnekli görevleri çalıştırmak için işlem düğümlerine izin vermek için ayarlara sahiptir. Bu ayarlar, ayrı bir sanal ağ gerekmez. Bununla birlikte, varsayılan olarak, bir lisans sunucusuna veya bir dosya sunucusu gibi bir Batch havuzu parçası olmayan sanal makineler ile düğümleri iletişim kuramıyor. Güvenli bir şekilde diğer sanal makinelerle veya bir şirket içi ağ ile iletişim kurmak işlem düğümleri havuzu izin vermek için bir Azure sanal ağ alt ağı havuzunda sağlayabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

* **Kimlik Doğrulaması**. Azure sanal ağı kullanmak için Batch istemci API'sinin Azure Active Directory (AD) kimlik doğrulamasını kullanması gerekir. Azure AD için Azure Batch desteği, [Batch hizmeti çözümlerinin kimliğini Active Directory ile doğrulama](batch-aad-auth.md) makalesinde belirtilmiştir. 

* **Bir Azure sanal ağı**. Sanal ağ gereksinimleri ve yapılandırma için aşağıdaki bölüme bakın. Bir sanal ağ bir veya daha fazla alt ağ ile önceden hazırlamak için Azure portalı, Azure PowerShell, Azure komut satırı arabirimi (CLI) veya diğer yöntemleri kullanabilirsiniz.  
  * Bir Azure Resource Manager tabanlı sanal ağ oluşturmak için bkz [sanal ağ oluşturma](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Resource Manager tabanlı sanal ağ, yeni dağıtımlar için önerilen ve yalnızca sanal makine yapılandırmasındaki havuzlarda desteklenir.
  * Klasik bir VNet oluşturmak için bkz: [birden çok alt ağa sahip bir sanal ağ (Klasik) oluşturmak](../virtual-network/create-virtual-network-classic.md). Klasik sanal ağ yalnızca ağ geçidi yapılandırmasındaki havuzlarda desteklenir.

## <a name="vnet-requirements"></a>Sanal ağ gereksinimleri

[!INCLUDE [batch-virtual-network-ports](../../includes/batch-virtual-network-ports.md)]

## <a name="create-a-pool-with-a-vnet-in-the-portal"></a>Sanal ağ içinde portal ile havuz oluşturma

Sanal ağınızdaki bir kez oluşturdunuz ve bir alt ağ atanmış, bu sanal ağ ile bir Batch havuzu oluşturabilirsiniz. Azure portalından bir havuz oluşturmak için aşağıdaki adımları izleyin: 

1. Azure portalında Batch hesabınıza gidin. Bu hesabı kullanmayı planlıyorsanız sanal ağ içeren bir kaynak grubu aynı abonelik ve aynı bölgede olması gerekir. 
2. İçinde **ayarları** penceresi seçin sol taraftaki **havuzları** menü öğesi.
3. İçinde **havuzları** penceresinde **Ekle** komutu.
4. Üzerinde **havuzu Ekle** penceresinde seçeneğini kullanmak için istediğinize **görüntü türü** açılır. 
5. Doğru seçin **yayımcı/teklif/Sku** için özel görüntü.
6. Gerekli ayarları dahil olmak üzere, kalan belirtin **düğüm boyutu**, **hedef adanmış düğümler**, ve **düşük öncelikli düğümler**, yanı sıra tüm istenen isteğe bağlı ayarlar.
7. İçinde **sanal ağ**, kullanmak istediğiniz alt ağ ve sanal ağ seçin.
  
   ![Sanal ağ ile havuz Ekle](./media/batch-virtual-network/add-vnet-pool.png)

## <a name="user-defined-routes-for-forced-tunneling"></a>Kullanıcı tanımlı yollar, zorlamalı tünel

Gereksinimleri, kuruluşunuzda (zorla) yeniden yönlendirme İnternet'e bağlı trafiği alt ağdan geri inceleme ve günlüğe kaydetme, şirket içi konumunuz gerekebilir. Sanal alt ağlar için zorlamalı tünel etkin. 

Azure Batch havuzu işlem düğümleriniz zorlamalı tünel etkinse bir Vnet'te çalıştığından emin olmak için aşağıdakileri ekleyin [kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md) bu alt ağ için:

* Batch hizmetinin görevleri zamanlamak için işlem düğümleri havuzu ile iletişim kurması gerekiyor. Bu iletişimi etkinleştirmek için Batch hesabınızın bulunduğu bölgede Batch hizmeti tarafından kullanılan her bir IP adresi için kullanıcı tanımlı bir yol ekleyin. Batch hizmetinin IP adresleri listesi elde etmek üzere öğrenmek için bkz: [hizmet etiketleri şirket içi](../virtual-network/security-overview.md#service-tags-in-on-premises)

* Azure depolama, giden trafiği emin olun (özellikle biçimindeki URL'ler `<account>.table.core.windows.net`, `<account>.queue.core.windows.net`, ve `<account>.blob.core.windows.net`) şirket içi ağ aletiniz engellenmez.

Kullanıcı tanımlı bir yol eklediğinizde, her bir ilgili Batch IP adresi ön eki için rota tanımlayın ve ayarlayın **sonraki atlama türü** için **Internet**. Aşağıdaki örneğe bakın:

![Kullanıcı tanımlı yol](./media/batch-virtual-network/user-defined-route.png)

## <a name="next-steps"></a>Sonraki adımlar

- Batch ayrıntılı genel bakış için bkz: [büyük ölçekli paralel işlem çözümleri geliştirme batch'le](batch-api-basics.md).
- Kullanıcı tanımlı bir yol oluşturma hakkında daha fazla bilgi için bkz. [kullanıcı tanımlı yönlendirme - Azure portalında oluşturma](../virtual-network/tutorial-create-route-table-portal.md).
