---
title: Microsoft Azure sanal ağa VPN aktarım hızını doğrulama | Microsoft Docs
description: Bu belgenin amacı, bir kullanıcının kendi şirket içi kaynaklardan gelen ağ aktarım hızını bir Azure sanal makinesi için doğrulama yardımcı olmaktır.
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: jasmc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2018
ms.author: radwiv;chadmat;genli
ms.openlocfilehash: 819415712d8e605825957aa602fc99dcf6902d82
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60457569"
---
# <a name="how-to-validate-vpn-throughput-to-a-virtual-network"></a>Bir sanal ağa yönelik VPN aktarım hızını doğrulama

Bir VPN ağ geçidi bağlantısı, güvenli bağlantı kurmanıza olanak sağlar, Azure içindeki sanal ağınız ile şirket içi arasında bağlantı şirket içi BT altyapısı.

Bu makalede, Azure sanal makine'de (VM) şirket içi kaynaklardan gelen ağ aktarım hızını doğrulama gösterilmektedir. Ayrıca, sorun giderme kılavuzu verilmiştir.

>[!NOTE]
>Bu makale ortak sorunları tanılayın ve giderin yardımcı olmak için hazırlanmıştır. Aşağıdaki bilgileri kullanarak sorunu çözmek zamanınız yoksa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
>
>

## <a name="overview"></a>Genel Bakış

VPN ağ geçidi bağlantısı, aşağıdaki bileşenleri içerir:

- Şirket içi VPN cihazı (bir listesini görüntülemek [doğrulanmış VPN cihazları)](vpn-gateway-about-vpn-devices.md#devicetable).
- Genel Internet
- Azure VPN ağ geçidi
- Azure VM

Aşağıdaki diyagramda bir Azure sanal ağa VPN aracılığıyla şirket ağının mantıksal bağlantı gösterir.

![Mantıksal bağlantı, müşteri ağı MSFT VPN kullanarak ağa](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-the-maximum-expected-ingressegress"></a>En fazla beklenen giriş/çıkış hesaplayın

1.  Uygulamanızın temel aktarım hızı gereksinimlerini belirleyin.
2.  Azure VPN ağ geçidi aktarım hızı limitlerinizi belirleyin. Yardım için "Ağ geçidi SKU'ları" bölümüne bakın. [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md#gwsku).
3.  Belirlemek [Azure VM işleme kılavuzuna](../virtual-machines/virtual-machines-windows-sizes.md) , VM boyutu için.
4.  Internet servis sağlayıcınıza (ISS) bant genişliğiniz belirler.
5.  Beklenen aktarım hızıyla - en az bant genişliği (ağ geçidi VM ISS) hesaplamak * 0.8.

Hesaplanan aktarım hızınızı uygulamanızın temel aktarım hızı gereksinimlerini karşılamıyorsa, performans sorunu tanımlanan kaynak bant genişliği artırmam gerekiyor. Bir Azure VPN ağ geçidini yeniden boyutlandırmak için bkz: [ağ geçidi SKU'sunu değiştirme](vpn-gateway-about-vpn-gateway-settings.md#gwsku). Bir sanal makineyi yeniden boyutlandırmak için bkz: [VM'yi yeniden boyutlandırma](../virtual-machines/virtual-machines-windows-resize-vm.md). Beklenen Internet bant genişliği yaşamadığınızdan, ISS'nize başvurun isteyebilirsiniz.

## <a name="validate-network-throughput-by-using-performance-tools"></a>Performans araçları kullanarak ağ aktarım hızını doğrulama

VPN tüneli aktarım hızı doygunluğu test sırasında doğru sonuç vermez gibi yoğun olmayan saatlerde bu doğrulama gerçekleştirilmesi gerekir.

Bu test için kullanırız hem Windows hem de Linux üzerinde çalışır ve hem istemci hem de sunucu modları olan iPerf aracıdır. 3 GB/sn için Windows Vm'leri için sınırlıdır.

Bu araç, disk için okuma/yazma işlemleri gerçekleştirmez. Yalnızca, diğer bir uçtan diğerine kendinden oluşturulmuş TCP trafiği oluşturur. Bu, istemci ve sunucu düğümler arasında kullanılabilir bant genişliğini ölçer deneme temel istatistikleri oluşturulur. İki düğüm arasında test ederken bir sunucu ve diğer istemci olarak görür. Bu test tamamlandıktan sonra her iki yükleme testi ve aktarım hızı her iki düğümünde indirmek için rolleri ters öneririz.

### <a name="download-iperf"></a>İPerf indirin
İndirme [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip). Ayrıntılar için bkz [iPerf belgeleri](https://iperf.fr/iperf-doc.php).

 >[!NOTE]
 >Bu makalede ele üçüncü taraf ürünler Microsoft'tan bağımsız şirketler tarafından üretilmektedir. Microsoft, zımni ya da aksi takdirde bu ürünlerin ve performans hakkında hiçbir garanti vermez.
 >
 >

### <a name="run-iperf-iperf3exe"></a>İPerf (iperf3.exe) çalıştırın
1. (Azure sanal makinesinde test genel IP adresi için) trafiğe izin veren bir NSG/ACL kuralı etkinleştirin.

2. Her iki düğümde 5001 bağlantı noktası için bir güvenlik duvarı özel durumu etkinleştirin.

    **Windows:** Yönetici olarak aşağıdaki komutu çalıştırın:

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    Test ederken kuralı kaldırmak için şu komutu çalıştırın, tamamlanır:

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
     
    **Azure Linux:**  Azure Linux görüntüleri esnek güvenlik duvarları vardır. Bir bağlantı noktasını dinleyen bir uygulamanın varsa, trafiğe aracılığıyla izin verilir. Güvenli, özel görüntüleri açıkça açılan bağlantı noktaları gerekebilir. Yaygın Linux işletim sistemi katmanlı güvenlik duvarları içeren `iptables`, `ufw`, veya `firewalld`.

3. Sunucu düğümünde iperf3.exe ayıkladığınız dizine geçin. Ardından iPerf sunucu modunda çalıştırın ve aşağıdaki komutları olarak 5001 bağlantı noktasında dinleyecek şekilde ayarlayın:

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. İstemci düğümde burada iperf aracı ayıklanır ve ardından aşağıdaki komutu çalıştırın dizinine değiştirin:

    ```CMD
    iperf3.exe -c <IP of the iperf Server> -t 30 -p 5001 -P 32
    ```

    İstemci seçeneği, 30 saniye sunucuya 5001 numaralı bağlantı noktasında trafiği inducing. Bayrak '-P ' bildiren 32 eşzamanlı bağlantı sunucusu düğümüne kullanıyoruz.

    Aşağıdaki ekranda, bu örnekteki Çıkışta gösterir:

    ![Çıktı](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. (İSTEĞE BAĞLI) Test sonuçlarını korumak için şu komutu çalıştırın:

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. Sunucu düğümünü artık istemciyi ve tersi olacak böylece önceki adımları tamamladıktan sonra tersine, rolleri ile aynı adımları yürütün.

## <a name="address-slow-file-copy-issues"></a>Dosya kopyalama sorunlarını gidermek
Windows Gezgini'ni kullanarak veya sürükleyip bir RDP oturumu zaman artıştan yavaş dosya karşılaşabilirsiniz. Bu sorun genellikle birini veya ikisini de aşağıdaki faktörler nedeniyle oluşur:

- Dosya kopyalama uygulamaları, Windows Gezgini ve RDP gibi birden çok iş parçacığı dosyaları kopyalarken kullanmayın. Daha iyi performans için çok iş parçacıklı dosya kopyalama uygulaması gibi kullanın [Richcopy](https://technet.microsoft.com/magazine/2009.04.utilityspotlight.aspx) 16 veya 32 iş parçacığı kullanarak dosyaları kopyalamak için. Dosya kopyalama Richcopy içinde için iş parçacığı sayısını değiştirmek için tıklayın **eylem** > **kopyalama seçenekleri** > **dosya kopyalama**.<br><br>
![Yavaş dosya kopyalama sorunları](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)<br>
- Yetersiz VM disk okuma/yazma hızı. Daha fazla bilgi için [Azure depolama sorunlarını giderme](../storage/common/storage-e2e-troubleshooting.md).

## <a name="on-premises-device-external-facing-interface"></a>Şirket içi cihaz dış karşılıklı arabirimi
Şirket içi VPN cihazının Internet'e yönelik IP adresi de dahil edilmişse [yerel ağ](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) azure'da tanımı, ara sıra VPN bağlantısını keser getirir bağlanamama ya da performans sorunları karşılaşabilirsiniz.

## <a name="checking-latency"></a>Gecikme süresi denetleniyor
Microsoft Azure Edge cihazına izleme tracert herhangi gecikmeleri 100 ms atlama arasında aşan olup olmadığını belirlemek için kullanın.

Şirket içi ağdan çalışacak *tracert* VIP Azure ağ geçidi veya VM için. Yalnızca gördüğünüzde * döndürülen, Azure edge döşemedeki biliyor. "Döndürülen MSN" içeren DNS adları gördüğünüzde, Microsoft omurga ulaştınız bildirin.<br><br>
![Gecikme süresi denetleniyor](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi veya Yardım için aşağıdaki bağlantıları kontrol edin:

- [Azure sanal makineleri için ağ aktarım hızını iyileştirme](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [Microsoft desteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
