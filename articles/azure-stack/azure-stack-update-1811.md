---
title: Azure Stack 1811 güncelleştirmesi | Microsoft Docs
description: Yenilikler dahil olmak üzere, Azure Stack tümleşik sistemleri 1811 güncelleştirmesi hakkında bilinen sorunlar ve güncelleştirmeyi yüklemek nereye öğrenin.
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
ms.lastreviewed: 02/28/2019
ms.openlocfilehash: 138913414a8e45084d498a0c7b2e864bc443197f
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59362051"
---
# <a name="azure-stack-1811-update"></a>Azure Stack 1811 güncelleştirme

*Şunlara uygulanır Azure Stack tümleşik sistemleri*

Bu makalede 1811 güncelleştirme paketinin içeriğini açıklar. Güncelleştirme paketinin bu sürümü, Azure Stack için yeni özellikler geliştirmeleri ve düzeltmeleri içerir. Bu makalede ayrıca bu sürümdeki bilinen sorunlara açıklar ve güncelleştirmeyi indirebilmesi bir bağlantı içerir. Bilinen sorunlar doğrudan güncelleştirme işlemiyle ilgili sorunları ve yapı (yükleme sonrası) ile ayrılır.

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1811 güncelleştirmenin yapı numarasıdır **1.1811.0.101**.

## <a name="hotfixes"></a>Düzeltmeler

Azure Stack düzeltmeleri düzenli olarak serbest bırakır. Yüklediğinizden emin olun [en son Azure Stack düzeltme](#azure-stack-hotfixes) Azure Stack için 1811 güncelleştirmeden önce 1809 için.

> [!TIP]  
> Aşağıdaki abone olmak *RSS* veya *Atom* Azure Stack düzeltmelerle birlikte kalmasını sağlamak için akışları:
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

### <a name="azure-stack-hotfixes"></a>Azure Stack düzeltmeler

- **1809**: [KB 4481548 – Azure Stack düzeltme 1.1809.12.114](https://support.microsoft.com/help/4481548/)
- **1811**: Geçerli düzeltme yok.

## <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> 1811 güncelleştirme yüklemesi sırasında tüm örnekleri Yönetici portalı'nın kapalı olduğundan emin olmanız gerekir. Kullanıcı Portalı açık kalır, ancak Yönetici portalı kapatılmalıdır.

- Azure Stack dağıtımınıza Azure Stack uzantısı ana bilgisayar için hazır olun. Aşağıdaki yönergeleri kullanarak sisteminizi hazırlayın: [Azure Stack için uzantı konak hazırlama](azure-stack-extension-host-prepare.md). 
 
- Yükleme [en son Azure Stack düzeltme](#azure-stack-hotfixes) 1811 için güncelleştirmeden önce 1809 için.

- Bu güncelleştirme yüklemesi başlamadan önce çalıştırması [Test AzureStack](azure-stack-diagnostic-test.md) bulunan tüm çalışma sorunlarını çözün ve Azure Stack durumunu doğrulamak için aşağıdaki parametreleri, tüm uyarılar ve hatalar dahil olmak üzere. Ayrıca etkin Uyarıları gözden geçirin ve eylemi gerektiren tüm çözümleyin.  

    ```powershell
    Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary, AzsHostingServiceCertificates
    ```

    Uzantı konak gereksinimleri karşılanıyor, yoksa `Test-AzureStack` çıkışı şu iletiyi görüntüler: 
  
    `To proceed with installation of the 1811 update, you will need to import 
    the SSL certificates required for Extension Host, which simplifies network 
    integration and increases the security posture of Azure Stack. Refer to this 
    link to prepare for Extension Host:
    https://docs.microsoft.com/azure/azure-stack/azure-stack-extension-host-prepare`

- Azure Stack 1811 güncelleştirmesi, Azure Stack ortamınıza zorunlu uzantısı ana bilgisayar sertifikaları düzgün bir şekilde aldınız gerektirir. 1811 güncelleştirmeyi yüklemeye devam etmek için uzantısı ana bilgisayar için gereken SSL sertifikaları içeri aktarmanız gerekir. Sertifikaları içeri aktarmak için bkz: [Bu bölümde](azure-stack-extension-host-prepare.md#import-extension-host-certificates).

    Her uyarıyı yoksayıp 1811 güncelleştirmeyi yüklemek isterseniz, güncelleştirme yaklaşık 1 saat şu iletiyle başarısız olur:   
 
    `The required SSL certificates for the Extension Host have not been found.
    The Azure Stack update will halt. Refer to this link to prepare for 
    Extension Host: https://docs.microsoft.com/azure/azure-stack/azure-stack-extension-host-prepare,
    then resume the update.
    Exception: The Certificate path does not exist: [certificate path here]` 
 
    Düzgün bir şekilde zorunlu uzantısı ana bilgisayar sertifikaları içe aktardıktan sonra yönetici portalından 1811 güncelleştirme devam edebilir. Microsoft Azure Stack operatörleri güncelleştirme işlemi sırasında bir bakım penceresi zamanlamak üzere önerir, ancak bir hata nedeniyle eksik uzantı ana bilgisayar sertifikaları var olan iş yükleri veya hizmetleri etkilememesi gerekir.  

    Bu güncelleştirmenin yüklenmesi sırasında Azure Stack Kullanıcı Portalı uzantısı konağı yapılandırılırken kullanılamaz. Uzantı konağın yapılandırmasını 5 saate kadar sürebilir. Bu süre boyunca bir güncelleştirmenin durumunu denetleyebilir veya [Azure Stack Yöneticisi PowerShell veya ayrıcalıklı uç noktayı](azure-stack-monitor-update.md) kullanarak başarısız bir güncelleştirme yüklemesini sürdürebilirsiniz.

- Azure Stack System Center Operations Manager (SCOM) tarafından yönetildiğinde 1811 uygulamadan önce Yönetim Paketi için Microsoft Azure Stack 1.0.3.11 sürümüne güncelleştirdiğinizden emin olun.

## <a name="new-features"></a>Yeni Özellikler

Bu güncelleştirme, aşağıdaki yeni özellikleri ve Azure Stack için geliştirmeler içerir:

- Bu sürümle birlikte, [uzantısı konağı](azure-stack-extension-host-prepare.md) etkinleştirilir. Uzantısı konağı, ağ tümleştirmeyi basitleştirir ve Azure Stack güvenlik duruşu artırır.

- Active Directory Federasyon Hizmetleri (AD FS) ile cihaz kimlik doğrulaması için destek eklendi özellikle Azure CLI kullanarak. Daha fazla bilgi için [kullanımı API sürümü profillerini Azure Stack'te Azure CLI ile](./user/azure-stack-version-profiles-azurecli2.md)

- Hizmet sorumlusu istemci gizli anahtarını kullanarak Active Directory Federasyon Hizmetleri ile (AD FS) için destek eklendi. Daha fazla bilgi için [AD FS için hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md#manage-service-principal-for-ad-fs).

- Bu sürüm aşağıdaki Azure depolama hizmeti API sürümleri için destek ekler: **2017-07-29**, **2017-11-09**. Destek, aşağıdaki Azure depolama kaynak sağlayıcısı API sürümleri için de eklenir: **2016-05-01**, **2016-12-01**, **2017-06-01**, ve **2017-10-01**. Daha fazla bilgi için [Azure Stack Depolama: Farklılıklar ve dikkat edilmesi gerekenler](./user/azure-stack-acs-differences.md).

- Güncelleştirme ve ADFS için hizmet ilkeleri kaldırmak için yeni ayrıcalıklı uç nokta komutlar eklendi. Daha fazla bilgi için [AD FS için hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md#manage-service-principal-for-ad-fs).

- Başlamak Azure Stack operatörü izin eklenen yeni ölçek birimi düğüm işlemleri durdurun ve bir ölçek birimi düğümlerini kapatın. Daha fazla bilgi için [ölçek birimi düğüm eylemlerinin Azure Stack'te](azure-stack-node-actions.md).

- Ortamın kayıt ayrıntılarını görüntüleyen yeni bir bölge özellikleri dikey eklendi. Tıklayarak bu bilgileri görüntüleyebilirsiniz **bölge Yönetimi** Yönetici portalı'nda varsayılan pano kutucuğunu seçip ardından **özellikleri**.

- Kullanıcı adı ve parola, fiziksel makinelerle iletişim kurmak için kullanılan BMC kimlik bilgileri güncelleştirmek için yeni bir ayrıcalıklı uç nokta komutu eklendi. Daha fazla bilgi için [temel kart yönetim denetleyicisi güncelleştirme \(BMC) kimlik bilgisi](azure-stack-rotate-secrets.md).

- Azure yol haritası ancak erişim olanağı, yönetici ve Kullanıcı Portalı, benzer şekilde, Azure portalında kullanılabilir sağ üst köşesindeki Yardım ve Destek simgesi (soru işareti) ekledik.

- Bağlantısı kesilmiş kullanıcılar için geliştirilmiş bir Market yönetimi deneyimi eklendi. Bağlantısı kesilmiş bir ortamda bir Market öğesi yayımlama için karşıya yükleme işlemi, görüntü ve Market paket ayrı olarak karşıya yükleme yerine bir adım için basitleştirilmiştir. Karşıya yüklenen ürün Market yönetim dikey penceresinde görünür olacaktır. Daha fazla bilgi için [Bu bölümde](azure-stack-download-azure-marketplace-item.md#import-the-download-and-publish-to-azure-stack-marketplace-1811-and-higher). 

- Bu sürüm gizli döndürme için gerekli bakım penceresi sırasında yalnızca dış sertifika döndürme olanağı ekleyerek azaltır [Azure Stack gizli dönüş](azure-stack-rotate-secrets.md).

- [Azure Stack PowerShell](azure-stack-powershell-install.md) 1.6.0 sürümüne güncelleştirildi. Bu güncelleştirme, Azure Stack'te depolama ile ilgili yeni özellikler için destek içerir. Sürüm notları için daha fazla bilgi için bkz. [Azure Stack yönetim modülünü PowerShell galerisinde 1.6.0](https://www.powershellgallery.com/packages/AzureStack/1.6.0) güncelleştiriliyor veya Azure Stack PowerShell'i yükleme hakkında daha fazla bilgi için bkz [için PowerShell yükleme Azure Stack](azure-stack-powershell-install.md).

- Yönetilen diskler artık varsayılan olarak etkindir Azure Stack portalını kullanarak sanal makineler oluştururken. Bkz: [bilinen sorunlar](#known-issues-post-installation) yönetilen VM oluşturma hataları önlemek diskleri için gerekli ek adımlar bölümüne.

- Bu sürüm uyarı tanıtır **onarım** Azure Stack operatörü eylemleri. 1811 içinde bazı uyarılarda sağlayan bir **onarım** sorunu çözmek için seçebileceğiniz uyarı düğmesi. Daha fazla bilgi için [izleme sistem durumu ve Uyarıları Azure Stack'te](azure-stack-monitor-health.md).

- Azure Stack'te güncelleştirme deneyimini güncelleştirmeleri. Güncelleştirme geliştirmeler şunları içerir: 
  - Güncelleştirme geçmişini daha iyi izleme güncelleştirme işlemleri sürüyor güncelleştirmeleri bölün ve güncelleştirmeleri tamamlandı sekmeler.
  - Temel Parçalar bölümünde yeni simgeler ve son yanı sıra geçerli ve OEM sürümleri için Düzen ile geliştirilmiş durum görselleştirmeler tarih güncelleştirildi.
  - **Görünüm** için sürüm notları sütun kullanıcıyı doğrudan belgeler genel güncelleştirme sayfası yerine bu güncelleştirmeyi belirli götürür bağlayın.
  - **Güncelleştirme geçmişi** sekmesini her güncelleştirmelerin çalıştırma zamanlarını belirlemek için kullanılan yanı sıra gelişmiş filtreleme yetenekleri.  
  - Bağlı azure Stack ölçek birimleri otomatik olarak hala alacağı **güncelleştirme kullanılabilir** kullanılabilir oldukça.
  - Bağlı olmadığınız azure Stack ölçek birimleri güncelleştirmeleri önce olduğu gibi aktarabilirsiniz. 
  - Portaldan JSON günlükleri indirmek için işlemindeki bir değişiklik bulunmamaktadır. Azure Stack operatörleri, ilerleme ifade adımları genişletme görürsünüz.

    Daha fazla bilgi için [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).


## <a name="fixed-issues"></a>Düzeltilen sorunlar

<!-- TBD - IS ASDK --> 
- İçinde genel IP adresi kullanım ölçüm verileri gösterilen aynı bir sorun düzeltildi **olay tarihi-saati** yerine her kaydın değerini **TimeDate** gösteren kaydın oluşturulduğu zaman damgası. Bu veriler artık ortak IP adresi kullanımının doğru hesap gerçekleştirmek için de kullanabilirsiniz.

<!-- 3099544 – IS, ASDK --> 
- Azure Stack portalını kullanarak bir yeni sanal makine (VM) oluştururken gerçekleşen bir sorun düzeltildi. Neden VM boyutunu seçme **ABD Doları/ay** görüntülenecek sütun bir **kullanılamıyor** ileti. Bu sütunu artık görünür; VM görüntüleme fiyatlandırma sütunu Azure Stack'te desteklenmiyor.

<!-- 2930718 - IS ASDK --> 
- İçinde bir sorun düzeltildi Yönetici portalını herhangi bir kullanıcı aboneliği ayrıntıları tıklandığında ve dikey pencereyi kapatmadan sonra erişirken **son**, kullanıcı abonelik adı görünmez. Kullanıcı abonelik adı görünür.

<!-- 3060156 - IS ASDK --> 
- İçinde yönetici ve kullanıcı portalı bir sorun düzeltildi: portalı ayarlarını tıklayıp seçerek **tüm ayarları ve özel panoları Sil** beklendiği gibi çalışmaması ve bir hata bildirimi zobrazilo. Bu seçenek artık düzgün şekilde çalışır.

<!-- 2930799 - IS ASDK --> 
- İçinde yönetici ve kullanıcı portalı bir sorun düzeltildi: altında **tüm hizmetleri**, varlık **DDoS koruma planları** yanlış listelenir. Azure Stack'te kullanılamıyor. Listenin kaldırıldı.
 
<!--2760466 – IS  ASDK --> 
- Yeni bir Azure Stack ortamına yüklendiğinde gerçekleşen bir sorun düzeltildi, uyarıyı gösterir **etkinleştirme gerekli** görüntüleme. Artık düzgün bir şekilde görüntüler.

<!--1236441 – IS  ASDK --> 
- AD FS kullanılırken bir kullanıcı grubuna RBAC ilkelerini uygulama önleyen bir sorun düzeltildi.

<!--3463840 - IS, ASDK --> 
- Bir genel VIP ağları erişilemez dosya sunucusundan nedeniyle başarısız olan altyapı yedeklemelerle bir sorun düzeltildi. Bu düzeltmenin hizmet altyapı yedekleme genel altyapı ağına geri taşınır. En son uyguladıysanız [1809 için Azure Stack düzeltme](#azure-stack-hotfixes) , bu sorunu giderir, 1811 güncelleştirme herhangi bir değişiklik yapmaz. 

<!-- 2967387 – IS, ASDK --> 
- Azure Stack yönetici veya kullanıcı portalı oturum açmak için kullandığınız hesabı görüntülendiği olarak bir sorun düzeltildi **tanımlanmayan kullanıcı**. Hesap ya da sahip olduğunda bu ileti görüntülendi bir **ilk** veya **son** adı belirtilmedi.   

<!--  2873083 - IS ASDK --> 
- Neden bir sanal makine ölçek kümesi (VMSS) oluşturmak için hangi portalı kullanarak bir sorun düzeltildi **örnek boyutu** doğru Internet Explorer kullanırken yüklememeye açılır. Bu tarayıcıyı artık düzgün şekilde çalışır.  

<!-- 3190553 - IS ASDK -->
- Bir altyapı rol örneği kullanılamıyor veya ölçek birimi düğümü çevrimdışı gösteren gürültülü uyarıları oluşturan bir sorun düzeltildi.

<!-- 2724961 - IS ASDK -->
- Fiexed bir sorun, VM genel bakış sayfasında sanal makine ölçüm grafiği doğru bir şekilde gösteremez. 

## <a name="changes"></a>Değişiklikler

- Yeni bir yolunu görüntüleyin ve kotalar bir plandaki düzenleyin 1811 kullanıma sunulmuştur. Daha fazla bilgi için [var olan bir kota görüntülemek](azure-stack-quota-types.md#view-an-existing-quota).

<!-- 3083238 IS -->
- Bu güncelleştirmede güvenlik geliştirmeleri dizin hizmeti rolü yedekleme boyutu artış sonuçlanır. Dış depolama konumu için yönergeler boyutlandırma almak için bkz. [altyapı backup belgeleri](azure-stack-backup-reference.md#storage-location-sizing). Bu değişiklik, daha büyük boyuta veri aktarımı nedeniyle yedekleme tamamlanması uzun zaman sonuçlanır. Bu değişiklik, tümleşik sistemler etkiler. 

- BitLocker kurtarma anahtarlarını almak için mevcut CESARETLENDİRİCİ cmdlet Get-AzsCsvsRecoveryKeys Get-AzsRecoveryKeys için gelen bir 1811 yeniden adlandırılır. BitLocker kurtarma anahtarlarını alma hakkında daha fazla bilgi için bkz. [anahtarlarını almak yönergeler](azure-stack-security-bitlocker.md).

## <a name="common-vulnerabilities-and-exposures"></a>Yaygın güvenlik açıklarına ve exposures'ı

Bu güncelleştirme, aşağıdaki güvenlik güncelleştirmeleri yükler:  

- [CVE-2018-8256](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8256)
- [CVE-2018-8407](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8407)
- [CVE-2018-8408](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8408)
- [CVE-2018-8415](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8415)
- [CVE-2018-8417](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8417)
- [CVE-2018-8450](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8450)
- [CVE-2018-8471](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8471)
- [CVE-2018-8476](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8476)
- [CVE-2018-8485](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8485)
- [CVE-2018-8544](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8544)
- [CVE-2018-8547](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8547)
- [CVE-2018-8549](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8549)
- [CVE-2018-8550](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8550)
- [CVE-2018-8553](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8553)
- [CVE-2018-8561](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8561)
- [CVE-2018-8562](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8562)
- [CVE-2018-8565](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8565)
- [CVE-2018-8566](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8566)
- [CVE-2018-8584](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8584)

Bu güvenlik açıkları hakkında daha fazla bilgi için yukarıdaki bağlantılara tıklayın veya Microsoft Bilgi Bankası makalelerine bakın [4478877](https://support.microsoft.com/help/4478877).

## <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

- Çalıştırdığınızda **Get-AzureStackLog** PowerShell cmdlet'ini çalıştırdıktan sonra **Test AzureStack** aynı ayrıcalıklı uç noktası (CESARETLENDİRİCİ) oturumunda **Get-AzureStackLog** başarısız olur. Bu sorunu geçici olarak çözmek için yürütüldüğü CESARETLENDİRİCİ oturumu kapatmak **Test AzureStack**ve sonra çalıştırmak için yeni bir oturum açma **Get-AzureStackLog**.

- 1811 yüklenmesi sırasında güncelleştirmesi, tüm örnekleri Yönetici portalı'nın bu süre boyunca kapalı olduğundan emin olun. Kullanıcı Portalı açık kalır, ancak Yönetici portalı kapatılmalıdır.

- Çalıştırırken [Test AzureStack](azure-stack-diagnostic-test.md), ya da **AzsInfraRoleSummary** veya **AzsPortalApiSummary** test başarısız olursa, çalıştırmanız istenir  **Test-AzureStack** ile `-Repair` bayrağı.  Bu komutu çalıştırırsanız, şu hata iletisiyle başarısız olur:  `Unexpected exception getting Azure Stack health status. Cannot bind argument to parameter 'TestResult' because it is null.`  Bu sorun gelecekteki bir sürümde düzeltilecektir.

- 1811 güncelleştirme yüklemesi sırasında Azure Stack kullanım portal uzantısı konağı yapılandırılırken kullanılamıyor. Uzantı konağın yapılandırmasını 5 saate kadar sürebilir. Bu süre boyunca bir güncelleştirmenin durumunu denetleyebilir veya [Azure Stack Yöneticisi PowerShell veya ayrıcalıklı uç noktayı](azure-stack-monitor-update.md) kullanarak başarısız bir güncelleştirme yüklemesini sürdürebilirsiniz. 

- 1811 güncelleştirme yüklemesi sırasında kullanıcı portalı Pano kullanılamayabilir ve özelleştirmeler kaybolmuş olabilir. Portal ayarlarını açıp seçerek Güncelleştirme tamamlandıktan sonra varsayılan ayarı Pano geri yükleyebilirsiniz **varsayılan ayarları geri**.

- Çalıştırdığınızda [Test AzureStack](azure-stack-diagnostic-test.md), temel kart yönetim denetleyicisi (BMC) bir uyarı iletisi görüntülenir. Bu uyarıyı güvenle yok sayabilirsiniz.

- <!-- 2468613 - IS --> Bu güncelleştirme yüklemesi sırasında başlık uyarılarla görebileceğiniz `Error – Template for FaultType UserAccounts.New is missing.` Bu uyarılar güvenle yok sayabilirsiniz. Bu güncelleştirme yüklemesi tamamlandıktan sonra uyarıları otomatik olarak kapanır.

- <!-- 3139614 | IS --> Azure Stack için bir güncelleştirme uyguladıysanız, OEM'den **güncelleştirme kullanılabilir** bildirim Azure Stack Yönetici portalı'nda görünmeyebilir. Microsoft update yüklemek için indirin ve burada bulunan yönergeleri kullanarak el ile içeri aktarılması [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).

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

<!-- TBD - IS ASDK --> 
- Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

<!-- TBD - IS ASDK --> 
- Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici çözüm olarak, [izinleri doğrulamak için PowerShell](/powershell/module/azs.subscriptions.admin/get-azssubscriptionplan).

### <a name="health-and-monitoring"></a>Sistem durumu ve izleme

<!-- 1264761 - IS ASDK -->  
- Uyarıları görebilirsiniz **sistem durumu denetleyicisi** aşağıdaki ayrıntıları olan bir bileşeni:  

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

    Her iki uyarılar güvenle yoksayılabilir. Bunlar, zaman içinde otomatik olarak kapatılacak.  

### <a name="compute"></a>İşlem

- Yeni bir Windows sanal makine (VM), oluştururken **ayarları** dikey gerektirir devam edebilmek için bir ortak gelen bağlantı noktası seçin. 1811, bu ayar gerekli değildir, ancak hiçbir etkisi olmaz. Azure güvenlik duvarı, Azure Stack'te uygulanmadı özelliğin bağımlı olmasıdır. Seçebileceğiniz **Hayır ortak gelen bağlantı noktası**, ya da VM oluşturma işlemine devam etmek için diğer seçeneklerden herhangi biri. Ayarın hiçbir etkisi olmaz.

- Yeni bir Windows sanal makine (VM) oluştururken, aşağıdaki hata görüntülenebilir:

   `'Failed to start virtual machine 'vm-name'. Error: Failed to update serial output settings for VM 'vm-name'`

   Bir VM'de önyükleme tanılamalarını etkinleştirir, ancak önyükleme tanılama depolama hesabınızı silerseniz bu hata oluşur. Bu sorunu çözmek için önceden kullanılmış şekilde aynı ada sahip depolama hesabını yeniden oluşturun.

- Oluştururken bir [Dv2 serisi VM](./user/azure-stack-vm-considerations.md#virtual-machine-sizes), D11-14v2 Vm'leri sırasıyla 4, 8, 16 ve 32 veri diskleri oluşturmanıza izin. Ancak, 8, 16, 32 ve 64 veri diski oluştur VM bölmesi gösterir.

- Azure Stack'te kullanım kayıtlarını beklenmeyen büyük/küçük harf içerebilir. Örneğin:

   `{"Microsoft.Resources":{"resourceUri":"/subscriptions/<subid>/resourceGroups/ANDREWRG/providers/Microsoft.Compute/
   virtualMachines/andrewVM0002","location":"twm","tags":"null","additionalInfo":
   "{\"ServiceType\":\"Standard_DS3_v2\",\"ImageType\":\"Windows_Server\"}"}}`

   Bu örnekte, kaynak grubu adı olması gereken **AndrewRG**. Bu tutarsızlık güvenle yok sayabilirsiniz.

<!-- 3235634 – IS, ASDK -->
- Vm'leri içeren boyutları ile dağıtmak için bir **v2** soneki; Örneğin, **işler için standart_a2_v2**, sonek olarak belirtmek **işler için standart_a2_v2** (küçük harf v). Kullanmayın **işler için standart_a2_v2** (Büyük Harf V). Bu genel Azure'da çalışır ve Azure Stack'te bir tutarsızlık olduğunu.

<!-- 2869209 – IS, ASDK --> 
- Kullanırken [ **Ekle AzsPlatformImage** cmdlet'i](/powershell/module/azs.compute.admin/add-azsplatformimage), kullanmalısınız **- OsUri** parametre olarak depolama hesabı URI'si disk nereye yüklenir. Yerel yol diskin kullanırsanız, cmdlet şu hatayla başarısız olur: 

    `Long running operation failed with status 'Failed'`

<!--  2795678 – IS, ASDK --> 
- VM, portalı sanal makineler (VM'ler) oluşturmak için bir premium VM boyutu (DS, Ds_v2, FS, FSv2) kullandığınızda, bir standart depolama hesabı oluşturulur. Bir standart depolama hesabı oluşturma IOPS, işlevsel olarak, etkilemez ya da fatura. Bildiren bir uyarıyı güvenle yok sayabilirsiniz: 

    `You've chosen to use a standard disk on a size that supports premium disks. This could impact operating system performance and is not recommended. Consider using premium storage (SSD) instead.`

<!-- 2967447 - IS, ASDK --> 
- Dağıtım için bir seçenek olarak, 7.2 CentOS tabanlı sanal makine ölçek kümesi (VMSS) oluşturma deneyimi sağlar. Bu görüntüyü Azure Stack üzerinde kullanılabilir olmadığından, dağıtımınız için başka bir işletim sistemi veya markette dağıtımdan önce indirdiğiniz başka bir CentOS görüntüsü belirten bir Azure Resource Manager şablonu kullanma işleci.  

<!-- 2724873 - IS --> 
- PowerShell cmdlet'lerini kullanırken **başlangıç AzsScaleUnitNode** veya **Stop-AzsScaleunitNode** ölçek birimleri yönetmek için başlatma veya durdurma ölçek birimi için yapılan ilk girişim başarısız olabilir. İlk çalıştırılmasında cmdlet'i başarısız olursa, cmdlet'in ikinci kez çalıştırın. İkinci çalıştırma işlemi başarıyla tamamlanmış olmalıdır. 

<!-- TBD - IS ASDK --> 
- Bir VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı sağlar.  

<!-- 1662991 IS ASDK --> 
- Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.  

<!-- 3507629 - IS, ASDK --> 
- Yönetilen diskler oluşturur iki yeni [kota türleri işlem](azure-stack-quota-types.md#compute-quota-types) sağlanabilir yönetilen diskler hizmetin maksimum kapasitesi sınırlamak için. Varsayılan olarak, 2048 GiB her yönetilen diskler kota türü için ayrılır. Ancak, aşağıdaki sorunlarla karşılaşabilirsiniz:

   - 2048 GiB ayrılır ancak 1808 güncelleştirmeden önce oluşturulan kotalarını 0 değerleri yönetilen diskler kotası yönetici Portalı'nda gösterir. Gerçek gereksinimlerinize ve yeni belirlenen göre değerini azaltın veya artırabilirsiniz kota değeri, 2048 GiB varsayılan'ı geçersiz kılar.
   - Kota değeri 0'a güncelleştirin, 2048 GiB varsayılan değerini eşdeğer olacaktır. Geçici çözüm olarak, kota değeri 1 olarak ayarlayın.

<!-- TBD - IS ASDK --> 
- Güncelleştirme 1811 uyguladıktan sonra yönetilen disklere sahip VM'ler dağıtırken aşağıdaki sorunlarla karşılaşabilirsiniz:

   - Yönetilen disklerle bir VM dağıtma 1808 güncelleştirmeden önce Abonelik oluşturulurken bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için her abonelik için şu adımları izleyin:
      1. Kiracı Portalı'nda Git **abonelikleri** ve aboneliği bulunamıyor. Seçin **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
      2. Aynı abonelik altında Git **erişim denetimi (IAM)**, doğrulayın **AzureStack DiskRP istemci** rol listelenmektedir.
   - Bir konuk dizin ile ilişkili bir abonelik içindeki Vm'leri dağıtma, çok kiracılı bir ortam yapılandırdıysanız, bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için aşağıdaki adımları izleyin. [bu makalede](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) her Konuk dizinlerinizi yeniden yapılandırmak için.

- Bir Ubuntu 18.04 etkinleştirilmiş SSH yetkilendirme ile oluşturulan VM, oturum açmak için SSH anahtarları kullanmak izin vermez. Geçici bir çözüm olarak VM erişimi Linux uzantısı için SSH anahtarları sağladıktan sonra uygulamak için kullanmak veya parola tabanlı kimlik doğrulaması kullanın.

### <a name="networking"></a>Ağ İletişimi  

<!-- 1766332 - IS ASDK --> 
- Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

<!-- 1902460 - IS ASDK --> 
- Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra reddedilir.

<!-- 16309153 - IS ASDK --> 
- DNS sunucusu ayarı ile oluşturulan bir sanal ağ üzerindeki **otomatik**, özel bir DNS sunucusuna değiştirme başarısız. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.

<!-- 2529607 - IS ASDK --> 
- Azure Stack sırasında *gizli dönüş*, bir süre içinde genel IP adresleri olan erişilemeyen iki ila beş dakikalığına yoktur.

<!-- 2664148 - IS ASDK --> 
- Bir S2S VPN tüneli aracılığıyla Kiracı sanal makineleri burada erişiyor senaryolarda, şirket içi alt ağ yerel ağ geçidi için ağ geçidi zaten oluşturulduktan sonra eklenmişse nerede bağlantı girişimleri başarısız bir senaryo hatalarla karşılaşabilirsiniz. 

- Azure Stack portalında bir VM örneğine iliştirilmiş bir ağ bağdaştırıcısına bağlı bir IP yapılandırması için statik bir IP adresi değiştirdiğinizde bildiren bir uyarı iletisi görürsünüz 

    `The virtual machine associated with this network interface will be restarted to utilize the new private IP address...`. 

    Bu iletiyi güvenle yoksayabilirsiniz; sanal makine örneği yeniden başlatma olsa bile IP adresi değişir.

- Portalında, üzerinde **ağ özellikleri** için bir bağlantı dikey **geçerli güvenlik kuralları** her ağ bağdaştırıcısı için. Bu bağlantıyı seçerseniz, hata iletisini gösteren yeni bir dikey pencere açılır `Not Found.` Azure Stack henüz desteklemediğinden, bu hata oluştuğunda **geçerli güvenlik kuralları**.

- Portalda, bir gelen güvenlik kuralı ekleyin ve seçerseniz, **hizmet etiketi** çeşitli seçenekler görüntülenir, kaynak olarak **kaynak etiketi** Azure Stack için kullanılabilir değil bir listesi. Azure Stack'te geçerli yalnızca seçenekleri aşağıdaki gibidir:

  - **İnternet**
  - **VirtualNetwork**
  - **AzureLoadBalancer**
  
    Diğer seçenekler olarak Azure Stack'te kaynak etiketleri desteklenmez. Benzer şekilde, bir giden güvenlik kuralı ekleyin ve seçin, **hizmet etiketi** hedef aynı seçeneklerinin listesi olarak **kaynak etiketi** görüntülenir. Aynı yalnızca geçerli seçenekler şunlardır **kaynak etiketi**önceki listede açıklandığı gibi.

- **New-Azurermıpsecpolicy** PowerShell cmdlet'i ayarını desteklemiyor **DHGroup24** için `DHGroup` parametresi.

- Azure Stack'te aynı şekilde olarak genel Azure ağ güvenlik grupları (Nsg'ler) çalışmaz. Azure'da birden çok bağlantı noktası üzerinde bir NSG kuralı ayarlayabilirsiniz (kullanarak portal, PowerShell ve Resource Manager şablonlarını). Azure Stack'te bir NSG kuralı portal aracılığıyla üzerinde birden çok bağlantı noktası ayarlanamıyor. Bu sorunu geçici olarak çözmek için bu ek kurallar Resource Manager şablonu kullanın.

### <a name="infrastructure-backup"></a>Altyapı yedekleme

<!--scheduler config lost, bug 3615401, new issue in 1811,  hectorl-->
<!-- TSG: https://www.csssupportwiki.com/index.php/Azure_Stack/KI/Backup_scheduler_configuration_lost --> 
- Otomatik yedeklemeler etkinleştirdikten sonra Zamanlayıcı hizmeti beklenmedik bir şekilde devre dışı durumuna geçtiğinde. Yedekleme denetleyici hizmeti, otomatik yedeklemeler devre dışı bırakılır ve bir Yönetici portalını uyarısı algılar. Bu uyarı, otomatik yedeklemeler devre dışı bırakıldığında beklenmektedir. 
    - Neden: Zamanlayıcı yapılandırmasının kaybı ile sonuçlanır hizmetinde bir hata nedeniyle bu sorunudur. Bu hata, depolama konumu, kullanıcı adı, parola ve şifreleme anahtarı değiştirmez.   
    - Düzeltme: Bu sorunu gidermek için altyapı yedekleme kaynak sağlayıcısındaki yedekleme denetleyicisi ayarlar dikey penceresi açın ve seçin **etkinleştirmek otomatik yedeklemeler**. İstenen sıklığı ve Bekletme dönemi ayarladığınızdan emin olun.
    - Örneği: Düşük 

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

Azure Stack 1811 güncelleştirme paketinden indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload). 

Yalnızca bağlı senaryolarda Azure Stack dağıtımları güvenli bir uç nokta düzenli aralıklarla denetleyin ve bulut için bir güncelleştirme varsa, otomatik olarak bilgilendirme. Daha fazla bilgi için [Azure Stack için güncelleştirmeleri yönetme](azure-stack-updates.md#using-the-update-tile-to-manage-updates).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  
- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).  
