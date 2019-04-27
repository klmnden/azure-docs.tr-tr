---
title: Çalışmaya başlama - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Bu eylem tehdit modelleme aracı vurgulama daha ayrıntılı bir genel bakıştır.
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: jegeib
ms.openlocfilehash: 6315e6d39a3b68854beb6563d075e3c79ca93a69
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610819"
---
# <a name="getting-started-with-the-threat-modeling-tool"></a>Tehdit modelleme aracı ile çalışmaya başlama

Microsoft tehdit modelleme aracı 2018 GA Eylül 2018'de ücretsiz yayımlanan  **[tıklatın indirme](https://aka.ms/threatmodelingtool)**. Teslim mekanizması değişiklik müşteriler için en son geliştirmeleri ve hata düzeltmeleri, Bakımı ve kullanımı kolay duruma aracın her açtığınızda anında iletme olanak sağlıyor.
Bu makalede, Microsoft SDL tehdit modelleme yaklaşımı ile çalışmaya başlama işlemi boyunca size yol gösterir ve aracının güvenlik işleminin temel öğesi olarak harika tehdit modelleri geliştirmek için nasıl kullanılacağını gösterir.

Bu makalede, var olan modelleme yaklaşımı SDL tehdit bilgisi üzerinde oluşturur. Hızlı bir inceleme için başvurmak **[tehdit modelleme Web uygulamaları](https://msdn.microsoft.com/library/ms978516.aspx)** ve arşivlenmiş bir sürümünü **[ortaya çıkarmaya güvenlik açıkları kullanarak adım yaklaşım](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy)** MSDN makalesi, 2006'yayımladı.

Hızlı bir şekilde özetlemek için bir diyagram oluşturma, tehditleri tanımlama, bunları azaltma ve her risk azaltma doğrulanıyor yaklaşım gerektirir. Bu işlem vurgular bir diyagramda şu şekildedir:

![SDL işlemi](./media/azure-security-threat-modeling-tool-feature-overview/sdlapproach.png)

## <a name="starting-the-threat-modeling-process"></a>Tehdit modelleme işlemi başlatılıyor

Tehdit modelleme Aracı'nı başlattığınızda resimde görüldüğü gibi birkaç şeyi fark edeceksiniz:

![Boş bir başlangıç sayfası](./media/azure-security-threat-modeling-tool-feature-overview/tmtstart.png)

### <a name="threat-model-section"></a>Tehdit modeli bölümü

| Bileşen                                   | Ayrıntılar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Geribildirim, öneri ve sorunları düğmesi** | Sizi **[MSDN Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=sdlprocess)** ilgili SDL her şey. Diğer kullanıcıların, geçici çözümlere ve öneriler birlikte yaptığı aracılığıyla okumak için bir fırsat sunar. Aradığınızı hala bulamıyorsanız, e-posta tmtextsupport@microsoft.com yardımcı olması destek ekibimize için                                                                                                                            |
| **Model oluşturma**                          | Diyagram çizmek boş bir tuval açılır. Modeliniz için kullanmak istediğiniz şablonun seçtiğinizden emin olun                                                                                                                                                                                                                                                                                                                                                                       |
| **Yeni modeller için şablon**                 | Bir modeli oluşturmadan önce kullanılacak şablonunu seçmelisiniz. Azure'a özel şablonlar, tehditleri ve risk azaltma işlemleri içeren Azure tehdit modeli şablonu, bizim ana şablonudur. Genel modelleri için SDL TM Bilgi Bankası aşağı açılan menüden seçim yapın. Kendi şablonunuzu oluşturun veya tüm kullanıcılar için yeni bir tane göndermek mi istiyorsunuz? Kullanıma sunduğumuz **[şablon deposu](https://github.com/Microsoft/threat-modeling-templates)** daha fazla bilgi için GitHub sayfası                              |
| **Model açma**                            | <p>Açılır, tehdit modelleri daha önce kaydedildi. En son dosyaları açmak gerekirse harika son açılan modelleri özelliğidir. Seçimin üzerine geldiğinizde, modelleri açmak için 2 yolu bulabilirsiniz:</p><p><ul><li>Açık bu bilgisayardan-yerel depolamayı kullanarak bir dosyayı açma Klasik yolu</li><li>Açık Onedrive'dan –, kaydetme ve paylaşma üretkenliği ve işbirliğini artırmak için tek bir konumda, tüm tehdit modelleri için OneDrive klasörlerinde takımlar kullanabilir</li></ul></p> |
| **Başlangıç Kılavuzu**                   | Açılır **[Microsoft tehdit modelleme aracı](./azure-security-threat-modeling-tool.md)** ana sayfası                                                                                                                                                                                                                                                                                                                                                                                            |

### <a name="template-section"></a>Şablon bölümü

| Bileşen               | Ayrıntılar                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Yeni şablon oluşturma** | Boş bir şablon oluşturmak size açılır. Şablonları sıfırdan oluşturmanın kapsamlı bilgi yoksa, mevcut olanlardan oluşturmanızı öneririz |
| **Açık şablon**       | Değişiklik yapmak şablonlar varolan açar                                                                                                              |

Tehdit modelleme aracı ekibi, deneyimi ve araç işlevselliğini geliştirmek için sürekli çalışmaktadır. Birkaç küçük değişiklikler, yıl boyunca yer alabilir, ancak yeniden Kılavuzu'ndaki tüm önemli değişiklikler gerektirir. Genellikle, en yeni Duyurular alın emin olmak için başvurun.

## <a name="building-a-model"></a>Bir model oluşturma

Bu bölümde, biz izleyin:

- Cristina (Geliştirme)
- Ricardo (program Yöneticisi) ve
- Olan Ashish (test edici)

Bunlar, ilk tehdit modeli geliştirme sürecinde oluşturacaksınız.

> Ricardo: Merhaba Cristina miyim tehdit modeli diyagram üzerinde çalışan ve ayrıntıları sağ yapılandırdığımıza sana haber vermek istedik. Bana kuruluşumuzun da yardımcı olabilir?
> Cristina: Kesinlikle. Şimdi bir göz atalım.
> Ricardo aracı açılır ve Cristina ile kendi ekranını paylaşır.

![Temel bir tehdit modeli](./media/azure-security-threat-modeling-tool-feature-overview/basictmt.png)

> Cristina: Tamam, doğrudan görünür ancak, bana üzerinden inceleyebileceğiniz?
> Ricardo: Emin! Dökümü aşağıda verilmiştir:
> - İnsan kullanıcı Dış varlık olarak çizilir; bir kare
> - Web Sunucumuz komutları gönderiyorsanız — daire
> - Web sunucusu, bir veritabanı (paralel satırlarını) danışmanlığı

Hangi Ricardo Cristina yalnızca gösterdi VAD, kısaltması olduğundan  **[veri akış diyagramı](https://en.wikipedia.org/wiki/Data_flow_diagram)**. Tehdit modelleme aracı kırmızı noktalı farklı varlıklar denetiminde nerede göstermek için satırlar tarafından belirtilen güven sınırları belirtme olanağı sağlar. Örneğin, Active Directory denetimi dışında bu nedenle BT yöneticilerinin kimlik doğrulama amacıyla bir Active Directory sistemi gerektirir.

> Cristina: Benim için en uygun arar. Tehditler hakkında neler diyeceksiniz?
> Ricardo: Size göstermek istiyorum.

## <a name="analyzing-threats"></a>Tehditleri çözümleme

Adlandırılan SDL yaklaşım kullanan simgesi menü seçimini (kendisi için varsayılan şablonu temel alan bir listesi oluşturulan tehditleri tehdit modelleme aracı bulunamadı alınır dosyasıyla Büyüteç) analiz görünümünden tıklatır sonra  **[ (Kimlik sahtekarlığı, kurcalama, bilgi İfşası, Red, hizmet reddi ve ayrıcalıkların) STRIDE](https://en.wikipedia.org/wiki/STRIDE_(security))**. Yazılım tahmin edilebilir bir 6 kategorilerine kullanarak bulunan tehditleri kümesi altında geldiğini olur.

Bu yaklaşım, her kapı ve pencere sağlayarak evinizde güvenli hale getirme kilitleme mekanizması bir alarm sistemi ekleme veya arama sonra hırsız önce var gibidir.

![Temel tehditler](./media/azure-security-threat-modeling-tool-feature-overview/basicthreats.png)

Listedeki ilk öğeyi seçerek Ricardo başlar. Şunlar olur:

İlk olarak, iki şablonlar arasındaki etkileşimi geliştirilmiştir

![Etkileşimi](./media/azure-security-threat-modeling-tool-feature-overview/interaction.png)

Tehdit hakkında ikinci, ek bilgi tehdit Özellikler penceresinde görünür.

![Etkileşim bilgileri](./media/azure-security-threat-modeling-tool-feature-overview/interactioninfo.png)

Oluşturulan tehdit ona olası tasarım fabrikadan anlamanıza yardımcı olur. Ek açıklama ona tam olarak bunu azaltmak için olası yollarla birlikte nerede olduğunu söyler. sırada STRIDE kategori ona olası saldırı vektörlerinin hakkında bir fikir sunar. Gerekçe ayrıntıları notları yazmak veya kuruluşunun hata çubuğu bağlı olarak öncelik derecelendirmeleri değiştirmek için o düzenlenebilir alanları kullanabilirsiniz.

Azure şablonları, kullanıcıların yalnızca sorun nedir, aynı zamanda Azure özgü belgelere açıklamaları, örnekleri ve köprüler ekleyerek bu sorunun nasıl anlamalarına yardımcı olmak için ek ayrıntılar sahiptir.

Açıklama yapılan ona sahte kullanıcıların önlemek için bir kimlik doğrulama mekanizması ekleme önemini fark üzerinde çalışılması gereken ilk tehdit açıklayacak. Birkaç dakika ile Cristina, tartışma içine, uygulama erişimi ve rolleri önemini anladım. Ricardo bunlar uygulandığına emin olmak için bazı hızlı notları doldurulur.

Bilgilerin açığa çıkması altında tehditleri içine Ricardo oluştu gibi kendisine erişim denetimi planı denetim ve rapor oluşturma için gerekli bazı salt okunur hesapları gerçekleşmiş. He bu yeni tehdit olmalıdır, ancak kendisine uygun şekilde tehdit belirtildiği şekilde azaltmaları aynı olan olup olmadığını hiç merak ettiniz.
Kendisi ayrıca hakkında bilgilerin açığa çıkması biraz daha düşündük ve yedekleme bantlarının şifreleme, operasyon ekibinin iş gerek giderek gerçekleşmiş.

Tehditleri tasarımı mevcut bir risk azaltma işlemleri ya da güvenlik nedeniyle uygulanamaz durumu açılan listeden garanti "Uygulanamaz" değiştirilebilir. Üç seçeneğiniz vardır: Başlatılmamış – varsayılan seçimi, araştırma – gereken öğeleri ve Mitigated – tam olarak üzerinde çalıştığı sonra takip için kullanılır.

## <a name="reports--sharing"></a>Rapor & Paylaşımı

Ricardo Cristina listesiyle geçer ve önemli notlar, risk azaltma işlemleri/Gerekçeleri, öncelik ve durum değişikliklerini ekler. sonra he seçer raporlar oluşturma tam raporu iş arkadaşlarınızla geçtikleri kendisine için kullanışlı bir raporda dışarı yazdırır Kaydet rapor -> ->. uygun güvenlik iş emin olmak için uygulanır.

![Etkileşim bilgileri](./media/azure-security-threat-modeling-tool-feature-overview/report.png)

Ricardo dosyayı yerine paylaşmak isterse, kendisinin kolayca kuruluşunun OneDrive hesabınıza kaydederek bunu yapabilirsiniz. Gerçekleştirir Müşterinizle sonra o belge bağlantıya Kopyala ve arkadaşları ile paylaşma. 

## <a name="threat-modeling-meetings"></a>Tehdit modelleme toplantıları

Ricardo OneDrive, Ashish Sınayıcısı'nı kullanarak iş arkadaşı için kendi tehdit modeli gönderildiğinde, underwhelmed. Gibi kolayca tehlikeye girebilir oldukça önemli köşe durumlarda, atlanan Ricardo ve Cristina olduğu görülüyor. Bu stratejiyle tamamlayan bir tehdit modelleri ' dir.

Ashish tehdit modeli geçen sonra bu senaryoda, he iki tehdit modelleme toplantılar için çağrılır: işlemi eşitlemek ve diyagramları yol için bir toplantı sonra ikinci bir toplantı tehdit için gözden geçirin ve kabul.

İlk toplantıda Ashish 10 dakika işlem modelleme SDL tehdit üzerinden herkese walking ayırıyor. Kendisi bu tehdit modeli diyagram çekilir ve ayrıntılı olarak açıklayan başlatıldı. Beş dakika içinde önemli eksik bir bileşen tanımlanmış.

Birkaç dakika daha sonra Ashish ve Ricardo Web sunucusuna nasıl oluşturulmuş bir genişletilmiş tartışma alındı. İdeal bir şekilde devam etmek bir toplantı için değil, ancak bunları gelecekte zaman tutarsızlık erken keşfetme yüklemesiyle herkesin sonunda kabul.

İkinci toplantı, takım tehditlerine öğrendiniz, onları adreslemek için bazı yollar ele alınan ve tehdit modeli üzerinde arındırıldıktan. Bunlar belge kaynak denetimine iade ve geliştirme ile devam eder.

## <a name="thinking-about-assets"></a>Varlıkları hakkında düşünmeye

Modellenmiş tehdit sahip bazı okuyucuların varlıkları hakkında hiç konuştuk henüz olduğunu fark edebilirsiniz. Birçok yazılım mühendisleri yazılımlarını bunlar varlıklar kavramı anlamak ve hangi varlıkları bir saldırganın ilginizi çekebilir daha iyi anlamak keşfettiğinize göre.

Bir ev tehdit modeli kullanacaksanız, tarafından ailesi, yerine yenisi konulamayacak fotoğraf ya da değerli çizim hakkında düşünmeye başlayabilir. Belki de tarafından kimin bozulabilir ve geçerli güvenlik sistemi hakkında düşünmeye başlayabilir. Veya havuzu veya ön porch gibi fiziksel özelliklerini dikkate alarak başlayabilir. Bu varlıklar, saldırganlar veya yazılım tasarımı hakkında düşünmeye benzer. Bu üç yaklaşımlardan herhangi bir iş.

Biz Burada sunulan modelleme tehdit ne Microsoft geçmişte yapmış daha önemli ölçüde daha basit yaklaşımdır. Yazılım tasarımı yaklaşımı birçok takım için düzgün çalıştığını bulduk. Sizinki içeren umuyoruz.

## <a name="next-steps"></a>Sonraki Adımlar

Soru, yorum ve konuları tmtextsupport@microsoft.com. **[İndirme](https://aka.ms/threatmodelingtool)**  kullanmaya başlamak için tehdit modelleme aracı.
