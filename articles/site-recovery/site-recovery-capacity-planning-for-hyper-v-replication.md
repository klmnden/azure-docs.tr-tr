---
title: "Hyper-V kapasite aracı Azure Site Recovery için planlama | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery için Hyper-V kapasite planlama aracı çalıştırmak açıklar"
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
ms.date: 11/28/2017
ms.author: nisoneji
ms.openlocfilehash: 80c3dcb65bd74bcfa3acc5f12dd5c45cd1c06455
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="hyper-v-capacity-planning-tool-for-site-recovery"></a>Hyper-V kapasite aracı Site kurtarma için planlama

Yeni bir Gelişmiş sürümü [Hyper-V Azure dağıtımı için Azure Site kurtarma dağıtım Planlayıcısı](site-recovery-hyper-v-deployment-planner.md) kullanıma sunulmuştur. Bu, eski aracı yerini alır. Yeni aracı, dağıtım planlama için kullanın. Aracı aşağıdaki yönergeleri sağlar:

* Diskler, disk boyutu, IOPS, karmaşıklık ve birkaç VM özelliklerini sayısına göre VM uygunluk değerlendirme
* Ağ bant genişliği karşı RPO değerlendirmesi gerekir
* Azure altyapı gereksinimleri
* Şirket içi altyapı gereksinimleri
* İlk çoğaltma Kılavuzu toplu işleme
* Toplam olağanüstü durum kurtarma maliyeti Azure tahmini

Azure Site Recovery dağıtımınızın bir parçası, çoğaltma ve bant genişliği gereksinimlerini çıkış şekil gerekir. Hyper-V kapasite aracı Site kurtarma için planlama Hyper-V VM çoğaltma bu gereksinimlerini belirlemenize yardımcı olur.

Bu makale, Hyper-V kapasite planlama aracı çalıştırmak açıklamaktadır. Bu aracı bilgileri ile birlikte kullanmak [kapasite planlama için Site Recovery](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Başlamadan önce
Birincil bir Hyper-V sunucusunda veya küme düğümünde aracı çalıştırın. Aracı çalıştırmak için Hyper-V ana bilgisayar sunucuları gerekir:

* İşletim sistemi: Windows Server 2012 veya 2012 R2
* Bellek: 20 MB (en az)
* CPU: yüzde 5'i yükünü (en az)
* Disk alanı: 5 MB (en az)

Aracı çalıştırılmadan önce birincil site hazırlamanız gerekir. İki şirket içi siteler arasında çoğaltma ve bant genişliğini denetlemek istiyorsanız aynı zamanda bir çoğaltma sunucusu hazırlamanız gerekir.

## <a name="step-1-prepare-the-primary-site"></a>Adım 1: birincil site hazırlama

1. Birincil sitede çoğaltmak istediğiniz tüm Hyper-V VM'ler listesini hazırlayın. Hyper-V konakları veya bulundukları kümeleri listeleyin. Aracı, tek bir küme, ancak ikisini birlikte veya birden çok tek başına ana bilgisayar için çalıştırabilirsiniz. Ayrıca her işletim sistemi için ayrı olarak çalıştırmanız gerekir. Hyper-V sunucuları hakkında aşağıdaki bilgileri toplayın:

   * Windows Server 2012 tek başına sunucular
   * Windows Server 2012 kümeleri
   * Windows Server 2012 R2 tek başına sunucular
   * Windows Server 2012 R2 kümeleri

2. Tüm Hyper-V konakları ve kümeleri Windows Yönetim Araçları için uzaktan erişimi etkinleştirin. Her bir sunucu veya küme güvenlik duvarı kuralları emin olmak için bu komutu çalıştırın ve kullanıcı izinleri ayarlayın:

        netsh firewall set service RemoteAdmin enable
3. Performans sunucuları ve kümeleri üzerinde aşağıdaki gibi izlemeyi etkinleştir:

   a. Açık Özellikli Windows Güvenlik Duvarı **Gelişmiş Güvenlik** ek bileşenini. 
   
   b. Gelen kuralı seçin **COM + Ağ erişimi (DCOM-ın)**. Tüm kuralları seçin **Uzak Olay Günlüğü Yönetimi** grubu.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>2. adım: çoğaltma sunucusunu (şirket içi çoğaltma) hazırlama
Azure'a çoğaltma varsa, bu adımı yapmanız gerekmez.

Böylece bir kukla VM bant genişliğini denetlemek için çoğaltılabilir tek bir Hyper-V ana bilgisayar kurtarma sunucusu olarak ayarlamanızı öneririz. Bu adımı atlayabilirsiniz, ancak bunu yapmanız sürece bant genişliği ölçmek olamaz.

1. Çoğaltma olarak bir küme düğümünü kullanmak istiyorsanız, Hyper-V çoğaltma aracısı yapılandırın:

   a. İçinde **Sunucu Yöneticisi'ni**, açık **yük devretme kümesi Yöneticisi**.

   b. Kümeye bağlanın ve küme adı vurgulayın. Seçin **Eylemler** > **rolü Yapılandır** Yüksek Kullanılabilirlik Sihirbazı'nı açın.

   c. İçinde **rolü Seç**seçin **Hyper-V çoğaltma aracısı**. Sihirbazı'nda için bir ad girin **NetBIOS adı**. İçin bir adres girin **IP adresi** (bir istemci erişim noktası adı verilir) kümeye bağlantı noktası olarak kullanılacak. **Hyper-V çoğaltma aracısı** yapılandırılır, dikkat etmeniz gerekir, bir istemci erişim noktası adı sonuçlanır.

   d. Hyper-V çoğaltma aracısı rolünün başarılı bir şekilde çevrimiçi olduğunu ve kümenin tüm düğümleri arasında yük devredebildiğini doğrulayın. Role sağ tıklayıp seçin **taşıma** > **düğüm Seç**. Bir düğüm seçin ve ardından **Tamam**.

   e. Sertifika tabanlı kimlik doğrulaması kullanırsanız, her bir küme düğümünün ve istemci erişim noktası tüm sertifikasının yüklü emin olun.

2. Çoğaltma sunucusunu etkinleştirmek için aşağıdaki adımları gerçekleştirin:

   a. Bir küme için açık **hatası Küme Yöneticisi'ni** ve kümeye bağlanın. Seçin **rolleri**ve ardından bir rol seçin. Seçin **çoğaltma ayarları** > **Bu kümeyi çoğaltma sunucusu olarak etkinleştir**. Çoğaltma olarak bir küme kullanıyorsanız da birincil sitedeki küme üzerinde Hyper-V çoğaltma aracısı rolünün mevcut olması gerekir.

   b. Bir tek başına sunucu için açık **Hyper-V Yöneticisi'ni**. İçinde **Eylemler** bölmesinde, **Hyper-V ayarları** etkinleştirmek istediğiniz sunucu için. İçinde **çoğaltma yapılandırması**seçin **bu bilgisayarı çoğaltma sunucusu olarak etkinleştir**.

3. Kimlik doğrulamasını ayarlamak için aşağıdaki adımları gerçekleştirin:

   a. Altında **kimlik doğrulaması ve bağlantı noktaları**, birincil sunucu ve kimlik doğrulama kimliğini doğrulamak için bağlantı noktalarını yöntemini seçin. Bir sertifika kullanıyorsanız seçin **Sertifika Seç** birini seçmek için. Birincil ve kurtarma Hyper-V konakları aynı etki alanında veya güvenilen etki alanlarında Kerberos kullanın. Farklı etki alanları için veya bir çalışma grubu dağıtımı için sertifikaları kullanın.

   b. Altında **yetkilendirme ve depolama**, izin **herhangi** çoğaltma verilerini Bu çoğaltma sunucusuna göndermek için (birincil) sunucu kimlik doğrulaması.

   ![Çoğaltma yapılandırması](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

   c. Dinleyici protokolü veya belirttiğiniz bağlantı noktası için çalışıp çalışmadığını denetlemek için çalıştırın **netsh http show servicestate**. 

4. Güvenlik duvarları ayarlayın. Hyper-V yüklemesi sırasında güvenlik duvarı kuralları (443 üzerindeki HTTPS) ve Kerberos 80 üzerinde bağlantı noktalarını varsayılan trafiğe izin verecek şekilde oluşturulur. Bu kurallar aşağıdaki gibi etkinleştirin:

    - Sertifika kimlik doğrulaması kümesindeki (443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
    - Kerberos kimlik doğrulaması kümesindeki (80):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
    - Tek başına sunucuda sertifika kimlik doğrulamasını:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
    - Tek başına sunucuda Kerberos kimlik doğrulamasını:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-the-capacity-planning-tool"></a>3. adım: kapasite planlama aracı çalıştırma
Birincil sitenizi hazırlamak ve bir kurtarma sunucusu kurma sonra aracını çalıştırabilirsiniz.

1. [Karşıdan](https://www.microsoft.com/download/details.aspx?id=39057) Microsoft Download Center aracından.

2. Birincil sunuculardan biri ya da birincil küme düğümlerinden biri aracı çalıştırın. .Exe dosyasını sağ tıklatın ve ardından **yönetici olarak çalıştır**.

3. İçinde **başlamadan önce**, veri toplamak istediğiniz ne kadar süreyle belirtin. Aracı'nı veri temsilcisi olduğundan emin olmak için üretim saatlerde çalıştırmanızı öneririz. Yalnızca ağ bağlantısını doğrulamak istiyorsanız, yalnızca bir dakika toplayabilirsiniz.

    ![Başlamadan önce](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. İçinde **Birincil Site ayrıntıları**, sunucu adını veya FQDN için bir tek başına konak belirtin. Bir küme için kümedeki istemci erişim noktası, küme adı veya herhangi bir düğümün FQDN'sini belirtin. **İleri**’yi seçin. Aracın çalıştığı sunucunun adını otomatik olarak algılar. Belirtilen sunucular için izlenebilir Vm'leri Yedekleme Aracı'nı seçer.

    ![Birincil Site ayrıntıları](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. İçinde **çoğaltma Site ayrıntıları**azure'a veya ikincil veri merkezine çoğaltma ve çoğaltma sunucusu ayarlamadıysanız seçin, **Atla çoğaltma sitesine içeren testler**. Bir ikincil veri merkezine çoğaltma ve bir yineleme tipi ayarlayın, küme için tek başına sunucu veya istemci erişim noktasının FQDN'sini girin **sunucu adı (veya) Hyper-V çoğaltma aracısı CAP**.

    ![Çoğaltma Site ayrıntıları](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. İçinde **genişletilmiş çoğaltma ayrıntıları**seçin **genişletilmiş çoğaltma sitesine içeren testleri atlayabilirsiniz**. Bu testler, Site Recovery tarafından desteklenmez.

7. İçinde **çoğaltılacak sanal makineleri seçin**, aracı sunucuya veya kümeye bağlanır. Sanal makineleri görüntüler ve üzerinde belirttiğiniz ayarlara göre birincil sunucuda çalışan disklerinin **Birincil Site ayrıntıları** sayfası. Zaten çoğaltma için etkinleştirilmiş veya çalışan olmayan VM'ler görüntülenmez. Ölçümleri toplamak istediğiniz sanal makineleri seçin. VHD'leri otomatik olarak seçme VM'ler için çok toplar.

8. Çoğaltma sunucusunun veya küme için yapılandırdığınız varsa **ağ bilgilerini**, birincil ve çoğaltma siteler arasında kullanılacak yaklaşık WAN bant genişliğini belirtin. Sertifika kimlik doğrulaması yapılandırılmışsa, Sertifikalar'ı seçin.

    ![Ağ bilgileri](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

9. İçinde **Özet**, ayarlarını kontrol edin. Seçin **sonraki** ölçümleri toplamaya başlamak için. Aracı ilerleme durumu ve durumu görüntülenir **hesapla kapasite** sayfası. Aracın çalışması tamamlandığında seçin **raporu görüntüle** çıkışı görüntülemek için. Varsayılan olarak, raporları ve günlükleri depolanmış **%systemdrive%\Users\Public\Documents\Capacity Planlayıcısı**.

   ![Kapasite Hesapla](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-the-results"></a>Adım 4: sonuçları yorumlama

Burada, önemli ölçümleri bulunmaktadır. Burada listelenmeyen ölçümleri yoksayabilirsiniz. Bunlar Site Recovery için uygun değil.

### <a name="on-premises-to-on-premises-replication"></a>Şirket içi çoğaltma

* Çoğaltma birincil ana bilgisayarın işlem ve bellek üzerindeki etkisi
* Birincil ana bilgisayarın kurtarma konağın depolama disk alanını ve IOPS çoğaltma etkisi
* Değişim çoğaltması (Mbps) için gerekli toplam bant genişliği
* Birincil ana makinesi (Mbps) kurtarma konağındaki arasındaki gözlenen ağ bant genişliği
* İki ana veya kümeleri arasında etkin paralel aktarımı ideal sayısını önerisi

### <a name="on-premises-to-azure-replication"></a>Şirket içi Azure çoğaltma

* Çoğaltma birincil ana bilgisayarın işlem ve bellek üzerindeki etkisi
* Çoğaltma birincil ana bilgisayarın depolama disk alanını ve IOPS etkisi
* Değişim çoğaltması (Mbps) için gerekli toplam bant genişliği

## <a name="more-resources"></a>Diğer kaynaklar

* Aracı hakkında daha fazla bilgi için aracı yükleme eşlik belgeyi okuyun.
* İzlenecek yol Keith Mayer'ın aracı izlemek [TechNet blogu](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [Sonuçlar elde](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) performans şirket içi Hyper-V çoğaltma için test etme.

## <a name="next-steps"></a>Sonraki adımlar

Kapasite planlama tamamladıktan sonra Site Recovery dağıtabilirsiniz:
* [VMM bulutlarındaki Hyper-V Vm'lerini Azure'a çoğaltma](site-recovery-vmm-to-azure.md)
* [Hyper-V Vm'lerini (VMM olmadan) Azure'a çoğaltma](site-recovery-hyper-v-site-to-azure.md)
* [Hyper-V Vm'lerini VMM siteler arası çoğaltma](site-recovery-vmm-to-vmm.md)
