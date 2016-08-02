<properties
   pageTitle="Azure Güvenlik Merkezi'nde güvenlik durumunu izleme | Microsoft Azure"
   description="Bu belge, Azure Güvenlik Merkezi'nde izleme işlevlerini kullanmaya başlamanıza yardımcı olur."
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
   ms.date="06/06/2016"
   ms.author="yurid"/>

#Azure Güvenlik Merkezi'nde güvenlik durumunu izleme
Bu belge, ilkelerle uyumluluğu izlemek için Azure Güvenlik Merkezi'ndeki izleme işlevlerini kullanmanıza yardımcı olur.

> [AZURE.NOTE] Bu belgedeki bilgiler Azure Güvenlik Merkezi önizleme sürümü için geçerlidir.

## Azure Güvenlik Merkezi nedir?
Güvenlik Merkezi, artırılmış görünürlük aracılığı ile tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

##Güvenlik durumunu izleme nedir?
Genellikle izlemeyi, izleme ve bir olayın gerçekleşmesini bekleyip duruma tepki verme olarak ele alırız. Güvenliği izleme, kuruluş standartlarıyla veya en iyi uygulamalarla uyumlu olmayan sistemleri tanımlamak için kaynaklarınızı denetleyen öngörülü bir stratejiye sahip olma anlamına gelir.

##Güvenlik durumunu izleme
Bir aboneliğin kaynakları için [güvenlik ilkelerini](security-center-policies.md) etkinleştirmenizin ardından, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder.  Ağ yapılandırmanız hakkındaki bilgiler anında kullanılabilir; ancak sanal makine yapılandırması hakkındaki bilgilerin (güvenlik güncelleştirme durumu ve işletim sistemi yapılandırması gibi) kullanılabilir hale gelmesi bir saat veya daha uzun sürebilir. **KaynaK Güvenlik Durumu** dikey penceresinde herhangi bir sorunun yanı sıra kaynaklarınızın güvenlik durumunu da görüntüleyebilirsiniz. Ayrıca, bu sorunların bir listesini **Öneriler** dikey penceresinde görüntüleyebilirsiniz.

Önerileri uygulama hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md)'yı okuyun.

**Kaynak güvenlik durumu** kutucuğunda kaynaklarınızın güvenlik durumunu izleyebilirsiniz. Aşağıdaki örnekte yüksek ve orta önem derecesine sahip olan ve dikkat gerektiren birkaç sorun görebilirsiniz. Etkinleştirilmiş güvenlik ilkeleri, izlenen denetim türlerini etkiler.

![Kaynak durumu](./media/security-center-monitoring/security-center-monitoring-fig1-new3.png)

Güvenlik Merkezi, eksik güvenlik güncelleştirmelerine sahip bir VM veya [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) olmayan bir alt ağ gibi ilgilenilmesi gereken bir güvenlik açığı tanımlarsa güvenlik açıkları burada listelenir.

###Sanal makineleri izleme
**Kaynakların güvenlik durumu** kutucuğunda **Sanal makinelere** tıkladığınızda, **Sanal makineler** dikey penceresi, aşağıda gösterildiği gibi Güvenlik Merkezi tarafından izlenen tüm VM'lerin bir listesinin yanı sıra ekleme ve önleme adımları hakkında daha fazla ayrıntı sağlar.

![VM tarafından eksik sistem güncelleştirmesi](./media/security-center-monitoring/security-center-monitoring-fig2-2-new.png)

- Ekleme adımları
- Sanal makine önerileri
- Sanal makineler

Her bir bölüm için sorunu çözmek üzere önerilen adımla ilgili daha fazla ayrıntı görmeye yönelik ayrı ayrı seçenekler belirleyebilirsiniz. Aşağıdaki bölümler bu alanları daha ayrıntılı bir şekilde ele alır.

####Ekleme adımları
Bu bölüm, veri toplama ve bunların geçerli durumu için başlatılan sanal makinelerin toplam sayısını gösterir. Tüm VM'lerde veri toplama başlatıldıktan sonra, VM'ler Güvenlik Merkezi güvenlik ilkelerini almaya hazır olurlar. Bu girdiye tıkladığınızda, **Veri toplamayı başlatma** dikey penceresi açılır ve aşağıda gösterildiği gibi **YÜKLEME DURUMU** sütununda sanal makinelerin adlarını ve veri toplamanın geçerli durumunu görebilirsiniz.

![Başlatma durumu](./media/security-center-monitoring/security-center-monitoring-fig3-new.png)


####Sanal makine önerileri
Bu bölümde, Azure Güvenlik Merkezi tarafından izlenen her bir VM için bir öneri kümesi bulunur. İlk sütunda öneri listelenir; ikinci sütunda da öneriden etkilenen VM'lerin toplam sayısı listelenir ve üçüncü sütun ise aşağıda gösterildiği gibi sorunun önem derecesini gösterir.

![VM Önerileri](./media/security-center-monitoring/security-center-monitoring-fig4-2-new.png)

Her bir öneride, üzerine tıklandığında gerçekleştirilebilen bir eylemler kümesi bulunur. Örneğin, **Eksik sistem güncelleştirmelerine** tıklarsanız **Eksik sistem güncelleştirmeleri** dikey penceresi açılır. Yamaları eksik olan VM'ler ve eksik güncelleştirmenin önem derecesi listelenir.

![Eksik sistem güncelleştirmeleri](./media/security-center-monitoring/security-center-monitoring-fig5-new.png)

**Eksik sistem güncelleştirmeleri** dikey penceresi şu bilgileri içeren bir tablo gösterir:

- **SANAL MAKİNE**: Güncelleştirmeleri eksik olan sanal makinenin adı.
- **SİSTEM GÜNCELLEŞTİRMELERİ**: Eksik sistem güncelleştirmelerinin sayısı.
- **SON TARAMA ZAMANI**: Güvenlik Merkezi'nin güncelleştirmeler için VM'yi son tarama zamanı.
- **DURUM**: Önerinin geçerli durumu:
    - **Açık**: Öneri henüz ele alınmadı
    - **Devam Eden**: Öneri şu anda bu kaynaklara uygulanıyor; sizin bir eylem yapmanız gerekmez
    - **Çözümlenen**: Öneri zaten tamamlandı (sorun çözüldükten sonra girdi gri renkte görüntülenir).
- **ÖNEM DERECESİ**: Belirli bir önerinin önem derecesini açıklar:
    - **Yüksek**: Anlamlı bir kaynakta (uygulama, VM, ağ güvenlik grubu) bir güvenlik açığı var ve dikkat gerektiriyor
    - **Orta**: Bir işlemin tamamlanması veya bir güvenlik açığının ortadan kaldırılması için gereken kritik olmayan adımlar veya ek adımlar
    - **Düşük**: Bir güvenlik açığının ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler sunulmaz ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.)

Öneri ayrıntılarını görüntülemek için VM'nin adına tıklayın. Aşağıda gösterildiği gibi bu VM için bir güncelleştirme listesiyle yeni bir dikey pencere açılır.

![VM başına eksik sistem güncelleştirmeleri](./media/security-center-monitoring/security-center-monitoring-fig6-new.png)

> [AZURE.NOTE] Buradaki güvenlik önerileri, Öneriler dikey penceresindekilerle aynıdır. Önerileri çözümleme hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md) makalesine bakın. Bu yalnızca VM'ler için değil, Kaynak Durumu kutucuğunda kullanılabilen tüm kaynaklar için geçerlidir.

####Sanal makineler bölümü
Sanal makineler bölümü, tüm sanal makineler ve öneriler için bir genel bakış sağlar. Her sütun, aşağıda gösterildiği gibi bir dizi öneriyi temsil eder:

![VM'ler](./media/security-center-monitoring/security-center-monitoring-fig7-new.png)

Her önerinin altında görüntülenen simge, hangi VM'lerle ilgilenilmesi gerektiğini ve önerinin türünü hızla tanımlamanıza yardımcı olur.

Yukarıdaki örnekte, bir VM'nin kötü amaçlı yazılımdan koruma programları ile ilgili kritik bir önerisi vardır. VM hakkında daha fazla bilgi edinmek için üzerine tıklayın. Aşağıda gösterildiği gibi bu VM'yi temsil eden yeni bir dikey pencere açılır.

![VM Güvenlik Ayrıntıları](./media/security-center-monitoring/security-center-monitoring-fig8-new.png)

Bu dikey pencere, VM için güvenlik ayrıntıları içerir. Bu dikey pencerenin en altında önerilen eylemi ve sorunların önem derecesini görebilirsiniz.

###Sanal ağları izleme
**Kaynak güvenlik durumu** kutucuğunda **Ağ**'a tıkladığınızda, **Ağ** dikey penceresi aşağıda gösterildiği gibi daha fazla ayrıntıyla açılır.

![Ağ](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

####Ağ önerileri

Bu dikey pencere, sanal makinelerin kaynak durumu bilgilerine benzer şekilde dikey pencerenin en üstünde sorunların bir özet listesini ve en altında izlenen ağların bir listesini sağlar.

Ağ durumu döküm bölümü, olası güvenlik sorunlarını listeler ve öneriler sunar. Olası sorunlar şunları içerebilir:

- Yeni Nesil Güvenlik Duvarı (NGFW) yüklü değil
- Alt ağlardaki Ağ Güvenlik Grupları (NSG'ler) etkin değil
- VM'lerdeki NSG'ler etkin değil
- Genel dış uç nokta aracılığıyla harici erişimi kısıtlama
- İnternete yönelik sağlıklı uç noktalar

Bu önerilerden birine tıkladığınızda, aşağıdaki örnekte gösterildiği gibi öneri hakkında daha fazla ayrıntıyla yeni bir dikey pencere açılır.

![Uç noktayı kısıtlama](./media/security-center-monitoring/security-center-monitoring-fig11-new2.png)

Bu örnekte **Alt Ağlar için Eksik Ağ Güvenlik Gruplarını Yapılandırma** dikey penceresinde alt ağların ve NSG koruması eksik olan sanal makinelerin bir listesi bulunur. NSG'yi uygulamak istediğiniz alt ağa tıklarsanız başka bir dikey pencere açılır.

**Ağ güvenlik grubunu seçme** dikey penceresinde alt ağ için en uygun Ağ Güvenlik Grubu'nu seçebilir veya yeni bir Ağ Güvenlik Grubu oluşturabilirsiniz. 

####İnternet'e yönelik uç noktalar bölümü

**İnternet'e yönelik uç noktalar** bölümünde şu anda İnternet'e yönelik uç noktayla yapılandırılmış VM'leri ve geçerli durumlarını görebilirsiniz.

![İnternet'e yönelik uç nokta](./media/security-center-monitoring/security-center-monitoring-fig121-new5.png)

Bu tabloda VM'i temsil eden uç noktanın adı, İnternet'e yönelik IP adresi, NSG ve NGFW'nun geçerli önem derecesi durumu bulunur. Tablo aşağıda açıklandığı gibi önem derecesine göre sıralanır:
- Kırmızı (en üstte): yüksek önceliklidir ve hemen ilgilenilmesi gerekir 
- Turuncu: orta önceliklidir ve olabildiğince yakın zamanda ilgilenilmesi gerekir
- Yeşil (sonuncu): sağlıklı durum

####Ağ topolojisi bölümü

**Ağ topolojisi** bölümünde aşağıda gösterildiği gibi kaynakların hiyerarşik bir görünümü bulunur:

![Ağ topolojisi](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Bu tablo aşağıda açıklandığı gibi önem derecesine göre sıralanır (VM'ler ve Alt ağlar):
- Kırmızı (en üstte): yüksek önceliklidir ve hemen ilgilenilmesi gerekir 
- Turuncu: orta önceliklidir ve olabildiğince yakın zamanda ilgilenilmesi gerekir
- Yeşil (sonuncu): sağlıklı durum

Bu topoloji görünümünde ilk düzeyde [Sanal Ağlar](../virtual-network/virtual-networks-overview.md), [Sanal Ağ Geçitleri](../vpn-gateway/vpn-gateway-site-to-site-create.md) ve [Sanal Ağ (klasik)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) bulunur. İkinci düzeyde alt ağlar ve üçüncü düzeyde de bu alt ağlara ait VM'ler bulunur. Sağ sütunda bu kaynaklar için Ağ Güvenlik Grubu'nun (NSG) geçerli durumu bulunur. Aşağıdaki örnek VM VM-CL-W1 seçmenin sonucudur:

![Ağ ağacı](./media/security-center-monitoring/security-center-monitoring-fig13-new2.png)

Bu dikey pencerenin alt kısmı, yukarıda açıklanana benzer şekilde bu VM için öneriler içerir. Daha fazla bilgi edinmek veya gerekli güvenlik denetimini/yapılandırmasını uygulamak için bir öneriye tıklayabilirsiniz.

###SQL kaynaklarını izleme
**Kaynak güvenlik durumu** kutucuğunda **SQL**'ye tıkladığınızda SQL dikey penceresi denetim ve saydam veri şifrelemesinin etkinleştirilmemiş olması gibi sorunlar için önerilerle birlikte açılır. Ayrıca, veritabanının genel sağlık durumu için öneriler içerir.

![SQL Kaynak Durumu](./media/security-center-monitoring/security-center-monitoring-fig15-new.png)

Bu önerilerin herhangi birine tıklayabilir ve sorunu çözümlemek için daha çok eylem hakkında daha fazla bilgi edinebilirsiniz. Aşağıdaki örnekte, **Veritabanı Denetimi etkinleştirilmemiş** önerisinin genişletilmiş hali gösterilmektedir.

![SQL Kaynak Durumu](./media/security-center-monitoring/security-center-monitoring-fig16-new.png)

**SQL veritabanlarında Denetimi etkinleştirme** dikey penceresinde aşağıdaki bilgiler bulunur:

- SQL veritabanlarının bir listesi
- Bulundukları sunucu
- Bu ayarın sunucudan devralınmış olduğuna veya bu veritabanında benzersiz olduğuna dair bilgiler
- Geçerli durum
- Sorunun önem derecesi

Bu öneriyle ilgilenmek için veritabanına tıkladığınızda **Denetim ve Tehdit algılama** dikey penceresi aşağıda gösterildiği gibi açılır.

![SQL Kaynak Durumu](./media/security-center-monitoring/security-center-monitoring-fig17-new.png)

Denetimi etkinleştirmek için, **Denetim** seçeneğinin altında **AÇIK**'ı seçmeniz ve ardından **Kaydet**'e tıklamanız yeterlidir.

###Uygulamaları izleme
Azure iş yükünüzün sağlanmış web bağlantı noktaları (TCP bağlantı noktaları 80 ve 443) ile [kaynak yönetici VM'lerinde](../resource-manager-deployment-model.md) bulunan uygulamaları varsa olası güvenlik sorunlarını tanımlamak ve düzeltme adımları önermek için Güvenlik Merkezi bunları izleyebilir. **Uygulamalar** kutucuğuna tıkladığınızda, önleme adımları bölümünde bir dizi öneriyle birlikte **Uygulamalar** dikey penceresi açılır. Ayrıca, aşağıda gösterildiği gibi ana bilgisayar/sanal IP başına uygulama dökümünü gösterir.

![Uygulamaların güvenlik durumu](./media/security-center-monitoring/security-center-monitoring-fig18-new.png)

Diğer önerilerde yaptığınız gibi, sorun ve sorunun nasıl düzeltileceği hakkında daha fazla ayrıntı görmek için buna tıklayabilirsiniz. Aşağıdaki çizimde gösterilen örnekte, güvenli olmayan bir web uygulaması olarak tanımlanan bir uygulama bulunur. Güvenli kabul edilmeyen uygulamayı seçtiğinizde, aşağıdaki seçenek kullanılabilir durumda olacak şekilde başka bir dikey pencere açılır:

![Uygulamalar](./media/security-center-monitoring/security-center-monitoring-fig19-new.png)

**Güvenli Olmayan Web Uygulamaları** dikey penceresi, güvenli kabul edilmeyen uygulamaları içeren tüm VM'lerin bir listesine sahiptir. Liste; VM'nin adını, sorunun geçerli durumunu ve sorunun önem derecesini gösterir. Bu web uygulamasına tıklarsanız **Web Uygulaması Güvenlik Duvarı Ekleme** dikey penceresi aşağıda gösterildiği gibi bir üçüncü taraf WAF (web uygulaması güvenlik duvarı) yüklemenize yönelik seçeneklerle açılır.

![WAF ekleme](./media/security-center-monitoring/security-center-monitoring-fig20-new.png)

## Sonraki adımlar
Bu belgede, Azure Güvenlik Merkezi'nde izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) - Azure Güvenlik Merkezi'nde güvenlik ayarlarını yapılandırma hakkında bilgi edinin
- [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanma hakkında sık sorulan soruları bulun
- [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun



<!--HONumber=Jun16_HO2-->


