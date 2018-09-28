---
title: Azure veri Kataloğu sürüm notları
description: Azure veri kataloğu için sürüm notları.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: 3aca9c49-45a4-4352-92e6-bd25ee3eacf7
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 12b8161cc5845bca749c34188835cef1d92b299a
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47404560"
---
# <a name="azure-data-catalog-release-notes"></a>Azure veri Kataloğu sürüm notları
## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Azure veri Kataloğu sürüm notları için 20 Kasım 2015
### <a name="opening-data-sources-in-power-bi-desktop"></a>Power BI Desktop açılırken veri kaynakları
"Açık Power BI Desktop'ta" seçeneğini kullanırken **Azure veri Kataloğu** portalı, kullanıcıların Power BI Desktop uygulamasında iki sorunlarından biriyle karşılaşabilirsiniz:

* "Alınamıyor açık belgeye" başlıklı bir iletişim kutusu görüntülenir
* Power BI Desktop uygulaması açılır, ancak dosya boş gibi görünüyor

Her durumda, indirip Power BI Desktop'ın en son sürümünü yükleme sorunu çözülebilir [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Azure veri Kataloğu sürüm notları için 13 Kasım 2015
### <a name="registering-and-connecting-to-teradata"></a>Kaydetme ve Teradata için bağlanma
Teradata veri kaynakları bağlanırken kullanılan yazılım bit genişliği (32 bit veya 64-bit) eşleşen doğru Teradata ODBC sürücüsü yüklü olmalıdır.

Bu en son ADC yayın tarihi itibariyle [windows (sürüm 15.10) için Teradata ODBC sürücüsü](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) uyumlu Office 2013, ancak Office 2016 ile değil.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Azure veri Kataloğu sürüm notları için 13 Temmuz 2015
### <a name="registering-and-connecting-to-oracle-database"></a>Oracle veritabanına bağlanma ve kaydetme
Oracle Database veri kaynakları bağlanırken kullanılan yazılım bit genişliği (32 bit veya 64-bit) eşleşen doğru Oracle sürücüleri yüklü olmalıdır.

* Oracle 32-bit Windows çalıştıran bir bilgisayardaki veri kaynaklarını kaydederek, 32 bit Oracle sürücüleri kullanılacak
* 64 bit Windows çalıştıran bir bilgisayarda Oracle veri kaynaklarını kaydederek, 64-bit Oracle sürücüleri kullanılacak
* Microsoft Office'in 32 bit sürümünü çalıştıran bir bilgisayarda Excel kullanarak Oracle veri kaynaklarına bağlanırken, 64 bit Windows üzerinde de dahil olmak üzere 32 bit Oracle sürücüleri kullanılacak
* Microsoft Office'in 64 bit sürümünü çalıştıran bir bilgisayarda Excel kullanarak Oracle veri kaynaklarına bağlanırken, 64-bit Oracle sürücüleri kullanılacak

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Kaydetme ve SQL Server Raporlama Hizmetleri'ne bağlanma
SQL Server Raporlama Hizmetleri (SSRS) veri kaynakları için destek, şu anda yalnızca yerel mod sunucular için sınırlıdır. Bir sonraki sürümde SSRS SharePoint modu desteği eklenecektir.

### <a name="opening-data-assets-in-excel"></a>Excel'de açılırken veri varlıkları
Veri varlıklarını Microsoft Excel'de açılırken **Azure veri Kataloğu** portal, kullanıcılar ile istenebilir bir **Microsoft Excel Güvenlik Bildirimi** iletişim kutusu. Bu standart, Beklenen davranış ve kullanıcıların seçebilirsiniz **etkinleştirme** devam etmek için.

Daha fazla bilgi için [etkinleştirmek veya devre dışı bağlantıların ve şüpheli Web sitelerine dosyalarından ilgili güvenlik uyarıları](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Proxy ve ilke yapılandırması ve veri kaynağı kaydı
Kullanıcılar nerede Azure veri Kataloğu portalı, ancak bunları oturum açmasını engelleyen bir hata iletisi karşılaştıkları veri kaynağı kayıt aracını oturum açmaya çalıştığınızda oturum bir durumla karşılaşabilirsiniz.

Bu sorun davranışı için iki olası nedeni vardır:

**1. neden: Active Directory Federasyon Hizmetleri Yapılandırma** veri kaynağı kayıt aracını, kullanıcı oturum açma işlemi Active Directory karşı doğrulamak için form kimlik doğrulaması kullanır. Başarılı oturum açma için bir Active Directory Yöneticisi tarafından genel kimlik doğrulama ilkesinin form kimlik doğrulaması etkinleştirilmelidir.

Bazı durumlarda, kullanıcının yalnızca şirket ağında olduğunda ya da yalnızca kullanıcı şirket ağının dışından bağlantı, bu hata davranış ortaya çıkabilir. Genel kimlik doğrulama ilkesi kimlik doğrulama yöntemleri, intranet ve extranet bağlantıları için ayrı ayrı etkinleştirilmesini sağlar. Form kimlik doğrulaması, kullanıcı bağlanan ağ için etkin değilse oturum açma hataları oluşabilir.

Daha fazla bilgi için [kimlik doğrulama ilkelerini yapılandırma](https://technet.microsoft.com/library/dn486781.aspx).

**2. neden: Ağ proxy yapılandırmasını** kurumsal ağ proxy sunucusu kullanıyorsa, kayıt aracı proxy üzerinden Azure Active Directory'ye bağlanmak mümkün olmayabilir. Kullanıcıların kayıt aracı aracının yapılandırma dosyasını düzenleyerek bu bölümde, dosyaya ekleme emin olabilirsiniz:

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


RegistrationTool.exe.config dosyayı bulmak için kayıt aracını başlatın ve ardından Windows Görev Yöneticisi yardımcı programını açın. Görev Yöneticisi'nde Ayrıntılar sekmesinde RegistrationTool.exe üzerinde sağ tıklayın ve açılan menüden dosya konumunu Aç'ı seçin.
