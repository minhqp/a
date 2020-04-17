## Tài liệu mô tả tích hợp cổng thông tin điện tử Bộ Xây dựng (gọi tắt là MoC Portal) và Dịch vụ chuyển văn bản thành giọng nói nhân tạo tiếng Việt Vbee (gọi tắt là VbeeTTS)

### Công nghệ: ASP.Net C#

### Các thành phần trong hệ thống bao gồm: MoC Portal và VbeeTTS.

### Luồng hoạt động:
1. MoC Portal gửi thông tin bài báo cần tổng hợp tới VbeeTTS qua API.
2. VbeeTTS xử lý bài báo và tổng hợp thành audio, gửi lại cho Moc Portal qua cơ chế callback.
3. MoC Portal nhận callback, xử lý lưu file audio, lưu vào CSDL.

### Chi tiết:
#### 1. Cấu trúc API tổng hợp bài báo
+ Mã nguồn xử lý chính: https://github.com/minhqp/tts-article-with-csharp/blob/master/Program.cs
+ Mã nguồn đầy đủ: https://github.com/minhqp/tts-article-with-csharp

```
+ method: POST
+ headers: { "Authorization": "<token>" }
+ body: {
    "content": "<content>",
    "websiteId": "<website_id>"
    "httpCallback": "<http_callback>",
    "voices": [
      { "id": "<voice_id>", "rate": "<rate>" }
    ]
  }
  ```
  
  Trong đó:
  + `token` có được bằng cách truy cập https://articles.vbee.vn/property >> Authorization
  + `content` là nội dung bài báo
  + `websiteId`là id của website lấy được tại http://articles.vbee.vn/websites >> Cột websiteId
  + `voices` chứa danh sách mã giọng muốn tổng hợp, mỗi phần tử trong mảng gôm `id` là mã giọng đọc và tham số tùy chọn `rate` là tốc độ giọng đọc.  
  Danh sách mã giọng đọc *cơ bản*:
    - `sg_male_minhhoang_dial_48k-hsmm`
    - `hn_male_xuantin_vdts_48k-hsmm`
    - `hn_female_thutrang_phrase_48k-hsmm`
    - `sg_female_thaotrinh_dialog_48k-hsmm`
    - `sg_female_thaotrinh_dialog_48k-hsmm`
    
  Danh sách mã giọng đọc *nâng cao*:
    - `sg_female_lantrinh_news_48k-d`
    - `sg_male_minhhoang_news_48k-d`
    - `sg_female_thaotrinh_news_48k-d`
  + `httpCallback`là đường dẫn để gọi lại truyền thông tin audio sau khi tổng hợp thành công.  
  Cấu trúc đường dẫn có dạng `https://moc.portal/api/tts-callback?id=<id>`  
    - domain `https://moc.portal` là domain của cổng thông tin
    - route `/api/tts-callback`
    - `id` là id của bài báo truyền lên
 
