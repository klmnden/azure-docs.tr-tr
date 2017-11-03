---
title: "Bir Microsoft Azure sanal ağı için VPN verimlilik doğrulama | Microsoft Docs"
description: "Bu belgenin amacı, bir kullanıcı, şirket içi kaynakları ağ akışından bir Azure sanal makinesi için doğrulama yardımcı olmaktır."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: jasmc
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2017
ms.author: radwiv;chadmat;genli
ms.openlocfilehash: 3a1a6e2acd2ff40c2b35a6099f8a9fc7eb104bbc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-validate-vpn-throughput-to-a-virtual-network"></a>Nasıl bir sanal ağ VPN verimlilik doğrulamak için

Bir VPN gateway bağlantısı güvenli kurmanızı sağlar, sanal ağınızı Azure içinde ve şirket içi arasında bağlantı şirket içi BT altyapısı.

Bu makalede, bir Azure sanal makinesini (VM) için ağ verimini şirket içi kaynaklardan doğrulamak gösterilmiştir. Ayrıca, sorun giderme kılavuzu sağlar.

>[!NOTE]
>Bu makale, tanılamanıza ve sık karşılaşılan sorunları gidermenize yardımcı olmak için tasarlanmıştır. Aşağıdaki bilgileri kullanarak sorunu çözmek yapamıyorsanız [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
>
>

## <a name="overview"></a>Genel Bakış

VPN ağ geçidi bağlantısı aşağıdaki bileşenleri içerir:

- Şirket içi VPN cihazı (bir listesini görüntülemek [doğrulanan VPN cihazları)](vpn-gateway-about-vpn-devices.md#devicetable).
- Genel Internet
- Azure VPN ağ geçidi
- Azure VM

Aşağıdaki diyagramda bir şirket içi ağ VPN aracılığıyla Azure sanal ağını mantıksal bağlantısını gösterir.

![Mantıksal bağlantı, müşteri ağa MSFT VPN kullanarak ağ](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-the-maximum-expected-ingressegress"></a>En fazla beklenen giriş/çıkış Hesapla

1.  Uygulamanızın temel üretilen iş gereksinimlerini belirleyin.
2.  Azure VPN ağ geçidi işleme sınırları belirleyin. Yardım için "SKU ve VPN türüne göre toplam verimliliği" bölümüne bakın [planlama ve tasarım VPN ağ geçidi için](vpn-gateway-plan-design.md).
3.  Belirlemek [Azure VM verimlilik Kılavuzu](../virtual-machines/virtual-machines-windows-sizes.md) VM boyutu.
4.  Internet servis sağlayıcısı (ISS) bant genişliğiniz belirler.
5.  Beklenen Verimlilik - en az bant genişliği (VM, ağ geçidi, ISS) hesaplamak * 0,8.

Hesaplanan verimlilik, uygulamanızın temel üretilen iş gereksinimlerini karşılamıyorsa, bant genişliği performans sorunu tanımlanan kaynağın artırmak gerekir. Bir Azure VPN ağ geçidi yeniden boyutlandırmak için bkz: [bir ağ geçidi SKU'su değiştirme](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku). Bir sanal makine yeniden boyutlandırmak için bkz: [bir VM'yi yeniden boyutlandırın](../virtual-machines/virtual-machines-windows-resize-vm.md). Beklenen Internet bant genişliği yaşıyorsanız değil, ISS'ye başvurun isteyebilirsiniz.

## <a name="validate-network-throughput-by-using-performance-tools"></a>Performans araçları kullanarak ağ verimliliği doğrulama

Test sırasında VPN tüneli verimlilik Doygunluk doğru sonuçlar vermez gibi bu doğrulama yoğun olmayan saatlerde gerçekleştirilmesi gerekir.

Bu test için kullandığımız hem Windows hem de Linux üzerinde çalışır ve hem istemci hem de sunucu modları içeren iPerf aracıdır. 3 GB/sn için Windows VM için sınırlıdır.

Bu araç, disk okuma/yazma işlemleri gerçekleştirmez. Bu yalnızca bir ucu kendinden oluşturulmuş TCP trafiği diğer üretir. İstemci ve sunucu düğümler arasında kullanılabilir bant genişliğini ölçer deneme bağlı olarak istatistikleri oluşturulur. İki düğüm arasında test edilirken bir sunucunun ve diğer istemci olarak görür. Bu test tamamlandıktan sonra her iki karşıya yükleme test ve her iki düğüm üzerinde üretilen işi indirmek için rolleri ters öneririz.

### <a name="download-iperf"></a>İPerf indirin
Karşıdan [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip). Ayrıntılar için bkz [iPerf belgelerine](https://iperf.fr/iperf-doc.php).

 >[!NOTE]
 >Bu makalede ele alınan üçüncü taraf ürünler Microsoft'tan bağımsız şirketler tarafından üretilmektedir. Microsoft, zımni ya da aksi takdirde performans veya bu ürünlerin hakkında hiçbir garanti vermez.
 >
 >

### <a name="run-iperf-iperf3exe"></a>Çalışma iPerf (iperf3.exe)
1. (Azure VM sınama ortak IP adresi için) trafiğe izin bir NSG/ACL kuralını etkinleştirin.

2. Her iki düğüm üzerinde 5001 bağlantı noktası için güvenlik duvarı özel durumunu etkinleştirin.

    **Windows:** yönetici olarak aşağıdaki komutu çalıştırın:

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    Test ederken kuralı kaldırmak için bu komutu çalıştırmak, tamamlanır:

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
    </br>
    **Azure Linux:** Azure Linux görüntüleri olan izin veren güvenlik duvarları. Bir bağlantı noktasını dinleyen bir uygulama olduğunda trafiği aracılığıyla izin verilir. Güvenli hale getirilen özel resimler açıkça açılan bağlantı noktaları gerekebilir. Ortak Linux işletim sistemi katmanlı güvenlik duvarları içeren `iptables`, `ufw`, veya `firewalld`.

3. Sunucu düğümünde iperf3.exe ayıkladığınız dizine geçin. Ardından iPerf sunucu modunda çalıştırın ve aşağıdaki komutları olarak 5001 bağlantı noktasında dinleyecek şekilde ayarlayın:

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. İstemci düğümde burada iperf aracı ayıkladığınız ve aşağıdaki komutu çalıştırın dizinine değiştirin:

    ```CMD
    iperf3.exe -c <IP of the iperf Server> -t 30 -p 5001 -P 32
    ```

    İstemci sunucuya 5001 numaralı bağlantı noktasında trafik 30 saniye inducing. Bayrağı '-P ' belirten 32 eşzamanlı bağlantıların sunucu düğümünü kullanarak biz.

    Aşağıdaki ekran bu örneğin çıktısını gösterir:

    ![Çıktı](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. (İSTEĞE BAĞLI) Test sonuçlarını korumak için bu komutu çalıştırın:

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. Sunucu düğümü şimdi istemci ve tam tersini olmayacaktır önceki adımları tamamladıktan sonra tersine, rolleri ile aynı adımları yürütün.

## <a name="address-slow-file-copy-issues"></a>Yavaş dosya kopyalama sorunlarını gidermek
Windows Gezgini'ni kullanarak veya sürükleyip bir RDP oturumu aracılığıyla zaman kopyalamayı yavaş dosya karşılaşabilirsiniz. Bu sorun, normalde birini veya her ikisini aşağıdaki etmenlere nedeniyle oluşur:

- Dosya kopyalama uygulamaları, Windows Gezgini ve RDP, gibi birden çok iş parçacığı dosya kopyalarken kullanmayın. Çok iş parçacıklı dosya kopyalama uygulaması gibi daha iyi performans için kullandığınız [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) 16 veya 32 iş parçacığı kullanarak dosyaları kopyalamak için. Dosya kopyalama Richcopy içindeki iş parçacığı sayısını değiştirmek için tıklatın **eylem** > **kopyalama seçenekleri** > **dosya kopyalama**.<br><br>
![Yavaş dosya kopyalama sorunları](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)<br>
- Yetersiz VM disk okuma/yazma hızı. Daha fazla bilgi için bkz: [Azure Storage sorunlarını giderme](../storage/common/storage-e2e-troubleshooting.md).

## <a name="on-premises-device-external-facing-interface"></a>Şirket içi cihaz dış karşılıklı arabirimi
Şirket içi VPN cihazı Internet'e yönelik IP adresini de dahil edilmişse [yerel ağ](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) Azure tanımında durumlarıyla VPN bağlantısını keser yukarı getirmek için bağlanamama ya da performans sorunları yaşayabilir.

## <a name="checking-latency"></a>Gecikme olmadığı denetleniyor
Microsoft Azure sınır cihazı izlemek için tracert atlama arasında 100 ms aşan gecikmeleri olup olmadığını belirlemek için kullanın.

Şirket içi ağ üzerinden çalıştırılabilecek *tracert* Azure ağ geçidi veya VM VIP için. Yalnızca gördüğünüzde * döndürdü, Azure kenar ulaştı, biliyor. "Döndürülen MSN" içeren DNS adları gördüğünüzde Microsoft omurga ulaştınız bilirsiniz.<br><br>
![Gecikme olmadığı denetleniyor](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi veya Yardım için aşağıdaki bağlantıları kontrol edin:

- [Azure sanal makineleri için ağ verimliliğini en iyi duruma getirme](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
