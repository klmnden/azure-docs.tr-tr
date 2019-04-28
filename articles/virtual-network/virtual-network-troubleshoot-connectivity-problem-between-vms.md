---
title: Azure sanal makineler arasında bağlantı sorunlarını giderme | Microsoft Docs
description: Azure sanal makineler arasında bağlantı sorunlarını giderme hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: fc3d6ab1d7fdf05963d9ecd350deccd940a95b87
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036409"
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a>Azure sanal makineler arasında bağlantı sorunlarını giderme

Azure sanal makineleri (VM'ler) arasında bağlantı sorunları yaşayabilirsiniz. Bu makalede, bu sorunu gidermenize yardımcı olmak için sorun giderme adımları sağlar. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Belirti

Bir Azure VM'yi başka bir Azure VM'sine bağlanılamıyor.

## <a name="troubleshooting-guidance"></a>Sorun giderme rehberi 

1. [NIC yanlış yapılandırılıp yapılandırılmadığını denetleyin](#step-1-check-whether-nic-is-misconfigured)
2. [NSG veya UDR ağ trafiğini bloke olup olmadığını denetleyin](#step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr)
3. [Ağ trafiği VM Güvenlik Duvarı tarafından engellenmediğini denetleyin](#step-3-check-whether-network-traffic-is-blocked-by-vm-firewall)
4. [VM uygulaması veya hizmeti bağlantı noktası üzerinde dinleyip olup olmadığını denetleyin](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [Sorun tarafından SNAT sebep olup olmadığını denetleyin](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [Klasik sanal makine için trafiği ACL'ler tarafından engellenmediğini denetleyin](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [Uç nokta için Klasik sanal makine oluşturulup oluşturulmadığını denetleyin](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [Bir VM ağ paylaşımına bağlanmak deneyin](#step-8-try-to-connect-to-a-vm-network-share)
9. [Inter-Vnet bağlantısını kontrol edin](#step-9-check-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

Sorunu gidermek için aşağıdaki adımları izleyin. Her adımı tamamladıktan sonra sorun çözülmüş olup olmadığını denetleyin. 

### <a name="step-1-check-whether-nic-is-misconfigured"></a>1. Adım: NIC yanlış yapılandırılıp yapılandırılmadığını denetleyin

Bağlantısındaki [Azure Windows VM için ağ arabirimi sıfırlama](../virtual-machines/windows/reset-network-interface.md). 

Ağ arabirimi (NIC) değiştirdikten sonra bir sorun oluşursa, aşağıdaki adımları izleyin:

**Multi-NIC VM**

1. Bir NIC ekleme
2. Hatalı NIC, sorunları gidermek veya hatalı NIC Kaldır  Daha sonra yeniden NIC ekleyin.

Daha fazla bilgi için [ağ arabirimlerine ekleyip sanal makinelerden](virtual-network-network-interface-vm.md).

**Tek NIC'li VM** 

- [Windows VM yeniden dağıtma](../virtual-machines/windows/redeploy-to-new-node.md)
- [Linux VM'yi yeniden dağıtma](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr"></a>2. Adım: NSG veya UDR ağ trafiğini bloke olup olmadığını denetleyin

Kullanım [Ağ İzleyicisi IP akışı doğrulama](../network-watcher/network-watcher-ip-flow-verify-overview.md) ve [NSG akış günlüğü](../network-watcher/network-watcher-nsg-flow-logging-overview.md) bir ağ güvenlik grubu (NSG) veya kullanıcı tanımlı yol (Bu trafik akışı ile başka hiçbir şekilde müdahale UDR) olup olmadığını belirlemek için.

### <a name="step-3-check-whether-network-traffic-is-blocked-by-vm-firewall"></a>3. Adım: Ağ trafiği VM Güvenlik Duvarı tarafından engellenmediğini denetleyin

Güvenlik Duvarı'nı devre dışı bırakın ve ardından test sonucu. Sorunun giderilip giderilmediğini Güvenlik Duvarı ayarları doğrulayın ve sonra Güvenlik Duvarı'nı yeniden etkinleştirin.

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-the-port"></a>4. Adım: VM uygulaması veya hizmeti bağlantı noktası üzerinde dinleyip olup olmadığını denetleyin

VM uygulaması veya hizmeti bağlantı noktasını dinlediğini olup olmadığını denetlemek için aşağıdaki yöntemlerden birini kullanabilirsiniz.

- Sunucu, bağlantı noktasını dinlediğini olup olmadığını denetlemek için aşağıdaki komutları çalıştırın.

**Windows VM**

    netstat –ano

**Linux VM**

    netstat -l

- Çalıştırma **telnet** sanal makinesinde bağlantı testi kendisine komutu. Test başarısız olursa, bu bağlantı noktasında dinleyecek şekilde uygulamaya veya hizmete yapılandırılmamış.

### <a name="step-5-check-whether-the-problem-is-caused-by-snat"></a>5. Adım: Sorun tarafından SNAT sebep olup olmadığını denetleyin

Bazı senaryolarda Azure dışındaki kaynaklarda bir bağımlılığa sahip bir yük dengeleme çözümü arkasındaki VM yerleştirilir. Aralıklı bağlantı sorunları yaşıyorsanız bu senaryolarda, sorunun nedeni olabilir [SNAT bağlantı noktası tükenmesi](../load-balancer/load-balancer-outbound-connections.md). Sorunu çözmek için yük dengeleyicinin arkasında ve NSG veya ACL ile güvenli her VM için bir VIP (veya Klasik için ILPIP) oluşturun. 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm"></a>6. Adım: Klasik sanal makine için trafiği ACL'ler tarafından engellenmediğini denetleyin

Erişim denetimi listesi (ACL) seçmeli olarak erişime izin verebilir veya trafiği bir sanal makine uç noktası için olanağı sağlar. Daha fazla bilgi için [bir uç nokta ACL'sini Yönet](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#manage-the-acl-on-an-endpoint).

### <a name="step-7-check-whether-the-endpoint-is-created-for-the-classic-vm"></a>7. Adım: Uç nokta için Klasik sanal makine oluşturulup oluşturulmadığını denetleyin

Klasik dağıtım modelini kullanarak Azure'da oluşturduğunuz tüm VM'ler otomatik olarak özel bir ağ kanalı diğer sanal makinelerle aynı bulut hizmetinde veya sanal ağ üzerinden iletişim kurabilirsiniz. Ancak, diğer sanal ağlardaki bilgisayarlara gelen ağ trafiğini bir sanal makineye yönlendirmek için uç noktalar gerektirir. Daha fazla bilgi için [uç noktaları ayarlama işlemini](../virtual-machines/windows/classic/setup-endpoints.md).

### <a name="step-8-try-to-connect-to-a-vm-network-share"></a>8. adım: Bir VM ağ paylaşımına bağlanmak deneyin

Bir VM ağ paylaşımına bağlanamıyorsanız, sorunu kullanılamaz NIC VM tarafından kaynaklanabilir. Kullanılamayan NIC silmek için bkz [kullanılamaz NIC silme](../virtual-machines/troubleshooting/reset-network-interface.md#delete-the-unavailable-nics)

### <a name="step-9-check-inter-vnet-connectivity"></a>9. adım: Inter-Vnet bağlantısını kontrol edin

Kullanım [Ağ İzleyicisi IP akışı doğrulama](../network-watcher/network-watcher-ip-flow-verify-overview.md) ve [NSG akış günlüğü](../network-watcher/network-watcher-nsg-flow-logging-overview.md) NSG veya trafik akışı ile başka hiçbir şekilde müdahale UDR olup olmadığını belirlemek için. Inter-Vnet yapılandırmanızı da doğrulayabilirsiniz [burada](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).

### <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.
Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.