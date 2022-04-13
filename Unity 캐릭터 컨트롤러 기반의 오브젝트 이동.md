# Unity 캐릭터 컨트롤러 기반의 오브젝트 이동

- 3D 사람 형태의 캐릭터 움직임과 관련된 제어를 위해 사용
- 캡슐 형태의 충돌 범위가 포함되어 있음

- Slope Limit : 올라갈 수 있는 경사 한계 각
- Step Offset : 설정 값보다 낮은 높이의 계단을 오를 수 있다.



## 1. 카메라를 전체 조향이 가능한 위치로 수정함.



## 2. Object 생성



## 3. 플레이어 위치 설정



## 4. Character Controller Component 추가



## 5. MoveMent Script 작성

```C#
using UnityEngine;
//클래스 명과 파일명이 항상 동일해야함
public class Movement3D : MonoBehaviour
{
    [SerializeField]
    private float moveSpeed = 5.0f; //이동 속도
    private Vector3 moveDirection; 
    
    private CharacterController characterController;
    
    private void Awake()
    {
        characterController = GetComponent<CharacterController>();
    }
    
    private void Update()
    {
        characterController.Move(moveDirection * moveSpeed * Time.deltaTime);
    }
    
    public void MoveTo(Vector direction)
    {
        moveDirection = direction;
    }
}
    
/*
CharacterController.Move(Vector3 motion);
매개변수로 이동방향, 속도, Time.deltaTime 등의 세부적인 이동 방법을 설정하면 이동을 수행함.
CharacterContorller.SimpleMove(Vector3 speed);
매개변수로 (x, y, z) 방향의 이동속도(= 속력)를 넣어 호출하면 이동을 수행함
*/
```

## 6. PlayerController Script 생성

```C#
public class PlayerController : MonoBhaviour
{
    private Movement3D movement3D;
    
    private void Awake()
    {
        movement3D = GetComponent<Movement3D>();
    }
    
    private void Update()
    {
        // x, z 방향 이동
        float x = Input.GetAxisRaw("Horizontal"); // 방향키 좌 / 우 움직임
        float z = Input.GetAxisRaw("Vertical");   // 방향키 위 / 아래 움직임
        
        movement3D.MoveTo(new Vector(x, 0, z))
    }
}
```



## 7. 위 두가지 스크립트를 player object에 컴포넌트로 추가함.





## 8. Movement Script에 중력 추가

```C#
//클래스 명과 파일명이 항상 동일해야함
public class Movement3D : MonoBehaviour
{
    [SerializeField]
    private float moveSpeed = 5.0f; // 이동 속도
    private float gravity = -9.81f; // 중력 계수
    private Vector3 moveDirection; 
    
    private CharacterController characterController;
    
    private void Awake()
    {
        characterController = GetComponent<CharacterController>();
    }
    
    private void Update()
    {
        //characterController.isGrounded : 출발 위치의 충돌을 체크해 충돌이 되면 true 충돌이 되지 않으면 false 값을 나타내는 변수
        // 만약 공중에 떠 있는 경우라면
        if (characterController.isGrounded == false) 
        {
            moveDirection.y += gravity * Time.deltaTime;
        }
        
        characterController.Move(moveDirection * moveSpeed * Time.deltaTime);
    }
    
    public void MoveTo(Vector direction)
    {
        //moveDirection = direction;
        moveDirection = new Vector3(direction.x, moveDirection.y, direction.z);
    }
}
    
/*
CharacterController.Move(Vector3 motion);
매개변수로 이동방향, 속도, Time.deltaTime 등의 세부적인 이동 방법을 설정하면 이동을 수행함.
CharacterContorller.SimpleMove(Vector3 speed);
매개변수로 (x, y, z) 방향의 이동속도(= 속력)를 넣어 호출하면 이동을 수행함
*/
```



## 9. 점프 구현

```C#
//클래스 명과 파일명이 항상 동일해야함
public class Movement3D : MonoBehaviour
{
    [SerializeField]
    private float moveSpeed = 5.0f; // 이동 속도
    [SerializeField]
    private float jumpForce = 3.0f;
    private float gravity = -9.81f; // 중력 계수
    private Vector3 moveDirection; 
    
    private CharacterController characterController;
    
    private void Awake()
    {
        characterController = GetComponent<CharacterController>();
    }
    
    private void Update()
    {
        //characterController.isGrounded : 출발 위치의 충돌을 체크해 충돌이 되면 true 충돌이 되지 않으면 false 값을 나타내는 변수
        // 만약 공중에 떠 있는 경우라면
        if (characterController.isGrounded == false) 
        {
            moveDirection.y += gravity * Time.deltaTime;
        }
        
        characterController.Move(moveDirection * moveSpeed * Time.deltaTime);
    }
    
    public void MoveTo(Vector direction)
    {
        //moveDirection = direction;
        moveDirection = new Vector3(direction.x, moveDirection.y, direction.z);
    }
    
    public void JumpTo()
    {
        if (characterController.isGrounded == true)
        {
            moveDirection.y = jumpForce;
        }
    }
}
    
/*
CharacterController.Move(Vector3 motion);
매개변수로 이동방향, 속도, Time.deltaTime 등의 세부적인 이동 방법을 설정하면 이동을 수행함.
CharacterContorller.SimpleMove(Vector3 speed);
매개변수로 (x, y, z) 방향의 이동속도(= 속력)를 넣어 호출하면 이동을 수행함
*/
```



## 10. player controller 스크립트 수정

```C#
public class PlayerController : MonoBhaviour
{
    [SerializeField]
    private KeyCode jumpKeyCode = KeyCode.Space;
    private Movement3D movement3D;
    
    private void Awake()
    {
        movement3D = GetComponent<Movement3D>();
    }
    
    private void Update()
    {
        // x, z 방향 이동
        float x = Input.GetAxisRaw("Horizontal"); // 방향키 좌 / 우 움직임
        float z = Input.GetAxisRaw("Vertical");   // 방향키 위 / 아래 움직임
        
        movement3D.MoveTo(new Vector(x, 0, z));
        
        if (Input.GetKeyDown(jumpKeyCode))
        {
            movement3D.JumpTo();
        }           
    }
}
```

