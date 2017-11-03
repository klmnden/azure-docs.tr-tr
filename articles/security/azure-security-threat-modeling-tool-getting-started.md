---
title: "-Microsoft tehdit modelleme aracı - Azure Başlarken | Microsoft Docs"
description: "Bu eylem tehdit modelleme aracı vurgulama daha derin bir genel bakıştır."
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 2d940b42108948f4cd36a585f1e79def05fe8fd3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-the-threat-modeling-tool"></a>Tehdit modelleme aracı ile çalışmaya başlama

Bulut ve kurumsal güvenlik araçları takım tehdit modelleme aracı Önizleme bu yıl ücretsiz olarak yayımlanan  **[tıklatın indirme](https://aka.ms/tmtpreview)**. Teslim mekanizması değişikliği en son geliştirmeleri ve hata düzeltmeleri müşterilere Bakımı ve kullanımı kolay duruma aracı, her açışlarında itme olanak tanır.
Bu makalede yaklaşım modelleme Microsoft SDL tehdidin Başlarken sürecinde alır ve aracının güvenlik işleminin omurga olarak harika bir tehdit modeli geliştirmek için nasıl kullanılacağı gösterilmektedir.

Bu makalede, mevcut bir yaklaşım modelleme SDL tehdit bilgilerini üzerinde oluşturur. Hızlı bir gözden geçirme için başvurmak  **[tehdit modelleme Web uygulamaları](https://msdn.microsoft.com/library/ms978516.aspx)**  ve arşivlenen bir sürümünü  **[ortaya çıkarmaya güvenlik açıkları kullanarak STRIDE yaklaşım](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy)**  MSDN makalesine 2006'yayımladı.

Hızlı bir şekilde özetlemek için bir diyagram oluşturma, tehditleri tanımlamak, bunları Azaltıcı ve her azaltma doğrulama yaklaşım içerir. Bu işlem vurgular bir diyagram şöyledir:

![SDL işlemi](./media/azure-security-threat-modeling-tool/sdlapproach.png)

## <a name="starting-the-threat-modeling-process"></a>İşlem modelleme tehdit başlatılıyor

Tehdit modelleme Aracı'nı başlatın, aşağıdaki resimde görüldüğü gibi birkaç şeyi görürsünüz:

![Boş başlangıç sayfası](./media/azure-security-threat-modeling-tool/tmtstart.png)

### <a name="threat-model-section"></a>Tehdit modeli bölümü

| Bileşen                                   | Ayrıntılar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Geri bildirim, öneriler ve sorunları düğmesi** | Sizi götürür  **[MSDN Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=sdlprocess)**  her şey SDL için. Diğer kullanıcıların, geçici çözümler ve öneriler birlikte gerçekleştirdiği aracılığıyla okumak için bir fırsat sunar. Aradığınızı hala bulamıyorsanız, e-posta tmtextsupport@microsoft.com yardımcı olması destek ekibimiz için                                                                                                                            |
| **Bir Model oluşturma**                          | Size, diyagram çizmek boş bir tuvalde açar. Modeliniz için kullanmak istediğiniz şablonun seçtiğinizden emin olun                                                                                                                                                                                                                                                                                                                                                                       |
| **Yeni modelleri için şablonu**                 | Bir modeli oluşturmadan önce kullanılacak şablonunu seçmelisiniz. Azure özel şablonlar, tehditleri ve bunları azaltmanın yollarını içeren Azure tehdit modeli şablonu bizim ana şablonudur. Genel modellerini SDL TM Bilgi Bankası aşağı açılan menüsünden seçin. Kendi şablonunuzu oluşturun veya tüm kullanıcılar için yeni bir tane göndermek istiyorsunuz? Kullanıma bizim  **[şablonu deposu](https://github.com/Microsoft/threat-modeling-templates)**  daha fazla bilgi için GitHub sayfası                              |
| **Bir Model açın**                            | <p>Açılır tehdit modelleri önceden kaydedilmiş. En son dosyalarınızı açmanız gerekirse harika son açılan modelleri özelliğidir. Seçimin üzerine geldiğinizde, modelleri açma 2 yolları bulabilirsiniz:</p><p><ul><li>Açık bu bilgisayardan – yerel depolama kullanarak bir dosyayı açma Klasik yolu</li><li>Açık OneDrive üzerinden –, kaydedin ve tüm tehdit modellerini üretkenliği ve işbirliği artırmaya yardımcı olmak için tek bir konumda paylaşmak için OneDrive klasörlerde takımlar kullanabilirsiniz</li></ul></p> |
| **Başlarken Kılavuzu**                   | Açılır  **[Microsoft tehdit modelleme aracı](./azure-security-threat-modeling-tool.md)**  ana sayfası                                                                                                                                                                                                                                                                                                                                                                                            |

### <a name="template-section"></a>Şablon bölümü

| Bileşen               | Ayrıntılar                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Yeni şablon oluşturun** | Oluşturmanızı boş bir şablon açar. Sıfırdan şablonları oluşturmanın kapsamlı bilgi olmadığı sürece, mevcut olanlardan oluşturmanızı öneririz |
| **Şablonunu Aç**       | Şablonlar, değişiklikler yapmak var olan açar                                                                                                              |

Tehdit modelleme araç takımı, aracı işlevselliğini geliştirmek ve denemek için sürekli çalışmaktadır. Birkaç küçük değişiklikler yıl boyunca gerçekleşmesi, ancak yeniden yazdırmaya Kılavuzu'ndaki tüm önemli değişiklikler gerektirir. Genellikle son Duyurular alma emin olmak için bakın.

## <a name="building-a-model"></a>Bir model oluşturma

Bu bölümde, biz izleyin:

- Cristina (Geliştirici)
- Attila (program Yöneticisi) ve
- (Bir tester) olan Ashish

Bunlar ilk kendi tehdit modeli geliştirmek sürecinde adımıdır.

> Attila: Merhaba Cristina, tehdit modeli diyagramı çalışılan ve emin olmak için istedik biz ayrıntıları sağ aldı. Beni Ara onu yardımcı olabilir?
> Cristina: kesinlikle. Bir göz atalım.
> Attila aracı açılır ve kendi ekran Cristina ile paylaşır.

![Temel tehdit modeli](./media/azure-security-threat-modeling-tool/basictmt.png)

> Cristina: Tamam, sonucunun görünüyor, ancak, bana üzerinden yol?
> Attila: emin! Çözümleme şöyledir:
> - Bizim insan kullanıcı dışında bir varlık olarak çizilir — kare
> - Bizim Web sunucusuna komutları gönderiyorsanız — daire
> - Web sunucusu (iki paralel satırları) veritabanı danışmanlık

Ne Attila Cristina yalnızca gösterdi DFD, kısaltması olan  **[veri akış diyagramı](https://en.wikipedia.org/wiki/Data_flow_diagram)**. Tehdit modelleme aracı kullanıcıların kırmızı noktalı farklı varlıkları denetiminde nerede göstermek için satırlar tarafından belirtilen güven sınırları belirtmesine izin verir. Örneğin, Active Directory kendi denetimi dışında olacak şekilde BT yöneticilerinin kimlik doğrulama amacıyla bir Active Directory sistemi gerektirir.

> Cristina: benim için en uygun arar. Tehditler hakkında neler?
> Attila: bana, Göster olanak tanır.

## <a name="analyzing-threats"></a>Tehditler analiz etme

Adlı SDL yaklaşım kullanan simgesi menü seçimi (varsayılan şablona göre oluşturulan tehditleri tehdit modelleme bulunan aracı listesine alınır kendisinin dosyasıyla Büyüteç), analiz görünümünden üzerinde tıklatır sonra  **[ STRIDE (, oynama, bilgi açıklama, hizmet reddi ve ayrıcalıkların kimlik sahtekarlığı)](https://en.wikipedia.org/wiki/STRIDE_(security))**. Yazılım bu 6 kategorileri kullanarak bulunabilir tehditleri tahmin edilebilir bir kümesi altında geldiğini olur.

Her kapı ve pencere sağlayarak evinizde güvenliğini sağlama kilitleme mekanizması bir alarm sistemi ekleme ya da sonra hırsız birleştirme önce olduğu gibi bu yaklaşımdır.

![Temel tehditleri](./media/azure-security-threat-modeling-tool/basicthreats.png)

Listesindeki ilk öğeyi seçerek Attila başlar. Şunlar olur:

İlk olarak, iki şablonlar arasındaki etkileşimi geliştirilmiştir

![Etkileşim](./media/azure-security-threat-modeling-tool/interaction.png)

İkinci, ek bilgiler tehdit hakkında tehdit Özellikleri penceresinde görüntülenir

![Etkileşim bilgisi](./media/azure-security-threat-modeling-tool/interactioninfo.png)

Oluşturulan tehdit ona olası tasarım açıkları anlamanıza yardımcı olur. Ek açıklama onu tam olarak ne yanı sıra olası yolları bunu azaltmak için yanlış olduğunu söyler sırada STRIDE kategori ona olası saldırı vektörlerinin hakkında bir fikir verir. Düzeltme ayrıntıları notları yazmak veya öncelik kuruluşunun hata çubuğu bağlı olarak derecelendirmeleri değiştirmek için kendisine düzenlenebilir alanları kullanabilirsiniz.

Azure şablonları yalnızca sorunun ne olduğunu, ancak ayrıca Azure özgü belgeleri açıklamaları, örnekler ve köprüleri ekleyerek gidermeye yönelik anlamalarına yardımcı olmak için ek ayrıntılar vardır.

Açıklama yapılan ona sahte kullanıcıların önlemek için bir kimlik doğrulama mekanizması ekleme önemini fark üzerinde çalışılacak ilk tehdit ortaya. Birkaç dakika ile Cristina, tartışma içine bunlar erişim denetimi ve rolleri uygulama önemini anladım. Attila bu uygulanan emin olmak için bazı hızlı notlar doldurulur.

Bilgilerin açığa çıkmasına altında tehditleri içine Attila oluştu olarak kendisine erişim denetimi planı bazı salt okunur hesapları denetleme ve rapor oluşturma için gerekli gerçekleşmiş. Ki bu yeni bir tehdit olmalıdır, ancak kendisine tehdit uygun şekilde belirtildiği şekilde Azaltıcı aynı olan olup olmadığını hiç merak ettiniz.
Kendisi ayrıca bilgilerin açığa çıkmasına biraz daha zorlayıcı ve yedekleme bantlarını şifrelemesi, işletim ekibi için bir işi gerek giderek gerçekleşmiş.

Tehditler tasarım varolan Azaltıcı Etkenler ya da güvenlik nedeniyle geçerli değil durumu açılan listeden garanti "İçin geçerli değil" olarak değiştirilebilir. Diğer üç seçenek vardır: başlamadı – varsayılan seçim, araştırma – gerekiyor tam olarak üzerinde çalıştığı bir kez öğeleri ve Mitigated – takip etmek için kullanılır.

## <a name="reports--sharing"></a>Raporlar ve paylaşma

Attila Cristina listesiyle geçer ve önemli notlar, Azaltıcı Etkenler/justifications, öncelik ve durum değişikliklerini ekler sonra he seçer raporlar tam raporu -> arkadaşlarınızla geçtikleri kendisine için iyi bir raporu yazdırır Kaydet Rapor Oluştur ->. uygun güvenlik çalışmasını sağlamak için uygulanır.

![Etkileşim bilgisi](./media/azure-security-threat-modeling-tool/report.png)

Bunun yerine dosya paylaşımı Attila istiyorsa, kendisinin kolayca kuruluşunun OneDrive hesabınıza kaydederek bunu yapabilirsiniz. Kendisine yapan sonra kendisi belgeyi bağlantıyı Kopyala ve kendi iş arkadaşlarınızla paylaşın. 

## <a name="threat-modeling-meetings"></a>Tehdit modelleme toplantılar

Attila OneDrive, Ashish, Sınayıcısı'nı kullanarak kendi iş arkadaşı kendi tehdit modeli gönderildiğinde underwhelmed. Atlanan kolayca tehlikeye girebilir oldukça birkaç önemli köşe durumlarda Attila ve Cristina gibi seemed. Kendi skepticism tehdit modelleri tamamlayan ' dir.

Tehdit modeli Ashish sürdü sonra bu senaryoda, kendisinin iki tehdit modelleme toplantılar için çağrılır: üzerinde işlem eşitlemek ve diyagramları yol için bir toplantı ve tehdit için ikinci bir toplantı gözden geçirin ve oturumu kapatma.

İlk toplantıya Ashish herkes işlem modelleme SDL tehdit aracılığıyla taramasını 10 dakika harcanan. Kendisi bu tehdit modeli diyagram çekilen ve ayrıntılı olarak açıklayan başlatıldı. Beş dakika içinde önemli bir bileşen eksik tanımlanmış.

Birkaç dakika daha sonra Ashish ve Attila Web sunucusuna nasıl oluşturulmuş bir genişletilmiş tartışma aldı. Devam etmek bir toplantı için ideal yöntem değil, ancak herkes sonunda bunları gelecekte zaman tutarsızlık erken keşfetme yüklemesiyle kabul ediyorum.

İkinci toplantı takım tehditleri gitti, bunları ele almak için bazı yollar ele alınan ve tehdit model üzerinde imzalı devre dışı. Bunlar, belge kaynak denetimine iade ve geliştirmeye devam etti.

## <a name="thinking-about-assets"></a>Varlıkları hakkında düşünmeye

Modellenen tehdit olan bazı okuyucular, biz varlıkları hakkında bilgileri hiç açıklandı henüz olduğunu fark edebilirsiniz. Biz, birçok yazılım mühendisleri yazılımlarını varlıklar kavramı anlamaları ve hangi varlıkları saldırgan ilginizi çekebilir çok daha iyi anlamak keşfettiniz.

Bir ev tehdit modeli için kullanacaksanız, aile, yerine yenisi konulamayacak fotoğraf ya da değerli çizim hakkında düşünerek başlayabilir. Belki de göz önünde bulundurulması kimin kesilebilir ve geçerli bir güvenlik sistemi tarafından başlayabilir. Veya havuza veya ön porch gibi fiziksel özellikleri dikkate alarak başlayabilir. Bunlar, varlıklar, saldırganlar veya yazılım tasarımı düşünmek için benzer. Bu üç yaklaşımlardan birini çalışır.

İş parçacığı biz Burada sunulan modelleme ne Microsoft geçmişte daha önemli ölçüde daha basit yaklaşımdır. Yazılım tasarım yaklaşımı birçok ekipler için iyi çalışır bulduk. Sizinki içeren umuyoruz.

## <a name="next-steps"></a>Sonraki Adımlar

Sorularınızı, açıklamalar ve sorunları için Gönder tmtextsupport@microsoft.com. **[Karşıdan](https://aka.ms/tmtpreview)**  başlamak için tehdit modelleme aracı.