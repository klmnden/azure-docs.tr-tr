---
title: Azure VMware çözümü tarafından CloudSimple hızlı başlangıç - hizmet oluşturma
description: Düğümler ayırmak CloudSimple hizmeti oluşturma ve düğümleri satın alma hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9b3b95db24f4b0f9a0cf8f5102dfeea5dc51e29f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577578"
---
# <a name="quickstart---create-service"></a>Hızlı Başlangıç - hizmet oluşturma

Başlamak için Azure portalında CloudSimple tarafından Azure VMware çözümü oluşturun.

## <a name="vmware-solution-by-cloudsimple---service-overview"></a>VMware çözümüyle CloudSimple - hizmetine genel bakış

CloudSimple service, Azure VMware CloudSimple çözümüyle kullanmasını sağlar.  Hizmet oluşturma, düğümleri satın, düğümler ayırmak ve özel Bulutlar Oluştur olanak tanır.  CloudSimple hizmet kullanılabilir olduğu her Azure bölgesinde CloudSimple hizmet ekleyin.  Hizmet, Azure VMware CloudSimple çözümüyle uç ağı tanımlar.  Bu uç ağı, VPN ve ExpressRoute Internet bağlantısı için özel bulutlarınızın içeren hizmetler için kullanılır.

CloudSimple hizmet eklemek için bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı/28 gerektirir ve edge ağ oluştururken kullanılır CIDR bloğu. Ağ geçidi alt ağ adres alanı benzersiz olmalıdır. Herhangi bir şirket içi ağ adresi alanlarını veya Azure sanal ağ adres alanı ile çakışamaz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="enable-microsoftvmwarecloudsimple-resource-provider"></a>Microsoft.VMwareCloudSimple kaynak Sağlayıcısı'nı etkinleştir

Kaynak sağlayıcı CloudSimple hizmeti etkinleştirmek için aşağıdaki adımları izleyin.

1. **Tüm Hizmetler**’i seçin.
2. Arayın ve seçin **abonelikleri**.

    ![Abonelikleri seçin](media/cloudsimple-service-select-subscriptions.png)

3. CloudSimple hizmetini etkinleştirmek istediğiniz aboneliği seçin
4. Tıklayarak **kaynak sağlayıcıları** abonelik için
5. Kullanım **Microsoft.VMwareCloudSimple** kaynak sağlayıcısı filtrelemek için
6. Seçin **Microsoft.VMwareCloudSimple** kaynak sağlayıcısı ve tıklayarak **kaydetme**

    ![Kaynak sağlayıcısını kaydetme](media/cloudsimple-service-enable-resource-provider.png)

## <a name="create-the-service"></a>Hizmeti oluşturma

>[!NOTE]
> Aboneliğinizde CloudSimple hizmetinin etkinleştirilmesi gerekir. Aboneliğinizi etkinleştirilmezse, hizmet oluşturmaya çalıştığınızda bir hata alırsınız.  Bağlantısındaki [CloudSimple Etkinleştirme hizmeti](https://docs.azure.cloudsimple.com/enable-cloudsimple-service) hizmetini etkinleştirmek için makale.

1. **Tüm Hizmetler**’i seçin.
2. Arama **CloudSimple hizmet**.

    ![Arama CloudSimple hizmeti](media/create-cloudsimple-service-search.png)

3. Seçin **CloudSimple Hizmetleri**.
4. Tıklayın **Ekle** yeni bir hizmet oluşturmak için.

    ![CloudSimple Hizmet Ekle](media/create-cloudsimple-service-add.png)

5. CloudSimple hizmetini oluşturmak istediğiniz aboneliği seçin.
6. Hizmeti için kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için tıklatın **Yeni Oluştur**.
7. Hizmetinizi tanımlayabilmek için adı girin.
8. Hizmet ağ geçidi için CIDR girin. Bir/28'i belirtin. tüm şirket içi alt ağlar, Azure alt ağlar veya planlanan CloudSimple alt ağlar ile çakışmayacak bir alt ağ. Hizmet oluşturulduktan sonra CIDR değiştiremezsiniz.

    ![CloudSimple hizmeti oluşturma](media/create-cloudsimple-service.png)

9. **Tamam** düğmesine tıklayın.

Hizmet oluşturulur ve hizmetler listesine eklenir.

## <a name="purchase-nodes"></a>Satın alma düğümleri

Ödeme--, go kapasite CloudSimple özel bulut ortamı ayarlamak için ilk Azure portalında düğümleri sağlayın.

1. **Tüm Hizmetler**’i seçin.
2. Arama **CloudSimple düğümleri**.

    ![Arama CloudSimple düğümleri](media/create-cloudsimple-node-search.png)

3. Seçin **CloudSimple düğümleri**.
4. Tıklayın **Ekle** düğümleri oluşturma.

    ![CloudSimple düğümleri Ekle](media/create-cloudsimple-node-add.png)

5. CloudSimple düğümleri satın almak istediğiniz aboneliği seçin.
6. Düğümleri için kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için tıklatın **Yeni Oluştur**.
7. Düğümleri tanımlamak için ön eki girin.
8. Düğüm kaynakların konumunu seçin.
9. Düğümü kaynaklarını barındıracak ayrılmış konumu seçin.
10. Düğüm türü seçin. Seçebileceğiniz [CS28 veya CS36 seçeneği](cloudsimple-node.md). İkinci seçeneği, en fazla işlem ve bellek kapasitesini içerir.
11. Sağlama için düğüm sayısını seçin.
12. Seçin **gözden geçir + Oluştur**.
13. Ayarları gözden geçirin. Herhangi bir ayarı değiştirmek için tıklayın **önceki**.
14. **Oluştur**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bulut oluşturma ve ortamını yapılandırma](quickstart-create-private-cloud.md)
* Daha fazla bilgi edinin [CloudSimple hizmeti](https://docs.azure.cloudsimple.com/cloudsimple-service)
