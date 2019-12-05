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



















