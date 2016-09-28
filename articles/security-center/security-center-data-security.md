<properties
   pageTitle="Azure Güvenlik Merkezi Veri Güvenliği | Microsoft Azure"
   description="Bu belgede Azure Güvenlik Merkezi'nde verilerin yönetilmesi ve korunması açıklanmaktadır."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="yurid"/>


# Azure Güvenlik Merkezi Veri Güvenliği
Müşterilerin tehditleri önlemesine, algılamasına ve yanıt vermesine yardımcı olmak amacıyla Azure Güvenlik Merkezi, Azure kaynaklarınıza ilişkin yapılandırma bilgileri, meta veriler, olay günlükleri, kilitlenme dökümü dosyaları ve diğer verileri toplar ve işler. Bu verilerin gizlilik ve güvenliğini korumak için önemli taahhütlerde bulunuyoruz. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır. 

Bu makalede Azure Güvenlik Merkezi'nde verilerin yönetilmesi ve korunması açıklanmaktadır.

## Veri Kaynakları
Azure Güvenlik Merkezi aşağıdaki kaynaklardan verileri analiz eder:

- Azure Hizmetleri: Dağıttığınız Azure hizmetlerinin yapılandırmasına ilişkin bilgileri, ilgili hizmetin kaynak sağlayıcısıyla iletişim kurarak okur.
- Ağ Trafiği: Microsoft’un altyapısından kaynak/hedef IP/bağlantı noktası, paket boyutu ve ağ protokolü gibi örneği alınmış ağ trafiği meta verilerini okur.
- İş Ortağı Çözümleri: Güvenlik duvarları ve kötü amaçlı yazılımdan koruma çözümleri gibi tümleşik iş ortağı çözümlerinden güvenlik uyarılarını toplar. Bu veriler şu anda Birleşik Devletler’de bulunan Azure Güvenlik Merkezi deposunda saklanır.
- Sanal Makineleriniz: Azure Güvenlik Merkezi, yapılandırma bilgilerini ve Windows olay ve denetim günlükleri, IIS günlükleri, syslog iletileri ve kilitlenme dökümü dosyaları gibi güvenlik olaylarına ilişkin bilgileri, veri toplama aracılarını kullanarak sanal makinelerinizden toplayabilir. Daha fazla ayrıntı için aşağıdaki "Veri Toplamayı yönetme" bölümüne bakın.  

Ayrıca, güvenlik uyarıları, öneriler ve güvenlik sistem durumuna ilişkin bilgiler şu anda Birleşik Devletler'de bulunan Azure Güvenlik Merkezi deposunda saklanır. Bu bilgiler size güvenlik uyarısı, öneri veya sistem durumu sağlamak üzere gerektiğinde sanal makinelerinizden toplanan ilgili yapılandırma bilgilerini ve güvenlik olaylarını içerebilir.

## Veri Koruma
**Veri ayırma**: Veriler hizmet boyunca her bir bileşende mantıksal olarak ayrı tutulur. Tüm veriler kuruluşa göre etiketlenir. Bu etiketleme, veri yaşam döngüsü boyunca devam eder ve her bir hizmet katmanında uygulanır. Ayrıca, sanal makinelerinizden toplanan veriler depolama hesaplarınıza depolanır.

**Veri erişimi**: Güvenlik önerileri sağlamak ve olası güvenlik tehditlerini araştırmak üzere Microsoft personeli, Azure hizmetleri tarafından toplanan ya da analiz edilen kilitlenme döküm dosyaları gibi bilgilere erişebilir. Kilitlenme döküm dosyaları ve işlem oluşturma olayları kasıtsız olarak müşteri verilerini veya sanal makinelerinizdeki kişisel verileri içerebilir. Microsoft’un Müşteri Verilerini kullanmayacağını veya reklam ya da benzeri ticari amaçlarla bundan bilgi türetmeyeceğini belirten [Microsoft Çevrimiçi Hizmet Koşulları](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) ve [Gizlilik Bildirimi](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx) belgelerine bağlı kalıyoruz. Müşteri Verileri yalnızca size Azure hizmetlerini sağlamak için, bu hizmetleri sağlamayla uyumlu amaçlar da dahil olmak üzere gerektiğinde kullanılır. Müşteri Verileri üzerindeki tüm haklarınız saklıdır.

**Veri kullanımı**: Microsoft önleme ve algılama özelliklerimizi geliştirmek amacıyla birden fazla kiracıda görülen modelleri ve tehdit bilgilerini kullanır; bunu [Gizlilik Bildirimi](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx) belgemizde açıklanan gizlilik taahhütlerine uygun şekilde yaparız.

**Veri konumu**: Sanal makinelerin çalıştığı her bölge için bir depolama hesabı belirtilir. Bunun yapılması verileri, verilerin toplandığı sanal makine ile aynı bölgede depolamanızı sağlar. Kilitlenme döküm dosyaları da dahil olmak üzere bu veriler, depolama hesabınıza kalıcı olarak depolanır. Hizmet ayrıca tümleşik iş ortağı çözümlerinden gelen uyarılar, öneriler ve sistem durumu dahil güvenlik uyarılarına ilişkin bilgileri şu anda Birleşik Devletler’de bulunan Azure Güvenlik Merkezi deposunda saklar.

## Sanal Makinelerden Veri Toplamayı Yönetme

Azure Güvenlik Merkezi’ni etkinleştirmeyi seçtiğinizde her aboneliğiniz için veri toplama etkinleştirilir. Veri toplamayı Azure Güvenlik Merkezi Panosunun “Güvenlik İlkesi” bölümünden kapatabilirsiniz. Veri Toplama açık olduğunda Azure Güvenlik Merkezi desteklenen tüm mevcut sanal makinelerde ve yeni oluşturulanlarda Azure Monitoring Agent’ı hazırlar. Azure Güvenlik İzleme uzantısı, güvenlikle ilgili çeşitli yapılandırmaları ve olayları [Windows için Olay İzleme](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) izlerinde tarar. Ayrıca, işletim sistemi, makinenin çalışması sırasında olay günlüğü olaylarını ortaya koyar. Bu tür verilerin örnekleri şunlardır: işletim sistemi türü ve sürümü, işletim sistemi günlükleri (Windows olay günlükleri), çalışan işlemler, makine adı, IP adresleri, oturum açmış kullanıcı ve kiracı kimliği. Azure Monitoring Agent olay günlüğü girişleri ile ETW izlerini okur ve analiz için depolama hesabınıza kopyalar. 

İçinde sanal makineler çalıştırdığınız her bölge için, ilgili bölgenin sanal makinelerinden toplanan verilerin depolandığı bir depolama hesabı belirtilir. Bu işlem, gizlilik ve veri egemenliği amacıyla verileri aynı coğrafi alanda tutmanızı kolaylaştırır. Her bölgenin depolama hesaplarını Azure Güvenlik Merkezi Panosunun “Güvenlik İlkesi” bölümünden yapılandırabilirsiniz.

Azure Monitoring Agent ayrıca kilitlenme dökümü dosyalarını depolama hesabınıza kopyalar.  Azure Güvenlik Merkezi, kilitlenme döküm dosyalarının kısa ömürlü kopyalarını toplar ve açıktan yararlanma girişimlerinin ve başarılı uzlaşmaların kanıtı olarak analiz eder.  Azure Güvenlik Merkezi bu analizi depolama hesabıyla aynı coğrafi bölgede gerçekleştirir ve analiz tamamlandığında kısa ömürlü kopyaları siler.

Sanal makinelerden veri toplamayı dilediğiniz zaman devre dışı bırakabilirsiniz; bunun yapılması Azure Güvenlik Merkezi tarafından daha önce yüklenmiş tüm İzleme Aracılarını kaldırır.


## Sonraki adımlar

Bu belgede Azure Güvenlik Merkezi'nde verilerin yönetilmesi ve korunması hakkında bilgi aldınız. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek bkz.:

- [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md) - Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin
- [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin
- [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmet kullanımı ile ilgili sık sorulan soruları burada bulabilirsiniz
- [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz



<!--HONumber=Sep16_HO3-->


