# OSRM

## Introduction
### OSRM là gì?
**OSRM** (**O**pen **S**ource **R**outing **M**achine) là một thư viện được dùng để tính toán các cung đường (routes), khoảng cách (distances) và thời gian di chuyển (travel times) giữa các vị trí trong không gian (các vị trí trên bản đồ).

OSRM được viết bằng ngôn ngữ C++ và có thể truy cập thông qua HTTP API hoặc C++ API.

OSRM được thiết kế để hoạt động tốt với dữ liệu từ OpenStreetMap.

### Các bài toán OSRM giải quyết
OSRM cung cấp các các công cụ để xây dựng backend server để giải quyết các bài toán sau:

- Nearest: tìm các điểm lân cận cho một tọa độ cho sẵn (thường là tìm các POIs)
- Route: tìm đường đi nhanh nhất giữa hai tọa độ cho trước.
- Table (distance matrix): tính toán khoảng cách, thời gian, và cách đi nhanh nhất giữa các cặp tọa độ cho trước.
- Match: tìm tọa độ dựa vào tín hiệu GPS
- Trip: giải bài toán Travelling Salesman Problem
- Tile: tạo các Mapbox vector tiles với các thông tin routing đính kèm.

Mỗi bài toán được giải quyết thông qua một **service** tương ứng trong OSRM. OSRM sẽ cung cấp các API tương ứng cho từng bài toán khác nhau.

Open Source backend server: https://github.com/Project-OSRM/osrm-backend

### Cài đặt OSRM
OSRM có thể được cài đặt bằng các cách chính sau:
- Biên dịch trực tiếp từ mã nguồn: https://github.com/Project-OSRM/osrm-backend/wiki/Building-OSRM
- Sử dụng container (Docker): https://github.com/Project-OSRM/osrm-backend/wiki/Docker-Recipes
























