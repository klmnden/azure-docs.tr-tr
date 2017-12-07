---
title: "StorSimple 8000 serisi güvenlik | Microsoft Docs"
description: "StorSimple hizmeti, cihaz ve şirket içinde ve bulutta veri koruma güvenlik ve gizlilik özellikleri açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/06/2017
ms.author: alkohli
ms.openlocfilehash: a8990d68b327e5688c7078a6b1a9d41ad0600a67
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="storsimple-security-and-data-protection"></a>StorSimple güvenlik ve veri koruması

## <a name="overview"></a>Genel Bakış

Güvenlik herkes teknolojisi gizli veya özel verilerle özellikle kullanıldığında, yeni bir teknoloji benimsenmesi için önemli bir konudur. Farklı teknolojiler değerlendirirken, daha yüksek riskleri ve veri koruması maliyetlerini dikkate almanız gerekir. Microsoft Azure StorSimple bir güvenlik ve gizlilik çözüm veri korumasına yardımcı olmak için sağlar:

* **Gizlilik** – yalnızca yetkili varlıklar verilerinizi görüntüleyebilirsiniz.
* **Bütünlük** – yalnızca yetkili varlıklar değiştirebilir veya verilerinizi silebilirsiniz.

Microsoft Azure StorSimple çözümünüzle birbiriyle etkileşimde dört ana bileşenden oluşur:

* **Microsoft Azure üzerinde barındırılan StorSimple cihaz Yöneticisi hizmeti** – yapılandırmak ve StorSimple cihazı sağlamak için kullandığınız yönetim hizmeti.
* **StorSimple cihazı** –, veri merkezinizde yüklü fiziksel bir cihaz. Tüm ana bilgisayarlar ve verileri oluşturmak istemciler StorSimple cihaza bağlanın ve cihaz verileri yönetir ve Azure buluta taşır uygun şekilde.
* **İstemcileri/ana bağlı cihaza** – StorSimple cihaza bağlanın ve korunması gereken veriler oluşturabilen istemcilerin altyapınızdaki.
* **Bulut depolama** – verilerinin depolandığı Azure bulut konumu.

Aşağıdaki bölümlerde, her biri bu bileşenlerin ve bunlar üzerinde depolanan verilerin korunmasına yardımcı olma StorSimple güvenlik özellikleri açıklanmaktadır. Ayrıca, Microsoft Azure StorSimple güvenliği ve karşılık gelen yanıtları ilgili olabilecek soruların listesini içerir.

## <a name="storsimple-device-manager-service-protection"></a>StorSimple cihaz Yöneticisi hizmeti koruma

Microsoft Azure üzerinde barındırılan ve kuruluşunuzun temin tüm StorSimple cihazlarını yönetmek için kullanılan bir yönetim hizmeti StorSimple Aygıt Yöneticisi'ni hizmetidir. StorSimple cihaz Yöneticisi hizmeti bir web tarayıcısı aracılığıyla Azure portalında oturum açmak için Kurumsal kimlik bilgilerinizi kullanarak erişebilirsiniz.

StorSimple cihaz Yöneticisi hizmetine erişim kuruluşunuz StorSimple içeren bir Azure aboneliği sahip olmasını gerektirir. Aboneliğiniz Azure portalında erişebilirsiniz özellikleri yönetir. Kuruluşunuzun bir Azure aboneliğine sahip değil ve isterseniz bunları hakkında daha fazla bilgi için bkz: [Azure'a kuruluş olarak kaydolma](../active-directory/sign-up-organization.md).

StorSimple cihaz Yöneticisi hizmeti Azure'da barındırıldığı için Azure güvenlik özellikleri tarafından korunur. Microsoft Azure tarafından sağlanan güvenlik özellikleri hakkında daha fazla bilgi için Git [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>StorSimple cihaz koruma

StorSimple cihazı katı hal sürücüleri (SSD) ve sabit disk sürücülerinin (HDD'ler), yedek denetleyicileri ve otomatik yük devretme yetenekleri ile birlikte içeren bir şirket içi karma depolama aygıttır. Denetleyicileri katmanlama, şu anda kullanılan (veya sık kullanılan) veri yerel depolamada (StorSimple cihazı veya şirket içi sunucuları), buluta daha az sık kullanılan veri taşırken yerleştirme depolama yönetin.

Cihazların Azure aboneliğinizde oluşturduğunuz StorSimple Aygıt Yöneticisi'ni hizmete katılmak için izin verilir StorSimple yalnızca yetkili. Bir aygıt yetkilendirmek için bunu StorSimple cihaz Yöneticisi hizmetiyle hizmet kayıt anahtarını sağlayarak kaydetmeniz gerekir. Hizmet kayıt anahtarını Azure portalında oluşturulan bir 128-bit rastgele anahtardır.

![Hizmet kayıt anahtarı](./media/storsimple-security/ServiceRegistrationKey.png)

Bilgi edinmek için nasıl bir Git hizmet kayıt anahtarını alın [2. adım: Hizmet kayıt anahtarını alın](storsimple-8000-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).

Hizmet kayıt anahtarını 100 + karakterler içeren uzun bir anahtardır. Anahtarı kopyalayın ve gerektiği gibi ek cihazlar yetkilendirmek için kullanabilmesi için güvenli bir konumda metin dosyasına kaydedin. İlk aygıtınızı kaydettikten sonra hizmet kayıt anahtarını kaybolursa, yeni bir anahtar StorSimple Aygıt Yöneticisi'ni hizmetinden oluşturabilir. Bu, var olan cihazların işlemini etkilemez.

Bir cihaz kaydedildikten sonra Microsoft Azure ile iletişim kurmak için belirteçlerini kullanır. Hizmet kayıt anahtarını cihaz kaydından sonra kullanılmaz.

> [!NOTE]
> Her kullandıktan sonra hizmet kayıt anahtarını yeniden öneririz.


## <a name="protect-your-storsimple-solution-via-passwords"></a>StorSimple çözümünüzün parolaları aracılığıyla koruma

Parolalar bilgisayar güvenlik önemli bir özelliği olan ve StorSimple çözümde verilerinizi yalnızca yetkili kullanıcılar için erişilebilir olduğundan emin olun yardımcı olmak için yaygın olarak kullanılır. StorSimple aşağıdaki parolalar yapılandırmanıza olanak sağlar:

* StorSimple cihaz Yöneticisi parolası
* Kimlik Doğrulama Protokolü (CHAP) başlatıcı ve hedef parolaları sınama
* StorSimple Snapshot Manager parolası

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>StorSimple ve StorSimple cihaz Yöneticisi parolası için Windows PowerShell

StorSimple için Windows PowerShell StorSimple cihazı yönetmek için kullanabileceğiniz bir komut satırı arabirimidir. StorSimple için Windows PowerShell, Cihazınızı kaydetmek, ağ arabiriminin aygıtınızda yapılandırın, belirli türden güncelleştirmeler yüklemeniz, destek oturum erişerek cihazınızın sorunlarını giderirken olanak sağlar ve cihaz durumunu değiştir özelliğe sahiptir. Cihazdaki seri konsoluna bağlanmak veya Windows PowerShell uzaktan iletişimini kullanarak StorSimple için Windows PowerShell erişebilir.

PowerShell uzaktan iletişimini HTTPS veya HTTP üzerinden yapılabilir. HTTPS üzerinden uzaktan yönetimi etkinleştirilirse, uzak yönetim sertifikası aygıttan karşıdan yükleyip uzak istemciye gerekecektir. PowerShell uzaktan iletişimi hakkında daha fazla bilgi için Git [StorSimple Cihazınızı uzaktan bağlanma](storsimple-8000-remote-connect.md).

Aygıta bağlanmayı StorSimple için Windows PowerShell kullandıktan sonra cihaza oturum açmak için cihaz Yöneticisi parolasını sağlamanız gerekir.

![Cihaz yöneticisi parolası](./media/storsimple-security/DeviceAdminPW.png)

Aşağıdaki en iyi uygulamaları göz önünde bulundurun:

* Uzaktan Yönetim varsayılan olarak kapalıdır. StorSimple cihaz Yöneticisi hizmeti etkinleştirmek için kullanabilirsiniz. En iyi güvenlik uygulaması olarak uzaktan erişim, yalnızca gerçekten gerekli zaman boyunca etkin olması.
* Parolayı değiştirirseniz, böylece beklenmeyen bağlantı kaybı yaşanmaz tüm uzaktan erişim kullanıcıları bilgilendirmek üzere emin olun.
* StorSimple cihaz Yöneticisi hizmeti var olan parolaların alınamıyor: Bu yalnızca bunları sıfırlayabilirsiniz. Bunu unutulursa parola sıfırlama gerekmez, tüm parolaları güvenli bir yerde depolamanız önerilir. Parola sıfırlama gerekiyorsa, parolanızı sıfırlamanız önce tüm kullanıcıları bilgilendirmek üzere emin olun.

Cihaz seri bağlantı kullanarak Windows PowerShell arabirimi erişebilirsiniz. Ayrıca, uzaktan HTTP veya ek güvenlik sağlayan HTTPS kullanarak erişebilirsiniz. HTTPS seri ya da HTTP bağlantısı değerinden daha yüksek düzeyde güvenlik sağlar. Ancak, HTTPS kullanmak üzere, ilk sertifika cihaz erişecek istemci bilgisayara yüklemeniz gerekir. Uzaktan erişim sertifikasının StorSimple cihaz Yöneticisi Hizmeti aygıt yapılandırma sayfasından indirebilirsiniz. Uzaktan erişim için sertifika kaybolursa, yeni bir sertifika indirin ve Uzaktan Yönetimi'ni kullanma yetkisine sahip tüm istemcilerine yayar.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Kimlik Doğrulama Protokolü (CHAP) başlatıcı ve hedef parolaları sınama

CHAP, StorSimple cihaz tarafından uzak istemcilerin kimliğini doğrulamak için kullanılan bir kimlik doğrulama düzenidir. Doğrulama bir paylaşılan parolasını temel alır. CHAP tek yönlü olabilir (tek yönlü) veya karşılıklı (çift yönlü). Tek yönlü CHAP ile hedef (StorSimple cihaz) Başlatıcı (ana bilgisayar) kimliğini doğrular. Karşılıklı veya ters CHAP hedef Başlatıcı kimlik doğrulaması ve Başlatıcı Hedef sonra kimlik doğrulaması gerektirir. StorSimple ya da yöntemi kullanmak üzere yapılandırılabilir.

CHAP yapılandırdığınızda, aşağıdakilere dikkat edin:

* CHAP kullanıcı adı 233'den az karakter içermelidir.
* CHAP parolası 12 ile 16 karakter arasında olmalıdır. Çalışırken daha uzun bir kullanıcı adı kullanın veya parola Windows ana bilgisayarda kimlik doğrulama hatası neden olur.
* CHAP başlatıcısını ve CHAP hedefi için aynı parolayı kullanamazsınız.
* Parola ayarladıktan sonra onu değiştirilebilir ancak geri alınamaz. Parola değiştirildiyse, StorSimple cihazı başarıyla bağlanabilmesi için tüm uzaktan erişim kullanıcıları bilgilendirmek üzere emin olun.

CHAP ve StorSimple çözümünüz için yapılandırma hakkında daha fazla bilgi için Git [StorSimple cihazınız için CHAP yapılandırma](storsimple-8000-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager parolası

StorSimple Snapshot Manager uygulamayla tutarlı yedeklemeler oluşturmak için birim grupları ve Windows birim gölge kopyası hizmeti kullanan bir Microsoft Yönetim Konsolu (MMC) ek bileşenidir. Ayrıca, StorSimple Snapshot Manager yedekleme zamanlamaları ve kopya oluşturmak veya birimleri geri yüklemek için kullanabilirsiniz.

StorSimple Snapshot Manager kullanmak için bir aygıt yapılandırdığınızda, StorSimple Snapshot Manager parolasını girmeniz gerekir. Bu parola ilk Windows PowerShell'de StorSimple için kayıt sırasında ayarlanır. Parola ayrıca ayarlayın ve StorSimple Aygıt Yöneticisi'ni hizmetinden değiştirildi. Bu parolayı, cihazı ile StorSimple Snapshot Manager kimliğini doğrular.

![StorSimple Snapshot Manager parolası](./media/storsimple-security/SnapshotMgrPassword.png)

StorSimple Snapshot Manager parolası 14-15 karakter olmalıdır ve 3 veya daha büyük harf, küçük harfler, sayısal ve özel karakter bir bileşimi içermesi gerekir. StorSimple Snapshot Manager parolası ayarladıktan sonra onu değiştirilebilir ancak geri alınamaz. Parolayı değiştirirseniz, tüm uzak kullanıcılara bildirmek emin olun.

StorSimple anlık görüntü Yöneticisi hakkında daha fazla bilgi için Git [StorSimple anlık görüntü Yöneticisi nedir?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Parola en iyi uygulamalar

StorSimple parolalarını güçlü ve iyi korumalı olmasını sağlamaya yardımcı olmak için aşağıdaki kılavuzları kullanın öneririz:

* Parolalarınızı üç ayda değiştirin. Parolaları değiştirme yıllık zorlanır.
* Güçlü parolalar kullanın. Daha fazla bilgi için Git [güçlü parolalar oluşturma ve bunları korumak](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
* Her zaman için farklı erişim düzenekler farklı parolalar kullanın; Her belirttiğiniz parolaların benzersiz olmalıdır.
* Parolalar StorSimple cihazı erişmek için yetkili değil kimseyle paylaşmayın.
* Bir parola başkalarının önünde hakkında konuşurken değil veya parola biçime bakma ipucu.
* Bir hesap veya parola aşılmış şüpheleniyorsanız, olay bilgi güvenlik departmanınız bildirin.
* Tüm parolaları hassas, gizli bilgi kabul eder. 

## <a name="storsimple-data-protection"></a>StorSimple veri koruması

Bu bölümde Aktarımdaki verileri ve depolanan verilerin korunmasına StorSimple güvenlik özellikleri açıklanmaktadır.

Diğer bölümlerinde açıklandığı gibi parolalar yetki verme ve StorSimple çözümünüzün erişim kazanmadan önce kullanıcıların kimliğini doğrulamak için kullanılır. Depolama sistemleri arasında ve onu depolandığı sürece aktarıldığı sırada başka bir güvenlik önlemleri yetkisiz kullanıcılardan veri koruyor. Aşağıdaki bölümlerde, StorSimple ile sağlanan veri koruma özellikleri açıklanmaktadır.

> [!NOTE]
> Yinelenenleri kaldırma StorSimple cihazında ve Microsoft Azure depolama alanında depolanan veriler için ek koruma sağlar. Veri yinelenenler kaldırılmadığında veri nesneleri ayrı olarak eşlemek ve bunlara erişmek için kullanılan meta verileri depolanır: birim yapısına, dosya sistemi veya dosya adı göre verileri yeniden oluşturmak için kullanılabilir depolama düzeyi bağlamı yok.


## <a name="protect-data-flowing-through-the-service"></a>Hizmet aracılığıyla akan verileri koruma

StorSimple cihaz Yöneticisi hizmeti birincil amacı, yönetme ve StorSimple cihazı yapılandırma olmaktır. StorSimple cihaz Yöneticisi hizmeti Microsoft Azure üzerinde çalışır. Aygıt yapılandırma verilerini girmek için Azure portal'ı kullanın ve ardından Microsoft Azure aygıta veri göndermek için StorSimple cihaz Yöneticisi hizmeti kullanır. StorSimple sistemi asimetrik anahtar çifti Azure hizmeti güvenliğinin depolanan bilgilerin güvenliğinin aşılmasına oluşturmayacaktır sağlamaya yardımcı olmak için kullanır.

![Yürütülen veri şifrelemesi](./media/storsimple-security/DataEncryption.png)

Asimetrik anahtar sistem hizmeti aracılığıyla şu şekilde akar verilerin korunmasına yardımcı olur:

1. Veri şifreleme sertifikasını, çifti cihazda oluşturulması ve verileri korumak için kullanılan bir asimetrik ortak ve özel anahtarı kullanır. İlk cihaz kaydedildiğinde anahtarları oluşturulur.
2. Veri şifreleme sertifikası anahtarları tarafından ilk cihaz kaydı sırasında rastgele oluşturulan bir güçlü 128-bit anahtar hizmet verileri şifreleme anahtarı tarafından korunan bir kişisel bilgi değişimi (.pfx) dosyasına dışarı aktarılır.
3. Sertifikanın ortak anahtarı güvenli bir şekilde StorSimple cihaz Yöneticisi hizmeti için kullanılabilir hale getirilir ve özel anahtar aygıtla birlikte kalır.
4. Veri hizmeti girişi Azure hizmeti cihaza akan verilerin şifresini çözemez sağlama ortak anahtar ve cihazda depolanan özel anahtar ile şifresi kullanılarak şifrelenir.

Hizmet verileri şifreleme anahtarı yalnızca hizmete kayıtlı ilk cihaz üzerinde oluşturulur. Hizmetine kayıtlı tüm sonraki aygıtları aynı hizmet verileri şifreleme anahtarı kullanmanız gerekir.

> [!IMPORTANT]
> Hizmet verileri şifreleme anahtarı bir kopyasını alın ve güvenli bir konuma kaydetmek çok önemlidir. Hizmet verileri şifreleme anahtarı bir kopyasını bir yetkili kişi tarafından erişilebilir ve cihaz Yöneticisi kolayca bildirilmesi şekilde depolanması gerekir.
> 
> Hizmet verileri şifreleme anahtarı kaybolursa, Microsoft Destek ekibiyle en az bir aygıt çevrimiçi bir durumda olması koşuluyla almak için yardımcı olabilir. Bunu alındıktan sonra hizmet verileri şifreleme anahtarı değiştirmenizi öneririz. Yönergeler için Git [hizmet verileri şifreleme anahtarı değiştirmek](storsimple-service-dashboard.md#change-the-service-data-encryption-key).

Hizmet verileri şifreleme anahtarı ve karşılık gelen veri şifreleme sertifikasını değiştirmek için adımları [StorSimple cihaz Yöneticisi hizmeti için hizmet verileri şifreleme anahtarı değiştirmek](storsimple-8000-manage-service.md#change-the-service-data-encryption-key). Şifreleme anahtarları değiştirilmesi tüm cihazlar yeni anahtarla güncelleştirilmesini gerektirir. Bu nedenle, tüm cihazlar çevrimiçi olduğunda bu anahtar değiştirmenizi öneririz. Cihaz çevrimdışı ise, kendi anahtarları farklı bir zamanda değiştirilebilir. Güncel olmayan anahtarları aygıtlarla hala yedeklemeleri çalıştırma mümkün olacaktır, ancak bunlar anahtar güncelleştirilene kadar verileri geri yüklemek mümkün olmaz.

Hizmet verileri şifreleme anahtarı ve veri şifreleme sertifikasını süresi dolmaz. Ancak, yıllık anahtar güvenliğinin aşılması önlemeye yardımcı olmak için hizmet verileri şifreleme anahtarı değiştirmenizi öneririz.

## <a name="protect-data-at-rest"></a>Rest verileri koruma

StorSimple cihazı, katmanlarda yerel olarak ve bulutta kullanım sıklığı bağlı olarak depolayarak veri yönetir. Cihaza bağlı tüm ana makineler uygun şekilde buluta verileri taşır cihaz verileri gönderin. Verileri CİHAZDAN buluta güvenli bir şekilde Internet üzerinden aktarılır. Her bir cihaz tüm paylaşılan birimler aygıtta ortaya çıkarır bir iSCSI hedefi vardır. Tüm verileri depolama buluta gönderilmeden önce şifrelenir. 

![Bulut depolama şifreleme anahtarı](./media/storsimple-security/CloudStorageEncryption.png)

Güvenlik ve buluta taşınmış veri bütünlüğünü sağlamaya yardımcı olmak için StorSimple, bulut depolama şifreleme anahtarları şu şekilde tanımlamanıza olanak sağlar:

* Birim kapsayıcısı oluşturduğunuzda, bulut depolama şifreleme anahtarı belirtin. Anahtarı değiştirilemiyor veya daha sonra eklenebilir.
* Bir birim kapsayıcısındaki tüm birimlerin aynı şifreleme anahtarını paylaşır. Farklı bir form belirli bir birimin şifreleme istiyorsanız, o birimi barındırmak için yeni bir birim kapsayıcısı oluşturmanızı öneririz.
* Bulut depolama şifreleme anahtarı StorSimple Aygıt Yöneticisi'ni hizmetinde girdiğinizde, anahtar hizmet verileri şifreleme anahtarı ortak kısmını kullanarak ve aygıta gönderilen şifrelenir.
* Bulut depolama şifreleme anahtarı herhangi bir yere hizmetinde depolanmaz ve yalnızca cihaz denir.
* Bir bulut depolama şifreleme anahtarı belirtmek isteğe bağlıdır. Ana aygıta şifrelenmiş veriler gönderebilir.

### <a name="additional-security-best-practices"></a>Ek güvenlik en iyi uygulamalar

* Trafik bölünmesi: Kurumsal bir ağda kullanıcı trafiğinden, iSCSI SAN tamamen ayrı bir ağ dağıtımı ve burada fiziksel yalıtım bir seçenek değildir VLAN'ları kullanarak yalıtmak. İSCSI depolama için ayrılmış bir ağ güvenliği ve iş açısından kritik verilerinizi performansını garanti. Kurumsal bir LAN üzerinden depolama ve kullanıcı trafiği karıştırma önerilmez ve gecikme süresini artırmak ve ağ hatalarına neden.
* Konak tarafındaki ağ güvenliği için TCP/IP'yi aktarım motoru (TOE) destekleyen ağ arabirimleri kullanın. TOE ağ bağdaştırıcısında TCP işleyerek CPU yükünü azaltır.

## <a name="protect-data-via-storage-accounts"></a>Depolama hesapları üzerinden veri koruma

Her bir Microsoft Azure aboneliği bir veya daha fazla depolama hesabı oluşturabilirsiniz. (Bir depolama hesabı benzersiz bir ad alanı Azure bulutta depolanan verilerle çalışmak için sağlar.) Bir depolama hesabına erişim izni, bu depolama hesabıyla ilişkilendirilmiş abonelik ve erişim anahtarları tarafından denetlenir.

Bir depolama hesabı oluşturduğunuzda, Microsoft Azure StorSimple cihazı depolama hesabı eriştiğinde biri kimlik doğrulaması için kullanılan iki 512 bit depolama erişim tuşu oluşturur. Bu anahtarlar yalnızca biri kullanımda olduğunu unutmayın. Başka bir anahtar anahtarlar düzenli aralıklarla döndürmek sağlayarak yedekte tutulur. Anahtarlarını döndürmek için ikincil anahtar etkin hale getirmek ve birincil anahtarı silin. Daha sonra sonraki döndürme sırasında kullanmak için yeni bir anahtar oluşturabilirsiniz. (Güvenlik nedeniyle, birçok veri merkezleri anahtar döndürme gerektirir.)

Bu anahtar döndürme için en iyi uygulamaları izlemeniz önerilir:

* Düzenli olarak depolama hesabınıza yetkisiz kullanıcılar tarafından erişilmez sağlamaya yardımcı olmak için depolama hesabı anahtarlarını döndürün.
* Düzenli aralıklarla Azure yöneticinize değiştirmek veya bu depolama hesabı doğrudan erişmek için Azure portal depolama bölümünü kullanarak birincil veya ikincil anahtarı yeniden gerekir.

## <a name="protect-data-via-encryption"></a>Veri şifreleme aracılığıyla koruma

StorSimple depolanan verileri korumak için aşağıdaki şifreleme algoritmalarını veya StorSimple çözümünüzün bileşenler arasında seyahat kullanır.

| Algoritması | Anahtar uzunluğu | Uygulamalar/protokolleri/açıklamaları |
| --- | --- | --- |
| RSA |2048 |RSA PKCS 1 v1.5 Azure portal tarafından cihaza gönderilen yapılandırma verilerini şifrelemek için kullanılan: Örneğin, depolama hesabının kimlik bilgilerini, StorSimple cihazı yapılandırma ve bulut depolama şifreleme anahtarları. |
| AES |256 |AES CBC ile Azure portalında StorSimple cihazı gönderilmeden önce ortak kısmını hizmet verileri şifreleme anahtarı şifrelemek için kullanılır. Bulut depolama hesabı için veri gönderilmeden önce verileri şifrelemek için bunu StorSimple cihaz tarafından da kullanılır. |

## <a name="storsimple-cloud-appliance-security"></a>StorSimple bulut uygulaması güvenliği

[!INCLUDE [storsimple Cloud Appliance security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

Bazı hakkında sorular ve yanıtlar güvenlik ve Microsoft Azure StorSimple verilmiştir.

**S:** Hizmetim tehlikeye. Ne sonraki adımlarım olması gerekiyor mu?

**Y:** hizmet verileri şifreleme anahtarı ve katmanlama veriler için kullanılan depolama hesabı için depolama hesabı anahtarlarını hemen değiştirmelisiniz. Yönergeler için aşağıdaki adrese gidin:

* [Değişiklik hizmeti veri şifreleme anahtarı](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)
* [Depolama hesaplarının anahtar döndürme](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**S:** için hizmet kayıt anahtarını soran yeni bir StorSimple cihazı vardır. Bunu nasıl aldığını?

**Y:** StorSimple cihaz Yöneticisi hizmeti ilk oluşturduğunuzda bu anahtarı oluşturuldu. Aygıta bağlanmayı StorSimple cihaz Yöneticisi hizmeti kullandığınızda, hizmet hızlı başlangıç sayfasını görüntülemek veya hizmet kayıt anahtarını yeniden oluşturmak için kullanabilirsiniz. Yeni bir hizmet kayıt anahtarı oluşturma, var olan kayıtlı cihazları etkilemez. Yönergeler için aşağıdaki adrese gidin:

* [Hizmet kayıt anahtarını yeniden oluşturmak veya görüntülemek](storsimple-8000-manage-service.md##regenerate-the-service-registration-key)

**S:** my hizmet verileri şifreleme anahtarı kaybolur. Ne yapmalıyım?

**Y:** Microsoft Destek'e başvurun. Bunlar, cihaz ve (en az bir aygıt çevrimiçi olması koşuluyla) anahtarı alıp Yardım desteği oturum oturum açabilirsiniz. Hizmet verileri şifreleme anahtarı edinmek hemen sonra yeni anahtarı yalnızca size bilinir emin olmak için değiştirmeniz gerekir. Yönergeler için aşağıdaki adrese gidin:

* [Değişiklik hizmeti veri şifreleme anahtarı](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)

**S:** ı bir hizmet verileri şifreleme anahtarı değişikliği için bir cihaz yetkili ancak anahtar değiştirme işlemi başlatılmadı. Ne yapmalıyım?

**Y:** zaman aşımı süresi sona erdi, hizmet verileri şifreleme anahtarı değiştirmek için cihaz yeniden yetkilendirin ve işlemini yeniden başlatmak gerekir.

**S:** hizmet verileri şifreleme anahtarı değiştirildi, ancak ı 4 saat içinde diğer cihazları güncelleştirmek mümkün değildi. Yeniden başlatma gerekiyor mu?

**Y:** değişikliği yalnızca başlatmaktan 4 saatlik süre olan. Yetkili StorSimple cihazında güncelleştirme işlemini başlattıktan sonra yetkilendirme tüm cihazlar güncelleştirilinceye kadar geçerlidir.

**S:** Mız StorSimple Yöneticisi şirket sol. Ne yapmalıyım?

**Y:** değiştirme ve sıfırlama StorSimple cihazı erişmesine izin vermek ve hizmet verileri şifreleme değiştirmek parolalar anahtar yeni bilgiler için bilinmiyor emin olmak için yetkisiz personel. Yönergeler için aşağıdaki adrese gidin:

* [Storsimple parolalarını değiştirmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-change-passwords.md)
* [Değişiklik hizmeti veri şifreleme anahtarı](storsimple-8000-manage-service.md#change-the-service-data-encryption-key)
* [StorSimple cihazınız için CHAP yapılandırma](storsimple-8000-configure-chap.md)

**S:** StorSimple Snapshot Manager parolasını StorSimple cihazı bağlanan bir konak sağlamak istiyorum, ancak parola kullanılabilir değil. Ne yapabilirim?

**Y:** parolanızı unuttuysanız, yeni bir tane oluşturmanız gerekir. Ardından, tüm mevcut Kullanıcılar Parola değiştirildi ve yeni parolayı kullanmak için istemcileri güncelleştirmeniz gerekir bilgilendirmek emin olun. Yönergeler için aşağıdaki adrese gidin:

* [StorSimple Snapshot Manager parolasını değiştirme](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password)
* [Bir cihaz kimlik doğrulaması](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**S:** StorSimple için Windows PowerShell için uzaktan erişim için sertifika cihazda değiştirildi. My uzaktan erişim istemcilerinin nasıl güncelleştiririm?

**Y:** StorSimple Aygıt Yöneticisi'ni hizmetinden yeni sertifikayı yükleyin ve ardından bunu, uzaktan erişim istemcilerinin sertifika deposunda yüklü olmasını sağlayın. Yönergeler için aşağıdaki adrese gidin:

* [Sertifika İçeri Aktar cmdlet'i](https://technet.microsoft.com/library/hh848630.aspx)

**S:** StorSimple cihaz Yöneticisi hizmeti aşılıp aşılmadığını korumalı my veri?

**Y:** bir web tarayıcısında görüntülediğinizde hizmet yapılandırma verilerini ortak anahtarınızla her zaman şifrelenir. Hizmet özel anahtarına erişime sahip olmadığından, hizmet herhangi bir veri görmeye olmaz. StorSimple cihaz Yöneticisi hizmeti aşılıp aşılmadığını üzerinde etkisi yoktur, StorSimple Aygıt Yöneticisi'ni hizmetinde depolanan anahtar olarak.

**S:** birisi veri şifreleme sertifikasını erişim sağlarsa verilerimi tehlikeye?

**Y:** Microsoft Azure Müşteri'nin veri şifreleme anahtarı (.pfx dosyası) şifrelenmiş biçimde depolar. Yalnızca .pfx dosyasına erişimi alma seçeneği .pfx dosyasını şifrelenir ve StorSimple hizmeti .pfx dosyasının şifresini çözmek için hizmet verileri şifreleme anahtarı yok olduğundan, tüm gizli kullanıma değil.

**S:** kamu varlık verilerim için Microsoft isterse ne olur?

**Y:** çünkü tüm verileri hizmette şifrelenir ve özel anahtarı aygıtla tutulur kamu varlık müşteri verilerini kaldırmasını isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

