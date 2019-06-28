---
title: VMware CloudSimple özel bulut çözümüyle Azure'ı yönetme
description: Etkinlik ve CloudSimple özel bulut kaynaklarını yönetmek kullanılabilen özellikleri açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 05a2fb451b3acce1011c1d5f4cf17f0a865d57d0
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332668"
---
# <a name="manage-private-cloud-resources-and-activity"></a>Özel bulut kaynaklarını ve etkinlik yönetme

Özel Bulutlar CloudSimple portalından yönetilir.  Durum, kullanılabilir kaynakları özel Bulut ve diğer ayarları CloudSimple portalından etkinliği denetleyin.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

Erişim [CloudSimple portalı](access-cloudsimple-portal.md).

## <a name="view-the-list-of-private-clouds"></a>Özel bulutların listesini görüntüleyin

**Özel Bulutları** sekmesinde **kaynakları** sayfası aboneliğinizdeki tüm özel Bulutları listeler. VSphere sayısı kümeleri, konum, özel bulut, kaynak bilgilerini ve geçerli durumunu, bilgi'ı adı içerir.

![Özel bulut sayfası](media/manage-private-cloud.png)

Ek bilgiler ve Eylemler için özel bir bulut seçin.

## <a name="private-cloud-summary"></a>Özel bulut özeti

Seçili özel bulut kapsamlı bir özetini görüntüleyin.  Özet sayfasında, üzerine özel buluta dağıtılan DNS sunucularını içerir.  Şirket içi DNS sunucularından, özel bulut DNS sunucularına iletme DNS ayarlayabilirsiniz.  DNS iletme hakkında daha fazla bilgi için bkz. [şirket içi vCenter özel bulut için ad çözümlemesi için DNS yapılandırma](https://docs.azure.cloudsimple.com/on-premises-dns-setup/).

![Özel bulut özeti](media/private-cloud-summary.png)

### <a name="available-actions"></a>Kullanılabilir eylemler

* [VSphere istemcisini başlatma](https://docs.azure.cloudsimple.com/vsphere-access/). Bu özel bulut için vCenter erişim.
* [Düğümleri satın](create-nodes.md). Düğümleri bu özel buluta ekleyin.
* [Genişletin](expand-private-cloud.md). Düğümleri bu özel buluta ekleyin.
* **Yenileme**. Bu sayfadaki bilgileri güncelleştirin.
* **Silme**. Özel bulut, istediğiniz zaman silebilirsiniz. **Silmeden önce tüm sistemler ve veriler yedekledim emin olun.** Bir özel bulut silinmesi, tüm sanal makineleri, vCenter yapılandırma ve verileri siler. Tıklayın **Sil** seçili özel bulut için Özet bölümünde. Silme işlemi tüm özel bulut verileri güvenli ve yüksek düzeyde uyumlu silinme işlemde silinir.
* [VSphere ayrıcalıkları değiştirme](escalate-private-cloud-privileges.md).  Bu özel bulutta sizin ayrıcalıklarınıza yöneticinize iletin.

## <a name="private-cloud-vlanssubnets"></a>Özel bulut VLAN'lar/alt ağlar

Seçili özel bulut için tanımlanan VLAN'lar/alt ağlar listesini görüntüleyin.  Listenin, özel bulut oluştururken VLAN'lar/alt ağlar oluşturulan yönetim içerir.

![Özel bulut - VLAN'lar/alt ağlar](media/private-cloud-vlans-subnets.png) 

### <a name="available-actions"></a>Kullanılabilir eylemler

* [VLAN'lar/alt ağlar eklemek](https://docs.azure.cloudsimple.com/create-vlan-subnet/). Bir VLAN/alt bu özel buluta ekleyin.

Bir VLAN/alt ağ için aşağıdaki eylemleri seçin
* [Güvenlik Duvarı tablo ekleme](https://docs.azure.cloudsimple.com/firewall/). Bir güvenlik duvarı tablo, bu özel buluta ekleyin.
* **Düzenle**
* **Silme** (yalnızca kullanıcı VLAN'lar/alt ağlar tanımlı)

## <a name="private-cloud-activity"></a>Özel bulut etkinliği

Seçili özel bulut için aşağıdaki bilgileri görüntüleyin.  Seçili özel bulut için tüm etkinlikleri filtrelenmiş bir listesini etkinlik bilgilerdir.  Bu sayfa, en fazla 25 son etkinlikleri gösterir.

* Son uyarılar
* Son olayları
* Son kullanılan görevler
* Son denetleme

![Özel bulut - etkinlik](media/private-cloud-activity.png)

## <a name="cloud-racks"></a>Bulut rafları

Bulut raflar, özel bulut yapı taşlarıdır. Her bir rafa kapasitesine sağlar. CloudSimple bulut raflar oluştururken veya özel Bulutu genişletme, seçimlerine göre otomatik olarak yapılandırır.  Bulut raflar tam listesini görüntülemek için özel bulut dahil olmak üzere her atanır.

![Özel Bulut - Bulut rafları](media/private-cloud-cloudracks.png)

## <a name="vsphere-management-network"></a>vSphere yönetim ağı

VMware yönetim kaynakları ve özel bulutta şu anda yapılandırılmış olan sanal makinelerin listesi. Yazılım sürümü, tam etki alanı adı (FQDN) ve kaynak IP adresi bilgileri içerir.

![Özel bulut - vSphere yönetim ağı](media/private-cloud-vsphere-management-network.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* Daha fazla bilgi edinin [özel Bulutları](cloudsimple-private-cloud.md)