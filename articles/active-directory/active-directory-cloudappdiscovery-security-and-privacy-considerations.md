---
title: "Cloud App Discovery güvenlik ve gizlilik konuları | Microsoft Docs"
description: "Bu konuda, bulut uygulaması bulma ile ilgili güvenlik ve gizlilik konuları açıklanmaktadır."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 2fce5c82-d3de-4097-808f-40214768df9e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 3ebcb5b3b4a84f7a5c25caa3f6b245f97bc8049f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud App Discovery güvenlik ve gizlilik konuları
Bu konu, nasıl veri toplanan, işlenen ve Azure Active Directory Cloud App Discovery içinde güvenli açıklar. Microsoft gizliliğinizi korumaya ve verilerinizin güvenliğini sağlamak için taahhüt eder. Yazılım geliştirme yaşam döngüsü yöntemler hizmet çalıştırma için güvenli hale getirmek için Microsoft uyar. Güvenli hale getirme ve verileri koruma Microsoft'taki önceliğe sahiptir.

> [!TIP] 
> Azure Active Directory'de tarafından geliştirilmiş (Azure AD), yeni aracısız Cloud App Discovery kullanıma [Microsoft Cloud App Security ile tümleştirme](https://portal.cloudappsecurity.com). 

## <a name="overview"></a>Genel Bakış
Cloud App Discovery Azure AD, bir özelliğidir ve Microsoft Azure'da barındırılır.  
Cloud App Discovery endpoint agent yönetilen BT makinelerden uygulama bulgu verilerini toplamak için kullanılır. Toplanan veriler güvenli bir şekilde Azure AD Cloud App Discovery hizmetine şifrelenmiş bir kanal üzerinden gönderilir. Bir kuruluş için Cloud App Discovery veri ardından Azure portalında görünür olur. 

![Cloud App Discovery nasıl çalışır?](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png) 

Aşağıdaki bölümlerde bilgi güvenli akışını kuruluşunuzun Cloud App Discovery hizmetine izleyin ve sonuçta Cloud App Discovery portalı.

## <a name="collecting-data-from-your-organization"></a>Kuruluşunuzdaki verileri toplama
Kuruluşunuzdaki çalışanlar tarafından kullanılan uygulamaları Öngörüler almak için Azure Active Directory'nin Cloud App discovery özelliğini kullanabilmek için önce kuruluşunuzda makineler için Azure AD Cloud App Discovery endpoint agent dağıtmanız gerekir.

Yöneticiler Azure Active Directory kiracısı (veya kendi temsilci) aracı yükleme paketi Azure Portalı'ndan yükleyebilir. Aracı ya da el ile yüklenebileceğini veya birden çok makine SCCM veya Grup İlkesi kullanılarak kuruluşta yüklü.

Dağıtım seçenekleri hakkında daha ayrıntılı yönergeler için bkz: [bulut uygulama bulma Grup İlkesi Dağıtım Kılavuzu](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).


### <a name="data-collected-by-the-agent"></a>Aracı tarafından toplanan veriler
Bir Web uygulaması için bir bağlantı yapıldığında aşağıdaki listede gösterilen bilgiler aracı tarafından toplanır. Bilgiler, yalnızca yönetici bulma için yapılandırılmış uygulamalar için toplanır. Microsoft Azure ad Cloud App Discovery'yi aracı izler bulut uygulamalarının listesini düzenleyebilirsiniz [Azure portal](https://portal.azure.com/)altında **ayarları**->**veri toplama**  -> **Uygulama koleksiyon listesi**. 

**Bilgileri kategorisi**: kullanıcı bilgileri  
**Açıklama**: Windows Güvenlik tanımlayıcısı (SID yanı sıra) kullanıcının hedef Web uygulaması (örneğin, etki alanı\kullanıcı adı) istekte işlemin Windows kullanıcı adı.

**Bilgileri kategorisi**: işlem bilgileri  
**Açıklama**: Hedef Web uygulaması (örneğin, iexplore.exe) istekte işlemin adı

**Bilgileri kategorisi**: Makine bilgileri  
**Açıklama**:, aracı yüklü olduğu bilgisayarın NetBIOS adı.

**Bilgileri kategorisi**: uygulama trafiği bilgileri  
**Açıklama**: Aşağıdaki bağlantı bilgileri:

* Kaynak (yerel bilgisayar) ve hedef IP adresleri ve bağlantı noktası numaraları
* İstek üzerinden gider kuruluş ortak IP adresi.
* İstek süresi
* Gönderilen ve alınan trafik hacmi
* IP sürümüne (4 veya 6)
* Yalnızca TLS bağlantıları için: sunucu adı göstergesi uzantısı veya sunucu sertifikası hedef ana bilgisayar adı.

Aşağıdaki HTTP bilgileri:

* Yöntem (GET, POST, vb.)
* Protokol (HTTP/1.1, vb.)
* Kullanıcı aracısı dizesi
* Ana Bilgisayar Adı
* Hedef URI (hariç sorgu dizesi)
* İçerik türü bilgileri
* Başvuran URL bilgileri (sorgu dizesi hariç)

> [!NOTE]
> Yukarıdaki HTTP bilgiler tüm şifreli olmayan bağlantıları için toplanır.
> TLS bağlantıları için portalda 'Ayrıntılı İnceleme' ayarı açık olduğunda bu bilgileri yalnızca yakalanır. 'ON' varsayılan ayardır.
> Daha fazla ayrıntı için aşağıda bkz ve [alma başlatılmış olan Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 

Aracı ağ etkinliği hakkında topladığı veriler ek olarak, bu da anonim hakkında toplar
* Yazılım ve donanım yapılandırma
* Hata raporları
* Aracının nasıl kullanıldığını üzerinde verileri.


### <a name="how-the-agent-works"></a>Aracının nasıl çalışır?
Aracı yüklemesi, iki bileşenleri içerir:

* Bir kullanıcı modu bileşeni
* Bir çekirdek modu sürücüsü bileşeni (Windows Filtre Platformu'nu sürücüsü)

Aracısı'nı ilk kez yüklendiğinde sonra Cloud App Discovery hizmetine ile güvenli bir bağlantı kurmak için kullandığı makinedeki makine özel bir güvenilir sertifika depolar. Aracı, bu güvenli bağlantı üzerinden Cloud App Discovery hizmetinden ilkesi yapılandırması düzenli olarak alır. İlkenin hangi bulut uygulamalarının izleme ve olup otomatik güncelleştirme, diğerlerinin yanında etkinleştirilmelidir hakkındaki bilgileri içerir.

Web trafiği gönderilen ve Internet Explorer ve Chrome makineden alınan gibi Cloud App Discovery Aracısı trafiğini analiz eder ve ilgili meta verileri ayıklar (bkz **aracısı tarafından toplanan veriler** yukarıdaki bölümde).  
Dakikada, şifrelenmiş bir kanal üzerinden Cloud App Discovery hizmetine toplanan meta Aracısı'nı yükler.

Sürücü bileşen şifrelenmiş trafik durdurur ve kendisini şifrelenmiş akışa ekler. Daha ayrıntılı olarak **şifreli bağlantıları (ayrıntılı İnceleme) verilerden kesintiye uğratan** bölümüne bakın.

### <a name="respecting-user-privacy"></a>Saygı kullanıcı gizliliği
Amacımız yöneticilerinin kuruluşlarında uygun şekilde uygulama kullanımı ve kullanıcı gizlilik içine ayrıntılı optik arasındaki dengeyi ayarlamak için Araçlar sağlamaktır. Bu amaçla, Portalı'ndaki Ayarları sayfasında aşağıdaki düğmelerini sağlar:

* **Veri toplama**: Yöneticiler hangi uygulamaları veya uygulama kategorilerini istedikleri bulma veri almanın belirtmeyi seçebilirsiniz.
* **Ayrıntılı İnceleme**: Yöneticiler aracı HTTP trafiği için SSL/TLS bağlantılarını toplar olmadığını belirtmek seçebilirsiniz (diğer adıyla **'Ayrıntılı İnceleme'**). Bu sonraki bölümde hakkında daha fazla.
* **Seçenekler onayı**: Yöneticiler veri toplama aracısı tarafından kullanıcılara bildirmek verip vermeyeceklerini Cloud App Discovery Portalı'nı kullanabilir ve kullanıcı istenip istenmeyeceğini kullanıcı verileri toplama Aracısı başlamadan önce onay vermiş olursunuz.

Cloud App Discovery endpoint agent yalnızca açıklanan bilgileri toplar **aracısı tarafından toplanan veriler** yukarıdaki bölümde.

### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Kesintiye uğratan verileri şifreli bağlantıları (ayrıntılı İnceleme)
Yukarıda belirtildiği gibi verileri şifreli bağlantıları ('ayrıntılı İnceleme') izlemek için aracı yapılandırabilirler. TLS ([Aktarım Katmanı Güvenliği](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)), Internet'te kullanılan en yaygın protokoller bugün biridir. TLS ile iletişim şifreleyerek güvenli ve özel iletişim kanalı web sunucusu ile bir istemci oluşturabilir; TLS kimlik doğrulaması kimlik bilgilerini geçirme için temel koruma sağlar ve hassas bilgilerin açığa çıkmasını önlemek.

TLS tarafından sağlanan uçtan uca güvenli şifreli kanal önemli güvenlik ve Gizlilik Koruması olanak sağlarken Protokolü genellikle kötü amaçlı veya alınan amaçları için kötüye. Çok bu nedenle, aslında, bu TLS genellikle "Evrensel güvenlik duvarı geçişini protokol" olarak adlandırılır. Kök sorunun çoğu güvenlik duvarı uygulama katmanı veriler SSL ile şifrelendiğinden TLS iletişim incelemek mümkün olmamasıdır. Bu bilgiye sahip olmak, saldırganlar sık bile en akıllı uygulama katmanı güvenlik duvarları için TLS tamamen görme ve yalnızca TLS iletişim ana bilgisayarlar arasında geçişine izin vermesi emin kullanıcı kötü amaçlı yüklerini iletmek için TLS yararlanın. Son kullanıcıların kendi güvenlik duvarları ve proxy sunucular tarafından zorlanan erişim denetimleri atlamak için TLS ortak proxy'leri olmayan TLS protokolleri İlkesi tarafından aksi engellenebilir güvenlik duvarı üzerinden tünel bağlanmasını kullanarak sık yararlanın.

Ayrıntılı İnceleme Cloud App Discovery aracının bir güvenilen man-in--middle davranmasına izin verir. Bir HTTPS korumalı kaynağa erişmek için bir istemci isteği yapıldığında, Endpoint Agent sürücü bağlantıyı durdurur ve SSL sertifikasını istemci adına hedef sunucuya yeni bir bağlantı alır oluşturur. Aracı (değil iptal edildi denetimi ve diğer sertifika denetimleri gerçekleştirerek) sertifika güvenebilir ve bu geçişi Endpoint Agent'ı sonra sunucu sertifikası bilgileri kopyalar ve bu bilgileri kullanarak bir kişiler tarafından ele sertifikası olarak--bilinen kendi sunucu sertifikası--oluşturur, ardından doğrular. İmzalı üzerinde-çalışma sırasında Windows güvenilir sertifika deposunda yüklü bir kök sertifikası endpoint agent tarafından kişiler tarafından ele sertifikasıdır. Bu otomatik olarak imzalanan sertifika aktarılamaz işaretlenir ve Yöneticiler için ACL vardı. Üzerinde oluşturulduğu makine hiçbir zaman ayrılmaz için tasarlanmıştır. Son kullanıcının istemci uygulaması kişiler tarafından ele sertifika aldığında, bu süreç boyunca tüm kök sertifikası için sertifika zinciri başarıyla doğrulamak için olduğundan bunu güvenir. Bu işlem, genellikle bir son kullanıcının bakış açısı aşağıda açıklandığı gibi birkaç uyarılarla saydamdır.

Ayrıntılı İnceleme etkinleştirerek, Cloud App Discovery Endpoint Agent şifresini çözmek ve şifrelenmiş TLS iletişimleri, şifrelenmiş bulut uygulamalarını kullanımı hakkında Öngörüler gürültü küçültebilir ve hizmet veren inceleyin.

#### <a name="a-word-of-caution"></a>Bir sözcük uyarı notu
Ayrıntılı İnceleme üzerinde kapatmadan önce amaçları yasal ve HR Departmanlar için iletişim ve bunların onayı almak önemle önerilir. Son kullanıcının özel şifreli iletişim inceleniyor önemli bir konu belirgin nedenleri olabilir. Bir üretim Top-dışı ayrıntılı İnceleme önce Kurumsal güvenlik emin olun ve kabul edilebilir kullanım ilkeleri şifreli iletişim denetlenecek belirtmek için güncelleştirilmiştir. Kullanıcı bildirimi ve hassas kabul sitelerin (örneğin bankacılık ve tıbbi siteler) muafiyet Ayrıca bunları izlemek için Cloud App Discovery yapılandırırsanız gerekli olabilir. Yukarıda belirtildiği gibi yöneticilerin veri toplama aracısı tarafından kullanıcılara mı yoksa ve kullanıcı verilerini toplama Aracısı'nı başlatmadan önce kullanıcı izni istenip istenmeyeceğini seçmek için Cloud App Discovery Portalı'nı kullanabilirsiniz.

### <a name="known-issues-and-drawbacks"></a>Bilinen sorunlar ve sakıncaları
Burada TLS kişiler tarafından ele son kullanıcı deneyimini etkileyebilecek bazı durumlar vardır:

* Genişletilmiş Doğrulama (EV) sertifikaları web tarayıcısının Adres çubuğuna yeşil, güvenilen bir web sitesini ziyaret ettiğiniz bir görsel ipucu davranacak şekilde işleyebilir. TLS denetleme EV EV sertifikaları kullanan web siteleri normal şekilde çalışması için istemcinin, verdiği sertifikanın aynı olamaz, ancak Adres çubuğuna yeşil görüntülenmez.  
* (Sertifika sabitleme olarak da bilinir) ortak anahtar sabitleme kullanıcılar man-in--middle saldırılarına karşı koruyun ve sertifika yetkililerini standart dışı yardımcı olmak için tasarlanmıştır. Sabitlenmiş bir site için kök sertifika bilinen iyi CA'ın birini eşleşmediğinde tarayıcı bir hata ile bağlantı reddeder. TLS kişiler tarafından ele, aslında bir adam-in--middle, olduğundan bu bağlantıları başarısız olur.
* Kullanıcıların tarayıcıda site bilgilerini incelemek için tarayıcı adres çubuğunda kilit simgesini tıklatın, Web sitesi sertifikasını imzalamak için kullanılan sertifika yetkilisi bitiş zinciri görmezsiniz, ancak bunun yerine Windows ile biten bir sertifika zinciri güvenilen sertifika deposunda.

Bu sorunları oluşumları azaltmak için biz bulut Hizmetleri ve etkilenen bağlantıları kesintiye uğratan önlemek için istemci uygulamaları Genişletilmiş Doğrulama veya ortak anahtar sabitleme kullanın ve uç nokta Aracısı istemeniz bilinen takip edin. Bile bu durumda, ancak, raporları bu bulut uygulamalarının kullanımını, aktarılan verilerin hacmi ve almaya devam, ancak bunlar Denetlenmekte derin olmadığından uygulamaları nasıl kullanılan hakkında ayrıntı yok olacak.

## <a name="sending-data-to-cloud-app-discovery"></a>Cloud App Discovery için verileri gönderme
Meta veriler aracı tarafından toplanan sonra en fazla bir dakika için veya 5 MB boyutunu önbelleğe alınan veriler ulaşana kadar makinede önbelleğe alınır. Bunun ardından sıkıştırılır ve Cloud App Discovery hizmetine güvenli bir bağlantı üzerinden gönderilir.

Aracı herhangi bir nedenle Cloud App Discovery hizmetiyle iletişim kuramazsa, toplanan meta veriler yalnızca (örneğin, Yöneticiler grubu) makinedeki ayrıcalıklı kullanıcılar tarafından erişilebilecek bir yerel dosya önbellekte depolanır.  
Aracı tarafından Cloud App Discovery hizmetine başarıyla alındı kadar önbelleğe alınan meta veri göndermeyi otomatik olarak çalışır.

## <a name="receiving-the-data-at-the-service-end"></a>Hizmet sonunda veri alma
Aracıları makine belirli istemci kimlik doğrulama sertifikası kullanarak hizmet yukarıda başvurulan ve şifrelenmiş bir kanal üzerinden veri ileten Cloud App Discovery için kimlik doğrulaması.  
Cloud App Discovery hizmetin çözümlemeler işlem hattı meta verileri her müşteri için ayrı olarak, tüm analytics ardışık düzen aşamaları mantıksal olarak bölümleme göre işler.
Çözümlenen meta verileri çeşitli raporlar portalındaki denetler.

İşlenmemiş meta verileri ve çözümlenen meta verileri 180 gün için depolanır. Ayrıca, müşterilerin istediği bir Azure blob depolama hesabındaki çözümlenen meta veri yakalama seçebilirsiniz.
Bu, verilerin uzun bekletme yanı sıra meta verilerinin çevrimdışı analiz için kullanışlıdır.

## <a name="accessing-the-data-using-the-azure-portal"></a>Azure portalını kullanarak verilere erişme
Toplanan meta verileri güvenli tutmak için bir çaba içinde varsayılan olarak yalnızca genel yöneticileri Kiracı Cloud App Discovery Özelliği Azure portalında erişebilir.  
Bununla birlikte, Yöneticiler bu erişim diğer kullanıcılar veya gruplar için temsilci seçebilirsiniz.

> [!NOTE]
> Daha fazla ayrıntı için bkz: [alma başlatılmış olan Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 


Portal verilere herhangi bir kullanıcı gerekir lisansına bir Azure AD Premium lisansı.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Nasıl ı Kuruluşum içinde kullanılan onaylanmamış bulut uygulamaları bulabilir](active-directory-cloudappdiscovery-whatis.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)

