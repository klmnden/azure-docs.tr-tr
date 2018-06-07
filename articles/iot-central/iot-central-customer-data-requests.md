---
title: İstek özellikleri Azure IOT merkezi müşteri verilerini | Microsoft Docs
description: Müşteri verileri Azure IOT merkezi özelliklerinde isteği
author: dominicbetts
ms.author: dobett
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: bb5e263b0332f957b4e7f4a928ccd53f639bcd9c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629414"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri verileri isteği özelliklerinin özeti

Azure IOT Orta bağlanmak, izlemek ve ölçekte IOT varlıklarınızı yönetmek, IOT verilerinizden ayrıntılı Öngörüler oluşturmak ve bilinçli eylemde daha kolay hale getirir, tam olarak yönetilen bir nesnelerin interneti (IOT) hizmet olarak yazılım çözümüdür.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Müşteri verileri tanımlama

Azure Active Directory nesne kimlikleri kullanıcılarını belirlemek ve rolleri atamak için kullanılır. Rol atamalarını ancak yalnızca Azure Active Directory nesne kimliği için Azure IOT merkezi portal görüntüler kullanıcı e-posta adresleri depolanır, e-posta adresi dinamik olarak Azure Active Directory'den sorgulanır. Azure IOT Orta yöneticileri görüntülemek, vermek ve uygulama kullanıcıları Azure IOT merkezi bir uygulamanın kullanıcı Yönetim bölümünde silin.

Uygulama içinde e-posta adreslerine uyarı almak için yapılandırılabilir. Bu durumda, e-posta adresleri IOT Orta içinde depolanır ve uygulama hesabı yönetim sayfasından yönetiliyor olması gerekir.

Cihazlar, ilgili Microsoft hiçbir bilgi tutar ve kullanıcı bağıntı cihaza etkinleştirir verilere erişim yok. Çoğu Azure IOT merkezi yönetilen cihazların kişisel aygıtlar, örneğin satış makine veya Kahve Makinesi değildir. Müşteriler, ancak, bazı cihazların kişisel olmasını düşünebilirsiniz ve kendi istediğiniz kadar kendi varlık veya izleme kişiler cihazlara tie sistemleri stok korumak. Azure IOT Orta yönetir ve kişisel veriler gibi cihazlar ile ilişkili tüm verileri depolar.

Microsoft enterprise hizmetlerini kullandığınızda, sistem tarafından oluşturulan günlükleri bilinen bazı bilgiler, Microsoft oluşturur. Bu günlükler hizmet ve tek cihazlara ilgili Tanılama verileri içinde gerçekleştirilen factual Eylemler oluşturur ve kullanıcı etkinliğini ilişkili değildir. Azure IOT Orta sistem tarafından oluşturulan günlükleri erişilebilir veya uygulama yöneticileri tarafından verilebilir değil.

## <a name="deleting-customer-data"></a>Müşteri verileri silme

Kullanıcı verileri silme olanağı yalnızca IOT Merkezi Yönetim sayfasına sağlanır. Uygulama yöneticileri, kullanıcının silinmesi ve tıklatın seçebilirsiniz **silmek** kaydı silinecek uygulamanın sağ üst köşesindeki. Uygulama yöneticileri aynı zamanda artık söz konusu uygulama ile ilişkili bireysel hesaplar kaldırabilirsiniz.

Bir kullanıcı silindikten sonra başka hiçbir uyarı onlara e-posta gönderilir. Ancak, kullanıcıların e-posta adresi ayrı ayrı her yapılandırılmış uyarıdan kaldırılmalıdır.

Daha fazla bilgi için bkz: [kurallar ve Eylemler, cihazınız için yapılandırma](tutorial-configure-rules.md).

## <a name="exporting-customer-data"></a>Müşteri verileri dışarı aktarma

Veri verme olanağı yalnızca IOT Merkezi Yönetim sayfasına sağlanır. Müşteri verileri, atanan roller dahil olmak üzere seçili, kopyalanır ve bir uygulama Yöneticisi tarafından yapıştırılan.

## <a name="links-to-additional-documentation"></a>Ek belgelere bağlantıları

Rol tanımları dahil olmak üzere, hesap yönetimi hakkında daha fazla bilgi için bkz: [uygulamanızı yönetme](howto-administer.md).
