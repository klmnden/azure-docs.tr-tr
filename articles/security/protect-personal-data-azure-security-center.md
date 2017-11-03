---
title: "Azure Güvenlik Merkezi ile kişisel verileri koruma | Microsoft Docs"
description: "Azure Güvenlik Merkezi'ni kullanarak kişisel verilerini koruma"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 3a941389713a4d3dbffbbfe8a717409927d85c6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a>Kişisel veriler ihlallerini ve saldırılarından korumak: Azure Güvenlik Merkezi

Bu makalede Azure Güvenlik Merkezi ihlallerini ve saldırılarından kişisel verilerinizi korumak için nasıl kullanılacağını anlamanıza yardımcı olur.

## <a name="scenario"></a>Senaryo 

Amerika Birleşik Devletleri'nde yönetim büyük seyahat şirket, İngiliz Adaları arasında yanı sıra Akdeniz ve Baltık seas masraflarını sunmaya işlemlerini genişletmektedir. Bu çalışmaları yardımcı olmak için onu İtalya, Almanya, Danimarka ve İngiltere dayanarak birkaç küçük seyahat satırları aldı

Şirket Microsoft Azure bulutta şirket verilerini depolamak için kullanır. Bu adlar, adresler, telefon numarası ve kredi kartı bilgileri gibi kişisel olarak tanımlanabilir bilgileri içerir. İnsan Kaynakları bilgi ayrıca gibi içerir:

- Adresler
- Telefon numaraları
- Vergi kimlik numaraları
- Sağlık bilgileri

Seyahat satır de ödül ve bağlılık programı üyelerinin büyük bir veritabanı tutar. Şirket çalışanları şirketin şubelere ağ erişimi ve seyahat tüm dünyada bulunan aracıları bazı şirket kaynaklarına erişimi vardır.
Kişisel veriler ağ üzerinden bu konumları ve Microsoft Veri Merkezi arasında geçen.

## <a name="problem-statement"></a>Sorun bildirimi

Azure kaynaklarını saldırılar tehdit endişe şirkettir. Bunlar yetkisiz kişilerin müşterilerin ve çalışanların kişisel veriler riskini önlemek isteyebilirsiniz. Hem önleme ve yanıt/düzeltme işlemi, hem de bulut kaynakları devam eden güvenliğini izlemek için etkili bir yol yönergeler isterler.
Güçlü bir günümüzün karmaşık ve organize saldırganlara karşı savunma hattı ihtiyaç duyar.

## <a name="company-goal"></a>Şirket hedefi

Şirketin hedeflerinden tehditlerine karşı koruma tarafından müşterilerin ve çalışanların kişisel verilerin gizliliği emin olmaktır. Hedeflerine hemen etkisini azaltmak için işaretlerine ihlal yanıt biridir. Güvenlik geçerli durumunu değerlendirmeye, güvenlik açığı yapılandırmaları tanımlamak ve düzeltmek için bir yol gerekir.

## <a name="solutions"></a>Çözümler

Microsoft Azure Güvenlik Merkezi (ASC) bir tümleşik güvenlik izleme ve ilke yönetimi çözümü sağlar. Bu, kullanımı kolay ve etkili tehdit önleme, saptama ve yanıtlama işlevlerini sunar.

### <a name="prevention"></a>Önleme

ASC güvenlik ilkeleri ayarlayın, yalnızca zaman erişim sağlamak ve güvenlik önerilerini uygulama olanak tanıyarak açıklarına yardımcı olur.

Bir güvenlik ilkesi belirtilen abonelik içindeki kaynaklar için önerilen denetimleri kümesini tanımlar. Yalnızca süresi erişimi saldırılara maruz kalma azaltma, Azure vm'lerine gelen trafik kilitlemek için kullanılabilir. Güvenlik önerileri, Azure kaynaklarınızın güvenlik durumunu çözümledikten sonra ASC tarafından oluşturulur.

#### <a name="how-do-i-set-security-policies-in-asc"></a>Güvenlik ilkeleri ASC nasıl ayarlarım?

Her bir abonelik için güvenlik ilkeleri yapılandırabilirsiniz. Güvenlik ilkesini değiştirmek için o aboneliğin sahibi veya katkıda bulunanı olmanız gerekir. Azure Portalı'nda, aşağıdakileri yapın:

1. Seçin **İlkesi** ASC panosunda.

2. İlke etkinleştirmek istediğiniz aboneliği seçin.

3. Seçin **önleme İlkesi** abonelik başına ilkelerini yapılandırmak için. **Sanal makinelerden veri toplama** ayarlanmalı **üzerinde.**

4. İçinde **önleme İlkesi** seçenekleri, select **üzerinde** abonelik için ilgili güvenlik önerilerini etkinleştirmek için.

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

Daha ayrıntılı yönergeler ve her etkinleştirilebilir İlkesi önerilerin bir açıklaması için bkz: [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).

#### <a name="how-do-i-configure-just-in-time-access-jit"></a>Zaman erişim (JIT) nasıl sadece yapılandırırım?

JIT etkinleştirilmişse, Güvenlik Merkezi gelen trafik, Azure vm'lerinin bir NSG kuralı oluşturarak kilitleyen. Aşağı gelen trafik için kilitlenir VM bağlantı noktalarını seçin. JIT erişimi kullanmak için aşağıdakileri yapın:

1. Seçin **zaman VM erişim döşemesinin sadece** ASC dikey.

2. Seçin **önerilen** sekmesi.

3. Altında **VM'ler**, etkinleştirmek istediğiniz sanal makineleri seçin. Bir VM yanında bir onay işareti koyar. 
4. Seçin **etkinleştirmek JIT** vm'lerde.
5. **Kaydet**’i seçin.

Daha sonra JIT için etkinleştirilen ASC önerir varsayılan bağlantı noktalarını görebilirsiniz. Ayrıca eklemek ve yapılandırmak istediğiniz yalnızca etkinleştirmek yeni bir bağlantı noktası zaman çözümde. **Saat VM erişimi hemen** Güvenlik Merkezi parçasında JIT erişimi için yapılandırılan VM sayısını gösterir. Ayrıca, geçen hafta yapılan onaylanan erişim istekleri sayısını gösterir.

![](media/protect-personal-data-azure-security-center/jit-vm.png)

Bunu yapmak yönergeler ve zaman Access'te sadece hakkında ek bilgi için bkz [tam zamanında kullanarak sanal makine erişimini yönetme.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

#### <a name="how-do-i-implement-asc-security-recommendations"></a>ASC güvenlik önerileri nasıl uygulansın mı?

Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde öneriler oluşturur. Gerekli denetimlerin yapılandırılması işlemi boyunca öneriler size rehberlik eder. 
1. Seçin **önerileri** döşeme ASC Panoda.

2. Her satır bir öneri temsil ettiği bir tablo biçiminde gösterilen önerileri görüntüleyin.

3. Filtre öneriler, select için **filtre** ve görmek istediğiniz önem ve durum değerleri seçin.

4. Geçerli olmayan bir öneri kapatmak için sağ tıklatın seçin ve **atla.**

5. Hangi öneri önce uygulanması gereken değerlendirin.

6. Öncelik sırasına göre önerileri geçerlidir.

Olası önerileri ve her uygulama nasıl kılavuzlarına listesi için bkz: [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)

### <a name="detection-and-response"></a>Algılama ile yanıtı

Bir tehdit algılandıktan sonra mümkün olan en kısa sürede yanıt vermek istediğiniz algılama ile yanıtı birlikte gidin.
ASC tehdit algılaması Azure kaynaklarınızdan, ağ ve bağlı iş ortağı çözümlerinden güvenlik bilgileri otomatik olarak toplayarak çalışır. Saldırganlar yeni ve giderek karmaşık ortaya çıkardıkça ASC kendi algılama algoritmalarını hızlı bir şekilde güncelleştirebilirsiniz. Tehdit algılama ASC'ın nasıl çalıştığı hakkında daha ayrıntılı bilgi için bkz: [Azure Güvenlik Merkezi algılama özellikleri](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).

#### <a name="how-do-i-manage-and-respond-to-security-alerts"></a>Nasıl yönetme ve güvenlik uyarılarını yanıtlama?

Öncelikli güvenlik uyarıları listesi sorun hızlı araştırmanız gereken bilgiler ile birlikte Güvenlik Merkezi'nde gösterilir. Güvenlik Merkezi, ayrıca nasıl saldırıyı düzeltmek öneriler içerir. Güvenlik uyarılarınızı yönetmek için aşağıdakileri yapın:

1. Seçin **güvenlik uyarıları** döşeme ASC panosunda. Her uyarı ayrıntılarını gösterir.

2. Tarih, durum ve önem derecesine göre uyarıları filtrelemek için seçin **filtre** ve görmek istediğiniz değerleri seçin.

3. Bir uyarıya yanıt vermek için bunu seçin, bilgileri gözden geçirin ve sonra Saldırıya uğrayan kaynağı seçin.

4. İçinde **açıklama** alan, önerilen düzeltmeyi dahil ayrıntılarını göreceksiniz.

![](media/protect-personal-data-azure-security-center/security-alerts.png)

Daha ayrıntılı güvenlik uyarılara yanıt verme hakkında yönergeler için bkz: [yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

Güvenlik Uyarıları araştırırken daha fazla yardım için şirket kendi SIEM çözümüyle ASC uyarıları tümleşebilir kullanılarak [Azure günlük tümleştirme](https://aka.ms/AzLog).

#### <a name="how-do-i-manage-security-incidents"></a>Güvenlik olayları nasıl yönetebilirim?

ASC bir güvenlik olayı sonlandırma zinciri desenlerle Hizala tüm uyarıların bir kaynak için bir toplama ' dir. Bir Olay, her olay hakkında daha fazla bilgi almanızı sağlayan ilgili uyarıların listesini ortaya çıkarır. Olaylar, güvenlik uyarıları kutucuğu ve dikey penceresi görünür.

Gözden geçirin ve güvenlik olayları yönetmek için aşağıdakileri yapın:

1. Seçin **güvenlik uyarıları** döşeme. bir güvenlik olayı algılanırsa, güvenlik uyarıları grafiğin altında görünür. Diğer uyarılardan farklı bir simge sahip olur.

2. Bu güvenlik olayı hakkında daha fazla ayrıntı için olay seçin. Ek ayrıntılar tam açıklamasını, kendi önem derecesi, geçerli durumu, Saldırıya uğrayan kaynağa, olay ve bu olayın eklenmiş olan Uyarılar için düzeltme adımları içerir.

Görmek için filtreleyebilirsiniz **olaylar yalnızca**, **yalnızca uyarıları**, veya **her ikisi de**.

#### <a name="how-do-i-access-the-threat-intelligence-report"></a>Tehdit Intelligence raporun nasıl erişirim?

ASC tehditleri tanımlamak için birden fazla kaynaktan bilgileri analiz eder. Takımlar araştırmak ve tehditleri düzeltmek olay yanıtlama yardımcı olmak için Güvenlik Merkezi algılandı tehdit hakkında bilgi içeren bir tehdit Intelligence rapor içerir.

Güvenlik Merkezi üç tür saldırı değişebilir tehdit rapor vardır.
Raporlar şunlardır:

- Etkinlik grubu raporu: saldırganlar, hedefler ve taktiği derin çekecek sağlar.

- Kampanya raporu: belirli saldırı Kampanyalar ayrıntılarını odaklanır.

- İş parçacığı özet raporu: önceki iki raporlarındaki tüm öğeleri içerir.

Bu tür bilgiler olay yanıtlama işlemi sırasında çok kullanışlıdır kaynağı saldırı, saldırganın sözleri ve ilerleyen bu sorunu azaltmak için yapmanız gerekenler anlamak için devam eden bir araştırma olduğu.

1. Tehdit Intelligence rapor erişmek için aşağıdakileri yapın:

2. Seçin **güvenlik uyarıları** döşeme ASC Panoda.

3. Daha fazla bilgi almak istediğiniz güvenlik uyarısı seçin.

4. İçinde **raporları** alan, tehdit Intelligence raporun bağlantısını tıklatın.

5. Bu indirebilirsiniz PDF dosyasını açar.

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

ASC tehdit Intelligence rapor hakkında ek bilgi için bkz: [Azure Güvenlik Merkezi tehdit Intelligence rapor.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)

### <a name="assessment"></a>Değerlendirme

Test etme, değerlendirme ve güvenlikle ilgili tutumunuzu değerlendirmesine yardımcı olması için ASC Qualys ile tümleşik güvenlik açığı değerlendirmesi için bulut aracıları, kendi sanal makine önerileri bileşen bir parçası olarak sağlar.

Qualys Aracısı Güvenlik Açığı ve sistem durumu izleme verilerini ASC olarak geri gönderir Qualys yönetim platformu için güvenlik açığı verilerini rapor eder. Bir güvenlik açığı değerlendirmesi çözüm eklemek için öneri görüntülenen **önerileri** ASC Pano dikey penceresinde.

Güvenlik açığı değerlendirme çözümü hedef sanal makineye yüklendikten sonra Güvenlik Merkezi sistem ve uygulamadaki güvenlik açıklarını saptayıp tanımlamak üzere sanal makineyi tarar. Algılanan sorunlar **Sanal Makine Önerileri** seçeneğinin altında gösterilir.

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a>Bir güvenlik açığı değerlendirmesi çözümü nasıl uygulansın mı? 

Bir sanal makine zaten dağıtılmış bir tümleşik güvenlik açığı değerlendirmesi çözüm yoksa, Güvenlik Merkezi yüklü olmasını önerir.

1. ASC panosunda üzerinde **önerileri** dikey penceresinde, select **bir güvenlik açığı değerlendirmesi çözüme ekleyin.**

2. Güvenlik Açığı değerlendirmesi çözümü yüklemek istediğiniz sanal makineleri seçin.

3. Tıklayın **[sayısı] Vm'lerinde yükleyin.**

4. Azure Market veya altındaki bir iş ortağı çözümü seçin **varolan bir çözümü kullanın** seçin **Qualys.**

5. Otomatik Güncelleştirme ayarlarını açıp kapatabilirsiniz **iş ortağı çözümleri** dikey.

Bir güvenlik açığı değerlendirmesi çözümü hakkında daha fazla yönerge için bkz: [Azure Güvenlik Merkezi'nde Güvenlik Açığı değerlendirmesi.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Merkezi Hızlı Başlangıç Kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [Azure Güvenlik Merkezi'ne giriş](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [Azure Güvenlik Merkezi uyarılarını Azure günlük tümleştirme ile tümleştirme](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [Artırma Azure Güvenlik Merkezi ile tümleşik güvenlik açığı değerlendirmesi](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)
