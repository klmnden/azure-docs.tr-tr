---
title: "DNS Azure yığınında anlama | Microsoft Docs"
description: "DNS özellikleri ve yetenekleri Azure yığınında anlama"
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 60f5ac85-be19-49ac-a7c1-f290d682b5de
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: scottnap
ms.openlocfilehash: 381947a5c936b2bbcae1cf61cdd36eb1653be30b
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="introducing-idns-for-azure-stack"></a>IDN'ler için Azure yığınına Tanıtımı

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

IDN'ler yığında Azure (örneğin, http://www.bing.com) dış DNS adlarını çözümlemek izin veren bir özelliktir.
Ayrıca, iç sanal ağ adları kaydetmenize olanak sağlar. Bunu yaparak, sanal makineleri aynı sanal ağda IP adresi yerine adına göre özel DNS sunucusu girdileri sağlamasına gerek kalmadan çözebilirsiniz.

Her zaman Azure'da var olmuştur bir şey olduğunu, ancak çok Windows Server 2016 ve Azure yığın de kullanılabilir.

## <a name="what-does-idns-do"></a>IDN'ler ne yapar?
Azure yığınında IDN'ler ile özel DNS sunucusu girdileri belirtmek zorunda kalmadan aşağıdaki özellikleri alın:

* Kiracı İş yükleri için DNS ad çözümleme hizmetleri paylaşılan.
* Ad çözümlemesi ve Kiracı sanal ağ içinde DNS kaydı için yetkili DNS hizmeti.
* Kiracı VM'ler Internet adlarından çözümlenmesi için yinelemeli DNS hizmeti. Kiracılar, artık Internet adları (örneğin, www.bing.com) çözümlemek için özel DNS girişlerini belirtmeniz gerekir.

Hala kendi DNS getirin ve istiyorsanız, özel DNS sunucuları kullanın. Ancak henüz artık, Internet DNS adları ve diğer sanal makinelerle aynı sanal ağda bağlanabilmek için herhangi bir şey belirtmenize gerek yoktur ve yalnızca çalışır çözümleyebilmesi istiyorsanız.

## <a name="what-does-idns-not-do"></a>Ne IDN'ler işe yarar?
Hangi IDN'ler yapmanıza olanak sağlar, sanal ağ dışında çözümlenebilir bir ad DNS kaydı oluşturmaktır.

Azure'da, bir ortak IP adresi ile ilişkilendirilebilir bir DNS ad etiketi belirtme seçeneğiniz vardır. Etiket (önek) seçebilirsiniz, ancak Azure genel IP adresi oluşturma bölgeye göre soneki seçer.

![Ekran görüntüsü, DNS ad etiketi](media/azure-stack-understanding-dns-in-tp2/image3.png)

Yukarıdaki resimde bir "A" kaydı DNS bölge altında belirtilen DNS ad etiketi için Azure oluşturacak **westus.cloudapp.azure.com**. Önek ve sonek birlikte bir tam etki alanı adı (genel Internet'te herhangi bir yere çözümlenebilen FQDN) oluşturun.

Aşağıdakileri yapamazsınız şekilde azure yığın IDN'ler dahili ad kaydı için yalnızca destekler:

* Varolan barındırılan bir DNS bölgesi (örneğin, local.azurestack.external) altında bir DNS kaydı oluşturun.
* Bir DNS bölgesi (Contoso.com gibi) oluşturur.
* Kendi özel DNS bölgesi altında bir kayıt oluşturun.
* Etki alanı adları satın destekler.

