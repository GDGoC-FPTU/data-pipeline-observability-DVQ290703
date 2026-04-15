[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23574126&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** qdovan03@gmail.com
**Name:** Đỗ Văn Quyết

---

## Mo ta

Bai lab xây dựng một ETL Pipeline tự động gồm 4 bước: Extract dữ liệu từ file JSON, Validate để loại bỏ các record không hợp lệ (giá ≤ 0 hoặc category rỗng), Transform để chuẩn hóa category và tính giá sau giảm 10%, và Load kết quả ra file CSV. Ngoài ra, bài lab còn thực hiện thí nghiệm so sánh độ chính xác của AI Agent khi chạy với dữ liệu sạch (`processed_data.csv`) và dữ liệu rác (`garbage_data.csv`) để minh họa tầm quan trọng của Data Quality.

---

## Cach chay (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chay ETL Pipeline
```bash
python solution.py
```

### Chay Agent Simulation (Stress Test)
```bash
# Tao garbage data
python generate_garbage.py

# Chay simulation voi ca 2 bo du lieu
python agent_simulation.py
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script
├── processed_data.csv       # Output cua pipeline
├── experiment_report.md     # Bao cao thi nghiem
└── README.md                # File nay
```

---

## Ket qua

- **Input:** 5 records từ `raw_data.json`
- **Dropped:** 2 records không hợp lệ (Mystery Box: giá âm, Phone: category rỗng)
- **Processed:** 3 records hợp lệ được lưu vào `processed_data.csv`
- **Transform:** category chuẩn hóa Title Case, thêm cột `discounted_price` (giảm 10%) và `processed_at`
- **Agent Simulation:** Với clean data → trả lời đúng; với garbage data → trả lời sai do outlier và sai kiểu dữ liệu
