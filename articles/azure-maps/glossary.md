---
title: Azure haritalar sözlüğü | Microsoft Docs
description: Azure haritalar, konum tabanlı hizmetler ve GIS ilişkili yaygın olarak kullanılan terimler sözlüğü.
author: rbrundritt
ms.author: richbrun
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 9be99bc9ac4683fea97333c9d6cb783f0fde35c5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64575339"
---
# <a name="glossary"></a>Sözlük

Azure Haritalar ile kullanılan ortak kelimeler listesi verilmiştir.

## <a name="a"></a>A

<a name="address-validation"></a> **Adres doğrulaması**: Bir adresi varlığını doğrulama işlemi.

<a name="advanced-routing"></a> **Gelişmiş yönlendirme**: Öncelikli işlemleri kullanarak erişilebilir aralıkları (izokron), mesafe matrisi ve toplu istekleri hesaplama gibi yönlendirme verileri yol, hizmetleri koleksiyonudur.

<a name="aerial-imagery"></a> **Havadan tanımayı**: Bkz: [uydu tanımayı](#satellite-imagery). 

<a name="along-a-route-search"></a> **Bir rota arama boyunca**: Belirtilen bir sapma saati veya uzaklığı bir rota yolu içinde verileri arar uzamsal sorgu.

<a name="altitude"></a> **Yükseklik**: Yükseklik veya bir başvuru yüzeyi üzerinde bir noktasının dikey yükseltme. Yükseklik ölçümler gibi ortalama Deniz düzeyinde bir verilen başvuru verisi temel alır. Ayrıca bkz: yükseltme.

<a name="ambiguous"></a> **Belirsiz**: Bir nesne iki veya daha fazla değer belirli bir öznitelik için uygun şekilde atanması, var olan veri sınıflandırması belirsizliğin durumu. Örneğin, coğrafi kodlama "CA" iki belirsiz sonuçlar döndürülür; "Kanada" ve "California" "CA" olarak, her bir ülke ve eyalet kodu sırasıyla. 

<a name="annotation"></a> **Ek açıklama**: Metin ya da grafik kullanıcıya bilgi sağlamak için harita üzerinde görüntülenir. Ek açıklama tanımlamak veya belirli bir eşleme varlığı açıklayan, harita üzerinde bir alan hakkındaki genel bilgileri girin veya eşleme hakkında bilgi sağlayın.

<a name="antimeridian"></a> **Antimeridian**: Olarak da bilinen 180<sup>th</sup> Meridyen nerede-180 derece ve boylam 180 derece karşılamak noktasıdır. Dünya üzerindeki asal meridyen ters olduğu.

<a name="application-programming-interface-api"></a> **Uygulama programlama arabiriminin (API)**: Geliştiricilerin uygulamalar oluşturmasına olanak sağlayan belirtim.

<a name="api-key"></a> **API anahtarı**: Azure haritalar anahtarını bakın.

<a name="area-of-interest-aoi"></a> **İlgi alanı (AOI)**: Odak alanı eşlemesi veya veritabanı üretim için tanımlamak için kullanılan uzantı.

<a name="asset-tracking"></a> **Varlık İzleme**: Bir kişi, araç veya başka bir nesne gibi bir varlığın konum izleme işlemi.

<a name="asynchronous-request"></a> **Zaman uyumsuz istek**: Bir bağlantı açar ve zaman uyumsuz istek için bir tanımlayıcı döndürür ve ardından bağlantıyı kapatır sunucuya istekte bir HTTP isteği. Sunucu isteği işlemeye devam eder ve kullanıcı tanımlayıcısını kullanarak durumu denetleyebilirsiniz. İsteğin işlenmesi Bitti, kullanıcı ardından yükleme yanıt olduğunda. Bu istek türü genellikle uzun süre için kullanılan çalışan işlemleri.

<a name="autocomplete"></a> **Otomatik Tamamlama**: Bir uygulamada bir özellik, bir kullanıcının bir word'ün rest tahmin eder. 

<a name="autosuggest"></a> **Otomatik öneri**: Bir özellik, bir uygulamanın hangi kullanıcı yazmak için mantıksal olasılıklar tahmin eder.

<a name="azure-location-based-services-lbs"></a> **Azure konum tabanlı hizmetler (LBS)**: Azure Önizleme aşamasında olduğu zaman haritalar önceki adı.

<a name="azure-maps-key"></a> **Azure haritalar anahtarını**: Bir Azure haritalar anahtarını, bir kullanıcının Azure haritalar uygulaması veya hizmeti istek kimliğini doğrulamak için kullanılan benzersiz bir dizedir. 

## <a name="b"></a>B

<a name="base-map"></a> **Temel harita**: Yollar, yer işareti ve siyasi sınırları gibi arka plan başvuru bilgileri görüntüleyen bir harita uygulamanın bir parçası.

<a name="batch-request"></a> **Toplu istek**: Birden çok isteği tek bir istek birleştirme işlemi.

<a name="bearing"></a> **Pul**: Yatay yöndeki bir diğer nokta ile ilgili bir noktanın. Bu, Açı 0-360 derece yönünde derece kuzeyden göreli olarak ifade edilir. 

<a name="boundary"></a> **Sınır**: Bir satır veya Çokgen ülkeler/bölgeler, bölgeleri ve özellikler gibi bitişik siyasi varlıkları ayrılması. Bir sınırı veya rivers, dağlarında yürüyüş ya da duvar gibi fiziksel özelliklerini uymayabilecek bir satırdır.

<a name="bounds"></a> **Sınırları**: Bkz: [Bounding kutusu](#bounding-box).

<a name="bounding-box"></a> **Sınırlayıcı kutu**: Harita üzerinde bir dikdörtgen alan temsil etmek için kullanılan koordinatları kümesini. 

## <a name="c"></a>C

<a name="cadastre"></a> **Cadastre**: Bir kayıt kayıtlı land ve özellikleri. Ayrıca bkz: [paket](#parcel).

<a name="camera"></a> **Kamera**: Etkileşimli harita denetimi bağlamında, bir kamera görünüm haritalar alanı tanımlar. Görünüm penceresinin kameranın birkaç harita parametrelerine göre belirlenir; Center, yakınlaştırma düzeyi, aralık, seçtiğiniz. 

<a name="centroid"></a> **Kütle Merkezi**: Bir özelliğin geometrik Merkezi. Kütle merkezi bir alan merkezini bulunurken bir satırın kütle merkezi Orta olacaktır.

<a name="choropleth-map"></a> **Choropleth'te harita**: Alanları bir ölçüm haritada görüntülenmesini istatistiksel bir değişkenin derlemekten gölgeli tematik eşlemesi. Örneğin, diğer tüm durumlara göreli yıllarındaki nüfusu göre her ABD devlet sınırları renklendirme.

<a name="concave-hull"></a> **İçbükey Kabuk**: Belirtilen veri kümesindeki tüm şekiller barındırır olası bir İçbükey geometrisini temsil eden bir şekli. Oluşturulan şekil verileri Plastik Sarma ile kaydırma ve ardından ısıtma benzer, bu nedenle büyük neden diğer veri noktası cave noktalarına arasında yayılır.

<a name="consumption-model"></a> **Kullanım modeli**: Bir araç yakıt veya elektrik kullanan oranı tanımlayan bilgiler. Ayrıca bkz: [tüketim modelini belgeleri](consumption-model.md).

<a name="control"></a> **Denetim**: Arabirimi için davranışları kümesini tanımlayan bir grafik kullanıcı arabirimi oluşan müstakil ya da yeniden kullanılabilir bileşen. Örneğin, bir harita denetimi olan genellikle etkileşimli bir harita yükler kullanıcı arabiriminin bir bölümü.

<a name="convex-hull"></a> **Dışbükey Kabuk**: Dışbükey Kabuk belirtilen veri kümesindeki tüm şekiller kapsayan en düşük dışbükey geometrisini temsil eden şekildir. Oluşturulan veri kümesi etrafında bir esnek bant kaydırma benzer şekildir.

<a name="coordinate"></a> **Koordinat**: Bir haritadaki bir konumu temsil etmek için kullanılan boylam ve enlem değerlerini içerir.

<a name="coordinate-system"></a> **Koordinat sistemi**: İki veya üç boyutta alanında noktalarının konumlarını tanımlamak için kullanılan bir başvuru çerçevesi.

<a name="country-code"></a> **Ülke kodu**: ISO standardına göre bir ülke/bölge için benzersiz bir tanımlayıcı. ISO2 ISO3 temsil eden üç karakterli (örneğin, ABD) kod ülke (örneğin, ABD), iki karakterli kodudur.

<a name="country-subdivision"></a> **Ülke alt**: Bir ülke/genellikle bir eyalet veya il bilinen bölge, birinci düzey bölümlemesini.

<a name="country-secondary-subdivision"></a> **Ülke ikincil alt**: Bir ülke/genellikle bir ilçe bilinen bölge, ikinci düzey bölümlemesini.

<a name="country-tertiary-subdivision"></a> **Ülke üçüncül alt**: Bir ülke/bölge, genellikle bir adlandırılmış alana bir anlatan gibi üçüncü düzey bölümlemesini.

<a name="cross-street"></a> **Çapraz Sokak**: İki veya daha fazla sokaklar kesiştiği bir nokta.

<a name="cylindrical-projection"></a> **Silindirik izdüşümü**: Spheroid veya sphere tanjantını veya secant silindir üzerine noktalarından dönüştüren bir projeksiyon. Silindir ardından üstten alta dilimlenebilir ve bir düzleme düzleştirilmiş.

## <a name="d"></a>D

<a name="datum"></a> **Datum**: Ölçüm sisteminin başvuru belirtimleri, bir koordinat sistemini bir yüzeyi (yatay datum) ya da yukarıdaki yüksekliklerini veya bir yüzeyi (dikey datum) altına yerleştirir.

<a name="dbf-file"></a> **DBF dosyası**: Şekil dosyaları (SHP) ile birlikte kullanılan bir veritabanı dosyası biçimi.

<a name="degree-minutes-seconds-dms"></a> **Derece saniye (DMS) dakika**: Enlem ve boylam tanımlamak için ölçü birimidir. 1/360 derecelik bir olduğu<sup>th</sup> bir daire. Bir ölçüde daha fazla 60 dakika içinde ayrılmış ve bir dakika içinde 60 saniye ayrılmıştır.

<a name="delaunay-triangulation"></a> **Delaunay Üçlü**: Bitişik, örtüşmeyen üçgenler ağıyla noktalarının kümesinden oluşturmak için kullanılan bir yöntem. Her üçgenin circumscribing daire kendi iç kümesinde hiç noktalarından içerir.

<a name="demographics"></a> **Demografik bilgiler**: İnsan popülasyonun bir istatistik özellikleri (örneğin, yaş, Doğum oranı ve gelir).

<a name="destination"></a> **Hedef**: Bir uç noktası veya, birisi için seyahatindeki konum.

<a name="digital-elevation-model-dem"></a> **Dijital yükseltme modeli (DEM)**: Değerleri, ortak bir veri kullanarak düzenli aralıklarla bir alanda üzerinden yakalanan bir yüzeyi için ilgili yükseltme bir veri kümesi. DEMs genellikle Arazi Tahliye temsil etmek için kullanılır.

<a name="dijkstra's-algorithm"></a> **Dijkstra'nın algoritması**: İki nokta arasındaki en kısa yolu bulmak için bir ağ bağlantısı inceler bir algoritma.

<a name="distance-matrix"></a> **Mesafe matrisi**: Matris çıkış noktaları kümesi ve hedefler arasında seyahat süresi ve uzaklık bilgilerini içerir. 

## <a name="e"></a>E

<a name="elevation"></a> **Yükseltme**: Bir nokta ya da yukarıdaki nesne veya bir başvuru yüzey veya datum (genellikle ortalama Deniz düzeyinde) aşağıda dikey uzaklık. Yükseltme, genelde land dikey yüksekliğine anlamına gelir.

<a name="envelope"></a> **Zarf**: Bkz: [Bounding kutusu](#bounding-box).

<a name="extended-postal-code"></a> **Genişletilmiş posta kodu**: Ek bilgi içerebilecek bir posta kodu. Örneğin, ABD beş basamak posta kodları vardır ancak zip + 4 olarak bilinen bir genişletilmiş posta kodu, dört ek basamaklar içerir. Bu ek basamaklar, beş basamaklı teslim alan bir şehir bloğu, binalarının bir grup veya verimli posta sıralama ve teslim yardımcı bir posta office kutusunu, gibi coğrafi bir Segmentte tanımlamak için kullanılır.

<a name="extent"></a> **Uzantı**: Bkz: [Bounding kutusu](#bounding-box).

## <a name="f"></a>F

<a name="federated-authentication"></a> **Federe kimlik doğrulaması**: Birden çok web ve mobil uygulamalarınızda kullanılmak üzere çoklu oturum açma/kimlik doğrulama mekanizması sağlayan bir kimlik doğrulama yöntemi. 

<a name="feature"></a> **Özellik**: Geometri ek meta veri bilgileri ile bir araya getiren bir nesne. 

<a name="feature-collection"></a> **Koleksiyon özellik**: Özellik nesnelerinin bir koleksiyonu.

<a name="find-along-route"></a> **Yol**: Belirtilen bir sapma saati veya uzaklığı bir rota yolu içinde verileri arar uzamsal sorgu.

<a name="find-nearby"></a> **Yakındaki Bul**: (Crow ile uçuyor gibi) sabit, doğrusal bir uzaklık noktası olan bir uzamsal sorgu.

<a name="fleet-management"></a> **Filo Yönetimi**: Araba, kamyon, verilir ve düzlemleri gibi ticari araçları yönetimi. Filo yönetimi, bir dizi araç Finansmanı, bakım, telematik (izleme ve tanılama) yanı sıra sürücü, hız, yakıt ve sistem durumu ve güvenlik yönetimi gibi işlevleri içerebilir. Filo yönetimi, riskleri en aza indirmek ve kamu mevzuatı uyum sağlarken kendi genel nakliye ve personel maliyetlerini azaltmak için kendi iş üzerinde nakliye kullanan şirketler tarafından kullanılan bir işlemdir.

<a name="free-flow-speed"></a> **Boş Akış hızı**: İdeal koşullarda beklenen boş Akış hızı. Genellikle hız sınırı.

<a name="free-form-address"></a> **Serbest biçimli adresi**: Tek metin satırı temsil edilen bir tam adresi.

<a name="fuzzy-search"></a> **Belirsiz arama**: Bir arama metnin bir adresi serbest biçimli dize veya ilgi noktası alır. 

## <a name="g"></a>G

<a name="geocode"></a> **Geocode**: Bir adres veya bu konumunu bir haritada görüntülemek için kullanılan bir koordinat dönüştürüldükten konum. 

<a name="geocoding"></a> **Coğrafi kodlama**: Olarak da bilinen ileriye doğru coğrafi kodlama, konum verileri adresini koordinatları haline dönüştürme işlemi olur. 

<a name="geodesic-path"></a> **Geodesic yolu**: Eğri yüzeyinde iki nokta arasındaki en kısa yolu. Azure haritalar üzerinde işlendiğinde bu yolu Mercator izdüşümünü nedeniyle bir eğri çizgi görünür.

<a name="geofence"></a> **Bölge sınırının**: Bir cihaz girdiğinde veya bölgede mevcut olaylarını tetiklemek için kullanılabilecek tanımlı coğrafi bölge.

<a name="geojson"></a> **GeoJSON**: Ortak bir JSON tabanlı dosya biçimi, noktaları, satırlar ve çokgenler gibi coğrafi vektör verilerini depolamak için kullanılır. **Not**: Azure haritalar kullanan GeoJSON genişletilmiş bir sürümünü [burada belgelenmektedir](extend-geojson.md).

<a name="geometry"></a> **Geometri**: Bir nokta, çizgi veya Çokgen gibi uzamsal bir nesneyi temsil eder.

<a name="geometrycollection"></a> **GeometryCollection**: Geometri nesneleri koleksiyonu.

<a name="geopol"></a> **GeoPol**: Tartışmalı kenarlık ve yer adlarına gibi ekleyemeyebilir hassas verileri ifade eder.

<a name="georeference"></a> **Jeo referans**: Coğrafi veriler veya bilinen bir koordinat sistemi için tanımayı hizalama işlemi. Bu işlem, kaydırma, döndürme, ölçeklendirme veya eğriltme verilerini oluşabilir.

<a name="georss"></a> **GeoRSS**: RSS özet akışlarına uzamsal veri eklemek için bir XML uzantısı.

<a name="gis"></a> **GIS**: "Coğrafi bilgi sistemi" kısaltması. Eşleme sektör açıklamak için kullanılan genel bir terimdir.

<a name="gml"></a> **GML**: Coğrafya biçimlendirme dili olarak da bilinir. Uzamsal veri depolamak için bir XML dosya uzantısı.

<a name="gps"></a> **GPS**: Olarak da bilinen genel konumlandırma sistemi Uyduları dünya cihazları konumuna belirlemek için kullanılan bir sistem olan. Gönderen yörüngedeki Uyduları trilateration aracılığıyla kendi konumunu hesaplamak GPS alıcısı herhangi bir yerinden izin sinyalleri iletir.

<a name="gpx"></a> **GPX**: XML dosya biçimi, GPS Takas Biçimi da bilinir, GPS cihazlardan yaygın olarak oluşturulur.  

<a name="great-circle-distance"></a> **Büyük daire uzaklık**: Kısa uzaklığı kürenin yüzeyinde iki nokta arasındaki.

<a name="greenwich-mean-time-gmt"></a> **Greenwich saati (GMT)**: Greenwich, İngiltere'deki Kraliyet Rasathanesi üzerinden çalışan asal meridyen zamanında.

<a name="guid"></a> **GUID**: Genel olarak benzersiz bir tanımlayıcı. Bir arabirim, sınıf, tür kitaplığı, bileşen kategorisine veya kaydı benzersiz olarak tanımlamak için kullanılan bir dize.

## <a name="h"></a>H

<a name="haversine-formula"></a> **Haversine formül**: Bir küre üzerinde iki nokta arasındaki mükemmel daire uzaklığı hesaplamak için kullanılan ortak bir eşitlik.

<a name="hd-maps"></a> **HD haritalar**: Olarak da bilinen yüksek tanımı haritalar oluşur Kulvar işaretler gibi yüksek uygunluğa sahip yol ağ bilgileri Tabela ve yönü ışık otonom sürüş için gerekli.

<a name="heading"></a> **Başlık**: Bir şey işaret eden veya karşılıklı yönü. Ayrıca bkz: [pul](#heading).

<a name="heatmap"></a> **Isı Haritası**: Bir renk aralığı noktalarının belirli bir alandaki yoğunluğu temsil bir veri görselleştirmesi. Ayrıca Tematik Haritası'na bakın.

<a name="hybrid-imagery"></a> **Karma resimler**: Uydu veya yol verileri ve çıktıklarını yayılan etiketleri olan havadan tanımayı.

## <a name="i"></a>I

<a name="iana"></a> **IANA**: Internet Atanmış Numaralar Yetkilisi kısaltması. Genel IP adresi ayırma'yı gözetir kar amacı gütmeyen bir grup.

<a name="isochrone"></a> **Isochrone**: Bir isochrone, birisi herhangi bir yönde nakliye türünü belirli bir süre içinde belirtilen bir konumdan ilerleyebilir alanı tanımlar. Ayrıca bkz: [erişilebilir aralığı](#reachable-range).

<a name="isodistance"></a> **Isodistance**: Bir konuma göz önünde bulundurulduğunda, bir isochrone, birisi için herhangi bir yönde nakliye türünü belirli bir uzaklıkta ilerleyebilir alanı tanımlar. Ayrıca bkz: [erişilebilir aralığı](#reachable-range).

## <a name="k"></a>K

<a name="kml"></a> **KML**: Olarak da bilinir anahtar deliği biçimlendirme dilidir noktaları, satırlar ve çokgenler gibi coğrafi vektör verilerini depolamak için ortak bir XML dosyası biçimi. 

## <a name="l"></a>L

<a name="landsat"></a> **Landsat**: Multispectral, dünya gönderen yörüngedeki Uyduları NASA tarafından geliştirilen, görüntülerin olan Tarım, Ormancılık ve cartography gibi çok sayıda sektörde kullanılır toplayın.

<a name="latitude"></a> **Enlem**: Kuzey veya Güney yönü ekvatorun dereceden olan Açısal uzaklık cinsinden.

<a name="level-of-detail"></a> **Ayrıntı düzeyi**: Yakınlaştırma bkz düzeyi.

<a name="lidar"></a> **Lidar**: Açık olan algılama ve değişen kısaltması. Yansıtıcı yüzeyleri için uzaklıkta ölçmek için Sever kullanan uzaktan algılama yöntemi.

<a name="linear-interpolation"></a> **Doğrusal enterpolasyon**: Bilinen değerler arasındaki doğrusal uzaklık kullanarak bilinmeyen bir değere tahmin.

<a name="linestring"></a> **LineString**: Bir satırı temsil etmek için kullanılan bir geometri. Çoklu çizgi olarak da bilinir. 

<a name="localization"></a> **Yerelleştirme**: Farklı dillere ve kültürlere desteği.

<a name="logistics"></a> **Lojistik**: Kişiler, araçları, kaynakları veya varlıklar koordineli bir şekilde taşıma işlemi.

<a name="longitude"></a> **Boylam**: Doğu veya Batı yönde asal meridyen dereceden olan Açısal uzaklık cinsinden.

## <a name="m"></a>M

<a name="map-tile"></a> **Kutucuk harita**: Bir eşlem tuvali bölümünü temsil eder bir dikdörtgen resim. Daha fazla bilgi için [yakınlaştırma düzeyleri ve döşeme Kılavuzu belgeleri](zoom-levels-and-tile-grid.md).

<a name="marker"></a> **İşaret**: Olarak da bilinen bir PIN veya Raptiye, bir noktası konumunu bir haritada temsil eden bir simge değil.

<a name="mercator-projection"></a> **Mercator izdüşümünü**: Meridyenleri ile bunları korumak Düz parçaları olarak rhumb satırları olarak bilinen sabit Elbette çizgilerini temsil yeteneğini nedeniyle Denizcilik amaçlar için standart harita izdüşümü dönüştü bir Silindirik harita izdüşümü. Tüm düz harita projeksiyonlar yeryüzünün doğru düzene karşılaştırıldığında harita boyutları ve şekiller çarpıtan. Daha küçük alanları kutupları yaklaştıkça harita üzerinde büyük görünür olacağı şekilde Mercator izdüşümünü ekvatorun gölgeden uzak alanları arttırdığınızda. 

<a name="multilinestring"></a> **MultiLineString**: Geometri LineString nesnelerinin bir koleksiyonunu temsil eder. 

<a name="multipoint"></a> **MultiPoint**: Geometri noktası nesnelerinin bir koleksiyonunu temsil eder.

<a name="multipolygon"></a> **MultiPolygon**: Geometri Çokgen nesnelerinin bir koleksiyonunu temsil eder. Örneğin, Hawaii sınırları göstermek için sahip bir çokgenin her Adası özetlenen ve Hawaii sınırları, bu nedenle bir MultiPolygon olacaktır.

<a name="municipality"></a> **Belediye**: Bir şehir veya şehir. 

<a name="municipality-subdivision"></a> **Belediye alt**: Komşuları veya "Şehir"merkezi gibi yerel adı gibi bir belediye bölümlemesini.

## <a name="n"></a>N

<a name="navigation-bar"></a> **Gezinti çubuğu**: Bir harita yakınlaştırma düzeyini, aralık döndürme ayarlama ve temel bir harita katmanını değiştirme denetim kümesi.

<a name="nearby-search"></a> **Arama yakındaki**: (Crow ile uçuyor gibi) sabit, doğrusal bir uzaklık noktası olan bir uzamsal sorgu.

<a name="neutral-ground-truth"></a> **Nötr çığır Truth**: Etiket resmi dilini temsil ettiği bölge ve yerel betikler varsa işler eşlemesi.

## <a name="o"></a>O

<a name="origin"></a> **Kaynak**: Bir başlangıç noktası veya bir kullanıcı olduğu konum.

## <a name="p"></a>P

<a name="panning"></a> **Kaydırma**: Bir sabit yakınlaştırma düzeyi sürdürürken herhangi bir yönde harita taşıma işlemi.

<a name="parcel"></a> **Paket**: Bir çizim land veya özellik sınırının.

<a name="pitch"></a> **Aralık**: Eğim miktarı harita 0 eşlemeyi burada arıyor doğrudan aşağı dikey göre vardır.

<a name="point"></a> **Noktası**: Geometri harita üzerinde tek bir konumu temsil eder. 

<a name="points-of-interest-poi"></a> **(POI) ilgi noktası**: İş, yer işareti veya ilgi ortak bir yerde.

<a name="polygon"></a> **Çokgen**: Bir haritadaki bir alanı temsil eden bir düz geometri. 

<a name="polyline"></a> **Çoklu çizgi**: Bir satırı temsil etmek için kullanılan bir geometri. LineString olarak da bilinir. 

<a name="position"></a> **Konum**: Boylam, enlem ve bir noktanın yüksekliği (x, y, z koordinatları).

<a name="post-code"></a> **Posta kodu**: Bkz: [postalcode](#postal-code).

<a name="postal-code"></a> **Posta kodu**: Bir dizi harf veya sayı veya her ikisi de posta teslimini basitleştirmek için coğrafi bölgeler bölgelere ayırmak için bir ülke/bölge posta hizmeti tarafından kullanılan belirli bir biçimde.

<a name="prime-meridian"></a> **Asal meridyen**: 0 derece boylam temsil eden boylam satırı. Genellikle, boylam değerleri 180 derece kadar westerly yönde dolaşırken azaltmak ve easterly yönde için -180 dolaşırken artırmak-derece. 

<a name="prj"></a> **PRJ**: Öngörülen koordinat sistemi hakkında bilgi içeren bir şekil dosyası dosyası genellikle eşlik eden bir metin dosyası veri kümesi bulunduğu.

<a name="projection"></a> **Projeksiyon**: Alan ve Robinson çapraz Mercator, Albers eşit olduğu gibi bir harita izdüşümü üzerinde temel bir öngörülen koordinat sistemi. Bunlar, bir iki boyutlu Kartezyen koordinat düzlemi küresel yüzeyine dünya proje haritalarını olanağı sağlar. Öngörülen koordinat sistemi harita izdüşümler adlandırılır.

## <a name="q"></a>Q

<a name="quadkey"></a> **Quadkey**: Önce quadtree döşeme sistemi içindeki bir kutucuk için 4 temel adresi dizini. Daha fazla bilgi için [yakınlaştırma düzeyleri ve döşeme Kılavuzu](zoom-levels-and-tile-grid.md) daha fazla bilgi için belgelere bakın.

<a name="quadtree"></a> **Önce Quadtree**: Her düğümü tam olarak dört alt öğeleri olan bir veri yapısı. Bir kullanıcı bir düzey yakınlaştırır gibi her bir harita kutucuğunu alt dört kutucuğa sonlarını, Azure haritalar içinde kullanılan döşeme sistemi quadtree yapısı kullanır.  Daha fazla bilgi için [yakınlaştırma düzeyleri ve döşeme Kılavuzu](zoom-levels-and-tile-grid.md) daha fazla bilgi için belgelere bakın.

<a name="queries-per-second-qps"></a> **Sorgular / saniye (QPS)**: Sorguları veya bir hizmeti veya platformu için bir saniye içinde yapılan isteklerinin sayısı. 

## <a name="r"></a>R

<a name="radial-search"></a> **Radyal arama**: (Crow ile uçuyor gibi) sabit, doğrusal bir uzaklık noktası olan bir uzamsal sorgu. 

<a name="raster-data"></a> **Hücresel veri**: Hücre (veya piksel) matrisin düzenlenmiş satırları ve sütunları (veya bir kılavuz) burada her hücre sıcaklık gibi bilgileri temsil eden bir değer içeriyor. Izgara'nın dijital havadan fotoğraflar, tanımayı Uyduları, resimleri ve taranan eşlemeleri içerir.

<a name="raster-layer"></a> **Izgara katman**: Izgara görüntülerini oluşan bir döşeme katmanı.

<a name="reachable-range"></a> **Erişilebilir aralığı**: Erişilebilir bir aralık içinde birisi bir süre veya bir konumdan herhangi bir yönde hareket etmek nakliye türünü için uzaklık içinde ilerleyebilir alanı tanımlar. Ayrıca bkz: [Isochrone](#isochrone) ve [Isodistance](#isodistance).

<a name="remote-sensing"></a> **Uzak Algılama**: Toplama ve bir uzaklıktan algılayıcı verileri yorumlama işlemi.

<a name="rest-service"></a> **REST hizmeti**: Kısaltması, temsili durum transferi REST anlamına gelir. Bir REST hizmeti, HTTP GET ve POST istekleri olan en yaygın yöntemleri iletişim kurmak için temel web teknolojisinden yararlanır bir URL tabanlı bir web hizmetidir. Bu tür hizmet, bana çok daha hızlı ve daha küçük geleneksel SOAP tabanlı Hizmetleri eğilimindedir.

<a name="reverse-geocode"></a> **Geocode ters**: Koordinat alma ve hangi adreste belirleme işlemini harita üzerinde temsil eder.

<a name="reproject"></a> **Reproject**: Bkz: [dönüştürme](#transformation).

<a name="rest-service"></a> **REST hizmeti**: Temsili durum aktarımı için kullanılan kısaltma. Merkezi olmayan, dağıtılmış bir ortamda eşler arasında bilgi alışverişi için bir mimari. REST programlar bir işletim sistemi veya platformdan bağımsız olarak bir Tekdüzen Kaynak Konum Belirleyicisi (URL) için bir Köprü Metni Aktarım Protokolü (HTTP) istek göndererek iletişim kurmak ve verileri geri almak için farklı bilgisayarlarda sağlar.

<a name="route"></a> **Rota**: Yönergeler için yol boyunca waypoints gibi ek bilgileri de içerebilir, iki veya daha fazla konum arasında bir yolu.

<a name="requests-per-second-rps"></a> **(RP'ler) saniye başına istek**: Bkz: [sorgular / saniye (QPS)](#queries-per-second-qps). 

<a name="rss"></a> **RSS**: Kaynak bağlı olarak kullanılan kısaltma gerçekten basit dağıtım, kaynak açıklaması Framework (RDF) Site özeti veya zengin Site özeti. Farklı Web siteler arasında içerik paylaşımını bir basit, yapılandırılmış XML biçimi. RSS belgelerin yazar, tarih, başlık, kısa bir açıklama ve köprü metnine gibi önemli meta verilerin öğeleri içerir. Bir kullanıcı (veya bir RSS yayımcı hizmeti) karar verin malzemeleri değer, bu bilgiler yardımcı olur, daha fazla araştırma.

## <a name="s"></a>S

<a name="satellite-imagery"></a> **Uydu tanımayı**: Düzlemleri ve düz aşağıyı Uyduları tarafından yakalanan görüntüler.

<a name="software-development-kit-sdk"></a> **Yazılım Geliştirme Seti (SDK)**: Belge, örnek kod ve geliştirici kullanım uygulamalar oluşturmak için bir API yardımcı olmak için örnek uygulamaları koleksiyonu.

<a name="shapefile-shp"></a> **Şekil dosyası (SHP)**: Olarak da bilinen bir ESRI şekil dosyasının konumu, Şekil ve coğrafi özelliklerinin öznitelikleri depolamak için bir vektör veri depolama biçimi olur. Bir şekil dosyası ilişkili dosyaları kümesi içinde depolanır.

<a name="spherical-mercator-projection"></a> **Küresel Mercator izdüşümünü**: Bkz: [Mercator Web](#web-mercator). 

<a name="spatial-query"></a> **Uzamsal sorgu**: Uzamsal işlemi gerçekleştiren bir hizmete yapılan bir istek. Böyle bir Radyal arama olarak veya bir rota arama boyunca.

<a name="spatial-reference"></a> **Uzamsal başvuru**: Tam olarak coğrafi varlıkları bulmak için kullanılan bir koordinat tabanlı yerel, Bölgesel ve genel sistem. Harita koordinatlarına göre gerçek dünyada konumlara ilişkilendirmek için kullanılan koordinat sistemini tanımlar. Uzamsal başvuruları doğru görüntüleme veya çözümleme için uzamsal veriler farklı katmanlar veya kaynaklarından tümleştirilebilir emin olun. Azure haritalar kullanan [EPSG:3857](https://epsg.io/3857) giriş geometri veri için başvuru sistemi ve WGS 84 koordinatı. 

<a name="sql-spatial"></a> **SQL uzamsal**: SQL Azure ve SQL Server 2008 ve üzeri yerleşik olan bir uzamsal işlevlerini gösterir. Bu uzamsal İşlevler, bağımsız olarak SQL Server kullanılan bir .NET kitaplığı olarak da kullanılabilir. Daha fazla bilgi için [uzamsal veriler (SQL Server) belgeleri](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) daha fazla bilgi için.

<a name="synchronous-request"></a> **Eşzamanlı istek**: Bir HTTP isteği, bir bağlantı açar ve bir yanıt bekler. Tarayıcılar sayfadan yapılan eşzamanlı HTTP istek sayısını sınırlayın. Aynı anda birden çok uzun süren zaman uyumlu istekleri yaptıysanız bu sınıra ulaşıldı ve istekleri diğer istekleri birine tamamlanıncaya kadar ertelendi.

## <a name="t"></a>T

<a name="telematics"></a> **Telematik**: Gönderme, alma ve telekomünikasyon cihazları aracılığıyla bilgileri uzak nesnelerde denetim etkileyen ile birlikte depolama. 

<a name="temporal-data"></a> **Zamana bağlı veriler**: Özellikle süreleri başvuruda bulunan veri veya tarih. Zamana bağlı veriler ışık durumda gibi ayrık olaylarına bakabilirsiniz; Tren gibi nesneleri taşıma; veya trafiği sensörlerden alınan sayımların gibi yinelenen gözlemler.

<a name="terrain"></a> **Arazi**: Land sandy Arazi veya mountainous Arazi gibi belirli bir özelliği olan bir alanı.

<a name="thematic-maps"></a> **Tematik haritalar**: Bir tema hakkında bir coğrafi bölge yansıtacak şekilde yapılan basit bir harita bir tematik haritasıdır. Bu tür bir eşleme için yaygın bir senaryo, bazı veri ölçüme göre ülkeler/bölgeler gibi yönetim bölgeleri renk sağlamaktır.

<a name="tile-layer"></a> **Döşeme katmanı**: Sürekli bir katmana harita kutucukları (dikdörtgen bölümlere) birleştirerek görüntülenen bir katman. Her iki tarama döşemeleri resim kutucukları veya kutucukları vektör. Izgara döşeme katmanları genellikle önceden işlenmiş ve sunucudaki bir görüntü olarak depolanır. Bu, depolama alanı çok fazla sürebilir. Vektör döşeme katmanları, istemci uygulamasının içinden hareket halindeyken işlenir, bu nedenle sunucu tarafı depolama gereksinimleri daha küçüktür.

<a name="time-zone"></a> **Saat dilimi**: Yasal, ticari ve sosyal amaçları için Tekdüzen bir Standart Saati uygulamasını gözlediğini dünya bölgesi. Saat dilimlerini ülkeler/bölgeler ve onların alt bölümleri sınırları izleyin eğilimindedir.

<a name="transaction"></a> **İşlem**: Azure haritalar kullanan bir işlem tabanlı lisanslama modeli burada;

- Bir işlem istenen her 15 eşlemesi ya da trafiği döşeme için oluşturulur.
- Bir işlem oluşturulur için her API çağrısı bir Azure haritalar Hizmetleri gibi arama veya yönlendirme.

<a name="transformation"></a> **Dönüştürme**: Farklı coğrafi koordinat sistemi arasında veri dönüştürme işlemidir. Örneğin, Birleşik Krallık'ta yakalanan ve OSGB 1936 coğrafi koordinat sistemini temel alan bazı veriler olabilir. Azure haritalar kullanan [EPSG:3857](https://epsg.io/3857) WGS84 koordinat başvuru sistemi değişken. Bu nedenle doğru verileri görüntülemek için başka bir sistemden dönüştürülmüş koordinatları sahip gerekecektir.

<a name="traveling-salesmen-problem-tsp"></a> **Gezici satıcı sorun (TSP)**:  Bir satış temsilcisi durdurur, bir dizi ziyaret etmek için en verimli şekilde bulmalıdır Hamiltonian devre sorun ardından başlangıç konumu döndürür.  

<a name="trilateration"></a> **Trilateration**: Tüm üç nokta arasındaki uzaklıkları ölçerek yeryüzünün diğer iki nokta ile ilgili bir noktasında konumunu belirleme işlemi.

<a name="turn-by-turn-navigation"></a> **Bırakma tarafından Aç Gezinti**: Rota yönergeleri her adım için bir rota kullanıcıları olarak sağlayan bir uygulama, sonraki öngören yaklaşıyor.

## <a name="v"></a>V

<a name="vector-data"></a> **Vektör verilerini**: Puan, çizgi veya çokgenler temsil edilen temel alan veri koordine edin.

<a name="vector-tile"></a> **Vektör kutucuk**: Harita denetiminin aynı döşeme sistemini kullanarak Jeo-uzamsal vektör verilerini depolamak için bir açık veri belirtimi. Ayrıca bkz: [döşeme katmanı](#tile-layer).

<a name="vehicle-routing-problem-vrp"></a> **Araç yönlendirme sorununu (VRP)**: Bir sınıf kümesi taşıtlardan filosundan için sıralı rota kısıtlamaları kümesini olarak dikkate alma sırasında hesaplandığı sorunları. Bu kısıtlamalar, teslim zaman pencereleri, birden çok yol kapasiteler gibi şeyler dahil ve seyahat süresi kısıtlamaları.

<a name="voronoi-diagram"></a> **Voronoi diyagram**: Alanlar veya hücre alanına ilişkin bir bölüm, geometrik bir nesne (genellikle noktası özellikleri) içine alın. Bu hücre veya çokgenler Delaunay üçgenler ölçütleri karşılaması gerekir. Bir alanı içindeki tüm konumları, kümedeki herhangi bir nesneye daha çevreleyen nesnesine yakındır. Voronoi diyagramları, genellikle coğrafi özellikler etrafında etki alanlarını ayırmak için kullanılır. 

## <a name="w"></a>W

<a name="waypoint"></a> **Güzergah noktası**: Bir güzergah noktası enlemini ve boylamını gezinme amacıyla kullanılan tarafından tanımlanan belirli bir coğrafi konumdur. Genellikle birisi bir rota üzerinden gider bir noktayı temsil etmek için kullanılır.

<a name="waypoint-optimization"></a> **Güzergah noktası iyileştirme**: Waypoints seyahat saati veya uzaklığı en aza indirmek için bir dizi yeniden sıralama işlemi sağlanan tüm waypoints geçirmek için gereklidir. Genellikle olarak adlandırılan [gezici satıcı sorun](#traveling-salesmen-problem-tsp) veya [Vehicle yönlendirme sorununu](#vehicle-routing-problem-vrp) iyileştirme karmaşıklığına bağlı olarak.

<a name="web-map-service-wms"></a> **Web (WMS) Map hizmeti**: WMS, görüntü tabanlı eşleme hizmeti tanımlayan bir açık coğrafi Consortium (OGC) standardıdır. WMS Hizmetleri, isteğe bağlı bir eşlem içindeki belirli alanları için harita görüntüleri sağlar. Görüntüleri, önceden işlenmiş simgesi içerir ve birkaç adlandırılmış stilleri birinde varsa hizmet tarafından tanımlanan çizilebilir.

<a name="web-mercator"></a> **Mercator web**: Olarak da bilinen küresel Mercator izdüşümünü bir hafif Mercator izdüşümünü, öncelikli olarak Web tabanlı eşleme programlarda kullanılan bir çeşididir. Formüller olarak kullanılan standart Mercator izdüşümünü küçük ölçekli haritalar için kullanır. Ancak, küresel formülleri hiç ölçeklendirir büyük ölçekli Mercator normalde eşleştirir ancak Web Mercator kullandığı projeksiyonun ellipsoidal formu kullanın. Tutarsızlık, global bir ölçekte imperceptible ancak aynı ölçekte ellipsoidal Mercator eşler true öğesinden biraz cluster_count_prıor yerel alanları haritaları neden olur.

<a name="wgs84"></a> **WGS84**: Harita yüzeyinde konumlara uzamsal koordinatları ilişkilendirmek için kullanılan sabit bir dizi. WGS84 datum en çevrimiçi eşleme sağlayıcılarının ve GPS aygıt tarafından kullanılan standart bir bileşendir. Azure haritalar kullanan [EPSG:3857](https://epsg.io/3857) WGS84 koordinat başvuru sistemi değişken.

## <a name="z"></a>Z

<a name="z-coordinate"></a> **Z koordinatı**: Bkz: [yüksekliği](#altitude). 

<a name="zip-code"></a> **Posta kodu**: Bkz: [postalcode](#postal-code).

<a name="Zoom level"></a> **Yakınlaştırma düzeyi**: Harita ne kadarının görünür olduğunu ve ayrıntı düzeyini belirtir. Tüm bir düzey 0 uzaklaştırılacağını, tam dünya haritası genellikle görünüm olacaktır, ancak sınırlı ayrıntıları ülke/bölge adları ve kenarlıklar gibi yanı ve Okyanus adları gösterilir. İçinde daha yakın düzeyi 17 uzaklaştırılacağını, harita birkaç Şehir blokları ayrıntılı yol bilgilerini ile bir alanı görüntüler. Daha fazla bilgi için [yakınlaştırma düzeyleri ve döşeme Kılavuzu](zoom-levels-and-tile-grid.md) belgeleri.

