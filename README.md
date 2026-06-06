# PaddleOCR — OCR và Document AI mã nguồn mở


[English](./readme/README_en.md) | Tiếng Việt
> README mặc định tiếng Việt cho fork [`theitm/PaddleOCR`](https://github.com/theitm/PaddleOCR). Dự án gốc: [`PaddlePaddle/PaddleOCR`](https://github.com/PaddlePaddle/PaddleOCR).

PaddleOCR là bộ công cụ **OCR (Optical Character Recognition)** và **phân tích tài liệu** của PaddlePaddle. Công cụ này giúp chuyển ảnh, tài liệu scan và PDF thành văn bản hoặc dữ liệu có cấu trúc như **JSON / Markdown**, phù hợp cho tìm kiếm, RAG, agent và các hệ thống xử lý tài liệu tự động.

## PaddleOCR dùng để làm gì?

- Nhận dạng chữ trong ảnh, ảnh chụp màn hình, ảnh scan, hóa đơn, giấy tờ, sách, biểu mẫu.
- Xử lý PDF hoặc tài liệu nhiều trang.
- Phân tích bố cục tài liệu: tiêu đề, đoạn văn, bảng, công thức, biểu đồ, vùng ảnh.
- Xuất kết quả sang JSON, Markdown hoặc các định dạng có thể đưa vào pipeline AI/RAG.
- Hỗ trợ hơn 100 ngôn ngữ và nhiều loại phần cứng: CPU, GPU, XPU, NPU.

## Có cần LLM không?

**Không bắt buộc.**

PaddleOCR có thể chạy OCR truyền thống bằng các model nhận dạng chữ và thị giác máy tính mà không cần gọi LLM bên ngoài.

- OCR ảnh/PDF sang text: **không cần LLM**.
- Nhận dạng layout, bảng, cấu trúc tài liệu: thường **không cần LLM**.
- PaddleOCR-VL là mô hình vision-language cho document parsing nâng cao, có thể chạy local tùy cấu hình.
- Nếu muốn hỏi đáp, tóm tắt, trích xuất ý nghĩa hoặc xây RAG trên tài liệu đã OCR, lúc đó mới cần LLM ở bước sau.

## Thành phần chính

- **PP-OCRv5**: nhận dạng chữ đa ngôn ngữ tốc độ cao.
- **PP-StructureV3**: phân tích cấu trúc tài liệu, bảng, layout, xuất Markdown/JSON.
- **PaddleOCR-VL**: mô hình vision-language nhẹ cho hiểu tài liệu phức tạp.
- **PP-DocTranslation**: dịch tài liệu.
- **PaddleOCR.js**: SDK chạy OCR trong trình duyệt.
- **MCP / LangChain integration**: tích hợp với agent, RAG và workflow AI.

## Cài đặt nhanh

### 1. Tạo môi trường Python

Khuyến nghị dùng Python 3.8–3.12.

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -U pip
```

### 2. Cài PaddlePaddle runtime

CPU:

```bash
python -m pip install paddlepaddle==3.2.0 -i https://www.paddlepaddle.org.cn/packages/stable/cpu/
```

GPU NVIDIA CUDA 11.8, Linux:

```bash
python -m pip install paddlepaddle-gpu==3.2.0 -i https://www.paddlepaddle.org.cn/packages/stable/cu118/
```

Với CUDA hoặc hệ điều hành khác, xem hướng dẫn cài đặt chính thức của PaddlePaddle.

### 3. Cài PaddleOCR

Cài đầy đủ:

```bash
python -m pip install "paddleocr[all]"
```

Hoặc cài gói cơ bản:

```bash
python -m pip install paddleocr
```

## Dùng nhanh qua CLI

OCR một ảnh:

```bash
paddleocr ocr -i ./image.png \
  --use_doc_orientation_classify False \
  --use_doc_unwarping False \
  --use_textline_orientation False \
  --engine paddle
```

Phân tích cấu trúc tài liệu/PDF:

```bash
paddleocr pp_structurev3 -i ./document.pdf --engine paddle
```

Kết quả thường được xuất ở dạng có cấu trúc, tùy pipeline và tham số chạy.

## Dùng trong Python

Ví dụ OCR cơ bản:

```python
from paddleocr import PaddleOCR

ocr = PaddleOCR(lang="en")
result = ocr.predict("image.png")

for page in result:
    print(page)
```

Với tiếng Việt hoặc tài liệu đa ngôn ngữ, cần kiểm tra model/ngôn ngữ phù hợp trong tài liệu chính thức vì cách cấu hình có thể thay đổi theo phiên bản PaddleOCR.

## Khi nào nên dùng PaddleOCR?

Nên dùng khi cần:

- OCR local, không muốn phụ thuộc API cloud.
- Xử lý nhiều ảnh/PDF scan.
- Nhận dạng đa ngôn ngữ.
- Tách bảng/layout tốt hơn OCR cơ bản.
- Tạo dữ liệu đầu vào cho RAG hoặc hệ thống agent.

Không nhất thiết cần dùng nếu:

- PDF đã có text selectable; khi đó PyMuPDF hoặc `pymupdf4llm` có thể nhẹ hơn.
- Chỉ cần OCR vài ảnh đơn giản; Tesseract hoặc công cụ online có thể đủ.
- Muốn hỏi đáp/tóm tắt trực tiếp bằng AI vision API và chấp nhận chi phí/API bên ngoài.

## So sánh nhanh

- **Tesseract**: nhẹ, lâu đời, phù hợp OCR cơ bản.
- **PaddleOCR**: mạnh hơn về đa ngôn ngữ, layout, production pipeline.
- **marker-pdf / MinerU**: mạnh về PDF sang Markdown, nhưng thường nặng hơn.
- **LLM Vision OCR**: dễ dùng, nhưng tốn API, chậm/đắt hơn và kém riêng tư hơn local OCR.

## Tài nguyên

- Trang chủ: <https://www.paddleocr.com>
- Repo gốc: <https://github.com/PaddlePaddle/PaddleOCR>
- Tài liệu: <https://paddlepaddle.github.io/PaddleOCR/>
- Hugging Face PaddleOCR: <https://huggingface.co/PaddlePaddle>

## Giấy phép

PaddleOCR được phát hành theo giấy phép **Apache-2.0**. Xem file [`LICENSE`](./LICENSE) trong repository.

## Ghi chú cho fork này

Fork này giữ nguyên mã nguồn từ PaddleOCR gốc và bổ sung README tiếng Việt để dễ tra cứu, cài đặt và đánh giá khả năng sử dụng trong các pipeline OCR/RAG tiếng Việt.
