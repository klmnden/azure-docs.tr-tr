---
title: "Azure Site Recovery için Hyper-V kapasite planlayıcısı aracı | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery için Hyper-V kapasite Planlayıcısı aracını çalıştırmak açıklar"
services: site-recovery
documentationcenter: na
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 2bc3832f-4d6e-458d-bf0c-f00567200ca0
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: nisoneji
ms.openlocfilehash: db790f9dc56605b5b752e7ab797903e32b2fc675
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="hyper-v-capacity-planner-tool-for-site-recovery"></a>Site Recovery için Hyper-V kapasite planlayıcısı aracı

Azure Site Recovery dağıtımınızın bir parçası, çoğaltma ve bant genişliği gereksinimlerini çıkış şekil gerekir. Site Recovery için Hyper-V kapasite planlayıcısı aracı, bunun için Hyper-V sanal makine çoğaltma için yardımcı olur.

Bu makale, Hyper-V kapasite Planlayıcısı aracını çalıştırmak açıklamaktadır. Bu araç, bilgileri ile birlikte kullanılması gerektiğini [kapasite planlaması için Site Recovery](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Başlamadan önce
Birincil bir Hyper-V sunucusunda veya küme düğümünde aracı çalıştırın. Aracı çalıştırmak için Hyper-V ana bilgisayar sunucuları gerekir:

* İşletim sistemi: Windows Server 2012 veya 2012 R2
* Bellek: 20 MB (en az)
* CPU: yüzde 5'i yükünü (en az)
* Disk alanı: 5 MB (en az)

Aracı çalıştırılmadan önce birincil site hazırlamanız gerekir. Arasında çoğaltıyorsanız iki şirket içi sitelere ve bant genişliğini denetlemek istiyorsanız, aynı zamanda bir çoğaltma sunucusu hazırlamanız gerekir.

## <a name="step-1-prepare-the-primary-site"></a>Adım 1: birincil site hazırlama

1. Birincil sitede çoğaltmak istediğiniz Hyper-V VM'ler ve üzerinde bulunduğu oldukları Hyper-V konakları/kümeleri tümünün listesini hazırlayın. Aracı birden çok tek başına ana bilgisayarlar için veya tek bir küme, ancak ikisini birlikte için çalıştırabilirsiniz. Ayrıca aşağıdaki gibi Hyper-V sunucuları hakkında bilgi toplamanız gereken şekilde her işletim sistemi için ayrı olarak çalıştırmanız gerekir:

   * Windows Server 2012 tek başına sunucular
   * Windows Server 2012 kümeleri
   * Windows Server 2012 R2 tek başına sunucular
   * Windows Server 2012 R2 kümeleri
2. Tüm Hyper-V konakları ve kümeleri WMI uzaktan erişimi etkinleştirin. Her sunucu/güvenlik duvarı kuralları ve kullanıcı izinleri ayarlandığından emin olmak için küme üzerinde bu komutu çalıştırın:

        netsh firewall set service RemoteAdmin enable
3. Aşağıdaki gibi sunucuları ve kümeleri, performans izlemeyi etkinleştir:

   * İle Windows Güvenlik Duvarı'nı açın **Gelişmiş Güvenlik** ek bileşeni ve ardından etkinleştir aşağıdaki gelen kuralları: **COM + Ağ erişimi (DCOM-ın)** ve tüm kuralları **Uzak Olay Günlüğü Yönetimi grubu**.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>2. adım: çoğaltma sunucusunu (şirket içi çoğaltma) hazırlama
Azure'a çoğaltma yapıyorsanız bu yapmanız gerekmez.

Böylece bir kukla VM bant genişliğini denetlemek için çoğaltılabilir tek bir Hyper-V ana bilgisayar kurtarma sunucusu olarak ayarlamanızı öneririz.  Bu atlayabilirsiniz, ancak bunu yapmanız sürece bant genişliği ölçmek açamazsınız.

1. Çoğaltmayı Hyper-V çoğaltma aracısı yapılandırırken bir küme düğümünü kullanmak istiyorsanız:

   * İçinde **Sunucu Yöneticisi'ni**, açık **yük devretme kümesi Yöneticisi**.
   * Kümeye bağlanın, küme adı vurgulayıp **Eylemler** > **rolü Yapılandır** Yüksek Kullanılabilirlik Sihirbazı'nı açın.
   * İçinde **rolü Seç**, tıklatın **Hyper-V çoğaltma aracısı**. Sihirbazda sağlayan bir **NetBIOS adı** ve **IP adresi** (bir istemci erişim noktası adı verilir) kümeye bağlantı noktası olarak kullanılacak. **Hyper-V çoğaltma aracısı** dikkat etmeniz gerekir, bir istemci erişim noktası adı elde yapılandırılır.
   * Hyper-V çoğaltma aracısı rolünün başarılı bir şekilde çevrimiçi olduğunu ve kümenin tüm düğümleri arasında yük devredebildiğini doğrulayın. Bunu yapmak için role sağ tıklayın, fareyle **taşıma**ve ardından **düğüm Seç**. Bir düğüm seçin > **Tamam**.
   * Sertifika tabanlı kimlik doğrulaması kullanıyorsanız, her bir küme düğümünün ve istemci erişim noktası tüm sertifikasının yüklü emin olun.
2. Bir çoğaltma sunucusu etkinleştirin:

   * Bir küme için hata Küme Yöneticisi'ni açın, kümeye bağlanın ve tıklatın **rolleri** > select rol > **çoğaltma ayarları** > **Bu kümeyi çoğaltma sunucusu olarak etkinleştir**. Çoğaltma olarak bir küme kullanıyorsanız da birincil sitedeki küme üzerinde Hyper-V çoğaltma aracısı rolünün mevcut olması gerekir.
   * Tek başına bir sunucu için Hyper-V Yöneticisi'ni açın. İçinde **Eylemler** bölmesinde tıklatın **Hyper-V ayarları** ve etkinleştirmek istediğiniz sunucu için **çoğaltma yapılandırması** tıklatın **bu bilgisayarı çoğaltma sunucusu olarak etkinleştir**.
3. Kimlik doğrulaması ayarlayın:

   * İçinde **kimlik doğrulaması ve bağlantı noktaları**, birincil sunucu ve kimlik doğrulama kimliğini doğrulamak için bağlantı noktalarını yöntemini seçin. Bir sertifika tıklatın kullanırsanız **Sertifika Seç** birini seçmek için. Birincil ve kurtarma Hyper-V konakları aynı etki alanında veya güvenilen etki alanlarında Kerberos kullanın. Farklı etki alanları için veya bir çalışma grubu dağıtımı için sertifikaları kullanın.
   * İçinde **yetkilendirme ve depolama**, izin **herhangi** çoğaltma verilerini Bu çoğaltma sunucusuna göndermek için (birincil) sunucu kimlik doğrulaması.

     ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)
   * Çalıştırma **netsh http show servicestate**, belirttiğiniz protokolü/bağlantı için dinleyicisinin çalıştığını denetlemek için:  
4. Güvenlik duvarları ayarlayın. Hyper-V yüklemesi sırasında güvenlik duvarı kuralları (443 üzerindeki HTTPS, Kerberos 80 üzerinde) bağlantı noktalarının varsayılan trafiğe izin verecek şekilde oluşturulur. Bu kurallar aşağıdaki gibi etkinleştirin:
  - Sertifika kimlik doğrulaması kümesindeki (443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
  - Kerberos kimlik doğrulaması kümesindeki (80):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
  - Tek başına sunucuda sertifika kimlik doğrulamasını:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
  - Tek başına sunucuda Kerberos kimlik doğrulamasını:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-the-capacity-planner-tool"></a>3. adım: kapasite Planlayıcısı Aracı'nı çalıştırma
Sonra birincil sitenizi hazırladığınız ve bir kurtarma sunucusu ayarlamanız, aracını çalıştırabilirsiniz.

1. [Karşıdan](https://www.microsoft.com/download/details.aspx?id=39057) Microsoft Download Center aracından.
2. Birincil sunucuda (veya birincil küme düğümlerinden biri) birinden aracı çalıştırın. .Exe dosyasını sağ tıklatın ve ardından **yönetici olarak çalıştır**.
3. İçinde **başlamadan önce**, veri toplamak istediğiniz ne kadar süreyle belirtin. Veri temsilcisi olduğundan emin olmak için üretim saatleri sırasında Aracı çalıştırdığınız öneririz. Yalnızca ağ bağlantısını doğrulamak çalışıyorsanız, yalnızca bir dakika toplayabilirsiniz.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)
4. İçinde **birincil site ayrıntıları**, sunucu adını veya FQDN için bir tek başına konak belirtin veya istemcinin FQDN'sini noktası kabul etmek için bir küme belirtin, küme adı veya kümedeki herhangi bir düğümün ve ardından **sonraki**. Aracın çalıştığı sunucunun adını otomatik olarak algılar. Belirtilen sunucular için izlenebilir Vm'leri Yedekleme Aracı'nı seçer.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)
5. İçinde **çoğaltma Site ayrıntıları**, Azure'da çoğaltma yaparken ya da, bir ikincil veri merkezine çoğaltma yapıyorsanız ve bir çoğaltma sunucusunu ayarlamadıysanız seçin **Atla çoğaltma sitesine içeren testler**. Bir ikincil veri merkezine çoğaltma yapıyorsanız ve bir çoğaltma türü ayarladınız, tek başına sunucu veya istemci erişim noktası FQDN'si küme için girin **sunucu adı (veya) Hyper-V çoğaltma aracısı CAP**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)
6. İçinde **genişletilmiş çoğaltma ayrıntıları**, etkinleştirme **genişletilmiş çoğaltma sitesine içeren testleri atlayabilirsiniz**. Bu testler, Site Recovery tarafından desteklenmez.
7. İçinde **çoğaltılacak sanal makineleri seçin**, Araçlar sunucuya veya kümeye bağlanır ve VM'ler görüntüler ve diskleri, birincil sunucuda çalışan ayarlara göre belirttiğiniz **Birincil Site ayrıntıları** sayfası. VM'ler için çoğaltma zaten etkinleştirilmiş veya çalıştırmıyor, görüntülenmez. Ölçümleri toplamak istediğiniz sanal makineleri seçin. VHD'leri otomatik olarak seçme VM'ler için çok toplar.
8. Çoğaltma sunucusunun veya küme, yapılandırdıysanız, **ağ bilgilerini**düşündüğünüz yaklaşık WAN bant genişliğini birincil kullanılır ve çoğaltma siteleri ve seçin, yapılandırmışsanız sertifikalar sertifika kimlik doğrulaması.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)
9. İçinde **Özet**, ayarları denetleyin ve tıklatın **sonraki** ölçümleri toplamaya başlamak için. Aracı ilerleme durumu ve durumu görüntülenir **hesapla kapasite** sayfası. Aracı çalıştırma bittiğinde, bilgisayarınızı **raporu görüntüle** çıkışı görüntülemek için. Varsayılan olarak, raporları ve günlükleri depolanmış **%systemdrive%\Users\Public\Documents\Capacity Planlayıcısı**.

   ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-the-results"></a>Adım 4: sonuçları yorumlama

Burada, önemli ölçümleri bulunmaktadır. Burada listelenmeyen ölçümleri yoksayabilirsiniz. Bunlar Site Recovery için uygun değil.

### <a name="on-premises-to-on-premises-replication"></a>Şirket içi çoğaltma

* Çoğaltma birincil ana bilgisayarın hesaplama, bellek üzerindeki etkisi
* Birincil, Kurtarma konakları'nın depolama disk alanını IOPS çoğaltma etkisi
* Değişim çoğaltması (Mbps) için gerekli toplam bant genişliği
* Birincil ana makinesi (Mbps) kurtarma konağındaki arasındaki gözlenen ağ bant genişliği
* İki ana bilgisayarları/kümesi arasında etkin paralel aktarımı ideal sayısını önerisi

### <a name="on-premises-to-azure-replication"></a>Şirket içi Azure çoğaltma

* Çoğaltma birincil ana bilgisayarın hesaplama, bellek üzerindeki etkisi
* Çoğaltma birincil ana bilgisayarın depolama disk alanı, IOPS etkisi
* Değişim çoğaltması (Mbps) için gerekli toplam bant genişliği

## <a name="more-resources"></a>Diğer kaynaklar
* Aracı hakkında ayrıntılı bilgi için aracı yükleme eşlik belgeyi okuyun.
* İzlenecek yol Keith Mayer'ın aracı izlemek [TechNet blogu](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [Sonuçlar elde](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) bizim performans şirket içi Hyper-V çoğaltma için test etme

## <a name="next-steps"></a>Sonraki adımlar

Kapasite planlama bitirdikten sonra Site Recovery dağıtımı başlatabilirsiniz:

* [VMM bulutlarındaki Hyper-V Vm'lerini Azure'a çoğaltma](site-recovery-vmm-to-azure.md)
* [Hyper-V Vm'lerini (VMM olmadan) Azure'a çoğaltma](site-recovery-hyper-v-site-to-azure.md)
* [Hyper-V Vm'lerini VMM siteler arası çoğaltma](site-recovery-vmm-to-vmm.md)
