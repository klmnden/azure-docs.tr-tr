<properties
   pageTitle="Azure Güvenlik Merkezi ile çalışmaya başlama | Microsoft Azure"
   description="Bu belge, Azure Güvenlik Merkezi'nde güvenlik ilkelerini yapılandırmanıza yardımcı olur."
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
   ms.date="06/03/2016"
   ms.author="yurid"/>

# Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama
Bu belge, bu görevi gerçekleştirmeye ilişkin gerekli adımlarda size kılavuzluk ederek Azure Güvenlik Merkezi'nde güvenlik ilkelerini yapılandırmanıza yardımcı olur.

> [AZURE.NOTE] Bu belgedeki bilgiler Azure Güvenlik Merkezi önizleme sürümü için geçerlidir.

## Azure Güvenlik Merkezi nedir?
Azure Güvenlik Merkezi, artırılmış görünürlük aracılığıyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

## Güvenlik ilkeleri nedir?
Güvenlik ilkesi, belirtilen abonelik veya kaynak grubundaki kaynaklar için önerilen denetimler kümesini tanımlar. Azure Güvenlik Merkezi'nde, şirketinizin güvenlik gereksinimlerine veya uygulamaların türüne ya da her abonelikteki verilerin duyarlılığına göre Azure abonelikleriniz veya kaynak grubu için ilkeler tanımlarsınız.

Örneğin; geliştirme veya test için kullanılan kaynaklar, üretim uygulamaları için kullanılanlardan farklı güvenlik gereksinimlerine sahip olabilir. Benzer şekilde, PII (Kişisel Bilgiler) gibi düzenlenen veriler içeren uygulamalar, daha yüksek bir güvenlik düzeyi gerektirebilir. Azure Güvenlik Merkezi'nde etkinleştirilen güvenlik ilkeleri, olası güvenlik açıklarını tanımlamanıza ve tehdit risklerini azaltmanıza yardımcı olmak için güvenlik önerilerini ve izlemeyi yürütür.

## Abonelikler için güvenlik ilkelerini ayarlama

Güvenlik ilkeleri, her bir abonelik veya kaynak grubu için yapılandırılabilir. Güvenlik ilkesini değiştirmek için o aboneliğin Sahibi veya Katkıda Bulunanı olmanız gerekir. Azure Güvenlik Merkezi'nde güvenlik ilkelerini yapılandırmak için şu adımları izleyin:

1. Azure Güvenlik Merkezi panosunda **Güvenlik İlkesi** kutucuğuna tıklayın.

2. Sağ tarafta açılan **Güvenlik İlkesi - Her abonelik veya kaynak grubu için ilke tanımlama** dikey penceresinde, güvenlik ilkesini etkinleştirmek istediğiniz aboneliği seçin. Aboneliğin tümü yerine bir Kaynak Gruba ilişkin Güvenlik İlkesi'ni etkinleştirmeyi tercih ederseniz Kaynak Gruplarına ilişkin güvenlik ilkelerini ayarlama hakkında bilgi verdiğimiz bir sonraki bölüm için aşağı kaydırın.

    ![İlke tanımlama](./media/security-center-policies/security-center-policies-fig01.png)

3. Söz konusu abonelik için **Güvenlik İlkesi** dikey penceresi, aşağıda gösterilene benzer seçenekler kümesiyle birlikte açılır:

    ![Veri koleksiyonunu etkinleştirme](./media/security-center-policies/security-center-policies-fig1-new.png)

4. **Sanal makinelerden veri toplama** seçeneğinin **Açık** olduğundan emin olun. Bu seçenek, var olan ve yeni kaynaklar için otomatik günlük koleksiyonunu etkinleştirir. 

    >[AZURE.NOTE] Güvenlik izlemenin var olan ve yeni VM'lerin hepsi için kullanılabilir olmasını sağlamak üzere her bir aboneliğinizde veri koleksiyonunu açmanızı kesinlikle öneririz. Veri koleksiyonunu etkinleştirme, izleme aracısını yükler. Veri koleksiyonunu şimdi bu konumdan açmak istemiyorsanız bu işlemi daha sonra sistem durumu ve öneriler görünümlerinden gerçekleştirebilirsiniz. Ayrıca, yalnızca abonelik için veya VM'leri seçmek için de veri koleksiyonunu etkinleştirebilirsiniz. Desteklenen VM'ler hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) 

5. Depolama hesabınız henüz yapılandırılmamışsa **Güvenlik İlkesi**'ni açtığınızda aşağıdaki şekilde gösterilene benzer bir uyarı görebilirsiniz:

    ![Storage seçimi](./media/security-center-policies/security-center-policies-fig2.png)

6. Bu uyarıyı görürseniz bu seçeneğe tıklayıp aşağıdaki şekilde gösterildiği gibi bölgeyi seçin:

    ![Storage seçimi](./media/security-center-policies/security-center-policies-fig3.png)

7. Sanal makinelerinizin çalıştığı her bir bölge için, bu sanal makinelerden toplanan verilerin depolandığı depolama hesabını seçin. Bu işlem, gizlilik ve veri egemenliği amacıyla verileri aynı coğrafi alanda tutmanızı kolaylaştırır. Hangi bölgeyi kullanacağınıza karar verdiğinizde bölgeyi ve ardından depolama hesabını seçin.

8. **Depolama hesaplarını seçme** dikey penceresinde **Tamam**'a tıklayın.

    > [AZURE.NOTE] İsterseniz verileri çeşitli bölgelerdeki sanal makinelerden merkezi bir depolama hesabına toplayabilirsiniz. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md).

9. Bu abonelikte kullanmak istediğiniz güvenlik önerilerini etkinleştirmek için **Güvenlik İlkesi** dikey penceresinde **Açık**'a tıklayın. **Önleme ilkesi** seçeneğine tıklayın. **Güvenlik İlkesi** dikey penceresi aşağıda gösterildiği gibi açılır. 

    ![Güvenlik ilkelerini seçme](./media/security-center-policies/security-center-policies-fig1-1-new2.png)

Her bir seçeneğin ne yapacağını anlamak için şu tabloyu başvuru için kullanın:

| İlke | Durum Açık olduğunda |
|----- |-----|
| Sistem Güncelleştirmeleri | Her gün, geçerli sanal makine için hangi hizmetin yapılandırıldığına bağlı olarak Windows Update veya WSUS'tan kullanılabilir güvenlik ve kritik güncelleştirmeler listesini alır ve eksik güncelleştirmelerin uygulanmasını önerir. |
| Temel Kurallar | Her gün, sanal makineyi saldırı karşısında daha savunmasız hale getirebilecek olan işletim sistemi yapılandırmalarını çözümler ve bu güvenlik açıklarına değinen yapılandırma değişiklikleri önerir. İzlenmekte olan belirli yapılandırmalar hakkında daha fazla bilgi için bkz. [Önerilen temel kurallar listesi](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). |
| Uç Nokta Koruması | Virüsleri, casus yazılımları ve diğer kötü amaçlı yazılımları tanımlamaya ve kaldırmaya yardımcı olmak için tüm Windows sanal makinelerine sağlamak üzere uç nokta koruması önerir. 
| Ağ Güvenlik Grupları | Alt ağlara ve ağ arabirimlerine gelen ve giden trafiği denetlemek için [Ağ Güvenlik Grupları](../virtual-network/virtual-networks-nsg.md)'nın (NSG'ler) yapılandırılmasını önerir. Aksi belirtilmediği sürece bir alt ağ için yapılandırılan NSG'ler, tüm sanal makine ağ arabirimleri tarafından devralınır. Bir NSG'nin yapılandırılıp yapılandırılmadığını denetlemin yanı sıra, bu seçenek gelen trafiğe izin veren güvenlik kuralı varsa bunları tanımlamak için gelen güvenlik kurallarını değerlendirir. |
| Web Uygulaması Güvenlik Duvarı | Web Uygulaması Güvenlik Duvarı'nın sanal makinelerde şu durumlarda sağlanmasını önerir: [Örnek Düzeyinde Ortak IP](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) kullanıldığında ve 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde ilişkili NSG Gelen Güvenlik Kuralları yapılandırıldığında. Yük dengeli IP (VIP) kullanılır, ilişkili yük dengeleme ve gelen NAT kuralları 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırılır (daha fazla bilgi için bkz. [Load Balancer için Azure Resource Manager Desteği](../load-balancer/load-balancer-arm.md)) |
| Yeni Nesil Güvenlik Duvarı | Bu, ağ korumalarını Azure içinde yerleşik olan Ağ Güvenlik Grupları'na genişletir. Güvenlik Merkezi, Yeni Nesil Güvenlik Duvarı'nın önerildiği dağıtımları bulur ve sanal gereç sağlamanızı imkan tanır. |
| SQL Denetimi | Azure SQL Sunucuları'na ve Veritabanları'na erişim denetiminin uyumluluk, gelişmiş algılama ve araştırma amacıyla etkinleştirilmesini önerir. |
| SQL Saydam Veri Şifrelemesi | Verileriniz ihlal edilse bile okunabilir olmamaları amacıyla Azure SQL veritabanlarınız, ilişkili yedeklemeler ve işlem günlük dosyaları için bekleyen şifrelemenin etkinleştirilmesini önerir. |

11. Tüm seçenekleri yapılandırmayı tamamladıktan sonra öneriler içeren **Güvenlik İlkesi** dikey penceresinde **Tamam**'a tıklayın ve ilk ayarları içeren **Güvenlik İlkesi** dikey penceresinde **Kaydet**'e tıklayın.

## Kaynak gruplar için güvenlik ilkelerini ayarlama

Her kaynak grubu için güvenlik ilkelerinizi yapılandırmak isterseniz izleyeceğiniz adımlar aboneliklere ilişkin güvenlik ilkelerini ayarlamak için kullandığınız adımlara benzerdir. İkisi arasındaki temel fark, abonelik adını genişletmenizin ve benzersiz güvenlik ilkesini yapılandırmak istediğiniz kaynak grubunu seçmenizin gerekmesidir.

![Kaynak grubu seçimi](./media/security-center-policies/security-center-policies-fig4.png)

Kaynak grubunu seçtikten sonra **Güvenlik ilkesi** dikey penceresi açılır. **Devralma** seçeneği varsayılan olarak etkindir; bu da, bu kaynak grubu için tüm güvenlik ilkelerinin abonelik düzeyinden devralındığı anlamına gelir. Her kaynak grubu için özel güvenlik ilkesi istemeniz durumunda bu yapılandırmayı değiştirebilirsiniz. Bu durumda, **Benzersiz**'i seçip **Önleme ilkesi** seçeneğinde değişiklik yapmanız gerekir.

![Her kaynak grubu için güvenlik ilkesi](./media/security-center-policies/security-center-policies-fig5-new.png)

> [AZURE.NOTE] Abonelik düzeyi ilkesi ile kaynak grubu düzeyi ilkesi arasında bir çakışma olması durumunda, kaynak düzeyi ilkesi önceliklidir.


## Sonraki adımlar

Bu belgede, Azure Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md) - Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu izleme hakkında bilgi edinin
- [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanma hakkında sık sorulan soruları bulun
- [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun



<!----HONumber=Jun16_HO2-->


