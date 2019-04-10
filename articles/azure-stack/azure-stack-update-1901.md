---
title: Azure Stack 1901 güncelleştirmesi | Microsoft Docs
description: Yenilikler dahil olmak üzere, Azure Stack tümleşik sistemleri 1901 güncelleştirmesi hakkında bilinen sorunlar ve güncelleştirmeyi yüklemek nereye öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2019
ms.author: sethm
ms.reviewer: adepue
ms.lastreviewed: 03/27/2019
ms.openlocfilehash: cd07ff5beddf65c9788c9ba94802ba2d37172923
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59360689"
---
# <a name="azure-stack-1901-update"></a>Azure Stack 1901 güncelleştirme

*Şunlara uygulanır Azure Stack tümleşik sistemleri*

Bu makalede 1901 güncelleştirme paketinin içeriğini açıklar. Güncelleştirme geliştirmeleri ve düzeltmeleri bu sürümü, Azure Stack için yeni özellikler içerir. Bu makalede ayrıca bu sürümdeki bilinen sorunlara açıklar ve güncelleştirmeyi indirmek için bir bağlantı içerir. Bilinen sorunlar doğrudan güncelleştirme işlemiyle ilgili sorunları ve yapı (yükleme sonrası) ile ayrılır.

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1901 güncelleştirmenin yapı numarasıdır **1.1901.0.95** veya **1.1901.0.99** 26 Şubat 2019 sonra. Aşağıdaki nota bakın:

> [!IMPORTANT]  
> Microsoft için 1901 1811 (1.1811.0.101) ' güncelleştirme müşteriler etkileyen bir sorun bulduğunda ve sorunu gidermek için güncelleştirilmiş bir 1901 paket kullanıma sundu: 1.1901.0.95 güncelleştirilmiş 1.1901.0.99, derleme. Zaten 1.1901.0.95 için güncelleştirilmiş müşteriler başka bir işlem yapmanıza gerek yoktur.
>
> Üzerinde 1811 olan bağlı müşteriler Yönetici portalı'nda kullanılabilir yeni 1901 (1.1901.0.99) paketi otomatik olarak görür ve hazır olduğunuzda yüklemeniz. Bağlantısı kesilmiş müşteriler indirebilir ve aynı işlemi kullanarak yeni 1901 paketini içeri aktarma [burada açıklanan](azure-stack-apply-updates.md).
>
> Sonraki tam veya düzeltme paketi yüklerken 1901 her iki sürümü ile müşteriler etkilenmeyecektir.

## <a name="hotfixes"></a>Düzeltmeler

Azure Stack düzeltmeleri düzenli olarak serbest bırakır. Yüklediğinizden emin olun [en son Azure Stack düzeltme](#azure-stack-hotfixes) Azure Stack için 1901 güncelleştirmeden önce 1811 için.

Azure Stack düzeltmeleri yalnızca Azure Stack tümleşik sistemleri için geçerlidir; üzerinde ASDK düzeltmelerini çalışmayın.

> [!TIP]  
> Aşağıdaki abone olmak *RSS* veya *Atom* Azure Stack düzeltmelerle birlikte kalmasını sağlamak için akışları:
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

### <a name="azure-stack-hotfixes"></a>Azure Stack düzeltmeler

Zaten sahip olduğunuz 1901 ve tüm düzeltmeler yüklü değil henüz yapabilecekleriniz [1902 doğrudan yükleme](azure-stack-update-1902.md), ilk 1901 düzeltmenin.

- **1809**: [KB 4481548 – Azure Stack düzeltme 1.1809.12.114](https://support.microsoft.com/help/4481548/)
- **1811**: Geçerli düzeltme yok.
- **1901**: [KB 4495662 – Azure Stack düzeltme 1.1901.3.105](https://support.microsoft.com/help/4495662)

## <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> Yükleme [en son Azure Stack düzeltme](#azure-stack-hotfixes) 1901 için güncelleştirmeden önce 1811 (varsa) için. Zaten sahip olduğunuz 1901 ve düzeltmeleri henüz yüklemediyseniz, 1901 düzeltme yüklemeden 1902 doğrudan yükleyebilirsiniz.

- Bu güncelleştirme yüklemesi başlamadan önce çalıştırması [Test AzureStack](azure-stack-diagnostic-test.md) bulunan tüm çalışma sorunlarını çözün ve Azure Stack durumunu doğrulamak için aşağıdaki parametreleri, tüm uyarılar ve hatalar dahil olmak üzere. Ayrıca etkin Uyarıları gözden geçirin ve eylemi gerektiren tüm çözümleyin:

    ```powershell
    Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary, AzsHostingServiceCertificates
    ```

- Azure Stack System Center Operations Manager (SCOM) tarafından yönetildiğinde 1901 uygulamadan önce Yönetim Paketi için Microsoft Azure Stack 1.0.3.11 sürümüne güncelleştirdiğinizden emin olun.

## <a name="new-features"></a>Yeni Özellikler

Bu güncelleştirme, aşağıdaki yeni özellikleri ve Azure Stack için geliştirmeler içerir:

- Azure Stack etkinleştir yönetilen görüntülerinde sunulacağından Vm'leri, yalnızca yönetilen oluşturabilirsiniz (hem de yönetilmeyen ve yönetilen) bir genelleştirilmiş sanal makine üzerinde bir yönetilen bir görüntü nesnesi oluşturmak için disk. Daha fazla bilgi için [Azure Stack yönetilen diskler](user/azure-stack-managed-disk-considerations.md#managed-images).

- **AzureRm 2.4.0**
   * **AzureRm.Profile**  
         Hata düzeltmesi - `Import-AzureRmContext` kaydedilen belirteç doğru bir şekilde seri durumdan çıkarılacak.  
   * **AzureRm.Resources**  
         Hata düzeltmesi - `Get-AzureRmResource` sorgu çalışmasına insensitively kaynak türüne göre.  
   * **Azure Depolama**  
         AzureRm toplama modülü artık içerir zaten yayımlanmış sürüm 4.5.0 destekleyen **API Sürüm 2017-07-29**.  
   * **AzureRm.Storage**  
         AzureRm toplama modülü artık içerir zaten yayımlanmış sürüm 5.0.4 destekleyen **API Sürüm 2017-10-01**.  
   * **AzureRm.Compute**  
         Eklenen basit parametre kümelerine `New-AzureRmVM` ve `New-AzureRmVmss`, `-Image` parametresini belirten kullanıcı görüntüleri destekler.  
   * **AzureRm.Insights**  
         AzureRm toplama modülü artık içerir zaten yayımlanmış sürüm 5.1.5 destekleyen **api sürümü 2018-01-01** ölçümler, ölçüm tanımlarını kaynak türleri için.

- **AzureStack 1.7.1** bu önemli bir değişiklik bırakın. Hataya neden olan değişikliklerin ayrıntıları için bkz. https://aka.ms/azspshmigration171
   * **Azs.Backup.Admin Modülü**  
         Yeni değişiklik: Sertifika tabanlı şifreleme modundaki değişiklikleri yedekleme. Simetrik anahtarlar için destek kullanım dışı bırakıldı.  
   * **Azs.Fabric.Admin Modülü**  
         `Get-AzsInfrastructureVolume` kullanım dışıdır. Yeni cmdlet kullanma `Get-AzsVolume`.  
         `Get-AzsStorageSystem` kullanım dışıdır.  Yeni cmdlet kullanma `Get-AzsStorageSubSystem`.  
         `Get-AzsStoragePool` kullanım dışıdır. `StorageSubSystem` Nesne kapasite özelliği içerir.  
   * **Azs.Compute.Admin modülü**  
         Hata düzeltmesi - `Add-AzsPlatformImage`, `Get-AzsPlatformImage`: Çağırma `ConvertTo-PlatformImageObject` yalnızca başarı yolunda.  
         BugFix - `Add-AzsVmExtension`, `Get-AzsVmExtension`: Yalnızca başarı yolunda ConvertTo-VmExtensionObject çağrılıyor.  
   * **Azs.Storage.Admin Modülü**  
         Hata düzeltmesi - yeni depolama kotası yoksa sağlanan varsayılan değerleri kullanır.

Güncelleştirilmiş modüller için başvuru incelemesi için bkz: [Azure Stack modül başvurusu](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.6.0&viewFallbackFrom=azurestackps-1.7.0).

## <a name="fixed-issues"></a>Düzeltilen sorunlar

- Portal, Azure Stack'te desteklenmeyen ilke tabanlı VPN ağ geçitleri oluşturma seçeneğiniz olduğunu gösterdi bir sorun düzeltildi. Bu seçenek, Portalı'ndan kaldırıldı.

<!-- 16523695 – IS, ASDK -->
- Bir sorun, sanal ağınızın DNS ayarlarınızı güncelleştirdikten sonra sabit **Azure Stack DNS'yi** için **özel DNS**, örnekler yeni ayarlarla güncelleştirilmedi.

- <!-- 3235634 – IS, ASDK -->
  İçeren boyutlarıyla dağıtma hangi VM'ler içinde bir sorun düzeltildi bir **v2** soneki; Örneğin, **işler için standart_a2_v2**, gerekli olarak sonekini belirten **işler için standart_a2_v2** () küçük harf v). Küresel Azure sayesinde, artık kullanabilirsiniz gibi **işler için standart_a2_v2** (Büyük Harf V).

<!--  2795678 – IS, ASDK --> 
- Bir premium VM boyutu (DS, Ds_v2, FS, FSv2) sanal makineleri (VM'ler) oluşturmak için portalı kullanıldığında bir uyarı üreten bir sorun düzeltildi. VM bir standart depolama hesabı oluşturulur. Bu işlevsellik, IOPS ve faturalandırma etkilemeyen olsa da, uyarı düzeltilmiştir.

<!-- 1264761 - IS ASDK -->  
- İle ilgili bir sorun düzeltildi **sistem durumu denetleyicisi** aşağıdaki uyarıları oluşturan bileşen. Uyarıları göz ardı edilebilir:

    - Uyarı #1:
       - ADI:  Sağlıksız altyapı rolü
       - ÖNEM DERECESİ: Uyarı
       - BİLEŞEN: Denetleyici sistem durumu
       - AÇIKLAMA: Sistem durumu denetleyici sinyal tarayıcı kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.  

    - Uyarı #2:
       - ADI:  Sağlıksız altyapı rolü
       - ÖNEM DERECESİ: Uyarı
       - BİLEŞEN: Denetleyici sistem durumu
       - AÇIKLAMA: Sistem durumu denetleyicisi hata tarayıcı kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.


<!-- 3507629 - IS, ASDK --> 
- Yönetilen diskler kotalar altında değeri ayarlanırken bir sorun düzeltildi [kota türleri işlem](azure-stack-quota-types.md#compute-quota-types) 0, 2048 GiB varsayılan değerine eşdeğer olan. Sıfır kota değeri artık uyulduğundan.

<!-- 2724873 - IS --> 
- PowerShell cmdlet'lerini kullanırken bir sorun düzeltildi **başlangıç AzsScaleUnitNode** veya **Stop-AzsScaleUnitNode** ölçek birimleri, başlatma veya durdurma ölçek birimi için yapılan ilk girişim başarısız olabilir, yönetilecek.

<!-- 2724961- IS ASDK --> 
- İçinde kayıtlı bir sorun düzeltildi **Microsoft.Insight** kaynak sağlayıcısındaki bir abonelik ayarları ve bir Windows VM konuk işletim sistemi etkin Tanılama ile oluşturuldu, ancak CPU yüzdesi grafiğin VM genel bakış sayfasında gösterme Ölçüm verileri. Veriler artık doğru şekilde görüntülenir.

- Hangi çalışır bir sorun düzeltildi **Get-AzureStackLog** cmdlet'ini çalıştırdıktan sonra başarısız **Test AzureStack** aynı ayrıcalıklı uç noktası (CESARETLENDİRİCİ) oturumunda. Artık, yürütüldüğü aynı CESARETLENDİRİCİ oturumu kullanabilirsiniz **Test AzureStack**.

<!-- bug 3615401, IS -->
- Otomatik yedeklemeler Zamanlayıcı hizmeti içine burada çıkacak ilgili sorun düzeltildi durumu beklenmedik bir şekilde devre dışı. 

<!--2850083, IS ASDK -->
- Kaldırılan **ağ geçidi sıfırlama** düğmesine tıkladıysanız, bir hata oluşturdu Azure Stack Portalı'ndan düğmesi. Karışıklığı önlemek için kaldırıldı için Azure Stack adanmış VM örneği yerine bir çok kiracılı ağ geçidi her Kiracı için VPN ağ geçidi, sahip olduğundan bu düğme Azure Stack'te hiçbir işlev görür. 

<!-- 3209594, IS ASDK -->
- Kaldırılan **geçerli güvenlik kuralları** bağlantı **ağ özellikleri** dikey penceresinde bu özellik olarak Azure Stack'te desteklenmiyor. Mevcut bağlantı olması, bu özellik destekleniyordu izlenim ancak çalışmıyor getirdi. Karışıklık hafifletmek için bağlantıyı kaldırdık.

<!-- 3139614 | IS -->
- Bir güncelleştirme bir OEM'den Azure Stack'e uygulandıktan sonra bir sorunu düzeltildi **güncelleştirme kullanılabilir** bildirim Azure Stack Yönetici portalı'nda görünmüyor.

## <a name="changes"></a>Değişiklikler

<!-- 3083238 IS -->
- Bu güncelleştirmede güvenlik geliştirmeleri dizin hizmeti rolü yedekleme boyutu artış sonuçlanır. Dış depolama konumu için yönergeler boyutlandırma almak için bkz. [altyapı backup belgeleri](azure-stack-backup-reference.md#storage-location-sizing). Bu değişiklik, daha büyük boyuta veri aktarımı nedeniyle yedekleme tamamlanması uzun zaman sonuçlanır. Bu değişiklik, tümleşik sistemler etkiler. 

- Ocak 2019 ' başlayarak, kayıtlı Kubernetes kümelerinde Active Directory Federasyon Hizmetleri'nde (AD FS) bağlı Azure Stack Damgalar (internet erişimi gerekli değildir) dağıtabilirsiniz. Yönergeleri izleyerek [burada](azure-stack-solution-template-kubernetes-cluster-add.md) yeni Kubernetes Market öğesi indirilemedi. Yönergeleri izleyerek [burada](user/azure-stack-solution-template-kubernetes-adfs.md) bir Kubernetes kümesi dağıtmak için. Hedef sistem Ekle ya da AD FS olup olmadığını gösteren yeni parametrelerini not kayıtlı. AD FS varsa yeni alanlar dağıtım sertifikanın depolandığı Key Vault parametreleri girmek kullanılabilir.

   AD FS desteği sayesinde bile, Kubernetes kümelerini dağıtımını internet erişimi gerektiğini unutmayın.

- Azure Stack için güncelleştirmeler veya düzeltmeler yüklendikten sonra yeni özellikler için bir veya birden çok kimlikli uygulamalarda verilebilmesi için yeni izinler gerektiren tanıtılmak. Bu izinleri verme giriş dizini yönetici erişimi gerektirir ve bu nedenle, otomatik olarak yapılamaz. Örneğin:

   ```powershell
   $adminResourceManagerEndpoint = "https://adminmanagement.<region>.<domain>"
   $homeDirectoryTenantName = "<homeDirectoryTenant>.onmicrosoft.com" # This is the primary tenant Azure Stack is registered to

   Update-AzsHomeDirectoryTenant -AdminResourceManagerEndpoint $adminResourceManagerEndpoint `
     -DirectoryTenantName $homeDirectoryTenantName -Verbose
   ```

- Doğru bir şekilde Azure Stack kapasite planlaması için yeni bir durum yoktur. 1901 güncelleştirmeyle, artık sanal makinelerin oluşturulabilir toplam sayısına bir sınır yoktur.  Bu sınır, çözüm kararsızlığı engellemek için geçici olması amaçlanmıştır. Kaynak konumunda VM'ler daha yüksek sayıda kararlılık sorunun ele ancak belirli bir zaman çizelgesi düzeltme için değil henüz karar verilmemiştir. 1901 güncelleştirme vardır, artık bir başına 60 VM'lerin sunucu sınırı 700 toplam çözüm sınırına sahip.  Örneğin, bir 8 sunucu Azure Stack VM sınırı 480 (8 * 60) olacaktır.  12-16 sunucusu için Azure Stack çözüm sınırı 700 olacaktır. Bu sınır, tüm işlem kapasiteyle alakalı durumlar dayanıklılık ayırma ve CPU gibi göz önünde bir işleç damgada sağlamak istediğiniz fiziksel/oranı sanal tutma oluşturuldu. Daha fazla bilgi için kapasite Planlayıcı'nın yeni sürümüne bakın.  
Aşağıdaki hata kodları, sonuç olarak, VM ölçek sınırına ulaşıldı, olay, döndürülürdü: VMsPerScaleUnitLimitExceeded, VMsPerScaleUnitNodeLimitExceeded. 
 

- İşlem API Sürüm 2017-12-01 arttı.

- Altyapı yedeklemesine artık yalnızca ortak anahtara sahip bir sertifika gerektirir (. CER) yedek verilerin şifrelenmesi. Simetrik şifreleme anahtar desteği içinde 1901 başlayarak kullanım dışı bırakılmıştır. Altyapı yedekleme için 1901 güncelleştirmeden önce yapılandırıldıysa, şifreleme anahtarlarını yerinde kalır. Daha fazla bilgi güncelleştirmeleri ile geriye dönük uyumluluk desteği Yedekleme ayarlarını güncelleştirmek için en az 2 gerekir. Daha fazla bilgi için [Azure Stack altyapısını yedekleme en iyi yöntemler](azure-stack-backup-best-practices.md).

## <a name="common-vulnerabilities-and-exposures"></a>Yaygın güvenlik açıklarına ve exposures'ı

Bu güncelleştirme, aşağıdaki güvenlik güncelleştirmeleri yükler:  

- [CVE-2018-8477](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8477)
- [CVE-2018-8514](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8514)
- [CVE-2018-8580](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8580)
- [CVE-2018-8595](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8595)
- [CVE-2018-8596](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8596)
- [CVE-2018-8598](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8598)
- [CVE-2018-8621](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8621)
- [CVE-2018-8622](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8622)
- [CVE-2018-8627](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8627)
- [CVE-2018-8637](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8637)
- [CVE-2018-8638](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8638)
- [ADV190001](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190001)
- [CVE 2019 0536](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0536)
- [CVE 2019 0537](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0537)
- [CVE 2019 0545](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0545)
- [CVE 2019 0549](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0549)
- [CVE 2019 0553](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0553)
- [CVE 2019 0554](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0554)
- [CVE 2019 0559](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0559)
- [CVE 2019 0560](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0560)
- [CVE 2019 0561](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0561)
- [CVE 2019 0569](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0569)
- [CVE 2019 0585](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0585)
- [CVE 2019 0588](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0588)


Bu güvenlik açıkları hakkında daha fazla bilgi için yukarıdaki bağlantılara tıklayın veya Microsoft Bilgi Bankası makalelerine bakın [4480977](https://support.microsoft.com/en-us/help/4480977).

## <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

- Çalıştırırken [Test AzureStack](azure-stack-diagnostic-test.md), ya da **AzsInfraRoleSummary** veya **AzsPortalApiSummary** test başarısız olursa, çalıştırmanız istenir  **Test-AzureStack** ile `-Repair` bayrağı.  Bu komutu çalıştırırsanız, şu hata iletisiyle başarısız olur:  `Unexpected exception getting Azure Stack health status. Cannot bind argument to parameter 'TestResult' because it is null.`

- Çalıştırdığınızda [Test AzureStack](azure-stack-diagnostic-test.md), temel kart yönetim denetleyicisi (BMC) bir uyarı iletisi görüntülenir. Bu uyarıyı güvenle yok sayabilirsiniz.

- <!-- 2468613 - IS --> Bu güncelleştirme yüklemesi sırasında başlık uyarılarla görebileceğiniz `Error – Template for FaultType UserAccounts.New is missing.` Bu uyarılar güvenle yok sayabilirsiniz. Bu güncelleştirme yüklemesi tamamlandıktan sonra uyarıları otomatik olarak kapanır.

## <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar

- Bu güncelleştirme yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için [düzeltmeleri](#hotfixes), hem de bizim [hizmet İlkesi](azure-stack-servicing-policy.md).  

- Rest şifreleme anahtarları, veri almak ve bunları Azure Stack dağıtımınıza dışında güvenli bir şekilde depolayın. İzleyin [anahtarlarını almak yönergeler](azure-stack-security-bitlocker.md).

## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

Bu derleme sürümü için yükleme sonrası bilinen sorunlar verilmiştir.

### <a name="portal"></a>Portal

<!-- 2930820 - IS ASDK --> 
- "Docker için" araması yaparsanız yönetici ve kullanıcı portalı yanlış öğesi döndürülür. Azure Stack'te kullanılamıyor. Bunu oluşturmayı denerseniz, bir hata göstergesi içeren bir dikey pencere görüntülenir. 

<!-- 2931230 – IS  ASDK --> 
- Kullanıcı aboneliği plan kaldırdığınızda bile, bir kullanıcı abonelikte eklenti planı eklendiği planları silinemiyor. Eklenti planı başvuru abonelikleri de silinene kadar plan kalır. 

<!-- TBD - IS ASDK --> 
- 1804 sürümü ile sunulan iki Yönetim abonelik türlerini kullanılmamalıdır. Abonelik türleridir **abonelik ölçümü**, ve **tüketim abonelik**. Bu abonelik türlerini 1804 sürümünden başlayarak yeni Azure Stack ortamlarında görülebilir ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir **varsayılan sağlayıcı** abonelik türü.

<!-- 3557860 - IS ASDK --> 
- Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

<!-- 1663805 - IS ASDK --> 
- Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici çözüm olarak, [izinleri doğrulamak için PowerShell](/powershell/module/azs.subscriptions.admin/get-azssubscriptionplan).

<!-- ### Health and monitoring -->

### <a name="compute"></a>İşlem

- Yeni bir Windows sanal makine (VM) oluştururken, aşağıdaki hata görüntülenebilir:

   `'Failed to start virtual machine 'vm-name'. Error: Failed to update serial output settings for VM 'vm-name'`

   Bir VM'de önyükleme tanılamalarını etkinleştirir, ancak önyükleme tanılama depolama hesabınızı silerseniz bu hata oluşur. Bu sorunu çözmek için önceden kullanılmış şekilde aynı ada sahip depolama hesabını yeniden oluşturun.

<!-- 2967447 - IS, ASDK, to be fixed in 1902 -->
- Dağıtım için bir seçenek olarak, 7.2 CentOS tabanlı sanal makine ölçek kümesi (VMSS) oluşturma deneyimi sağlar. Bu görüntüyü Azure Stack üzerinde kullanılabilir olmadığından, dağıtımınız için başka bir işletim sistemi veya markette dağıtımdan önce indirdiğiniz başka bir CentOS görüntüsü belirten bir Azure Resource Manager şablonu kullanma işleci.  

<!-- TBD - IS ASDK --> 
- Güncelleştirme 1901 uyguladıktan sonra yönetilen disklere sahip VM'ler dağıtırken aşağıdaki sorunlarla karşılaşabilirsiniz:

   - Yönetilen disklerle bir VM dağıtma 1808 güncelleştirmeden önce Abonelik oluşturulurken bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için her abonelik için şu adımları izleyin:
      1. Kiracı Portalı'nda Git **abonelikleri** ve aboneliği bulunamıyor. Seçin **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
      2. Aynı abonelik altında Git **erişim denetimi (IAM)**, doğrulayın **AzureStack DiskRP istemci** listelenir.
   - Bir konuk dizin ile ilişkili bir abonelik içindeki Vm'leri dağıtma, çok kiracılı bir ortam yapılandırdıysanız, bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için aşağıdaki adımları izleyin. [bu makalede](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) her Konuk dizinlerinizi yeniden yapılandırmak için.

- Bir Ubuntu 18.04 etkinleştirilmiş SSH yetkilendirme ile oluşturulan VM, oturum açmak için SSH anahtarları kullanmak izin vermez. Geçici bir çözüm olarak VM erişimi Linux uzantısı için SSH anahtarları sağladıktan sonra uygulamak için kullanmak veya parola tabanlı kimlik doğrulaması kullanın.

### <a name="networking"></a>Ağ İletişimi  

<!-- 3239127 - IS, ASDK -->
- Azure Stack portalında bir VM örneğine iliştirilmiş bir ağ bağdaştırıcısına bağlı bir IP yapılandırması için statik bir IP adresi değiştirdiğinizde bildiren bir uyarı iletisi görürsünüz 

    `The virtual machine associated with this network interface will be restarted to utilize the new private IP address...`.

    Bu iletiyi güvenle yoksayabilirsiniz; sanal makine örneği yeniden başlatma olsa bile IP adresi değişir.

<!-- 3632798 - IS, ASDK -->
- Portalda, bir gelen güvenlik kuralı ekleyin ve seçerseniz, **hizmet etiketi** çeşitli seçenekler görüntülenir, kaynak olarak **kaynak etiketi** Azure Stack için kullanılabilir değil bir listesi. Azure Stack'te geçerli yalnızca seçenekleri aşağıdaki gibidir:

  - **İnternet**
  - **VirtualNetwork**
  - **AzureLoadBalancer**
  
    Diğer seçenekler olarak Azure Stack'te kaynak etiketleri desteklenmez. Benzer şekilde, bir giden güvenlik kuralı ekleyin ve seçin, **hizmet etiketi** hedef aynı seçeneklerinin listesi olarak **kaynak etiketi** görüntülenir. Aynı yalnızca geçerli seçenekler şunlardır **kaynak etiketi**önceki listede açıklandığı gibi.

- Azure Stack'te aynı şekilde olarak genel Azure ağ güvenlik grupları (Nsg'ler) çalışmaz. Azure'da birden çok bağlantı noktası üzerinde bir NSG kuralı ayarlayabilirsiniz (kullanarak portal, PowerShell ve Resource Manager şablonlarını). Azure Stack'te ancak Şirket portalı aracılığıyla bir NSG kuralı birden çok bağlantı noktası ayarlanamıyor. Bu sorunu çözmek için bu ek kuralları ayarlamak için bir Resource Manager şablonu veya PowerShell kullanın.

<!-- 3203799 - IS, ASDK -->
- Azure Stack, 4'ten fazla ağ arabirimlerini (NIC'ler) bir sanal makine örnekleri için örnek boyutu ne olursa olsun bugün bağlanmasını desteklemez.

<!-- ### SQL and MySQL-->

### <a name="app-service"></a>Uygulama Hizmeti

<!-- 2352906 - IS ASDK --> 
- İlk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısını kaydetmeniz gerekir.


<!-- ### Usage -->

 
<!-- #### Identity -->
<!-- #### Marketplace -->

### <a name="syslog"></a>Syslog

- Syslog yapılandırmasını syslog istemci yapılandırmasını ve iletilen durdurmak için syslog iletileri kaybetmenize neden olan bir güncelleştirme döngüsü boyunca kalıcı olmaz. Bu sorun, syslog istemci (1809) genel kullanım tüm Azure Stack sürümleri için geçerlidir. Bu sorunu çözmek için bir Azure Stack güncelleştirme uygulandıktan sonra syslog istemci yeniden yapılandırın.

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1901 güncelleştirme paketinden indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload). 

Yalnızca bağlı senaryolarda Azure Stack dağıtımları güvenli bir uç nokta düzenli aralıklarla denetleyin ve bulut için bir güncelleştirme varsa, otomatik olarak bilgilendirme. Daha fazla bilgi için [Azure Stack için güncelleştirmeleri yönetme](azure-stack-updates.md#using-the-update-tile-to-manage-updates).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  
