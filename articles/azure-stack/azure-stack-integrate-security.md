---
title: Azure Stack veri merkezi tümleştirmesi - güvenlik
description: Azure Stack güvenliği ile Veri Merkezi güvenlik tümleştirmeyi öğrenin
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 02/28/2018
ms.author: jeffgilb
ms.reviewer: wfayed
keywords: ''
ms.openlocfilehash: 9f356b814ac1ac6ca8b6d6efe7cb9f5d9ed66270
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39442484"
---
# <a name="azure-stack-datacenter-integration---security"></a>Azure Stack veri merkezi tümleştirmesi - güvenlik
Azure Stack tasarlanmış ve güvenlikten ödün üretilmiştir. Azure Stack kilitlenmiş sistem olduğundan yazılım güvenlik aracı yüklemesi desteklenmiyor.

Bu makale, Azure yığını'nın güvenlik özellikleri ile veri merkezinizde zaten dağıtılmış güvenlik çözümlerini tümleştirmenize yardımcı olur.

## <a name="security-logs"></a>Güvenlik günlükleri

Azure Stack, iki dakikada bir işletim sistemi ve altyapı rollerini ve ölçek birimi düğümleri için güvenlik olaylarını toplar. Günlükler, depolama hesabının blob kapsayıcılarda depolanır.

Altyapı rol başına bir depolama hesabı ve tüm genel işletim sistemi olaylar için bir genel depolama hesabı yok.

Sistem kaynak sağlayıcısı, blob kapsayıcısına URL'sini almak için REST protokolü aracılığıyla çağrılabilir. Üçüncü taraf güvenlik çözümleri API ve depolama hesapları, olayları işleme almak için kullanabilirsiniz.

### <a name="use-azure-storage-explorer-to-view-events"></a>Olayları görüntülemek için Azure Depolama Gezgini'ni kullanma

Azure Stack kullanarak Azure Depolama Gezgini adında bir araç tarafından toplanan olayları alabilirsiniz. Azure Depolama Gezgini'nden indirebileceğiniz [ http://storageexplorer.com ](http://storageexplorer.com).

Aşağıdaki yordam, Azure Depolama Gezgini'ni Azure Stack için yapılandırmak için kullanabileceğiniz bir örnektir:

1. Operatör Azure Stack Yönetici portalında oturum açın.
1. Gözat **depolama hesapları** ve Ara **frphealthaccount**. **Frphealthaccount** hesabı, tüm işletim sistemi olayları depolamak için kullanılan genel depolama hesabıdır.

   ![Depolama hesapları](media/azure-stack-integrate-security/storage-accounts.png)

1. Seçin **frphealthaccount**, ardından **erişim anahtarlarını**.

   ![Erişim tuşları](media/azure-stack-integrate-security/access-keys.png)

1. Erişim anahtarı panonuza kopyalayın.
1. Azure Depolama Gezgini'ni açın.
1. Üzerinde **Düzenle** menüsünde **hedef Azure Stack**.
1. Seçin **hesabı Ekle**ve ardından **bir depolama hesabı adı ve anahtarı kullan**.

   ![Depolama birimini bağlayın](media/azure-stack-integrate-security/connect-storage.png)

1. **İleri**’ye tıklayın.
1. Üzerinde **dış depolama Ekle** sayfası:

   a. Hesap adını yazın **frphealthaccount**.

   b. Depolama hesabı erişim anahtarını yapıştırın.

   c. Altında **depolama uç noktaları etki alanı**seçin **diğer**ve depolama uç noktasını belirtin **[Bölge]. [ DomainName]**.

   d. Seçin **HTTP kullan** onay kutusu.

   ![Dış depolama Ekle](media/azure-stack-integrate-security/attach-storage.png)

1. Tıklayın **sonraki**, özeti gözden geçirin ve **son** Sihirbazı.
1. Artık tek tek blob kapsayıcıları göz atabilir ve olayları indirin.

   ![Blob'lara göz at](media/azure-stack-integrate-security/browse-blob.png)

### <a name="use-programming-languages-to-access-events"></a>Programlama dili için erişim olaylarını kullanın

Çeşitli programlama dilleri, bir depolama hesabına erişmek için kullanabilirsiniz. Dilinizi eşleşen örnek seçmek için aşağıdaki belgeleri kullanın:

[https://azure.microsoft.com/resources/samples/?term=storage+account](https://azure.microsoft.com/resources/samples/?term=storage+account)

## <a name="device-access-auditing"></a>Cihaz erişimini denetleme

Azure stack'teki tüm fiziksel cihazlar TACACS veya RADIUS kullanımını destekler. Bu, ağ anahtarlarını ve temel kart yönetim denetleyicisine (BMC) erişim içerir.

Azure Stack çözümleri, RADIUS veya TACACS yerleşik olarak bulunmaz. Ancak, çözümler piyasadaki mevcut RADIUS veya TACACS çözüm kullanımını desteklemek üzere doğrulandı.

RADIUS için yalnızca MSCHAPv2 doğrulandı. Bu, RADIUS kullanan en güvenli uygulamasını temsil eder.
Azure Stack çözümünüzle birlikte dahil edilen cihazlar TACAS veya RADIUS etkinleştirmek için OEM donanım satıcınıza başvurun.

## <a name="syslog"></a>Syslog

Azure stack'teki tüm fiziksel cihazlar, Syslog iletileri gönderebilir. Syslog sunucusu ile Azure Stack çözümleri bulunmaz. Ancak, çözümler piyasadaki mevcut Syslog çözümleri iletileri gönderme desteklemek üzere doğrulandı.

İsteğe bağlı parametresi dağıtım için toplanan Syslog hedef adresidir ancak dağıtım sonrası da eklenebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Hizmet İlkesi](azure-stack-servicing-policy.md)
