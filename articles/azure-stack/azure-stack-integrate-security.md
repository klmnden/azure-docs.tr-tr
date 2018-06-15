---
title: Azure yığın datacenter tümleştirmesi - güvenlik
description: Azure yığın güvenlik, veri merkezi güvenliği tümleştirmek öğrenin
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 02/28/2018
ms.author: jeffgilb
ms.reviewer: wfayed
keywords: ''
ms.openlocfilehash: 8ce9045a3e4fd12d61e9b1600ee98880762bc544
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
ms.locfileid: "29734436"
---
# <a name="azure-stack-datacenter-integration---security"></a>Azure yığın datacenter tümleştirmesi - güvenlik
Azure yığın tasarlanmış ve güvenlik göz önünde bulundurularak ile yapılandırılır. Azure yığın kilitli sistem olduğundan, yazılım güvenlik aracı yükleme desteklenmiyor.

Bu makale Azure yığın güvenlik özellikleri, veri merkezinizde zaten dağıtılmış güvenlik çözümleri ile tümleştirmenize yardımcı olur.

## <a name="security-logs"></a>Güvenlik günlükleri

Azure yığını, her iki dakikada işletim sistemi ve altyapı rollerini ve ölçek birimi düğümler için güvenlik olaylarını toplar. Günlükler, depolama hesabı blob kapsayıcı depolanır.

Altyapı rol başına bir depolama hesabı ve tüm tipik işletim sistemi olaylar için bir genel depolama hesabı yok.

Sistem kaynak sağlayıcısı blob kapsayıcısını URL'sini almak için REST protokolü aracılığıyla çağrılabilir. Üçüncü taraf güvenlik çözümleri API ve depolama hesapları olayları işleme almak için kullanabilirsiniz.

### <a name="use-azure-storage-explorer-to-view-events"></a>Azure Storage Gezgini olayları görüntülemek için kullanın

Azure Storage Gezgini adlı bir aracı kullanarak Azure yığını tarafından toplanan olayları alabilirsiniz. Azure Depolama Gezgini'nden indirebilirsiniz [http://storageexplorer.com](http://storageexplorer.com).

Aşağıdaki yordamda Azure yığını için Azure Storage Gezgini yapılandırmak için kullanabileceğiniz bir örnek verilmiştir:

1. Azure yığın Yönetici portalı'na bir operatör olarak oturum açın.
2. Gözat **depolama hesapları** ve Ara **frphealthaccount**. **Frphealthaccount** hesabı tüm işletim sistemi olayları depolamak için kullanılan genel depolama hesabıdır.

   ![Depolama hesapları](media/azure-stack-integrate-security/storage-accounts.png)

3. Seçin **frphealthaccount**, ardından **erişim tuşları**.

   ![Erişim tuşları](media/azure-stack-integrate-security/access-keys.png)

4. Erişim anahtarı panonuza kopyalayın.
5. Azure Depolama Gezgini'ni açın.
6. Üzerinde **Düzenle** menüsünde, select **hedef Azure yığın**.
7. Seçin **hesabı Ekle**ve ardından **bir depolama hesabı adı ve anahtar kullanmak**.

   ![Depolama birimini bağlayın](media/azure-stack-integrate-security/connect-storage.png)

8. **İleri**’ye tıklayın.
9. Üzerinde **harici depolama ekleme** sayfa:

   a. Hesap adını yazın **frphealthaccount**.

   b. Depolama hesabının erişim anahtarı yapıştırın.

   c. Altında **depolama uç noktaları etki alanı**seçin **diğer**ve depolama uç nokta belirtin **[Bölge]. [ DomainName]**.

   d. Seçin **HTTP kullan** onay kutusu.

   ![Harici depolama ekleme](media/azure-stack-integrate-security/attach-storage.png)

10. Tıklatın **sonraki**, özeti gözden geçirin ve **son** Sihirbazı.
11. Şimdi, tek tek blob kapsayıcıları göz atın ve olayları indirin.

   ![BLOB'ları Gözat](media/azure-stack-integrate-security/browse-blob.png)

### <a name="use-programming-languages-to-access-events"></a>Programlama dili erişim olaylar için kullanın

Bir depolama hesabına erişmek için çeşitli programlama dillerini kullanabilirsiniz. Dilinizi eşleşen bir örnek seçmek için aşağıdaki belgeleri kullanın:

[https://azure.microsoft.com/resources/samples/?term=storage+account](https://azure.microsoft.com/resources/samples/?term=storage+account)

## <a name="device-access-auditing"></a>Aygıt erişim denetimi

Tüm fiziksel cihazlar Azure yığınında TACACS ya da RADIUS kullanımını destekler. Temel kart yönetim denetleyicisi (BMC) ve ağ anahtarları erişim de buna dahildir.

Azure yığın çözümleri, RADIUS veya yerleşik TACACS bulunmaz. Ancak, çözümleri pazarında varolan RADIUS veya TACACS çözümleri kullanılabilir kullanımını desteklemek için doğrulandı.

Yalnızca RADIUS için MSCHAPv2 doğrulandı. Bu, RADIUS kullanan en güvenli uygulamasını temsil eder.
Azure yığın çözümünüzle birlikte bulunan aygıtları TACAS veya RADIUS etkinleştirmek için OEM donanım satıcınıza başvurun.

## <a name="syslog"></a>Syslog

Tüm fiziksel cihazlar Azure yığınında Syslog iletileri gönderebilir. Azure yığın çözümleri Syslog sunucusuyla bulunmaz. Ancak, varolan Syslog çözümleri iletileri gönderme pazarında desteklemek için çözümler doğrulandı.

Syslog hedef adresi için dağıtım toplanan isteğe bağlı bir parametredir, ancak dağıtım sonrası da eklenebilir.

## <a name="next-steps"></a>Sonraki adımlar

[İlke Bakımı](azure-stack-servicing-policy.md)
