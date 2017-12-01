---
title: "Microsoft Azure üzerinde Oracle çözümleri | Microsoft Docs"
description: "Desteklenen yapılandırmalar ve Oracle çözümlerinin Microsoft Azure ile ilgili sınırlamalar hakkında bilgi edinin."
services: virtual-machines-linux
documentationcenter: 
manager: timlt
author: rickstercdn
tags: azure-resource-management
ms.assetid: 5d71886b-463a-43ae-b61f-35c6fc9bae25
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: rclaus
ms.openlocfilehash: 1bc03d15096e7f1d4538d6642a61aaee9bb572f7
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="oracle-solutions-and-their-deployment-on-microsoft-azure"></a>Oracle çözümleri ve Microsoft Azure üzerinde dağıtım
Bu makalede Microsoft Azure ile ilgili çeşitli Oracle çözümleri başarıyla dağıtmak için gereken bilgiler yer almaktadır. Bu çözümleri Azure Marketi Oracle tarafından yayımlanan sanal makine görüntülerini temel alır. Şu anda kullanılabilir görüntüleri listesini almak için aşağıdaki komutu çalıştırın:
```azurecli-interactive
az vm image list --publisher oracle -o table --all
```
6-16-2017 itibariyle görüntülerin listesi aşağıda verilmiştir:
```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Linux            Oracle       6.4                     Oracle:Oracle-Linux:6.4:6.4.20141206                         6.4.20141206
Oracle-Linux            Oracle       6.7                     Oracle:Oracle-Linux:6.7:6.7.20161007                         6.7.20161007
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20161020                         6.8.20161020
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20170406                         6.9.20170406
Oracle-Linux            Oracle       7.0                     Oracle:Oracle-Linux:7.0:7.0.20141217                         7.0.20141217
Oracle-Linux            Oracle       7.2                     Oracle:Oracle-Linux:7.2:7.2.20161020                         7.2.20161020
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20170320                         7.3.20170320
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Bu görüntüler "Kendi lisansını getir" olarak kabul edilir ve bu nedenle, yalnızca işlem, depolama ve ağ maliyetlerini VM çalıştırarak ücrete ücretlendirilir.  Doğru Oracle yazılım ve Oracle ile yerinde geçerli bir destek sözleşmesi sahip kullanmayı lisansına sahip olduğunuz varsayılır. Oracle lisans taşınabilirliği şirket içinden Azure'a garanti. Yayımlanan bkz [Oracle ve Microsoft](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) lisans taşınabilirliği hakkında ayrıntılı bilgi için unutmayın. 

Kişiler de Azure sıfırdan oluşturabilir veya kendi şirket içi ortamından özel bir görüntü karşıya özel bir görüntü çözümleri temel seçebilirsiniz.

## <a name="support-for-jd-edwards"></a>JD Edirneli desteği
Oracle destek Not göre [belge kimliği 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4) , JD Edirneli EnterpriseOne versions 9.2 ve üzerinde yukarıdaki desteklenen **tüm genel bulut teklifi** kendi özel karşılayan `Minimum Technical Requirements` (MTR).  İşletim sistemi ve yazılım uygulama uyumluluğu için kendi MTR belirtimlere özel resimler oluşturmanız gerekir. 

## <a name="oracle-database-virtual-machine-images"></a>Oracle veritabanı sanal makine görüntüleri
Oracle, Oracle Linux tabanlı sanal makine görüntüleri, Azure'da çalışan Oracle DB 12,1 Standard ve Enterprise sürümleri destekler.  Azure üzerinde Oracle DB, üretim iş yükleri için en iyi performans için düzgün şekilde VM görüntü boyutunu ve yönetilen Premium Depolama tarafından yedeklenen diskler kullanan emin olun. VM görüntüsü, hızlı bir şekilde hazır ve çalışır bir Oracle DB bu Oracle kullanarak Azure'da almak yönergeler yayımlanmış [Oracle DB Hızlı Başlangıç Kılavuzu deneyin](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Ekli disk yapılandırma seçenekleri

Kullanıma açılan diskler Azure Blob Depolama hizmetini kullanır. Her standart disk (IOPS) saniyede yaklaşık 500 giriş/çıkış işlemlerinin teorik maksimum yeteneğine sahiptir. Bizim premium disk sunumu, yüksek performanslı veritabanı iş yükleri için tercih edilir ve disk başına en fazla 5000 IOP elde edebilirsiniz. Performans gereksinimlerinizi karşılayan, tek bir disk kullanabilirsiniz - birden çok ekli disk kullanırsanız, etkili IOPS performansını iyileştirebilirsiniz olsa da, veritabanı veri bunları yayılan ve Oracle otomatik Depolama Yönetimi (ASM) kullanın. Bkz: [Oracle otomatik depolama genel bakış](http://www.oracle.com/technetwork/database/index-100339.html) daha fazla Oracle ASM belirli bilgiler için. Yükleme ve Oracle ASM bir Linux Azure VM - yapılandırma örneği için deneyebilirsiniz [yükleme ve Oracle otomatik Depolama Yönetimi yapılandırma](configure-oracle-asm.md) Öğreticisi.

## <a name="oracle-real-application-cluster-oracle-rac"></a>Oracle gerçek uygulama küme (Oracle RAC)
Oracle RAC, bir şirket içi çok düğümlü bir küme yapılandırmasında tek bir düğüm hatası azaltmak için tasarlanmıştır. Hiper ölçekli genel bulut ortamları için yerel olmayan iki şirket içi teknolojisi kullanır: ağ çok noktaya yayın ve paylaşılan disk. Veritabanı çözümünüzü azure'da Oracle RAC gerektiriyorsa, bu teknolojiler etkinleştirmek için 3 taraf yazılımları gerekir.  A **Azure Microsoft Sertifikalı** adlı teklifi [FlashGrid düğümü için Oracle RAC](https://azuremarketplace.microsoft.com/marketplace/apps/flashgrid-inc.flashgrid-racnode?tab=Overview) FlashGrid Inc. tarafından yayınlanan Azure Marketi'nde kullanılabilir Bu çözüm ve Azure'da nasıl çalıştığı hakkında daha fazla bilgi için lütfen bkz [FlashGrid çözüm sayfası](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="high-availability-and-disaster-recovery-considerations"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma değerlendirmeleri
Oracle veritabanları Azure'da kullanırken herhangi kesinti süresini önlemek için yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü uygulamak için sorumlu. 

(Olmadan Oracle RAC üzerinde bağlı olan) yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Oracle veritabanı Enterprise Edition elde edilebilir Azure kullanarak [veri koruma, etkin Data Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html), veya [Oracle Golden kapısı](http://www.oracle.com/technetwork/middleware/goldengate), iki ayrı sanal makinelere iki veritabanı ile. Her iki sanal makineleri aynı olmalıdır [sanal ağ](https://azure.microsoft.com/documentation/services/virtual-network/) bunlar erişebilir birbirine özel kalıcı IP adresi emin olmak için.  Ayrıca, sanal makineleri ayrı hata etki alanları yerleştirin ve etki alanlarını yükseltme Azure izin verecek şekilde ayarlamanız kullanılabilirlik yerleştirme öneririz.  Coğrafi yedeklilik - olmasını isterseniz, bu iki veritabanı arasında iki farklı bölgelerde çoğaltabilir ve iki örneği bir VPN ağ geçidi ile bağlanmak olabilir.

Bir öğretici sahip olduğumuz "[uygulama Oracle DataGuard azure'da](configure-oracle-dataguard.md)", hangi anlatılmaktadır, temel kurulum yordam boyunca deneme sürenizi bu Azure üzerinde.  

Oracle Data Guard ile yüksek oranda kullanılabilir bir sanal makine, başka bir sanal makine, ikincil (bekleme) veritabanında birincil veritabanı ile elde edilebilir ve tek yönlü çoğaltma aralarında ayarlayın. Veritabanının kopyasını okuma erişimi sonucudur. Oracle GoldenGate ile iki veritabanı arasında iki yönlü çoğaltma yapılandırabilirsiniz. Bu araçları kullanarak veritabanlarınız için yüksek kullanılabilirlik çözümü ayarlama hakkında bilgi edinmek için bkz: [etkin Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) ve [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) Oracle Web sitesindeki belgeleri. Erişim veritabanının kopyasını salt okunur gereksinim duyarsanız, kullanabileceğiniz [Oracle etkin Data Guard](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

Bir öğretici sahip olduğumuz "[uygulama Oracle GoldenGate azure'da](configure-oracle-golden-gate.md)", hangi anlatılmaktadır, temel kurulum yordam boyunca deneme sürenizi bu Azure üzerinde.

Azure'da tasarlanmış bir HA ve DR çözümü sahip olmasına rağmen bir yedekleme stratejisi veritabanınızı geri yerinde olduğundan emin olun istiyor.  Bir öğretici sahibiz [yedekleme ve kurtarma Oracle veritabanına](oracle-backup-recovery.md) hangi kılavuzluk, tutarlı bir yedeklemenin oluşturma temel yordamı.

## <a name="oracle-weblogic-server-virtual-machine-images"></a>Oracle WebLogic Server sanal makine görüntüleri
* **Kümeleme Enterprise Edition'da yalnızca desteklenir.** WebLogic yalnızca WebLogic Server Enterprise Edition kullanırken kümeleme kullanmak üzere lisanslanır. WebLogic Server Standard Edition ile kümeleme kullanmayın.
* **Çok noktaya yayın UDP desteklenmiyor.** Azure UDP tek, ancak çok noktaya yayın veya yayın destekler. WebLogic Server Azure UDP tek noktaya yayın özelliklerine dayanan yapabiliyor. En iyi sonuçları, UDP tek noktaya yayın üzerinde bağlı olan WebLogic küme boyutu statik tutulması önerilir için veya kümeye dahil en fazla 10 yönetilen sunucularla tutulmalıdır.
* **WebLogic Server T3 erişim (örneğin, Kurumsal JavaBeans kullanırken) için aynı olması için ortak ve özel bağlantı noktalarını bekliyor.** Adlı vNet içindeki iki veya daha fazla VM oluşan bir WebLogic Server kümesinde bir hizmet Katmanı (EJB) uygulaması çalıştığı bir çok katmanlı senaryoyu düşünün **SLWLS**. İstemci hizmet katmanında EJB çağrısı çalışılırken basit bir Java program çalıştıran aynı vnet'teki farklı bir alt ağ katmandır. Yük Dengelemesi hizmet katmanı için gerekli olduğundan, ortak bir yük dengeli uç nokta WebLogic sunucu kümesindeki sanal makineler için oluşturulması gerekir. Belirttiğiniz özel bağlantı noktası (örneğin, 7006:7008) ortak bağlantı noktasından farklı ise, aşağıdaki gibi bir hata oluşur:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Yük Dengeleyici bağlantı noktası ve aynı olması için yönetilen WebLogic server bağlantı noktası WebLogic Server herhangi T3, uzaktan erişim için bekliyor olmasıdır. Önceki durumda, istemci bağlantı noktası 7006 (yük dengeleyici bağlantı noktası) erişme ve 7008 (özel bağlantı noktası) üzerinde yönetilen sunucu dinliyor. Bu kısıtlama, yalnızca T3 erişimi için değil HTTP geçerlidir.

   Bu sorunu önlemek için aşağıdaki geçici çözümlerden birini kullanın:

  * T3 erişimi ayrılmış yük dengeli uç noktalar için aynı özel ve genel bağlantı noktası numaralarını kullanın.
  * WebLogic Server başlatırken aşağıdaki JVM parametresini içerir:

         -Dweblogic.rjvm.enableprotocolswitch=true

İlgili bilgi için bkz. KB makalesi **860340.1** adresindeki <http://support.oracle.com>.

* **Dinamik Kümeleme ve sınırlamaları Yük Dengeleme.** WebLogic Server dinamik bir küme kullanıp bir tek, ortak yük dengeli uç nokta Azure üzerinden kullanıma istediğinizi varsayalım. (Bir aralığından dinamik olarak atanır) yönetilen sunucuların her birinde için sabit bağlantı noktası numarasını kullanın ve yönetilen sunucular daha yönetici izleme makineler çok başlatılmaz sürece bu yapılabilir (diğer bir deyişle, birden fazla sunucu başına sanal m yönetilen akinesi). Sanal makineler çok başlatılmakta daha fazla WebLogic sunucuları yapılandırmanızı sonuçları (diğer bir deyişle, burada birden çok WebLogic Server örneklerini aynı sanal makine paylaşım), birden fazla WebLogic sunucuları örneklerine için mümkün değildir verilen bağlantı noktası numarası için – bağlamak için bu sanal makineye diğerleri başarısız.

   Benzersiz bağlantı noktası numaraları, yönetilen sunuculara otomatik olarak atamak için yönetim sunucusu yapılandırırsanız, bunun için gerekli olacak şekilde Azure birden çok özel bağlantı noktaları, tek bir ortak bağlantı noktası eşleme desteklemediği için sonra Yük Dengeleme olası değil yapılandırma.
* **Bir sanal makinede Weblogic Server birden çok örneği.** Dağıtımınızın gereksinimlerine bağlı olarak, sanal makine büyüklükte ise WebLogic Server birden çok örneğini aynı sanal makinede çalışan seçeneği düşünebilirsiniz. Örneğin, bir orta büyüklükte sanal iki çekirdek içeren makinede WebLogic Server'ın iki örnek çalıştıracak şekilde seçebilir. Ancak hala tek hata noktaları bulundurmaktan WebLogic Server birden çok örneğini çalıştıran tek bir sanal makine kullandıysanız, durumda olur, mimariye Tanıtımı kaçının öneririz olduğunu unutmayın. En az iki sanal makineleri kullanarak daha iyi bir yaklaşım olabilir ve bu sanal makinelerin her biri daha sonra WebLogic Server birden çok örneğini çalıştırabilir. Her WebLogic Server'ın bu örneği hala aynı kümenin parçası olabilir. Not, ancak bunu şu anda Azure yük dengeleyici Yük Dengeleme sunucuları arasında benzersiz dağıtılacak gerektirdiğinden Azure aynı sanal makine içinde bu tür WebLogic Server dağıtımlar tarafından sunulan Yük Dengeleme uç noktaları kullanmak mümkün değildir sanal makineler.

## <a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK sanal makine görüntüleri
* **JDK 6 ve 7 en son güncelleştirmeleri.** Java (şu anda 8 Java) son genel, desteklenen sürümünü kullanmanızı öneririz, ancak Azure ayrıca JDK 6 ve 7 görüntüleri kullanılabilir hale getirir. Bu, henüz JDK 8'e yükseltme için hazır olmayan eski uygulamalar için tasarlanmıştır. Önceki JDK görüntülerine artık Oracle, Microsoft yöneticileriyle verilen genel kullanım için kullanılabilir güncelleştirmeler sırada JDK 6 ve 7 görüntüleri Azure tarafından sağlanan normalde için Oracle tarafından sunulan daha yeni bir genel güncelleştirme içerecek şekilde tasarlanmıştır Oracle yalnızca belirli bir grup müşterilerin desteklenen. JDK görüntüleri yeni sürümlerini JDK 6 ve 7, güncelleştirilmiş sürümleriyle zamanla kullanıma sunulacaktır.

   Bu JDK 6 ve 7 görüntüleri ve sanal makineler ve kendilerinden türetilmiş görüntüleri bulunan JDK yalnızca Azure içinde kullanılabilir.
* **64-bit JDK.** Oracle WebLogic Server sanal makine görüntüleri ve Azure tarafından sağlanan Oracle JDK sanal makine görüntüleri Windows Server ve JDK 64-bit sürümünü içerir.

## <a name="next-steps"></a>Sonraki adımlar
Artık Microsoft Azure üzerinde geçerli Oracle çözümleri genel bir bakış vardır. Sonraki adımınız, Azure üzerinde ilk, Oracle veritabanı dağıtmaktır.
- Deneyin [Azure üzerinde bir Oracle veritabanı oluşturma](oracle-database-quick-create.md) kullanmaya başlama Öğreticisi.