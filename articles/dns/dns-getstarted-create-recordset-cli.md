---
title: CLI kullanarak bir DNS Bölgesi için kayıt kümesi ve kayıt oluşturma| Microsoft Docs
description: Azure DNS için ana bilgisayar kayıtları nasıl oluşturulur? CLI kullanarak kayıt kümelerini ve kayıtları ayarlama
services: dns
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: ''

ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: sewhee

---
# CLI kullanarak DNS kayıt kümelerini ve kayıtları oluşturma
> [!div class="op_single_selector"]
> * [Azure Portalı](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI](dns-getstarted-create-recordset-cli.md)
> 
> 

Bu makale, CLI kullanarak kayıtlar ve kayıt kümeleri oluşturma işlemi boyunca size yol gösterir. DNS bölgenizi oluşturduktan sonra, etki alanınız için DNS kayıtları eklemeniz gereklidir. Bunu yapmak için öncelikle DNS kayıtlarını ve kayıt kümelerini anlamanız gerekir.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## Kayıt kümesi ve kayıt oluşturma
Bu bölümde bir kayıt kümesinin ve kayıtların nasıl oluşturulduğunu size göstereceğiz. Bu örnekte, "contoso.com" DNS bölgesinde "www" göreli adına sahip olan bir kayıt kümesi oluşturacaksınız. "www.contoso.com" kayıtların tam adıdır. Kayıt türü "A" ve yaşam süresi (TTL) 60 saniyedir. Bu adımı tamamladıktan sonra, boş bir kayıt kümesi oluşturmuş olacaksınız.

Bölgenin tepesinde bir kayıt kümesi oluşturmak için (bu durumda "contoso.com"), tırnak işaretleri dahil olmak üzere "@" kayıt adını kullanın. Bu genel bir DNS kuralıdır.

### 1. Kayıt kümesi oluşturma
Kayıt kümesi oluşturmak için `azure network dns record-set create` kullanın. Kaynak grubunu, bölge adını, kayıt kümesinin göreli adını, kayıt türünü ve TTL'yi belirtin. `--ttl` parametresi tanımlı değilse değer varsayılan olarak dört (saniye cinsinden) olur. Bu adımı tamamladıktan sonra, boş bir "www" kayıt kümesine sahip olursunuz.

*Kullanım: network dns record-set create <resource-group> <dns-zone-name> <name> <type> <ttl>*

    azure network dns record-set create myresourcegroup  contoso.com  www A  60

### 2. Kayıt ekleme
Yeni oluşturulan "www" kayıt kümesini kullanmak için buna kayıt eklemeniz gerekir. `azure network dns record-set add-record` kullanarak kayıt kümelerine kayıt ekleyebilirsiniz.

Bir kayıt kümesine kayıt eklemeye yönelik parametreler, kayıt kümesinin türüne bağlı olarak farklılık gösterir. Örneğin, "A" türünde bir kayıt kümesi kullanırken, yalnızca `-a <IPv4 address>` parametreli kayıtları belirtebileceksiniz.

Aşağıdaki komutu kullanarak "www" kayıt kümesine IPv4 *A* kayıtları ekleyebilirsiniz:

*Kullanım: network dns record-set add-record <resource-group> <dns-zone-name> <record-set-name> <type>*

    azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 134.170.185.46

## Ek kayıt türü örnekleri
Aşağıdaki örnekler, her bir kayıt türü için bir kayıt kümesinin nasıl oluşturulacağını gösterir. Her bir kayıt kümesi tek bir kayıt içerir.

[!INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]

## Sonraki adımlar
Kayıt kümenizi ve kayıtları yönetmek için bkz. [DNS kayıtlarını ve kayıt kümelerini CLI kullanarak yönetme](dns-operations-recordsets-portal.md).

Azure DNS hakkında daha fazla bilgi için bkz. [Azure DNS'ye Genel Bakış](dns-overview.md).

<!--HONumber=Oct16_HO1-->


