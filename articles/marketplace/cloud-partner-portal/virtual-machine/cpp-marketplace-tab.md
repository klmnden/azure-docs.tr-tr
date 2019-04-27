---
title: Azure için bulut iş ortağı portalı sanal makine Marketi sekmesinden | Microsoft Docs
description: Bir Azure Market VM teklifi oluşturmak için kullanılan Marketi sekmesinden açıklar.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: b1b62c68ef4e18f4d4d36a78078ad7431717b754
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60332780"
---
# <a name="virtual-machine-marketplace-tab"></a>Sanal makine Marketi sekmesi

**Market** sekmesinde **yeni teklif** sayfası, pazarlama, satış ve yasal bilgileri ve sözleşmeleri, müşteri adaylarınıza sağlamak ve oluşturulan müşteri adayları yönetmenize olanak tanır Market. Bu uzun biçimi dört bölümlere ayrılmıştır: **Genel Bakış**, **Yapıtları pazarlama**, **sağlama Yönetim**, ve **yasal**. 

## <a name="overview-section"></a>Genel Bakış bölümünde
Bu bölümde, Azure Market teklifi hakkında genel bilgileri girin.  Alan adı eklenmiş bir yıldız (*) gerekli olduğunu gösterir.

![Genel Bakış bölümünde sanal makineler için yeni teklifi formdaki Market sekmenin](./media/publishvm_008.png)

Aşağıdaki tabloda, amacı ve bu alanların içeriğini açıklar.

|  **Alan**                |     **Açıklama**                                                          |
|  ---------                |     ---------------                                                          |
| **Başlık**                 | Teklif, genellikle uzun, biçimsel adı başlığı. Bu konu başlığı, Market'te göze çarpacak şekilde görüntülenir.  En fazla 50 karakter uzunluğunda. |
| **Özet**               | Brief amaçlı veya çözümün işlevi.  En fazla 100 karakter uzunluğunda. |
| **Uzun özeti**          | Amaç veya çözümün işlevi.  En fazla 256 karakter uzunluğunda. |
| **Açıklama**           | Çözüm açıklaması.  En fazla 3000 karakter uzunluğunda basit HTML biçimlendirmeyi destekler. |
| **Microsoft CSP satıcısı kanal** | Bulut çözümü sağlayıcıları (CSP) iş ortağı kanalı katılımı kullanıma sunuldu.  Lütfen [bulut çözüm sağlayıcıları](../../cloud-solution-providers.md) teklifinizi Microsoft CSP aracılığıyla pazarlama hakkında daha fazla bilgi için iş ortağı kanalı. |
| **Pazarlama tanımlayıcısı**  | Bu teklif için ilişkilendirmek için benzersiz bir URL, kuruluşunuz ve çözüm adı, en fazla uzunluğu 50 karakterden genellikle içerir.  Örneğin: <br/> `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp`  |
| **Önizleme abonelik kimlikleri** | Bir ila 100 abonelik tanımlayıcıları nizleyicileri ekleyin. Canlı geçmeden önce yayımlandıktan sonra bu teknik listelenen abonelikler teklif erişebilir. |
| **Faydalı bağlantılar**          | URL'leri, belgeler, sürüm notları, SSS ve benzeri ekleyin. |
| **Önerilen kategorileri (en fazla 5)** | İş ve teknik teklif kategorilerini çoklu seçim ile ilişkili en iyi olabilir.  En fazla beş izin verilir.  |
|  |  |


## <a name="marketing-artifacts-section"></a>Pazarlama Yapıtları bölümü

Bu ikinci bölümde üç alt bölümlere ayrılmıştır: **Logo**, **ekran**, ve **videoları**. Logo yalnızca tüm için en iyi müşteri ilgi çekecek şiddetle tavsiye edilir ancak yapıtlar, pazarlama gereklidir.

![Sanal makineler için yeni teklifi formdaki Market sekmesinin Yapıtları bölümündeki pazarlama](./media/publishvm_009.png)

|  **Alan**                |     **Açıklama**                                                          |
|  ---------                |     ---------------                                                          |
| *Logo*  |  |
| **Küçük**                 | 40 x 40 piksel .ico bit eşlem                                                      |
| **Orta**                | 90 x 90 piksel .ico bit eşlem                                                      |
| **Büyük**                 | 115 x 115 piksel .ico bit eşlem                                                   |
| **Geniş**                  | 255 x 115 piksel .ico bit eşlem                                                    |
| **Hero**                  | 815 x 290 bit eşlem.  İsteğe bağlı, ancak bir kez karşıya yüklenen hero simge silinemiyor. |
| *Ekran görüntüleri*  | İsteğe bağlı, ancak en fazla beş ekran görüntüleri başına SKU. |
| **Ad**                  | Adı veya başlığı <!-- TODO - max char length? none specified in UI -->                               |
| **Görüntü**                 | Ekran yakalama görüntüsünü, 533 x 324 piksel                                         |
| *Videolar*  |  |
| **Ad**                  | Adı veya başlığı  <!-- TODO - max char length? -->                              |
| **Bağlantı**                  | YouTube veya Vimeo barındırılan video URL'si                                        |
| **Küçük resim**             | 533 x 324 bit eşlem                                                               |
|  |  |


### <a name="logo-guidelines"></a>Logo yönergeleri

<!-- TD: It seems like this section could be better located in some common area, maybe an AMP Marketing/Design section 
+1 this should all be in a common area and referenced from here to that location.-->

Bulut iş ortağı portalına karşıya logo yönergeleri izlemelidir:

*  Azure tasarımının basit bir renk paleti vardır. Logonuzu düşük tutmak için ikincil renk ve birincil sayısı.
*  Azure portal'ın Tema renkleri beyaz ve siyah. Bu nedenle bu renkler, logolar arka plan rengi kullanmaktan kaçının. Logo, Azure portalında belirgin hale getirir bazı renk kullanın. Basit birincil renkleri öneririz. Saydam arka plan kullanıyorsanız, logolar/metin olmadığından beyaz veya mavi veya siyah emin olun.
*  Arka plan gradyan logonuzu kullanmayın.
*  Metin yerleştirmekten kaçının — bile, şirketiniz veya marka adı — logosu üzerinde. Logonuzu Görünüm ve yapısını "Düz" olmalıdır ve gradyanları kaçınmalıdır.
*  Logo esnetme değil.

#### <a name="hero-logo"></a>Kahraman logosu

Hero logosu isteğe bağlıdır; Ancak, karşıya sonra hero simge silinemiyor.  Hero logosu simgesi yönergeleri izlemelidir:

*  Siyah, beyaz ve saydam arka planlar için hero simgeler izin verilmez.
*  Herhangi bir ışık rengi hero simge arka planı olarak kullanmaktan kaçının.  Yayımcı görünen adı, planı başlık ve uzun Özet teklif beyaz renkte görüntülenir ve arka plan karşı göze gerekir.
*  Hero logosu tasarlarken çoğu metin kullanmaktan kaçının.  Teklif listeleri Yayımcı adı, planı başlık, uzun Özet teklif ve Oluştur düğmesine program aracılığıyla hero simgesi gömülür. 
* Kullanılmayan bir dikdörtgen boyutu 415 x 100 piksel, hero simgesini sağ tarafında içerir ve 370 uzaklık piksel soldan.  

Örneğin, Azure Container Service için aşağıdaki hero simgesi bulunuyor.  <!-- TD: It would be nice to have the raw bitmap, e.g.before and after embedding. -->

![Azure Container Service için örnek hero simgesi](./media/publishvm_010.png)


### <a name="marketing-information-example"></a>Pazarlama bilgileri örneği 

Aşağıdaki görüntüde, bilgi pazarlama Microsoft Windows Server ana ürün sayfada nasıl görüntüleneceğini gösterir.

![Microsoft Windows Server için örnek ürün sayfası](./media/publishvm_011.png)


## <a name="lead-management-section"></a>Yönetim bölümünde sağlama
<!-- this all should be referenced in a common location for lead management, not in this file. nothing unique for a vm specifically. -->

Üçüncü bölüm müşteriler toplamanızı sağlayan Azure Marketi Teklifleriniz oluşturulan müşteri adayları. Bu müşteri adayı bilgileri için aşağıdaki depolama seçenekleri (aşağı açılan listeden) sunar.

* **Hiçbiri** -varsayılan olarak, müşteri adayı bilgileri alınamadı.
* Azure tablo - Azure tablosuna bir bağlantı dizesi tarafından belirtilen.
* Dynamics CRM Online - yazılan [Microsoft Dynamics 365 Online](https://dynamics.microsoft.com/) örneği, belirtilen URL'yi ve kimlik doğrulaması bir kimlik bilgileri.
* HTTPS uç - belirtilen HTTPS uç noktası bir JSON yükü olarak yazılır.
* Marketo - belirtilen yazılan [Marketo](https://www.marketo.com/) sunucu kimliği, munchkin kimliği ve form kimliği tarafından belirtilen örnek,
* Salesforce - yazılan bir [Salesforce](https://www.salesforce.com/) veritabanı, bir nesne tanımlayıcısı belirtildi.

Teklifinizi başarıyla yayımladıktan sonra müşteri adayı bağlantı doğrulanır ve bir test müşteri adayı için yapılandırılan hedef otomatik olarak gönderilir. Müşteri adayı bilgilerini sürekli olarak yönetilmesi gerektiğini ve müşteri yönetimi Mimarinizi değişiklik yapıldığında bu ayarları en kısa sürede güncelleştirildi.

<!-- TD: For more info, see [Need a topic on lead information and processing that mimics the Appendix of the VM Pub Guide]. -->

## <a name="legal-section"></a>Yasal bölümü

Bu son bölümde her teklif için gereken iki yasal belgeler girmenize olanak tanır: Gizlilik İlkesi ve kullanım koşulları.

|  **Alan**                |     **Açıklama**                                                          |
|  ---------                |     ---------------                                                          |
| **Gizlilik İlkesi URL'si**    | Gizlilik ilkeniz URL'si                                            |
| **Kullanım koşulları**          | İlke olarak basit bir HTML veya düz metin.  <!-- TODO - max char length? -->       |
|  |  |

<br/>

Sonraki [Destek](./cpp-support-tab.md) sekmesi, teklifiniz için teknik ve kullanıcı Destek kaynakları sağlayacaktır.

