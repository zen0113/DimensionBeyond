
<!-- ===================== -->
<!--  GitHub Project README -->
<!-- ===================== -->

<div align="center">

![header](https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=180&section=header&text=Dimension%20Beyond&fontSize=60&animation=twinkling)

</div>

<br>

## 📌 Overview

차원너머(Dimension Beyond)는  
XR Interaction Toolkit을 기반으로 개발된  
**VR 포탈 기반 퍼즐 탈출 & 보스 레이드 게임**입니다.

플레이어는 VR 컨트롤러를 사용해  
직접 포탈을 생성하고, 물체를 집어 던지며,  
공간을 이동하고 전투에 참여합니다.

Raycast 기반 포탈 생성,  
물리 엔진과 결합된 투척 메커닉을 통해  
VR 환경에서의 직관적인 공간 상호작용에 집중했습니다.

<br><br>

### Project Information

<div align="center">

| 항목 | 내용 |
| :-- | :-- |
| 개발 기간 | 2024.12.06 ~ 2024.12.16 (10일) |
| 플랫폼 | Unity VR (Meta Quest) |
| 개발 형태 | 1인 개발 |
| 역할 | 프로그래밍 / 게임 기획 |

</div>

<br><br><br>

## 🎮 Demo

<div align="center">

| 플레이 화면 | 설명 |
| :--: | :-- |
| <img src="https://github.com/zen0113/DimensionBeyond/blob/main/DimensionBeyond_1.gif?raw=true" width="300"/> <img src="https://github.com/zen0113/DimensionBeyond/blob/main/DimensionBeyond_4.gif?raw=true" width="300"/> | 포탈건을 활용한 이동 및 퍼즐 해결 |
| <img src="https://github.com/zen0113/DimensionBeyond/blob/main/DimensionBeyond_2.gif?raw=true" width="300"/> <img src="https://github.com/zen0113/DimensionBeyond/blob/main/DimensionBeyond_3.gif?raw=true" width="300"/> | 물리 기반 투척과 회피를 요구하는 보스 전투 |

</div>

<br><br><br>

## 💻 Core Systems

### Raycast Portal & Teleport System

어떤 각도의 벽면에서도  
포탈이 자연스럽게 생성되고,  
이동 시 플레이어가 지형에 끼지 않도록 설계하는 것이 핵심 과제였습니다.

이를 위해  
Raycast 결과의 법선 벡터를 기준으로  
포탈의 회전과 텔레포트 좌표를 계산했습니다.

* `RaycastHit.normal`을 기반으로 포탈 회전값 계산
* `Quaternion.LookRotation`을 사용해 벽면을 등지는 방향 유지
* 텔레포트 시 출구 방향으로 거리 보정하여 안전한 이동 보장

```csharp
private void CreatePortal(Vector3 hitPoint, Vector3 surfaceNormal) {
    Quaternion rotation = Quaternion.LookRotation(surfaceNormal);
    portalA = Instantiate(portalPrefabA, hitPoint, rotation);
}

public Vector3 GetTeleportDestination(Transform targetPortal) {
    return targetPortal.position + targetPortal.forward * 1.0f;
}
````

<br><br>

### VR Physics Interaction

VR 특유의 손 동작이
자연스럽게 게임 플레이로 이어지도록
XR Interaction Toolkit을 커스터마이징했습니다.

* `XRGrabInteractable` 이벤트 기반 물체 잡기
* 투척 시 Grab 해제 후 `AddRelativeForce` 적용
* 로컬 좌표계를 기준으로 한 일관된 투척 궤적 구현

이를 통해
플레이어의 손 동작과
물리 결과 사이의 이질감을 최소화했습니다.

<br><br>

### Event-driven & Feedback System

전투와 상호작용의 타격감을 강화하기 위해
이벤트 기반 구조를 도입했습니다.

* `UnityEvent`를 활용해 보스 피격 / 페이즈 전환 처리
* 코드 수정 없이 인스펙터에서 사운드 및 이펙트 연결
* 카메라 쉐이크와 컨트롤러 진동을 연동한 피드백 설계

<br><br><br>

## 🎯 Stage & Combat Design

### Stage Structure
<img src="https://github.com/zen0113/DimensionBeyond/blob/main/007.png?raw=true" width="500"/> <img src="https://github.com/zen0113/DimensionBeyond/blob/main/008.png?raw=true" width="500"/> 
<img src="https://github.com/zen0113/DimensionBeyond/blob/main/010.png?raw=true" width="500"/> 


게임은
튜토리얼 → 응용 → 전투
구조로 단계적으로 확장됩니다.

1. 포탈건 조작 학습 및 큐브 이동 퍼즐
2. 다층 구조에서의 정밀한 포탈 활용
3. 보스전: 폭탄 투척과 레이저 회피 중심 전투

<br>

### Interaction Design

* 포탈을 통한 공간 이동
* 물리 기반 오브젝트 투척
* VR 시점에서의 거리 감각과 위치 인지 강화

<br><br><br>

## 🛠 Technical Stack
![Unity](https://img.shields.io/badge/Unity-000000?style=for-the-badge&logo=unity&logoColor=white)
![C#](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=csharp&logoColor=white)
![XR Interaction Toolkit](https://img.shields.io/badge/XR%20Interaction%20Toolkit-2C2255?style=for-the-badge)


</div>

<br>

**주요 기술 요소**

* Unity XR Interaction Toolkit (Action-based)
* Raycast & Vector Mathematics
* Physics-based Grab & Throw
* Event-driven Architecture
* Haptic Feedback & Camera Shake

<br><br><br>

## 📚 Lessons Learned

이번 프로젝트를 통해
다음과 같은 경험을 얻었습니다.

* 법선 벡터와 전방 벡터를 활용한 3D 공간 제어 이해
* VR 환경에서 텔레포트 시 UX 안정성이 얼마나 중요한지 체감
* 이벤트 기반 구조가 유지보수와 확장성에 효과적임을 학습
* 물리 엔진과 사용자 입력을 결합한 상호작용 설계 경험

<br><br><br>

<div align="center">

![footer](https://capsule-render.vercel.app/api?type=waving\&color=gradient\&customColorList=6,11,20\&height=120\&section=footer)

</div>
