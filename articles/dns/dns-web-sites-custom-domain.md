---
title: Bir web uygulaması için özel DNS kayıtları oluşturma | Microsoft Docs
description: Özel etki alanı, web uygulaması kullanarak Azure DNS için DNS kayıtları oluşturma
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
ms.assetid: 6c16608c-4819-44e7-ab88-306cf4d6efe5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: kumud
ms.openlocfilehash: 7ee3dbdcd4d8b2627273a871aec94583b6c5dd6a
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059805"
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Özel bir etki alanında bir web uygulaması için DNS kayıtları oluşturma

Azure DNS özel bir etki alanı için web uygulamalarınızı barındırmak için kullanabilirsiniz. Örneğin, bir Azure web uygulaması oluşturuyorsunuz ve ya da erişmesini istediğiniz contoso.com veya www.contoso.com bir FQDN kullanarak.

Bunu yapmak için iki kaydı oluşturmak zorunda:

* Contoso.com etki alanına işaret eden bir kök "A" kaydı
* Bir A kaydına işaret eden www adı için "CNAME" kaydı

Akılda tutulması için web app değişiklikler temel alınan IP adresi güncelleştirildi, Azure'da bir web uygulaması için bir A kaydı oluşturursanız, A kaydını el ile olması gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce ilk Azure DNS'de bir DNS bölgesi oluşturur ve gerekir, Azure DNS kayıt şirketinizde bölgeyi devredin.

1. Bir DNS bölgesi oluşturmak için adımları [DNS bölgesi oluşturma](dns-getstarted-create-dnszone.md).
2. Azure DNS, DNS temsilci seçmek için adımları izleyin. [DNS etki alanı temsilcisi](dns-domain-delegation.md).

Bölge oluşturma ve Azure DNS'e temsilci olarak görevlendirme sonra özel etki alanınız için kayıtları daha sonra oluşturabilirsiniz.

## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Özel etki alanınız için bir A kaydı oluşturun

Bir A kaydı, bir IP adresine eşlemek için kullanılır. Aşağıdaki örnekte, biz atar \@ bir IPv4 adresi için bir A kaydı olarak:

### <a name="step-1"></a>1. Adım

Bir A kaydı oluşturun ve bir değişkene $rs atayın

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a>2. Adım

Önceden oluşturulmuş bir kayıt kümesine IPv4 değeri Ekle "\@" atanan $rs değişken kullanma. Web uygulamanız için IP adresi atanmış IPv4 değer olacaktır.

Web uygulaması için IP adresini bulmak için adımları izleyin. [Azure App Service'te özel etki alanı adı yapılandırma](../app-service/app-service-web-tutorial-custom-domain.md).

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a>3. Adım

Kayıt kümesi için değişiklikleri kaydedin. Kullanım `Set-AzureRMDnsRecordSet` değişikliklerin kayıt kümesi için Azure DNS'yi yüklemek için:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Özel etki alanınız için bir CNAME kaydı oluşturun

Etki alanınız zaten Azure DNS tarafından yönetiliyorsa (bkz [DNS etki alanı temsilcisi](dns-domain-delegation.md), aşağıdakileri kullanabilirsiniz contoso.azurewebsites.net için bir CNAME kaydı oluşturmak için örnek.

### <a name="step-1"></a>1. Adım

PowerShell'i açın ve yeni bir CNAME kaydı kümesi oluşturma ve bir değişkene $rs atayın. Bu örnekte "contoso.com" adlı DNS bölgesinde 600 saniye "bir süresi" ile bir kayıt kümesi türü CNAME oluşturun.

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

Aşağıdaki örnek, yanıttır.

```
Name              : www
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>2. Adım

CNAME kaydı kümesi oluşturulduktan sonra web uygulamasına işaret edecek bir diğer ad değeri oluşturmanız gerekir.

Önceden atanmış değişken "$rs" kullanarak web uygulaması contoso.azurewebsites.net için diğer ad oluşturmak için aşağıdaki PowerShell komutu kullanabilirsiniz.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

Aşağıdaki örnek, yanıttır.

```
    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>3. Adım

Kullanarak değişiklikleri `Set-AzureRMDnsRecordSet` cmdlet:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

Kaydı doğrulamak için aşağıda gösterildiği gibi "www.contoso.com" nslookup kullanarak sorgulayarak doğru şekilde oluşturulmuş:

```
PS C:\> nslookup
Default Server:  Default
Address:  192.168.0.1

> www.contoso.com
Server:  default server
Address:  192.168.0.1

Non-authoritative answer:
Name:    <instance of web app service>.cloudapp.net
Address:  <ip of web app service>
Aliases:  www.contoso.com
contoso.azurewebsites.net
<instance of web app service>.vip.azurewebsites.windows.net
```

## <a name="create-an-awverify-record-for-web-apps"></a>Web apps için bir "awverify" kayıt oluşturma

Web uygulamanız için bir A kaydı kullanmaya karar verirseniz, özel etki alanının sahibi olmak için bir doğrulama işleminden gitmeniz gerekir. Bu doğrulama adımı "awverify" adlı özel bir CNAME kaydı oluşturarak yapılır. Bu bölüm yalnızca kayıtlar için geçerlidir.

### <a name="step-1"></a>1. Adım

"Awverify" kaydı oluşturun. Aşağıdaki örnekte, özel etki alanı sahipliğini doğrulamak contoso.com için "aweverify" kayıt oluşturacağız.

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

Aşağıdaki örnek, yanıttır.

```
Name              : awverify
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>2. Adım

"Awverify" kayıt kümesi oluşturulduktan sonra diğer adı ayarlama CNAME kaydı atayın. Aşağıdaki örnekte, biz awverify.contoso.azurewebsites.net için diğer adı ayarlama CNAMe kaydı atar.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

Aşağıdaki örnek, yanıttır.

```
    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>3. Adım

Kullanarak değişiklikleri `Set-AzureRMDnsRecordSet cmdlet`aşağıdaki komutta gösterildiği gibi.

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a>Sonraki adımlar

Bağlantısındaki [App Service için bir özel etki alanı adı yapılandırma](../app-service/app-service-web-tutorial-custom-domain.md) web uygulamanızı özel bir etki alanını kullanacak şekilde yapılandırmak için.
