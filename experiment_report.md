# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-2A202600042
**Name:** Đỗ Văn Quyết
**Date:** 2026-04-15

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "Agent: Based on my data, the best choice is Laptop at $1200." | 9 | Đúng sản phẩm, đúng giá, phù hợp với câu hỏi |
| Garbage Data (`garbage_data.csv`) | "Agent: Based on my data, the best choice is Nuclear Reactor at $999999." | 1 | Outlier cực đoan làm Agent đưa ra kết quả vô nghĩa |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Khi sử dụng `garbage_data.csv`, Agent đã trả về kết quả hoàn toàn sai vì dữ liệu đầu vào chứa nhiều vấn đề nghiêm trọng về chất lượng:

1. **Duplicate IDs**: ID số 1 xuất hiện hai lần (Laptop và Banana), gây mơ hồ khi tham chiếu dữ liệu. Agent không thể xác định record nào là chính xác.

2. **Wrong data types**: Trường `price` của "Broken Chair" có giá trị là chuỗi `"ten dollars"` thay vì số. Điều này khiến Pandas đọc toàn bộ cột `price` dưới dạng kiểu `object` (chuỗi), dẫn đến hàm `idxmax()` so sánh theo thứ tự từ điển thay vì so sánh số học.

3. **Extreme Outlier**: "Nuclear Reactor" có giá $999,999 — một giá trị bất thường hoàn toàn không phù hợp thực tế. Vì cột `price` bị xử lý dạng chuỗi, `'999999'` lại là giá trị lớn nhất theo thứ tự từ điển, khiến Agent chọn sản phẩm vô nghĩa này.

4. **Null values**: Record "Ghost Item" có `id = None` và `category = None`, gây lỗi tiềm ẩn khi lọc dữ liệu theo category và có thể làm hỏng logic của Agent.

Tổng hợp lại, những lỗi trên khiến Agent — dù được viết đúng logic — vẫn đưa ra câu trả lời hoàn toàn vô nghĩa và gây mất tin tưởng vào hệ thống.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** Đồng ý hoàn toàn.

Dù Agent có logic xử lý đúng và prompt rõ ràng, kết quả vẫn sai hoàn toàn khi dữ liệu đầu vào bị "nhiễm". Một prompt tốt không thể chữa được dữ liệu xấu — rác vào thì rác ra (Garbage In, Garbage Out). Đây là lý do tại sao bước Validate trong ETL Pipeline là bắt buộc: làm sạch dữ liệu từ đầu là nền tảng để mọi hệ thống AI hoạt động đáng tin cậy.
