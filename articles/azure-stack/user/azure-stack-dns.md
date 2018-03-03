---
title: "DNS Azure yığınında | Microsoft Docs"
description: "Azure Stack’te DNS"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 394abe5295af4ed99e48d50b5886ac93af87e875
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="dns-in-azure-stack"></a>Azure Stack’te DNS

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın aşağıdaki DNS özellikleri içerir:
* DNS ana bilgisayar adı çözümlemesi için destek
* DNS bölgeleri ve API kullanarak kayıtları oluşturma ve yönetme

## <a name="support-for-dns-hostname-resolution"></a>DNS ana bilgisayar adı çözümlemesi için destek
Bir DNS etki alanı adı etiketi için bir eşleme oluşturur genel bir IP kaynağı için belirttiğiniz *domainnamelabel.location*. genel IP adresine Azure yığınında cloudapp.azurestack.external yönetilen DNS sunucuları.  

Örneğin, bir ortak IP kaynağıyla oluşturursanız **contoso** bir etki alanı adı etiketi yerel Azure yığın konumda, tam etki alanı adı (FQDN) olarak **contoso.local.cloudapp.azurestack.external**kaynağı ortak IP adresine çözümler. Bu FQDN, özel bir etki alanı Azure yığınında ortak IP adresine işaret eden bir CNAME kaydı oluşturmak için kullanabilirsiniz.

> [!IMPORTANT]
> Oluşturulan her etki alanı adı etiketi Azure yığın konumuna içinde benzersiz olmalıdır.

Portalı kullanarak genel IP adresi oluşturursanız, şöyle görünür:

![Ortak IP adresi oluştur](media/azure-stack-whats-new-dns/image01.png)

Bu yapılandırma, bir ortak IP adresi yük dengeli bir kaynakla ilişkilendirmek istiyorsanız yararlıdır. Örneğin, bir web uygulamasından istekleri işlemeyi bir yük dengeleyici olabilir. Yük Dengeleyici bir veya daha fazla sanal makine üzerinde bulunan bir web sitesinde ' dir. Şimdi bir DNS adı yerine IP adresi yük dengeli web sitesine erişebilirsiniz.

## <a name="create-and-manage-dns-zones-and-records-using-api"></a>DNS bölgeleri ve API kullanarak kayıtları oluşturma ve yönetme
Oluşturun ve DNS bölgeleri ve Azure yığınında kayıtları yönetin.  

Azure yığını, Azure'nın DNS API'leri ile tutarlı API'lerini kullanarak Azure'nın gibi bir DNS hizmet sağlar.  Azure yığın DNS'de etki alanlarınızı barındırarak, diğer Azure hizmetleriyle DNS kayıtlarınızı aynı kimlik bilgilerini, API'leri, Araçlar, faturalama ve Destek ile yönetebilirsiniz. 

Belirgin nedenlerle Azure yığın DNS altyapısı Azure'un daha büyük/küçük harf kısadır. Bu nedenle, kapsam, ölçek ve performans Azure yığın dağıtımına ve dağıtıldığı ortamı ölçeğini bağlıdır.  Bu nedenle, performans, kullanılabilirlik, genel dağıtım ve yüksek kullanılabilirlik (HA) gibi dağıtım dağıtım farklılık gösterebilir.

## <a name="comparison-with-azure-dns"></a>Azure DNS ile karşılaştırma
Azure yığın DNS'de, iki ana istisnalar Azure DNS'de benzer:
* **AAAA kayıt desteklemiyor**

    Azure yığın IPv6 adresleri desteklemediğinden azure yığın AAAA kayıt desteklemez.  Azure DNS'de ve Azure yığın arasında önemli bir fark budur.
* **Çok kiracılı değil**

    Azure farklı olarak, DNS hizmeti Azure yığını, çok kiracılı değil. Bu nedenle her bir kiracı aynı DNS bölgesi oluşturulamıyor. Bölge oluşturma girişiminde ilk abonelik başarılı olursa ve sonraki istekleri başarısız olur.  Bu bilinen bir sorundur ve Azure ve Azure yığın DNS arasındaki en önemli fark olur. Bu sorun gelecekteki bir sürümde çözümlenir.

Ayrıca, Azure yığın DNS etiketleri, meta verileri, Etag'ler ve sınırları nasıl uyguladığını bazı küçük farklar vardır.

Aşağıdaki bilgiler, özellikle Azure yığın DNS için geçerlidir ve Azure DNS'den biraz değişir. Azure DNS hakkında daha fazla bilgi için bkz: [DNS bölgeleri ve kayıtları](../../dns/dns-zones-records.md) Microsoft Azure belgelerine sitesinde.

### <a name="tags-metadata-and-etags"></a>Etiketler, meta verileri ve Etag'ler

**Etiketler**

DNS bölge kaynakları Azure Resource Manager etiketleri kullanarak Azure yığın DNS destekler. Alternatif 'meta verileri' desteklenir gibi DNS kaydı sonraki açıklandığı gibi ayarlar ancak DNS kayıt kümelerini üzerinde etiketleri desteklemez.

**Metadata**

Kayıt kümesi etiketleri alternatif olarak, Azure yığın DNS kayıt kümelerini 'meta verileri' kullanarak açıklanmasını destekler. Benzer şekilde etiketleri, meta verileri, ad-değer çiftleri her bir kayıt kümesi ile ilişkilendirmek sağlar. Örneğin, bu amacı, her bir kayıt kümesi kaydetmek yararlı olabilir. Etiketler, aksine meta verileri Azure faturasını filtre uygulanmış bir görünümünü sağlamak için kullanılamaz ve bir Azure Resource Manager ilkesinde belirtilemez.

**Etag'ler**

İki kişinin veya iki işlemler aynı anda bir DNS kaydı değiştirmeye çalıştığınızda varsayalım. Hangisinin WINS? Ve başkaları tarafından oluşturulan değişikliklerin üzerine kazanan bilir?

Azure yığın DNS Etag'ler aynı kaynak eşzamanlı değişiklikleri güvenli bir şekilde işlemek için kullanır. Azure Kaynak Yöneticisi'nden 'Etiketleri' Etag'ler ayrıdır. Her DNS kaynak (bölge veya kayıt kümesi) ilişkili bir ETag değerine sahip. Bir kaynak alınır olduğunda, kendi Etag de alınır. Bir kaynak güncelleştirirken Azure yığın DNS doğrulayın şekilde Etag sunucu eşleşmeleri ile Etag geri geçirmek seçebilirsiniz. Yeniden Etag bir kaynağa her bir güncelleştirme sonuçları olduğundan, bir Etag uyuşmazlığı eşzamanlı bir değişiklik oluştuğunu gösterir. Etag'ler da yeni bir kaynak oluştururken kaynak zaten yoksa emin olmak için kullanılabilir.

Varsayılan olarak, Azure yığın DNS PowerShell Etag'ler bölgelere eşzamanlı değişiklikleri engellemek ve kayıt kümeleri için kullanır. İsteğe bağlı *-üzerine* anahtar Etag denetimlerini gizlemek için kullanılabilir, her eşzamanlı durumda oluşan değişikliklerin üzerine yazılır.

Azure yığın DNS REST API düzeyinde Etag'ler HTTP üstbilgileri kullanılarak belirtilir. Davranışlarını aşağıdaki tabloda verilmiştir:

| Üst bilgi | Davranış|
|--------|---------|
| None   | PUT (Etag denetimleri) her zaman başarılı|
| IF-match| PUT yalnızca kaynak var ve Etag eşleşiyorsa başarılı|
| IF-match *| Kaynak varsa PUT yalnızca başarılı|
| IF-none-match *| Kaynak yoksa, PUT yalnızca başarılı|

### <a name="limits"></a>Sınırlar

Aşağıdaki varsayılan sınırları Azure yığın DNS kullanırken geçerlidir:

| Kaynak| Varsayılan limit|
|---------|--------------|
| Abonelik başına bölgeleri| 100|
| Her bölge için kayıt kümeleri| 5000|
| Kayıt kümesi başına kayıt| 20|

## <a name="next-steps"></a>Sonraki adımlar
[IDN'ler için Azure yığınına Tanıtımı](azure-stack-understanding-dns.md)
