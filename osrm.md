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

### Các bước cơ bản
Sau khi cài đặt OSRM chúng ta có thể thao tác trên dữ liệu từ OpenStreetMap sử dụng các tool binary như:
- osrm-contract
- osrm-customize
- osrm-extract
- osrm-partition 
- osrm-routed

Các bài toán mà OSRM giải quyết dựa trên lý thuyết đồ thị (Graph Theory). OSRM sẽ không làm việc trực tiếp trên dữ liệu từ OpenStreetMap. Thay vào đó chúng ta cần chuyển đổi dữ liệu từ OSM thành một dạng đồ thị có trọng số và hướng (directed weighted graph). Cụ thể (chúng ta chỉ tập trung vào MLD):

Việc tạo một đồ thị cơ bản sẽ được thực hiện bởi tool `osrm-extract`
```
osrm-extract hanoi.osm.pbf -p profiles/car.lua
```

Tiếp theo dữ liệu sẽ được chia thành các cell một cách đệ quy sử dụng `osrm-partition`
```
osrm-partition hanoi.osrm
```

Tính toán trọng số (weight) cho các liên kết trong đồ thị (edge) sử dụng `osrm-customize`
```
osrm-customize hanoi.osrm
```

Sau các bước trên OSRM sẽ tạo ra khá nhiều files phục vụ cho quá trình giải quyết các bài toàn routing:
```
hanoi.osm.pbf		     hanoi.osrm.ebg_nodes	    hanoi.osrm.names	     hanoi.osrm.tls
hanoi.osrm		     hanoi.osrm.edges		    hanoi.osrm.nbg_nodes     hanoi.osrm.turn_duration_penalties
hanoi.osrm.cell_metrics      hanoi.osrm.enw		    hanoi.osrm.partition     hanoi.osrm.turn_penalties_index
hanoi.osrm.cells	     hanoi.osrm.fileIndex	    hanoi.osrm.properties    hanoi.osrm.turn_weight_penalties
hanoi.osrm.cnbg		     hanoi.osrm.geometry	    hanoi.osrm.ramIndex      
hanoi.osrm.cnbg_to_ebg	     hanoi.osrm.icd		    hanoi.osrm.restrictions
hanoi.osrm.datasource_names  hanoi.osrm.maneuver_overrides  hanoi.osrm.timestamp
hanoi.osrm.ebg		     hanoi.osrm.mldgr		    hanoi.osrm.tld

```

#### Tạo đồ thị từ dữ liệu OpenStreetMap
Không phải tất cả dữ liệu mà OSM cung cấp đều được dùng cho việc giải quyết vấn đề routing, ví dụ như các tags của nodes hoặc ways. Ngoài ra, dữ liệu từ OSM không có một chuẩn chung duy nhất (có thể dùng các định dạng như PBF, OSM, XML). Do vậy, OSRM sẽ thực hiện bóc tách dữ liệu cần thiết cho việc routing từ dữ liệu gốc của OpenStreetMap.

Công việc này được thực hiện bởi OSRM **extractor** tool. Công cụ này có nhiệm vụ chuyển đổi dữ liệu gốc từ OSM thành cách dạng dữ liệu metadata phục vụ cho routing.

Trong OSRM, việc chuyển đổi dữ liệu sẽ dựa trên các **Profiles**. Profiles định nghĩa các hành vi định tuyến, hay hiểu đơn giản hơn là định nghĩa các phương thức di chuyển (ví dụ như: đi bộ, đi xe đạp, đi oto,...). Profile mô tả việc chúng ta có nên di chuyển trên một tuyến đường nhất định nào đó hoặc di chuyển qua một địa điểm nào đó hay không (ví dụ đường 1 chiều, đường chỉ dành cho oto,...). Đồng thời tìm được thời gian di chuyển cho từng phương thức. Các profile chính cung cấp bởi OSRM bao gồm:

- foot
- bicycle
- car

Reference: https://github.com/Project-OSRM/osrm-backend/tree/master/profiles

#### Chạy OSRM thông qua HTTP server
OSRM cung cấp công cụ để chạy OSRM HTTP server một cách đơn giản:
```
osrm-routed -p 5000 -i 0.0.0.0 -t 10 hanoi.osrm 
```
Sau khi chạy HTTP server, các API do OSRM cung cấp có thể được truy cập. Chúng ta sẽ bàn về các API rõ hơn trong phần sau.

Example request:
```
curl "http://127.0.0.1:5000/route/v1/driving/13.388860,52.517037;13.385983,52.496891?steps=true"
```








