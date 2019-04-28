---
title: Azure Active Directory Connect Health SSS - Azure | Microsoft Docs
description: Bu SSS, Azure AD Connect Health hakkında sorular yanıtlanmaktadır. Bu SSS; faturalama modeli, özellikler, sınırlamalar ve destek dahil olmak üzere hizmetin kullanımı hakkındaki soruları kapsar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: f1b851aa-54d7-4cb4-8f5c-60680e2ce866
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 07/18/2017
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec88caafa9a6168860a8e9e2ff9e2abe0cfd0e77
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62096136"
---
# <a name="azure-ad-connect-health-frequently-asked-questions"></a>Azure AD Connect Health hakkında sık sorulan sorular
Bu makale, Azure Active Directory (Azure AD) Connect Health hakkında sık sorulan sorular (SSS) yanıtlarını içerir. Bu SSS, özellik, sınırlamalar ve Destek faturalandırma modeli içeren hizmetin nasıl kullanılacağını hakkında sorular kapsar.

## <a name="general-questions"></a>Genel sorular
**S: Birden çok Azure AD dizini yönetebilirim. Azure Active Directory Premium içeren bir nasıl geçiş yapabilirim?**

Farklı arasında geçiş yapmak için Azure AD kiracılarıyla seçin şu anda oturum açmış **kullanıcı adı** üzerinde sağ üst köşe ve ardından uygun hesabı seçin. Hesap burada listelenmemişse seçin **oturumunuzu**ve ardından Azure Active Directory Premium oturum açmak için etkin olan dizini genel yönetici kimlik bilgilerini kullanın.

**S: Azure AD Connect Health tarafından kimlik rolleri hangi sürümü desteklenir?**

Aşağıdaki tabloda rolleri listeler ve işletim sistemi sürümlerinde desteklenir.

|Rol| İşletim sistemi / sürüm|
|--|--|
|Active Directory Federasyon Hizmetleri (AD FS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|
|Azure AD Connect | 1.0.9125 sürümü veya üzeri|
|Active Directory etki alanı Hizmetleri (AD DS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|

Hizmet tarafından sağlanan özellikleri rol ile işletim sistemine göre değişebileceğini unutmayın. Diğer bir deyişle, tüm özellikler tüm işletim sistemi sürümleri için kullanılabilir olmayabilir. Ayrıntılar için özellik açıklamaları bakın.

**S: Altyapımı izlemek kaç tane lisansın gerekiyor?**

* İlk Connect Health aracısını en az bir Azure AD Premium lisansı gerektirir.
* Her bir ek kayıtlı aracı 25 ek Azure AD Premium lisansı gerektirir.
* Aracı sayısı, tüm izlenen rolleri arasında (AD FS, Azure AD Connect ve AD DS) kayıtlı aracı toplam sayısını eşdeğerdir.
* AAD Connect Health lisans için belirli kullanıcılara lisans atamanız gerekmez. Yalnızca geçerli lisans önkoşul sahip olması gerekir.

Lisans bilgilerini üzerinde bulunan ayrıca [Azure AD fiyatlandırma sayfası](https://aka.ms/aadpricing).

Örnek:

| Kayıtlı aracı | Gerekli lisansları | Örnek izleme yapılandırması |
| ------ | --------------- | --- |
| 1 | 1 | 1 azure AD Connect sunucusu |
| 2 | 26| 1 azure AD Connect sunucusu ve 1 etki alanı denetleyicisi |
| 3 | 51 | 1 active Directory Federasyon Hizmetleri (ADFS) sunucusu, 1 AD FS proxy ve 1 etki alanı denetleyicisi |
| 4 | 76 | 1 AD FS sunucusu, 1 AD FS proxy ve 2 etki alanı denetleyicileri |
| 5 | 101 | 1 azure AD Connect sunucusu, 1 AD FS sunucusu, 1 AD FS proxy ve 2 etki alanı denetleyicileri |

**S: Azure AD Connect Health, Azure Almanya Bulutu destekliyor mu?**

Azure AD Connect Health, Almanya bulutunda dışında desteklenmiyor [eşitleme hata raporu özelliği](how-to-connect-health-sync.md#object-level-synchronization-error-report).

| Roller | Özellikler | Alman bulutunda desteklenir |
| ------ | --------------- | --- |
| Eşitleme için connect Health | İzleme / Insight / uyarıları / analizi | Hayır |
|  | Eşitleme hata raporu | Evet |
| AD FS için connect Health | İzleme / Insight / uyarıları / analizi | Hayır |
| EKLER için connect Health | İzleme / Insight / uyarıları / analizi | Hayır |

Eşitleme için Connect Health Aracısı bağlantısı sağlamak için lütfen yapılandırma [yükleme gereksinimini](how-to-connect-health-agent-install.md#outbound-connectivity-to-the-azure-service-endpoints) uygun şekilde.

## <a name="installation-questions"></a>Yükleme soruları

**S: Tek başına sunucularda Azure AD Connect Health Aracısı yükleme etkisi nedir?**

Yükleme Microsoft Azure AD Connect Health Aracısı, AD FS, web uygulaması Ara sunucuları, Azure AD Connect (eşitleme) sunucuları, etki alanı denetleyicileri etkisidir CPU, bellek kullanımı, ağ bant genişliği ve depolama göre en az.

Aşağıdaki numaralarını yaklaşık şunlardır:

* CPU tüketimi: yaklaşık 1-%5 artırın.
* Bellek tüketimi: Toplam sistem belleği en fazla % 10.

> [!NOTE]
> Aracısı, Azure ile iletişim kuramıyor, aracıyı yerel olarak tanımlanan bir üst sınırı için verileri depolar. Aracı "az yakın bir zamanda hizmet" olarak "önbelleğe alınan" verilerin üzerine yazar.
>
>

* Azure AD Connect Health aracıları için yerel arabellek depolama alanı: yaklaşık 20 MB.
* AD FS sunucuları için 1024 MB (1 GB) disk alanının sağlamak için Azure AD Connect Health, yazılmadan önce tüm denetim verilerini işlemek aracıları için AD FS denetim kanalı öneririz.

**S: Azure AD Connect Health aracılarını yükleme sırasında Sunucularım yeniden başlatma gerekecek mi?**

Hayır. Aracıların yüklemesi, sunucunun yeniden başlatılmasını gerektirmez. Ancak, bazı önkoşul adımlarını yüklenmesi sunucunun yeniden başlatılmasını gerektirebilir.

Örneğin, Windows Server 2008 R2'de .NET 4.5 Framework'ün yüklemesini bir sunucu yeniden başlatma gerekiyor.

**S: Azure AD Connect Health, doğrudan bir HTTP proxy üzerinden çalışır mı?**

Evet. Devam eden işlemler için giden HTTP isteklerini iletmek için bir HTTP proxy kullanmak için Sistem Durumu Aracısı'nı yapılandırabilirsiniz.
Daha fazla bilgi edinin [Health aracılarını HTTP Proxy Yapılandırma](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy).

Aracı kaydı sırasında bir proxy yapılandırmanız gerekiyorsa, Internet Explorer Proxy ayarlarınızı önceden değiştirmek gerekebilir.

1. Internet Explorer'ı açın > **ayarları** > **Internet Seçenekleri** > **bağlantıları** > **LAN Ayarları** .
2. Seçin **AĞINIZ için bir Proxy sunucusu kullan**.
3. Seçin **Gelişmiş** farklı proxy bağlantı noktası için HTTP ve HTTPS/güvenli varsa.

**S: Azure AD Connect Health için HTTP Proxy'si bağlanırken, temel kimlik doğrulamasını destekliyor mu?**

Hayır. Bir rastgele kullanıcı adı ve temel kimlik doğrulaması için parola belirtmek için bir mekanizma şu anda desteklenmiyor.

**S: Azure AD Connect Health çalışmaya aracısı için açmak hangi güvenlik duvarı bağlantı noktaları gerekir?**

Bkz: [gereksinimler bölümüne](how-to-connect-health-agent-install.md#requirements) güvenlik duvarı bağlantı noktaları ve diğer bağlantı gereksinimleri listesi için.

**S: Azure AD Connect Health Portalı'nda aynı ada sahip iki sunucu neden görüyorum?**

Bir sunucudan bir aracıyı kaldırdığınızda, sunucunun Azure AD Connect Health Portalı'ndan otomatik olarak kaldırılmaz. El ile bir sunucudan bir aracı kaldırma veya sunucunun kendisini kaldırırsanız, sunucu giriş Azure AD Connect Health portalından el ile silmeniz gerekir.

Bir sunucu görüntüsünü yeniden ya da aynı ayrıntılarını (örneğin, makine adı) ile yeni bir sunucu oluşturun. Azure AD Connect Health Portalı'ndan zaten kayıtlı sunucu kaldırmadı ve aracıyı yeni sunucuda yüklü ise, aynı ada sahip iki girişe görebilirsiniz.

Bu durumda, daha eski bir sunucuya ait Giriş el ile silin. Bu sunucu için verilerin güncel olması gerekir.

## <a name="health-agent-registration-and-data-freshness"></a>Sistem Durumu Aracısı kaydı ve veri yenilik

**S: Sistem Durumu Aracısı kayıt hatalarının sık karşılaşılan nedenleri nelerdir ve sorunları nasıl giderebilirim?**

Durum Aracısı, olası nedeni şunlar kaydetmek başarısız olabilir:

* Trafiği bir güvenlik duvarı tarafından engellendiği için Aracısı gerekli uç noktaları ile iletişim kuramıyor. Bu, özellikle web uygulaması Ara sunucularında yaygındır. Gerekli uç noktaları ve bağlantı noktalarına giden iletişime izin emin olun. Bkz: [gereksinimler bölümüne](how-to-connect-health-agent-install.md#requirements) Ayrıntılar için.
* Giden iletişimin ağ katmanı tarafından SSL incelemeden bağımlıdır. Bu İnceleme sunucu/varlık tarafından değiştirilmesini aracıyı kullanan sertifika neden olur ve aracı kaydını tamamlamak için gereken adımları başarısız.
* Kullanıcı aracı kaydını gerçekleştirme erişimine sahip değil. Genel Yöneticiler, varsayılan olarak erişimi vardır. Kullanabileceğiniz [rol tabanlı erişim denetimi](how-to-connect-health-operations.md#manage-access-with-role-based-access-control) diğer kullanıcılara erişim vermek.

**S: Ben "sistem sağlığı hizmeti verileri güncel değil," uyarı Bu sorunu nasıl giderebilirim?**

Azure AD Connect Health, tüm veri noktalarına sunucudan son iki saat içinde almazsa bir uyarı oluşturur. [Daha fazla bilgi edinin](how-to-connect-health-data-freshness.md).

## <a name="operations-questions"></a>İşlem soruları
**S: Web uygulaması Ara sunucularında denetimi etkinleştirme gerekiyor mu?**

Hayır, denetim web uygulaması Ara sunucularında etkinleştirilmesi gerekmez.

**S: Azure AD Connect Health uyarıları nasıl çözümleneceğini?**

Azure AD Connect Health uyarıları bir başarı koşulu temel çözülebilir. Azure AD Connect Health aracılarını algılamak ve başarılı koşullar hizmete düzenli aralıklarla bildirin. Birkaç uyarı gizleme zamana bağlı. Diğer bir deyişle, aynı hata durumu, uyarı oluşturma 72 saat içinde uyulmuyor, uyarı otomatik olarak çözülür.

**S: Ben "Test kimlik doğrulama isteği (yapay işlem) bir belirteç almak başarısız olduğunu." uyarı Bu sorunu nasıl giderebilirim?**

Azure AD Connect Health AD FS için sistem durumu aracısı tarafından başlatılan bir yapay işlem bir parçası olarak bir belirteç almak bir AD FS sunucusunda yüklü Sistem Durumu Aracısı başarısız olduğunda bu uyarı oluşturur. Sistem Durumu Aracısı yerel sistem bağlamı kullanır ve kendi kendine bağlı olan taraf için bir belirteç almaya çalışır. AD FS belirteçleri verme durumunda olduğundan emin olmak için bir genel test budur.

Genellikle AD FS grubu adını çözümlemek sistem durumu aracısı silemiyor çünkü bu test başarısız olur. AD FS sunucuları ağ yük dengeleyicilerini istek (aksine, önündeki yük dengeleyici olan normal bir istemci) yük dengeleyicinin arkasındaki olan düğümün başlatıldığı kullanıyorsanız bu durum ortaya çıkabilir. Bu, "AD FS sunucusunun IP adresini veya AD FS grubu adı (örneğin, sts.contoso.com) için bir geri döngü IP adresi (127.0.0.1) içerecek şekilde C:\Windows\System32\drivers\etc" altında bulunan "ana bilgisayarlar" dosyasını güncelleştirerek düzeltilebilir. Ana bilgisayar dosyası ekleme, böylece belirteci almak sistem durumu aracısı izin vererek ağ çağrısı, kısa devre oluşturur.

**S: İçin son fidye yazılımı saldırılarında makinelerime yama değil belirten bir e-posta aldım. Bu e-postayı neden aldınız mı?**

Azure AD Connect Health hizmeti tüm gerekli düzeltme eklerini emin olmak için izler makinelere yüklenen taranır. En az bir makine kritik düzeltme ekleri yoksa Kiracı yöneticilerine e-posta gönderildi. Aşağıdaki mantık, bunun belirlenmesi için kullanıldı.
1. Makinede yüklü tüm düzeltmeler bulun.
2. Düzeltmeleri tanımlı listeden en az biri mevcut olup olmadığını denetleyin.
3. Yanıt Evet ise, makine korunuyor. Aksi durumda, saldırı risk makinedir.

El ile bu denetimi gerçekleştirmek için aşağıdaki PowerShell betiğini kullanabilirsiniz. Yukarıdaki mantığını uygular.

```powershell
Function CheckForMS17-010 ()
{
    $hotfixes = "KB3205409", "KB3210720", "KB3210721", "KB3212646", "KB3213986", "KB4012212", "KB4012213", "KB4012214", "KB4012215", "KB4012216", "KB4012217", "KB4012218", "KB4012220", "KB4012598", "KB4012606", "KB4013198", "KB4013389", "KB4013429", "KB4015217", "KB4015438", "KB4015546", "KB4015547", "KB4015548", "KB4015549", "KB4015550", "KB4015551", "KB4015552", "KB4015553", "KB4015554", "KB4016635", "KB4019213", "KB4019214", "KB4019215", "KB4019216", "KB4019263", "KB4019264", "KB4019472", "KB4015221", "KB4019474", "KB4015219", "KB4019473"

    #checks the computer it's run on if any of the listed hotfixes are present
    $hotfix = Get-HotFix -ComputerName $env:computername | Where-Object {$hotfixes -contains $_.HotfixID} | Select-Object -property "HotFixID"

    #confirms whether hotfix is found or not
    if (Get-HotFix | Where-Object {$hotfixes -contains $_.HotfixID})
    {
        "Found HotFix: " + $hotfix.HotFixID
    } else {
        "Didn't Find HotFix"
    }
}

CheckForMS17-010

```

**S: PowerShell cmdlet neden mu <i>Get-MsolDirSyncProvisioningError</i> sonuçta daha az eşitleme hatalarını göster?**

<i>Get-MsolDirSyncProvisioningError</i> yalnızca DirSync hazırlama hataları döndürür. Yanı sıra, Connect Health portalı ayrıca diğer eşitleme dışarı aktarma hataları gibi hata türlerini gösterir. Bu, Azure AD Connect delta sonucu ile tutarlıdır. Daha fazla bilgi edinin [Azure AD Connect Eşitleme hataları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sync-errors).

**S: My ADFS neden olduğunu değil oluşturulmasını denetler?**

Lütfen PowerShell cmdlet'ini kullanın <i>Get-AdfsProperties - AuditLevel</i> denetim günlüklerini değil de emin olmak için durumu devre dışı. Daha fazla bilgi edinin [AD FS denetim günlüklerini](https://docs.microsoft.com/windows-server/identity/ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server#auditing-levels-in-ad-fs-for-windows-server-2016). AD FS sunucusuna gönderilen denetim ayarları var. Gelişmiş, auditpol.exe ile herhangi bir değişiklik (uygulama üretilen yapılandırılmamışsa üzerine olay) olacaktır dikkat edin. Bu durumda, lütfen üretilen uygulama hatalarını ve başarı günlüğe kaydetmek için yerel güvenlik ilkesi ayarlayın.

**S: Aracı sertifikası, otomatik yenilenen önce sona erme ne zaman sunulacaktır?**
Aracı sertifika otomatik olarak yenilenmesi **6 ay** , sona erme tarihinden önce. Lütfen yenilenmezse, kararlı Aracısı'nın ağ bağlantısı olduğundan emin olun. Aracı hizmetleri yeniden başlatın veya en son sürüme güncelleştirme de sorunu çözebilir.


## <a name="related-links"></a>İlgili bağlantılar
* [Azure AD Connect Health](whatis-hybrid-identity-health.md)
* [Azure AD Connect Health Aracısı yüklemesi](how-to-connect-health-agent-install.md)
* [Azure AD Connect Health işlemleri](how-to-connect-health-operations.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](how-to-connect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](how-to-connect-health-sync.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](how-to-connect-health-adds.md)
* [Azure AD Connect Health sürüm geçmişi](reference-connect-health-version-history.md)
