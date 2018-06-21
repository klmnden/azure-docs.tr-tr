---
title: Azure CDN içeriğini ülkeye göre kısıtla | Microsoft Docs
description: Coğrafi filtreleme özelliğini kullanarak Azure CDN içeriğinizi ülkeye göre erişimi kısıtlamak öğrenin.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: v-deasim
ms.openlocfilehash: 661356aeb2369bc1bbddd6caee57b256dd9e1212
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285024"
---
# <a name="restrict-azure-cdn-content-by-country"></a>Azure CDN içeriğini ülkeye göre kısıtla

## <a name="overview"></a>Genel Bakış
Kullanıcı varsayılan olarak, içerik istediğinde, içerik isteği yapan kullanıcı konumundan bağımsız olarak sunulur. Ancak, bazı durumlarda, içeriğinizi ülkeye göre erişimi sınırlamak isteyebilirsiniz. İle *coğrafi filtreleme* özelliği, oluşturabileceğiniz kuralları belirli yollarında izin vermeyi veya engellemeyi içerik seçili ülkede CDN uç noktanız üzerinde.

> [!IMPORTANT]
> **Azure CDN standart Microsoft** profilleri yol tabanlı coğrafi filtreleme desteklemez.
> 

## <a name="standard-profiles"></a>Standart profiller
Bu bölümdeki yordamlar içindir **akamai'den Azure CDN standart** ve **verizon'dan Azure CDN standart** yalnızca profilleri. 

İçin **verizon'dan Azure CDN Premium** profilleri kullanmalıdır **Yönet** coğrafi filtrelemeyi etkinleştirmek için portal. Daha fazla bilgi için bkz: [Azure CDN Premium Verizon profillerinden](#azure-cdn-premium-from-verizon-profiles).

### <a name="define-the-directory-path"></a>Dizin yolu tanımlayın
Coğrafi filtreleme özelliği erişmek için portal dahilinde CDN uç noktanız seçin, sonra seçin **coğrafi filtreleme** sol taraftaki menüyü ayarları altında. 

![Standart coğrafi filtreleme](./media/cdn-filtering/cdn-geo-filtering-standard.png)

Gelen **yolu** kutusunda, konumu için kullanıcılara izin verilen ya da erişim reddedildi göreli yolunu belirtin. 

Bir iletme ile tüm dosyalarınızı eğik çizgi (/) veya dizin yollarını belirterek belirli klasörleri seçin coğrafi filtreleme uygulayabilirsiniz (örneğin, */pictures/*). Ayrıca coğrafi filtreleme tek bir dosyaya uygulayabilirsiniz (örneğin */pictures/city.png*). Birden çok kurala izin verilir; bir kural girdikten sonra sonraki kural girmek boş bir satır görüntülenir.

Örneğin, aşağıdaki dizin yolu filtreleri geçerli değildir:   
*/*                                 
*/Photos/*     
*/Photos/Strasbourg /*     
*/Photos/Strasbourg/City.PNG*

### <a name="define-the-type-of-action"></a>Eylemin türünü tanımlayın

Gelen **eylem** listesinde **izin** veya **blok**: 

- **İzin**: yalnızca belirtilen ülkelerinden kullanıcılara özyinelemeli yolundan istenen varlıklara erişimi verilir.

- **Blok**: Belirtilen ülkelerin kullanıcılardan özyinelemeli yolundan istenen varlıklara erişim engellenir. Bu konumda hiçbir diğer ülke filtreleme seçenekleri yapılandırıldıysa diğer tüm kullanıcıların erişim verilmez.

Yolun engellemede Örneğin, bir coğrafi filtreleme kuralı */fotoğraflar/Strasbourg/* aşağıdaki dosyaları filtreler:     
*http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg*
*http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg*

### <a name="define-the-countries"></a>Ülkelerin tanımlayın
Gelen **ülke KODLARINA** listesinde, engellemek veya yolu için izin vermek istediğiniz ülkelerin seçin. 

Ülkelerin seçerek bitirdikten sonra seçin **kaydetmek** yeni coğrafi filtreleme Kuralı etkinleştirmek için. 

![Coğrafi filtreleme kuralları](./media/cdn-filtering/cdn-geo-filtering-rules.png)

### <a name="clean-up-resources"></a>Kaynakları temizleme
Kural silmek için listeden seçin **coğrafi filtreleme** sayfasında sonra seçin **silmek**.

## <a name="azure-cdn-premium-from-verizon-profiles"></a>Azure CDN Premium Verizon profillerinden
İçin **verizon'dan Azure CDN Premium için** profiller, kullanıcı arabirimi farklı coğrafi filtreleme kuralı oluşturmak için:

1. Azure CDN profilinizi üst menüden seçin **Yönet**.

2. Verizon Portalı'ndan seçin **HTTP büyük**seçeneğini belirleyip **ülke filtreleme**.

    ![Standart coğrafi filtreleme](./media/cdn-filtering/cdn-geo-filtering-premium.png)

3. Seçin **ülke filtre eklemek**.

    **Adım bir:** sayfası görüntülenir.

4. Dizin yolu girin, seçin **blok** veya **Ekle**seçeneğini belirleyip **sonraki**.

    **Adım iki:** sayfası görüntülenir. 

5. Listeden bir veya daha fazla ülke seçin ve ardından **son** Kuralı etkinleştirmek için. 
    
    Yeni Kural tabloda kasasındaki **ülke filtreleme** sayfası.

    ![Coğrafi filtreleme kuralları](./media/cdn-filtering/cdn-geo-filtering-premium-rules.png)

### <a name="clean-up-resources"></a>Kaynakları temizleme
Ülke filtreleme kuralları tabloda silmek için bir kural yanındaki Sil simgesini veya değiştirmek için Düzenle simgesini seçin.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Coğrafi filtreleme yapılandırması değişiklikleri hemen etkili olmaz:
   * **Microsoft’tan Azure CDN Standart** profilleri için yayma işlemi genellikle 10 dakikada tamamlanır. 
   * **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
   * İçin **verizon'dan Azure CDN standart** ve **verizon'dan Azure CDN Premium** profilleri, yayma işlemi genellikle 10 dakika içinde tamamlanır. 
 
* Bu özellik joker karakterleri desteklemez (örneğin, *).

* Göreli yol ile ilişkili coğrafi filtreleme uygulanan yinelemeli olarak bu yol için bir yapılandırmadır.

* Yalnızca bir kural aynı göreli yoluna uygulanabilir. Diğer bir deyişle, aynı göreli yolu noktası birden fazla ülke filtre oluşturulamıyor. Ancak, ülke filtreleri özyinelemeli olduğundan, bir klasör birden fazla ülke filtreye sahip olabilir. Diğer bir deyişle, önceden yapılandırılmış bir klasörün bir alt farklı ülke filtre atanabilir.

* Coğrafi filtreleme özelliği ülke kodlarına içinden bir istek izin verildiğini veya güvenli bir dizin için engellenen ülkelerin tanımlamak için kullanır. Aynı ülke kodlarına çoğunu Akamai ve Verizon profillerini desteklese de, bazı farklar vardır. Daha fazla bilgi için bkz: [Azure CDN ülke kodlarına](https://msdn.microsoft.com/library/mt761717.aspx). 

