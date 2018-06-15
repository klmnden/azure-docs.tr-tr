---
title: Medya Hizmetleri PlayReady lisans şablonu genel bakış
description: Bu konu, PlayReady lisansları yapılandırmak için kullanılan bir PlayReady lisans şablonu genel bir bakış sağlar.
author: juliako
manager: cfowler
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: fddce5d0-1278-478f-ae05-9b985c748731
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 74a2eb579f38cfc885234fac7fd3ad4be1747ad7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788833"
---
# <a name="media-services-playready-license-template-overview"></a>Medya Hizmetleri PlayReady lisans şablonu genel bakış
Azure Media Services şimdi PlayReady lisansları teslim etmek için bir hizmet sunar. PlayReady korumalı içeriği yürütmek (örneğin, Silverlight) player çalıştığında, bir istek lisans edinmeye yönelik lisans teslimat hizmeti gönderilir. Lisans hizmeti isteği onaylarsa, istemciye gönderilen ve şifresini çözmek ve belirtilen içeriği yürütmek için kullanılan lisans verir.

Media Services, PlayReady lisanslarınızı yapılandırmak için kullanabileceğiniz API'ler de sağlar. Korumalı içeriği kayıttan yürütmek bir kullanıcı çalıştığında zorlamak için PlayReady dijital hak yönetimi (DRM) çalışma zamanı istediğiniz kısıtlamaları ve lisansları hakları içerir.
Belirleyebileceğiniz PlayReady lisans kısıtlamaları bazı örnekleri şunlardır:

* Tarih ve saat Lisans geçerli.
* Lisans sona erdiğinde tarih saat değeri. 
* Kalıcı depolama alanına istemcide kaydedilmesi için lisans. Kalıcı lisanslar genellikle çevrimdışı kayıttan yürütme içeriğin izin vermek için kullanılır.
* Bir oynatıcı içeriğinizi yürütmek için gereken en düşük güvenlik düzeyi. 
* Audio\video içerik için çıkış denetimleri için çıkış koruma düzeyi. 
* Daha fazla bilgi için "Çıkış denetimleri" (3.5) bölümüne bakın [PlayReady uyumluluk kuralları](https://www.microsoft.com/playready/licensing/compliance/) belge.

> [!NOTE]
> Şu anda, PlayReady lisans PlayRight yalnızca yapılandırabilirsiniz. Bu hak gereklidir. PlayRight istemci içeriği kayıttan olanağı sağlar. Kayıttan yürütme için belirli kısıtlamalar yapılandırmak için PlayRight de kullanabilirsiniz. Daha fazla bilgi için bkz: [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).
> 
> 

PlayReady lisansları Media Services kullanarak yapılandırmak için Media Services PlayReady lisans şablonu yapılandırmanız gerekir. Şablon XML içinde tanımlandı.

Aşağıdaki örnek, bir temel akış lisans yapılandırır basit (ve en yaygın) şablonu gösterir. Bu lisansı ile istemcileriniz PlayReady korumalı içeriği oynatabilirsiniz.

    <?xml version="1.0" encoding="utf-8"?>
    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" 
                                      xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <PlayRight />
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

"PlayReady lisans şablonu XML Şeması" bölümünde tanımlanan PlayReady lisans şablonu XML şeması için XML uyumlu.

Media Services de seri hale getirmek ve XML gelen ve giden serisini kaldırmak için kullanılan .NET sınıfları kümesini tanımlar. Ana Sınıf açıklaması için bkz: [Media Services .NET sınıfları](media-services-playready-license-template-overview.md#classes) lisans şablonları yapılandırmak için kullanılır.

PlayReady lisans şablonu yapılandırmak için .NET sınıfları kullanan uçtan uca örnek için bkz: [kullanım PlayReady dinamik şifreleme ve lisans teslimat hizmeti](media-services-protect-with-playready-widevine.md).

## <a id="classes"></a>Lisans şablonları yapılandırmak için kullanılan Media Services .NET sınıfları
Medya Hizmetleri PlayReady lisans şablonları yapılandırmak için kullanılan ana .NET sınıfları sınıflarıdır. Bu sınıfların eşleme tanımlanan türlerine [PlayReady lisans şablonu XML Şeması](media-services-playready-license-template-overview.md#schema).

[MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) sınıfı seri hale getirmek ve Media Services lisans şablonu XML gelen ve giden serisini kaldırmak için kullanılır.

### <a name="playreadylicenseresponsetemplate"></a>PlayReadyLicenseResponseTemplate
[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx): Bu sınıf kullanıcıya geri gönderilen yanıtı için şablonu temsil eder. Lisans sunucusu ve (için özel uygulama mantığını yararlı olabilir) uygulama arasında özel verileri dize için bir alan içeriyor. Ayrıca, bir veya daha fazla lisans şablonları listesini içerir.

Şablonu hiyerarşisindeki "üst düzey" sınıf olarak yanıt şablonu lisans şablonları listesini içerir. Lisans şablonları (doğrudan veya dolaylı olarak) dahil şablon verileri seri hale yapmak diğer tüm sınıflar.

### <a name="playreadylicensetemplate"></a>PlayReadyLicenseTemplate
[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx): Bu sınıf kullanıcılara döndürülecek PlayReady lisansları oluşturmak için kullanılan bir lisans şablonu temsil eder. İçerik anahtarı lisans verilerini içerir. Ayrıca, tüm hakları veya kısıtlamaları içerik anahtarı kullanıldığında, PlayReady DRM çalışma zamanı zorlamalıdır de içerir.

### <a id="PlayReadyPlayRight"></a>PlayReadyPlayRight
[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx): Bu sınıf PlayReady lisans PlayRight temsil eder. Bu kullanıcının lisans hem de (için kayıttan yürütme özel ilke) PlayRight kendisini yapılandırılan sınırlamalara tabi içeriği kayıttan olanağı verir. Bir PlayRight ilkesindeki çoğunu üzerinden içerik çalınabilir çıkışları türlerini denetlemek çıkış kısıtlamaları ilgilidir. Ayrıca, belirli bir çıkış kullanıldığında yerinde konulmalıdır kısıtlamaları içerir. Örneğin, DigitalVideoOnlyContentRestriction etkinleştirilirse, DRM çalışma zamanı dijital çıkışları görüntülenecek video yalnızca izin verir. (Analog video çıkışına içeriği geçmesine izin verilmez.)

> [!IMPORTANT]
> Bu tür kısıtlamaların güçlü olabilir, ancak müşteri deneyimi de etkileyebilir. Çıktı korumaları çok kısıtlayıcı varsa, içeriği bazı istemcilerde unplayable olabilir. Daha fazla bilgi için bkz: [PlayReady uyumluluk kuralları](https://www.microsoft.com/playready/licensing/compliance/).
> 
> 

Silverlight destekleyen bir örnek koruma düzeyleri için bkz: [çıkışı korumalardan Silverlight desteği](http://go.microsoft.com/fwlink/?LinkId=617318).

## <a id="schema"></a>PlayReady lisans şablonu XML Şeması
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:ser="http://schemas.microsoft.com/2003/10/Serialization/" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />
      <xs:complexType name="AgcAndColorStripeRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction" />
      <xs:simpleType name="ContentType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Unspecified" />
          <xs:enumeration value="UltravioletDownload" />
          <xs:enumeration value="UltravioletStreaming" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="ContentType" nillable="true" type="tns:ContentType" />
      <xs:complexType name="ExplicitAnalogTelevisionRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="BestEffort" type="xs:boolean" />
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ExplicitAnalogTelevisionRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction" />
      <xs:complexType name="PlayReadyContentKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="PlayReadyContentKey" nillable="true" type="tns:PlayReadyContentKey" />
      <xs:complexType name="ContentEncryptionKeyFromHeader">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence />
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromHeader" nillable="true" type="tns:ContentEncryptionKeyFromHeader" />
      <xs:complexType name="ContentEncryptionKeyFromKeyIdentifier">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence>
              <xs:element minOccurs="0" name="KeyIdentifier" type="ser:guid" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromKeyIdentifier" nillable="true" type="tns:ContentEncryptionKeyFromKeyIdentifier" />
      <xs:complexType name="PlayReadyLicenseResponseTemplate">
        <xs:sequence>
          <xs:element name="LicenseTemplates" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
          <xs:element minOccurs="0" name="ResponseCustomData" nillable="true" type="xs:string">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseResponseTemplate" nillable="true" type="tns:PlayReadyLicenseResponseTemplate" />
      <xs:complexType name="ArrayOfPlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfPlayReadyLicenseTemplate" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
      <xs:complexType name="PlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AllowTestDevices" type="xs:boolean" />
          <xs:element minOccurs="0" name="BeginDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element name="ContentKey" nillable="true" type="tns:PlayReadyContentKey" />
          <xs:element minOccurs="0" name="ContentType" type="tns:ContentType">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExpirationDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="GracePeriod" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="LicenseType" type="tns:PlayReadyLicenseType" />
          <xs:element minOccurs="0" name="PlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
      <xs:simpleType name="PlayReadyLicenseType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Nonpersistent" />
          <xs:enumeration value="Persistent" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="PlayReadyLicenseType" nillable="true" type="tns:PlayReadyLicenseType" />
      <xs:complexType name="PlayReadyPlayRight">
        <xs:sequence>
          <xs:element minOccurs="0" name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AllowPassingVideoContentToUnknownOutput" type="tns:UnknownOutputPassingOption">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AnalogVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="DigitalVideoOnlyContentRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExplicitAnalogTelevisionOutputRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="FirstPlayExpiration" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComponentVideoRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComputerMonitorRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyPlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
      <xs:simpleType name="UnknownOutputPassingOption">
        <xs:restriction base="xs:string">
          <xs:enumeration value="NotAllowed" />
          <xs:enumeration value="Allowed" />
          <xs:enumeration value="AllowedWithVideoConstriction" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="UnknownOutputPassingOption" nillable="true" type="tns:UnknownOutputPassingOption" />
      <xs:complexType name="ScmsRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction" />
    </xs:schema>



## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

