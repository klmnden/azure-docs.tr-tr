---
title: HSM fiziksel güvenlik - Azure ayrılmış HSM | Microsoft Docs
description: Azure ayrılmış HSM cihazlarınızın Veri merkezlerindeki fiziksel güvenliği hakkında bilgi
services: dedicated-hsm
author: barclayn
manager: barbkess
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: barclayn
ms.openlocfilehash: 6fe0557aa90ec3345d4ca0c3d9292cab3435bb2f
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62118069"
---
# <a name="azure-dedicated-hsm-physical-security"></a>Azure ayrılmış HSM fiziksel güvenlik

Azure ayrılmış HSM anahtar depolama alanı için Gelişmiş güvenlik gereksinimlerini karşılamanıza yardımcı olur. Bu, müşterilerin ihtiyaçlarını karşılamak için tam yaşam döngüsü boyunca yönetilen aşağıdaki katı güvenlik uygulamalarını olur.

## <a name="security-through-procurement"></a>Tedarik aracılığıyla güvenliği

Microsoft Güvenli tedarik süreci izler. Biz gözetim zinciri yönetmek ve söz konusu cihaz sıralı ve sevk veri merkezlerimize gelen cihaz olduğundan emin olun. Yetkisiz olay plastik telefonla geri cihazlardır. Bir veri merkezinin veri galeride yetkilendirilen kadar güvenli depolama alanında depolanır.  HSM cihazlarına içeren sunucu rafları, yüksek iş impact(HBI) olarak kabul edilir. Cihazlar kilitlidir ve ön ve arka Kameralı altında her zaman.

## <a name="security-through-deployment"></a>Dağıtım aracılığıyla güvenliği

HSM'ler raflar ilişkili ağ bileşenleri ile birlikte yüklenir. Kullanılabilir hale getirilmeden önce bir kez yüklendikten sonra yapılandırılmaları gerekir Azure ayrılmış HSM hizmetini bir parçası olarak. Bu yapılandırma etkinlik, bir arka plan onayı yapılmıştır Microsoft çalışanları tarafından gerçekleştirilir. "Yalnızca zamanında" (JIT) yönetim yalnızca doğru çalışanlar ve erişim için gerekli olan zamanı erişimi sınırlamak için kullanılır. Ayrıca, sistemler ve yordamları HSM cihazlarına ilişkili tüm etkinlik'in açtığınızdan emin olun.

## <a name="security-in-operations"></a>İşlem güvenliği

HSM'ler, donanım Gereçleri (PCI kartın gereç içinde olan gerçek HSM) olan bileşen düzeyinde sorunları ortaya çıkabilir mümkündür. Olası sorunları içerir ancak fanı ve güç kaynağı hatalarını sınırlı değildir. Bu olay türü bakım gerektiren veya arıza/tamir etkinlikleri swappable bileşenleri değiştirilecek.

### <a name="component-replacement"></a>Bileşen değiştirme

Bir cihaz sağlandıktan sonra ve müşteri Yönetimi altında takılıp çıkarılabilen güç kaynağı yerine geçecekti tek bileşenler ' dir. Bu bileşen, güvenlik sınırı dışında ve kurcalamaya olay neden olmaz. Bir bilet sistemi HBI raf arka erişmek için bir Microsoft mühendisi yetkilendirmek için kullanılır. Geçici bir fiziksel anahtar, anahtar işlendiğinde verilir. Bu anahtarı cihaza mühendisi erişmenizi ve etkilenen bileşeni takas etmenizi sağlar. Herhangi bir erişim (diğer bir deyişle, olay neden değiştirmesine) bir cihaz, bu nedenle güvenlik ve kullanılabilirlik riski en aza müşteriye ayrılmamış olduğunda uygulanır.  

### <a name="device-replacement"></a>Cihaz değiştirme

Toplam cihaz hatası durumunda bileşeni hatası sırasında kullanılan benzer bir işlem izlenir. Müşteri cihaz zeroize mümkün olmayan veya bilinmeyen bir durumda cihaz, cihaz pul veri kaldırılır ve bir raf yok etme depo yerleştirilir. Depo yerleştirilen cihazlar denetimli ve güvenli bir şekilde yok edilir. Cihazlar bir HBI raf pul veri bir Microsoft Veri merkezinde bırakır.

### <a name="other-rack-access-activities"></a>Diğer raf erişimi etkinlikleri

Bir Microsoft Mühendisi (örneğin, ağ aygıtı bakım) HSM cihazlar tarafından kullanılan raf erişmesi gerekiyorsa, standart güvenliği yordamları HBI güvenli raf erişmek için kullanılır. Tüm erişim Kameralı altında olacaktır. HSM cihazlarına doğrulanmıştır [FIPS 140-2 Düzey 3](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.140-2.pdf) HSM cihazları yetkisiz erişim müşteriye sinyal ve veri zeroized.

## <a name="logical-level-security-considerations"></a>Mantıksal düzeyi güvenlik konuları

HSM'ler, müşteri tarafından oluşturulmuş bir sanal ağ için sağlanır. Bir müşterinin özel IUP adres alanı budur.  Bu yapılandırma, değerli mantıksal ağ düzeyinde yalıtım sağlar ve yalnızca müşteri tarafından erişim sağlar. Bu, tüm mantıksal düzeyi güvenlik denetimleri, müşteri sorumluluğu olduğunu gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ve güvenlik ve desteklenebilirliği gibi hizmete tüm temel kavramlarını gibi iyi cihaz sağlama, uygulama tasarım veya dağıtımınızın önce anlaşıldığından önerilir.

* [Yüksek kullanılabilirlik](high-availability.md)
* [Ağ](networking.md)
* [Desteklenebilirliği](supportability.md)
* [İzleme](monitoring.md)
* [Dağıtım mimarisi](deployment-architecture.md)