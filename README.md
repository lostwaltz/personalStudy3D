# PersonalStudy3D

3D 공간의 유니티 엔진을 적응을 위한 개인 프로젝트입니다.

## 조작법

- **이동**: `W`, `A`, `S`, `D`
- **점프**: `SPACE`
- **인벤토리**: `TAB`
- **상호작용**: `E`

---

<details>
<summary>토글 접기/펼치기</summary>

### 구현 기능

---

### **기본 이동 및 점프**


- **난이도**: ★★☆☆☆
    - `InputSystem`의 `Invoke`를 통해 이벤트를 트리거하고 `Movement`에서 구독하여 입력값에 따른 변수를 초기화합니다.
    - 상태(State)에 따라 실제 움직일 함수를 호출합니다.

---

### **체력바 UI**


- **난이도**: ★★☆☆☆
    - 체력바는 `ValueSystem`을 상속받아 `Update`를 추상화합니다.
    - `VitalController`에서 추상화된 `ValueSystem`의 함수를 제공하고 호출합니다.

---

### **동적 환경 조사**


- **난이도**: ★★★☆☆
    - `Interaction` 컴포넌트에서 레이 충돌을 검사합니다.
    - 충돌한 오브젝트의 `IInteractable` 인터페이스를 통해 추상화 함수를 호출합니다.

---

### **점프대** *(Class JumpPad)*


- **난이도**: ★★★☆☆
    - `OnCollisionEnter`를 통해 충돌 시 충돌된 `Collision`의 `Rigidbody`에 `AddForce`를 호출합니다.

---

### **아이템 데이터**


- **난이도**: ★★★☆☆
    - `ItemSO`를 상속받는 `ConsumableSO`, `EquipmentSO`를 생성했습니다.

---

### **아이템 사용**


- **난이도**: ★★★☆☆
    - `ItemSO`의 `Use` 메소드를 추상화하고, 상속받는 SO에서 `Use`를 구현하도록 했습니다. 아이템의 사용과 장비의 장착은 인벤토리에서 클릭하는것으로 이루어집니다.

---

## 3️⃣ 도전 기능 가이드

> 🚨 모든 필수 기능 구현을 마친 후 선택적으로 도전하는 기능입니다.
> 

---

### **추가 UI**

- **난이도**: ★★☆☆☆
    - **스태미너 UI**와 **인벤토리 UI**를 제작했습니다.
    - `ValueSystem`을 상속받아 `Update`를 추상화하고, `VitalController`에서 함수를 제공하고 호출합니다.
    - 인벤토리 UI는 단순하게 구현되어 있어, 개편의 필요성을 느끼고 있습니다.

---

### **3인칭 시점**

- **난이도**: ★★★☆☆
    - `Movement`의 `ApplyLook` 메소드에서 3인칭 시점이 될 수 있도록 계산하고 있습니다.

---

### **움직이는 플랫폼 구현**

- **난이도**: ★★★☆☆
    - `Platform` 컴포넌트를 통해 유니티에서 제공하는 `Mathf.PingPong`을 사용하여 0부터 1까지 값을 얻고 해당 값으로 보간했습니다.
    - `OnCollisionEnter` 시 플레이어의 계층 구조를 `Platform`의 자식으로 변경합니다.

---

### **벽 타기 및 매달리기**

- **난이도**: ★★★★☆
    - 플레이어의 앞 방향으로 `Raycast`를 수행합니다.
    - 바라보는 곳 앞에 사다리가 존재하면 `Climb` 상태로 전이됩니다.
    - 해당 상태에서는 `ApplyMove` 대신 `ApplyClimb`을 호출해 진행했습니다.
    - `AddForce.Acceleration`을 사용해 구현해봤으나 자연스러운 움직임 구현이 어려워 입력값을 Y값으로 치환해 구현했습니다.

---

### **다양한 아이템 구현**

- **난이도**: ★★★★☆
    - `ConsumableSO`에서 `Use` 메소드를 재정의하여 `Type`에 따라 `switch` 분기를 나누었습니다.
    - 많은 분기가 생길 경우 다형성을 통해 리팩토링해야 함을 느끼고 있습니다.

---

### **장비 장착**

- **난이도**: ★★★★☆
    - `Equipment`, `Equip` 클래스를 통해 구현했습니다.
    - 장착 시 `EquipmentSO`를 받습니다.

---

### **레이저 트랩**

- **난이도**: ★★★★☆
    - `Raycast`를 사용해 특정 구간을 레이로 감시합니다.
    - 플레이어가 레이저를 통과하면 `Text`를 활성화하고 코루틴을 통해 유지시켰습니다.

---

### **상호작용 가능한 오브젝트 표시**

- **난이도**: ★★★★★
    - `Interaction`에서 `IInteractable`의 `OnHitRay` 메소드를 통해 호출됩니다.
    - `Cannon`에서 상속받은 `IInteractable`의 `OnHitRay`를 구현하면서 `WorldCanvas`의 `Text` 위치를 상호작용 중인 오브젝트의 머리 위에 위치하게 했습니다.

---

### **플랫폼 발사기**

- **난이도**: ★★★★★
    - `Cannon`에서 구현한 `IInteractable`의 `OnInteract`에서 발사를 구현했습니다.
    - 현재 프로젝트 구조상 `ApplyMove`가 호출될 때 입력값이 없으면 `Velocity`를 0으로 초기화하는데, 입력값이 없을 때 항상 호출되어 `AddForce`를 통해 발사돼도 X, Z축 힘이 초기화되는 문제가 있었습니다.
    - **상태 패턴**을 통해 `InteractionState`를 만들었고, 해당 상태에서 `ApplyMove`를 호출하지 않도록 하여 해결했습니다.
    - **의문점 및 개선 사항**:
        - `InteractionState`에 진입하기 위한 조건 검사에 있어서 더 좋은 방법이 없을까 고민하고 있습니다.
        - 플레이어가 `IInteractable` 오브젝트를 찾아 이벤트를 구독하는 것은 동적 생성될 객체에 문제가 있고, 동적 생성될 객체가 플레이어를 찾아 직접 이벤트를 연결해주는 것도 깔끔하지 않은 것 같습니다.
        - 현재 구조는 `OnInteract` 호출 시 플레이어의 상태를 외부에서 강제로 `TransitionTo` 하는 것인데, 외부에서 값 수정이 빈번하면 값의 신뢰도, 순서, 변수 제어가 어려워집니다.
        - 어떤 방법이 가장 확실한지 감을 잡지 못했지만, 학습하고 수정하도록 하겠습니다.

---

### **미구현 기능**

- **발전된 AI**
    - **난이도**: ★★★★★

</details>
