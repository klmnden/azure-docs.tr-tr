---
title: Azure Stack'te DNS | Microsoft Docs
description: Azure Stack'te DNS kullanma
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.openlocfilehash: acb8b262256031ae8615180e0f55c98cb56b538d
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060369"
---
# <a name="using-dns-in-azure-stack"></a>Azure Stack'te DNS kullanma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack aşağıdaki etki alanı adı sistemi (DNS) özellikleri destekler:

* DNS ana bilgisayar adı çözümlemesi
* DNS bölgelerini ve kayıtlarını API'sini kullanarak oluşturma ve yönetme

## <a name="support-for-dns-hostname-resolution"></a>DNS ana bilgisayar adı çözümlemesi için destek

Genel IP kaynağı için bir DNS etki alanı ad etiketi belirtebilirsiniz. Azure Stack kullanan *domainnamelabel.location*. etiket adının ve Azure Stack'te genel IP adresi eşlemeleri için cloudapp.azurestack.external yönetilen DNS sunucuları.

Örneğin, bir genel IP kaynağı oluşturursanız **contoso** yerel Azure Stack konumda bir etki alanı adı etiketi olarak [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN)  **contoso.Local.cloudapp.azurestack.external** kaynağın genel IP adresine çözümler. Bu FQDN, özel bir etki alanını Azure Stack'te genel IP adresini işaret eden bir CNAME kaydı oluşturmak için kullanabilirsiniz.

Ad çözümlemesi hakkında daha fazla bilgi için bkz [DNS çözümlemesi](https://docs.microsoft.com/azure/dns/dns-for-azure-services?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) makalesi.

> [!IMPORTANT]
> Oluşturduğunuz her etki alanı ad etiketi kendi Azure Stack konumunda benzersiz olmalıdır.

Sonraki ekran yakalama programları **genel IP adresi oluşturma** portalını kullanarak genel bir IP adresi oluşturmak için iletişim.

![Genel IP adresi oluşturma](media/azure-stack-whats-new-dns/image01.png)

**Örnek senaryo**

Bir web uygulamasından isteklerini işleme bir yük dengeleyici var. Yük Dengeleyici bir veya daha fazla sanal makineler üzerinde çalışan bir web sitesi ' dir. Yük dengeli bir DNS adı yerine IP adresi kullanarak web sitesi erişebilirsiniz.

## <a name="create-and-manage-dns-zones-and-records-using-the-api"></a>DNS bölgeleri ve kayıtları API'sini kullanarak oluşturma ve yönetme

Oluşturun ve DNS bölgeleri ve kayıtlarını Azure Stack'te yönetme.

Azure Stack, Azure'nın DNS API'leri ile tutarlı API'lerini kullanarak Azure'nın gibi bir DNS hizmeti sağlar.  Azure Stack DNS etki alanlarınızı barındırarak aynı kimlik bilgilerini, API'leri ve araçları kullanarak DNS kayıtlarınızı yönetebilirsiniz. Ayrıca, aynı faturalandırma kullanın ve diğer Azure hizmetlerinde destekler.

Azure Stack DNS altyapısı, Azure'un daha kısadır. Azure Stack dağıtım konumunu ve boyutunu DNS kapsam, ölçek ve performans etkiler. Bu durum, performans, kullanılabilirlik, genel dağıtım ve yüksek kullanılabilirlik dağıtımı dağıtım değişebilir da gelir.

## <a name="comparison-with-azure-dns"></a>Azure DNS ile karşılaştırma

Azure Stack'te DNS, Azure DNS'de benzerdir, ancak büyük özel durumlar anlamak için ihtiyacınız vardır.

* **AAAA kayıt desteklemiyor**

    Azure Stack, Azure Stack IPv6 adreslerini desteklemediğinden AAAA kayıt desteklemez.  Azure DNS'de ve Azure Stack arasında önemli bir fark budur.
* **Çok kiracılı değil**

    Azure Stack'te DNS hizmeti çok kiracılı değil. Her Kiracı aynı DNS bölgesi oluşturulamıyor. Bölge oluşturma girişimi şu ilk abonelik başarılı ve başarısız istekler.  Bu bilinen bir sorundur ve Azure ve Azure Stack DNS arasındaki en önemli fark olur. Bu sorun gelecek sürümde çözülecektir.
* **Etiketler, meta verileri ve Etag'ler**

    Azure Stack, etiketler, meta verileri, Etag'ler ve sınırları nasıl işlediğini küçük farklılıklar vardır.

Azure DNS hakkında daha fazla bilgi için bkz: [DNS bölgeleri ve kayıtları](../../dns/dns-zones-records.md).

### <a name="tags-metadata-and-etags"></a>Etiketler, meta verileri ve Etag'ler

**Etiketler**

Azure Stack DNS, DNS bölgesi kaynakları Azure Resource Manager etiketleri kullanarak destekler. 'Metadata' alternatif desteklenir gibi sonraki açıklandığı gibi DNS kayıt kümelerinin ancak DNS kayıt kümeleri üzerinde etiketleri desteklememektedir.

**Meta verileri**

Kayıt kümesi etiketleri alternatif, Azure Stack DNS yorumlama 'metadata' kullanarak kayıt kümelerini destekler. Benzer şekilde etiketler, meta verileri, ad-değer çiftleri her bir kayıt kümesi ile ilişkilendirmek sağlar. Örneğin, bu amacı, her bir kayıt kümesi kaydetmek yararlı olabilir. Etiketleri farklı meta verileri Azure faturanızı filtrelenmiş bir görünümünü sağlamak için kullanılamaz ve bir Azure Resource Manager ilkesinde belirtilemez.

**Etag'ler**

Aynı anda bir DNS kaydı değiştirmek iki kişinin veya iki işlem ile deneyin varsayalım. Hangisinin kazandı? Ve kazanan başkaları tarafından oluşturulan değişikliklerin üzerine bilir?

Azure Stack DNS Etag'ler aynı kaynağa eş zamanlı değişiklikleri güvenli bir şekilde işlemek için kullanır. Azure Resource Manager'dan 'Etiketleri' Etag'ler ayrıdır. Her DNS kaynak (bölge ya da kayıt kümesi), kendisiyle ilişkili bir Etag'e sahiptir. Bir kaynak alınan her dosyanın etag değeri de alınır. Bir kaynak güncelleştirirken, Azure Stack DNS doğrulayabilmeniz için Etag sunucu eşleşmeleri ile Etag geri geçirmek seçebilirsiniz. Bir kaynağa her bir güncelleştirme yeniden oluşturuluyor Etag sonuçları olduğundan, bir Etag uyuşmazlığı eşzamanlı bir değişiklik meydana geldiğini gösterir. Etag'ler de yeni bir kaynak oluşturulurken kaynak zaten yoksa emin olmak için kullanılabilir.

Varsayılan olarak, Azure Stack DNS PowerShell bölgelere eş zamanlı değişiklikleri engellemek ve kaydı kümeleri için Etag'ler kullanır. İsteğe bağlı *-üzerine* anahtar Etag denetimlerini gizlemek için kullanılabilir, eş zamanlı her durumda, gerçekleşen değişikliklerin üzerine yazılır.

Azure Stack DNS REST API düzeyinde Etag'ler HTTP üst bilgilerini kullanarak belirtilir. Davranışları aşağıdaki tabloda verilmiştir:

| Üst bilgi | Davranış|
|--------|---------|
| None   | PUT (herhangi bir Etag denetimi) her zaman başarılı|
| IF-match| PUT yalnızca kaynak var ve Etag eşleşiyorsa başarılı|
| IF-match *| Kaynak yoksa PUT yalnızca başarılı|
| IF-none-match *| Kaynak yoksa PUT yalnızca başarılı|

### <a name="limits"></a>Sınırlar

Azure Stack DNS kullanırken aşağıdaki varsayılan sınırlar geçerlidir:

| Kaynak| Varsayılan limit|
|---------|--------------|
| Abonelik başına bölge| 100|
| Bölge başına kayıt kümeleri| 5000|
| Kayıt kümesi başına kayıt| 20|

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack için iDNS ile tanışın](azure-stack-understanding-dns.md)
