---
title: Bir Azure DNS alt etki alanı temsilcisi
description: Bir Azure DNS alt etki alanı temsilcisi öğrenin.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 2/7/2019
ms.author: victorh
ms.openlocfilehash: 31543db8e177701ddfe6beaaa3091d6465b0e9cd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60790819"
---
# <a name="delegate-an-azure-dns-subdomain"></a>Bir Azure DNS alt etki alanı temsilcisi

Bir DNS alt etki alanı için temsilci seçmek için Azure portalını kullanabilirsiniz. Örneğin, contoso.com etki alanına aitse, adlı bir alt etki alanını devredebilirsiniz *mühendislik* başka ve ayrı bölgesine contoso.com bölgesi ayrı olarak yönetebilir.

Tercih ederseniz, kullanarak bir alt etki alanını devredebilirsiniz [Azure PowerShell](delegate-subdomain-ps.md).

## <a name="prerequisites"></a>Önkoşullar

Bir Azure DNS alt etki alanı atanacak genel etki alanınızı Azure DNS'e devretmeniz gerekir. Bkz: [bir etki alanını Azure DNS'ye devretme](./dns-delegate-domain-azure-dns.md) ad sunucularınızın temsilcisi için yapılandırma hakkında yönergeler için. Etki alanınızda, Azure DNS bölgesini temsilci sonra alt etki alanını devredebilirsiniz.

> [!NOTE]
> Contoso.com, bu makalenin tamamında örnek olarak kullanılır. contoso.com yerine kendi etki alanı adınızı yazın.

## <a name="create-a-zone-for-your-subdomain"></a>Bir bölge, alt etki alanı oluşturma

İlk olarak, bölgenin oluşturma **mühendislik** alt etki alanı.

1. Azure portalından seçin **kaynak Oluştur**.
2. Arama kutusuna **DNS**seçip **DNS bölgesi**.
3. **Oluştur**’u seçin.
4. İçinde **DNS bölgesi oluştur** bölmesinde, türü **engineering.contoso.com** içinde **adı** metin kutusu.
5. Bölgeniz için kaynak grubunu seçin. Aynı kaynak grubu, benzer kaynakları bir arada tutmak için bir üst bölge kullanmak isteyebilirsiniz.
6. **Oluştur**’a tıklayın.
7. Dağıtım başarılı olduktan sonra yeni bölgesine gidin.

## <a name="note-the-name-servers"></a>Ad sunucularını unutmayın

Ardından, mühendislik alt etki alanı için dört ad sunucusunun unutmayın.

Üzerinde **mühendislik** bölge bölmesinde, bölge için dört ad sunucusunun unutmayın. Bu ad sunucularını daha sonra kullanacaksınız.

## <a name="create-a-test-record"></a>Bir test kaydı oluşturun

Oluşturma bir **A** test etmek için kullanılacak kayıt. Örneğin, oluşturun bir **www** A kaydetmek ve onunla yapılandırmak bir **10.10.10.10** IP adresi.

## <a name="create-an-ns-record"></a>Bir NS kayıt oluşturma

Ardından, bir ad sunucusu (NS) kaydı için oluşturma **mühendislik** bölge.

1. Üst etki alanı için bölgeyi gidin.
2. **+ Kayıt kümesi**’ni seçin.
3. Üzerinde **kayıt kümesi Ekle** bölmesinde, türü **mühendislik** içinde **adı** metin kutusu.
4. İçin **türü**seçin **NS**.
5. Altında **ad sunucusu**, daha önce gelen kaydettiğiniz dört ad sunucusunun girin **mühendislik** bölge.
6. **Tamam**'ı tıklatın.

## <a name="test-the-delegation"></a>Temsilci seçmeyi test

Temsilci seçmeyi test etmek için nslookup kullanın.

1. Bir PowerShell penceresi açın.
2. Komut istemine yazın `nslookup www.engineering.contoso.com.`
3. Adresini gösteren bir yetkili olmayan yanıt alması gereken **10.10.10.10**.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [Azure'da barındırılan hizmetleri için ters DNS yapılandırma](dns-reverse-dns-for-azure-services.md).