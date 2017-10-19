---
title: "Güvenlik ürünlerinizi Operations Management Suite (OMS) Güvenlik ve Denetim Çözümü’ne bağlama | Microsoft Belgeleri"
description: "Bu belge, Common Event Format’ı kullanarak güvenlik ürünlerinizi Operations Management Suite Güvenlik ve Denetim Çözümü’ne bağlamanıza yardımcı olur."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 46eee484-e078-4bad-8c89-c88a3508f6aa
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.openlocfilehash: 710a1fe0ce2b7a1841187cf75f4ffb090cc161e5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connecting-your-security-products-to-the-operations-management-suite-oms-security-and-audit-solution"></a>Güvenlik ürünlerinizi Operations Management Suite (OMS) Güvenlik ve Denetim Çözümü’ne bağlama 
Bu belge, güvenlik ürünlerinizi OMS Güvenlik ve Denetim Çözümü’ne bağlamanıza yardımcı olur. Aşağıdaki kaynaklar desteklenir:

- Common Event Format (CEF) olayları
- Cisco ASA olayları


## <a name="what-is-cef"></a>CEF nedir?
Common Event Format (CEF), Syslog iletilerine ek olarak sektörde standart haline gelmiş, farklı platformlar arasında olay birlikte çalışabilirliği sağlamak için birçok güvenlik hizmeti satıcısı tarafından kullanılan bir biçimdir. OMS Güvenlik ve Denetim Çözümü CEF kullanarak veri alımını destekler ve bu sayede güvenlik ürünlerinizi OMS Security’ye bağlayabilirsiniz. 

Veri kaynağınızı OMS’ye bağlayarak bu platformun birer parçası olan aşağıdaki özelliklerden yararlanma olanağına sahip olursunuz:

- Arama ve Bağıntı
- Denetim
- Uyarı
- Tehdit Bilgisi
- Önemli sorunlar

## <a name="collection-of-security-solution-logs"></a>Güvenlik çözümü günlüklerinin toplanması

OMS Security, CEF kullanarak Syslog’lar ve [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/) günlükleri üzerinden günlüklerin toplanmasını destekler. Bu örnekte, kaynak (günlükleri oluşturan bilgisayar) syslog-ng daemon çalıştıran bir Linux bilgisayarı, hedef ise OMS Security’dir. Linux bilgisayarı hazırlamak için aşağıdaki görevleri gerçekleştirmeniz gerekir:

- Linux için OMS Aracısı’nın 1.2.0-25 veya üzeri bir sürümünü indirin.
- Aracıyı yüklemek ve çalışma alanınıza eklemek için [bu makalenin](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) **Hızlı Yükleme Kılavuzu** bölümünü izleyin.

Aracı genellikle günlüklerin oluşturulduğu bilgisayardan farklı bir bilgisayara yüklenir. Günlüklerin aracı makineye iletilmesi genellikle aşağıdaki adımları gerektirir:

- Günlük tutan ürünü/makineyi gerekli olayları aracı makinedeki syslog daemon’a (rsyslog veya syslog-ng) iletecek şekilde yapılandırın.
- Aracı makinede syslog daemon’u uzak bir sistemden gönderilen iletileri alacak şekilde yapılandırın.

Aracı makinede olayların syslog daemon’dan yerel 25226 numaralı yerel UDP bağlantı noktasına gönderilmesi gerekir. Aracı bu bağlantı noktasında gelen olayları dinler. Aşağıda, tüm olayları yerel sistemden aracıya göndermeye yönelik örnek bir yapılandırma verilmiştir (yapılandırmayı yerel ayarlarınıza uyacak şekilde değiştirebilirsiniz):

1. Terminal penceresini açıp */etc/syslog-ng/* dizinine gidin 
2. Yeni bir dosya (*security-config-omsagent.conf*) oluşturun ve şu içeriği ekleyin:  OMS_facility = local4
    
    filter f_local4_oms { facility(local4); };

    destination security_oms { tcp("127.0.0.1" port(25226)); };

    log { source(src); filter(f_local4_oms); destination(security_oms); };
    
3. OMS Aracısı bilgisayarda *security_events.conf* adlı dosyayı indirip */etc/opt/microsoft/omsagent/conf/omsagent.d/* dizinine ekleyin.
4. Syslog daemon'u başlatmak için aşağıdaki komutu yazın: *For syslog-ng run:*
    
    ```
    sudo service rsyslog restart
    ```

    *For rsyslog run:*
    
    ```
    /etc/init.d/syslog-ng restart
    ```
5. OMS Aracısı’nı başlatmak için aşağıdaki komutu yazın:

    *For syslog-ng run:*
    
    ```
    sudo service omsagent restart
    ```

    *For rsyslog run:*
    
    ```
    systemctl restart omsagent
    ```
6. Aşağıdaki komutu yazın ve sonucu gözden geçirerek OMS Aracısı günlüğünde bir hata olmadığını doğrulayın:

    ``` 
    tail /var/opt/microsoft/omsagent/log/omsagent.log
    ```

## <a name="reviewing-collected-security-events"></a>Toplanan güvenlik olaylarını gözden geçirme

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

Yapılandırma tamamlandıktan sonra güvenlik olayı OMS Security tarafından alınmaya başlanır. Bu olayları görselleştirmek için Günlük Arama’yı açıp arama alanına *Type=CommonSecurityLog* komutunu yazın ve ENTER tuşuna basın. Aşağıdaki örnekte bu komutun sonucu gösterilmiştir; bu durumda OMS Security’nin zaten birden çok satıcıdan güvenlik günlükleri aldığını fark edebilirsiniz:
   
![OMS Güvenlik ve Denetim Temeli Değerlendirmesi](./media/oms-security-connect-products/oms-security-connect-products-fig1.png)

Bu aramayı tek bir satıcıyı yansıtacak şekilde daraltabilirsiniz. Örneğin, çevrimiçi Cisco günlüklerini görselleştirmek için şunu yazın: *Type=CommonSecurityLog DeviceVendor=Cisco*. “CommonSecurityLog”da temel uzantıları içeren tüm CEF üst bilgileri için önceden tanımlanmış alanlar vardır ve “Özel Uzantı” olup olmadığından bağımsız olarak diğer tüm uzantılar "AdditionalExtensions" alanına eklenir. Özel Alanlar özelliğini kullanarak adanmış alanlar elde edebilirsiniz. 

### <a name="accessing-computers-missing-baseline-assessment"></a>Temel değerlendirmesi eksik bilgisayarlara erişim
OMS, Windows Server 2008 R2'den Windows Server 2012 R2'ye kadar etki alanı üyesi temel profilini destekler. Windows Server 2016 temeli, henüz tamamlanmamıştır ve yayımlandıktan hemen sonra eklenecektir. OMS Güvenlik ve Denetim temeli değerlendirmesiyle taranan tüm diğer işletim sistemleri **Temel değerlendirmesi eksik bilgisayarlar** bölümünde görünür.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, CEF çözümünüzü OMS’ye nasıl bağlayacağınızı öğrendiniz. OMS Güvenlik hakkında daha fazla bilgi edinmek için şu makalelere göz atın:

* [Operations Management Suite'e (OMS) genel bakış](operations-management-suite-overview.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Güvenlik Uyarılarını İzleme ve Yanıtlama](oms-security-responding-alerts.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Kaynakları İzleme](oms-security-monitoring-resources.md)

