---
title: Azure Güvenlik Merkezi'nde türe göre güvenlik uyarıları | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi’nde bulunan farklı güvenlik uyarısı türleri ele alınmaktadır.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: b3e7b4bc-5ee0-4280-ad78-f49998675af1
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: rkarlin
ms.openlocfilehash: 9ed52af3dd5f8da6e81c53bcf8f26e76bcceee47
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847788"
---
# <a name="understanding-security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama
Bu makale Azure Güvenlik Merkezi'nde bulunan farklı güvenlik uyarısı türlerini ve ilgili öngörüleri anlamanıza yardımcı olur. Uyarıların ve olayların nasıl yönetileceği hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md).

Gelişmiş algılamaları ayarlamak için Azure Güvenlik Merkezi Standart sürümüne yükseltme yapın. Ücretsiz bir deneme sürümü mevcuttur. Yükseltmek için [güvenlik ilkesinde](security-center-azure-policy.md) **Fiyatlandırma Katmanı**’nı seçin. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

> [!NOTE]
> Güvenlik Merkezi Linux makinelerdeki kötü amaçlı davranışlarını algılamak için denetim kayıtlarını kullanan yeni bir algılama kümesi ve ortak denetim çerçevesi içeren sınırlı bir önizleme sürümüyle yayımlanmıştır. Önizlemeye katılmak için abonelik kimliklerinizi [bize](mailto:ASC_linuxdetections@microsoft.com) e-posta ile gönderin.

## <a name="what-type-of-alerts-are-available"></a>Hangi tür uyarılar mevcuttur?
Azure Güvenlik Merkezi, ortamlarını hedefleyen potansiyel saldırılar konusunda müşterileri uyarmak için çeşitli [algılama özelliklerini](security-center-detection-capabilities.md) kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Uyarılarda bulunan bilgiler, tehdidi algılamak için kullanılan analiz türüne göre çeşitlilik gösterir. Tehdit inceleme sırasında yararlı olabilecek ek bağlamsal bilgiler olaylarda da bulunabilir.  Bu makalede aşağıdaki uyarı türleri hakkında bilgi sağlanmıştır:

* Sanal Makine Davranış Analizi (VMBA)
* Ağ Analizi
* SQL Veritabanı ve SQL Veri Ambarı Analizi
* Bağlamsal Bilgiler

## <a name="virtual-machine-behavioral-analysis"></a>Sanal makine davranış analizi
Azure Güvenlik Merkezi; sanal makine günlüklerinin analizine göre tehlike giren kaynakları belirlemek amacıyla davranış analizini kullanabilir. Örneğin, İşlem Oluşturma Olayları ve Oturum Açma Olayları. Ayrıca, yaygın bir kampanyanın kanıtını desteklemek üzere denetlenmesi gereken diğer sinyallerle bir bağlantı vardır.

> [!NOTE]
> Güvenlik Merkezi algılama özelliklerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi algılama özellikleri](security-center-detection-capabilities.md).


### <a name="event-analysis"></a>Olay analizi
Güvenlik Merkezi; sanal makine günlüklerinin analizine göre tehlikeye giren kaynakları belirlemek amacıyla gelişmiş analiz kullanır. Örneğin, İşlem Oluşturma Olayları ve Oturum Açma Olayları. Ayrıca, yaygın bir kampanyanın kanıtını desteklemek üzere denetlenmesi gereken diğer sinyallerle bir bağlantı vardır.

* **Şüpheli işlem yürütme algılandı**: Saldırganlar genellikle zararsız işlem kılığına girerek algılanmadan kötü amaçlı kod yürütmeye çalışır. Bu uyarılar bir işlem yürütmenin aşağıdaki desenlerden biriyle eşleştiğini gösterir:
    * Kötü amaçlar için kullanılacağı bilinen bir işlemin yürütülmesi. Tek tek komutlarla zararsız görünebilse de, uyarı tabanlı birleştirilmesinden oluşan bu komutları puanlanır.
    * Bir işlemin genel olmayan bir konumdan yürütülmesi.
    * Bir işlemin, bilinen şüpheli dosyalarla ortak bir konumdan yürütülmesi.
    * Bir işlemin şüpheli bir yoldan yürütülmesi.
    * Bir işlemin anormal bir bağlamda yürütülmesi.
    * Bir işlemin olağan dışı bir hesap tarafından yürütülmesi
    * Şüpheli uzantıya sahip bir işlemin yürütülmesi.
    * Şüpheli çift uzantıya sahip bir işlemin yürütülmesi.
    * Dosya adında şüpheli bir soldan sağa (RLO) karakteri olan bir işlemin yürütülmesi.
    * Benzer ancak farklı bir yaygın olarak çalıştırılan işlem adı olan bir işlemin yürütülmesi
    * Adı bilinen bir saldırgan aracına karşılık gelen bir işlemin yürütülmesi.
    * Rastgele ada sahip bir işlemin yürütülmesi.
    * Şüpheli uzantıya sahip bir işlemin yürütülmesi.
    * Gizli bir dosyanın yürütülmesi.
    * Bir işlemin, başka bir ilgisiz işlemin alt öğesi olarak yürütülmesi.
    * Bir sistem işlemi tarafından olağan dışı bir işlemin oluşturulması.
    * Windows güncelleştirme hizmeti tarafından anormal bir işlemin başlatılması.
    * Bir işlemin olağan dışı bir komut satırı ile yürütülmesi. Bu durum, kötü amaçlı içeriklerin yürütülmesi için yasal işlemlerin çalınmasıyla ilişkilendirilir.
    * Komut satırından bir dizindeki tüm yürütülebilir dosyaları (*.exe) başlatma girişiminin yürütülmesi.
    * İşlemleri uzaktan çalıştırmak için kullanılabilen PsExec yardımcı programı tarafından bir işlemin yürütülmesi.
    * Kötü amaçlı komutlar barındırabilen veya başlatabilen şüpheli alt işlemleri başlatmak için Apache Tomcat® yürütülebilir dosyasının (Tomcat#.exe) kullanılması.
    * Kötü amaçlı olabilecek yürütülebilir kodu başlatmak için Microsoft Windows "Program Uyumluluğu Yardımcısı"nın (pcalua.exe) kullanılması.
    * Şüpheli bir işlem sonlandırma hamlesinin algılanması.
    * SVCHOST sistem işleminin anormal bir bağlamda yürütülmesi.
    * SVCHOST sistem işleminin nadir bir hizmet grubunda yürütülmesi.
    * Şüpheli bir komut satırının yürütülmesi.
    * Bir PowerShell betiğinin bilinen şüpheli betiklerle ortak özelliklere sahip olması.
    * Bilinen kötü amaçlı PowerShell Powersploit cmdlet’inin yürütülmesi.
    * Yerleşik bir SQL kullanıcısının normalde yürütmeyeceği bir işlem yürütmesi
    * Bir saldırganın, bir dizi komut aracılığıyla hareket halinde bir yürütülebilir dosya oluşturarak algılamayı geçersiz kılmaya çalıştığını belirten base-64 ile kodlanmış bir yürütülebilir dosya algılanması.

* **Şüpheli RDP kaynak etkinliği**: Saldırganlar genellikle deneme yanılma saldırıları ile RDP gibi açık yönetim bağlantı noktalarını hedefler. Bu uyarılar aşağıdaki durumları ifade eden şüpheli Uzak Masaüstü oturum açma etkinliğini belirtir:
    * Uzak Masaüstü oturumu açma girişiminde bulunulması.
    * Geçersiz hesaplar kullanılarak Uzak Masaüstü oturumu açma girişiminde bulunulması.
    * Uzak Masaüstü oturumu açma girişiminde bulunulması, bazıları makinede başarıyla oturum açamaz.
* **Şüpheli SSH kaynak etkinliği**: Saldırganlar genellikle deneme yanılma saldırıları ile SSH gibi açık yönetim bağlantı noktalarını hedefler. Bu uyarılar aşağıdaki durumları ifade eden şüpheli SSH oturum açma etkinliğini belirtir:
    * Başarısız SSH oturumu açma girişiminde bulunulması.
    * Bazıları başarılı olan SSH oturumu açma girişiminde bulunulması.
* **Şüpheli WindowPosition kayıt defteri değeri**: Bu uyarı, masaüstünün görünür olmayan bölümlerinde uygulama pencerelerinin gizlendiğini ifade edebilecek bir WindowPosition kayıt defteri yapılandırma değişikliğinin denendiğini belirtir.
* **AppLocker’ı atlamak için olası girişim**: AppLocker, Windows üzerinde çalışabilen işlemleri sınırlandırmak ve kötü amaçlı yazılımların etkilerini sınırlamak için kullanılabilir. Bu uyarı, güvenilmeyen kod yürütmek için güvenilir yürütülebilir dosyalar (AppLocker İlkesi tarafından izin verilen) kullanarak AppLocker kısıtlamalarını atlamaya yönelik olası bir girişimi belirtir.
* **Şüpheli adlandırılmış kanal iletişimleri**: Bu uyarı bir Windows konsol komutundan yerel bir adlandırılmış kanala verilerin yazıldığını belirtir. Adlandırılmış kanalların, kötü amaçlı bir yerleştirme ile saldırganlar tarafından görev oluşturmak ve iletişim kurmak için kullanıldığı bilinir.
* **Yerleşik certutil.exe aracı ile yürütülebilir dosyanın kodunu çözme**: Bu uyarı, bir yürütülebilir dosyanın kodunu çözmek için certutil.exe adlı yerleşik yönetici yardımcı programının kullanıldığını gösterir. Saldırganların kötü amaçlı eylemler kullanmak gerçekleştirmek için yasal yönetici araçlarının işlevlerini kötüye kullandığı bilinir; örneğin daha sonra yürütülecek kötü amaçlı bir yürütülebilir dosyanın kodunu çözmek için certutil.exe gibi bir araç kullanarak.
* **Bir olay günlüğü temizlendi**: Bu uyarı genellikle saldırganlar tarafından izlerini örtmek için kullanılan şüpheli olay günlüğü temizleme işlemini belirtir.
* **IIS günlük dosyalarını devre dışı bırakma ve silme**: Bu uyarı IIS günlük dosyasının devre dışı bırakıldığını ve/veya silindiğini belirtir; bu işlem genellikle saldırganlar tarafından izlerini örtmek için kullanılır.
* **Şüpheli dosya silme**: Bu uyarı bir saldırgan tarafından kötü amaçlı ikili dosyaların kanıtını ortadan kaldırmak için kullanılabilecek şüpheli dosya silme işlemini belirtir.
* **Tüm dosya gölge kopyaları silindi**: Bu uyarı gölge kopyaların silindiğini belirtir.
* **Şüpheli dosya temizleme komutları**: Bu uyarı, tehlike sonrası otomatik temizleme etkinliği gerçekleştirmek için kullanılan sistem bilgisi komutlarının bir birleşimini belirtir.  *systeminfo.exe* yasal bir Windows aracı olsa da, burada gerçekleşen şekilde bir silme komutundan sonra art arda iki kez çalıştırılması nadiren görülür.
* **Şüpheli hesap oluşturma**: Bu uyarı, var olan yerleşik bir yönetici ayrıcalığı hesabına yakından benzeyen bir hesabın oluşturulduğunu belirtir. Bu teknik, algılanmayacak bir dolandırıcı hesabı oluşturmak üzere saldırganlar tarafından kullanılabilir.
* **Şüpheli birim gölge kopya etkinliği**: Bu uyarı, kaynakta gölge kopya silme etkinliği olduğunu belirtir. Birim Gölge Kopyası (VSC), veri anlık görüntülerini depolayan önemli bir yapıttır. Bu etkinlik genellikle Ransowmare ile ilişkilendirilir, ancak aynı zamanda yasal olabilir.
* **Windows kayıt defteri kalıcılık yöntemi**: Bu uyarı, yürütülebilir bir dosyayı Windows kayıt defterinde kalıcı hale getirme girişimini belirtir. Kötü amaçlı yazılımlar genellikle bir önyüklemeden kurtulmak için böyle bir teknik kullanır.
* **Şüpheli yeni güvenlik duvarı kuralı**: Bu uyarı, şüpheli bir konumdaki yürütülebilir dosyadan gelen trafiğe izin vermek için *netsh.exe* aracılığıyla yeni bir Güvenlik Duvarı kuralının eklendiğini belirtir.
* **Şüpheli XCOPY yürütmeleri**: Bu uyarı bir dizi makinelerinizi birini tehlikeye girdiği ve kötü amaçlı yazılım yaymak için kullanıldığı sinyal XCOPY yürütmeleri gösterir.
* **Oturum açma sırasında kullanıcılara gösterilen yasal bildirimi gizlemeden**: Bu uyarı yasal bir bildirim gösterilip gösterilmeyeceğini denetleyen kayıt defteri anahtarı için bir değişiklik, kullanıcılara gösterilir, oturum açtığında belirtir. Bu, bir konağı tehlikeye attıktan sonra saldırganlar tarafından gerçekleştirilen yaygın bir etkinliktir.
* **Komut satırında büyük ve küçük harflerin anormal karışımı algılandı**: Bu uyarı, büyük/küçük harfe duyarlı veya karma tabanlı makine kuralından gizlenmek için saldırganlar tarafından kullanılan bir teknik olarak komut satırında büyük ve küçük harflerin anormal bir karışımının kullanıldığını belirtir.
* **Karıştırılmış komut satırı**: Bu uyarı, komut satırında şüpheli karartma göstergelerinin algılandığını belirtir.
* **Birden fazla etki alanı hesabı sorgulandı**: Saldırganlar genellikle kullanıcılar, etki alanı yönetici hesapları, etki alanı denetleyicileri ve etki alanları arasındaki güven ilişkileri üzerinde keşif yaparken AD etki alanı hesaplarını sorgular. Bu uyarı, kısa süre içinde birbirinden farklı çok fazla sayıda etki alanı hesabının sorgulandığını belirtir.
* **Olası yerel keşif etkinliği**: Bu uyarı, Keşif etkinliği ile ilişkili sistem bilgisi komutlarının bir birleşimini yürütüldüğünü belirtir.  *systeminfo.exe* yasal bir Windows aracı olsa da, art arda iki kez yürütülmesi nadir olarak görülür.
* **Keygen yürütülebilir dosyası yürütme olasılığı**: Bu uyarı, adıyla bir keygen aracını ifade edebilecek bir işlemin yürütüldüğünü belirtir. Bu tür araçlar genellikle yazılım lisanslama mekanizmalarını aşmak için kullanılır, ancak genellikle diğer kötü amaçlı yazılımlar ile paket halinde indirilir.
* **rundll32.exe aracılığıyla şüpheli yürütme**: Bu uyarı, saldırganlar tarafından tehlikeye girmiş bir konağa ilk aşamada yerleştirme yüklemek amacıyla kullanılan işlem adlandırma şemasıyla tutarlı, yaygın olmayan bir ad ile işlem yürütmek için rundll32.exe dosyasının kullanıldığını belirtir.
* **HTA ile PowerShell’in şüpheli birleşimi**: Bu uyarı bir Microsoft HTML Uygulama Konağı’nın (HTA) PowerShell komutları başlattığını belirtir. Bu teknik, saldırganlar tarafından kötü amaçlı PowerShell betikleri başlatmak için kullanılır.
* **UAC’yi atlamak için kötüye kullanılabilecek bir kayıt defteri değişikliği**: Bu uyarı, genellikle tehlikeye atılmış bir konakta saldırganlar tarafından ayrıcalıksız (standart kullanıcı) erişimden ayrıcalıklı (örneğin, yönetici) erişimine geçmek üzere kullanılan bir teknik olarak, UAC’yi (Kullanıcı Hesabı Denetimi) atlamak için kötüye kullanılabilecek bir kayıt defteri anahtarı olduğunu belirtir.
* **Komut satırı içinde şüpheli etki alanı adı kullanımı**: Bu uyarı şüpheli bir etki alanı adının kullanıldığını belirtir; bu etki alanı, bir saldırganın kötü amaçlı araçlar barındırdığına yönelik bir kanıt ve verilerin komut ve denetimi ile dışarı sızması için uç nokta olabilir.
* **24 saatlik süre içinde birden çok konakta hesap oluşturuldu**: Bu uyarı, birden çok konakta aynı kullanıcı hesabını oluşturmak için deneme yapıldığını belirtir; bu durum, bir veya daha fazla ağ varlığı tehlikeye girdikten sonra bir saldırganın ağ üzerinde yanal olarak hareket ettiğini gösterebilir.
* **Sistemin güvenlik durumunu indirgemek için şüpheli CACLS kullanımı**: Bu uyarı, değişiklik erişim denetim listesinin (CACLS) değiştirildiğini belirtir. Bu teknik genellikle saldırganlar tarafından ftp.exe, net.exe, wscript.exe gibi sistem ikili dosyalarına tam erişim vermek üzere kullanılır.
* **Şüpheli Kerberos Altın Bilet saldırısı parametreleri**: Bu uyarı, bir Kerberos Altın Bilet saldırısıyla tutarlı komut satırı parametrelerinin yürütüldüğünü belirtir. Güvenliği aşılmış bir krbtgt anahtarı, saldırgan tarafından dilediği kullanıcının kimliğine bürünmek için kullanılabilir.
* **WDigest UseLogonCredential kayıt defteri anahtarını etkinleştirme**: Bu uyarı, oturum ardından bellekten toplanan LSA bellekte düz metin olarak depolanması için kimlik bilgilerini verin kayıt defteri anahtarının değiştirildiğini belirtir.
* **Telegram aracının şüpheli olabilecek kullanımı**: Bu uyarı, saldırganların başka bir bilgisayar, telefon veya tablete kötü amaçlı ikili dosyalar aktarmak için kullandığı ücretsiz, bulut tabanlı bir anlık mesajlaşma hizmeti olan Telegram’ın yüklendiğini belirtir.
* **Yeni ASEP oluşturma**: Bu uyarı, komut satırında tanımlanan işlem adının otomatik olarak başlatılmasına neden olan ve kalıcılığı sağlamak için bir saldırgan tarafından kullanılabilen yeni bir ASEP (Otomatik Başlatma Genişletilebilirlik Noktası) oluşturulduğunu belirtir.
* **Şüpheli Set-ExecutionPolicy ve WinRM değişiklikleri**: Bu uyarı, kötü amaçlı ChinaChopper web kabuğunun kullanımıyla ilişkili yapılandırma değişikliklerini belirtir.
* **Kritik hizmetleri devre dışı bırakma**: Bu uyarı, SharedAccess veya Windows Güvenlik Merkezi gibi kritik hizmetleri durdurmak için "net.exe stop" komutunun kullanıldığını belirtir.
* **FTP -s anahtarının şüpheli kullanımı**: Bu uyarı, uzak bir FTP sunucusuna bağlanmak ve ek kötü amaçlı ikili dosyalar indirmek için kötü amaçlı yazılım tarafından kullanılabilen FTP "-s" anahtarının kullanımını belirtir.
* **Şüpheli VBScript.Encode komutu yürütme**: Bu uyarı, betikleri okunamaz metinler halinde kodlayarak kullanıcıların kodu incelemesini zorlaştıran *VBScript.Encode* komutunun yürütüldüğünü belirtir.
* **VBScript HTTP nesne ayırma**: Bu uyarı, Komut İstemi ile bir VBScript dosyasının oluşturulduğunu belirtir; bu dosya, kötü amaçlı dosyaları indirmek için kullanılabilir.
* **Yapışkan tuş saldırısı**: Bu uyarı, bir saldırganın arka kapı erişimi sağlamak için bir erişilebilirlik ikili dosyasını (örneğin yapışkan tuşlar, ekran klavyesi, ekran okuyucusu) bozduğunu belirtir.
* **Petya fidye yazılımı göstergeleri**: Bu uyarı, Petya fidye yazılımı ile ilişkili tekniklerin gözlendiğini belirtir.
* **AMSI’yi devre dışı bırakma denemesi**: Bu uyarı, kötü amaçlı yazılımdan koruma algılamasını devre dışı bırakabilecek kötü amaçlı yazılımdan koruma tarama arabirimi (AMSI) özelliğini devre dışı bırakma denemesi yapıldığını belirtir.
* **Fidye yazılımı göstergeleri**: Bu uyarı, geleneksel olara kilit ekranı ve şifreleme fidye yazılımı ile ilişkilendirilen şüpheli bir etkinliği belirtir.
* **Şüpheli izleme toplama çıkış dosyası**: Bu uyarı bir izlemenin (örneğin, ağ etkinliği) toplandığını ve olağan dışı bir dosya türünde çıkarıldığını belirtir.
* **Yüksek riskli yazılım**: Bu uyarı, kötü amaçlı yazılım yüklemeyle ilişkilendirilmiş yazılım kullanımını belirtir. Saldırganlar genellikle bu uyarıda görülen araçlar gibi diğer durumlarda zararsız olacak araçları kötü amaçlı yazılımlarla birlikte paketler ve kötü amaçlı yazılımı arka planda sessizce yükler.
* **Şüpheli dosya oluşturma**: Bu uyarı, bir kimlik avı belgesindeki ek açıldıktan sonra güvenliği aşılmış bir konağa ek kötü amaçlı yazılımlar indirmek üzere saldırganlar tarafından kullanılan bir işlemin oluşturulduğunu veya yürütüldüğünü belirtir.
* **Komut satırında şüpheli kimlik bilgileri**: Bu uyarı, bir dosyayı yürütmek için şüpheli bir parola kullanıldığını belirtir. Bu teknik, Pirpi kötü amaçlı yazılımını yürütmek üzere saldırganlar tarafından kullanılmıştır.
* **Kötü amaçlı yazılım bırakma aracını yürütme olasılığı**: Bu uyarı, saldırganlar tarafından kötü amaçlı yazılım yüklemek için kullanılan bir dosya adını belirtir.
* **rundll32.exe aracılığıyla şüpheli yürütme**: Bu uyarı, saldırganların kullandığı işlem ekleme tekniğiyle tutarlı bir şekilde notepad.exe veya reg.exe dosyası yürütmek için rundll32.exe kullanıldığını belirtir.
* **Şüpheli komut satırı bağımsız değişkenleri**: Bu uyarı HYDROGEN etkinlik grubu tarafından kullanılan bir ters kabuk ile birlikte kullanılan şüpheli komut satırı bağımsız değişkenlerini belirtir.
* **Şüpheli belge kimlik bilgileri**: Bu uyarı, bir dosyayı yürütmek için kullanılan kötü amaçlı yazılımın şüpheli ve yaygın bir önceden hesaplanmış parola karması kullandığını belirtir.
* **Dinamik PS betiği oluşturma**: Bu uyarı bir PowerShell betiğinin dinamik olarak oluşturulduğunu belirtir. Saldırganlar IDS sistemlerinden kurtulmak amacıyla aşamalı olarak betik derlemek için bu tekniği kullanır.
* **Metasploit göstergeleri**: Bu uyarı bir dizi saldırgan yeteneği ve aracı sağlayan Metasploit çerçevesiyle ilişkili etkinliği gösterir.
* **Şüpheli hesap etkinliği**: Bu uyarı yakın zamanda güvenliği aşılmış bir hesabı kullanarak bir makineye bağlanma girişiminde bulunulduğunu gösterir.
* **Hesap oluşturma**: Bu uyarı, makine üzerinde yeni bir hesap oluşturulduğunu belirtir.


### <a name="crash-analysis"></a>Kilitlenme analizi


Kilitlenme bellek dökümü analizi, geleneksel güvenlik çözümlerini atlatabilen karmaşık kötü amaçlı yazılımları algılamak için kullanılan bir yöntemdir. Kötü amaçlı yazılımların çeşitli türleri, diske hiçbir zaman yazmayarak veya diske yazılmış yazılım bileşenlerini şifreleyerek virüsten koruma ürünleri tarafından algılanma olasılığını azaltmaya çalışır. Bu teknik, kötü amaçlı yazılımların geleneksel kötü amaçlı yazılımdan koruma yaklaşımlarıyla algılanmasını zor hale getirir. Ancak, bu tür kötü amaçlı yazılımlar çalışmak için bellekte iz bırakmak zorunda olduğundan bellek analizi kullanılarak algılanabilir.

Yazılım kilitlendiğinde bir kilitlenme dökümü kilitlenme sırasında belleğin bir kısmını yakalar. Kilitlenme durumu kötü amaçlı yazılımlardan, genel uygulama veya sistem sorunlarından kaynaklanabilir. Kilitlenme dökümündeki belleği analiz eden Güvenlik Merkezi, yazılımdaki açıklardan yararlanmak, gizli verilere erişmek ve tehlikeye giren bir makineye gizlice sızmak için kullanılan teknikleri algılayabilir. Analiz Güvenlik Merkezi arka ucu tarafından gerçekleştirildiği için bu özellik, ana bilgisayarların performansına en az etki ile sağlanır.

* **Kod ekleme bulundu**: Kod ekleme, çalışmakta olan işlemlere veya iş parçacıklarına yürütülebilir modüllerin eklenmesidir. Bu teknik, kötü amaçlı yazılımlar tarafından verilere erişmek, gizleme veya (örneğin, Kalıcılık) da kaldırılmasını önlemek için kullanılır. Bu uyarı, eklenen bir modülün kilitlenme bilgi dökümünde mevcut olduğunu belirtir. Yasal yazılım geliştiricileri, var olan bir uygulama ya da işletim sistemi bileşenini değiştirme ya da genişletme gibi nadir durumlarda kötü amaçlı olmayan amaçlar için kod ekleme gerçekleştirir. Güvenlik Merkezi, kötü amaçlı olan ve olmayan eklenen modülleri birbirinden ayırt etmenize yardımcı olmak için eklenen modülün bir şüpheli davranış profiline uygun olup olmadığını denetler. Bu denetimin sonucu, uyarının “SIGNATURE” alanı tarafından gösterilir ve uyarının önem derecesi, uyarı açıklaması ve uyarı düzeltme adımlarında yansıtılır.
* **Şüpheli kod kesimi**: Şüpheli kod kesimi uyarısı, bir kod kesiminin yansıtmalı ekleme ve boş işlem gibi standart olmayan yöntemler kullanılarak ayrıldığını belirtir. Bildirilen kod kesiminin özellik ve davranışlarıyla ilgili bağlam sağlamak için kod kesiminin ek özelliklerini işleme alınır.
* **Kabuk kodu bulundu**: Kabuk Kodu, kötü amaçlı yazılım bir yazılım güvenlik açığından yararlandıktan sonra çalıştırılan yüktür. Bu uyarı, kilitlenme dökümü analizinin kötü amaçlı yükler tarafından yaygın olarak gerçekleştirilen davranışları sergileyen yürütülebilir kodlar algıladığını belirtir. Bu davranış kötü amaçlı olmayan yazılımlar tarafından gerçekleştiriliyor olabilir, ancak normal yazılım geliştirme uygulamaları için alışıldık bir davranış değildir.
* **Modül ele geçirme bulundu**: Windows, yazılımların ortak Windows sistem işlevselliğinden yararlanmasına izin vermek için dinamik bağlantı kitaplıklarını (DLL) kullanır. DLL Ele Geçirme, kötü amaçlı iş yüklerinin rastgele kodların yürütülebileceği belleğe yüklenmesi için kötü amaçlı yazılım tarafından DLL yükleme sırası değiştirildiğinde gerçekleşir. Bu uyarı, kilitlenme döküm analizinde iki farklı yoldan yüklenip benzer ada sahip bir modülün algılandığını gösterir. Yüklenen yollardan biri, ortak bir Windows sistem ikili dosyası konumundan gelir. Yasal yazılım geliştiricileri, Windows işletim sistemini veya bir Windows uygulamasını izleme, genişletme gibi kötü amaçlı olmayan gerekçelerden dolayı nadiren DLL yükleme sırasını değiştirir. Güvenlik Merkezi, DLL yükleme sırasında yapılan kötü amaçlı değişikliklerle zararsız olabilecek değişikliklerin birbirinden ayırt edilmesine yardımcı olmak için yüklenen bir modülün şüpheli bir profile uygun olup olmadığını denetler.
* **Kendini gizleyen Windows modülü algılandı**: Kötü amaçlı yazılım, araya karışmak ve kötü amaçlı yazılımın gerçek yapısını sistem yöneticilerinden saklamak amacıyla Windows sistem ikili dosyaları (örn. SVCHOST.EXE) veya modülleri (örn. NTDLL.DLL) için yaygın olarak kullanılan adları kullanabilir. Bu uyarı, kilitlenme dökümü dosyalarının Windows sistem modülü adlarını kullanan, ancak normal Windows modüllerine yönelik diğer kriterleri karşılamayan modüller içerdiğinin algılandığını gösterir. Kendini gizleyen modülün diskteki kopyasının çözümlenmesi, bu modülün yasal mı yoksa kötü amaçlı mı olduğu konusunda daha fazla bilgi sağlayabilir.
* **Değiştirilen sistem ikili dosyası bulundu**: Kötü amaçlı yazılım, verilere gizlice erişmek veya güvenliği ihlal edilmiş bir sistemde kendini gizleyerek kalıcı olmak için çekirdek sistem ikili dosyalarını değiştirebilir. Bu uyarı, kilitlenme dökümü analizi tarafından bellekte veya diskte çekirdek Windows OS ikili dosyalarının değiştirildiğinin algılandığını gösterir. Yasal uygulama geliştiricileri, Sapmalar veya uygulama uyumluluğu gibi kötü amaçlı olmayan nedenlerle bellekte sistem modüllerini nadiren değiştirir. Güvenlik Merkezi, kötü amaçlı modüllerle yasal olabilecek modüllerin birbirinden ayırt edilmesine yardımcı olmak için değiştirilen modülün bir şüpheli profile uygun olup olmadığını denetler.

## <a name="network-analysis"></a>Ağ analizi
Güvenlik Merkezi ağ tehdidi algılaması, Azure IPFIX (İnternet Protokolü Akış Bilgileri Verme) trafiğinizden güvenlik verilerini otomatik olarak toplayarak çalışır. Tehditleri belirlemek amacıyla bu bilgileri genellikle birden fazla kaynaktan bilgileri ilişkilendirerek analiz eder.

* **Gelen SQL deneme yanılma girişimleri olasılığı**: Ağ trafiği analizinde şüpheli gelen SQL iletişimi algılandı. Bu etkinlik, SQL sunucularına karşı deneme yanılma girişimleriyle tutarlıdır.
* **Birden fazla kaynaktan şüpheli gelen RDP ağ etkinliği**: Ağ trafiği analizinde birden fazla kaynaktan anormal gelen Uzak Masaüstü Protokolü (RDP) iletişimi algılandı. Özellikle, örneği oluşturulmuş ağ verileri makinenize bağlanan benzersiz IP’leri gösterir ve bu ortam için anormal olarak kabul edilir. Bu etkinlik, birden çok konaktan (Botnet) RDP uç noktanızda deneme yanılma girişiminde bulunulduğunu gösterebilir.
* **Şüpheli gelen RDP ağ etkinliği**: Ağ trafiği analizinde anormal gelen Uzak Masaüstü Protokolü (RDP) iletişimi algılandı. Özellikle, örneği oluşturulmuş ağ verileri makinenize bağlanan yüksek sayıda gelen bağlantıyı gösterir ve bu ortam için anormal olarak kabul edilir. Bu etkinlik, RDP uç noktanızda deneme yanılma girişiminde bulunulduğunu gösterebilir.
* **Birden fazla hedefe şüpheli giden RDP ağ etkinliği**: Ağ trafiği analizinde birden fazla hedefe anormal giden Uzak Masaüstü Protokolü (RDP) iletişimi algılandı. Bu etkinlik, makinenizin güvenliğinin aşıldığını ve şu anda dış RDP uç noktalarında deneme yanılma girişimleri için kullanıldığını belirtebilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.
* **Şüpheli giden RDP ağ etkinliği**: Ağ trafiği analizinde anormal giden Uzak Masaüstü Protokolü (RDP) iletişimi algılandı. Özellikle, örneği oluşturulmuş ağ verileri makinenizden yüksek sayıda giden bağlantıyı gösterir ve bu ortam için anormal olarak kabul edilir. Bu etkinlik, makinenizin güvenliğinin aşıldığını ve şu anda dış RDP uç noktalarında deneme yanılma girişimleri için kullanıldığını belirtebilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.
* **Şüpheli gelen SSH ağ etkinliği**: Ağ trafiği analizinde anormal gelen SSH iletişimi algılandı. Özellikle, örneği oluşturulmuş ağ verileri makinenize bağlanan yüksek sayıda gelen bağlantıyı gösterir ve bu ortam için anormal olarak kabul edilir. Bu etkinlik, SSH uç noktanızda deneme yanılma girişiminde bulunulduğunu gösterebilir.
* **Birden çok kaynaktan şüpheli gelen SSH ağ etkinliği**: Ağ trafiği analizinde anormal gelen SSH iletişimi algılandı. Özellikle, örneği oluşturulmuş ağ verileri makinenize bağlanan benzersiz IP’leri gösterir ve bu ortam için anormal olarak kabul edilir. Bu etkinlik, birden çok konaktan (Botnet) SSH uç noktanızda deneme yanılma girişiminde bulunulduğunu gösterebilir.
* **Şüpheli giden SSH ağ etkinliği**: Ağ trafiği analizinde anormal giden SSH iletişimi algılandı. Özellikle, örneği oluşturulmuş ağ verileri makinenizden yüksek sayıda giden bağlantıyı gösterir ve bu ortam için anormal olarak kabul edilir. Bu etkinlik, makinenizin güvenliğinin aşıldığını ve şu anda dış SSH uç noktalarında deneme yanılma girişimleri için kullanıldığını belirtebilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.
* **Birden fazla hedefe şüpheli giden SSH ağ etkinliği**: Ağ trafiği analizinde birden fazla hedefe anormal giden SSH iletişimi algılandı. Özellikle, örneği oluşturulmuş ağ verileri benzersiz IP’lerinize bağlanan makinenizi gösterir ve bu ortam için anormal olarak kabul edilir. Bu etkinlik, makinenizin güvenliğinin aşıldığını ve şu anda dış SSH uç noktalarında deneme yanılma girişimleri için kullanıldığını belirtebilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.
* **Kötü amaçlı makine ile ağ iletişimi algılandı**: Ağ trafiği analizi, makinenizin Komut ve Denetim merkezi olabilecek bir yerle iletişim kurduğunu belirtir.
* **Güvenliği aşılmış olabilecek makine algılandı**: Ağ trafiği analizinde botnet’in bir parçası olarak hareket ettiği anlamına gelebilecek bir giden etkinlik algılandı. Analiz, genel DNS kayıtları ile birlikte kaynağınız tarafından erişilen IP'leri temel alır.


## <a name="sql-database-and-sql-data-warehouse-analysis"></a>SQL Veritabanı ve SQL Veri Ambarı analizi

Güvenlik Merkezi kaynak analizi, [Azure SQL Veritabanı için Tehdit Algılama](../sql-database/sql-database-threat-detection.md) ve Azure SQL Veri Ambarı özelliği ile tümleştirme gibi hizmet olarak platform (PaaS) hizmetlerine odaklanır. SQL Tehdit Algılama, veritabanlarına erişmeye ve veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar:

* **SQL Ekleme Güvenlik Açığı**: Bu uyarı, veritabanında bir uygulama hatalı SQL deyimi oluşturduğunda tetiklenir. Bu, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Hatalı deyim oluşturulmasının iki olası nedeni vardır:
    * Hatalı SQL deyimini oluşturan uygulama kodunda hata
    * Uygulama kodu veya depolanan yordamlar, hatalı SQL deyimi yapılandırılırken kullanıcı girişini temizlemez ve SQL Ekleme sırasında bu açıktan yararlanılabilir
* **Olası SQL ekleme**: Bu uyarı, SQL ekleme işlemine tanımlı uygulama güvenlik açığına karşı etkin bir açıktan yararlanma görüldüğünde tetiklenir. Bu, saldırganın güvenlik açığına sahip uygulama kodu veya depolanan yordamları kullanan kötü amaçlı SQL deyimleri eklemeye çalıştığı anlamına gelir.
* **Sıra dışı bir konumdan erişim**: Bu uyarı, SQL sunucusunun erişim deseninde değişiklik olduğunda, bir kişi SQL sunucusuna sıra dışı bir coğrafi konumdan eriştiğinde tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
* **Azure veri merkezinden erişim**: Bu uyarı, SQL sunucusunun erişim deseninde değişiklik olduğunda, bir kişi SQL sunucusuna kısa süre önce bu sunucuda görünen bir Azure veri merkezinden eriştiğinde tetiklenir. Bazı durumlarda uyarı güvenli bir işlem (Azure, Power BI, Azure SQL Sorgu Düzenleyicisi gibi platformlardaki yeni uygulamanız) algılar. Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
* **Sıra dışı bir sorumludan erişim**: Bu uyarı, SQL sunucusunun erişim deseninde değişiklik olduğunda, bir kişi SQL sunucusuna sıra dışı bir sorumludan (SQL kullanıcısı) eriştiğinde tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
* **Zararlı olabilecek bir uygulamadan erişim**: Bu uyarı, zararlı olabilecek bir uygulama veritabanına erişmeye çalıştığında tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
* **SQL kimlik bilgilerine deneme yanılma saldırısı**: Bu uyarı, farklı kimlik bilgileri kullanılarak sıra dışı sayıda başarısız oturum açma denemesi olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.

## <a name="contextual-information"></a>Bağlamsal bilgiler
Araştırma sırasında analistlerin, tehdidin doğası ve nasıl azaltılacağı hakkında bir sonuca ulaşabilmesi için ek bağlam gerekir.  Bir ağ anomalisinin algılandığını varsayalım. Böyle bir durumda, ağ genelinde veya hedeflenen kaynakla ilgili olarak ortaya çıkan diğer sorunları hesaba katmadan, gerçekleştirilecek bir sonraki eylemin ne olacağını anlamak çok zordur. Bununla birlikte yardımcı olmak için bir güvenlik olayı yapıtlar, ilgili olaylar ve araştırmacı yardımcı olabilecek bilgiler içerebilir. Ek bilgilerin kullanılabilirliği, algılanan tehdit türüne ve ortamınızın yapılandırmasına göre değişiklik gösterir ve tüm Güvenlik Olayları için geçerli değildir.

Sağlanan ek bilgiler (varsa) uyarı listesinin altındaki Güvenlik Olayında gösterilir. Burada aşağıdaki gibi bilgiler bulunabilir:

- Günlük temizleme olayları
- Bilinmeyen bir cihaza takılı olan PNP cihazı
- Uyarılar eyleme dönüştürülebilir değildir
- Yeni hesap oluşturma
- CertUtil aracı kullanılarak kodu çözülen dosya

![Olağan dışı erişim uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig20.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi’ndeki farklı güvenlik uyarısı türleri hakkında bilgi edindiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik olayı işleme](security-center-incident.md)
* [Azure Güvenlik Merkezi algılama özellikleri](security-center-detection-capabilities.md)
* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md)
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.
