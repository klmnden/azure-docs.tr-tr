---
title: Azure sanal makinelerinde Oracle çözümleri | Microsoft Docs
description: Desteklenen yapılandırmalar ve Microsoft Azure üzerinde Oracle sanal makine görüntüleri sınırlamalar hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: rogirdh
ms.custom: seodec18
ms.openlocfilehash: 70e87a38373688c1b364a079cd07934309662e3e
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707428"
---
# <a name="oracle-vm-images-and-their-deployment-on-microsoft-azure"></a>Oracle VM görüntülerinin ve dağıtım üzerinde Microsoft Azure

Bu makalede, Azure Market'te Oracle tarafından yayımlanan sanal makine görüntüleri göre Oracle çözümleri hakkında bilgiler ele alınmaktadır. Bulutlar arası uygulama çözümlerinde Oracle bulut altyapısı ile ilgileniyorsanız bkz [Oracle uygulama çözümlerini Microsoft Azure'da ve Oracle bulut altyapısı tümleştirme](oracle-oci-overview.md).

Şu anda kullanılabilir görüntülerin listesini almak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az vm image list --publisher oracle -o table --all
```

Aşağıdaki resimlerde Mayıs 2019'den itibaren kullanılabilir:

```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Ee      Oracle       12.2.0.1                Oracle:Oracle-Database-Ee:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Ee      Oracle       18.3.0.0                Oracle:Oracle-Database-Ee:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Se      Oracle       12.2.0.1                Oracle:Oracle-Database-Se:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Se      Oracle       18.3.0.0                Oracle:Oracle-Database-Se:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Linux            Oracle       6.10                    Oracle:Oracle-Linux:6.10:6.10.20190506                       6.10.20190506
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20190506                         6.8.20190506
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20190506                         6.9.20190506
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20190506                         7.3.20190506
Oracle-Linux            Oracle       7.4                     Oracle:Oracle-Linux:7.4:7.4.20190506                         7.4.20190506
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20181207                         7.5.20181207
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20190506                         7.5.20190506
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20181207                         7.6.20181207
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20190506                         7.6.20190506
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Bu görüntüler, "Kendi lisansını getir" olarak kabul edilir ve bu nedenle, yalnızca işlem, depolama ve ağ maliyetleri VM çalıştırmaktan kaynaklanan için ücretlendirilirsiniz.  Oracle yazılımı ve Oracle ile yerinde geçerli bir destek sözleşmesi olması kullanmak için düzgün şekilde lisanslandığından varsayılır. Oracle lisans taşınabilirliği şirket içinden Azure'a garantisi. Yayımlanan bkz [Oracle ve Microsoft](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) Not lisans taşınabilirliği hakkında ayrıntılı bilgi için. 

Kişiler de azure'da sıfırdan oluşturabilir veya kendi şirket içi ortamınızı özel bir görüntüyü karşıya yükleme özel bir görüntü çözümleri temel seçebilirsiniz.

## <a name="support-for-jd-edwards"></a>Azure'da JD Edwards desteği
Oracle desteği Not göre [belge kimliği 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4), azure'da JD Edwards EnterpriseOne sürümleri 9.2 ve üzeri desteklenir **tüm genel bulut teklifi** kendi özel karşılayan `Minimum Technical Requirements` (MTR).  İşletim sistemi ve yazılım uygulama uyumluluğu için kendi MTR belirtimlere özel görüntüleri oluşturmak için ihtiyacınız. 

## <a name="oracle-database-vm-images"></a>Oracle veritabanı VM görüntüleri
Oracle, Oracle Linux tabanlı sanal makine görüntülerinde, Azure'da çalıştırılan Oracle VT 12,1 Standard ve Enterprise sürümleri destekler.  Azure'da Oracle DB, üretim iş yükleri için en iyi performans için Premium Depolama tarafından yedeklenen yönetilen diskler düzgün bir şekilde sanal makine görüntüsünü boyut ve emin olun. VM görüntüsü, Oracle DB çalışmaya Oracle kullanarak Azure'da hızlıca konusunda yönergeler yayımlanan [Oracle DB Hızlı Başlangıç Kılavuzu deneyin](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Bağlı disk yapılandırma seçenekleri

Kullanıma açılan diskler Azure Blob Depolama hizmetinin avantajlarından yararlanın. Her standart disk, yaklaşık 500 giriş/çıkış işlemi (IOPS) saniyede teorik üst sınırı yeteneğine sahiptir. Premium disk teklifimize yüksek performanslı veritabanı iş yükleri için tercih edilir ve disk başına en fazla 5000 IOPS değerlerine ulaşabilir. Performans gereksinimlerinizi karşılayan tek bir disk kullanabilirsiniz. Ancak, birden çok kullanıma açılan diskler kullanmak, veritabanı verileri bunların arasında yaymaktır ve Oracle otomatik Depolama Yönetimi (ASM) kullanın, maliyetli IOPS performansı artırabilir. Bkz: [Oracle otomatik depolama genel bakış](https://www.oracle.com/technetwork/database/index-100339.html) daha fazla Oracle ASM belirli bilgi. Yükleme ve Linux Azure sanal makinesinde Oracle ASM yapılandırma örneği için bkz. [yükleme ve Oracle otomatik Depolama Yönetimi yapılandırma](configure-oracle-asm.md) öğretici.

### <a name="shared-storage-configuration-options"></a>Paylaşılan depolama alanı yapılandırma seçenekleri

Azure NetApp dosyaları, veritabanları gibi yüksek performanslı iş yükleri bulutta çalışan çekirdek gereksinimlerini karşılamak üzere tasarlanmıştır ve sağlar;
- Azure yerel paylaşılan VM yerel NFS İstemcisi veya Oracle dNFS veya Oracle iş yüklerini çalıştırmak için NFS depolama hizmeti
- Gerçek aralık IOPS taleplerini yansıtan Ölçeklenebilir performans katmanları
- Düşük gecikme süresi
- Yüksek kullanılabilirlik, yüksek bir dayanıklılık düzeyi ve uygun ölçekte, genellikle talep eden iş açısından kritik kurumsal iş yükleri (örneğin, SAP ve Oracle) yönetilebilirliği
- Hızlı ve verimli yedekleme ve kurtarma, en agresif RTO ve RPO SLA'lara elde etmek için

Bu özellikler Azure NetApp dosyaları – Azure yerel bir hizmet olarak Azure veri merkezi ortamında çalışan NetApp ONTAP® tamamen flash sistemler bağlı olduğu mümkündür. Sonuç, sağlanan ve tüketilen gibi diğer Azure depolama seçenekleri yalnızca ideal veritabanı depolama teknolojisidir. Bkz: [Azure NetApp dosyaları belgeleri](https://docs.microsoft.com/azure/azure-netapp-files/) dağıtma ve Azure NetApp dosya NFS birimleri erişim hakkında daha fazla bilgi için. Bkz: [Oracle Azure dağıtım en iyi uygulama kılavuzu kullanarak Azure NetApp dosya çubuğunda](https://www.netapp.com/us/media/tr-4780.pdf) Azure NetApp dosya çubuğunda bir Oracle veritabanına işletme için en iyi yöntem önerileri için.


## <a name="oracle-real-application-cluster-oracle-rac"></a>Oracle gerçek uygulama kümesi (Oracle RAC)
Oracle RAC, bir şirket içi çok düğümlü küme yapılandırmasında tek bir düğümün hatalarını azaltmak için tasarlanmıştır. Hiper ölçekli genel bulut ortamları için yerel olmayan iki şirket içi teknolojisi kullanır: ağ çok noktaya yayın ve paylaşılan disk. Veritabanı çözümünüze azure'da Oracle RAC gerektiriyorsa, üçüncü gerekir bu teknolojiler etkinleştirmek için taraf yazılım. Oracle RAC hakkında daha fazla bilgi için bkz. [FlashGrid SkyCluster sayfa](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="high-availability-and-disaster-recovery-considerations"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma değerlendirmeleri
Oracle veritabanlarının Azure'da kullanırken herhangi kapalı kalma süresini önlemek için bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü uygulamak için sorumlu olursunuz. 

(Bağlı olan üzerinde Oracle RAC) olmadan yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Oracle Database Enterprise Edition sürümünü kullanarak Azure üzerinde gerçekleştirilebilir [veri koruma, etkin Data Guard](https://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html), veya [Oracle Goldengate'i](https://www.oracle.com/technetwork/middleware/goldengate), iki ayrı sanal makinelere iki veritabanı ile. Her iki sanal makine aynı olmalıdır [sanal ağ](https://azure.microsoft.com/documentation/services/virtual-network/) bunlar erişebildiğinden birbirine özel kalıcı IP adresi emin olmak için.  Ayrıca, sanal makineler aynı kullanılabilirlik kümesinde bunları ayrı hata etki alanlarına yerleştirin ve yükseltme etki alanlarında izin vermeniz yerleştirme öneririz. Coğrafi yedeklilik sahip olmasını isterseniz, iki farklı bölgeler arasında çoğaltmak ve iki örnek bir VPN ağ geçidi ile bağlanmak için iki veritabanı ayarlayın.

Öğreticiyi [azure'da uygulama Oracle Data Guard](configure-oracle-dataguard.md) Azure'da temel kurulum yordam boyunca size yol gösterir.  

Oracle Data Guard ile bir sanal makine, başka bir sanal makine, ikincil (Bekleyen) veritabanında birincil veritabanı ile yüksek kullanılabilirlik sağlanabilir ve tek yönlü çoğaltma aralarında ayarlayın. Veritabanı kopyasını okuma erişimi sonucudur. Oracle Goldengate'i ile iki veritabanı arasında iki yönlü çoğaltmayı yapılandırabilirsiniz. Bu araçları kullanarak veritabanlarınız için bir yüksek kullanılabilirlik çözümü hakkında bilgi edinmek için bkz: [etkin Data Guard](https://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) ve [Goldengate'i](https://docs.oracle.com/goldengate/1212/gg-winux/index.html) Oracle Web sitesinden belgeleri. Access veritabanı kopyasına salt okunur gereksinim duyarsanız, kullanabileceğiniz [etkin Oracle Data Guard](https://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

Öğreticiyi [uygulama azure'da Oracle Goldengate'i](configure-oracle-golden-gate.md) Azure'da temel kurulum yordam boyunca size yol gösterir.

Azure'da desteklemesi için bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü sahip olmaya ek olarak, veritabanınızı geri yüklemek için yerinde bir yedekleme stratejisi olmalıdır. Öğreticiyi [yedekleme ve Kurtarma bir Oracle veritabanına](oracle-backup-recovery.md) tutarlı yedek oluşturmak için temel yordamı gösterilmektedir.

## <a name="oracle-weblogic-server-virtual-machine-images"></a>Oracle WebLogic Server sanal makine görüntüleri

* **Kümeleme Enterprise Edition'da yalnızca desteklenir.** WebLogic Server Enterprise Edition kullanırken WebLogic kümeleme kullanmak üzere lisanslanır. WebLogic Server Standard Edition ile kümeleme kullanmayın.
* **UDP çok noktaya yayın desteklenmiyor.** Azure, UDP aktarmaya, ancak çok noktaya yayın veya yayın destekler. WebLogic Server, Azure UDP tek noktaya yayın özelliklerine dayanan kuramıyor. UDP tek noktaya yayın üzerinde bağlı olan en iyi sonuçlar için statik ya da en fazla 10 yönetilen sunucularla tutulan WebLogic küme boyutu tutulur öneririz.
* **WebLogic Server (örneğin, Kurumsal JavaBeans kullanırken) T3 erişim için aynı olması için genel ve özel bağlantı noktaları bekliyor.** Adlı bir sanal ağda iki veya daha fazla VM oluşan bir WebLogic Server kümesinde bir hizmet Katmanı (EJB) uygulaması çalıştığı çok katmanlı oluşturduğunuzu *SLWLS*. İstemci katmanı, hizmet katmanında EJB çağrılmaya çalışılıyor, basit bir Java programı çalıştırma aynı sanal ağdaki farklı bir alt ağdaki içindir. Yük Dengeleme hizmet katmanı için gerekli olduğundan, genel bir yük dengeli uç nokta WebLogic Server kümedeki sanal makinelerin oluşturulması gerekir. Belirttiğiniz özel bağlantı noktası (örneğin, 7006:7008) ortak bağlantı noktasından farklı ise, aşağıdaki gibi bir hata oluşur:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Yük Dengeleyici bağlantı noktası ve yönetilen WebLogic server bağlantı noktası ile aynı WebLogic Server hiçbir uzaktan T3 erişim için bekliyor olmasıdır. Yukarıdaki durumda, istemci bağlantı noktası (yük dengeleyici bağlantı noktası) 7006 erişiyor ve 7008 (özel bağlantı noktası) üzerinde yönetilen sunucu dinliyor. Bu kısıtlama yalnızca T3 erişim için HTTP değil geçerlidir.

   Bu sorunu önlemek için aşağıdaki geçici çözümlerden birini kullanın:

  * T3 erişim için atanmış bir yük dengeli uç noktaları için aynı özel ve genel bağlantı noktası numaralarını kullanın.
  * Aşağıdaki JVM parametresini WebLogic Server başlatırken şunlardır:

    ```
    -Dweblogic.rjvm.enableprotocolswitch=true
    ```

İlgili bilgi için bkz. KB makalesi **860340.1** adresindeki <https://support.oracle.com>.

* **Dinamik Kümeleme ve sınırlamalar Yük Dengeleme.** Dinamik bir kümeye WebLogic Server kullanın ve bunu bir tek, genel yük dengeli uç noktası aracılığıyla azure'da kullanıma istediğinizi varsayalım. (Bir aralıktan dinamik olarak atanır) yönetilen sunucuların her birinde bir sabit bağlantı noktası numarası kullanmak ve yönetilen sunucuların fazla yönetici izleme makine sayısından başlatmayın sürece bu yapılabilir. Diğer bir deyişle, var. sanal makine başına birden fazla yönetilen sunucu) Başlatılmakta olan sanal makinelerin sayısından daha fazla WebLogic sunucusu yapılandırmanızı neden oluyorsa (diğer bir deyişle, WebLogic Server birden fazla aynı sanal makineye paylaşacağı), birden fazla WebLogic sunucuları örneklerini için mümkün değildir Belirtilen bağlantı noktasını birkaç bağlamak için. Bu sanal makinedeki diğer başarısız.

   Benzersiz bağlantı noktası numaraları, yönetilen sunuculara otomatik olarak atamak için yönetim sunucusu yapılandırırsanız, bunun için gerekli olduğu gibi Azure birden çok özel bağlantı noktası, tek bir genel bağlantı noktası eşleme desteklemediği için ardından Yük Dengeleme mümkün değildir yapılandırma.
* **WebLogic Server bir sanal makinede birden çok örneğini.** Sanal makine yeterince büyük olduğunda dağıtımınızın gereksinimlerine bağlı olarak, WebLogic Server'ın birden çok örneği aynı sanal makinede çalışan düşünebilirsiniz. Örneğin, bir orta büyüklükte iki çekirdek içeren tıkladığımda, WebLogic Server'ın iki örneği çalıştırmak seçebilirsiniz. Ancak yine de, tek hata noktalarından WebLogic Server'ın birden çok örneğini çalıştıran tek bir sanal makineyi kullandıysanız, durum olacaktır, mimarisine ile tanışın kaçınmanızı öneririz. En az iki sanal makine kullanarak daha iyi bir yaklaşım olabilir ve her bir sanal makine ardından WebLogic Server'ın birden çok örneğini çalıştırabilirsiniz. WebLogic Server'ın her örneğinin hala aynı kümenin parçası olabilir. Ancak, şu anda Azure yük dengeleyici, Yük Dengeleme sunucuları arasında benzersiz olarak dağıtılmasını gerektirdiğinden aynı sanal makine içinde tür WebLogic Server dağıtımlar tarafından kullanıma sunulan Yük Dengeleme uç noktalarına Azure kullanmak mümkün değil sanal makineler.

## <a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK sanal makine görüntüleri
* **JDK 6 ve 7 en son güncelleştirmeleri.** Java (şu anda Java 8) en son genel, desteklenen sürümünü kullanmanızı öneririz, ancak Azure de JDK 6 ve 7 görüntüleri kullanılabilir hale getirir. Bu, henüz JDK 8'e yükseltme için hazır olmayan eski uygulamalar için tasarlanmıştır. Önceki JDK görüntülerine artık Oracle, Microsoft ortaklığı verilen genel kullanım için kullanılabilir güncelleştirmeler sırasında JDK 6 ve 7 görüntüleri Azure tarafından sağlanan normalde Oracle tarafından sunulan daha yeni bir genel olmayan güncelleştirmeyi içeren yöneliktir Oracle yalnızca select Grup müşterilerin desteklenen. Yeni sürümler JDK görüntülerin güncelleştirilmiş sürümleri JDK 6 ve 7 ile zaman içinde kullanılabilir hale getirilir.

   JDK 6 ve 7 görüntüleri ve sanal makineleri ve bunları, türetilmiş görüntüleri bulunan JDK, yalnızca Azure içinde kullanılabilir.
* **64 bit JDK.** Oracle WebLogic Server sanal makine görüntüleri ve Azure tarafından sağlanan Oracle JDK sanal makine görüntüleri hem Windows Server hem de JDK 64-bit sürümleri içerir.

## <a name="next-steps"></a>Sonraki adımlar
Artık Microsoft Azure sanal makine görüntüleri göre geçerli Oracle çözümler genel bakış var. Azure'da ilk Oracle veritabanı, sonraki adım dağıtmaktır.

> [!div class="nextstepaction"]
> [Azure'da Oracle veritabanı oluşturma](oracle-database-quick-create.md)
