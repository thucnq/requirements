# OSM Researching Notes

## OSM là gì?
OSM là một dự án mã nguồn mở (open-source project) cung cấp các công cụ phục vụ cho việc tạo và chia sẻ dữ liệu bản đồ. Dữ liệu bản đồ cung cấp bởi OSM là hoàn toàn miễn phí (non-profit) và có thể được sử dụng để xây dựng các ứng dụng liên quan đến bản đồ.

Code base của các project liên quan đến OSM trên GitHub: https://github.com/openstreetmap

## Cách hoạt động của OSM
Cách hoạt động cơ bản của OSM project như sau:
- Những người đóng góp (contributors) sử dụng các phần mềm chỉnh sửa dữ liệu bản đồ như JOSM và iD (tool được cung cấp bởi OSM) để tạo các thay đổi cho dữ liệu map hiện tại.
- Các thay đổi trên sẽ được submit lên OSM thông qua hệ thống các APIs mà OSM cung cấp.
- OSM sẽ tiếp nhận các thay đổi trên và thực hiện việc lưu trữ các thay đổi đó. OSM thực hiện việc quản lý toàn bộ dữ liệu bản đồ (kích thước khá lớn - trên 30GB cho dữ liệu đã nén). Dữ liệu bản đồ của OSM là mở và và có thể tải xuống miễn phí.
- OSM sẽ cung cấp các công cụ xung quanh để giúp người dùng thao tác trên dữ liệu bản đồ gốc, ví dụ **osm2pgsql** (chuyển đổi dữ liệu bản đồ gốc sang các bảng trong PostgreSQL), **Nominatim** (dùng để search dữ liệu trên bản đồ), **mod_tile** (Apache extension dùng để thực hiện việc serving các map tiles).

> Đối với beMaps OSM sẽ được sử dụng chính trong việc:
> - Lấy dữ liệu nền cho việc xây dựng bản đồ với một số layer cơ bản như Standard Layer và Transport Layer
> - Sử dụng các tool OSM cung cấp để chuyển đổi dữ liệu raw của OSM sang dạng dữ liệu phục vụ cho việc xây dựng ứng dụng

## Cách sử dụng dữ liệu từ OSM
Dữ liệu OSM được sử dụng cho 2 mục đích cơ bản:
- Sử dụng bởi các nhà nghiên cứu hệ thống thông tin địa lý (GIS user) cho việc chỉnh sửa, cập nhật, truy xuất và phân tích các dữ liệu bản đồ
- Sử dụng cho việc phát triển các ứng dụng có dữ dụng dữ liệu bản đồ. Thông thường OSM được sử dụng để xây dựng các layer cơ bản, các layer khác thường được xây dựng dựa trên mục đích của từng ứng dụng cụ thể. 

> Ví dụ đối với beMaps, một số layer quan trọng có thể kể ra là: Transport Layers và POIs Layers, Traffic Layers, ...

## Cách truy xuất dữ liệu từ OSM
Dữ liệu bản đồ từ OSM có thể được export trực tiếp từ OSM website, tuy nhiên có một số giớ hạn sau:
- Không cho phép export dữ liệu với vùng bản đồ quá lớn.
- Không hỗ trợ việc export cho từng vùng, từng nước hoặc từng thành phố
- Hiện tại chỉ hỗ trợ định dạng **.osm**

Dữ liêu bản đồ có thể được truy xuất qua một số nguồn sau:
- Planet OSM: https://planet.openstreetmap.org - cung cấp dữ liệu cho toàn bộ trái đất. Chỉ đưa ra ở đây với mục đích tham khảo, trên thực tế rất ít ứng dụng sử dụng toàn bộ dữ liệu của OSM.
- Overpass API: https://overpass-api.de
- Geofabrik: https://download.geofabrik.de

Các định dạng dữ liệu OSM:
- **XML**: có thể đọc được ở dạng text, khá phổ biến, tuy nhiên kích thước khá lớn do mất khá nhiều bytes để lưu dữ liệu.
- **OSM**: có thể đọc được ở dạng text, định dạng này chỉ được dử dụng cho dữ liệu bản đồ cung cấp bởi OSM. Định dang này phù hợp cho việc nghiên cứu cũng như chỉnh sửa dữ liệu bản đồ.
- **PBF**: dữ liệu dang binary, đã được nén sử dụng chuẩn Protocol Buffers của Google. Định dạng này có kích thước khá nhỏ và có tốc độ đọc/ghi nhanh hơn so với các dạng dữ liệu khác.

> Định dạng **PBF** sẽ được sử dụng chính trong beMaps

Ví dụ về việc tải dữ liệu từ OSM. Chúng ta sẽ sử dụng Geofabrik:
Dữ liệu trên Geofabrik được chia theo từng châu lục và từng quốc gia khác nhau

Ví dụ, dữ liệu cho khu vực châu Á sẽ được truy cập tại đây: https://download.geofabrik.de/asia
Dữ liệu cho Việt Nam nói riêng và các quốc gia khác nói chung sẽ gồm 2 phần chính:
- Dữ liệu hiện tại: https://download.geofabrik.de/asia/vietnam-latest.osm.pbf (khoảng 70MB)
- Dữ liệu cập nhật: https://download.geofabrik.de/asia/vietnam-updates

Dùng lệnh sau để tải dữ liệu từ Geofabrik
```
wget https://download.geofabrik.de/asia/vietnam-latest.osm.pbf
```
Ngoài các công cụ trên, chúng ta có thể sử dụng công cụ được cung cấp bởi Humanitarian OpenStreetMap Team để export dữ liệu cho các khu vực nhỏ hơn, như quận, huyện, thành phố: https://export.hotosm.org

> beMaps sẽ sử dụng nguồn dữ liệu bản đồ chính từ Geofabrik

## OSM API
OSM cung cấp một hệ thống các APIs cho phép các contributors tạo và cập nhật các thay đổi trên dữ liệu bản đồ hiện tại. Các API requests sẽ được gửi đến và được xử lý bởi các servers của OSM.

API documentation: https://wiki.openstreetmap.org/wiki/API_v0.6

> Đối với beMaps, chúng ta không cần quan tâm quá nhiều đến các APIs trên. APIs thường được sử dụng chính bởi các contributors. Ngoài ra các APIs trên được hosted và quản lý bởi OSM, nên việc sử dụng chúng thường có giới hạn. Các APIs này có thể được sử dụng để tìm hiểu về cấu trúc dữ liệu bên dưới của OSM.

## OSM Data Model
Dữ liệu bản đồ OSM được cấu thành từ các features, mỗi feature sẽ được cấu thành từ các **geometries** và các **attributes** (hay còn gọi là metadata).

**Geometries** trong OSM bao gồm: **node**, **way** và **relation**

**Atributes** còn được gọi là **tags**

### Geometries
#### Node
**Node** được dùng để biểu diễn các điểm trên bản đồ thông thường là cách **POIs**. Một node có thể chứa thêm các tags dùng để cung cấp các thông tin liên quan đến node đó. Bên dưới là một số ví dụ về node trong OSM:

Một node cơ bản, không có thông tin metadata (tags). Một node sẽ chứa các thông tin cơ bản như: latitude, longitude, visible, và timestamp (thời gian được update)
```
<node id="1008752931" visible="true" version="1" changeset="6472947" timestamp="2010-11-27T20:20:01Z" user="richlv" uid="47892" lat="21.0179339" lon="105.8432779"/>
```
Node có thể chứa thêm các tags, dùng để biểu diễn cách thông tin khác liên quan đến node như, số nhà, tên đường,...
```
<node id="1008752956" visible="true" version="3" changeset="72909444" timestamp="2019-08-01T17:47:19Z" user="mama_bear" uid="6980194" lat="21.0199102" lon="105.8496639">
  <tag k="addr:country" v="VN"/>
  <tag k="addr:housenumber" v="80a"/>
  <tag k="addr:street" v="Phố Bà Triệu"/>
 </node>
```

#### Way
Các nodes có thể liên kết với nhau để tạo thành các **ways** (ví dụ như một con đường, 1 dòng sông).

Nếu node đầu vào node cuối của một way được liên kết với nhau, ta có một **closed way**. Cấu trúc này có thể được sử dụng để tạo thành các tòa nhà, công viên, hồ nước,...

Ngoài ra way có thể chứa thêm các tags để cung cấp thêm các metadata cần thiết.

Một số ví dụ về way

A closed way:
```
<way id="9566428" visible="true" version="9" changeset="72991951" timestamp="2019-08-04T15:08:38Z" user="TuanIfan" uid="625303">
  <nd ref="74127918"/>
  <nd ref="309602506"/>
  <nd ref="74127919"/>
  <nd ref="5857305469"/>
  <nd ref="309602519"/>
  <nd ref="74127921"/>
  <nd ref="309602531"/>
  <nd ref="74127922"/>
  <nd ref="309602462"/>
  <nd ref="74127918"/>
  <tag k="name" v="Đảo Rùa"/>
  <tag k="name:en" v="Turtle Isle"/>
 </way>
```
A normal way:
```
<way id="9566542" visible="true" version="10" changeset="68385112" timestamp="2019-03-21T19:14:26Z" user="KB Kang" uid="680210">
  <nd ref="1893253381"/>
  <nd ref="1893253440"/>
  <tag k="bridge" v="yes"/>
  <tag k="highway" v="footway"/>
  <tag k="layer" v="1"/>
  <tag k="material" v="wood"/>
  <tag k="name" v="Cầu Thê Húc"/>
  <tag k="name:de" v="Brücke der aufgehenden Sonne"/>
  <tag k="name:en" v="Morning Sunlight Bridge"/>
  <tag k="name:id" v="Jambatan The Huc"/>
  <tag k="name:ko" v="테훅교"/>
  <tag k="name:ru" v="Мост Восходящего солнца"/>
  <tag k="name:vi" v="Cầu Thê Húc"/>
  <tag k="tourism" v="attraction"/>
 </way>
```

#### Relation
Relation được sử dụng để tổ chức các nodes và ways thành một tổng thể nhất quán. Ví dụ hồ Hoàn Kiếm sẽ được bao quanh bởi mốt số con đường nhất định. Relation cũng sẽ chứa các tags dùng để mô tả chi tiết relation đó.

Ví dụ về relation:
```
<relation id="198437" visible="true" version="13" changeset="75008366" timestamp="2019-09-27T12:38:04Z" user="Marc Choisy" uid="8836956">
  <member type="way" ref="9566396" role="outer"/>
  <member type="way" ref="39168425" role="inner"/>
  <member type="way" ref="9566428" role="inner"/>
  <tag k="alt_name" v="Hồ Gươm"/>
  <tag k="alt_name:de" v="Schwertsee"/>
  <tag k="alt_name:en" v="Sword Lake"/>
  <tag k="alt_name:vi" v="Hồ Gươm"/>
  <tag k="name" v="Hồ Hoàn Kiếm"/>
  <tag k="name:de" v="Hoan-Kiem-See"/>
  <tag k="name:en" v="Hoàn Kiếm Lake"/>
  <tag k="name:id" v="Danau Hoan Kiem"/>
  <tag k="name:ja" v="ホアンキエム湖"/>
  <tag k="name:ko" v="호안끼엠 호"/>
  <tag k="name:vi-hani" v="湖還劍"/>
  <tag k="name:zh" v="还剑湖"/>
  <tag k="name:zh-Hans" v="还剑湖"/>
  <tag k="name:zh-Hant" v="還劍湖"/>
  <tag k="natural" v="water"/>
  <tag k="old_name" v="Hồ Lục Thủy"/>
  <tag k="old_name:vi" v="Hồ Lục Thủy"/>
  <tag k="old_name:zh" v="绿水湖"/>
  <tag k="type" v="multipolygon"/>
  <tag k="water" v="lake"/>
  <tag k="wikidata" v="Q1151254"/>
  <tag k="wikipedia" v="vi:Hồ Hoàn Kiếm"/>
 </relation>
```

#### Tags
Được dùng để mô tả chi tiết các thành phần của dữ liệu bản đồ. Tag được cấu thành dưới dạng key-value. Một số loại tags có thể kể ra:
- Tên đường
- Loại đường
- Số nhà
- Chất liệu
- ...

Ví dụ về tags:
```
<tag k="name" v="Hà Nội"/>
<tag k="amenity" v="cafe"/>
<tag k="internet_access" v="wlan"/>
<tag k="tourism" v="hotel"/>
```

## OSM System Architecture 

![OSM System Components](https://wiki.openstreetmap.org/w/images/2/27/OSM_Components.svg)

Cấu trúc hệ thống của OSM là khá phức tạp. Tuy nhiên chúng ta sẽ chỉ tập trung vào một số thành phần quan trọng của hệ thống này. Cụ thể OSM system sẽ được cấu thành từ các thành phần cơ bản sau:

- OSM Geodata store: là một cơ sở dữ liệu quan hệ, thường là  PostgreSQL được dùng để lưu trữ nodes, ways và relations
- OSM APIs: cung cấp các APIs dùng để thực hiện việc cập nhật dữ liệu bản đồ cũng như truy xuất các dữ liệu bản đồ từ OSM server.
- Backend: cung cấp các endpoints cho việc searching, geocoding và reverse-geocoding trên bản đồ.
- Renderers: được dùng để chuyển đổi dữ liệu gốc từ OSM thành các tiles. Các tiles đó sẽ được dùng để thực hiện việc render map trên các môi trường frontend như web browser hoặc mobile application.
- Visualization: Sử dụng dữ liệu từ các renderers để tạo thành các dạn map động (interactive map). Thông thường bao gồm việc tạo và xử lý các map layers.
- Frontend: Là thành phần dùng để tạo lên giao diện người dùng của OpenStreetMap.

### OSM Database
OSM sử dụng hệ cở sở dữ liệu quan hệ PostgreSQL để lưu trữ các dữ liệu bản đồ. Lý do dùng PostgreSQL:
- PostgreSQL hỗ trợ tốt cho việc lưu trữ các dữ liệu dạng geometry (geometric data types).
- PostGIS là một extension cho PostgreSQL được phát triển để tăng khả năng xử lứ các dữ liệu liên quan đến bản đồ như cung cấp các dạng dữ liệu mới cũng như cho phép truy xuất dữ liệu bản đồ trực tiếp qua các database queries.

Reference: https://postgis.net









