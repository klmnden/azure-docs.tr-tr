---
title: StorSimple 8000 serisi güvenlik | Microsoft Docs
description: StorSimple hizmet, cihaz ve şirket içindeki ve buluttaki verileri korumak güvenlik ve gizlilik özellikleri açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/18/2018
ms.author: alkohli
ms.openlocfilehash: 734b0cf9373ea98ab33c06b45ad53b46a3355dd6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62117034"
---
# <a name="storsimple-security-and-data-protection"></a>StorSimple güvenlik ve veri koruma

## <a name="overview"></a>Genel Bakış

Güvenlik, herhangi bir teknoloji ile gizli veya özel verileri özellikle kullanıldığında, kullanıcı yeni bir Teknoloji benimseme önemli bir konudur. Farklı teknolojilerde değerlendirirken, daha yüksek riskleri ve veri koruma maliyetlerini dikkate almanız gerekir. Microsoft Azure StorSimple güvenlik ve gizlilik çözüm veri korumasına yardımcı olmak için sağlar:

* **Gizlilik** – yalnızca yetkili varlıkları verilerinizi görüntüleyebilirsiniz.
* **Bütünlük** – yalnızca yetkili varlıklar değiştirebilir veya verilerinizi silebilirsiniz.

Microsoft Azure StorSimple çözümü birbiriyle etkileşim dört ana bileşenden oluşur:

* **StorSimple cihaz Yöneticisi hizmeti Microsoft Azure'da barındırılan** – yapılandırmak ve StorSimple cihaz sağlama için kullandığınız yönetim hizmeti.
* **StorSimple cihaz** – veri merkezinizde yüklü fiziksel bir cihaz. Tüm ana bilgisayarlar ve veri üretme istemciler için StorSimple cihazı bağlayın ve cihaz verileri yönetir ve Azure bulutuna taşır uygun şekilde.
* **İstemciler/ana bilgisayarları bağlı cihaza** – için StorSimple cihazı bağlayın ve korunması gereken veriler oluşturabilen istemcilerin altyapınızdaki.
* **Bulut depolama** – Azure bulutunda verilerin depolandığı konum.

Aşağıdaki bölümlerde, her biri bu bileşenler ve bunlar üzerinde depolanan verileri korumaya yardımcı olmak StorSimple güvenlik özellikleri açıklanmaktadır. Ayrıca, Microsoft Azure StorSimple güvenliği ve karşılık gelen yanıtları ilgili olabilecek sorular listesini içerir.

## <a name="storsimple-device-manager-service-protection"></a>StorSimple cihaz Yöneticisi hizmeti koruma

StorSimple cihaz Yöneticisi hizmeti Microsoft Azure'da barındırılan ve kuruluşunuzun temin tüm StorSimple cihazları yönetmek için kullanılan bir yönetim hizmetidir. StorSimple cihaz Yöneticisi hizmeti bir web tarayıcısı üzerinden Azure portalında oturum açmak için Kurumsal kimlik bilgilerinizi kullanarak erişebilirsiniz.

StorSimple cihaz Yöneticisi hizmetine erişim, kuruluşunuzun StorSimple içeren bir Azure aboneliği olmasını gerektirir. Aboneliğiniz, Azure portalında erişebildiğiniz özellikleri yönetir. Kuruluşunuzun bir Azure aboneliğine sahip değil ve isterseniz bunları hakkında daha fazla bilgi için bkz [Azure'a kuruluş olarak kaydolma](../active-directory/fundamentals/sign-up-organization.md).

StorSimple cihaz Yöneticisi hizmetine Azure'da barındırıldığı için Azure güvenlik özellikleri tarafından korunur. Microsoft Azure tarafından sağlanan güvenlik özellikleri hakkında daha fazla bilgi için [Microsoft Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/security/)’ne gidin.

## <a name="storsimple-device-protection"></a>StorSimple cihaz koruma

StorSimple cihazı katı hal sürücüleri (SSD'ler) ve sabit disk sürücülerinin (HDD'ler), yedekli denetleyicileri ve otomatik yük devretme özellikleri ile birlikte içeren bir şirket içi karma depolama cihazdır. Depolama katmanlamayı, şu anda kullanılan (veya sık erişimli) veri (StorSimple cihazı veya şirket içi sunucular için), yerel depolama bulut için daha az sık kullanılan veri taşırken yerleştirerek denetleyicilerini yönetin.

Yalnızca StorSimple cihazları, Azure aboneliğinizde oluşturduğunuz StorSimple cihaz Yöneticisi hizmetine ekleme izni olan yetkili. Bir cihaz yetkilendirmek için StorSimple cihaz Yöneticisi hizmeti ile hizmet kayıt anahtarı sağlayarak kaydetmelisiniz. Hizmet kayıt anahtarını Azure portalında oluşturulan bir 128-bit rastgele anahtardır.

![Hizmet kayıt anahtarı](./media/storsimple-security/ServiceRegistrationKey.png)

Bilgi edinmek için nasıl bir Git hizmet kayıt anahtarını Al [2. adım: Hizmet kayıt anahtarı alma](storsimple-8000-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).

Hizmet kayıt anahtarını 100'den fazla karakter içeren bir uzun bir anahtardır. Anahtarı kopyalayın ve gerektiğinde ek cihazlar yetkilendirmek için kullanabilmesi için güvenli bir konumda metin dosyasına kaydedin. İlk aygıtınızı kaydettikten sonra hizmet kayıt anahtarını kaybolursa, yeni bir anahtar StorSimple cihaz Yöneticisi hizmetinden oluşturabilirsiniz. Bu işlemi var olan cihazları etkilemez.

Bir cihaz kaydedildikten sonra Microsoft Azure ile iletişim kurmak için belirteçleri kullanır. Hizmet kayıt anahtarını, cihaz kaydından sonra kullanılmaz.

> [!NOTE]
> Her kullanımdan sonra hizmet kayıt anahtarını yeniden öneririz.


## <a name="protect-your-storsimple-solution-via-passwords"></a>StorSimple çözümünüzü parolaları aracılığıyla koruma

Parolalar, bir bilgisayar güvenliği için önemlidir ve verilerinizi yalnızca yetkili kullanıcılar için erişilebilir olduğundan emin olun yardımcı olmak için StorSimple çözümünün azure'da yaygın olarak kullanılır. StorSimple, aşağıdaki parolalar yapılandırmanıza olanak sağlar:

* StorSimple cihaz Yöneticisi parolası
* Kimlik Doğrulama Protokolü (CHAP) Başlatıcısı ve hedef parolaları meydan okuyun
* StorSimple Snapshot Manager parolası

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>Windows PowerShell, StorSimple ve StorSimple cihaz Yöneticisi parolası

StorSimple için Windows PowerShell, StorSimple Cihazınızı yönetmek için kullanabileceğiniz bir komut satırı arabirimidir. StorSimple için Windows PowerShell, Cihazınızı kaydetmek, Cihazınızda ağ arabirimini yapılandırın, belirli türden güncelleştirmeler yüklemeniz, destek oturumu erişerek cihazınızın sorunlarını olanak sağlar ve cihaz durumunu değiştir özelliklere sahiptir. Cihazdaki seri konsoluna bağlanarak veya Windows PowerShell uzaktan iletişimini kullanarak StorSimple için Windows PowerShell erişebilirsiniz.

PowerShell uzaktan iletişimini HTTPS veya HTTP üzerinden yapılabilir. HTTPS üzerinden uzaktan yönetimi etkinse, CİHAZDAN uzaktan yönetim sertifikası indirin ve uzak istemciye yüklemek gerekecektir. PowerShell uzaktan iletişimi hakkında daha fazla bilgi için Git [StorSimple cihazınıza uzaktan bağlanma](storsimple-8000-remote-connect.md).

StorSimple için Windows PowerShell cihaza bağlanmak için kullandıktan sonra cihaza oturum açmak için cihaz yönetici parolası girmeniz gerekir.

![Cihaz yöneticisi parolası](./media/storsimple-security/DeviceAdminPW.png)

Aşağıdaki en iyi uygulamaları göz önünde bulundurun:

* Uzaktan Yönetim varsayılan olarak kapalıdır. StorSimple cihaz Yöneticisi hizmetini etkinleştirmek için kullanabilirsiniz. Güvenlik açısından en iyisi, uzaktan erişim, yalnızca gerçekten gerekli zaman diliminde etkin olması.
* Parolayı değiştirirseniz, böylece beklenmedik bağlantı kaybı yaşamazsınız tüm uzaktan erişim kullanıcıları bilgilendir emin olun.
* StorSimple cihaz Yöneticisi hizmeti, var olan parolaların alınamıyor: Bu yalnızca bunları sıfırlayabilirsiniz. Unutulursa, parola sıfırlama gerekmez, tüm parolaları güvenli bir yerde depolamanız önerilir. Parola sıfırlama gerekiyorsa, bunu sıfırlamadan önce tüm kullanıcılara bildirmek emin olun.

Windows PowerShell arabirimini, cihaza bir seri bağlantı kullanarak erişebilirsiniz. Ayrıca, uzaktan HTTP veya ek güvenlik sağlayan HTTPS kullanarak erişebilirsiniz. HTTPS, seri veya HTTP bağlantısı daha yüksek seviyede güvenliği sağlar. Ancak, HTTPS kullanmak için önce bir sertifika cihaz erişen istemci bilgisayarda yüklemelisiniz. Uzaktan erişim sertifikasının, StorSimple cihaz Yöneticisi hizmeti, cihaz yapılandırma sayfasından indirebilirsiniz. Uzaktan erişim için sertifika kaybolursa, yeni bir sertifika indirin ve Uzaktan Yönetimi'ni kullanma yetkisi verilen tüm istemcilere yayar.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Kimlik Doğrulama Protokolü (CHAP) Başlatıcısı ve hedef parolaları meydan okuyun

CHAP, StorSimple cihaz tarafından uzak istemcilerin kimliğini doğrulamak için kullanılan bir kimlik doğrulama düzeni ' dir. Doğrulama bir paylaşılan parolasını temel alır. CHAP tek yönlü olabilir (tek yönlü) veya karşılıklı (çift yönlü). Tek yönlü CHAP ile hedef (StorSimple cihazı) bir başlatıcı (ana bilgisayar) kimliğini doğrular. Karşılıklı veya çift ters CHAP hedefi Başlatıcı kimlik doğrulaması ve Başlatıcı Hedef kimlik doğrulaması yapmak gerekir. StorSimple'ınızı ya da bu yöntemi kullanmak üzere yapılandırılabilir.

CHAP yapılandırdığınızda şunlara dikkat edin:

* CHAP kullanıcı adı, 233'den az karakter içermelidir.
* CHAP parolasının uzunluğu 12 ile 16 karakter arasında olmalıdır. Çalışılıyor, uzun bir kullanıcı adı veya parola kimlik doğrulama hatası Windows konağında neden olur.
* CHAP Başlatıcısı ve CHAP hedefi için aynı parolayı kullanamazsınız.
* Parola ayarladıktan sonra değiştirilmesi gerektiğini ancak geri alınamaz. Parola değiştirildiyse, StorSimple cihazına başarıyla bağlanabilmesi için tüm uzaktan erişim kullanıcıları bilgilendir emin olun.

CHAP ve StorSimple çözümünüz için yapılandırma hakkında daha fazla bilgi için Git [StorSimple cihazınız için CHAP yapılandırma](storsimple-8000-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager parolası

StorSimple Snapshot Manager uygulamayla tutarlı yedeklemeler oluşturmak için birim gruplarını ve Windows birim gölge kopyası hizmeti kullanan bir Microsoft Yönetim Konsolu (MMC) ek bileşenidir. Ayrıca, StorSimple Snapshot Manager yedekleme zamanlamaları ve kopya oluşturmak veya birimleri geri yüklemek için kullanabilirsiniz.

Bir cihaz, StorSimple Snapshot Manager kullanmak üzere yapılandırdığınızda, StorSimple Snapshot Manager parolasını girmeniz gerekir. Bu parola ilk Windows PowerShell StorSimple için kayıt sırasında ayarlanır. Parola ayrıca ayarlayın ve StorSimple cihaz Yöneticisi hizmetinden değiştirildi. Bu parola, StorSimple Snapshot Manager ile cihaz kimliğini doğrular.

![StorSimple Snapshot Manager parolası](./media/storsimple-security/SnapshotMgrPassword.png)

StorSimple Snapshot Manager parolası 14-15 karakter olmalıdır ve 3 veya daha büyük harf, küçük harfler, sayısal ve özel karakterler bir birleşiminden oluşmalıdır. StorSimple Snapshot Manager parolası ayarladıktan sonra değiştirilmesi gerektiğini ancak geri alınamaz. Tüm uzak kullanıcılara bildirmek parolayı değiştirirseniz, unutmayın.

StorSimple Snapshot Manager hakkında daha fazla bilgi için Git [StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Parola en iyi uygulamalar

StorSimple parolalarını güçlü ve iyi korumalı olduğundan emin olmak için aşağıdaki yönergeleri kullanmanızı öneririz:

* Her üç ayda bir parolalarınızı değiştirin. Parolaları değiştirme yıllık zorlanır.
* Güçlü parolalar kullanın. Daha fazla bilgi için Git [güçlü parolalar oluşturma ve bunları koruma](https://cloudblogs.microsoft.com/microsoftsecure/2014/08/25/create-stronger-passwords-and-protect-them/).
* Her zaman farklı parolalar için farklı erişim mekanizmaları kullanın. Belirttiğiniz parola her benzersiz olmalıdır.
* Parolalar StorSimple cihazına erişmek için yetkili değil kimseyle paylaşmayın.
* Başkalarının önünde bir parola hakkında konuşmak değil veya parola biçime bakma ipucu.
* Bir hesabınız veya parolanız tehlikede şüpheleniyorsanız, bilgi güvenlik departmanınız olay bildirin.
* Tüm parolalar, duyarlı, gizli bilgi kabul eder. 

## <a name="storsimple-data-protection"></a>StorSimple veri koruması

Bu bölümde, Taşınmakta olan veriler ve depolanan verileri korumaya StorSimple güvenlik özellikleri açıklanmaktadır.

Diğer bölümlerinde açıklandığı gibi parolaları yetkilendirin ve bunlar StorSimple çözümünüze erişim kazanmadan önce kullanıcıların kimliklerini doğrulamak için kullanılır. Depolama sistemleri arasındaki ve depolanmakta olduğu sırada aktarıldığı sırada başka bir güvenlik önlemleri verilerin yetkisiz kullanıcılara karşı korumaktır. Aşağıdaki bölümlerde, StorSimple ile sağlanan veri koruma özellikleri açıklanmaktadır.

> [!NOTE]
> Yinelenen verileri kaldırma, StorSimple cihazında ve Microsoft Azure Depolama'da depolanan veriler için ek koruma sağlar. Veri yinelenenler kaldırılmadığında veri nesnelerini eşleyin ve bunlara erişmek için kullanılan meta verilerinden ayrı olarak depolanır: birim yapısına, dosya sistemi veya dosya adı göre verileri yeniden oluşturmak için kullanılabilir depolama düzeyi bağlamı yok.


## <a name="protect-data-flowing-through-the-service"></a>Hizmet aracılığıyla akan verileri koruma

Birincil amacı, StorSimple cihaz Yöneticisi hizmeti, StorSimple cihaz yönetip sağlamaktır. StorSimple cihaz Yöneticisi hizmeti, Microsoft Azure üzerinde çalışır. Cihaz yapılandırma verileri girmek için Azure portal'ı kullanın ve ardından cihaza veri göndermek için StorSimple cihaz Yöneticisi hizmeti Microsoft Azure kullanır. StorSimple, Azure hizmetini'nın güvenliğinin içinde depolanan bilgilerin'ın güvenliğinin oluşturmayacaktır sağlamaya yardımcı olmak için asimetrik anahtar çifti bir sistem kullanır.

![Uçuştaki veri şifreleme](./media/storsimple-security/DataEncryption.png)

Asimetrik anahtar sistem hizmet aracılığıyla gibi akan verileri korumaya yardımcı olur:

1. Veri şifreleme sertifikasını, çifti cihazda oluşturulur ve verileri korumak için kullanılan bir asimetrik ortak ve özel anahtarı kullanır. İlk cihaz kaydedildiğinde anahtarları oluşturulur.
2. Veri şifreleme sertifikası anahtarları, ilk cihaz kayıtta rastgele oluşturulan bir tanımlayıcı 128 bit anahtar hizmet veri şifreleme anahtarı tarafından korunan bir kişisel bilgi değişimi (.pfx) dosyasına dışarı aktarılır.
3. Sertifikanın ortak anahtarı güvenli bir şekilde StorSimple cihaz Yöneticisi hizmeti için kullanılabilir hale getirileceğini ve özel anahtarı cihazla kalır.
4. Veri Hizmeti, Azure hizmet veri akışını cihaza şifresi çözülemiyor sağlama ortak anahtar ve cihazda depolanan özel anahtar ile şifresi kullanarak şifrelenir.

Hizmet veri şifreleme anahtarı yalnızca hizmete kayıtlı ilk cihaz üzerinde oluşturulur. Hizmete kayıtlı olan ve sonraki tüm cihazlar aynı hizmet veri şifreleme anahtarını kullanmanız gerekir.

> [!IMPORTANT]
> Hizmet veri şifreleme anahtarının bir kopyasını oluşturun ve güvenli bir konuma kaydetmek çok önemlidir. Hizmet veri şifreleme anahtarının bir kopyasını yetkili bir kişi tarafından erişilebilir ve cihaz Yöneticisi için kolayca bildirilmesi bir şekilde depolanması gerekir.
> 
> Hizmet veri şifreleme anahtarı kaybolursa, bir Microsoft Destek ekibiyle, en az bir cihaz çevrimiçi bir durumda olması koşuluyla, alınacak yardımcı olabilir. Sonra alınan hizmet veri şifreleme anahtarı değiştirmenizi öneririz.

Hizmet veri şifreleme anahtarı ve ilgili veri şifreleme sertifikasını değiştirmek için adımları izleyin. [StorSimple cihaz Yöneticisi hizmeti için hizmet veri şifreleme anahtarı değiştirme](storsimple-8000-manage-service.md#change-the-service-data-encryption-key). Şifreleme anahtarları değiştirmek, tüm cihazlar yeni anahtarla güncelleştirilmesini gerektirir. Bu nedenle, tüm cihazlar çevrimiçi olduğunda bu anahtar değiştirmenizi öneririz. Cihaz çevrimdışı ise, anahtarlarına farklı bir zamanda değiştirilebilir. Güncel olmayan anahtarlar cihazlarla yedeklemeler çalışmaya devam edebilir, ancak bunlar anahtar güncelleştirilene kadar veri geri yükleme olanağınız olmayacaktır.

Hizmet veri şifreleme anahtarı ve veri şifreleme sertifikasını süresi dolmaz. Ancak, yıllık anahtar güvenliğinin aşılması önlemeye yardımcı olmak için hizmet veri şifreleme anahtarı değiştirmenizi öneririz.

## <a name="protect-data-at-rest"></a>Bekleyen verileri koruma

StorSimple cihazı, bu katmanda yerel olarak ve kullanım sıklığına bağlı olarak bulutta depolayarak verileri yönetir. Cihaza bağlı tüm konak makineleri uygun olarak bulutta veri taşınır cihaz verileri gönderebilirsiniz. Veriler CİHAZDAN buluta güvenli bir Internet üzerinden aktarılır. Her cihaz, tüm paylaşılan birimler bu cihazda ortaya çıkarır. bir iSCSI hedef vardır. Tüm verileri bulut depolama gönderilmeden önce şifrelenir. 

![Bulut depolama şifreleme anahtarı](./media/storsimple-security/CloudStorageEncryption.png)

Güvenlik ve buluta taşınan veri bütünlüğünü sağlamaya yardımcı olmak için StorSimple bulut depolama şifreleme anahtarları şu şekilde tanımlamanıza olanak sağlar:

* Birim kapsayıcısı oluşturduğunuzda, bulut depolama şifreleme anahtarı belirtin. Anahtarı değiştirilemez veya daha sonra eklenebilir.
* Bir birim kapsayıcısındaki tüm birimler aynı şifreleme anahtarını paylaşır. Şifreleme belirli bir birim için farklı bir form istiyorsanız, söz konusu birimde barındırmak için yeni bir birim kapsayıcısı oluşturmanızı öneririz.
* Anahtarı, StorSimple cihaz Yöneticisi hizmeti bulut depolama şifreleme anahtarını girin, hizmet veri şifreleme anahtarı ortak kısmını kullanarak ve ardından cihaza gönderilen şifrelenir.
* Bulut depolama şifreleme anahtarı hizmette herhangi bir yerde depolanmaz ve yalnızca cihaz adı verilir.
* Bir bulut depolama şifreleme anahtarı belirten isteğe bağlıdır. Cihaz için ana bilgisayar şifrelenmiş veriler gönderebilir.

### <a name="additional-security-best-practices"></a>Ek güvenlik en iyi uygulamalar

* Trafik bölünmesi: tamamen ayrı bir ağ dağıtmak ve fiziksel yalıtım bir seçenek olduğu VLAN'ları kullanarak, iSCSI SAN bir kurumsal ağ üzerinde kullanıcı trafiğinden yalıtmak. İSCSI depolama için ayrılmış bir ağ güvenliği ve iş açısından kritik verileriniz performansı garanti eder. Kurumsal bir LAN üzerinden depolama ve kullanıcı trafiğinin karıştırma önerilmez gecikme süresini ve ağ hatalarına neden.
* Konak tarafı ağ güvenliği için TCP/IP'yi aktarım motoru (TOE) destekleyen ağ arabirimleri kullanın. TOE ağ bağdaştırıcısında TCP işleyerek CPU yükü azaltır.

## <a name="protect-data-via-storage-accounts"></a>Depolama hesapları ile verileri koruma

Her bir Microsoft Azure aboneliği bir veya daha fazla depolama hesabı oluşturabilirsiniz. (Depolama hesabı benzersiz bir ad alanı Azure bulutunda depolanan verilerle çalışmak için sağlar.) Bir depolama hesabına erişim için bu depolama hesabıyla ilişkilendirilmiş abonelik ve erişim anahtarlarını tarafından denetlenir.

Bir depolama hesabı oluşturduğunuzda, Microsoft Azure StorSimple cihazı depolama hesabına eriştiğinde biri kimlik doğrulaması için kullanılan iki adet 512 bit depolama erişim tuşu oluşturur. Bu anahtarlar yalnızca biri kullanımda olduğunu unutmayın. Başka bir tuşa anahtarlarını düzenli aralıklarla döndürmenizi sağlayan yedekte tutulur. Anahtarlarını döndürmek için ikincil anahtar etkin hale getirin ve ardından birincil anahtarı silin. Sonra sonraki döndürme sırasında kullanım için yeni bir anahtar oluşturabilirsiniz. (Güvenlik nedeniyle, çok sayıda veri merkezleri anahtar döndürme gerektirir.)

Anahtar döndürme için bu en iyi uygulamaları izlemenizi öneririz:

* Düzenli olarak, depolama hesabınıza yetkisiz kullanıcılar tarafından erişmediğinden emin olmak amacıyla depolama hesabı anahtarlarını döndürün.
* Düzenli aralıklarla Azure yöneticiniz değiştirmek veya bu depolama hesabına doğrudan erişmek için Azure portalında depolama bölümünü kullanarak birincil veya ikincil anahtarını yeniden gerekir.

## <a name="protect-data-via-encryption"></a>Şifreleme yoluyla verileri koruma

StorSimple, depolanan verileri korumak için aşağıdaki şifreleme algoritmalarını veya StorSimple çözümünüzün bileşenleri arasında seyahat kullanır.

| Algoritması | Anahtar uzunluğu | Uygulamalar/protokolleri/açıklamaları |
| --- | --- | --- |
| RSA |2048 |RSA PKCS 1 v1.5 Azure portal tarafından cihaza gönderilen yapılandırma verilerini şifrelemek için kullanılır: Örneğin, depolama hesabı kimlik bilgileri, StorSimple cihaz yapılandırması ve bulut depolama şifreleme anahtarları. |
| AES |256 |AES CBC ile Azure portalında StorSimple cihazından gönderilmeden önce ortak kısmını hizmet veri şifreleme anahtarı şifrelemek için kullanılır. Bu ayrıca, StorSimple cihaz tarafından verileri bulut depolama hesabına gönderilmeden önce verileri şifrelemek için kullanılır. |

## <a name="storsimple-cloud-appliance-security"></a>StorSimple Cloud Appliance güvenlik

[!INCLUDE [storsimple Cloud Appliance security](../../includes/storsimple-virtual-device-security.md)]

## <a name="managing-personal-information"></a>Kişisel bilgilerini yönetme

Fiziksel ve sanal serisi için StorSimple cihaz Yöneticisi, aşağıdaki anahtar örneklerinde kişisel bilgilerinizi toplar:

- Uyarı kullanıcı ayarları kullanıcı e-posta adresi nerede yapılandırılır. Bu bilgiler görüntülenebilir ve yönetici tarafından temizlenir. Bu StorSimple 8000 serisi cihazlar ve StorSimple sanal diziler için geçerlidir.
  * StorSimple 8000 serisi için ayarları temizleyin ve görüntülemek için adımları izleyin. [görünümü ve StorSimple Uyarıları yönetme](storsimple-8000-manage-alerts.md#configure-alert-settings)
  * StorSimple Virtual Array için ayarları temizleyin ve görüntülemek için adımları izleyin. [görünümü ve StorSimple Uyarıları yönetme](storsimple-virtual-array-manage-alerts.md#configure-alert-settings)
- Paylaşımlarında bulunan veriler erişebilen kullanıcılar. Paylaşımı verileri erişebilen kullanıcıları içeren bir liste görüntülenir ve görüntülenebilir. Bu liste de silinir paylaşımları silindiğinde. Bu, yalnızca StorSimple sanal dizilerine uygulanır.
  * Kimin erişebileceğini veya bir paylaşımı silmek için adımları izleyin. kullanıcı listesini görüntülemek için [StorSimple sanal dizisi paylaşımları yönetme](storsimple-virtual-array-manage-shares.md)

Daha fazla bilgi için, [Güven Merkezi](https://www.microsoft.com/trustcenter)’nde Microsoft Gizlilik ilkesini gözden geçirin.

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

Bazı sorular ve cevaplar güvenlik ve Microsoft Azure StorSimple hakkında aşağıda verilmiştir.

**S:** Hizmetimi gizliliği. Ne sonraki adımlarım olması gerekiyor mu?

**C:** Hizmet veri şifreleme anahtarı ve katmanlama veri için kullanılan depolama hesabı için depolama hesabı anahtarları hemen değiştirmeniz. Yönergeler için şuraya gidin:

* [Hizmet veri şifreleme anahtarı değiştirme](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)
* [Depolama hesabı anahtar rotasyonu](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**S:** Hizmet kayıt anahtarı için soran yeni bir StorSimple cihazı var. Bunu nasıl aldığını?

**C:** Bu anahtar, StorSimple cihaz Yöneticisi hizmetine ilk oluşturduğunuzda oluşturulur. Cihaza bağlanmak için StorSimple cihaz Yöneticisi hizmeti kullandığınızda, görüntülemek veya hizmet kayıt anahtarını yeniden oluşturmak için hizmet hızlı başlangıç sayfasını kullanabilirsiniz. Yeni bir hizmet kayıt anahtarı oluşturma, var olan kayıtlı cihazları etkilemez. Yönergeler için şuraya gidin:

* [Görüntülemek veya hizmet kayıt anahtarını yeniden oluştur](storsimple-8000-manage-service.md##regenerate-the-service-registration-key)

**S:** Ben my hizmet veri şifreleme anahtarı kayboldu. Ne yapmalıyım?

**C:** Microsoft Desteği'ne başvurun. Bunlar destek oturumu cihaz ve Yardım (en az bir cihaz çevrimiçi olması koşuluyla) anahtarı almak için oturum açabilir. Hizmet veri şifreleme anahtarını elde hemen sonra yeni anahtarı yalnızca sizin bildiğiniz emin olmak için değiştirmeniz gerekir. Yönergeler için şuraya gidin:

* [Hizmet veri şifreleme anahtarı değiştirme](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)

**S:**  Bir cihaz için bir hizmet veri şifreleme anahtarı değiştirme yetkisi, ancak anahtar değiştirme işlemi başlatılamadı. Ne yapmalıyım?

**C:** Zaman aşımı süresi dolmuşsa, hizmet veri şifreleme anahtar değişimi için cihaz yeniden yetkilendirin ve işlemi yeniden başlatmak gerekir.

**S:**  Hizmet veri şifreleme anahtarı değiştirdim ama ben 4 saat içinde diğer cihazları güncelleştirmek mümkün değildi. Yeniden başlatmak zorunda mıyım?

**C:** Yalnızca değişiklik başlatmak için 4 saatlik bir zaman aralığıdır. Yetkili StorSimple cihazında güncelleştirme işlemini başlattıktan sonra tüm cihazlar güncelleştirilene kadar yetkilendirme geçerli değil.

**S:** Bizim StorSimple yönetici şirketten. Ne yapmalıyım?

**C:** Değiştirin ve StorSimple cihazı için erişime izin ver parolaları sıfırlama ve yeni bilgilerin yetkisiz personele bilinmiyor emin olmak için hizmet veri şifreleme anahtarı değiştirme. Yönergeler için şuraya gidin:

* [StorSimple cihaz Yöneticisi hizmetini kullanarak storsimple parolalarını değiştirmek için kullanın](storsimple-8000-change-passwords.md)
* [Hizmet veri şifreleme anahtarı değiştirme](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)
* [StorSimple cihazınız için CHAP yapılandırma](storsimple-8000-configure-chap.md)

**S:** StorSimple Snapshot Manager parolasını StorSimple cihaz için bağlanan bir konak sağlamak istiyorum, ancak parola kullanılabilir değil. Ne yapabilirim?

**C:** Parolanızı unuttuysanız, yeni bir tane oluşturmanız gerekir. Ardından, tüm mevcut Kullanıcılar Parola değiştirildi ve yeni parolayı kullanmak üzere istemcileri güncelleştirmelisiniz bilgilendirmek emin olun. Yönergeler için şuraya gidin:

* [StorSimple Snapshot Manager parolasını değiştirme](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password)
* [Bir cihaz kimlik doğrulaması](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**S:** StorSimple için Windows PowerShell için uzaktan erişim için sertifika cihazda değiştirildi. Uzaktan erişim müşterilerim nasıl güncelleştirebilirim?

**C:** StorSimple cihaz Yöneticisi hizmetinden yeni sertifikayı yükleyin ve ardından, uzaktan erişim istemcilerinin sertifika deposunda yüklü olmasını sağlayın. Yönergeler için şuraya gidin:

* [Sertifikayı içeri aktarma cmdlet'i](https://docs.microsoft.com/powershell/module/pkiclient/import-certificate)

**S:** My veri korumalı StorSimple cihaz Yöneticisi hizmetinin güvenliği aşılmış ise?

**C:** Bir web tarayıcısında görüntülediğinizde hizmet yapılandırma verilerini sizin ortak anahtarınızla her zaman şifrelenir. Hizmet, hizmet özel anahtarına erişime sahip olmadığından, tüm verileri görmek mümkün olmayacaktır. StorSimple cihaz Yöneticisi hizmeti tehlikedeyse etkisi yoktur, olarak StorSimple cihaz Yöneticisi hizmeti depolanan anahtar yok.

**S:** Birisi veri şifreleme sertifikasını erişim kazanırsa, verilerimi zarar görecektir?

**C:** Microsoft Azure Müşteri'nin veri şifreleme anahtarı (.pfx dosyası) şifrelenmiş biçimde depolar. Yalnızca .pfx dosyasına erişimi alma seçeneği .pfx dosyasını şifrelenir ve .pfx dosyasının şifresini çözmek için hizmet veri şifreleme anahtarı StorSimple hizmeti yoktur çünkü gizli dizileri açığa çıkarmamak.

**S:** Bir mercilerce Microsoft'tan benim verilerimi isterse, ne olur?

**C:** Hizmette tüm veriler şifrelenir ve özel anahtarı cihazla tutulur çünkü mercilerce müşteri verilerini istemeleri gerekir.



## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

