---
title: Microsoft Azure Stack Geliştirme Seti sürüm notları | Microsoft Docs
description: Geliştirmeleri, düzeltmeleri ve bilinen sorunlar için Azure Stack Geliştirme Seti.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2018
ms.author: sethm
ms.reviewer: misainat
ms.openlocfilehash: d322fe378e7f662c233e9572dfc79dcd961137bd
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237815"
---
# <a name="azure-stack-development-kit-release-notes"></a>Azure Stack Geliştirme Seti sürüm notları  
Bu makalede, geliştirmeleri, düzeltmeler ve Azure Stack geliştirme Seti'ni'de bilinen sorunlar hakkında bilgi sağlar. Hangi sürümü çalıştırdığınızdan emin değilseniz yapabilecekleriniz [denetlemek için portal'ı kullanmanızı](.\.\azure-stack-updates.md#determine-the-current-version).

> Abone olarak ASDK yenilikler ile güncel kalın [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [akış](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11809xxx"></a>1.1809.x.xx oluşturun

### <a name="new-features"></a>Yeni Özellikler
Bu derleme, Azure Stack için aşağıdaki geliştirmeleri ve düzeltmeleri içerir.  

- <!--  2712869   | IS  ASDK -->  **Azure Stack syslog istemcisi (Genel kullanım)** bu istemci denetimler, uyarılar ve Azure Stack altyapısının bir syslog sunucusu veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılım ilgili güvenlik günlükleri iletilmesini sağlar. Azure Stack için dış. Syslog istemci artık syslog sunucunun dinlediği bağlantı noktasını destekler.

Bu sürümle birlikte, syslog istemci genel olarak kullanılabilir ve üretim ortamlarında kullanılabilir.

Daha fazla bilgi için [Azure Stack syslog iletmeyi](../azure-stack-integrate-security.md).

### <a name="fixed-issues"></a>Düzeltilen sorunlar

- <!-- 2702741 -  IS ASDK --> Dinamik Ayırma kullanılarak dağıtılan hangi genel IP'ler, yöntemi değildi neden olan sorun düzeltildi durdurun-serbest verildiği sonra korunması garanti. Bunlar artık korunur.

- <!-- 3078022 - IS ASDK --> Bir VM 1808 önce durdu-serbest olduysa, 1808 güncelleştirmeden sonra yeniden tahsis bulunamadı.  Bu sorun 1809 içinde düzeltilmiştir. Bu düzeltmeyle 1809 içinde başlatılamadı ve bu durumda örnek başlatılabilir. Düzeltme aynı zamanda bu sorunu yeniden oluşmasını engeller.

- **Çeşitli düzeltmeleri** performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi


### <a name="changes"></a>Değişiklikler

- <!-- 1697698  | IS, ASDK --> *Hızlı Başlangıç öğreticileri* Kullanıcı Portalı Panosu şimdi bağlantıda ilgili makaleleri çevrimiçi Azure Stack belgeleri.

- <!-- 2515955   | IS ,ASDK--> *Tüm hizmetler* değiştirir *diğer hizmetler* Azure Stack yönetici ve Kullanıcı Portalı'nda. Artık *tüm hizmetleri* Azure portallarında yaptığınız gibi Azure Stack portallarında gezinmek için alternatif olarak.

- <!--  TBD – IS, ASDK --> *Temel A* sanal makine boyutları için devre dışı [sanal makine ölçek kümeleri oluşturma](../azure-stack-compute-add-scalesets.md) (VMSS) portal üzerinden. Bu boyut ile bir VMSS oluşturmak için PowerShell ya da bir şablon kullanın. 

### <a name="known-issues"></a>Bilinen sorunlar

#### <a name="portal"></a>Portal  

- <!-- 1697698  | IS, ASDK --> *Hızlı Başlangıç öğreticileri* Kullanıcı Portalı Panosu şimdi bağlantıda ilgili makaleleri çevrimiçi Azure Stack belgeleri.

- <!-- 2515955   | IS ,ASDK--> *Tüm hizmetler* değiştirir *diğer hizmetler* Azure Stack yönetici ve Kullanıcı Portalı'nda. Artık *tüm hizmetleri* Azure portallarında yaptığınız gibi Azure Stack portallarında gezinmek için alternatif olarak.

- <!--  TBD – IS, ASDK --> *Temel A* sanal makine boyutları için devre dışı [sanal makine ölçek kümeleri oluşturma](../azure-stack-compute-add-scalesets.md) (VMSS) portal üzerinden. Bu boyut ile bir VMSS oluşturmak için PowerShell ya da bir şablon kullanın.

#### <a name="health-and-monitoring"></a>Sistem durumu ve izleme

- <!-- 1264761 - IS ASDK -->  Uyarıları görebilirsiniz *sistem durumu denetleyicisi* aşağıdaki ayrıntıları olan bir bileşeni:  

   Uyarı #1:
   - ADI: Sağlıksız altyapı rolü
   - Önem DERECESİ: uyarı
   - BİLEŞENİ: Sistem durumu denetleyicisi
   - Açıklama: Sistem durumu denetleyici sinyal tarayıcı kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.  

  Uyarı #2:
   - ADI: Sağlıksız altyapı rolü
   - Önem DERECESİ: uyarı
   - BİLEŞENİ: Sistem durumu denetleyicisi
   - Açıklama: Hata tarayıcı durumu denetleyicisi kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.

  Her iki uyarılar güvenle yoksayılabilir ve zaman içinde otomatik olarak kapatılacak.  

- <!-- 2368581 - IS. ASDK --> Düşük bellek uyarısı alırsanız ve Kiracı sanal makinelerini başarısız ile dağıtmak bir Azure Stack operatörü bir *Fabric VM oluşturma hatası*, Azure Stack damga kullanılabilir bellek yetersiz olabilir. Kullanım [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) iş yükleriniz için kapasite en iyi anlamak için.


#### <a name="compute"></a>İşlem  

- <!-- 3090289 – IS, ASDK --> Güncelleştirme 1808 uyguladıktan sonra yönetilen disklere sahip VM'ler dağıtırken aşağıdaki sorunlarla karşılaşabilirsiniz:

   1. Aboneliği, yönetilen diskler ile VM dağıtma 1808 güncelleştirmeden önce oluşturulduysa, bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için her abonelik için şu adımları izleyin:
      1. Kiracı Portalı'nda Git **abonelikleri** ve aboneliği bulunamıyor. Tıklayın **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
      2. Aynı abonelik altında Git **erişim denetimi (IAM)**, doğrulayın **Azure Stack – yönetilen Disk** listelenir.
   2. Bir konuk dizin ile ilişkili bir abonelik içindeki Vm'leri dağıtma, çok kiracılı bir ortam yapılandırdıysanız, bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için aşağıdaki adımları izleyin:
      1. Uygulama [1808 Azure Stack düzeltme](https://support.microsoft.com/help/4465859).
      2. Bağlantısındaki [bu makalede](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) her Konuk dizinlerinizi yeniden yapılandırmak için.

- <!-- 2869209 – IS, ASDK --> Kullanırken [ **Ekle AzsPlatformImage** cmdlet'i](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), kullanmalısınız **- OsUri** parametre olarak depolama hesabı URI'si disk nereye yüklenir. Yerel yol diskin kullanırsanız, cmdlet şu hatayla başarısız olur: *işlemi uzun süre çalışan 'Başarısız' durumuyla başarısız oldu*. 

- <!--  2966665 – IS, ASDK --> Premium boyutuna SSD veri diskleri ekleme, yönetilen disk sanal makineler (DS, DSv2, Fs, Fs_V2) bir hatayla başarısız oluyor: *'vmname' hata sanal makinenin diskleri güncelleştirilemedi: İstenen işlem gerçekleştirilemiyor depolama hesabı türü ' Premium_LRS'VM boyutu için desteklenmeyen ' Standard_DS/Ds_V2/FS/Fs_v2)*

   Bu sorunu geçici olarak çözmek için kullanın *Standard_LRS* veri diskleri yerine *Premium_LRS diskleri*. Kullanım *Standard_LRS* veri diskleri IOPS veya fatura ücreti değiştirmez.  

- <!--  2795678 – IS, ASDK --> VM, portalı sanal makineler (VM) oluşturmak için bir premium VM boyutu (DS, Ds_v2, FS, FSv2) kullandığınızda, bir standart depolama hesabı oluşturulur. Bir standart depolama hesabı oluşturma IOPS, işlevsel olarak, etkilemez ya da fatura. 

   Bildiren bir uyarıyı güvenle yok sayabilirsiniz: *premium diskleri destekleyen bir boyutta standart disk kullanmayı seçtiniz. Bu işletim sisteminin performansını etkileyebilir ve önerilmez. Premium depolamayı (SSD) kullanmayı düşünün.*

- <!-- 2967447 - IS, ASDK --> Sanal makine ölçek kümesi (VMSS) deneyimi oluşturmasına 7.2 CentOS tabanlı dağıtım için bir seçenek olarak sağlar. Bu görüntüyü Azure Stack üzerinde kullanılabilir olmadığından, dağıtımınız için başka bir işletim sistemi seçin veya Market'ten dağıtımdan işleciyle indirildi başka bir CentOS görüntüsü belirten bir ARM şablonu kullanın.

- <!-- TBD -  IS ASDK --> Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, D serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. Tüm desteklenen D serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

- <!-- TBD -  IS ASDK --> Bir VM görüntüsü oluşturulması başarısız olduğunda, nelze odstranit başarısız bir öğe için VM görüntüleri işlem dikey eklenebilir.

  Geçici çözüm olarak, Hyper-V ile oluşturduğunuz işlevsiz bir VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yolu C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silme engelleyen sorunu çözer. Ardından, işlevsiz bir görüntü oluşturduktan sonra 15 dakika başarıyla silebilirsiniz.

  Ardından, daha önce başarısız olan VM görüntüsünün indirilmesi yeniden deneyebilir.

- <!-- TBD -  IS ASDK --> VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

- <!-- 1662991 - IS ASDK --> Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.

- <!-- 2724961- IS ASDK --> Kaydettiğinizde **Microsoft.Insight** abonelik ayarlarında, kaynak sağlayıcısı ve bir Windows VM konuk işletim sistemi etkin Tanılama ile oluşturma, VM genel bakış sayfasında CPU yüzdesi grafik, ölçüm verilerini göstermek mümkün olmayacaktır.
 
  Sanal makine için CPU yüzdesi grafik bulmak için Git **ölçümleri** dikey penceresinde ve desteklenen tüm Windows VM show Konuk ölçümleri.

 

#### <a name="networking"></a>Ağ
- <!-- 1766332 - IS, ASDK --> Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

- <!-- 1902460 -  IS ASDK --> Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

- <!-- 16309153 -  IS ASDK --> DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.

- <!-- 2702741 -  IS ASDK --> Dinamik ayırma yöntemi kullanılarak dağıtılan genel IP'ler garanti edilmez durdurun-serbest verildiği sonra korunması.

- <!-- 2529607 - IS ASDK --> Azure Stack sırasında *gizli dönüş*, bir süre içinde genel IP adresleri olan erişilemeyen iki ila beş dakikalığına yoktur.

-   <!-- 2664148 - IS ASDK --> Bir S2S VPN tüneli aracılığıyla Kiracı sanal makinelerini burada erişiyor senaryolarda, yerel ağ geçidi için şirket içi alt ağ geçidi zaten oluşturulduktan sonra eklenmişse nerede bağlantı girişimleri başarısız bir senaryo hatalarla karşılaşabilirsiniz. 


<!--  #### SQL and MySQL  -->


#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- <!-- TBD - IS ASDK --> (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.  


#### <a name="usage"></a>Kullanım  
- <!-- TBD -  IS ASDK --> Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

<!-- #### Identity -->




## <a name="build-11807076"></a>Derleme 1.1807.0.76

### <a name="new-features"></a>Yeni Özellikler
Bu derleme, Azure Stack için aşağıdaki geliştirmeleri ve düzeltmeleri içerir.  

- <!-- 1658937 | ASDK, IS --> **Önceden tanımlanmış bir zamanlamaya göre yedeklemeleri başlatmak** -gereçlerden biri Azure Stack artık otomatik olarak düzenli aralıklarla insan müdahalesi ortadan kaldırmak için altyapı yedekleme tetikleyebilirsiniz. Azure Stack tanımlanan saklama süresinden daha eski yedeklemeler için dış paylaşımı da otomatik olarak temizler. Daha fazla bilgi için [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](.\.\azure-stack-backup-enable-backup-powershell.md).

- <!-- 2496385 | ASDK, IS -->  **Eklenen veri aktarımı için toplam yedek zamanına zaman.** Daha fazla bilgi için [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](.\.\azure-stack-backup-enable-backup-powershell.md).

-   <!-- 1702130 | ASDK, IS -->  **Yedekleme dış kapasite artık dış paylaşım doğru kapasitesini gösterir.** (Daha önce bu kodu sabit-10 GB.) Daha fazla bilgi için [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](.\.\azure-stack-backup-enable-backup-powershell.md).
 
- <!-- 2753130 |  IS, ASDK   -->  **Azure Resource Manager şablonları artık destek koşulu öğesi** -artık bir kaynak bir koşulunu kullanarak bir Azure Kaynak Yöneticisi şablonu olarak dağıtabilirsiniz. Bir parametre değeri varsa, değerlendirme gibi bir koşula bağlı olarak kaynak dağıtma için şablonunuzu tasarlayabilirsiniz. Bir koşul olarak bir şablon kullanma hakkında daha fazla bilgi için bkz: [koşullu olarak kaynak dağıtma](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) ve [değişkenler bölümü Azure Resource Manager şablonlarının](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) Azure belgelerinde. 

   Şablonlar için de kullanabilirsiniz [birden fazla abonelik veya kaynak grubu için kaynakları dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment).  

- <!--2753073 | IS, ASDK -->  **Microsoft.Network API kaynak sürüm desteği güncelleştirildi** API Sürüm 2017-10-01 2015-06-15 arasında Azure Stack ağ kaynakları için desteği eklenecek.  2017-10-01 2015-06-15 arasındaki kaynak sürümleri desteği bu sürümde bulunmayan, ancak gelecekteki bir sürümde eklenecek.  Lütfen [Azure Stack ağ iletişimi için Değerlendirmeler](.\.\user\azure-stack-network-differences.md) işlev farklılıkları için.

- <!-- 2272116 | IS, ASDK   -->  **Azure Stack, harici olarak Azure Stack altyapı uç noktalarına yönelik için ters DNS araması için destek ekledi** (değil portal, adminportal, yönetim ve adminmanagement için). Bu, bir IP adresinden çözülmesi Azure Stack dış uç nokta adları sağlar.

- <!-- 2780899 |  IS, ASDK   --> **Azure Stack artık mevcut bir VM'ye ek ağ arabirimi eklenmesine izin verir.**  Bu işlev, portal, PowerShell ve CLI kullanarak kullanılabilir. Daha fazla bilgi için [ekleme veya kaldırma ağ arabirimleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) Azure belgeleri. 

- <!-- 2222444 | IS, ASDK   -->  **Kullanım ölçümleri ağ doğruluk ve dayanıklılık geliştirmeleri yapıldı**. Ağ kullanım ölçümleri, artık daha doğru ve hesabı askıya alınmış Abonelikleri, kesinti süreleri ve yarış durumları alın.

- <!-- 2297790 | IS, ASDK -->  **Azure Stack syslog istemciye (Önizleme özelliği) geliştirmeleri**. Bu istemci, Denetim ve Azure Stack altyapısının bir syslog sunucusu veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılım Azure Stack'e dış ilgili günlükleri iletilmesini sağlar. Syslog istemci, artık düz metin veya varsayılan yapılandırmayı ikinci olan, TLS 1.2 şifrelemeyi TCP protokolünü destekler. TLS bağlantısını yalnızca sunucu ya da karşılıklı kimlik doğrulaması ile yapılandırabilirsiniz.

  Syslog istemci (protokol, şifreleme ve kimlik doğrulaması gibi) nasıl iletişim kurduğu syslog sunucusu ile yapılandırmak için Set-SyslogServer cmdlet'ini kullanın. Bu cmdlet, ayrıcalıklı uç noktasından (CESARETLENDİRİCİ) kullanılabilir. 

  Syslog istemci TLS 1.2 karşılıklı kimlik doğrulaması için istemci tarafı sertifika eklemek için CESARETLENDİRİCİ kümesi SyslogClient cmdlet'ini kullanın.

  Bu önizleme ile çok fazla sayıda denetimleri ve uyarılar görebilirsiniz. 

  Bu özellik hala Önizleme aşamasında olduğundan, üzerinde üretim ortamlarına güvenmeyin.

  Daha fazla bilgi için [Azure Stack syslog iletmeyi](.\.\azure-stack-integrate-security.md).

- <!-- ####### | IS, ASDK -->  **Azure Resource Manager, bölge adını içerir.** Bu sürümle birlikte, Azure Resource Manager'dan alınan nesneler artık bölge adı özniteliği içerir. Varolan bir PowerShell komut dosyasını doğrudan nesne başka bir cmdlet'e geçerse, betik hataya neden ve başarısız. Bu, Azure Resource Manager uyumlu davranıştır ve bölge öznitelik çıkarılacak çağıran istemci gerektirir. Azure Resource Manager bakın hakkında daha fazla bilgi için [Azure Resource Manager belgeleri](https://docs.microsoft.com/azure/azure-resource-manager/).

- <!-- TBD | IS, ASDK -->  **Abonelikler, temsilci sağlayıcılar arasında taşıyın.** Aynı dizin kiracısına ait yeni veya var olan bir temsilci sağlayıcısı abonelikler arasında abonelikler artık taşıyabilirsiniz. Varsayılan sağlayıcı aboneliğine ait abonelikleri de aynı dizin kiracısında temsilci sağlayıcı aboneliği için taşınabilir. Daha fazla bilgi için [temsilci sunan Azure Stack'te](.\.\azure-stack-delegated-provider.md).
 
- <!-- 2536808 IS ASDK --> **VM oluşturma zamanı geliştirilmiş** görüntüleri Azure Market'te indir ile oluşturulan sanal makineleri için.

### <a name="fixed-issues"></a>Düzeltilen sorunlar

- <!-- TBD | ASDK, IS --> Çeşitli iyileştirmeler, daha güvenilir hale getirmek için güncelleştirme işlemi için yapılmıştır. Ayrıca, düğüm boşaltma, böylece güncelleştirme sırasında iş yükleri için olası kapalı kalma süresini en aza artıran altyapının düzeltmeler yapılmıştır.

-   <!--2292271 | ASDK, IS --> Burada değiştirilmiş bir kota sınırı mevcut aboneliklerin uygulamadı sorununu düzelttik.  Artık, bir teklif parçası olan bir ağ kaynağı için bir kota sınırı artırmak ve kullanıcı abonelikle planı, önceden mevcut abonelikler, aynı zamanda için yeni abonelikler yeni sınır geçerlidir.

- <!-- 2448955 | IS ASDK --> Etkinlik günlükleri bir UTC + N saat diliminde dağıtılan sistemler için artık başarılı bir şekilde sorgulayabilirsiniz.    

- <!-- 2319627 |  ASDK, IS --> Yedekleme yapılandırması parametreleri (yol/kullanıcı adı/parola/şifreleme anahtarı) için ön denetim, artık için yedekleme yapılandırması ayarları yanlış ayarlar. (Daha önce yedeklemeyi ayarları yanlış ayarlanmış ve yedekleme ardından tetiklendiğinde başarısız olur.)

- <!-- 2215948 |  ASDK, IS --> Dış paylaşımından el ile yedekleme sildiğinizde yedekleme listesinde artık yeniler.

- <!-- 2360715 |  ASDK, IS -->  Bundan sonra veri merkezi tümleştirmeyi ayarladığınızda, AD FS meta veri dosyası bir paylaşımdan erişemezsiniz. Daha fazla bilgi için [Federasyon meta veri dosyası sağlayarak AD FS tümleştirmenin ayarlanması](.\.\azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

- <!-- 2388980 | ASDK, IS --> Bir sorunu düzelttik döndürülmesiyle kullanıcılardan var olan bir genel IP adresi atanmış, daha önce bir ağ arabirimiyle ya da yeni bir ağ arabirimiyle ya da yük dengeleyici için yük dengeleyici için atanmış.  

- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, *genel bakış* Yöneticisi ya da Kullanıcı Portalı'nda depolama hesabı için *Essentials* bölmesi artık görüntüler beklenen tüm bilgilerin doğru. 

- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, *etiketleri* için bir depolama hesabı yönetici veya Kullanıcı Portalı'nda, bilgileri artık doğru görüntüler.

- <!-- TBD - IS ASDK --> Azure Stack bu sürümü, sürücü güncelleştirmelerini uygulamadan OEM uzantı paketleri önleyen sorunu giderir.

-   <!-- 2055809- IS ASDK --> VM'nin oluşturulması başarısız oldu. işlem dikey penceresinden Vm'leri silmesi önleyen bir sorunu düzelttik.  

- <!--  2643962 IS ASDK -->  Uyarı *düşük bellek kapasitesi* artık yanlış görüntülenir.

- <!--  TBD ASDK --> Ayrıcalıklı uç noktasını (CESARETLENDİRİCİ) barındıran sanal makine için 4 GB arttı. ASDK bu sanal makine AzS-ERCS01 olarak adlandırılır.

- **Çeşitli düzeltmeleri** performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi


<!-- ### Changes  -->
<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Bilinen sorunlar

#### <a name="portal"></a>Portal  
- <!-- 2931230 – IS  ASDK --> Kullanıcı aboneliği plan kaldırdığınızda bile, bir kullanıcı abonelikte eklenti planı eklendiği planları silinemiyor. Eklenti planı başvuru abonelikleri de silinene kadar plan kalır. 

- <!--2760466 – IS  ASDK --> Bu sürümünü çalıştıran yeni bir Azure Stack ortamına yüklediğinizde, uyarıyı gösterir *etkinleştirme gerekli* görüntülenmeyebilir. [Etkinleştirme](.\.\azure-stack-registration.md) Market dağıtım kullanabilmeniz için gereklidir. 

- <!-- TBD - IS ASDK --> İki Yönetim abonelik türlerini [1804 sürümü ile sunulan](.\.\azure-stack-update-1804.md#new-features) kullanılmamalıdır. Abonelik türleridir **abonelik ölçümü**, ve **tüketim abonelik**. Bu abonelik türleri **abonelik ölçümü**, ve **tüketim abonelik**. Bu abonelik türlerini 1804 sürümünden başlayarak yeni Azure Stack ortamlarında görülebilir ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir **varsayılan sağlayıcı aboneliği** türü.

- <!-- 2403291 - IS ASDK --> Yatay kaydırma çubuğunun alt kısmında bulunan yönetici ve Kullanıcı Portalı'nın kullanımını olmayabilir. Yatay kaydırma çubuğunun erişemiyorsanız, önceki bir dikey pencere portaldaki dikey penceresinin adı seçerek, gitmek için içerik haritaları en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.
  ![İçerik haritası](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

- <!-- TBD -  IS ASDK --> Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

- <!--  TBD | ASDK -->  Azure Stack dağıtımınıza için varsayılan saat dilimini artık UTC'ye ayarlar. Ancak, bunu otomatik olarak UTC'ye varsayılan olarak yükleme sırasında dönecek Azure Stack, yükleme sırasında bir saat dilimi seçebilirsiniz.

#### <a name="health-and-monitoring"></a>Sistem durumu ve izleme
- <!-- 1264761 - IS ASDK -->  Uyarıları görebilirsiniz *sistem durumu denetleyicisi* aşağıdaki ayrıntıları olan bir bileşeni:  

   Uyarı #1:
   - ADI: Sağlıksız altyapı rolü
   - Önem DERECESİ: uyarı
   - BİLEŞENİ: Sistem durumu denetleyicisi
   - Açıklama: Sistem durumu denetleyici sinyal tarayıcı kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.  

  Uyarı #2:
   - ADI: Sağlıksız altyapı rolü
   - Önem DERECESİ: uyarı
   - BİLEŞENİ: Sistem durumu denetleyicisi
   - Açıklama: Hata tarayıcı durumu denetleyicisi kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.

  Her iki uyarılar güvenle yoksayılabilir ve zaman içinde otomatik olarak kapatılacak.  

- <!-- 2368581 - IS. ASDK --> Düşük bellek uyarısı alırsanız ve Kiracı sanal makinelerini başarısız ile dağıtmak bir Azure Stack operatörü bir *Fabric VM oluşturma hatası*, Azure Stack damga kullanılabilir bellek yetersiz olabilir. Kullanım [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) iş yükleriniz için kapasite en iyi anlamak için.

- <!-- TBD - IS. ASDK --> Çalıştırırken **Test AzureStack** ayrıcalıklı uç noktası (CESARETLENDİRİCİ) cmdlet'ini **Azure Stack altyapısını rol örneği performans** test ERCS VM için bir uyarı iletisi üretir. Güvenli bir şekilde UYAR iletiyi yoksayıp ASDK kullanmaya devam edin.

#### <a name="compute"></a>İşlem
- <!-- 2494144 - IS, ASDK --> Bir sanal makine dağıtımı için bir sanal makine boyutu seçerken, bazı F serisi VM boyutlarının görünür olmayan bir VM oluşturduğunuzda boyut seçici bir parçası olarak. Aşağıdaki VM boyutları seçicide görünmez: *F8s_v2*, *F16s_v2*, *F32s_v2*, ve *F64s_v2*.  
  Geçici bir çözüm olarak, bir VM'yi dağıtmak için aşağıdaki yöntemlerden birini kullanın. Her bir yöntemin, kullanmak istediğiniz VM boyutu belirtmeniz gerekir.

  - **Azure Resource Manager şablonu:** bir şablon kullandığınızda *vmSize* şablonunda kullanmak istediğiniz VM boyutuna eşit olur. Örneğin, şu girişi kullanan bir VM dağıtmak için kullanılan *F32s_v2* boyutu:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI:** kullanabileceğiniz [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) benzer bir parametre olarak bir VM boyutu belirtin ve komutu `--size "Standard_F32s_v2"`.

  - **PowerShell:** kullanabileceğiniz PowerShell ile [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) benzer VM boyutunu belirten parametresiyle `-VMSize "Standard_F32s_v2"`.


- <!-- TBD -  IS ASDK --> Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, D serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. Tüm desteklenen D serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

- <!-- TBD -  IS ASDK --> Bir VM görüntüsü oluşturulması başarısız olduğunda, nelze odstranit başarısız bir öğe için VM görüntüleri işlem dikey eklenebilir.

  Geçici çözüm olarak, Hyper-V ile oluşturduğunuz işlevsiz bir VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yolu C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silme engelleyen sorunu çözer. Ardından, işlevsiz bir görüntü oluşturduktan sonra 15 dakika başarıyla silebilirsiniz.

  Ardından, daha önce başarısız olan VM görüntüsünün indirilmesi yeniden deneyebilir.

- <!-- TBD -  IS ASDK --> VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

- <!-- 1662991 - IS ASDK --> Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.

- <!-- 2724961- IS ASDK --> Kaydettiğinizde **Microsoft.Insight** abonelik ayarlarını, kaynak sağlayıcısındaki bir Windows VM oluşturma ve konuk işletim sistemi etkin Tanılama ile VM genel bakış sayfasında ölçüm verilerini da göstermez. 

   Sanal makine için CPU yüzdesi grafik gibi ölçüm verileri bulmak için Git **ölçümleri** dikey penceresinde ve desteklenen tüm Windows VM show Konuk ölçümleri.

#### <a name="networking"></a>Ağ
- <!-- 1766332 - IS, ASDK --> Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

- <!-- 1902460 -  IS ASDK --> Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

- <!-- 16309153 -  IS ASDK --> DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.

- <!-- 2702741 -  IS ASDK --> Dinamik ayırma yöntemi kullanılarak dağıtılan genel IP'ler garanti edilmez durdurun-serbest verildiği sonra korunması.

- <!-- 2529607 - IS ASDK --> Azure Stack sırasında *gizli dönüş*, bir süre içinde genel IP adresleri olan erişilemeyen iki ila beş dakikalığına yoktur.

-   <!-- 2664148 - IS ASDK --> Bir S2S VPN tüneli aracılığıyla Kiracı sanal makinelerini burada erişiyor senaryolarda, yerel ağ geçidi için şirket içi alt ağ geçidi zaten oluşturulduktan sonra eklenmişse nerede bağlantı girişimleri başarısız bir senaryo hatalarla karşılaşabilirsiniz. 


#### <a name="sql-and-mysql"></a>SQL ve MySQL
- <!-- TBD - ASDK --> Veritabanını barındıran sunucu kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan bir örnek kullanılamaz.

- <!-- IS, ASDK --> Özel karakterler, boşluk ve nokta dahil olmak üzere desteklenmez **ailesi** adı, SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda.

#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- <!-- TBD - IS ASDK --> (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.  

- <!-- TBD - IS ASDK --> App Service yalnızca dağıtılabilir içine *varsayılan sağlayıcı aboneliği* şu anda. App Service gelecek bir güncelleştirmede yeni dağıtacağınız *abonelik ölçümü* Azure Stack 1804 ' tanıtılmıştır. Ölçüm kullanım için desteklenir, bu yeni bir abonelik türü için tüm mevcut dağıtımları geçirilecektir.

#### <a name="usage"></a>Kullanım  
- <!-- TBD -  IS ASDK --> Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

<!-- #### Identity -->






## <a name="build-11805147"></a>Derleme 1.1805.1.47

> [!TIP]  
> Müşteri geri bildirimi doğrultusunda, Microsoft Azure Stack için kullanılan sürüm şema için bir güncelleştirme yok. Bu güncelleştirmeyle, 1805, başlangıç yeni şemayı daha iyi geçerli bulut sürümü temsil eder.  
>
> Sürüm şeması sunulmuştur *Version.YearYearMonthMonth.MinorVersion.BuildNumber* ikinci ve üçüncü kümeleri nerede göstermek sürüm ve sürüm. Örneğin, 1805.1 temsil *yayın için üretim* 1805 sürümü (RTM).  


### <a name="new-features"></a>Yeni Özellikler
Bu derleme, Azure Stack için aşağıdaki geliştirmeleri ve düzeltmeleri içerir.  

- <!-- 2297790 - IS, ASDK --> **Azure Stack artık içeren bir *Syslog* istemci** olarak bir *önizleme özelliği*. Bu istemci, Azure Stack için dış bir Syslog sunucusu veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılımı için Azure Stack altyapısının ilgili denetim ve güvenlik günlüklerini iletilmesini sağlar. Şu anda, Syslog istemci varsayılan bağlantı noktası 514'üzerinden yalnızca kimliği doğrulanmamış UDP bağlantıyı destekler. Her Syslog ileti yükünü Common Event Format (CEF) olarak biçimlendirilir.

  Syslog istemcisini yapılandırmak için kullanın **kümesi SyslogServer** ayrıcalıklı uç noktasında cmdlet'i.

  Bu önizlemeyle, aşağıdaki üç uyarılar görebilirsiniz. Azure yığını tarafından sunulan, bu uyarılar dahil *açıklamaları* ve *düzeltme* Kılavuzu.
  - Başlık: Kod bütünlüğü devre dışı  
  - Başlık: Denetim modunda kod bütünlüğü
  - Başlık: Oluşturulan kullanıcı hesabı

  Bu özellik Önizleme aşamasında olduğu sürece, bu bağlı üretim ortamlarında dayanan olmamalıdır.   


### <a name="fixed-issues"></a>Düzeltilen sorunlar
- Sorunu engellenen düzelttik [açılan listeden yeni bir destek isteği açma](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen Yönetici portalındaki. Artık bu seçenek, beklendiği gibi çalışır.

- <!--  TBD ASDK --> Ayrıcalıklı uç noktasını (CESARETLENDİRİCİ) barındıran sanal makine için 4 GB arttı. ASDK bu sanal makine AzS-ERCS01 olarak adlandırılır.

- **Çeşitli düzeltmeleri** performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi


<!-- ### Changes  -->


<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Bilinen sorunlar

#### <a name="portal"></a>Portal
- <!-- 2931230 – IS  ASDK --> Kullanıcı aboneliği plan kaldırdığınızda bile, bir kullanıcı abonelikte eklenti planı eklendiği planları silinemiyor. Eklenti planı başvuru abonelikleri de silinene kadar plan kalır. 

- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **genel bakış** için yönetici veya Kullanıcı Portalı, bilgileri bir depolama hesabında *Essentials* bölmesinde görüntülemez.  Temel bileşenler bölmesine gibi hesabıyla ilgili bilgileri görüntüler, *kaynak grubu*, *konumu*, ve *abonelik kimliği*.  Diğer seçenekleri genel bakış için erişilebilir gibi *Hizmetleri* ve *izleme*, farklı seçenekleri için *Gezgini'nde Aç* veya *depolama hesabını Sil* .  

  Kullanılamayan bilgileri görüntülemek için kullanın [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell cmdlet'i.

- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **etiketleri** Yöneticisi ya da Kullanıcı Portalı'nda depolama hesabı için bilgiler yüklenemiyor ve görüntülenmez.  

  Kullanılamayan bilgileri görüntülemek için kullanın [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) PowerShell cmdlet'i.

- <!-- TBD - IS ASDK --> Yeni Yönetim abonelik türlerini kullanmayın *abonelik ölçümü*, ve *tüketim abonelik*. Bu yeni abonelik türleriyle 1804 sürümüyle kullanıma sunulan ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir *varsayılan sağlayıcı* abonelik türü.  
- <!-- TBD - IS ASDK --> OEM uzantı paketinin bu sürümü Azure Stack ile kullanarak, sürücü güncelleştirmelerini uygulayamazsınız.  Bu sorun için geçici çözüm yoktur.
 
- <!-- TBD - IS ASDK --> Özelliği [açılan listeden yeni bir destek isteği açmak için](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen içinde Yönetici portalı kullanılamıyor. Bunun yerine, aşağıdaki bağlantıyı kullanın:     
    - Azure Stack Geliştirme Seti için kullanmak https://aka.ms/azurestackforum.    

- <!-- 2403291 - IS ASDK --> Yatay kaydırma çubuğunun alt kısmında bulunan yönetici ve Kullanıcı Portalı'nın kullanımını olmayabilir. Yatay kaydırma çubuğunun erişemiyorsanız, önceki bir dikey pencere portaldaki dikey penceresinin adı seçerek, gitmek için içerik haritaları en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.
  ![İçerik haritası](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

- <!-- TBD -  IS ASDK --> Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.


#### <a name="health-and-monitoring"></a>Sistem durumu ve izleme
- <!-- 1264761 - IS ASDK -->  Uyarıları görebilirsiniz *sistem durumu denetleyicisi* aşağıdaki ayrıntıları olan bir bileşeni:  

   Uyarı #1:
   - ADI: Sağlıksız altyapı rolü
   - Önem DERECESİ: uyarı
   - BİLEŞENİ: Sistem durumu denetleyicisi
   - Açıklama: Sistem durumu denetleyici sinyal tarayıcı kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.  

  Uyarı #2:
   - ADI: Sağlıksız altyapı rolü
   - Önem DERECESİ: uyarı
   - BİLEŞENİ: Sistem durumu denetleyicisi
   - Açıklama: Hata tarayıcı durumu denetleyicisi kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.

    Her iki uyarı #1 ve 2 # güvenle yoksayılabilir ve zaman içinde otomatik olarak kapatılması. 

  Aşağıdaki uyarı da görebilirsiniz *kapasite*. Bu uyarı için bir açıklama tanımlanan kullanılabilir bellek yüzdesini değişebilir:  

  Uyarı #3:
   - ADI: Düşük bellek kapasitesi
   - Önem DERECESİ: kritik
   - Bileşen: kapasite
   - Açıklama: Birden fazla %80.00 kullanılabilir bellek bölgesini tüketti. Büyük miktarlarda bellek ile sanal makine oluşturma başarısız olabilir.  

  Azure Stack bu sürümünde, yanlış bu uyarıyı tetikleyebilir. Kiracı sanal makineleri başarılı bir şekilde dağıtmak devam ederseniz, bu uyarıyı güvenle yok sayabilirsiniz. 
  
  Uyarı #3 otomatik olarak kapatmaz. Bu uyarıyı kapatırsanız, Azure Stack 15 dakika içinde aynı uyarı oluşturur.  

- <!-- 2368581 - IS. ASDK --> Düşük bellek uyarısı alırsanız ve Kiracı sanal makinelerini başarısız ile dağıtmak bir Azure Stack operatörü bir *Fabric VM oluşturma hatası*, Azure Stack damga kullanılabilir bellek yetersiz olabilir. Kullanım [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) iş yükleriniz için kapasite en iyi anlamak için.

- <!-- TBD - IS. ASDK --> Test AzureStack cmdlet (CESARETLENDİRİCİ) ayrıcalık uç noktada çalıştırılırken ERCS VM için bir uyarı iletisi oluşturur. ASDK kullanmaya devam edebilirsiniz. 


#### <a name="compute"></a>İşlem
- <!-- TBD - IS, ASDK --> Bir sanal makine dağıtımı için bir sanal makine boyutu seçerken, bazı F serisi VM boyutlarının görünür olmayan bir VM oluşturduğunuzda boyut seçici bir parçası olarak. Aşağıdaki VM boyutları seçicide görünmez: *F8s_v2*, *F16s_v2*, *F32s_v2*, ve *F64s_v2*.  
  Geçici bir çözüm olarak, bir VM'yi dağıtmak için aşağıdaki yöntemlerden birini kullanın. Her bir yöntemin, kullanmak istediğiniz VM boyutu belirtmeniz gerekir.

  - **Azure Resource Manager şablonu:** bir şablon kullandığınızda *vmSize* şablonunda kullanmak istediğiniz VM boyutuna eşit olur. Örneğin, şu girişi kullanan bir VM dağıtmak için kullanılan *F32s_v2* boyutu:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI:** kullanabileceğiniz [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) benzer bir parametre olarak bir VM boyutu belirtin ve komutu `--size "Standard_F32s_v2"`.

  - **PowerShell:** kullanabileceğiniz PowerShell ile [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) benzer VM boyutunu belirten parametresiyle `-VMSize "Standard_F32s_v2"`.


- <!-- TBD -  IS ASDK --> Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, D serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. Tüm desteklenen D serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

- <!-- TBD -  IS ASDK --> Bir VM görüntüsü oluşturulması başarısız olduğunda, nelze odstranit başarısız bir öğe için VM görüntüleri işlem dikey eklenebilir.

  Geçici çözüm olarak, Hyper-V ile oluşturduğunuz işlevsiz bir VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yolu C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silme engelleyen sorunu çözer. Ardından, işlevsiz bir görüntü oluşturduktan sonra 15 dakika başarıyla silebilirsiniz.

  Ardından, daha önce başarısız olan VM görüntüsünün indirilmesi yeniden deneyebilir.

- <!-- TBD -  IS ASDK --> VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

- <!-- 1662991 - IS ASDK --> Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.

#### <a name="networking"></a>Ağ
- <!-- TBD - IS ASDK --> Yönetici veya Kullanıcı Portalı ya da kullanıcı tanımlı yollar oluşturamazsınız. Geçici çözüm olarak, [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS, ASDK --> Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

- <!-- 2388980 -  IS ASDK --> Bir VM oluşturulur ve bir genel IP adresiyle ilişkili sonra o VM'den bir IP adresi ilişkisini olamaz. İlişkisi kaldırma çalışıyor gibi görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu durumda (genellikle olarak adlandırılan bir *VIP takas*). Tüm gelecek bu IP adresi sonucu özgün VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.

- <!-- 2292271 - IS ASDK --> Bir teklif ve Kiracı abonelikle ilişkilendirilmiş planı parçası olan bir ağ kaynağı için bir kota sınırı yükseltirseniz, bu abonelik için yeni sınır uygulanmaz. Ancak, yeni sınır kota artırılır sonra oluşturulan yeni abonelikler için geçerlidir.

  Bu sorunu gidermek için plan aboneliği ile ilişkili olduğunda, bir ağ Kotayı artırmak için bir eklenti planı kullanın. Daha fazla bilgi için bkz. nasıl [eklenti planı kullanılabilmesini](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> DNS bölgesi veya yol tablosu kaynaklar ile ilişkili olan bir abonelik silinemiyor. Abonelik başarıyla silmek için DNS bölgesi ve yol tablosu kaynakları Kiracı abonelikten silmeniz gerekir.


- <!-- 1902460 -  IS ASDK --> Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

- <!-- 16309153 -  IS ASDK --> DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.

- <!-- TBD -  IS ASDK --> Azure Stack, VM dağıtıldıktan sonra ek ağ arabirimi bir sanal makine örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.


#### <a name="sql-and-mysql"></a>SQL ve MySQL
- <!-- TBD - ASDK --> Veritabanını barındıran sunucu kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan bir örnek kullanılamaz.

- <!-- IS, ASDK --> Özel karakterler, boşluk ve nokta dahil olmak üzere desteklenmez **ailesi** adı, SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda.

#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- <!-- TBD - IS ASDK --> (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.  

- <!-- TBD - IS ASDK --> App Service yalnızca dağıtılabilir içine *varsayılan sağlayıcı aboneliği* şu anda. <!-- In a future update, App Service will deploy into the new *Metering subscription* that was introduced in Azure Stack 1804. When Metering is supported for use, all existing deployments will be migrated to this new subscription type. -->

#### <a name="usage"></a>Kullanım  
- <!-- TBD -  IS ASDK --> Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

<!-- #### Identity -->

