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

> beMaps sẽ sử dụng PostgreSQL và PostGIS extension để lưu trữ và truy xuất dữ liệu bản đồ từ OSM

#### osm2pgsql
OSM community phát triển một công cụ để chuyển đổi dữ liệu raw từ OSM sang dạng dữ liệu được lưu trong các bảng của PostgreSQL - **osm2pgsql**. Reference: https://github.com/openstreetmap/osm2pgsql

Các cài đặt phiên bản mới nhất của osm2pgsql
```
mkdir ~/src
cd ~/src
git clone git://github.com/openstreetmap/osm2pgsql.git
cd osm2pgsql

sudo apt install make cmake g++ libboost-dev libboost-system-dev libboost-filesystem-dev libexpat1-dev zlib1g-dev libbz2-dev libpq-dev libgeos-dev libgeos++-dev libproj-dev lua5.2 liblua5.2-dev

mkdir build && cd build
cmake ..

make
sudo make install
```

Sau khi cài đặt chúng ta có thể acccess command - **osm2pgsql*

Trước khi convert dữ liệu chúng ta cần tải dữ liệu raw từ OSM, ví dụ:
```
wget https://download.geofabrik.de/asia/vietnam-latest.osm.pbf
```
Để convert và insert dữ liệu OSM vào database, chúng ta sử dụng lệnh sau:
```
osm2pgsql -d gis --create --slim  -G --hstore --tag-transform-script ~/src/openstreetmap-carto/openstreetmap-carto.lua -C 2500 --number-processes 1 -S ~/src/openstreetmap-carto/openstreetmap-carto.style ~/vietnam-latest.osm.pbf
```
Quá trình này tiêu tốn khá nhiều tài nguyên hệ thống như CPU, RAM và Disk I/O. Thông thường để import toàn bộ dữ liệu Việt Nam chúng ta cần vài chục phút đến 1 hoặc 2 tiếng, phụ thuộc vào cấu hình máy.

Giải thích command phía trên:
```
-d gis
```
Ở đây chúng ta sẽ định nghĩa tên database mà osm2pgsql sẽ dùng để lưu dữ liệu. Trong ví dụ trên tên database là **gis**

```
--create
```
Load dữ liệu vào một database trống thay vì một database đã tồn tại dữ liệu

```
--slim
```
Database table layout được osm2pgsql sử dụng, ở đấy "slim" layout phù hợp cho quá trình rendering sau này.

```
--hstore
```
Cho phép các tags không tương ứng với bất kì cột nào trong database được sử dụng cho việc rendering

```
--tag-transform-script
```
Script được sử dụng để transform các tags trước khi apply các styles khác nhau cho tags.

```
-C 2500
```
Lượng RAM sẽ được sử dụng cho quá trình import (theo MB). Trong ví dụ trên ta dùng 2.5GB

```
--number-process 1
```
Số core CPU được sử dụng cho quá trình import

Và cuối cùng là file dữ liệu gốc từ OSM - định dạng là .pbf

#### PostgreSQL Tuning
Dữ liệu OSM sau khi được convert và lưu trữ trong PostgreSQL cần được đánh index chi tiết nhằm tăng performance khi thực hiện các công việc như searching. OSM cung cấp một số hướng dẫn nhằm tối ưu hiệu năng của PostgreSQL - https://wiki.openstreetmap.org/wiki/PostgreSQL#Tune_the_database

Tuy nhiên hiện tại be sử dụng Cloud Platform cho việc cấu hình database nên việc tuning PostgreSQL sẽ có những khác biệt. Việc tuning PostgreSQL không nằm trong ph

#### OSM Database Schema

![OSM Database](https://wiki.openstreetmap.org/w/images/5/58/OSM_DB_Schema_2016-12-13.svg)

Cơ sở dữ liệu hiện tại là khá phức tạp và phục vụ cho khá nhiều chức năng như: user authentication, changesets, user messaging, user diaries,..

Chúng ta chỉ quan tâm các các bảng dùng để lưu trữ dữ liệu map hiện tại, cụ thể là các bảng sau:
- changesets
- changeset_tags
- current_ways
- current_way_tags
- current_way_nodes
- current_nodes
- current_node_tags
- current_relations
- current_relation_tags

> Cấu trúc dữ liệu hiện tại của OSM sẽ được sử dụng như một tài liệu tham khảo khi thiết kế database cho beMaps

OSM sử dụng nhiều cơ sở dữ liệu khác nhau cho các tác vụ khác nhau đối với dữ liệu map. Một số ví dụ như:
- Database schema cung cấp bởi **osm2pgsql**: được sử dụng chủ yếu cho việc lưu trữ dữ liệu về node, ways, relations. Dữ liệu đó sau đó sẽ được sử dụng để render các tiles.
- Database schema cung cấp bởi **Nominatim**: được sử dụng chủ yếu cho việc searching map data.

Các yếu tố quyết định việc lựa chọn database schema phù hợp:
- Updatable: ứng dụng cần sử dụng các dữ liệu bản đồ mới nhất từ OSM. Dữ liệu sẽ được cập nhật sử dụng **osmChange** (https://wiki.openstreetmap.org/wiki/OsmChange) và không cần import toàn bộ dữ liệu lại từ đầu.
- Geometries: schema cần hỗ trợ việc lưu trữ các dữ liệu dạng geometry hay không?
- Lossless: toàn bộ thông tin từ OSM sẽ được lưu trữ bao gồm: version, thông tin user, changeset và tags. Những thông tin này là cần thiết cho quá trình chỉnh sửa và update dữ liệu bản đồ. Tuy nhiên trong các ứng dụng thực tế, chúng ta chỉ cần dữ liệu ở dạng **lossy**, dữ liệu bản đồ chỉ cần đủ để phục vụ cho việc routing, geocoding,..

Dưới đây là danh sách các database schema thường được sử dụng với OSM data.

![](./osm-db-schemas.png)

Đối với beMaps, chúng ta sẽ sử dụng 2 database schema chính sau:
- **osm2pgsql schema**: database schema này được sử dụng để lưu trữ nodes, ways, relations.
- **Nominatim schema**: dùng để lưu trữ dữ liệu phục vụ cho việc searching.


**osm2pgsql schema**
![](./osm2pgsql-schema.jpg)

osm2pgsql sử dụng schema này để lưu trữ dữ liệu sau khi convert dữ liệu OSM gốc sang PostgreSQL. Các bảng chính được sử dụng để lưu từng loại dữ liệu tương ứng với OSM data model.
- osm_point
- osm_nodes
- osm_line
- osm_polygon
- osm_rels (relations)
- osm_roads
- osm_ways

**Nominatim schema**
Nominatim schema khá phức tạp với việc chưa rất nhiều **partitioned tables** được dùng để lưu các dữ liệu phục vụ cho việc searching.
Cấu trúc database của Nominatim có thể tham khảo tại đây: https://github.com/openstreetmap/Nominatim/tree/master/sql

Đối với beMaps, việc sử dụng PostgreSQL hoàn toàn cho việc search sẽ không hiệu quả và thường không mở rộng (scaling) được. Một phương pháp để giải quyết vấn đề này là sử dụng Elasticsearch để cải thiện tốc độ và độ chính xác của quá trình searching.

Elastic hỗ trợ tốt trong việc xử lý các loại dữ liệu liên quan đến bản đồ
https://www.elastic.co/webinars/geospatial-applications-with-elasticsearch

![](./osm-elasticsearch.png)

- Dữ liệu từ OSM, sẽ được lọc, chuẩn hóa và index vào Elasticsearch
- Elasticsearch có thể được scale sử dụng các kĩ thuật như **sharding** và **clustering**
- beMaps backend có thể thực hiện các **geographic queries** sử dụng các Elasticsearch index.

### OSM Frontend
OSM frontend được viết sử dụng các ngôn ngữ chính là Ruby (sử dụng Ruby on Rails framework), JavaScript và Perl

Các thành phần chính của OSM frontend bao gồm:
- The front page map
- Data browser
- User diaries
- User messaging
- Friends
- Permissions and roles

Chúng ta chỉ cần quan tâm đến 2 thành phần quan trọng là **front page map** và **data browser**. Các thành phần khác không thực sự phù hợp trong ngữ cảnh của beMaps

#### The front page map

Component này được sử dụng để hiện thị map, thực hiện việc routing và truy xuất các dữ liệu liên quan đến các thành của map như roads, POIs,...

##### Displaying map
Đây là quá trình render các tiles lên các môi trường như web browser. OSM sử dụng một kĩ thuật với tên gọi là **Slippy Map**.

**Slippy Map** sử dụng AJAX để load dữ liệu từ server. Dữ liệu map dược tải xuống từ OSM server ở background sử dụng Javascript (không cần phải tải lại toàn bộ trang). Quá trình này giúp cho việc thực hiện các hành động như zooming và panning trên map trở nên mượt mà hơn (giúp cải thiện trải nghiệm người dùng).

Nhìn chung, **displaying map** là quá trình tạo các tiles và serving các map tiles đó sử dụng **tile server**.

Có khá nhiều công cụ open-source có thể được sử dụng để xây dựng các hệ thống map động (interactive map). Hai công cụ khá phổ biến có thể áp dụng vào beMaps bao gồm:
- **OpenLayers** (https://openlayers.org): được sử dụng để hiển thị các map tiles, các dữ liệu khác như vector data, map markers được lấy từ OpenStreetMap hoặc các nguồn dữ liệu khác như (Bing, MapBox, Satemen,...). OpenLayers có thể được sử dụng để quản lý các layers vào tạo các tương tác (interactions) trên các layer đó. OpenLayers được viết bằng JavaScript, do vậy công cụ này khá phù hợp cho môi trường web.
- **Leaflet** (https://leafletjs.com): Tương tự như OpenLayers, Leaflet là một thư viên JavaScript dùng để tạo các map động. Leaflet có thể được sử dụng để render các map tiles, tạo map markers, tạo các hành động như zooming, panning, scaling,...

##### Routing
Chúng ta sẽ có một section riêng để bàn chi tiết về việc routing trong OSM. Quá trình routing trong OSM được thực hiện sử dụng **OSRM** và **GraphHopper**

**OSRM**: là một công cụ open-source hoàn toàn được viết bằng C++. OSRM cung cấp các toolkit cho việc xây dựng cả backend và frontend để giải quyết vấn đề routing. Các bài toán mà OSRM có thể giải quyết bao gồm:
- Nearest: tìm các điểm lân cận cho một tọa độ cho sẵn (thường là tìm các POIs)
- Route: tìm đường đi nhanh nhất giữa hai tọa độ cho trước.
- Table (distance matrix): tính toán khoảng cách, thời gian, và cách đi nhanh nhất giữa các cặp tọa độ cho trước.
- Match: tìm tọa độ dựa vào tín hiệu GPS
- Trip: giải bài toán Travelling Salesman Problem
- Tile: tạo các Mapbox vector tiles với các thông tin routing đính kèm.

Reference: https://github.com/Project-OSRM/osrm-backend/blob/master/docs/http.md

Một số ví dụ sử dụng OSRM API để tìm đường ngắn nhất giữa hai điểm trên bản đồ.

API cần sử dụng là:
```
GET /route/v1/{profile}/{coordinates}?alternatives={true|false|number}&steps={true|false}&geometries={polyline|polyline6|geojson}&overview={full|simplified|false}&annotations={true|false}
```

Trong đó:

|Option      |Giá trị                                       |Mô tả                                                                    |
|------------|---------------------------------------------|-------------------------------------------------------------------------------|
|alternatives|`true`, `false` (default), or Number         |Search for alternative routes. Passing a number `alternatives=n` searches for up to `n` alternative routes.\*                            |
|steps       |`true`, `false` (default)                    |Returned route steps for each route leg                                        |
|annotations |`true`, `false` (default), `nodes`, `distance`, `duration`, `datasources`, `weight`, `speed`  |Returns additional metadata for each coordinate along the route geometry.      |
|geometries  |`polyline` (default), `polyline6`, `geojson` |Returned route geometry format (influences overview and per step)              |
|overview    |`simplified` (default), `full`, `false`      |Add overview geometry either full, simplified according to highest zoom level it could be display on, or not at all.|
|continue\_straight |`default` (default), `true`, `false`  |Forces the route to keep going straight at waypoints constraining uturns there even if it would be faster. Default value depends on the profile. |
|waypoints   | `{index};{index};{index}...`                |Treats input coordinates indicated by given indices as waypoints in returned Match object. Default is to treat all input coordinates as waypoints.    |

Giả sử chúng ta cần tìm đường ngắn nhất giữa Hồ Hoàn Kiếm và Nhà thờ Lớn, sử dụng request sau:
```
http://router.project-osrm.org/route/v1/driving/105.85239,21.02876;105.85025,21.02703?overview=false&alternatives=true&steps=true
```
Dữ liệu trả về sẽ tương tự như sau
```
{
  "code": "Ok",
  "routes": [
    {
      "distance": 569.5,
      "duration": 252.3,
      "legs": [
        {
          "distance": 569.5,
          "duration": 252.3,
          "steps": [
            ...
            {
              "distance": 8.1,
              "driving_side": "right",
              "duration": 5.8,
              "geometry": "qyi_Ck{`eSMD",
              "intersections": [
                {
                  "bearings": [
                    165,
                    195,
                    345
                  ],
                  "entry": [
                    true,
                    false,
                    true
                  ],
                  "in": 1,
                  "location": [
                    105.850299,
                    21.026969
                  ],
                  "out": 2
                }
              ],
              "maneuver": {
                "bearing_after": 340,
                "bearing_before": 16,
                "location": [
                  105.850299,
                  21.026969
                ],
                "modifier": "straight",
                "type": "turn"
              },
              "mode": "driving",
              "name": "Phố Nhà Chung",
              "weight": 5.8
            },
            {
              "distance": 0,
              "driving_side": "right",
              "duration": 0,
              "geometry": "_zi_Ce{`eS",
              "intersections": [
                {
                  "bearings": [
                    161
                  ],
                  "entry": [
                    true
                  ],
                  "in": 0,
                  "location": [
                    105.850273,
                    21.027038
                  ]
                }
              ],
              "maneuver": {
                "bearing_after": 0,
                "bearing_before": 341,
                "location": [
                  105.850273,
                  21.027038
                ],
                "type": "arrive"
              },
              "mode": "driving",
              "name": "Phố Nhà Chung",
              "weight": 0
            }
          ],
          "summary": "Phố Lê Thái Tổ, Phố Quang Trung",
          "weight": 339.2
        }
      ],
      "weight": 339.2,
      "weight_name": "routability"
    }
  ],
  "waypoints": [
    {
      "distance": 102.32887047821046,
      "hint": "-uFRhv___38AAAAAVQAAAGYBAABLAQAAAAAAAJZP4kFaSjxD_vt8QgAAAAArAAAAZgEAAHMAAADOXAAADipPBrHfQAHmLU8GmN9AAQUA3wt1cxsx",
      "location": [
        105.851406,
        21.028785
      ],
      "name": "Phố Lê Thái Tổ"
    },
    {
      "distance": 2.549732775530881,
      "hint": "rOFRhv___386AAAASwEAAAAAAADXAAAAmKYBQQ0GTkIAAAAAyNQaQjoAAACmAAAAAAAAAGwAAADOXAAAoSVPBt7YQAGKJU8G1thAAQAATxV1cxsx",
      "location": [
        105.850273,
        21.027038
      ],
      "name": "Phố Nhà Chung"
    }
  ]
}

```

**GraphHopper**: cung cấp các API cho việc xử lý directions and routing. Công cụ này không open-source hoàn toàn, tuy nhiên nó có khá nhiều chức năng liên quan đến việc giải quyết các bài toàn tối ưu. Đặc biệt công cu này cung cấp cả chức năng khá quan trong là *geocoding*. Cụ thể như sau:
- Routing and nagivation
- Routing optimization
- Geocoding
- Time-dependent route optimization
- Map matching
- Distance matrices
- Isochrone calculation

**GraphHopper** cũng cung cấp một số công cụ open-source. Trong đó 2 công cụ nổi bật nhất là:
- Jsprit (https://github.com/graphhopper/jsprit): một java-based toolkit được sử dụng để giải quyết bài toán Traveling Salesman Problems cũng như Vehicle Routing Problems
- Routing Engine (https://github.com/graphhopper/graphhopper): được sử dụng để giải quyết các bài toán liên quan đến routing như tìm đương đi ngắn nhất. Routing Engine này làm việc khá hiểu quả với OpenStreetMap data.

> Đối với beMaps, chúng ta sẽ tập trung chủ yếu vào OSRM. Trong những phần sau, chúng ta sẽ tìm hiểu rõ hơn về các thuật toán OSRM sử dung, các APIs cho backend mà OSRM cung cấp, cũng như cách để triển khai OSRM trên môi trường th

##### Querying Tool
Chức năng này cho phép người dùng click vào một điểm trên map và dữ liệu liên quan đến điểm đó - POIs sẽ được trả về.

OSM sử dụng Overpass API (http://overpass-api.de) để truy xuất các điểm POIs trên bản đồ.

> Đối với beMaps, chúng ta sẽ không sử dụng API được cung cấp bởi Overpass. Chúng ta cần tập hợp dữ liệu POIs từ các nguồn khác nhau (đặc biệt là Goong) và xây dựng thế thống các APIs để thực hiện truy xuất nguồn dữ liệu đó. Overpass khá tốt, tuy nhiên nguồn dữ liệu của Overpass là hạn chế và không được cập nhật thường xuyên.


##### Data layer overview
Trong phần sau chúng ta sẽ làm rõ hơn về map layers trong OpenStreetMap. OpenStreetMap sử dụng một số layer cơ bản sau:
- Standard layer: layer mặc định, cung cấp bởi OSM
- Cycle Map: cung cấp bởi Thunderforest - https://www.thunderforest.com
- Transport Map:  cung cấp bởi Thunderforest - https://www.thunderforest.com
- Humanitarian: cung cấp bởi Humanitarian OpenStreetMap team.

Mỗi map layer cung cấp một hiệu ứng hình ảnh khác nhau. Các map layer có thể được sử dụng cùng nhau để cung cấp nhiều dữ liệu hơn cho map hiện tại.

#### Data browser
Các thành thần của map (node, way, relation,...) sẽ có đường dẫn truy cập tương ứng trên OSM. Truy cấp các URL đó sẽ cung cấp các dữ liệu chi tiết về các thành phần trên.

### Tiles, Tile Rendering và Tile Renderers
#### Tiles
Lưu trữ và render toàn bộ dữ liệu bản đồ của một vùng rộng là không khả thi và tốn khá nhiều tài nguyên hệ thống. Do vậy OSM và các nhà cung cấp bản đồ khác như Google Maps sử dụng **map tiles**. **Tiles** là các ảnh bitmap hình vuông, được sử dụng trong một grid để hiển thị bản đồ của một khu vực.

**Tiles** thường có kích thước 256x256 pixels. Chúng được render từ các cơ sở dữ liệu bản đồ. Ngoài ra **tiles** cũng có thể có các size khác như:
- 64x64: sử dụng cho môi trường mobile
- 512x512: sử dụng cho map có độ phân giải cao

OSM không cho phép sử dụng các **tiles** được cung cấp trực tiếp từ OSM server. Để sử dụng OSM chúng ta có thể:
- Sử dụng dữ liệu OSM, import dữ liệu đó vào PostgreSQL và sử dụng đữ liệu đã import để tạo các map tiles
- Sử dụng các dịch vụ của bên thứ 3 như OpenMapTiles (https://openmaptiles.org)

> Đối với beMaps, việc tạo các tile sẽ được thực hiển bởi be mà không sử dụng bên thứ 3.

**Tileset** là tập hợp các tiles dùng để tạo nên một khu vực trên map. Tileset thường có zoom levels cho phép hiển thị các mức độ chi tiết khác nhau của bản đồ. Tileset thường không được hiển thị cùng một thời điểm OSM sử dụng các thư viện JavaScript giúp cho việc tải và hiển thị các map tiles được diễn ra ở background. Một số thư viện phổ biến:
- OpenLayers
- Leaflet

#### Tile Rendering
Title rendering là quá trình chuyển đổi dữ liệu map của OSM sang các map tiles và hiển thị các map tiles đó trên frontend (web browser hoặc mobile applications). Quá trình này thường tiêu tốn khá nhiều tài nguyên hệ thống.

Các map tiles không được tạo theo thời gian thực. Tiles thường được tạo trước và lưu lại trên đĩa cứng.

#### Serving Tiles
Tiles được cung cấp đến người dùng cuối thông qua **tile servers**

![](https://switch2osm.org/serving-tiles/serving-tiles.png)

Chúng ta sẽ đi qua một ví dụ đơn giản trong việc khởi tạo và serving map tiles cho tập dữ liệu là bản đồ của Việt Nam.

Serving tiles yêu cầu cấu hình server tương đối tốt và ổn định. Ở đây chúng ta sẽ giả sử server có cấu hính như sau:
- Storage: 10 - 20GB
- Memory 4GB
- 4-cores CPU

OSM sử dụng Apache web server để tiếp nhận requests và truyền các requests đó đến **mod_tile**. **mode_tile** là một extension của Apache phục vụ cho việc rendering và serving các map tiles.

Các tác vụ chính của **mod_tile** bao gồm:
- Kiểm tra tile đã được tạo và sẵn sàng để sử dụng
- Kiểm tra tile xem có cần được update không
- Nếu tile đã sẵn sàng, tile sẽ được gửi trực tiếp về cho bên client
- Nếu tile cần update hoặc render từ đầu, mod_tile sẽ khởi tạo một **rendering request** và đẩy request đó vào hàng đợi (queue). Renderer sẽ pop request từ hàng đợi, thực hiện việc render và trả lại dữ liệu cho bên client.

Để xây dựng một tile server đơn giản, chúng ta cần các software sau:
- **mod_tile** for serving (Apache)
- **renderd** as rendering daemon
- **mapnik** for the actual render

Đầu tiên chúng ta cần cài đặt các packages cần thiết
```
sudo apt install software-properties-common
sudo add-apt-repository ppa:kakrueger/openstreetmap
sudo apt update
sudo apt install libapache2-mod-tile
```
Các lệnh trên sẽ giúp chúng ta cài đặt **mod_tile** và các phụ thuộc cần thiết khác.

Tiếp theo chúng ta cần download dữ liệu bản đồ Việt Nam và import dữ liệu đó vào PostgreSQL (đã trình bày trong phần **osm2pgsql**)

```
wget https://download.geofabrik.de/asia/vietnam-latest.osm.pbf
osm2pgsql --slim -C 1500 --number-processes 4 vietnam-latest.osm.pbf
```

**mod_tile** được sử dụng để render các tiles với các cập nhập mới nhất. Các tile cần update sẽ được **mod_tile** xử lý trong quá trình serving. **mod_tile** sẽ kiểm tra các timestamp của tile và detect các tile đã outdated.

Cuối cùng chúng ta cần khởi động lại rendering deamon:
```
sudo /etc/init.d/renderd restart
```

#### Tile renderers
OSM sử dụng Mapnik để render layer chuẩn.
Mapnik thực hiện việc render thông quá **mod_tile**. Mapnik đồng thời quản lý việc caching và queueing các rendering requests. Khi một tile được đánh giá là **dirty** (outdated) dựa vào timestamp của tile đó, Mapnik sẽ thực hiện việc render lại tile đó.

## Nominatim 
Nominatim là công cụ tìm kiếm dữ liệu OSM bằng cách nhập tên, địa chỉ hoặc tọa độ.
### Nominatim database
![Nominatim database](./nominatim-db-schemas.png)

### Geocoding API
Geocoding API cho phép tìm kiếm một hoặc nhiều vị trí từ văn bản mô tả. Nominatim hỗ trợ tìm kiếm bằng cả hai kiểu có cấu trúc và tự do. Với beMap chúng ta quan tâm đến dạng tìm kiếm bằng văn bản tự do.
#### Cú pháp
```
<domain>/search?<query>
```
Trong đó: 
- `domain` là tên miền / địa chỉ máy chủ (ví dụ: https://nominatim.openstreetmap.org, localhost:3000)
- `query` là truy vấn với các tham số bên dưới

#### Tham số

|Tham số     | Loại tham số | Ý nghĩa | Khoảng giá trị |Ví dụ |
|------------|--------------|---------|---------------|-------|
|q|chính| (viết tắt của query) văn bản hoặc chuỗi ký tự mô tả về đại điểm muốn định vị |  |q=ha+noi |
|format|định dạng kết quả trả về| yêu cầu kết quả là 1 trong các định dạng cột bên|xml / json / jsonv2 / geojson / geocodejson/ html. Mặc định: xml | format=html|
|addressdetails|       chi tiết kết quả   | thêm chi tiết vị trí vào kết quả hay không |  0 hoặc 1. Mặc định: 1| addressdetails=0|
|namedetails|      chi tiết kết quả    | thêm các tên khác của địa chỉ vào kết quả hay không |0 hoặc 1. Mặc định: 0| namedetails=1|
|countrycodes|      lọc kết quả    | giới hạn kết quả trong một số quốc gia | Cột country code trong bảng [Country codes](https://wiki.openstreetmap.org/wiki/Nominatim/Country_Codes) | countrycodes=vn,de|
|exclude_place_ids|     lọc kết quả     |id Nominatim của một số địa điểm không mong muốn|  |exclude_place_ids=595794,595795 |
|limit|lọc kết quả| giới hạn số kết quả trả về |0 < limit <= 50. Mặc định: 10 |limit=1 |
|email|khác|email xác thực khi gọi số lượng lớn yêu cầu|  |email=map@be.xyz |

#### Ví dụ
- Tác nhân muốn lấy thông tin chi tiết vị trí đầu tiên có với từ khoá `hồ gươm`, định dạng `json`, trên lãnh thổ `Việt Nam`.

Yêu cầu: 
```
GET: https://nominatim.openstreetmap.org/?addressdetails=1&q=hồ+gươm&format=json&limit=1&countrycodes=vn
```

Kết quả:
```
[
  {
    "place_id": 198136364,
    "licence": "Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright",
    "osm_type": "relation",
    "osm_id": 198437,
    "boundingbox": ["21.0259197", "21.0316049", "105.8512023", "105.8535692"],
    "lat": "21.0287601",
    "lon": "105.852585241026",
    "display_name": "Hồ Hoàn Kiếm, Phố Tràng Thi, Hàng Trống, Hoàn Kiếm, Quận Hoàn Kiếm, Hà Nội, 01235, Việt Nam",
    "class": "natural",
    "type": "water",
    "importance": 0.482526166251558,
    "address": {
      "water": "Hồ Hoàn Kiếm",
      "road": "Phố Tràng Thi",
      "suburb": "Hàng Trống",
      "town": "Hoàn Kiếm",
      "county": "Quận Hoàn Kiếm",
      "city": "Hà Nội",
      "postcode": "01235",
      "country": "Việt Nam",
      "country_code": "vn"
    }
  }
]
```
- Tác nhân muốn lấy thông tin chi tiết vị trí đầu tiên có với từ khoá `hồ gươm`, định dạng html, trên lãnh thổ Việt Nam.

Yêu cầu:
```
GET: https://nominatim.openstreetmap.org/?addressdetails=1&q=hồ+gươm&format=html&limit=1&countrycodes=vn
```

Kết quả:
![geocoding html result](./geocoding-html-result.png)


  
### Autocomplete API
Nominatim không hỗ trợ API Autocomplete, xem xét thay thế bằng [Pelias](http://pelias.io/)
Pelias là phần mềm mã hoá địa lý mã nguồn mở, xây dựng trên Elasticsearch. Cơ sở dữ liệu có chứa dữ liệu về các địa điểm và con đường của OSM.

#### Cú pháp
```
<domain>/reverse?<query>
```
Trong đó:
- `domain` là tên miền / địa chỉ máy chủ (vd: https://api.geocode.earth/v1, localhost:3000)
- `query` là truy vấn với các tham số bên dưới

#### Tham số
|Tham số     | Loại tham số | Ý nghĩa | Khoảng giá trị |Ví dụ |
|------------|--------------|---------|---------------|-------|
|text|chính| văn bản hoặc chuỗi ký tự mô tả về đại điểm muốn định vị |  |text=london |
|api_key|chính, bắt buộc| api_key đăng ký với Pelias |  |api_key=ge-6071d21825e9421c |
|size|giới hạn kết quả| số lượng kết quả trả về |size > 0  |size=1 |
|focus.point.lat|giới hạn kết quả| yêu cầu kết quả quanh vĩ độ này|-180 < focus.point.lat <= 180. Mặc định: ưu tiên kết quả nổi tiếng|focus.point.lat=40.72579524 |
|focus.point.lon|giới hạn kết quả| yêu cầu kết quả quanh kinh độ này | -180 < focus.point.lon <= 180. Mặc định: ưu tiên kết quả nổi tiếng hơn|focus.point.lon=-73.980560 |
|sources|lọc kết quả|lọc nguồn dữ liệu  | openstreetmap / osm / openaddresses / oa / geonames / gn / whosonfirst / wof |sources=openstreetmap|
|boundary.circle.lat|lọc kết quả| yêu cầu kết quả quanh vĩ độ này |-180 < boundary.circle.lat <= 180  |boundary.circle.lat=40.7257952|
|boundary.circle.lon|lọc kết quả| yêu cầu kết quả quanh vĩ độ này| -180 < boundary.circle.lon <= 180 |boundary.circle.lon=-73.980560|
|boundary.circle.radius|lọc kết quả|yêu cầu kết quả quanh bán kính này (km) | Mặc định: 50 |boundary.circle.radius=35 |
|boundary.country|lọc kết quả| yêu cầu kết quả trong một số quốc gia |  Cột country code trong bảng [Country codes](https://wiki.openstreetmap.org/wiki/Nominatim/Country_Codes) | boundary.country=’VN,FRA’ |

#### Ví dụ
- Tác nhân muốn tìm `Thành phố Hồ Chí Minh`, nhập `ho chi`

Yêu cầu:
```
GET: https://api.geocode.earth/v1/autocomplete?text=ho%20chi&api_key=ge-6071d21825e9421c
```
Kết quả:
```
{
  "geocoding": {
    "version": "0.2",
    "attribution": "https://geocode.earth/guidelines",
    "query": {
      "text": "ho chi",
      "parser": "pelias",
      "parsed_text": { "subject": "ho", "locality": "ho", "admin": "chi" },
      "size": 10,
      "layers": [
        "venue",
        "street",
        "country",
        "macroregion",
        "region",
        "county",
        "localadmin",
        "locality",
        "borough",
        "neighbourhood",
        "continent",
        "empire",
        "dependency",
        "macrocounty",
        "macrohood",
        "microhood",
        "disputed",
        "postalcode",
        "ocean",
        "marinearea"
      ],
      "private": false,
      "lang": {
        "name": "Vietnamese",
        "iso6391": "vi",
        "iso6393": "vie",
        "defaulted": false
      }
    },
    "warnings": ["performance optimization: excluding 'address' layer"],
    "engine": { "name": "Pelias", "author": "Mapzen", "version": "1.0" },
    "timestamp": 1575444412188
  },
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [106.66667, 10.75] },
      "properties": {
        "id": "421176809",
        "gid": "whosonfirst:locality:421176809",
        "layer": "locality",
        "source": "whosonfirst",
        "source_id": "421176809",
        "name": "Thành phố Hồ Chí Minh",
        "accuracy": "centroid",
        "country": "Việt Nam",
        "country_gid": "whosonfirst:country:85632763",
        "country_a": "VNM",
        "region": "Thành phố Hồ Chí Minh",
        "region_gid": "whosonfirst:region:85680809",
        "region_a": "HC",
        "county": "Quan 8",
        "county_gid": "whosonfirst:county:1108782041",
        "locality": "Thành phố Hồ Chí Minh",
        "locality_gid": "whosonfirst:locality:421176809",
        "continent": "Châu Á",
        "continent_gid": "whosonfirst:continent:102191569",
        "label": "Thành phố Hồ Chí Minh, Việt Nam"
      },
      "bbox": [106.66667, 10.75, 106.66667, 10.75]
    },
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [111.680538, 40.815902] },
      "properties": {
        "id": "102027913",
        "gid": "whosonfirst:locality:102027913",
        "layer": "locality",
        "source": "whosonfirst",
        "source_id": "102027913",
        "name": "Hohhot",
        "accuracy": "centroid",
        "country": "Trung Quốc",
        "country_gid": "whosonfirst:country:85632695",
        "country_a": "CHN",
        "region": "Nội Mông",
        "region_gid": "whosonfirst:region:85669847",
        "region_a": "NM",
        "county": "Huhehaote",
        "county_gid": "whosonfirst:county:890513779",
        "locality": "Hohhot",
        "locality_gid": "whosonfirst:locality:102027913",
        "continent": "Châu Á",
        "continent_gid": "whosonfirst:continent:102191569",
        "label": "Hohhot, Trung Quốc"
      },
      "bbox": [111.644944101, 40.7756927228, 111.770285539, 40.8602232705]
    },
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [-3.096817, 53.335465] },
      "properties": {
        "id": "node/2093633700",
        "gid": "openstreetmap:venue:node/2093633700",
        "layer": "venue",
        "source": "openstreetmap",
        "source_id": "node/2093633700",
        "name": "Ho Ho Cottage (Chinese)",
        "accuracy": "point",
        "country": "Vương quốc Liên hiệp Anh và Bắc Ireland",
        "country_gid": "whosonfirst:country:85633159",
        "country_a": "GBR",
        "macroregion": "Anh",
        "macroregion_gid": "whosonfirst:macroregion:404227469",
        "region": "Wirral",
        "region_gid": "whosonfirst:region:85684519",
        "region_a": "WQ",
        "locality": "Heswall",
        "locality_gid": "whosonfirst:locality:101872157",
        "continent": "Châu  u",
        "continent_gid": "whosonfirst:continent:102191581",
        "label": "Ho Ho Cottage (Chinese), Heswall, Anh, Vương quốc Liên hiệp Anh và Bắc Ireland"
      }
    },
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [-97.675828, 30.410268] },
      "properties": {
        "id": "node/2548060800",
        "gid": "openstreetmap:venue:node/2548060800",
        "layer": "venue",
        "source": "openstreetmap",
        "source_id": "node/2548060800",
        "name": "Ho Ho Chinese B.B.Q",
        "housenumber": "13000",
        "street": "N IH 35 Bldg. 6",
        "postalcode": "78753",
        "accuracy": "point",
        "country": "Hoa Kỳ",
        "country_gid": "whosonfirst:country:85633793",
        "country_a": "USA",
        "region": "Texas",
        "region_gid": "whosonfirst:region:85688753",
        "region_a": "TX",
        "county": "Quận Travis",
        "county_gid": "whosonfirst:county:102081935",
        "county_a": "TV",
        "locality": "Austin",
        "locality_gid": "whosonfirst:locality:101724577",
        "neighbourhood": "Coxville",
        "neighbourhood_gid": "whosonfirst:neighbourhood:85812665",
        "continent": "Bắc Mỹ",
        "continent_gid": "whosonfirst:continent:102191575",
        "label": "Ho Ho Chinese B.B.Q, Austin, TX, USA"
      }
    },
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [-79.286094, 43.044981] },
      "properties": {
        "id": "way/663636841",
        "gid": "openstreetmap:venue:way/663636841",
        "layer": "venue",
        "source": "openstreetmap",
        "source_id": "way/663636841",
        "name": "Ho Ho Chinese Food",
        "accuracy": "point",
        "country": "Canada",
        "country_gid": "whosonfirst:country:85633041",
        "country_a": "CAN",
        "region": "Ontario",
        "region_gid": "whosonfirst:region:85682057",
        "region_a": "ON",
        "county": "Niagara",
        "county_gid": "whosonfirst:county:890456991",
        "locality": "Pelham",
        "locality_gid": "whosonfirst:locality:101736395",
        "continent": "Bắc Mỹ",
        "continent_gid": "whosonfirst:continent:102191575",
        "label": "Ho Ho Chinese Food, Pelham, ON, Canada"
      },
      "bbox": [-79.286201, 43.044899, -79.285987, 43.045062]
    },
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [-71.52779, 41.870717] },
      "properties": {
        "id": "way/230777844",
        "gid": "openstreetmap:venue:way/230777844",
        "layer": "venue",
        "source": "openstreetmap",
        "source_id": "way/230777844",
        "name": "Ho-Ho Chinese Restaurant",
        "accuracy": "point",
        "country": "Hoa Kỳ",
        "country_gid": "whosonfirst:country:85633793",
        "country_a": "USA",
        "region": "Rhode Island",
        "region_gid": "whosonfirst:region:85688509",
        "region_a": "RI",
        "county": "Providence County",
        "county_gid": "whosonfirst:county:102081597",
        "county_a": "PR",
        "localadmin": "Smithfield",
        "localadmin_gid": "whosonfirst:localadmin:404488381",
        "continent": "Bắc Mỹ",
        "continent_gid": "whosonfirst:continent:102191575",
        "label": "Ho-Ho Chinese Restaurant, Smithfield, RI, USA"
      },
      "bbox": [-71.527894, 41.8706028, -71.5276866, 41.8708314]
    },
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [103.01588, 21.79391] },
      "properties": {
        "id": "1242588277",
        "gid": "whosonfirst:locality:1242588277",
        "layer": "locality",
        "source": "whosonfirst",
        "source_id": "1242588277",
        "name": "Hồ Chim",
        "accuracy": "centroid",
        "country": "Việt Nam",
        "country_gid": "whosonfirst:country:85632763",
        "country_a": "VNM",
        "region": "Điện Biên",
        "region_gid": "whosonfirst:region:85680593",
        "region_a": "DB",
        "county": "Muong Cha",
        "county_gid": "whosonfirst:county:1108780961",
        "locality": "Hồ Chim",
        "locality_gid": "whosonfirst:locality:1242588277",
        "continent": "Châu Á",
        "continent_gid": "whosonfirst:continent:102191569",
        "label": "Hồ Chim, Việt Nam"
      },
      "bbox": [103.01588, 21.79391, 103.01588, 21.79391]
    },
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [126.723927, 37.504838] },
      "properties": {
        "id": "way/468414719",
        "gid": "openstreetmap:venue:way/468414719",
        "layer": "venue",
        "source": "openstreetmap",
        "source_id": "way/468414719",
        "name": "HO Chicken",
        "accuracy": "point",
        "country": "Hàn Quốc",
        "country_gid": "whosonfirst:country:85632231",
        "country_a": "KOR",
        "region": "Incheon",
        "region_gid": "whosonfirst:region:85673177",
        "region_a": "IN",
        "county": "Nam-gu",
        "county_gid": "whosonfirst:county:1108746503",
        "locality": "Nhân Xuyên",
        "locality_gid": "whosonfirst:locality:102026447",
        "continent": "Châu Á",
        "continent_gid": "whosonfirst:continent:102191569",
        "label": "Hàn Quốc Incheon Nam-gu HO Chicken"
      },
      "bbox": [126.7238714, 37.5047836, 126.7239834, 37.5048926]
    },
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [106.03708, 21.3733] },
      "properties": {
        "id": "1226058045",
        "gid": "whosonfirst:locality:1226058045",
        "layer": "locality",
        "source": "whosonfirst",
        "source_id": "1226058045",
        "name": "Hô Chính",
        "accuracy": "centroid",
        "country": "Việt Nam",
        "country_gid": "whosonfirst:country:85632763",
        "country_a": "VNM",
        "region": "Bắc Giang",
        "region_gid": "whosonfirst:region:85680685",
        "region_a": "BG",
        "county": "Tan Yen",
        "county_gid": "whosonfirst:county:1108781147",
        "locality": "Hô Chính",
        "locality_gid": "whosonfirst:locality:1226058045",
        "continent": "Châu Á",
        "continent_gid": "whosonfirst:continent:102191569",
        "label": "Hô Chính, Việt Nam"
      },
      "bbox": [106.03708, 21.3733, 106.03708, 21.3733]
    },
    {
      "type": "Feature",
      "geometry": { "type": "Point", "coordinates": [107.328949, 10.455974] },
      "properties": {
        "id": "node/6757037492",
        "gid": "openstreetmap:venue:node/6757037492",
        "layer": "venue",
        "source": "openstreetmap",
        "source_id": "node/6757037492",
        "name": "chị hồ",
        "accuracy": "point",
        "country": "Việt Nam",
        "country_gid": "whosonfirst:country:85632763",
        "country_a": "VNM",
        "region": "Bà Rịa - Vũng Tàu",
        "region_gid": "whosonfirst:region:85680781",
        "region_a": "BV",
        "county": "Dat Do",
        "county_gid": "whosonfirst:county:1108782001",
        "continent": "Châu Á",
        "continent_gid": "whosonfirst:continent:102191569",
        "label": "chị hồ, Việt Nam"
      }
    }
  ],
  "bbox": [-97.675828, 10.455974, 126.7239834, 53.335465]
}
```

### Kịch bản kết hợp Pelias và Nominatim
#### Kịch bản chuẩn
- Tác nhân muốn tìm địa điểm “A B C”, nhập “A" trên giao diện.
- Hệ thống kiểm tra kết quả của “A” trong cache autocomplete nếu không có gọi api autocomplete (`Pelias`) (các tham số lọc cài đặt dựa trên vị trí hiện tại của tác nhân). Trả về kết quả là mảng các chuỗi “A B", “ A B C”, “A B D” trên giao diện.
- Tác nhân chọn “A B C”.
Hệ thống kiểm tra kết quả của “A B C” trong cache geocoding (`Nominatim`) nếu không có gọi api geocoding (các tham số lọc cài đặt dựa trên vị trí hiện tại của tác nhân). Trả về kết quả thông tin địa điểm “A B C”.

#### Kịch bản ngoại lệ 1
- Tác nhân muốn tìm địa điểm “A B C”, nhập “B" trên giao diện.
Hệ thống kiểm tra kết quả của “B” trong cache autocomplete nếu không có gọi api autocomplete (các tham số lọc cài đặt dựa trên vị trí hiện tại của tác nhân). Trả về kết quả là mảng các chuỗi “B D", “B E”, “A B C” trên giao diện.
Tác nhân chọn “A B C”.
- Hệ thống kiểm tra kết quả của “A B C”trong cache geocoding nếu không có gọi api geocoding (các tham số lọc cài đặt dựa trên vị trí hiện tại của tác nhân). Trả về kết quả thông tin địa điểm “A B C”

#### Kịch bản ngoại lệ 2
- Tác nhân muốn tìm địa điểm “A B C”, nhập “A" trên giao diện.
- Hệ thống kiểm tra kết quả của “A” trong cache autocomplete nếu không có gọi api autocomplete (các tham số lọc cài đặt dựa trên vị trí hiện tại của tác nhân). Trả về kết quả là mảng các chuỗi “A B", “ A B C”, “A B D” trên giao diện.
- Tác nhân nhập tiếp thành chuỗi “A C”
- Hệ thống kiểm tra kết quả của “A C” trong cache autocomplete nếu không có gọi api autocomplete (các tham số lọc cài đặt dựa trên vị trí hiện tại của tác nhân). Trả về kết quả là mảng các chuỗi “A C E", “ A C F”, “A B C”, “A D C” trên giao diện.
- Tác nhân chọn “A B C”.
- Hệ thống kiểm tra kết quả của “A B C”trong cache geocoding nếu không có gọi api geocoding (các tham số lọc cài đặt dựa trên vị trí hiện tại của tác nhân). Trả về kết quả thông tin địa điểm “A B C”

### Ưu điểm
- Cả Nominatim và Pelias đều là mã nguồn mở và miễn phí, có trên docker
- Pelias có đầy đủ cơ sở dữ liệu của OSM.
- Pelias và Nominatim có tham số tương đồng để giới hạn không gian tìm kiếm và lọc kết quả tìm kiếm (text/q, size/limit, boundary.country/ countrycodes)
- Autocomplete hạn chế sai chính tả cho Nominatim, tăng độ chính xác và giảm không gian tìm kiếm.

### Nhược điểm
- Nominatim không công khai DB Schema. [DB trong mã nguồn](https://github.com/openstreetmap/Nominatim/blob/master/sql/tables.sql) không giống [DB OSM](https://wiki.openstreetmap.org/w/images/5/58/OSM_DB_Schema_2016-12-13.svg) và không định nghĩa quan hệ giữa các bảng. Quan hệ giữa các bảng định nghĩa trong [tệp perl](https://github.com/openstreetmap/Nominatim/blob/5e45e0b3d7d07180bb914490c81355e6db0f8692/data-sources/wikipedia-wikidata/mysql2pgsql.perl#L754) => khó xác định cấu trúc cơ sở dữ liệu
- Nominatim chỉ tìm kiếm chính xác, không gợi ý, không chấp nhận gõ sai chính tả.

### Áp dụng cho beMap
- Cần cải thiện phần gợi ý khi gặp lỗi chính tả liên quan đến ngọng vùng miền.
- Cache response tại server của mình.
- Client gọi tối đa 5 - 10 request mỗi giây để đảm bảo trải nghiệm và khả năng chịu tải.


