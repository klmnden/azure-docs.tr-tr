---
title: Azure yığın depolama giriş
description: Azure yığın depolama hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: 092aba28-04bc-44c0-90e1-e79d82f4ff42
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/21/2018
ms.author: mabrigg
ms.openlocfilehash: d97a5f8aff57f4bbfd7d5222a87d258fa5c92da8
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604395"
---
# <a name="introduction-to-azure-stack-storage"></a>Azure yığın depolama giriş

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

## <a name="overview"></a>Genel Bakış

Azure yığın depolama BLOB'lar, tablolar ve Azure Storage Hizmetleri ile tutarlı olan kuyrukları içeren bir bulut depolama hizmetleri kümesidir.

## <a name="azure-stack-storage-services"></a>Azure yığın depolama hizmetleri

Azure yığın depolama aşağıdaki üç hizmetleri sağlar:

- **Blob Depolama**

    BLOB storage yapılandırılmamış nesne verilerini depolar. Blob; bir belge, ortam dosyası veya uygulama yükleyici gibi herhangi bir türde metin veya ikili veri olabilir.

- **Tablo depolama**

    Table storage yapılandırılmış veri kümelerini depolar. Table Storage, yüksek miktarda verinin hızla dağıtılmasını ve verilere hızla erişilebilmesini sağlayan NoSQL anahtar özniteliği veri deposudur.

- **Kuyruk depolama**

    Kuyruk depolama, iş akışı işleme ve bulut Hizmetleri bileşenleri arasındaki iletişim için güvenilir Mesajlaşma sağlar.

Bir Azure yığın depolama Azure yığın depolama Hizmetleri'nde erişmenizi sağlayan güvenli bir hesap hesabıdır. Depolama hesabınız depolama kaynaklarınız için benzersiz ad alanı sağlar. Aşağıdaki diyagramda bir depolama hesabı Azure yığın depolama kaynakları arasındaki ilişkiler gösterilmektedir:

![Azure yığın depolama genel bakış](media/azure-stack-storage-overview/AzureStackStorageOverview.png)

### <a name="blob-storage"></a>Blob depolama

Bulutta depolamak kullanıcılar için büyük miktarda yapılandırılmamış nesne veri, blob storage etkili ve ölçeklenebilir bir çözüm sunar. İçeriği depolamak için blob depolama kullanabilirsiniz:

- Belgeler
- Fotoğraf, video, müzik ve blog gibi sosyal veriler
- Dosyaların, bilgisayarların, veritabanlarının ve cihazların yedekleri
- Web uygulamaları için görüntüler ve metinler
- Bulut uygulamaları için yapılandırma verileri
- Günlükler ve diğer büyük veri kümeleri gibi büyük veriler

Her blob bir kapsayıcı halinde düzenlenmiştir. Kapsayıcılar ayrıca nesne gruplarına güvenlik ilkeleri atamaya ilişkin kullanışlı bir yöntem sunar. Bir kapsayıcı herhangi bir sayıda depolama hesabının sınırını dolduracak kadar BLOB içerebilir ve bir depolama hesabı kapsayıcıların herhangi bir sayı içerebilir.

BLOB storage üç tür BLOB sunar:

- **Blok blobları**

    Blok blobları, akış ve bulut nesnelerini depolamak için en iyi duruma getirilir ve belgeler, ortam dosyaları, yedeklemeler ve vb. depolamak için iyi bir seçimdir.

- **Ekleme blobları**

    Ekleme blobları blok bloblarına benzer ancak ekleme işlemleri için en iyi duruma getirilmiştir. Bir ekleme blobu yalnızca sonuna yeni bir blok eklenerek güncelleştirilebilir. Ekleme blobları, yeni verilerin yalnızca blobun sonuna yazılması gereken günlüğe kaydetme gibi senaryolar için iyi bir seçenektir.

- **Sayfa BLOB'ları**

    Sayfa blobları Iaas disklerini temsil etmek için en iyi duruma getirilir ve rastgele destekleme boyutu 1 TB'ye kadar olan yazar. Bir Azure yığın sanal makineye disk bir sayfa blob'u olarak kaydedilen bir vhd'dir Iaas eklendi.

### <a name="table-storage"></a>Table Storage

Modern uygulamalar genellikle eski nesil yazılımların gerektirdiğinden daha fazla ölçeklenebilirlik ve esneklik özelliklerine sahip veri depoları gerektirir. Table Storage yüksek seviyede kullanılabilir ve ölçeklenebilir depolama sunar, böylece uygulamanız kullanıcı taleplerini karşılayacak şekilde otomatik olarak ölçeklendirilir. Table storage Microsoft'un NoSQL anahtar/öznitelik deposudur--geleneksel ilişkisel veritabanlarından farklı yapma şemasız bir tasarıma sahiptir. Şemasız veri deposu sayesinde, uygulamanızın ihtiyaçları geliştikçe verilerinizi uyarlamak da kolaylaşır. Table Storage’ın kullanımı son derece kolaydır, böylece geliştiriciler uygulamalarını hızla geliştirebilir.

Table storage bir tablodaki her değer bir belirlenmiş bir özellik adıyla depolandığı anlamına gelir anahtar öznitelik deposudur. Özellik adı filtreleme ve seçim kriterlerinin belirlenmesi için kullanılabilir. Özellik ve değerlerinin toplamı bir varlığı oluşturur. Table storage şemasız olduğu iki varlık aynı tablodaki farklı özellik koleksiyonları içerebilir ve bu özellikler farklı türde olabilir.

Tablo depolama web uygulamaları, adres defterleri, cihaz bilgilerini ve başka türde bir hizmetiniz için gerekli meta veriler için kullanıcı verileri gibi esnek veri kümelerini depolamak için kullanabilirsiniz. Bugünün Internet tabanlı uygulamaları için table storage gibi NoSQL veritabanları geleneksel ilişkisel veritabanları için popüler bir alternatif sunar.

Herhangi bir sayıda varlıklar, depolama hesabının kapasite sınırına kadar tablo içerebilir ve bir depolama hesabı tabloları herhangi bir sayıda içerebilir.

### <a name="queue-storage"></a>Kuyruk depolama

Ölçeklendirmek üzere uygulama tasarlarken, uygulama bileşenleri birbirinden bağımsız şekilde ölçeklenebilmek için genellikle birbirinden ayrılır. Bulutta, masaüstünde, bir şirket içi sunucu veya bir mobil cihaz çalıştırdıkları olup olmadığını kuyruk depolama uygulama bileşenleri arasında zaman uyumsuz iletişim için güvenilir bir Mesajlaşma çözümü sağlar. Kuyruk depolama zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını da destekler.

Bir sıranın depolama hesabının kapasite sınırını kadar ileti herhangi bir sayı içerebilir ve bir depolama hesabı sıraların herhangi bir sayı içerebilir. Tek bir ileti boyut olarak en fazla 64 KB olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure tutarlı Depolama: farklar ve dikkat edilmesi gerekenler](azure-stack-acs-differences.md)

- Azure Storage hakkında daha fazla bilgi için bkz: [Microsoft Azure Storage'a giriş](../../storage/common/storage-introduction.md)
