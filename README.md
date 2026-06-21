# MyDFRobotLcdDisplay (DFR0997 Extension for micro:bit MakeCode)

이 저장소는 DFRobot사의 **Gravity: 2.0" IPS Color Serial Display (SKU: DFR0997)** 모듈을 마이크로비트 메이크코드(MakeCode)에서 사용하기 위한 확장 프로그램 라이브러리입니다.

> [!NOTE]  
> 이 프로젝트는 DFRobot의 공식 [pxt-DFRobot_lcdDisplay](https://github.com/DFRobot/pxt-DFRobot_lcdDisplay) 라이브러리를 포크(Fork)하여 **한글(Korean/Hangul) 출력을 지원하도록 개선한 버전**입니다.

---

## 🛠️ 개선 목적 및 변경 사항 (Korean Support)

### 한글이 깨지던 원인
기존 공식 라이브러리는 텍스트 데이터를 전송할 때 1바이트씩 잘라내는 `charCodeAt(0)` 방식을 사용하여, 255를 초과하는 유니코드 값(한글 문자 등)을 보낼 때 데이터 상위 바이트가 손실되어 화면에 한글이 깨지거나 빈 칸으로 출력되었습니다.

### 해결 방식 및 구현
* **UTF-8 인코딩 헬퍼 도입**: 아두이노 공식 라이브러리([DFRobot_LcdDisplay](https://github.com/DFRobot/DFRobot_LcdDisplay))의 문자열 전송 방식을 기반으로 검증하였습니다. 아두이노 컴파일러처럼 UTF-16 문자열을 UTF-8 바이트 시퀀스로 변환하는 `stringToUtf8Bytes` 헬퍼 함수를 타입스크립트로 구현했습니다.
* **패킷 크기 및 데이터 동기화**: 텍스트, 이미지 경로, 차트 축 레이블 등을 전송할 때 실제 변환된 UTF-8 바이트 배열의 길이 및 바이트 데이터를 실어 보냄으로써, LCD 모듈 내에 장착된 폰트 칩(GT30L24A3W)이 한글을 인식하고 정상적으로 출력할 수 있도록 개선하였습니다.

---

## 📦 사용 방법 (Installation)

마이크로비트 메이크코드 에디터에서 다음 단계를 통해 본 개선판 라이브러리를 추가하여 사용할 수 있습니다.

1. [마이크로비트 메이크코드](https://makecode.microbit.org/) 에디터를 엽니다.
2. 우측 상단의 **설정(톱니바퀴 아이콘) -> 확장(Extensions)**으로 이동합니다.
3. 검색창에 아래의 이 저장소 주소를 복사하여 붙여넣고 검색합니다:
   ```text
   https://github.com/sjh5637/MyDFRobotLcdDisplay
   ```
4. 검색 결과에 나오는 `lcdDisplay` 카드로 확장 프로그램을 프로젝트에 추가합니다.

---

## 💻 사용 예제 (Example)

한글을 출력하려면 기존과 동일하게 텍스트 출력 블록 또는 코드를 사용하면 됩니다:

```typescript
// 1번 텍스트 슬롯에 (x: 10, y: 10) 위치로 흰색(0xFFFF)의 큰 글씨로 "안녕하세요" 출력
lcdDisplay.lcdDisplayText("안녕하세요", 1, 10, 10, lcdDisplay.FontSize.Large, 0xFFFF)
```

---

## 📝 License & Original
* **Original Codebase**: [DFRobot/pxt-DFRobot_lcdDisplay](https://github.com/DFRobot/pxt-DFRobot_lcdDisplay)
* **License**: MIT
