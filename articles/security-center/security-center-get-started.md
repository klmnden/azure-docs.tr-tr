<properties
   pageTitle="Azure Güvenlik Merkezi Hızlı Başlangıç Kılavuzu | Microsoft Azure"
   description="Bu belge, güvenlik izleme ve ilke yönetimi bileşenleri hakkında size rehberlik ederek ve sonraki adımlara yönlendirerek Azure Güvenlik Merkezi ile hızlıca çalışmaya başlamanıza yardımcı olur."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/26/2016"
   ms.author="terrylan"/>

# Azure Güvenlik Merkezi hızlı başlangıç kılavuzu

Bu belge, güvenlik izleme ve ilke yönetimi bileşenleri hakkında size rehberlik ederek ve sonraki adımlara yönlendirerek Azure Güvenlik Merkezi ile hızlıca çalışmaya başlamanıza yardımcı olur.

> [AZURE.NOTE] Bu belgedeki bilgiler Azure Güvenlik Merkezi önizleme sürümü için geçerlidir. Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır. Bu, adım adım ilerleyen bir kılavuz değildir.

## Azure Güvenlik Merkezi nedir?
 Güvenlik Merkezi, artırılmış görünürlük aracılığıyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

## Önkoşullar

Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Güvenlik Merkezi, aboneliğiniz ile etkinleştirilir. Bir aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

 Güvenlik Merkezi'ne [Azure portalından](https://azure.microsoft.com/features/azure-portal/) erişilir. Daha fazla bilgi edinmek için bkz. [portal belgeleri](https://azure.microsoft.com/documentation/services/azure-portal/).


## Güvenlik Merkezi'ne erişme

Güvenlik Merkezi'ne erişmek için portalda şu adımları izleyin:

1. **Gözat**'ı seçin ve ardından **Güvenlik Merkezi** seçeneğine kaydırın.
![Portalda Azure Güvenlik Merkezi'ne erişme][1]

2. **Güvenlik Merkezi**'ni seçin. Bu, **Güvenlik Merkezi** dikey penceresini açar.
3. Gelecekte, **Güvenlik Merkezi** dikey penceresine kolay erişim için **Panoya dikey pencereyi sabitleme** seçeneğini (sağ üstte) seçin.
![Dikey pencereyi panoya sabitleme seçeneği][2]

## Güvenlik Merkezi'ni Kullanma

Azure abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırabilirsiniz. Aboneliğiniz için bir güvenlik **ilkesi** yapılandıralım:

1. **Güvenlik Merkezi** dikey penceresinde **İlke** kutucuğunu seçin.
![Güvenlik Merkezi][3]

2. **Güvenlik ilkesi - Her bir abonelik veya kaynak grubu için ilke tanımlama** dikey penceresinde bir abonelik seçin.
![Azure Güvenlik Merkezi'ndeki güvenlik ilkesi dikey penceresi][4]

3. Günlükleri otomatik olarak toplamak için **Güvenlik ilkesi** dikey penceresinde **Veri koleksiyonu** seçeneğini etkinleştirin. **Veri koleksiyonu** seçeneği etkin hale gelirse abonelikteki tüm geçerli ve yeni VM'lere izleme uzantısı da sağlanır.
4. **Her bölge için bir depolama hesabı seçmeyi** belirleyin. Sanal makinelerinizin çalıştığı her bir bölge için bu sanal makinelerden toplanan verilerin depolandığı depolama hesabını seçin. Her bölge için bir depolama hesabı seçmezseniz sizin için oluşturulur. Toplanan veriler, güvenlik nedenleriyle diğer müşterilerin verilerinden mantıksal olarak yalıtılır.

     > [AZURE.NOTE] Veri koleksiyonunuzu açmanızı ve öncelikle abonelik düzeyinde olan bir depolama hesabı seçmenizi öneririz.  Güvenlik ilkeleri, Azure aboneliği düzeyinde ve kaynak grubu düzeyinde ayarlanabilir ancak veri koleksiyonu ve depolama hesabı yalnızca abonelik düzeyinde oluşur.

5. Güvenlik ilkenizin bir parçası olarak görmek istediğiniz **Öneriler** seçeneğini etkinleştirin. Örnekler:

 - **Sistem güncelleştirmeleri** seçeneği etkinleştirilirse eksik işletim sistemi güncelleştirmelerini bulmak için desteklenen tüm sanal makineler taranır.
 - **Temel kurallar** seçeneği etkinleştirilirse sanal makineyi saldırı karşısında daha savunmasız hale getirebilecek bir işletim sistemi yapılandırmasını tanımlamak üzere, desteklenen tüm sanal makineler taranır.

**Öneriler**'i ele alma:

1. **Güvenlik Merkezi** dikey penceresine geri dönüp **Öneriler** kutucuğunu seçin. Güvenlik Merkezi düzenli aralıklarla Azure kaynaklarınızın güvenlik durumunu çözümler. Olası güvenlik açıkları tanımlandığında, öneri burada gösterilir.
2.  Ek bilgileri görüntülemek ve/veya sorunu çözmek amacıyla eyleme geçmek için öneriye tıklayın.
![Azure Güvenlik Merkezi'nde öneriler][5]

**Kaynak güvenlik durumu** aracılığıyla kaynaklarınızın sistem ve güvenlik durumunu görüntüleyin:

1.  **Güvenlik Merkezi** dikey penceresine geri dönün.
2.  **Kaynak güvenlik durumu** kutucuğu; **Sanal makineler**, **Ağ**, **SQL** ve **Uygulamalar**'a ilişkin güvenlik durumu göstergelerini içerir.
3.  Daha fazla bilgi görüntülemek için **Sanal makineler** seçeneğini belirleyin.
4.  **Sanal makineler** dikey penceresi, kötü amaçlı yazılımdan koruma programlarının, sistem güncelleştirmelerinin, yeniden başlatmaların durumunu ve sanal makinelerinizin temel kurallarını gösteren bir durum özeti görüntüler.
5.  Daha fazla bilgi görüntülemek ve/veya gerekli denetimleri yapılandırmak amacıyla eyleme geçmek için **SANAL MAKİNE ÖNERİLERİ** altında bir öğe seçin.
6.  Belirli sanal makinelere ilişkin ek bilgileri görüntülemek için ayrıntılara göz atın.
![Azure Güvenlik Merkezi'nde kaynak durumu kutucuğu][6]

**Güvenlik Uyarıları**'nı ele alma:

1.  **Güvenlik Merkezi** dikey penceresine geri dönüp **Güvenlik Uyarıları** kutucuğunu seçin. **Güvenlik uyarıları** dikey penceresinde uyarıların bir listesi görüntülenir. Uyarılar, güvenlik günlüklerinizin ve ağ etkinliğinin Güvenlik Merkezi çözümlemesi tarafından oluşturulur. Tümleşik iş ortağı çözümlerinden gelen uyarılar da dahildir.
![Azure Güvenlik Merkezi'nde güvenlik uyarıları][7]

2.  Ek bilgileri görüntülemek için bir uyarı seçin.
![Azure Güvenlik Merkezi'nde güvenlik uyarıları ayrıntıları][8]

**İş ortağı çözümleri**'nizin sistem durumunu görüntüleyin:

1. **Güvenlik Merkezi** dikey penceresine geri dönün. **İş ortağı çözümleri** kutucuğu, Azure aboneliğinizle tümleşik iş ortağı çözümlerinizin sistem durumunu bir bakışta izlemenizi sağlar.
2. **İş ortağı çözümleri** kutucuğunu seçin. Güvenlik Merkezi'ne bağlı iş ortağı çözümlerinizin listesini görüntüleyen dikey pencere açılır.
![İş ortağı çözümleri][9]

3. İş ortağı çözümü seçin. Bu örnekte **F5 WAF2** çözümünü seçelim.  İş ortağı çözümünün durumunu ve çözümle ilişkili kaynakları gösteren dikey pencere açılır. Bu çözüme ilişkin iş ortağı yönetimi deneyimini açmak için **Çözüm konsolunu** seçin.
![İş ortağı çözümü ayrıntısı][10]

## Sonraki adımlar
Bu belgede, Güvenlik Merkezi'nde güvenlik izleme ve ilke yönetimi bileşenlerine giriş yaptınız. Daha fazla bilgi için şunlara bakın:

- [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) - Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
- [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) - Önerilerin Azure kaynaklarınızı korumanıza nasıl yardım edeceği hakkında bilgi edinin.
- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
- [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-get-started/security-tile.png
[2]: ./media/security-center-get-started/pin-blade.png
[3]: ./media/security-center-get-started/security-center.png
[4]: ./media/security-center-get-started/security-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/partner-solutions-detail.png



<!--HONumber=Jun16_HO2-->


