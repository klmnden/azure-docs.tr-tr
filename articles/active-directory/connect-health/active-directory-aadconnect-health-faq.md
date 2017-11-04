---
title: Azure Active Directory Connect sistem durumu ile ilgili SSS - Azure | Microsoft Docs
description: "Bu SSS, Azure AD Connect Health hakkında sorular yanıtlanmaktadır. Bu SSS; faturalama modeli, özellikler, sınırlamalar ve destek dahil olmak üzere hizmetin kullanımı hakkındaki soruları kapsar."
services: active-directory
documentationcenter: 
author: billmath
manager: samueld
editor: curtand
ms.assetid: f1b851aa-54d7-4cb4-8f5c-60680e2ce866
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 0c32ac27187a88dd13bb747f541968d2e81c5064
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="azure-ad-connect-health-frequently-asked-questions"></a>Azure AD Connect Health hakkında sık sorulan sorular
Bu makale, Azure Active Directory (Azure AD) Connect Health hakkında sık sorulan sorular (SSS) yanıtlarını içerir. Bu SSS, özellikleri, sınırlamalar ve Destek faturalama modeli içerir hizmetini kullanma hakkında sorular kapsar.

## <a name="general-questions"></a>Genel sorular
**S: birden çok Azure AD dizinlerinden yönetin. Azure Active Directory Premium sahip bir nasıl geçiş yapabilirim?**

Farklı arasında geçiş yapmak için Azure AD kiracıları seçin şu anda açan **kullanıcı adı** üzerinde sağ üst köşe ve ardından uygun hesabı seçin. Hesap buraya listelenmemişse seçin **oturumu**ve ardından Azure Active Directory Premium oturum açmak için etkin olan dizin genel yönetici kimlik bilgilerini kullanın.

**S: hangi sürümünün kimlik rolleri Azure AD Connect Health tarafından destekleniyor mu?**

Aşağıdaki tabloda rolleri listeler ve desteklenen işletim sistemi sürümleri.

|Rol| İşletim sistemi / sürümü|
|--|--|
|Active Directory Federasyon Hizmetleri (AD FS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|
|Azure AD Connect | Sürüm 1.0.9125 veya üzeri|
|Active Directory etki alanı Hizmetleri (AD DS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|

Hizmet tarafından sağlanan özellikleri rolü ve işletim sistemine göre farklılık gösterebilir unutmayın. Diğer bir deyişle, tüm özellikler tüm işletim sistemi sürümleri için kullanılamayabilir. Özellik açıklamaları Ayrıntılar için bkz.

**S: my altyapısını izlemek kaç tane gerekiyor mu?**

* İlk Connect Health aracısını en az bir Azure AD Premium lisansı gerektirir.
* Her bir ek kayıtlı aracının 25 ek Azure AD Premium lisansı gerektirir.
* Aracı sayısı, tüm izlenen rolleri arasında (AD FS, Azure AD Connect ve/veya AD DS) kayıtlı olan aracıları toplam sayısı eşdeğerdir.

Lisans bilgileri üzerinde bulunan de [Azure AD fiyatlandırma sayfası](https://aka.ms/aadpricing).

Örnek:

| Kayıtlı aracıları | Gerekli lisansları | Örnek izleme yapılandırması |
| ------ | --------------- | --- |
| 1 | 1 | 1 azure AD Connect sunucusu |
| 2 | 26| 1 azure AD Connect sunucusu ve 1 etki alanı denetleyicisi |
| 3 | 51 | 1 active Directory Federasyon Hizmetleri (AD FS) server, 1 AD FS proxy ve 1 etki alanı denetleyicisi |
| 4 | 76 | 1 AD FS sunucusu, 1 AD FS proxy ve 2 etki alanı denetleyicileri |
| 5 | 101 | 1 azure AD Connect sunucusu, 1 AD FS sunucusu, 1 AD FS proxy ve 2 etki alanı denetleyicileri |

**S: Azure AD Connect Health destek Azure Almanya bulut mu?**

Azure AD Connect Health sahip bir [yükleme](active-directory-aadconnect-health-agent-install.md) Azure Almanya için. Almanca bulut müşteriler için tüm verileri Azure Almanya bulut içinde tutulur.


## <a name="installation-questions"></a>Yükleme soruları

**S: Azure AD Connect Health Aracısı tek başına sunucularda yükleme etkisi nedir?**

Yükleme Microsoft Azure AD Connect Health Aracısı, AD FS, web uygulaması proxy sunucuları, Azure AD Connect (eşitleme) sunucuları, etki alanı denetleyicileri etkisidir CPU, bellek kullanımı, ağ bant genişliği ve depolama göre en az.

Şu sayıları yaklaşık şunlardır:

* CPU tüketimi: % ~ 1-5 artırma.
* Bellek tüketimi: toplam sistem belleğinin % 10'ye kadar.

> [!NOTE]
> Aracıyı Azure ile iletişim kuramıyorsa aracı yerel olarak tanımlanmış bir üst sınırı için verileri depolar. Aracı "yakın zamanda hizmet" olarak "önbelleğe alınan" verilerin üzerine yazar.
>
>

* Azure AD Connect Health aracıları için yerel arabellek Depolama: ~ 20 MB.
* AD FS sunucuları için 1024 MB (1 GB) disk alanı sağlamak için Azure AD Connect Health, yazılmadan önce tüm denetim verileri işlemek aracıları için AD FS denetim kanalı öneririz.

**S: Sunucularım Azure AD Connect Health aracılarını yükleme sırasında yeniden başlatma gerekecek mi?**

Hayır. Aracıların yüklenmesi sunucunun yeniden başlatılmasını gerektirmez. Ancak, bazı adımlar yüklenmesi sunucunun yeniden başlatılmasını gerektirebilir.

Örneğin, Windows Server 2008 R2'de .NET Framework 4.5 yüklemesi bir sunucu yeniden başlatma gerekiyor.

**S: Azure AD Connect Health iş doğrudan bir HTTP proxy üzerinden mi?**

Evet. Devam eden işlemler için sistem durumu aracısı giden HTTP isteklerini iletmek için bir HTTP Ara sunucusunu kullanacak şekilde yapılandırabilirsiniz.
Daha fazla bilgi edinin [Health aracılarını HTTP proxy'sini yapılandırma](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy).

Aracı kaydı sırasında bir proxy sunucu yapılandırmak gerekirse, Internet Explorer Proxy ayarlarınızı önceden değiştirmeniz gerekebilir.

1. Internet Explorer'ı Aç > **ayarları** > **Internet Seçenekleri** > **bağlantıları** > **LAN Ayarları**.
2. Seçin **AĞINIZ için bir Proxy sunucusu kullan**.
3. Seçin **Gelişmiş** HTTP ve HTTPS/güvenli için farklı bir proxy bağlantı noktası varsa.

**S: Azure AD Connect Health destek temel kimlik doğrulaması HTTP Proxy'si için bağlanırken mu?**

Hayır. Bir isteğe bağlı bir kullanıcı adı ve temel kimlik doğrulaması için parola belirtmek için bir mekanizma şu anda desteklenmiyor.

**S: Azure AD Connect Health çalışmaya aracısı için açmak hangi güvenlik duvarı bağlantı noktalarını gerekiyor mu?**

Bkz: [gereksinimleri bölümüne](active-directory-aadconnect-health-agent-install.md#requirements) güvenlik duvarı bağlantı noktaları ve diğer bağlantı gereksinimleri listesi.

**Azure AD Connect Health Portalı'nda aynı ada sahip iki sunucu neden görüyor musunuz?**

Bir sunucudan bir aracıyı kaldırdığınızda, sunucunun Azure AD Connect Health Portalı'ndan otomatik olarak kaldırılmaz. El ile bir sunucudan bir aracı kaldırma veya sunucu kaldırırsanız, Azure AD Connect Health Portalı'ndan sunucu girdisini el ile silmeniz gerekir.

Bir sunucu görüntüsünü yeniden oluşturmak veya yeni bir sunucu (örneğin, makine adı) aynı ayrıntılarla oluşturun. Azure AD Connect Health Portalı'ndan zaten kayıtlı sunucu kaldırmadı ve aracının yeni sunucuda yüklü, aynı ada sahip iki girişleri görebilirsiniz.

Bu durumda, daha eski bir sunucuya ait girdisini el ile silin. Bu sunucu için veriler güncel olmalıdır.

## <a name="health-agent-registration-and-data-freshness"></a>Sistem Durumu Aracısı kaydı ve veri yenilik

**S: sistem durumu aracısı kayıt hataları yaygın nedenler nelerdir ve sorunları nasıl giderebilirim?**

Sistem Durumu Aracısı, olası nedeni şunlar kaydetmek başarısız olabilir:

* Bir güvenlik duvarı trafiği engelleyen olduğundan aracı gerekli uç noktalar ile iletişim kuramıyor. Bu, özellikle web uygulama proxy sunucularında yaygındır. Gerekli uç noktaları ve bağlantı noktalarına giden iletişime izin emin olun. Bkz: [gereksinimleri bölümüne](active-directory-aadconnect-health-agent-install.md#requirements) Ayrıntılar için.
* Giden iletişim için SSL denetlemesi ağ katmanı tarafından bağımlıdır. Bu aracının denetleme sunucu/varlık tarafından değiştirilmesi için kullandığı sertifikayı neden olur ve aracı kaydını tamamlamak için gereken adımları başarısız.
* Kullanıcı Aracısı'nın kayıt gerçekleştirmek için erişim yok. Genel Yöneticiler, varsayılan olarak erişimi. Kullanabileceğiniz [rol tabanlı erişim denetimi](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) diğer kullanıcılara yetki vermek için.

**S: "sistem sağlığı hizmeti verileri güncel değil," uyarı Sorun giderme nasıl?**

Azure AD Connect Health, tüm veri noktaları sunucudan son iki saat içinde almazsa bir uyarı oluşturur. Bu uyarı için birden çok nedeni olabilir.

* Bir güvenlik duvarı trafiği engelleyen olduğundan aracı gerekli uç noktalar ile iletişim kuramıyor. Bu, özellikle web uygulama proxy sunucularında yaygındır. Gerekli uç noktalar ve bağlantı noktalarına giden iletişime izin emin olun. Bkz: [gereksinimleri bölümüne](active-directory-aadconnect-health-agent-install.md#requirements) Ayrıntılar için.
* Giden iletişim için SSL denetlemesi ağ katmanı tarafından bağımlıdır. Bu aracının denetleme sunucu/varlık tarafından değiştirilmesi için kullandığı sertifikayı neden olur ve Azure AD Connect Health hizmetine veri yüklemeye işlemi başarısız olur.
* Aracı yerleşik bağlantı komutunu kullanabilirsiniz. [Daha fazla bilgi edinin](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service).
* Aracılar Ayrıca kimliği doğrulanmamış bir HTTP Proxy üzerinden giden bağlantıyı destekler. [Daha fazla bilgi edinin](active-directory-aadconnect-health-agent-install.md##configure-azure-ad-connect-health-agents-to-use-http-proxy).

## <a name="operations-questions"></a>Operations sorular
**S: web uygulaması Ara sunucularında denetimi etkinleştirmeniz gerekiyor mu?**

Hayır, denetim web uygulama proxy sunucularda etkinleştirilmesi gerekmez.

**S: nasıl Azure AD Connect Health uyarıları çözülmesi?**

Azure AD Connect Health uyarıları, bir başarı koşulu çözülmüş. Azure AD Connect Health aracılarını algılamak ve başarılı koşullar hizmete düzenli aralıklarla rapor. Birkaç uyarıları gizleme zaman tabanlıdır. Aynı hata durumuna uyarı oluşturma 72 saat içinde gözlenir değil, diğer bir deyişle, uyarı otomatik olarak çözümlenir.

**S: "Test kimlik doğrulama isteği (yapay işlem) bir belirteci edinmesine başarısız olduğunu." uyarı Sorun giderme nasıl?**

Azure AD Connect Health AD FS için yapay bir işlem sistem durumu aracısı tarafından başlatılan bir parçası olarak bir belirteç elde etmek bir AD FS sunucusunda yüklü Sistem Durumu Aracısı başarısız olduğunda bu uyarı oluşturur. Sistem Durumu Aracısı yerel sistem bağlamında kullanır ve kendi kendine bağlı olan taraf için bir belirteç almaya çalışır. Bu AD FS belirteçleri verme durumunda olduğundan emin olmak için bir catch tüm denemedir.

Sistem Durumu Aracısı AD FS grup adı çözümlemek edemediği çoğunlukla bu sınama başarısız olur. Bu durum bir ağ yük dengeleyici AD FS sunucuları varsa ve bu isteği (aksine yük dengeleyicinin önünde normal istemci) yük dengeleyici arkasında olduğu bir düğümden başlatılan meydana gelebilir. Bu, AD FS sunucusunun IP adresi veya AD FS grubu adı (örneğin, sts.contoso.com) için bir geri döngü IP adresi (127.0.0.1) dahil etmek için "C:\Windows\System32\drivers\etc" altında bulunan "hosts" dosyasını güncelleştirerek düzeltilebilir. Ana bilgisayar dosyası ekleme, böylece belirteci almak sistem durumu aracısı izin vererek ağ çağrısı kısa devre oluşturur.

**S: my makineler için son ransomeware saldırıları düzeltme değil belirten bir e-posta aldı. Bu e-posta neden aldınız mı?**

Azure AD Connect Health hizmeti tüm gerekli düzeltme eklerini emin olmak için izlediği makinelere yüklenen taraması. En az bir makine kritik yazılım düzeltme eklerini değilse Kiracı Yöneticiler e-posta gönderildi. Aşağıdaki mantık, bunun belirlenmesi için kullanıldı.
1. Makinede yüklü olan tüm düzeltmeleri bulun.
2. Düzeltmeleri tanımlanmış listeden en az biri yüklü olup olmadığını denetleyin.
3. Yanıt Evet ise, makine korunuyor. Aksi durumda, saldırı risk makinesidir.

Bu denetimi el ile gerçekleştirmek için aşağıdaki PowerShell betiğini kullanabilirsiniz. Yukarıdaki mantığı uygular.

```
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



## <a name="related-links"></a>İlgili bağlantılar
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Aracısı yüklemesi](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health işlemleri](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](active-directory-aadconnect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health sürüm geçmişi](active-directory-aadconnect-health-version-history.md)
