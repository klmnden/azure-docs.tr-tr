---
title: "Medya Hizmetleri PlayReady lisans şablonu genel bakış"
description: "Bu konu, PlayReady lisansları yapılandırmak için kullanılan bir PlayReady lisans şablonu genel bir bakış sağlar."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: fddce5d0-1278-478f-ae05-9b985c748731
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: be19f616e36916655390cd05e738e93c08dcdf68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="media-services-playready-license-template-overview"></a>Medya Hizmetleri PlayReady lisans şablonu genel bakış
Azure Media Services, artık Microsoft PlayReady lisansları teslim etmek için bir hizmet sunar. Son kullanıcı player (örneğin, Silverlight) korumalı PlayReady içeriğinizi oynatma denediğinde, bir istek lisans edinmeye yönelik lisans teslimat hizmeti gönderilir. Lisans hizmeti isteği onaylarsa, istemciye gönderilen ve şifresini çözmek ve belirtilen içeriği yürütmek için kullanılan lisans verir.

Media Services PlayReady lisanslarınızı yapılandırmanıza olanak tanıyan API'ler de sağlar. Lisansları hakları ve PlayReady DRM çalışma zamanı için bir kullanıcı kayıttan yürütme korumalı içeriği çalışırken uygulamak istediğinize kısıtlamaları içerir.
Aşağıda, belirtebilirsiniz PlayReady lisans kısıtlamaları bazı örnekleri şunlardır:

* Lisans geçerli olduğu tarih saat.
* Lisans sona erdiğinde tarih saat değeri. 
* Kalıcı depolama alanına istemcide kaydedilmesi için lisans. Kalıcı lisanslar genellikle çevrimdışı kayıttan yürütme içeriğin izin vermek için kullanılır.
* Bir oynatıcı içeriğinizi yürütmek için gereken en düşük güvenlik düzeyi. 
* Audio\video içerik için çıkış denetimleri için çıkış koruma düzeyi. 
* Daha fazla bilgi için çıkış denetimleri (3.5) bölümüne bakın [PlayReady uyumluluk kuralları](https://www.microsoft.com/playready/licensing/compliance/) belge.

> [!NOTE]
> Şu anda, (Bu hakkı gereklidir) PlayReady lisans PlayRight yalnızca yapılandırabilirsiniz. PlayRight istemci içeriği kayıttan yürütme için olanak sağlar. Kayıttan yürütme için belirli kısıtlamalar yapılandırma PlayRight de sağlar. Daha fazla bilgi için bkz: [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).
> 
> 

PlayReady lisansları Media Services'i kullanarak yapılandırmak için Media Services PlayReady lisans şablonu yapılandırmanız gerekir. Şablon XML içinde tanımlandı.

Aşağıdaki örnek, bir temel akış lisans yapılandırır basit (ve en yaygın) şablonu gösterir. Bu lisansı ile istemcileriniz için kayıttan yürütme gerçekleştirebilir, PlayReady korumalı içeriği.

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

PlayReady lisans şablonunda XML Şeması bölümünde tanımlanan PlayReady lisans şablonu XML şeması için XML uyumlu.

Media Services, ayrıca bir dizi serileştirilmiş ve XML gelen ve giden seri kullanılabilir .NET sınıfları tanımlar. Ana sınıf bir açıklaması için bkz: [Media Services .NET sınıfları](media-services-playready-license-template-overview.md#classes). Lisans şablonları yapılandırmak için kullanılır.

PlayReady lisans şablonu yapılandırmak için .NET sınıfları kullanan uçtan uca örnek için bkz: [dinamik şifreleme kullanarak PlayReady ve lisans teslimat hizmeti](media-services-protect-with-drm.md).

## <a id="classes"></a>Lisans şablonları yapılandırmak için kullanılan Media Services .NET sınıfları
Medya Hizmetleri PlayReady lisans şablonları yapılandırmak için kullanılan ana .NET sınıflar verilmiştir. Bu sınıfların eşleme tanımlanan türlerine [PlayReady lisans şablonu XML Şeması](media-services-playready-license-template-overview.md#schema).

[MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) sınıfı seri hale getirmek ve Media Services lisans şablonu XML gelen ve giden serisini kaldırmak için kullanılır.

### <a name="playreadylicenseresponsetemplate"></a>PlayReadyLicenseResponseTemplate
[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) -Bu sınıf son kullanıcıya gönderilen yanıtı için şablonu temsil eder. Bir veya daha fazla lisans şablonların listesi yanı sıra lisans sunucusunu ve (için özel uygulama mantığını yararlı olabilir) uygulama arasında özel verileri dize için bir alan içeriyor.

Bu şablonu hiyerarşisindeki "üst düzey" sınıftır. Yanıt şablonu lisans şablonları listesini içerir ve lisans şablonları (doğrudan veya dolaylı olarak) dahil anlamına tüm şablonu verileri seri hale yapmak diğer sınıflar.

### <a name="playreadylicensetemplate"></a>PlayReadyLicenseTemplate
[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) -son kullanıcılara döndürülecek PlayReady lisansları oluşturmak için bir lisans şablonu sınıfı temsil eder. Lisans ve hak veya kısıtlamaları PlayReady DRM çalışma zamanı tarafından içerik anahtarı kullanırken zorlanacak içerik anahtarı üzerindeki verileri içerir.

### <a id="PlayReadyPlayRight"></a>PlayReadyPlayRight
[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) -Bu sınıf PlayReady lisans PlayRight temsil eder. Kullanıcı kayıttan yürütme lisans hem de (için kayıttan yürütme belirli bir ilke) PlayRight kendisini yapılandırılan sıfır veya daha fazla kısıtlamalarına tabidir içerik yeteneği verir. PlayRight ilkesindeki çoğunu üzerinden içerik çalınabilir çıkışları türlerini denetleyen çıkış kısıtlamaları ve belirli bir çıkış kullanırken yerinde götürüldüğü herhangi bir kısıtlamanın ile ilgilidir. Örneğin, DigitalVideoOnlyContentRestriction etkinleştirilirse, DRM çalışma zamanı yalnızca dijital çıkışları (analog video çıkışına içeriği geçirmek kullanmasına izin verilmeyecek) görüntülenecek video sağlayacaktır.

> [!IMPORTANT]
> Bu tür kısıtlamaların çok güçlü olabilir ancak tüketici deneyimini de etkileyebilir. Çıktı korumaları çok kısıtlayıcı olarak yapılandırılırsa, içeriğin bazı istemcilerde unplayable olabilir. Daha fazla bilgi için bkz: [PlayReady uyumluluk kuralları](https://www.microsoft.com/playready/licensing/compliance/) belge.
> 
> 

Silverlight destekler ne koruma düzeyleri örneği için bkz: [çıkışı korumalardan Silverlight desteği](http://go.microsoft.com/fwlink/?LinkId=617318).

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
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

