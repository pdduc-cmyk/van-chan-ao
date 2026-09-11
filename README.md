# Vấn Chẩn Ảo — luyện kỹ năng vấn chẩn YHCT với bệnh nhân mô phỏng bằng AI

Ứng dụng web một file dành cho sinh viên Y học cổ truyền, Trường Đại học Y Dược Cần Thơ.
Sinh viên hỏi bệnh một bệnh nhân mô phỏng bằng AI (gõ hoặc **nói trực tiếp**), tự chấm theo bảng kiểm,
rồi đối chiếu với kết quả chấm của AI để thấy mình bỏ sót ở đâu.

Mô phỏng tiến trình một trạm OSCE: **Hướng dẫn → Vấn chẩn → Marking → Câu hỏi giám khảo → Kết quả**.

## Dùng thử

Mở trang GitHub Pages của repo này, vào **Cài đặt**, dán khoá API Gemini
(lấy miễn phí tại [Google AI Studio](https://aistudio.google.com/apikey)), rồi chọn ca ở **Thư viện ca**.

Chạy tại máy:

```bash
git clone <repo-url> && cd van-chan-ao
python3 -m http.server 8000
# mở http://localhost:8000
```

Phải chạy qua `http`/`https` — mở bằng `file://` thì Chrome chặn micro.

## Chức năng

- **Thư viện ca** lọc theo vấn đề lâm sàng, độ khó, trạng thái đã làm
- **Vấn chẩn** — chat với bệnh nhân ảo, đồng hồ đếm ngược 12 phút, panel 3 tab (kịch bản khoá với sinh viên)
- **Nói trực tiếp** — micro nhận dạng tiếng Việt, bệnh nhân trả lời thành tiếng, chế độ rảnh tay để hội thoại liên tục
- **Marking hai chế độ** — sinh viên tự chấm trước, sau đó AI chấm có trích dẫn nguyên văn câu đã hỏi; màn đối chiếu ba cột làm nổi các mục lệch nhau
- **Điểm số** theo 5 miền, mục trọng yếu bỏ sót thì không xếp loại Đạt
- **Hồ sơ cá nhân** — đường tiến bộ, độ chính xác tự đánh giá, mục hay bỏ sót, xuất/nhập JSON
- **Chế độ giảng viên** — xem kịch bản, ghi đè kết quả AI, nhập/xuất ca, kiểm tra tính toàn vẹn ca

## Bảng kiểm

52 mục dùng chung cho mọi ca, chia 5 miền:

| Miền | Số mục | Trọng số |
|---|---|---|
| Mở đầu & giao tiếp | 7 | 15 |
| Bệnh sử YHHĐ (SOCRATES) | 9 | 25 |
| Thập vấn YHCT | 22 | 30 |
| Tiền sử & lối sống | 7 | 15 |
| ICE & kết thúc | 7 | 15 |

Thập vấn được chi tiết hoá theo 10 vấn: hàn nhiệt · hãn · đầu thân · nhị tiện · ẩm thực ·
hung phúc · nhĩ mục · thuỵ miên · cựu bệnh và nhân do · kinh đới thai sản.

Điểm cuối = điểm trạm × 0,9 + điểm câu hỏi giám khảo (0–10), thang 100.

## Thêm ca bệnh

Bật **Chế độ giảng viên** trong Cài đặt → mục **Giảng viên** → *Tải mẫu ca rỗng*, điền rồi *Nhập ca từ file JSON*.
Nút *Kiểm tra* đối chiếu bảng kiểm với kịch bản và cảnh báo những dữ kiện còn thiếu —
nếu bảng kiểm hỏi mà kịch bản không có câu trả lời, bệnh nhân ảo sẽ trả lời "không rõ".

## Lưu ý về khoá API

Bản prototype giữ khoá Gemini trong `localStorage` của từng máy, nên chỉ dùng trong nội bộ nhóm thử nghiệm.
Khi mở cho cả khoá học, khoá phải chuyển về máy chủ.

## Kỹ thuật

Một file `index.html`, không phụ thuộc thư viện ngoài. Bệnh nhân ảo dùng Gemini API;
giọng nói dùng Web Speech API của trình duyệt (Chrome/Edge). Dữ liệu học tập lưu trong `localStorage`.

---

Khoa Y học cổ truyền · Trường Đại học Y Dược Cần Thơ
