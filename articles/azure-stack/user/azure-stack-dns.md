---
title: DNS Azure yığınında | Microsoft Docs
description: DNS kullanarak Azure yığını
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: mabrigg
ms.openlocfilehash: 4e854a2751ce366e3ca3a353487f2c972401c248
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="using-dns-in-azure-stack"></a>DNS kullanarak Azure yığını

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın aşağıdaki etki alanı adı sistemi (DNS) özellikleri destekler:

* DNS ana bilgisayar adı çözümlemesi
* DNS bölgeleri ve API kullanarak kayıtları oluşturma ve yönetme

## <a name="support-for-dns-hostname-resolution"></a>DNS ana bilgisayar adı çözümlemesi için destek

Genel IP kaynakları için bir DNS etki alanı adı etiketi belirtebilirsiniz. Azure yığınını kullanan *domainnamelabel.location*. etiket adı ve Azure yığınında genel IP adresi eşlemelerini cloudapp.azurestack.external yönetilen DNS sunucuları.

Örneğin, bir ortak IP kaynağıyla oluşturursanız **contoso** bir etki alanı adı etiketi yerel Azure yığın konum olarak [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN)  **contoso.Local.cloudapp.azurestack.external** kaynağı ortak IP adresine çözümler. Bu FQDN, özel bir etki alanı Azure yığınında ortak IP adresine işaret eden bir CNAME kaydı oluşturmak için kullanabilirsiniz.

Ad çözümlemesi hakkında daha fazla bilgi için bkz [DNS çözümlemesi](https://docs.microsoft.com/en-us/azure/dns/dns-for-azure-services?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) makalesi.

> [!IMPORTANT]
> Oluşturduğunuz her etki alanı adı etiketi Azure yığın konumuna içinde benzersiz olmalıdır.

Sonraki ekran yakalama programları **ortak IP adresi oluştur** Portalı'nı kullanarak bir ortak IP adresi oluşturmak için iletişim.

![Ortak IP adresi oluştur](media/azure-stack-whats-new-dns/image01.png)

**Örnek senaryo**

Bir web uygulamasından istekleri işlemeyi bir yük dengeleyiciye sahip. Yük Dengeleyici bir veya daha fazla sanal makine üzerinde çalışan bir web sitesi ' dir. Yük dengeli bir DNS adı yerine IP adresi kullanarak web sitesi erişebilir.

## <a name="create-and-manage-dns-zones-and-records-using-the-api"></a>DNS bölgeleri ve API kullanarak kayıtları oluşturma ve yönetme

Oluşturun ve DNS bölgeleri ve Azure yığınında kayıtları yönetin.

Azure yığını, Azure'nın DNS API'leri ile tutarlı API'lerini kullanarak Azure'nın gibi bir DNS hizmet sağlar.  Azure yığın DNS'de etki alanlarınızı barındırarak DNS kayıtlarınızı aynı kimlik bilgileri, API ve araçları kullanarak yönetebilirsiniz. Ayrıca, aynı fatura kullanın ve diğer Azure hizmetlerinizi destekler.

Azure yığın DNS altyapısı Azure'un daha kısadır. Bir Azure yığın dağıtımına konumunu ve boyutunu DNS kapsam, ölçek ve performans etkiler. Bu ayrıca performans, kullanılabilirlik, genel dağıtım ve yüksek kullanılabilirlik dağıtımı dağıtım değişebilir anlamına gelir.

## <a name="comparison-with-azure-dns"></a>Azure DNS ile karşılaştırma

Azure DNS'de DNS Azure yığınında benzer, ancak ana özel durumları anlamanız gerekir.

* **AAAA kayıt desteklemiyor**

    Azure yığın IPv6 adresleri desteklemediğinden azure yığın AAAA kayıt desteklemez.  Azure DNS'de ve Azure yığın arasında önemli bir fark budur.
* **Çok kiracılı değil**

    Azure yığınında DNS hizmeti çok kiracılı değil. Her bir kiracı aynı DNS bölgesi oluşturulamıyor. Bölge oluşturma girişiminde ilk abonelik başarılı olursa ve sonraki istekleri başarısız olur.  Bu bilinen bir sorundur ve Azure ve Azure yığın DNS arasındaki en önemli fark olur. Bu sorun gelecekteki bir sürümde çözümlenir.
* **Etiketler, meta verileri ve Etag'ler**

    Etiketler, meta verileri, Etag'ler ve sınırları Azure yığın nasıl işlediğini küçük farklar vardır.

Azure DNS hakkında daha fazla bilgi için bkz: [DNS bölgeleri ve kayıtları](../../dns/dns-zones-records.md).

### <a name="tags-metadata-and-etags"></a>Etiketler, meta verileri ve Etag'ler

**Etiketler**

DNS bölge kaynakları Azure Resource Manager etiketleri kullanarak Azure yığın DNS destekler. Alternatif 'meta verileri' desteklenir gibi DNS kaydı sonraki açıklandığı gibi ayarlar ancak DNS kayıt kümelerini üzerinde etiketleri desteklemez.

**Meta veriler**

Kayıt kümesi etiketleri alternatif olarak, Azure yığın DNS kayıt kümelerini 'meta verileri' kullanarak açıklanmasını destekler. Benzer şekilde etiketleri, meta verileri, ad-değer çiftleri her bir kayıt kümesi ile ilişkilendirmek sağlar. Örneğin, bu amacı, her bir kayıt kümesi kaydetmek yararlı olabilir. Etiketler, aksine meta verileri Azure faturasını filtre uygulanmış bir görünümünü sağlamak için kullanılamaz ve bir Azure Resource Manager ilkesinde belirtilemez.

**Etag'ler**

İki kişinin veya iki işlemler aynı anda bir DNS kaydı değiştirmeye çalıştığınızda varsayalım. Hangisinin WINS? Ve başkaları tarafından oluşturulan değişikliklerin üzerine kazanan bilir?

Azure yığın DNS Etag'ler aynı kaynak eşzamanlı değişiklikleri güvenli bir şekilde işlemek için kullanır. Azure Kaynak Yöneticisi'nden 'Etiketleri' Etag'ler ayrıdır. Her DNS kaynak (bölge veya kayıt kümesi) ilişkili bir ETag değerine sahip. Bir kaynak alınır olduğunda, kendi Etag de alınır. Bir kaynak güncelleştirirken Azure yığın DNS doğrulayın şekilde Etag sunucu eşleşmeleri ile Etag geri geçirmek seçebilirsiniz. Yeniden Etag bir kaynağa her bir güncelleştirme sonuçları olduğundan, bir Etag uyuşmazlığı eşzamanlı bir değişiklik oluştuğunu gösterir. Etag'ler da yeni bir kaynak oluştururken kaynak zaten yoksa emin olmak için kullanılabilir.

Varsayılan olarak, Azure yığın DNS PowerShell Etag'ler bölgelere eşzamanlı değişiklikleri engellemek ve kayıt kümeleri için kullanır. İsteğe bağlı *-üzerine* anahtar Etag denetimlerini gizlemek için kullanılabilir, her eşzamanlı durumda oluşan değişikliklerin üzerine yazılır.

Azure yığın DNS REST API düzeyinde Etag'ler HTTP üstbilgileri kullanılarak belirtilir. Davranışlarını aşağıdaki tabloda verilmiştir:

| Üst bilgi | Davranış|
|--------|---------|
| Hiçbiri   | PUT (Etag denetimleri) her zaman başarılı|
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
