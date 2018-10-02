---
title: Azure Stack depolamaya giriş
description: Azure Stack depolama hakkında bilgi edinin
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
ms.date: 09/28/2018
ms.author: mabrigg
ms.openlocfilehash: 13fdf3257ed44212f45eeb3d2820a2022f54d777
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585247"
---
# <a name="introduction-to-azure-stack-storage"></a>Azure Stack depolamaya giriş

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

## <a name="overview"></a>Genel Bakış

Azure Stack depolama Blobları, tablolar ve Azure depolama hizmetleriyle tutarlı olan kuyrukları içeren bir bulut depolama hizmetleri kümesidir.

## <a name="azure-stack-storage-services"></a>Azure Stack depolama hizmetleri

Azure Stack depolama, aşağıdaki üç hizmeti sunar:

- **Blob Depolama**

    BLOB Depolama, yapılandırılmamış nesne verilerini depolar. Blob; bir belge, ortam dosyası veya uygulama yükleyici gibi herhangi bir türde metin veya ikili veri olabilir.

- **Tablo depolama**

    Tablo depolama, yapılandırılmış veri kümelerini depolar. Table Storage, yüksek miktarda verinin hızla dağıtılmasını ve verilere hızla erişilebilmesini sağlayan NoSQL anahtar özniteliği veri deposudur.

- **Kuyruk depolama**

    Kuyruk depolama, iş akışı işlemeye ve bulut hizmetlerinin bileşenleri arasında iletişime yönelik güvenilir Mesajlaşma sağlar.

Bir Azure Stack depolama hesabı, Azure Stack depolama hizmetleri sunan güvenli bir hesap erişim ' dir. Depolama hesabınız depolama kaynaklarınız için benzersiz ad alanı sağlar. Aşağıdaki diyagramda bir depolama hesabındaki Azure Stack depolama kaynakları arasındaki ilişkiler gösterilmektedir:

![Azure Stack depolamaya genel bakış](media/azure-stack-storage-overview/AzureStackStorageOverview.png)

### <a name="blob-storage"></a>Blob depolama

Blob depolama, bulutta depolamak kullanıcılar için büyük miktarda yapılandırılmamış nesne verilerini, etkili ve ölçeklenebilir bir çözüm sunar. Blob depolama gibi içerik depolamak için kullanabilirsiniz:

- Belgeler
- Fotoğraf, video, müzik ve blog gibi sosyal veriler
- Dosyaların, bilgisayarların, veritabanlarının ve cihazların yedekleri
- Web uygulamaları için görüntüler ve metinler
- Bulut uygulamaları için yapılandırma verileri
- Günlükler ve diğer büyük veri kümeleri gibi büyük veriler

Her blob bir kapsayıcı halinde düzenlenmiştir. Kapsayıcılar ayrıca nesne gruplarına güvenlik ilkeleri atamaya ilişkin kullanışlı bir yöntem sunar. Bir depolama hesabında herhangi bir sayıda kapsayıcı olabilir ve bir kapsayıcı herhangi bir sayıda depolama hesabının sınırını dolduracak kadar BLOB içerebilir.

BLOB Depolama üç tür BLOB sunar:

- **Blok blobları**

    Blok blobları, akış ve bulut nesnelerini depolamak için optimize edilmiş ve belgeler, ortam dosyaları, yedeklemeleri ve vb. depolamak için iyi bir seçimdir.

- **Ekleme blobları**

    Ekleme blobları blok bloblarına benzer ancak ekleme işlemleri için en iyi duruma getirilmiştir. Bir ekleme blobu yalnızca sonuna yeni bir blok eklenerek güncelleştirilebilir. Ekleme blobları, yeni verilerin yalnızca blobun sonuna yazılması gereken günlüğe kaydetme gibi senaryolar için iyi bir seçenektir.

- **Sayfa blobları**

    Sayfa blobları Iaas disklerini temsil etmek için optimize edilmiş ve rastgele destekleyen, boyutu 1 TB'ye kadar olan yazar. Bir Azure Stack sanal makine disk bir sayfa blobu depolanan bir vhd'dir Iaas diskine bağlı.

### <a name="table-storage"></a>Table Storage

Modern uygulamalar genellikle eski nesil yazılımların gerektirdiğinden daha fazla ölçeklenebilirlik ve esneklik özelliklerine sahip veri depoları gerektirir. Table Storage yüksek seviyede kullanılabilir ve ölçeklenebilir depolama sunar, böylece uygulamanız kullanıcı taleplerini karşılayacak şekilde otomatik olarak ölçeklendirilir. Tablo depolama, Microsoft'un NoSQL anahtar/öznitelik deposu--geleneksel ilişkisel veritabanlarından farklı olarak şemasız bir tasarıma sahiptir. Şemasız veri deposu sayesinde, uygulamanızın ihtiyaçları geliştikçe verilerinizi uyarlamak da kolaylaşır. Table Storage’ın kullanımı son derece kolaydır, böylece geliştiriciler uygulamalarını hızla geliştirebilir.

Tablo depolama türü belirtilmiş bir özellik adı ile bir tablodaki her değerin saklandığı anlamına gelir bir anahtar öznitelik deposudur. Özellik adı filtreleme ve seçim kriterlerinin belirlenmesi için kullanılabilir. Özellik ve değerlerinin toplamı bir varlığı oluşturur. Table storage şemasız olduğu olduğundan, aynı tablodaki iki varlık farklı özellik koleksiyonları içerebilir ve bu özellikler farklı türde olabilir.

Tablo depolama, web uygulamaları, adres defterleri, cihaz bilgileri ve hizmetiniz için gerekli meta verileri başka bir türü için kullanıcı verileri gibi esnek veri kümelerini depolamak için kullanabilirsiniz. Bugünün Internet tabanlı uygulamaları için table storage gibi NoSQL veritabanları, geleneksel veri tabanlarına göre popüler bir alternatif sunar.

Bir depolama hesabı herhangi bir sayıda tablo içerebilir ve herhangi bir sayıda varlıklar, depolama hesabının kapasite sınırına kadar tablo içerebilir.

### <a name="queue-storage"></a>Kuyruk depolama

Ölçeklendirmek üzere uygulama tasarlarken, uygulama bileşenleri birbirinden bağımsız şekilde ölçeklenebilmek için genellikle birbirinden ayrılır. Kuyruk depolama, uygulama bileşenleri arasında zaman uyumsuz iletişim için güvenilir bir Mesajlaşma çözümü sağlar, ister bulutta, masaüstünde, şirket içi sunucusunda veya mobil bir cihazda çalışıyor olsun. Kuyruk depolama zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını da destekler.

Bir depolama hesabı herhangi sayıda kuyruk içerebilir ve bir kuyruk herhangi bir sayıda depolama hesabının kapasite sınırına kadar ileti içerebilir. Tek bir ileti boyut olarak en fazla 64 KB olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ile tutarlı Depolama: farklılıklar ve dikkat edilmesi gerekenler](azure-stack-acs-differences.md)

- Azure depolama hakkında daha fazla bilgi için bkz: [Microsoft Azure Depolama'ya giriş](../../storage/common/storage-introduction.md)
