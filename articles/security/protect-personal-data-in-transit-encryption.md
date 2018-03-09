---
title: "Kişisel Aktarımdaki verileri şifreleme Azure ile koruma | Microsoft Docs"
description: "kişisel verilerinizi korumak için Azure şifreleme hakkında bilgi genel veri koruma düzenleme (GDPR) ile uyumlu olacak şekilde çalışmalarını yararlı olabilir."
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2018
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 6975358d40206a497a53de16731d16ef374db905
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a>Azure şifreleme teknolojilerini: şifrelemesi ile Aktarım kişisel verileri koruma

Bu makalede anlamak ve Azure şifreleme teknolojilerini verilerin güvenliğini sağlamak için aktarım sırasında kullanmanıza yardımcı olur. Ağ üzerinden geçen gibi kişisel verilerin gizliliği koruma, çok katmanlı savunma güvenlik stratejisinin önemli bir parçasıdır. Şifreleme Aktarımdaki görüntüleyebilir veya verileri kullanma engeller iletimleri karşılar bir saldırganın önlemek için tasarlanmıştır. Bu makalede yer alan bilgileri genel veri koruma düzenleme (GDPR) ile uyum sağlamak için bir kuruluşun çaba yararlı olabilir.

## <a name="scenario"></a>Senaryo

Amerika Birleşik Devletleri'nde yönetim büyük seyahat şirket, masraflarını Akdeniz, Adriatic ve Baltık seas yanı sıra İngiliz Adaları arasında içinde sunmaya işlemlerini genişletmektedir. Bu çalışmalarını desteklemek için daha küçük birkaç seyahat satırlarını İtalya, Almanya, Danimarka ve İngiltere göre aldı 

Şirket Microsoft Azure bulutta şirket verilerini depolamak için kullanır. Bu ad, adres, telefon numaraları gibi kişisel olarak tanımlanabilir bilgileri ve genel müşteri tabanı, kredi kartı bilgilerini içerir. Ayrıca tüm konumlarda adresleri, telefon numaralarını, vergi kimlik numaraları ve şirket çalışanlar hakkında diğer bilgiler gibi geleneksel İnsan Kaynakları bilgileri içerir. Seyahat satır de geçerli ve geçmiş müşterilerle ilişkilerini izlemek için kişisel bilgi içeren büyük bir veritabanı ödül ve bağlılık programı üyelerinin tutar.

Müşteriler, kişisel verileri şirketin şubelere ve seyahat tüm dünyada bulunan aracılardan gelen veritabanında girilir. Müşteri bilgilerini içeren belgeleri, Azure depolama alanına ağ üzerinden aktarılır.

## <a name="problem-statement"></a>Sorun bildirimi

Aktarım için ve Azure hizmetlerinden ederken şirket müşterilerin ve çalışanların kişisel verilerin gizliliği korumanız gerekir.

## <a name="company-goal"></a>Şirket hedefi

Kişisel veriler disk zaman devre dışı şifrelendiğinden emin olmak için şirket hedef. Yetkisiz kişilerin disk kapalı kişisel verilere müdahale okunamaz kılacak bir formda olması gerekir. Şifreleme uygulama kolay ya da kullanıcılar ve Yöneticiler için tamamen saydam olmalıdır.

## <a name="solutions"></a>Çözümler

Birden çok Araçlar ve teknolojiler aktarım kişisel verileri korumaya yardımcı olmak için Azure hizmetleri sağlar.

### <a name="azure-storage"></a>Azure Storage

Bulutta depolanan verileri Azure veri merkezine dünyada fiziksel olarak bulunan herhangi bir yere olabilecek istemciden gitmeniz gerekir. Bu verileri kullanıcılar tarafından alındığında, yeniden ters yönde dolaşır. Aktarımdaki verileri ortak Internet üzerinden saldırganlar tarafından riskinin her zaman altındadır. Konumlar arasında hareket ederken güvenliğini sağlamak için aktarım düzeyinde şifreleme kullanılarak kişisel verilerin gizliliği korumak önemlidir.

HTTPS protokolü, Internet üzerinden güvenli, şifrelenmiş iletişim kanalı sağlar. HTTPS, REST API'leri çağrılırken Azure Storage ve nesneleri erişmek için kullanılmalıdır. Kullanırken, HTTPS protokolünü kullanılmasını zorunlu [paylaşılan erişim imzaları](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) Azure Storage nesnelere erişimi devretmek için (SAS). SAS iki tür vardır: hizmet SAS ve hesap SAS.

#### <a name="how-do-i-construct-a-service-sas"></a>Hizmet SAS nasıl oluşturmak?

Hizmet SAS depolama hizmetleri (blob, kuyruk, tablo veya dosya hizmeti) yalnızca birinde bir kaynağa erişim atar. Hizmet SAS oluşturmak için aşağıdakileri yapın:

1. İmzalı sürüm alanını belirtin

2. İmzalı kaynak (Blob ve yalnızca dosya hizmeti) belirtin

3. Yanıt Üstbilgileri (Blob hizmeti ve yalnızca dosya hizmeti) geçersiz kılmak için sorgu parametrelerini belirtin

4. (Yalnızca tablo hizmeti) tablo adı belirtin

5. Erişim ilkesini belirtin

6. İmza geçerlilik aralığı belirtin.

8. İzinleri belirtin

9. IP adresi veya IP aralığını belirtin

10. HTTP protokolünü belirtin

11. Tablo erişim aralıkları belirtin

12. İmzalı tanımlayıcısını belirtin

13. İmza belirtin

Daha ayrıntılı yönergeler için bkz: [hizmet SAS oluşturma](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).

#### <a name="how-do-i-construct-an-account-sas"></a>Bir hesap SAS nasıl oluşturmak?

Bir hesap SAS bir veya daha fazla depolama hizmetindeki kaynaklara erişim atar. Bununla birlikte hizmet SAS ile izin verilmeyen blob kapsayıcılar, tablolar kuyruklar ve dosya paylaşımları üzerinde okuma, yazma ve silme işlemleri için yetkilendirme yapabilirsiniz. Bir hesap SAS yapımı, bir hizmet SAS benzer. Ayrıntılı yönergeler için bkz: [bir hesap SAS oluşturma.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a>HTTPS nasıl REST API'leri çağrılırken zorunlu?

REST API'leri depolama hesaplarındaki erişim nesnelere çağrılırken HTTPS kullanımını zorunlu kılmak için güvenli aktarım gerekli depolama hesabı için etkinleştirebilirsiniz. 

1. Azure portalında seçin **depolama hesabı oluştur**, veya mevcut bir depolama hesabını seçin **ayarları** ve ardından **yapılandırma**.

2. Altında **güvenli aktarım gerekli**seçin **etkin**.

![Bir depolama hesabı oluşturma](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

Daha ayrıntılı yönergeler için güvenli aktarım gerekli program aracılığıyla etkinleştirmek için bkz: nasıl dahil olmak üzere [gerektiren güvenli aktarım](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a>Nasıl Azure dosya depolama birimindeki verileri şifreliyor mu?

İle Aktarımdaki verileri şifrelemek için [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), SMB kullanabilirsiniz 3.x Windows 8, 8.1 ve 10 ve Windows Server 2012 R2 ve Windows Server 2016 ile. Azure dosyaları hizmetini kullanırken, "güvenli aktarım gerekli" etkinleştirilmişse, şifreleme olmadan herhangi bir bağlantı başarısız olur. Bu, SMB 2.1, SMB 3.0 şifreleme olmadan ve Linux SMB istemcisi bazı özellikleri kullanarak senaryolar içerir.

#### <a name="azure-client-side-encryption"></a>Azure istemci tarafı şifreleme

Bir istemci uygulaması ve Azure Storage arasında aktarıldığı sırada kişisel verileri korumak için başka bir seçenek [istemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/storage-client-side-encryption). Azure depolama alanına aktarılmadan önce veriler şifrelenir ve verileri Azure depolama biriminden aldığınızda, istemci tarafında alındıktan sonra verilerin şifresi çözülür.

### <a name="azure-site-to-site-vpn"></a>Azure siteden siteye VPN

Bir şirket ağı veya kullanıcı ve Azure sanal ağı arasında aktarımda kişisel verilerinizi korumak için etkili bir yol kullanmaktır bir [siteden siteye](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) veya [noktası site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) sanal özel ağ (VPN). Bir VPN bağlantısı Internet üzerinden şifrelenmiş güvenli bir tünel oluşturur.

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a>Siteden siteye VPN bağlantısı nasıl oluşturulur?

Siteden siteye VPN şirket ağındaki birden çok kullanıcı için Azure bağlanır. Azure portalında bir siteden siteye bağlantı oluşturmak için aşağıdakileri yapın:

1. Sanal ağ oluşturun.

2. Bir DNS sunucusu belirtin.

3. Ağ geçidi alt ağını oluşturun.

4. VPN ağ geçidi oluşturun. 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. Yerel ağ geçidi oluşturun.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. VPN Cihazınızı yapılandırın.

7. VPN bağlantısı oluşturun.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. VPN bağlantısını doğrulayın.

Azure portalında bir siteden siteye bağlantı oluşturma konusunda daha ayrıntılı yönergeler için bkz. [Azure Portalı'nda bir siteden siteye bağlantı oluşturun.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a>Bir noktadan siteye VPN bağlantısı nasıl oluşturulur?

Noktadan siteye VPN, tek bir istemci bilgisayardan bir sanal ağa güvenli bir bağlantı oluşturur. Uzak bir konumdan Azure gibi giriş veya bir otel veya konferans Merkezi'nden bağlanmak istediğinizde kullanışlıdır. Azure portalında bir noktadan siteye bağlantı oluşturmak için

1. Sanal ağ oluşturun.

2. Bir ağ geçidi alt ağı ekleyin.

3. Bir DNS sunucusu belirtin. (isteğe bağlı)

4. Bir sanal ağ geçidi oluşturun.

5. Sertifikaları oluşturur.

6. İstemcisi adres havuzunu ekleyin.

7. Kök sertifika ortak sertifikası verileri karşıya yükleyin.

8. Oluştur ve VPN istemcisi yapılandırma paketini yükleyin.

9. Dışarı aktarılan istemci sertifikası yükleyin.

10. Azure'a bağlayın.

11. Bağlantınızı doğrulayın.

Daha ayrıntılı yönergeler için bkz: [sertifika kimlik doğrulaması kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma: Azure Portal.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)

### <a name="ssltls"></a>SSL/TLS

Microsoft, her zaman SSL/TLS protokolleri farklı konumlar arasında veri değişimi için kullanmanızı önerir. Kullanmayı tercih kuruluşlar [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) ayrılmış bir yüksek hızlı WAN üzerinden büyük veri kümeleri taşımak için bağlantı verilerinin uygulama düzeyinde de şifreleyebilirsiniz SSL/TLS ya da diğer protokolleri kullanarak koruma eklendi.

### <a name="encryption-by-default"></a>Varsayılan şifreleme

Microsoft, müşteriler ve Azure bulut hizmetleri arasında Aktarımdaki verileri korumak için şifreleme kullanır. Azure Portalı aracılığıyla Azure Storage ile etkileşim, tüm işlemleri HTTPS oluşur.

[Aktarım Katmanı Güvenliği](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) protokolüdür, Microsoft veri merkezleri Microsoft bulut hizmetlerine bağlanmak istemci sistemleri ile belirlemeye çalışacaktır. TLS, güçlü kimlik doğrulaması, ileti gizliliği ve bütünlük (ileti oynama, kişiler tarafından ele ve sahtekarlığı saptama etkinleştirir), birlikte çalışabilirlik, algoritma esnekliği, dağıtım ve kullanım kolaylığı sağlar.

[Kusursuz iletme gizliliği](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) de işe benzersiz anahtarlar böylece müşterilerin istemci sistemleri ve Microsoft'un bulut hizmetleri arasında her bağlantı kullanır. Microsoft bulut hizmetlerine bağlantıları da temel RSA 2.048 bit şifreleme anahtar uzunluklarını yararlanın. TLS, RSA 2.048 bit anahtar uzunluğu, birleşimi ve PFS kılar çok daha zor birisi için Microsoft bulut hizmetlerine ve müşterileri arasında aktarımda ıntercept ve erişim veriler için.

Aktarımdaki verileri [Data Lake Store'da] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview) her zaman şifrelenir. Kalıcı medyaya depolama önce veri şifrelemeye ek olarak, aktarımdaki veriler de her zaman HTTPS kullanılarak korunmaktadır. HTTPS, Data Lake Store REST arabirimleri için desteklenen tek protokoldür.

## <a name="summary"></a>Özet

Şirket HTTPS bağlantıları için Azure Storage zorlama, paylaşılan erişim imzaları kullanarak ve güvenli aktarım gerekli depolama hesaplarında etkinleştirme kişisel veriler ve bu tür verilerin gizliliği korumanın amacı gerçekleştirebilirsiniz. Bunlar aynı zamanda kişisel verileri SMB 3.0 bağlantıları kullanarak ve istemci tarafı şifreleme uygulama koruyabilirsiniz. Siteden siteye VPN bağlantıları Azure sanal ağı için Kurumsal ağdan ve bireysel kullanıcılardan noktadan siteye VPN bağlantıları üzerinden güvenli bir şekilde kişisel veriler seyahat güvenli bir tünel oluşturur. Microsoft'un varsayılan şifreleme yöntemler kişisel verilerin gizliliği daha fazla koruma sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure veri güvenliği ve şifreleme en iyi uygulamalar](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [VPN Gateway için planlama ve tasarım](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [VPN Gateway ile ilgili SSS](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [Satın alma ve Azure uygulama hizmetiniz için bir SSL sertifikası yapılandırma](https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site)
