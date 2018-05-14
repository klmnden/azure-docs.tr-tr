---
title: Azure VM'ler arasında bağlantı sorunlarını giderme | Microsoft Docs
description: Azure VM'ler arasında bağlantı sorunlarını giderme öğrenin.
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
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 6decb0e9188db00608be35d9ba4e84df92ceb671
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a>Azure VM'ler arasında bağlantı sorunlarını giderme

Azure sanal makineleri (VM'ler) arasındaki bağlantı sorunları yaşayabilirsiniz. Bu makalede, bu sorunu gidermenize yardımcı olmak için sorun giderme adımlarını sağlar. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Belirti

Bir Azure VM için başka bir Azure VM bağlanamıyor.

## <a name="troubleshooting-guidance"></a>Sorun giderme rehberi 

1. [NIC yanlış yapılandırılmış olup olmadığını denetleyin](#step-1-check-whether-nic-is-misconfigured)
2. [Ağ trafiği NSG veya UDR tarafından engellenmiş olup olmadığını denetleyin](#step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr)
3. [Ağ trafiği VM Güvenlik Duvarı tarafından engellenir olup olmadığını denetleyin](#step-3-check-whether-network-traffic-is-blocked-by-vm-firewall)
4. [VM uygulama veya hizmet bağlantı noktasını dinleyen olup olmadığını denetleyin](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [Sorun tarafından SNAT neden olup olmadığını denetleyin](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [Trafik Klasik VM için ACL'ler tarafından engellenmiş olup olmadığını denetleyin](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [Uç nokta için Klasik VM oluşturulup oluşturulmadığını denetleyin](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [Bir VM ağ paylaşımına bağlanmak deneyin](#step-8-try-to-connect-to-a-vm-network-share)
9. [Inter-Vnet bağlantısını kontrol edin.](#step-9-check-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

Sorunu gidermek için aşağıdaki adımları izleyin. Her adımı tamamladıktan sonra sorunun giderilmiş olup olmadığını denetleyin. 

### <a name="step-1-check-whether-nic-is-misconfigured"></a>1. adım: NIC yanlış yapılandırılmış olup olmadığını denetleyin

Adımları [ağ arabirimi için Azure Windows VM sıfırlamaya](../virtual-machines/windows/reset-network-interface.md). 

Ağ arabirimi (NIC) değiştirdikten sonra sorun oluşursa, şu adımları izleyin:

**Multi-NIC VM**

1. Bir NIC ekleme
2. Hatalı NIC sorunları gidermek veya hatalı NIC kaldırma  Ardından NIC tekrar ekleyin.

Daha fazla bilgi için bkz: [ağ arabirimlerine ekleyip sanal makinelerden](virtual-network-network-interface-vm.md).

**Tek NIC VM** 

- [Windows VM yeniden dağıtın](../virtual-machines/windows/redeploy-to-new-node.md)
- [Linux VM yeniden dağıtın](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-whether-network-traffic-is-blocked-by-nsg-or-udr"></a>2. adım: ağ trafiği NSG veya UDR tarafından engellenmiş olup olmadığını denetleyin

Kullanım [Ağ İzleyicisi IP akış doğrulayın](../network-watcher/network-watcher-ip-flow-verify-overview.md) ve [NSG akış günlüğü](../network-watcher/network-watcher-nsg-flow-logging-overview.md) bir ağ güvenlik grubu (NSG) veya kullanıcı tanımlı yol (trafik akışı ile engel UDR) olup olmadığını belirlemek için.

### <a name="step-3-check-whether-network-traffic-is-blocked-by-vm-firewall"></a>3. adım: ağ trafiği VM Güvenlik Duvarı tarafından engellenir olup olmadığını denetleyin

Güvenlik Duvarı'nı devre dışı bırakın ve ardından test sonucu. Sorunun giderilip giderilmediğini güvenlik duvarı ayarlarını doğrulayın ve ardından Güvenlik Duvarı'nı yeniden etkinleştirin.

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-the-port"></a>4. adım: VM uygulama veya hizmet bağlantı noktasını dinleyen olup olmadığını denetleyin

VM uygulama veya hizmet bağlantı noktasını dinleyen olup olmadığını denetlemek için aşağıdaki yöntemlerden birini kullanabilirsiniz.

- Sunucu bu bağlantı noktasında dinleme olup olmadığını denetlemek için aşağıdaki komutları çalıştırın.

**Windows VM**

    netstat –ano

**Linux VM**

    netstat -l

- Çalıştırma **telnet** sanal makineye bağlantı noktası test kendisine komutu. Sınama başarısız olursa, bu bağlantı noktasında dinleyecek şekilde uygulaması veya hizmeti yapılandırılmamış.

### <a name="step-5-check-whether-the-problem-is-caused-by-snat"></a>5. adım: SNAT tarafından soruna neden olup olmadığını denetleyin

Bazı senaryolarda, kaynakların Azure dışında bir bağımlılığa sahip bir yük dengeleme çözümü arkasındaki VM yerleştirilir. Aralıklı bağlantısı sorunlar yaşıyorsanız bu senaryolarda, sorunun nedeni olabilir [SNAT bağlantı noktası Tükenme](../load-balancer/load-balancer-outbound-connections.md). Sorunu çözmek için yük dengeleyicinin arkasındaki ve NSG veya ACL ile güvenli her VM için bir VIP (veya Klasik ILPIP) oluşturun. 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm"></a>6. adım: trafiği Klasik VM için ACL'ler tarafından engellenmiş olup olmadığını denetleyin

Erişim denetimi listesi (ACL) seçmeli olarak izin veren veya trafiği bir sanal makine uç noktası için reddetme olanağı sağlar. Daha fazla bilgi için bkz: [bir uç nokta ACL'sini Yönet](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

### <a name="step-7-check-whether-the-endpoint-is-created-for-the-classic-vm"></a>7. adım: Klasik VM için uç nokta oluşturulup oluşturulmadığını denetleyin

Klasik dağıtım modelini kullanarak Azure içinde oluşturduğunuz tüm sanal makineleri otomatik olarak bir özel ağ kanalı diğer sanal makinelerle aynı bulut hizmetinde veya sanal ağ üzerinden iletişim kurabilir. Ancak, diğer sanal ağlardaki bilgisayarlara bir sanal makineye gelen ağ trafiğini yönlendirmek için uç noktalar gerektirir. Daha fazla bilgi için bkz: [uç noktaları nasıl kurulur](../virtual-machines/windows/classic/setup-endpoints.md).

### <a name="step-8-try-to-connect-to-a-vm-network-share"></a>8. adım: bir VM ağ paylaşımına bağlanmak deneyin

VM ağ paylaşımına bağlanamıyorsanız, sorun VM'deki kullanılamaz NIC'ler nedeni olabilir. Kullanılamayan NICs silmek için bkz: [kullanılamaz NIC'ler silme](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)

### <a name="step-9-check-inter-vnet-connectivity"></a>9. adım: Onay Inter-Vnet bağlantısı

Kullanım [Ağ İzleyicisi IP akış doğrulayın](../network-watcher/network-watcher-ip-flow-verify-overview.md) ve [NSG akış günlüğü](../network-watcher/network-watcher-nsg-flow-logging-overview.md) bir NSG veya trafik akışı ile engellemesini UDR olup olmadığını belirlemek için. Ayrıca Inter-Vnet yapılandırmasını doğrulayın [burada](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).

### <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.
Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.