---
title: Azure Güvenlik Merkezi'nde güvenlik yapılandırmalarını düzeltmek | Microsoft Docs
description: Bu belgede "Düzelt güvenlik yapılandırmalarını.", Azure Güvenlik Merkezi öneri uygulamak nasıl gösterir
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2018
ms.author: terrylan
ms.openlocfilehash: 3af8f211c19fde9d2fc79f41fc13009570a9b4de
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285925"
---
# <a name="remediate-security-configurations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik yapılandırmalarını Düzelt
Azure Güvenlik Merkezi günlük sanal makineleri (VM'ler) ve sanal makineleri hale getirebilecek bir yapılandırma için ve bilgisayarları daha savunmasız işletim sistemi (OS) çözümler. Güvenlik Merkezi, işletim sistemi yapılandırması önerilen güvenlik yapılandırma kuralları eşleşmiyor ve bu güvenlik açıklarına değinen yapılandırma değişiklikleri önerir güvenlik açıkları gidermek önerir.

İzlenmekte olan belirli yapılandırmalar hakkında daha fazla bilgi için bkz: [önerilen yapılandırma kuralları listesi](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Güvenlik Yapılandırma değerlendirmeleri özelleştirmek öğrenmek için bkz: [işletim sistemi özelleştirme (Önizleme) Azure Güvenlik Merkezi'nde güvenlik yapılandırmalarını](security-center-customize-os-security-config.md).

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
"Güvenlik yapılandırmalarını Düzelt" Güvenlik Merkezi'nde bir öneri olarak sunulur. Önerinin altında görüntülenen **önerileri** > **işlem**.

Bu örnekte "güvenlik yapılandırmalarını Düzelt" önerinin altında kapsayan **işlem**.
1. Sol bölmede, Güvenlik Merkezi'nde seçin **işlem**.  
  **İşlem** penceresi açılır.

   ![Güvenlik yapılandırmalarını düzeltme][1]

2. Seçin **güvenlik yapılandırmalarını düzeltmek**.  
  **Güvenlik yapılandırmalarını** penceresi açılır.

   !["Güvenlik yapılandırmalarını" penceresi][2]

  Pano görüntüler üst kısmında:

  - **Önem derecesine göre kuralları başarısız**: işletim sistemi yapılandırması VM'ler ve önem derecesine göre bölünen bilgisayarlar arasında başarısız kuralları toplam sayısı.
  - **Türe göre kuralları başarısız**: işletim sistemi yapılandırması VM'ler ve türe göre bölünen bilgisayarlar arasında başarısız kuralları toplam sayısı.
  - **Windows kuralları başarısız**: Windows işletim sistemi yapılandırmalarınızı başarısız kuralları toplam sayısı.
  - **Linux kuralları başarısız**: başarısız, Linux işletim sistemi yapılandırmalarını tarafından kuralları toplam sayısı.

  Pano alt bölümünü Vm'leriniz ve bilgisayarlar ve eksik güncelleştirmenin önem derecesi için tüm başarısız kuralları listeler. Liste aşağıdaki öğeleri içerir:

  - **CCEID**: CCE kuralı için benzersiz tanımlayıcı. Güvenlik Merkezi, benzersiz tanımlayıcıları için yapılandırma kuralları atamak için Common Configuration Enumeration (CCE) kullanır.
  - **Ad**: başarısız kuralının adı.
  - **Kural türü**: *kayıt defteri anahtarı*, *Güvenlik İlkesi*, *Denetim İlkesi*, veya *IIS* kural türü.
  - **Hayır. sanal makineleri & bilgisayarları**: VM'ler ve başarısız kuralın uygulanacağı bilgisayarların toplam sayısı.
  - **Kural önem**: CCE değeri *kritik*, *önemli*, veya *uyarı*.
  - **Durum**: önerinin geçerli durumu:

    - **Açık**: Öneri henüz ele alınmadı.
    - **Devam eden**: öneri şu anda kaynaklara uygulanıyor ve herhangi bir işlem yapmanız gerekmez.
    - **Çözümlenen**: öneri uygulanır. Sorun çözüldüğünde, giriş soluk görüntülenir.

3. Başarısız bir kural ayrıntılarını görüntülemek için listeden seçin.

   ![Ayrıntılı görünümünü başarısız yapılandırma kuralı][3]

   Ayrıntılı görünümü aşağıdaki bilgileri görüntüler:

   - **Ad**: kuralının adı.
   - **CCIED**: CCE kuralı için benzersiz tanımlayıcı.
   - **İşletim sistemi sürümü**: VM veya bilgisayar işletim sistemi sürümü.
   - **Kural önem**: CCE değeri *kritik*, *önemli*, veya *uyarı*.
   - **Tam açıklama**: Kural açıklaması.
   - **Güvenlik Açığı**: güvenlik açığı veya kural değil uygulanırsa risk açıklaması.
   - **Olası etkisini**: kuralı uygulandığında iş etkisi.
   - **Karşı önlem**: düzeltme adımları.
   - **Beklenen değer**: Güvenlik Merkezi kural karşı VM OS yapılandırmanızı inceler, beklenen değer.
   - **Gerçek değer**: VM OS yapılandırmanızı kural karşı analizini sonra döndürülen değer.
   - **Kural işlemi**: Güvenlik Merkezi tarafından VM OS yapılandırmanızı kural karşı Çözümleme sırasında kullanılan kural işlemi.

4. Ayrıntılı Görünüm penceresinin en üstünde seçin **arama**.  
  Arama VM'ler ve seçili güvenlik yapılandırmalarını uyumsuzluğu bilgisayarlarla sahip çalışma alanları listesi açılır. Çalışma alanı seçimi yalnızca seçilen kuralı farklı çalışma alanlarına bağlanan birden çok VM için geçerli olup olmadığını gösterilir.

   ![Listelenen çalışma alanları][4]

5. Bir çalışma alanı seçin.  
  Günlük analizi arama sorgusu güvenlik yapılandırmalarını uyumsuzluğu ile çalışma alanına filtrelenmiş açar.

   ![İşletim sistemi güvenlik açığı ile çalışma][5]

6. Listeden bir bilgisayar seçin.  
  Yalnızca bu bilgisayar için filtre bilgi içeren yeni bir arama sonucu açar.

   ![Seçilen bilgisayarla ilgili ayrıntılı bilgiler][6]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede "Düzelt güvenlik yapılandırmalarını." Güvenlik Merkezi öneriyi uygulamayı nasıl oluşturulacağını gösterir Güvenlik Yapılandırma değerlendirmeleri özelleştirmek öğrenmek için bkz: [işletim sistemi özelleştirme (Önizleme) Azure Güvenlik Merkezi'nde güvenlik yapılandırmalarını](security-center-customize-os-security-config.md).

İzlenmekte olan belirli yapılandırmalar gözden geçirmek için bkz: [önerilen yapılandırma kuralları listesi](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Güvenlik Merkezi, benzersiz tanımlayıcıları için yapılandırma kuralları atamak için Common Configuration Enumeration (CCE) kullanır. Daha fazla bilgi için Git [CCE](https://nvd.nist.gov/cce/index.cfm) site.

Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* Desteklenen Windows ve Linux VM'ler listesi için bkz: [desteklenen platformlar Azure Güvenlik Merkezi'nde](security-center-os-coverage.md).
* Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerinin nasıl yapılandırılacağını öğrenmek için bkz: [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md).
* Önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı bilgi edinmek için [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md).
* Azure kaynaklarınızı sağlığını izlemek öğrenmek için bkz: [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md).
* Yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinmek için [yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md).
* İş ortağı çözümlerinizin sistem durumunu öğrenmek için bkz: [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md).
* Hizmet kullanma hakkında sık sorulan soruların yanıtları için bkz: [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md).
* Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını için bkz: [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/).

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png
