# PdfCraft 한국어판 — 수정 내용과 빌드 방법

원본: https://github.com/storytold/pdfcraft (2026-10-21 커밋 기준, MIT OR Apache-2.0)

## 바뀐 것

| 영역 | 파일 | 내용 |
|---|---|---|
| 한/EN 전환 버튼 | `crates/ui-egui/src/chrome.rs` | 상단 오른쪽 프린터 버튼 오른쪽에 `한 / EN` 버튼. 환경설정의 표시 언어와 같은 설정이라 다음에 열어도 유지됩니다. |
| 한국어 메뉴 | `crates/ui-egui/src/i18n/ko.tsv`, `i18n/mod.rs` | 화면 문구 2,742개 전체 번역. 브라우저/OS 언어가 한국어면 처음부터 한국어로 열립니다. |
| 한글 화면 글꼴 | `crates/ui-egui/src/theme.rs` | 한국어 화면에서는 나눔고딕을 일본어 글꼴보다 먼저 사용 (한글·한자·문장부호가 한 기준선에). 다른 언어에서도 한글 파일명이 깨지지 않음. |
| 텍스트 편집 | `crates/edit/src/text.rs` | 기존 PDF의 글자를 고칠 때 원래 글꼴에 한글이 없으면 나눔 글꼴로 대체 (명조 계열 문단은 나눔명조, 그 외 나눔고딕). 이전에는 오류로 거부됨. |
| 텍스트 추가 | `crates/edit/src/added.rs`, `crates/fonts/src/shaping.rs` | 텍스트 추가·타자기 도구에서 한글·한자 입력 가능. 한국어는 띄어쓰기 단위로 줄바꿈. 이전에는 서유럽 문자만 가능. |
| 한글 PDF 보기 | `crates/render/src/raster.rs` | 글꼴을 내장하지 않은 한국어 PDF(HY신명조·HY견고딕·굴림·돋움·바탕·맑은 고딕·한컴 HCR 등)를 나눔 글꼴로 대신 그림. 이전에는 글자가 비어 보임. |
| 날짜 | `crates/engine/src/dates.rs` | 날짜 도장 등에 한국어 월·요일 이름 (1월, 월요일). |
| 글꼴 묶음 | `korean-fonts/` | 나눔고딕 Regular·Bold, 나눔명조 Regular (OFL-1.1, google/fonts) + 원본이 쓰는 일본어·아랍어·태국어 글꼴. |

## 빌드 (웹 버전)

```sh
rustup target add wasm32-unknown-unknown
cargo install trunk --locked

cd apps/pdfcraft-web
CRAFT_FONTS_DIR="$(pwd)/../../korean-fonts" CRAFT_FONTS_REQUIRED=1 trunk build --release
```

결과는 `dist/web/`에 생깁니다. 그 안의 파일을 그대로 정적 웹 서버(GitHub Pages 등)에 올리면 됩니다.
Windows PowerShell에서는 `$env:CRAFT_FONTS_DIR="..\..\korean-fonts"` 처럼 환경 변수를 먼저 지정하세요.

`CRAFT_FONTS_DIR`를 빼고 빌드하면 한글 글꼴이 들어가지 않아 한글이 네모로 보입니다.

## 데스크톱 앱

```sh
CRAFT_FONTS_DIR="$(pwd)/korean-fonts" cargo run --release -p pdfcraft
```
