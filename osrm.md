# OSRM

## Giới thiệu chung
### OSRM là gì?
**OSRM** (**O**pen **S**ource **R**outing **M**achine) là một thư viện được dùng để tính toán các cung đường (routes), khoảng cách (distances) và thời gian di chuyển (travel times) giữa các vị trí trong không gian (các vị trí trên bản đồ).

OSRM được viết bằng ngôn ngữ C++ và có thể truy cập thông qua HTTP API hoặc C++ API.

OSRM được thiết kế để hoạt động tốt với dữ liệu từ OpenStreetMap.

### Các bài toán OSRM giải quyết
OSRM cung cấp các các công cụ để xây dựng backend server để giải quyết các bài toán sau:

- Nearest: tìm các điểm lân cận cho một tọa độ cho sẵn
- Route: tìm đường đi nhanh nhất giữa hai tọa độ cho trước.
- Table (distance matrix): tính toán khoảng cách, thời gian, và cách đi nhanh nhất giữa các cặp tọa độ cho trước.
- Match: tìm tọa độ dựa vào tín hiệu GPS
- Trip: giải bài toán Travelling Salesman Problem
- Tile: tạo các Mapbox vector tiles với các thông tin routing đính kèm.

Mỗi bài toán được giải quyết thông qua một **service** tương ứng trong OSRM. OSRM sẽ cung cấp các API tương ứng cho từng bài toán khác nhau.

Open Source backend server: https://github.com/Project-OSRM/osrm-backend

> OSRM sẽ được dùng làm *routing engine* chính cho beMaps. Các bài toán có thể áp dụng cho beMaps: **Route** và **Table** (Distance Matrix)

### Cài đặt OSRM
OSRM có thể được cài đặt bằng các cách chính sau:
- Biên dịch trực tiếp từ mã nguồn: https://github.com/Project-OSRM/osrm-backend/wiki/Building-OSRM
- Sử dụng container (Docker): https://github.com/Project-OSRM/osrm-backend/wiki/Docker-Recipes

## Tổng quan về cách sử dụng OSRM

### Download dữ liệu từ OpenStreetMap
Trước khi làm việc với OSRM chúng ta cần lấy dữ liệu từ OpenStreetMap - tham khảo [tại đây](https://github.com/rnd-forests/requirements/blob/master/notes.md#c%C3%A1ch-truy-xu%E1%BA%A5t-d%E1%BB%AF-li%E1%BB%87u-t%E1%BB%AB-osm)

Chúng ta sẽ ví dụ với dữ liệu từ Hà Nội, dữ liệu OpenStreetMap sẽ được lưu trong tệp với tên `hanoi.osm.pbf`

### Tiền xử lý dữ liệu
OSRM cung cấp hai quy trình tiền xử lý dữ liệu:
- **Contraction Hierarchies (CH)**: phù hợp với các trường hợp mà hiệu suất truy vấn là điểm mấu chốt, ví dụ như giải quyết bài toán Distance Matrix với một số lương lớn các cặp tọa độ.
- **Multi-Level Dijkstra (MLD)**: phù hợp với các trường hợp và hiệu suất truy vấn cần đạt ở mức tốt và có thể xử lý được các vấn đề thay đổi dữ liệu theo thời gian thực, ví dụ như các dữ liệu liên quan đến tình trạng giao thông hiện tại (traffic updates)

> Đối với beMaps **Multi-Level Dijkstra (MLD)** sẽ được sử dụng chính do một số tính năng chỉ có trên MLD như việc tìm các route thay thế (alternative routes) ngoài route chính. Ngoài ra, OSRM community sẽ sẽ cung cấp nhiều tính năng mới trên MLD hơn so với CH.






















