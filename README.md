# 🧠 Gemma 3 270M IT - Hosted Weights

[![Model](https://img.shields.io/badge/Model-Gemma_3_270M_IT-blue.svg)](https://huggingface.co/google/gemma-3-270m-it)
[![Format](https://img.shields.io/badge/Format-Q8_Task-orange.svg)]()
[![Release](https://img.shields.io/github/v/release/hwisu-jung/gemma3-270m-it-weights?color=success)]()

이 저장소는 Android 기기 온디바이스 AI(On-Device AI) 어플리케이션에서 사용하기 위해 **Gemma 3 270M IT** 모델의 경량화(Quantization)된 가중치(Weights) 파일을 호스팅합니다.

## 🚀 프로젝트 개요 (Overview)

모바일 환경에서 구동 가능한 소형 언어 모델(SLM)을 안드로이드 앱에 통합하기 위해 구성된 레포지토리입니다. Hugging Face의 API 토큰(HF_TOKEN) 인증 과정 없이, GitHub Releases 기능을 CDN처럼 활용하여 앱 내에서 모델 파일을 안정적으로 직접 다운로드할 수 있도록 구축했습니다.

- **Model**: Google Gemma 3 270M Instruct
- **Quantization**: INT8 (Q8)
- **Format**: LiteRT `.task` 포맷
- **File Size**: 약 290 MB

## 📦 사용 방법 (Usage)

안드로이드 앱 코드 내에서 아래의 URL을 통해 모델을 직접 다운로드하여 사용할 수 있습니다.

```kotlin
// Android 코드 내 다운로드 URL 설정 예시
const val MODEL_URL = "https://github.com/hwisu-jung/gemma3-270m-it-weights/releases/download/v1.0/gemma3-270m-it-q8.task"
