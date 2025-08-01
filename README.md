# 🎹 Console MIDI Piano

Windows 콘솔 환경에서 키보드를 사용해 연주할 수 있는 간단한 C++ MIDI 피아노 프로그램입니다. Windows Multimedia API (`winmm.lib`)를 사용하여 MIDI 출력을 제어합니다.

## ✨ 주요 기능

* **키보드 연주:** 컴퓨터 키보드(QWERTY 및 ZXC 라인)를 사용하여 피아노를 연주할 수 있습니다.
* **악기 변경:** 128가지의 다양한 MIDI 악기 중에서 선택하여 연주할 수 있습니다.
* **옥타브 조절:** 음의 높낮이(옥타브)를 조절할 수 있습니다.
* **볼륨 조절:** 전체 연주 볼륨을 세밀하게 조절할 수 있습니다.

## 🚀 사용 방법

프로그램을 실행하면 콘솔 창에 피아노 건반 UI가 나타납니다. 아래 키를 사용하여 피아노를 제어하세요.

| 키 | 기능 |
| :--- | :--- |
| **Z, S, X, D, C, V, ...** | 피아노 건반 연주 (흰 건반 및 검은 건반) |
| **← / → (방향키)** | 악기 변경 (이전/다음 악기) |
| **↑ / ↓ (방향키)** | 옥타브 조절 (한 옥타브씩 상승/하강) |
| **+ / -** | 볼륨 조절 (증가/감소) |
| **ESC** | 프로그램 종료 |

## 🛠️ 컴파일 및 실행 환경

### 요구 사항
* Windows 운영체제
* C++ 컴파일러 (Visual Studio, MinGW 등)

### 컴파일 방법
이 코드는 Windows Multimedia API를 사용하므로, 컴파일 시 **`winmm.lib`** 라이브러리를 반드시 링크해야 합니다.

-   **Visual Studio:**
    1.  프로젝트 속성 -> 링커 -> 입력 -> 추가 종속성으로 이동합니다.
    2.  `winmm.lib`를 추가합니다.
-   **GCC/MinGW:**
    -   컴파일 명령어에 `-lwinmm` 옵션을 추가합니다.
        ```bash
        g++ your_source_file.cpp -o piano.exe -lwinmm
        ```

코드 내에 `#pragma comment(lib, "winmm.lib")` 지시문이 포함되어 있어, Visual Studio 환경에서는 별도의 설정 없이 컴파일될 수 있습니다.

## 📝 코드 구조 및 설명

이 프로그램은 Windows의 저수준 MIDI API를 직접 호출하여 작동합니다.

-   `MIDIShortMSG`: MIDI 메시지를 보내기 위한 `union` 자료구조입니다.
-   `MIDIOpen()` / `MIDIClose()`: MIDI 출력 장치를 열고 닫는 함수입니다.
-   `MIDISendShortMsg()`: `Note On`, `Note Off`, `Program Change` (악기 변경), `Control Change` (볼륨 조절) 등의 MIDI 메시지를 전송하는 핵심 함수입니다.
-   `main()`: 메인 루프 안에서 `GetKeyState()` 함수를 통해 키보드 입력을 실시간으로 감지하고, 해당 키에 맞는 MIDI 메시지를 전송하여 소리를 재생하거나 제어합니다.
