---
title: Müşteri verilerini istek özelliklerini Azure IOT Central | Microsoft Docs
description: Azure IOT Central, müşteri verileri istek özellikleri
author: dominicbetts
ms.author: dobett
ms.date: 04/24/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 4c63ac8d6b16b2a0476600f67c66b2ccc8dc2aab
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64685567"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri verilerini talep özelliklerin özeti

Azure IOT Central bağlayın, izleyin ve IOT varlıklarınızı uygun ölçekte yönetin, IOT ayrıntılı Öngörüler oluşturun ve bilinçli Eylemler kolaylaştıran tam olarak yönetilen bir nesnelerin interneti (IOT) hizmet olarak yazılım çözümüdür.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Müşteri verileri tanımlama

Azure Active Directory nesne kimlikleri, kullanıcıları ve rolleri atamak için kullanılır. Rol atamaları ancak yalnızca Azure Active Directory nesne kimliği için Azure IOT Central portal görüntüler kullanıcı e-posta adreslerini depolanır, e-posta adresini dinamik olarak Azure Active Directory'den sorgulanır. Azure IOT Central yöneticileri görüntülemek, dışarı aktarma ve uygulama kullanıcılarını bir Azure IOT Central uygulamasına kullanıcı yönetim bölümündeki silin.

Uygulama içinde e-posta adreslerine uyarı almak için yapılandırılabilir. Bu durumda, e-posta adreslerini IOT Central içinde depolanır ve uygulama içi hesabı yönetim sayfasından yönetilmelidir.

Cihazlar, ilgili Microsoft hiçbir bilgi tutar ve kullanıcı bağıntı cihaza sağlayan veri erişimi yok. Çoğu Azure IOT Central yönetilen cihazların kişisel cihazlar örneğin satış makine veya kahve Oluşturucu değildir. Müşteriler, ancak bazı cihazların kişisel olmasını dikkate almanız ve kendi kararımıza kendi varlık veya envanter izleme sistemleri, cihazlar kişilere tie olması. Azure IOT Central yönetir ve kişisel veriler gibi cihazlar ile ilişkili tüm verileri depolar.

Microsoft enterprise Hizmetleri kullandığınızda, sistem tarafından oluşturulan günlükleri olarak bilinen bazı bilgiler, Microsoft oluşturur. Bu günlükler, cihazlara ayrı ayrı ilgili tanılama veri ve hizmet içinde gerçekleştirilen yansıttığından eylemleri oluşturur ve kullanıcı etkinliğini ilişkili değildir. Azure IOT Central sistem tarafından oluşturulan günlükleri erişilebilir veya uygulama yöneticileri tarafından verilebilir değildir.

## <a name="deleting-customer-data"></a>Müşteri verileri silme

Kullanıcı verilerini silme olanağı, yalnızca IOT Central Yönetim sayfası aracılığıyla sağlanır. Uygulama yöneticileri silinecek ve seçmek için kullanıcı seçebilir **Sil** uygulama kaydı silmek için sağ üst köşesindeki. Uygulama yöneticileri Ayrıca, artık söz konusu uygulama ile ilişkili olmayan tek tek hesapları kaldırabilirsiniz.

Bir kullanıcı silindikten sonra başka hiçbir uyarı için e-posta gönderilir. Bununla birlikte, e-posta adresi tek tek her yapılandırılmış uyarıdan kaldırılmalıdır.

Daha fazla bilgi için [kurallar ve Eylemler cihazınız için yapılandırma](tutorial-configure-rules.md).

## <a name="exporting-customer-data"></a>Müşteri verilerini dışarı aktarma

Verileri dışarı aktarma özelliği, yalnızca IOT Central Yönetim sayfası aracılığıyla sağlanır. Atanan roller dahil olmak üzere, müşteri verilerini, kopyalanan ve uygulama yönetici tarafından yapıştırılan seçili.

## <a name="links-to-additional-documentation"></a>Ek belgelere bağlantılar

Rol tanımları da dahil olmak üzere, hesap yönetimi hakkında daha fazla bilgi için bkz. [uygulamanızı yönetmek nasıl](howto-administer.md).
