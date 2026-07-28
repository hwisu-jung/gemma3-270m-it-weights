# gemma3-270m-it-weights

Android 온디바이스 추론용 Gemma 3 270M IT 가중치를 호스팅하는 저장소입니다.

HuggingFace의 litert-community 저장소는 gated라서 앱에서 직접 받으면 401이 납니다.
그래서 같은 파일을 GitHub Releases에 올려두고 토큰 없이 받아 쓰고 있습니다.

## 파일

가중치는 동일하고 컨테이너 포맷만 다릅니다. 사용하시는 런타임에 맞춰 받으시면 됩니다.

| 파일 | 크기 | 런타임 |
|---|---|---|
| `gemma3-270m-it-q8.litertlm` | 290 MB | LiteRT-LM (`com.google.ai.edge.litertlm:litertlm-android`) |
| `gemma3-270m-it-q8.task` | 290 MB | MediaPipe (`com.google.mediapipe:tasks-genai`) — 레거시 |

INT8 (Q8) 양자화, CPU 백엔드입니다. 270M은 아직 GPU/NPU 가속이 지원되지 않습니다.

두 포맷은 서로 호환되지 않습니다. `.task`를 LiteRT-LM `Engine`에 넘기면 네이티브에서 SIGABRT로 종료됩니다.
확장자만 바꾸는 것으로는 해결되지 않으니 파일을 따로 받으셔야 합니다.

MediaPipe LLM Inference API는 현재 maintenance-only 상태입니다. 새로 시작하시는 경우 `.litertlm`을 사용하시면 됩니다.

## 사용

```kotlin
const val MODEL_URL =
    "https://github.com/hwisu-jung/gemma3-270m-it-weights/releases/download/v1.0/gemma3-270m-it-q8.litertlm"
```

```kotlin
// build.gradle.kts
implementation("com.google.ai.edge.litertlm:litertlm-android:0.13.1")
```

```kotlin
val engine = Engine(
    EngineConfig(
        modelPath = File(context.getExternalFilesDir(null), "gemma3-270m-it-q8.litertlm").absolutePath,
        backend = Backend.CPU(),
        cacheDir = context.cacheDir.path,
    )
)
engine.initialize()   // 최대 10초 소요되므로 백그라운드에서 호출해 주세요

engine.createConversation().sendMessageAsync("안녕하세요").collect { print(it) }
```

MediaPipe를 계속 사용하신다면 URL 끝만 `gemma3-270m-it-q8.task`로 바꾸시면 됩니다.

## 다운로드 시 유의사항

- Release URL은 302로 리다이렉트됩니다. `HttpURLConnection`은 HTTP↔HTTPS 리다이렉트를 따라가지 않으므로 OkHttp를 쓰시거나 직접 처리해 주셔야 합니다.
- 네이티브가 `fopen`으로 파일을 여는 구조라 `content://` URI나 assets 경로는 사용할 수 없습니다. `getExternalFilesDir()`이나 `filesDir` 아래에 저장해 주세요.
- 로드 전에 파일 크기를 확인하시는 것이 좋습니다. 다운로드가 중간에 끊기거나 에러 페이지를 받으면 몇 KB짜리 파일이 남는데, 그대로 로드하면 네이티브 크래시라 `try-catch`로 잡히지 않습니다.

```kotlin
require(file.exists() && file.length() > 200L * 1024 * 1024) { "모델 파일 손상" }
```

## 출처

- [litert-community/gemma-3-270m-it](https://huggingface.co/litert-community/gemma-3-270m-it) — 변환본 원본
- [LiteRT-LM Android 문서](https://ai.google.dev/edge/litert-lm/android)

## 라이선스

Gemma is provided under and subject to the Gemma Terms of Use found at
[ai.google.dev/gemma/terms](https://ai.google.dev/gemma/terms).

사용 및 재배포 시 [Gemma Terms of Use](https://ai.google.dev/gemma/terms)와
[Prohibited Use Policy](https://ai.google.dev/gemma/prohibited_use_policy)를 준수하셔야 하며,
동일한 제한 조항을 하위 수령자에게 전달해야 합니다.
