---
title: "Azure veri Kataloğu sürüm notları | Microsoft Docs"
description: "Azure veri kataloğu için sürüm notları."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 3aca9c49-45a4-4352-92e6-bd25ee3eacf7
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: d3db9bee0558cac5fb4f5b8fb4ab67a35ce0f141
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-data-catalog-release-notes"></a>Azure veri Kataloğu sürüm notları
## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Sürüm Notları 20 Kasım 2015 için Azure veri Kataloğu
### <a name="opening-data-sources-in-power-bi-desktop"></a>Power BI Desktop'ta açılış veri kaynakları
"Açık olarak Power BI Desktop" seçeneğini kullanırken **Azure veri Kataloğu** portalı, kullanıcıların Power BI Desktop uygulamada iki sorunlardan biri karşılaşabilirsiniz:

* "Oluşturulamıyor açık belgeye" başlıklı bir iletişim kutusu görüntülenir
* Power BI Desktop uygulamasını açar, ancak dosya boş gibi görünüyor

Her durum için Power BI Desktop'ın en son sürümünü indirip sorun çözülebilir [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Sürüm Notları 13 Kasım 2015 için Azure veri Kataloğu
### <a name="registering-and-connecting-to-teradata"></a>Kaydetme ve Teradata için bağlanma
Teradata veri kaynakları kullanıcıları bağlanırken kullanılan yazılım verileri (32 bit veya 64 bit) eşleşen doğru Teradata ODBC sürücüsü yüklü olmalıdır.

Bu en son ADC yayın tarihi itibariyle [windows (sürüm 15.10) için Teradata ODBC sürücüsü](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) uyumlu Office 2013, ancak Office 2016 ile değil.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Sürüm Notları 13 Temmuz 2015 için Azure veri Kataloğu
### <a name="registering-and-connecting-to-oracle-database"></a>Kaydetme ve Oracle veritabanına bağlanma
Bağlanırken Oracle veritabanı veri kaynakları kullanıcıları kullanılan yazılım verileri (32 bit veya 64 bit) ile eşleşen doğru Oracle sürücüleri yüklü olmalıdır.

* 32 bit Windows çalıştıran bir bilgisayarda Oracle veri kaynaklarını kaydederek, 32-bit Oracle sürücüler kullanılır
* 64 bit Windows çalıştıran bir bilgisayarda Oracle veri kaynaklarını kaydederek, 64-bit Oracle sürücüler kullanılır
* Microsoft Office'in 32 bit sürümünü çalıştıran bir bilgisayarda Excel kullanarak Oracle veri kaynaklarına bağlanırken, 64 bit Windows'ta dahil olmak üzere 32-bit Oracle sürücüler kullanılır
* Microsoft Office 64-bit sürümünü çalıştıran bir bilgisayarda Excel kullanarak Oracle veri kaynaklarına bağlanırken 64-bit Oracle sürücüler kullanılır

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Kaydetme ve SQL Server Raporlama Hizmetleri'ne bağlanma
SQL Server Reporting Services (SSRS) veri kaynakları için destek, yalnızca yerel mod sunucular için şu anda sınırlıdır. SSRS desteği SharePoint modunda daha sonraki bir sürümde eklenecek.

### <a name="opening-data-assets-in-excel"></a>Excel açılırken veri varlıklarını
Microsoft Excel'den veri varlıklarını açılırken **Azure veri Kataloğu** portal, kullanıcılar ile istenebilir bir **Microsoft Excel Güvenlik Bildirimi** iletişim kutusu. Bu standart, beklenen davranıştır ve kullanıcılar seçebilir **etkinleştirmek** devam etmek için.

Daha fazla bilgi için bkz: [etkinleştirmek veya devre dışı bağlantılar ve şüpheli Web sitelerine dosyalarından ilgili güvenlik uyarıları](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Proxy ve ilke yapılandırması ve veri kaynağı kaydı
Kullanıcılar nerede Azure veri Kataloğu portalına, ancak bunları oturum açmasını engelleyen bir hata iletisi karşılaştıkları veri kaynağı kayıt aracını oturum açmaya çalıştığınızda oturum durumlar yaşayabilirsiniz.

Bu sorunu davranış iki olası nedenleri şunlardır:

**1. neden: Active Directory Federasyon Hizmetleri Yapılandırma** veri kaynağı kayıt aracını kullanıcı oturumları Active Directory karşı doğrulamak için form kimlik doğrulaması kullanır. Başarılı oturum açma işlemi için form kimlik doğrulaması Active Directory yönetici tarafından genel kimlik doğrulama ilkesinde etkinleştirilmesi gerekir.

Bazı durumlarda, yalnızca kullanıcının şirket ağında olmadığında veya yalnızca kullanıcı gelen şirket ağının dışından bağlanırken, bu hata davranış ortaya çıkabilir. Genel kimlik doğrulama ilkesini intranet ve extranet bağlantıları için ayrı ayrı etkinleştirilmesi için kimlik doğrulama yöntemlerini sağlar. Form kimlik doğrulaması için kullanıcı bağlanma ağ etkin değilse oturum açma hataları oluşabilir.

Daha fazla bilgi için bkz: [kimlik doğrulaması ilkelerini yapılandırma](https://technet.microsoft.com/library/dn486781.aspx).

**2. neden: Ağ proxy yapılandırmasını** kurumsal ağ proxy sunucusu kullanıyorsa, kayıt aracı proxy üzerinden Azure Active Directory'ye bağlanamıyor olmayabilir. Kullanıcılar, kayıt aracı aracın yapılandırma dosyasını düzenleyerek bu bölümde dosyasına ekleyerek sağlayabilirsiniz:

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


RegistrationTool.exe.config dosyayı bulmak için kayıt aracını başlatın ve sonra Windows Görev Yöneticisi yardımcı programını açın. Görev Yöneticisi'nde ayrıntıları sekmesinde RegistrationTool.exe üzerinde sağ tıklayın ve açılan menüden dosya konumunu Aç'ı seçin.
