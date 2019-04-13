---
title: Azure'da ağ sanal Gereci sorunlarını giderme | Microsoft Docs
description: Azure'da ağ sanal Gereci sorunlarını giderme hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/26/2018
ms.author: genli
ms.openlocfilehash: b7ac96d3588923727a71cf6152ba36481ef44545
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59526665"
---
# <a name="network-virtual-appliance-issues-in-azure"></a>Azure'da ağ sanal Gereci sorunları

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir veya VPN bağlantı sorunlarını ve hataları üçüncü kullanırken, ağ sanal Gereci (NVA) Microsoft azure'da taraf VM karşılaşabilirsiniz. Bu makalede, NVA yapılandırmaları için temel Azure Platform gereksinimleri doğrulamanıza yardımcı olmak için temel adımlar sağlanmaktadır.

Üçüncü taraf nva'ları ve Azure platformuyla kendi tümleştirme için teknik destek NVA satıcısı tarafından sağlanır.

> [!NOTE]
> Bir bağlantı veya bir NVA gerektirir yönlendirme sorunu varsa, şunları yapmalısınız [satıcısını NVA'ın](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines) doğrudan.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="checklist-for-troubleshooting-with-nva-vendor"></a>NVA satıcı ile sorun giderme denetim listesi

- Yazılım güncelleştirmeleri için NVA VM'inin yazılım
- Hizmet hesabı kurulumu ve işlevleri
- Kullanıcı tanımlı yollar (Udr) üzerinde doğrudan nva'ya trafik sanal ağ alt ağları
- Udr'ler NVA gelen trafiği doğrudan sanal ağ alt ağları üzerinde
- Yönlendirme tabloları ve kuralların NVA'dan (örneğin, NIC1 NIC2 için)
- NVA NIC'leri alma ve ağ trafiğini göndermek doğrulamak için izleme
- Standart SKU ve genel IP'ler kullanırken NVA'ya yönelik yönlendirilecek bir NSG oluşturulur ve açık bir kural trafiğe izin verecek şekilde koyulmalıdır.

## <a name="basic-troubleshooting-steps"></a>Temel sorun giderme adımları

- Temel yapılandırmayı kontrol edin
- NVA performansını kontrol etme
- Gelişmiş ağ sorunlarını giderme

## <a name="check-the-minimum-configuration-requirements-for-nvas-on-azure"></a>Azure'da nva'ları için en düşük yapılandırma gereksinimlerini denetleme

Her nva'nın, Azure üzerinde doğru çalışması için temel yapılandırma gereksinimleri vardır. Aşağıdaki bölümde, bu temel yapılandırmalar doğrulamak için adımlar sağlar. Daha fazla bilgi için [satıcısını NVA'ın](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).

**IP iletimi NVA etkin olup olmadığını denetleyin**

Azure portalı kullanma

1. NVA kaynak bulun [Azure portalında](https://portal.azure.com), ağı seçin ve ardından ağ arabirimi seçin.
2. Ağ arabirimi sayfasında, IP yapılandırması seçin.
3. IP iletme etkinleştirilmiş olduğundan emin olun.

PowerShell kullanma

1. PowerShell'i açın ve Azure hesabınızda oturum açın.
2. Aşağıdaki komutu çalıştırın (köşeli parantez içindeki değerleri kendi bilgilerinizle değiştirin):

   ```powershell
   Get-AzNetworkInterface -ResourceGroupName <ResourceGroupName> -Name <NicName>
   ```

3. Denetleme **EnableIPForwarding** özelliği.
4. IP iletimi etkin değilse, bunu etkinleştirmek için aşağıdaki komutları çalıştırın:

   $nic2 = Get-AzNetworkInterface - ResourceGroupName <ResourceGroupName> -adı <NicName> $nic2. EnableIPForwarding = 1 kümesi AzNetworkInterface - Networkınterface $nic2 yürütün: $nic2 #and beklenen çıktı: EnableIPForwarding: TRUE NetworkSecurityGroup: null

**Standart SKU Pubilc IP kullanırken denetlemek için NSG** standart SKU ve genel IP'ler kullanırken olmalıdır nva'nın trafiğe izin vermek için bir NSG oluşturulur ve açık bir kural.

**Trafiği NVA yönlendirilebilir olup olmadığını denetleyin**

1. Üzerinde [Azure portalında](https://portal.azure.com)açın **Ağ İzleyicisi**seçin **sonraki atlama**.
2. NVA ve sonraki atlamayı görüntülemek, hedef IP adresi için trafiği yönlendirmek için yapılandırılmış bir sanal makine belirtin. 
3. NVA olarak listelenmemişse **sonraki atlama**denetleyin ve Azure rota tablolarını güncelleştir.

**Trafiği NVA ulaşıp ulaşamadığını denetleyin**

1. İçinde [Azure portalında](https://portal.azure.com)açın **Ağ İzleyicisi**ve ardından **IP akışı doğrulama**. 
2. VM ve NVA'ın IP adresini belirtin ve ardından trafik bir ağ güvenlik grupları (NSG) tarafından engellenmediğini denetleyin.
3. Trafiği engelleyen bir NSG kuralı varsa, NSG'de bulun **geçerli güvenlik** kurallar ve trafiğin geçmesine izin verecek şekilde güncelleştirin. Ardından çalıştırın **IP akışı doğrulama** yeniden ve **bağlantı sorunlarını giderme** , iç veya dış IP adresine TCP iletişimi VM'den test etmek için.

**NVA ve VM'ler için beklenen trafik dinleyip olup olmadığını denetleyin**

1. NVA'ya yönelik RDP veya SSH kullanarak bağlanın ve ardından aşağıdaki komutu çalıştırın:

    Windows için:

        netstat -an

    Linux için:

        netstat -an | grep -i listen
2. Sonuçlarda listelenen NVA yazılım tarafından kullanılan TCP bağlantı noktası görmüyorsanız, VM ve NVA dinlemek ve bu bağlantı noktalarını ulaştığında trafiğe yanıt için uygulamayı yapılandırmanız gerekir. [Gerektiğinde Yardım almak için NVA satıcısına başvurun](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).

## <a name="check-nva-performance"></a>NVA performansını kontrol etme

### <a name="validate-vm-cpu"></a>VM CPU doğrula

CPU kullanımı yüzde 100'e yakın alırsa, ağ paketi düşme etkileyen sorunları yaşayabilirsiniz. VM raporlarınızı, Azure portalında bir özel zaman aralığı için CPU ortalama. Bir CPU ani sırasında Konuk VM yüksek CPU neden olan işlem araştırmak ve, mümkünse azaltın. Daha büyük bir SKU boyutu için veya sanal makine ölçek kümesi için VM'yi yeniden boyutlandırın, örnek sayısını artırın veya CPU kullanımı otomatik ölçeklendirme kümesi gerekebilir. Bu sorunların ya da [NVA satıcısına başvurmanız](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines), gerektiğinde.

### <a name="validate-vm-network-statistics"></a>VM ağı istatistikleri doğrula

VM ağı kullanırsanız ani veya daha yüksek performans özelliklerinden elde etmek için VM SKU boyutunu yükseltin gerekebilir yüksek kullanım dönemleri gösterir. Hızlandırılmış ağ etkin sağlayarak, sanal Makineyi yeniden dağıtabilirsiniz. NVA hızlandırılmış ağ özelliğini destekleyip desteklemediğini doğrulamak için [NVA satıcısına başvurmanız](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines), gerektiğinde.

## <a name="advanced-network-administrator-troubleshooting"></a>Gelişmiş ağ yöneticinize sorun giderme

### <a name="capture-network-trace"></a>Ağ izleme yakalama
Kaynak VM üzerinde eşzamanlı ağ izlemesi, nva'nın ve ' % s'hedef VM, çalışırken yakalama **[PsPing](https://docs.microsoft.com/sysinternals/downloads/psping)** veya **nmap komutunu çalıştırdığınız**ve sonra izlemeyi durdurun.

1. Eşzamanlı ağ izlemesi yakalamak için aşağıdaki komutu çalıştırın:

   **Windows için**

   Netsh izi Yakalamayı Başlat = yes tracefile=c:\server_ıp.etl scenario = NetConnection

   **Linux için**

   sudo tcpdump -s0 -i eth0 -X -w vmtrace.cap

2. Kullanım **PsPing** veya **nmap komutunu çalıştırdığınız** hedef VM kaynak VM (örneğin: `PsPing 10.0.0.4:80` veya `Nmap -p 80 10.0.0.4`).
3. Ağ izleme kullanarak VM hedefinizden açın [Ağ İzleyicisi](https://www.microsoft.com/download/details.aspx?id=4865) veya tcpdump'ı. Kaynak çalıştırdığınız VM'nin IP'si için bir ekran filtresi uygulayın **PsPing** veya **nmap komutunu çalıştırdığınız** , gibi `IPv4.address==10.0.0.4 (Windows netmon)` veya `tcpdump -nn -r vmtrace.cap src or dst host 10.0.0.4` (Linux).

### <a name="analyze-traces"></a>İzlemeleri analiz edin

Arka uç VM izlemesine gelen paketleri görmüyorsanız, büyük olasılıkla bir NSG yok veya UDR Engellemesi ya da NVA yönlendirme tablolarını yanlıştır.

Gelen paketleri görüyorsanız ancak yanıt yoksa, VM uygulaması veya güvenlik duvarı sorununu çözmeniz gerekebilir. Bu sorunların ya da [gerektiğinde Yardım almak için NVA satıcısına başvurun](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).