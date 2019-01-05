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
ms.date: 01/05/2019
ms.author: sethm
ms.openlocfilehash: ba1e310234485d972646320f082d8b882a3d43f1
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052351"
---
# <a name="using-dns-in-azure-stack"></a>Azure Stack'te DNS kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack aşağıdaki etki alanı adı sistemi (DNS) özellikleri destekler:

* DNS ana bilgisayar adı çözümlemesi
* DNS bölgeleri ve kayıtları API'sini kullanarak oluşturma ve yönetme

## <a name="support-for-dns-hostname-resolution"></a>DNS ana bilgisayar adı çözümlemesi için destek

Genel IP kaynağı için bir DNS etki alanı ad etiketi belirtebilirsiniz. Azure Stack kullanan **domainnamelabel.location.cloudapp.azurestack.external** etiket adı ve eşlemeler için Azure Stack'te genel IP adresi için DNS sunucularının yönetilen.

Örneğin, bir genel IP kaynağı oluşturursanız **contoso** yerel Azure Stack konumda bir etki alanı adı etiketi olarak [tam etki alanı adı (FQDN)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)  **contoso.Local.cloudapp.azurestack.external** kaynağın genel IP adresine çözümler. Bu FQDN, özel bir etki alanını Azure Stack'te genel IP adresini işaret eden bir CNAME kaydı oluşturmak için kullanabilirsiniz.

Ad çözümlemesi hakkında daha fazla bilgi için bkz. [DNS çözümlemesi](../../dns/dns-for-azure-services.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) makalesi.

> [!IMPORTANT]
> Oluşturduğunuz her etki alanı ad etiketi kendi Azure Stack konumunda benzersiz olmalıdır.

Aşağıdaki ekran görüntüsü gösterildiği **genel IP adresi oluşturma** iletişim portalını kullanarak genel bir IP adresi oluşturmak için:

![Genel IP adresi oluşturma](media/azure-stack-whats-new-dns/image01.png)

### <a name="example-scenario"></a>Örnek senaryo

Bir web uygulamasından isteklerini işleme bir yük dengeleyici var. Yük Dengeleyici bir veya daha fazla sanal makineler üzerinde çalışan bir web sitesi ' dir. Yük dengeli bir DNS adı yerine IP adresi kullanarak web sitesi erişebilirsiniz.

## <a name="create-and-manage-dns-zones-and-records-using-the-api"></a>DNS bölgeleri ve kayıtları API'sini kullanarak oluşturma ve yönetme

Oluşturun ve DNS bölgeleri ve kayıtlarını Azure Stack'te yönetme.

Azure Stack, Azure DNS API'leri ile tutarlı API'lerini kullanarak Azure'a benzer bir DNS hizmeti sağlar.  Azure Stack DNS etki alanlarınızı barındırarak aynı kimlik bilgilerini, API'leri ve araçları kullanarak DNS kayıtlarınızı yönetebilirsiniz. Ayrıca, aynı faturalandırma kullanın ve diğer Azure hizmetlerinde destekler.

Azure Stack DNS altyapısı Azure daha kısadır. Azure Stack dağıtım konumunu ve boyutunu, DNS kapsam, ölçek ve performans etkiler. Bu durum, performans, kullanılabilirlik, genel dağıtım ve yüksek kullanılabilirlik dağıtımı dağıtım değişebilir da gelir.

## <a name="comparison-with-azure-dns"></a>Azure DNS ile karşılaştırma

Azure Stack'te DNS, Azure DNS'de benzerdir, ancak birkaç önemli özel durum vardır:

* **AAAA kayıt desteklemiyor**: Azure Stack, Azure Stack IPv6 adreslerini desteklemediğinden AAAA kayıt desteklemez. Azure DNS'de ve Azure Stack arasında önemli bir fark budur.

* **Çok kiracılı değil**: Azure Stack'te DNS hizmeti çok kiracılı değil. Her Kiracı aynı DNS bölgesi oluşturulamıyor. Bölge oluşturma girişimi şu ilk abonelik başarılı ve başarısız istekler. Azure ve Azure Stack DNS arasında önemli bir fark budur.

* **Etiketler, meta verileri ve Etag'ler**: Azure Stack, etiketler, meta verileri, Etag'ler ve sınırları nasıl işlediğini küçük farklılıklar vardır.

Azure DNS hakkında daha fazla bilgi için bkz: [DNS bölgeleri ve kayıtları](../../dns/dns-zones-records.md).

### <a name="tags"></a>Etiketler

Azure Stack DNS, DNS bölgesi kaynakları Azure Resource Manager etiketleri kullanarak destekler. Etiketleri DNS kayıt kümeleri, alternatif olarak olsa da, desteklemediği **meta verileri** sonraki bölümde açıklandığı gibi DNS kayıt kümeleri üzerinde desteklenir.

### <a name="metadata"></a>Meta Veriler

Kayıt kümesi etiketleri alternatif, Azure Stack DNS destekler kullanarak kayıt kümelerini yorumlama *meta verileri*. Benzer şekilde etiketler, meta verileri, ad-değer çiftleri her bir kayıt kümesi ile ilişkilendirmek sağlar. Örneğin, bu amacı, her bir kayıt kümesi kaydetmek yararlı olabilir. Etiketler, farklı Azure faturanızı filtrelenmiş bir görünümünü sağlamak için meta verileri kullanamaz ve meta verileri bir Azure Resource Manager ilkesinde belirtilemez.

### <a name="etags"></a>Etag'ler

Aynı anda bir DNS kaydı değiştirmek iki kişinin veya iki işlem ile deneyin varsayalım. Hangisinin kazandı? Ve kazanan başkaları tarafından oluşturulan değişikliklerin üzerine bilir?

Azure Stack DNS kullanır *Etag'ler* aynı kaynağa eş zamanlı değişiklikleri güvenli bir şekilde işlemek için. Azure Resource Manager'dan farklı Etag'ler *etiketleri*. Her DNS kaynak (bölge ya da kayıt kümesi), kendisiyle ilişkili bir Etag'e sahiptir. Dosyanın etag değeri, bir kaynak alındığında de alınır. Bir kaynak güncelleştirirken, Azure Stack DNS doğrulayabilmeniz için Etag sunucu eşleşmeleri ile Etag geri geçirmek seçebilirsiniz. Bir kaynağa her bir güncelleştirme yeniden oluşturuluyor Etag sonuçları olduğundan, bir Etag uyuşmazlığı eşzamanlı bir değişiklik meydana geldiğini gösterir. Etag'ler de yeni bir kaynak oluşturulurken kaynak zaten yoksa emin olmak için kullanılabilir.

Varsayılan olarak, bölgeler için eş zamanlı değişiklikler engellemek ve kaydı kümeleri için Etag'ler Azure Stack DNS PowerShell cmdlet'lerini kullanın. Kullanabileceğiniz isteğe bağlı `-Overwrite` Etag denetimlerini gizlemek için neden olan üzerine yazılmasını gerçekleşen değişikliklerin eşzamanlı geçiş.

Azure Stack DNS REST API düzeyinde Etag'ler HTTP üst bilgilerini kullanarak belirtilir. Davranışları aşağıdaki tabloda açıklanmıştır:

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

- [Azure Stack için iDNS ile tanışın](azure-stack-understanding-dns.md)
