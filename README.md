# 🧟Crypto Zombie

# 컨트랙트

- 이더리움 애플리케이션의 기본적인 구성 요소로, 솔리디티의 모든 변수와 함수는 어느 한 컨트랙트에 속하게 된다.
- 예시 ; 비어있는 HelloWorld 컨트랙트
    
    ```solidity
    contract HelloWorld {
    
    }
    ```
    

### Version Pragma

- 해당 코드가 이용해야 하는 솔리디티 버전을 선언하는 것으로 모든 솔리디티 소스 코드는 version pragma로 시작한다.
- 이후에 새로운 컴파일러 버전이 나와도 기존 코드가 깨지지 않도록 예방
    
    ```solidity
    pragma solidity ^0.4.19;
    
    contract HelloWorld{
    
    }
    ```
    

# State Variables & Integers

> 상태변수 & 정수
> 
- 상태변수
    - 컨트랙트 저장소에 영구적으로 저장된다. 즉, 이더리움 블록체인에 기록되는 변수로 데이터베이스에 데이터를 쓰는 것과 동일
    
    ```solidity
    contract Example {
    	// 이 변수는 블록체인에 영구적으로 저장된다.
    	uint myUnsignedInteger = 100;
    }
    
    // myUnsignedInteger라는 uint을 생성하여 100이라는 값 배정
    ```
    

### 부호없는 정수 : uint

- uint자료형은 부호없는 정수로, 값이 음수가 아니어야 한다는 의미.
- 부호 있는 정수는 int자료형 사용

# Math Operations

> 수학 연산
> 
- Addition : x+y
- Subtracton: x-y
- Multiplication: x*y
- Division: x/y
- Modulus /  remainder: x%y

- 지수연산
    
    ```solidity
    uint x = 5**2; // 즉, 5^2 = 25
    ```
    

# Structs

> 구조체
> 

복잡한 자료형이 필요할 때

```solidity
struct Person{
	uint age;
	string name;
}
```

# Arrays

> 배열 (고정배열, 동적배열)
> 

```solidity
// 2개의 원소를 담을 수 있는 고정 길이의 배열:
uint[2] fixedArray;
// 또 다른 고정 배열으로 5개의 스트링을 담을 수 있다:
string[5] stringArray;
// 동적 배열은 고정된 크기가 없으며 계속 크기가 커질 수 있다:
uint[] dynamicArray;
```

구조체 배열

```solidity
Person[] people; 
//동적 배열, 원소 계속 추가 가능
```

동적배열을 생성하면 마치 데이터베이스처럼 컨트랙트에 구조화 된 데이터를 저장하는데 유용

### Public 배열

- public으로 배열 선언 가능
- 솔리디티는 이런 배열을 위해 getter메소드를 자동적으로 생성
    
    ```solidity
    Person[] public people;
    ```
    
    - 다른 컨트랙트들이 이 배열을 읽을 수 있게 된다.(read only, 사용은 불가)
    - 컨트랙트에 공개 데이터를 저장할 때 유용한 패턴

# Function Declarations

> 함수 선언
> 

```solidity
function eatHambergers(string _name, uint _amount) {

}
```

eatHambergers라는 함수. string 과 uint 2개의 인자를 전달받고 있다.

함수의 내용은 비어있다.

> 함수 인자명을 언더스코어(_)로 시작해서 전역 변수와 구별하는 것이 관례.
> 

위에 선언한 함수 호출은 다음과 같이 한다.

```solidity
eatHambergers("vitalik", 100);
```

# Working With Structs and Arrays

> 구조체와 배열 활용하기
> 

### 새로운 구조체 생성하기

```solidity
// 미리 생성한 구조체
struct Person {
	uint age;
	string name;
}

Person[] public people;
```

새로운 Person을 생성, people배열에 추가하려면

```solidity
// 새로운 사람을 생성한다:
Person satoshi = Person(172, "Satoshi")
// 이 사람을 배열에 추가한다:
people.push(satoshi);
```

위 두 코드를 한 줄로 표현하면,

```solidity
people.push(Person(16, "Vitalik"));
```

# Private / Public Functions

- 솔리디티에서 함수는 기본적으로 public 으로 선언된다.
    - 누구나 (혹 다른 어느 컨트랙트가) 컨트랙트의 함수를 호출하고 코드를 실행할 수 있다는 의미
    - 컨트랙트가 공격에 취약해질 수도 있음
        
        ⇒ 기본적으로 함수를 private으로 선언하고, 공개할 함수만 public으로 선언하는 것이 좋다.
        

### Private 함수

```solidity
uint[] numbers;

function _addToArray(uint _number) private{
	numbers.push(_number);
}
```

- 컨트랙트 내의 다른 함수들만이 이 함수를 호출하여 numbers 배열로 무언가를 추가할 수 있다
- private 키워드는 함수명 다음에 적고, 함수 인자명과 마찬가지로 private함수명도 언더바로 시작하는 것이 관례(public함수는 언더스코어 x)

# More on Functions

### 반환값(Return Values)

```solidity
string greeting = "What's up dog";

function sayHello() public returns (string) {
	return greeting;
}
```

- 솔리디티에서 함수 선언은 반환값 종류를 포함한다 (이 경우에는 string)

### 함수 제어자(Function modifiers)

- view함수
    - 위의 sayHello()는 솔리디티에서 상태를 변화시키지 않는다. 즉, 어떤 값을 변경하거나 무언가를 사용하지 않는다.
    - 이런 경우 함수를 view함수로 선언하는데, 이는 함수가 데이터를 보기만 하고 변경은 하지 않는다는 의미이다.
    
    ```solidity
    function sayHello() public view returns (string) {
    ```
    
- pure 함수
    - 함수가 앱에서 어떤 데이터도 접근하지 않는 것을 의미한다.
    
    ```solidity
    function _multiply(uint a, uint b) private pure returns (uint) {
    	return a * b;
    }
    ```
    
    - 앱에서 읽는 것도 하지 않고, 다만 반환값이 전달된 인자값에 따라 달라진다.
    

# Keccak256 and Typecasting

> Keccak256과 형 변환
> 

이더리움은 SHA3의 한 버전인 keccak256을 내장 해시 함수로 가진다.

해시 함수는 기본적으로 입력 스트링을 랜덤 256비트 16진수로 매핑

스트링에 약간의 변화라도 있으면 해시값은 크게 달라지게 된다.

- 해시함수로 pseudo-random number generator(의사 난수 발생기) 만들기
    
    ```solidity
    //6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5
    keccak256("aaaab");
    //b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9
    keccak256("aaaac");
    ```
    
    한 글자가 달라졌지만 반환값은 완전히 달라진다.
    

### Typecasting(형 변환)

```solidity
uint8 a = 5;
uint b = 6;
// a * b가 uint8이 아닌 uint를 반환하기 때문에 에러가 뜬다
uint8 c = a * b;
// b를 uint8으로 형 변환해서 코드가 제대로 작동하도록 해야 함
uint8 c = a * uint8(b);
```

# Events

컨트랙트가 블록체인 상에서 앱의 유저 단에서 무언가 액션이 발생했을 때 의사소통하는 방법

컨트랙트는 특정 이벤트가 일어나는지 주목하다가 그 이벤트가 발생하면 행동

```solidity
// 이벤트 선언
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public {
	uint result = _x + _y;
	//이벤트를 실행하여 앱에게 add 함수가 실행되었음을 알림
	IntegersAdded(_x, _y, result);
	return result;
}
```

자바스크립트로 구현하면 다음과 같음

```jsx
YourContract.IntegersAdded(function(error, result) {
	//결과와 관련된 행동을 취한다.
})
```

# Web3.js

이더리움은 Web3.js라고 하는 자바스크립트 라이브러리를 가진다.

```solidity
// 만든 컨트랙트에 접근하는 방법 제시
var abi = /* abi generated by the compiler */
var ZombieFactoryContract = web3.eth.contract(abi)
var contractAddress = /* our contract address on Etehreum after deploying */
var ZombieFactory = ZombieFactoryContract.at(contractAddress)
// 'ZombieFactory'는 우리 컨트랙트의 public함수와 이벤트에 접근할 수 있다.

//일종의 이벤트 리스너가 텍스트 입력값을 취한다:
$('#ourButton").click(function(e) {
	var name= $("#nameInput").val()
	// 우리 컨트랙트의 'createRandomZombie'함수를 호출한다:
	ZombieFactory.createRandomZombie(name)
})

// 'Newzombie' 이벤트가 발생하면 사용자 인터페이스를 업데이트
var event = ZombieFactory.NewZombie(function(error, result) {
	if (error) return
	generateZombie(result.zombieId, result.name, result.dna)
})

// 좀비 DNA값을 받아서 이미지를 업데이트
function generateZombie(id, name, dna) {
	let dnaStr = String(dna)
	// DNA값이 16자리 수보다 작은 경우 앞 자리를 0으로 채운다
	while (dnaStr.length < 16)
		dnaStr = "0" + dnaStr

	let zombieDetails = {
		// 첫 2자리는 머리의 타입을 결정한다. 머리 타입에는 7가지가 있다. 그래서 모듈로(%) 7 연산을 하여
    // 0에서 6 중 하나의 값을 얻고 여기에 1을 더해서 1에서 7까지의 숫자를 만든다. 
    // 이를 기초로 "head1.png"에서 "head7.png" 중 하나의 이미지를 불러온다:
		headChoice : dnaStr.substring(0, 2) % 7 + 1,
		// 두번째 2자리는 눈 모양을 결정한다. 눈 모양에는 11가지가 있다:
    eyeChoice: dnaStr.substring(2, 4) % 11 + 1,
    // 셔츠 타입에는 6가지가 있다:
    shirtChoice: dnaStr.substring(4, 6) % 6 + 1,
		// 마지막 6자리는 색깔을 결정하며, 360도(degree)까지 지원하는 CSS의 "filter: hue-rotate"를 이용하여 아래와 같이 업데이트된다:
    skinColorChoice: parseInt(dnaStr.substring(6, 8) / 100 * 360),
    eyeColorChoice: parseInt(dnaStr.substring(8, 10) / 100 * 360),
    clothesColorChoice: parseInt(dnaStr.substring(10, 12) / 100 * 360),
    zombieName: name,
    zombieDescription: "A Level 1 CryptoZombie",
  }
return zombieDetails
}
```

# Mappings and Addresses

> 매핑과 주소
> 

### 주소

- 이더리움 블록체인은 은행 계좌와 같은 계정들(accounts)로 이루어져 있다.
- 계정은 이더리움 블록체인상의 통화인 Ether의 잔액을 갖고, 계정을 통해 다른 계정과 이더를 주고받을 수 있다.
- 각 계정은 은행 계좌 번호와 같은 주소(adress)를 가진다. 주소는 특정 계정을 가리키는 고유 식별자로, 다음과 같이 표현한다.
    
    ```solidity
    0x0cE446255506E92DF41614C46F1d6df9Cc969183
    ```
    
- 주소는 특정 유저 혹은 스마트 컨트랙트가 소유한다. ⇒ 고유 id로 활용 가능

### 매핑

- 매핑 : 솔리디티에서 구조화된 데이터를 저장하는 방법 (구조체와 배열처럼)

```solidity
//금융 앱용으로, 유저의 계좌 잔액을 보유하는 uint를 저장한다:
mapping (address => uint) public accountBalance;
//혹은 userID로 유저 이름을 저장/검색하는 데 매핑을 쓸 수도 있다
mapping (uint => string) userIdToName;
```

- 매핑은 기본적으로 key-value 저장소로, 데이터를 저장하고 검색하는데 이용
- 첫번째 예시 ) 키: address , 값 : uint
두번째 예시 ) 키: uint , 값 : string

# Msg.sender

- 솔리디티에서 모든 함수에서 이용 가능한 특정 전역 변수
- 현재 함수를 호출한 유저 (혹은 스마트 컨트랙트)의 주소를 가르킨다.
- 솔리디티에서 함수 실행은 항상 외부 호출자가 시작한다. 컨트랙트는 누군가가 컨트랙트의 함수를 호출할 때까지 블록체인 상에서 아무런 작업도 하지 않는다 ⇒ 항상 msg.sender필요
- 예시) msg.sender를 이용하고, mapping을 업데이트
    
    ```solidity
    mapping (address => uint) favoriteNumber;
    
    function setMyNumber(uint _myNumber) public {
    	// 'msg.sender'에 대해 '_myNumber'가 저장되도록 'favoriteNumber'매핑을 업데이트
    	favoriteNumber[msg.sender] = _myNumber;
    	// 데이터를 저장하는 구문은 배열로 데이터를 저장할 때와 동일하다.
    }
    
    function whatIsMyNumber() public view returns (uint) {
    	//sender의 주소에 저장된 값을 불러온다
    	//sender가 'setMyNumber'을 아직 호출하지 않았다면 반환값은 '0'이 될 것
    	return favoriteNumber[msg.sender];
    }
    ```
    
    ⇒ setMyNumber을 호출하여 본인의 주소와 연결된 우리 컨트랙트 내에 uint를 저장할 수 있다.
    
- msg.sender를 활용하면 이더리움 블록체인의 보안성을 이용할 수 있게 된다.

# Require

- 특정 조건이 참이 아닐 때 함수가 에러 메시지를 발생하고 실행을 멈추게 된다.

```solidity
function sayHiToVitalik(string _name) public returns (string) {
	// _name이 'Vitalik'인지 비교. 참이 아닐 경우 에러 메시지를 발생하고 함수를 벗어남
	// 참고 : 솔리디티는 고유의 스트링 비교 기능을 가지고 있지 않기 때문에
	// 스트링의 keccak256 해시값을 비교하여 스트링 값이 같은지 판단한다.
	require(keccak256(_name) == keccak256("Vitalik"));
	// 참이면 함수 실행을 진행한다.
	return "Hi!";
```

- sayHiToVitalik(”Vitalik”)로 이 함수를 실행하면 “Hi!”반환
- “Vitalik”이 아닌 다른 값으로 이 함수를 호출할 경우, 에러 메시지가 뜨고 함수가 실행되지 않는다.
- 함수를 실행하기 전에 참이어야 하는 특정 조건을 확인하는 데 있어 꽤 유용

# Inheritance

> 상속
> 

```solidity
contract Doge {
	function catchphrase() public returns (string) {
		return "So Wow CryptoDoge";
	}
}

contract BabyDoge is Doge {
	function anotherCatchphrase() public returns (string) {
		return "Such Moon BabyDoge";
	}
}
```

- babyDoge 컨트랙트는 Doge컨트랙트를 상속
    
    즉, BabyDoge컨트랙트를 컴파일해서 구축할 때, BabyDoge 컨트랙트가 catchphrase()함수와 anotherCatchphrase()함수에 모두 접근할 수 있다.
    
- 상속개념은 “고양이는 동물이다” 의 경우처럼 부분집합 클래스가 있을 때 논리적 상속을 위해 활용될 수 있다. 하지만 동일한 로직을 다수의 클래스로 분할해서 단순히 코드를 정리할 때도 활용한다.

# Import

- 다수의 파일이 있고 어떤 파일을 다른 파일로 불러오고 싶을 때, 솔리디티는 import 라는 키워드를 이용한다.

```solidity
import "./someothercontract.sol";

contract newContract is SomeOtherContract {

}
```

⇒ 이 컨트랙트와 동일한 폴더(./)에 someothercontract.sol이라는 파일이 있을 때, 이 파일을 컴파일러가 불러오게 된다.

# Storage vs Memory

솔리디티에는 변수를 저장하는 공간으로 storage와 memory 두 공간이 있다.

- **Storage**
    - 블록체인 상에 영구적으로 저장되는 변수
- **Memory**
    - 임시적으로 저장되는 변수로, 컨트랙트 함수에 대한 외부 호출들이 일어나는 사이에 지워진다.
- 상태변수(함수 외부에 선언된 변수)는 초기 설정상 storage에 선언, 블록체인에 영구적으로 저장
- 함수 내에 선언된 변수는 memory로 자동 선언, 함수 호출이 종료되면 사라진다.
- 함수 내의 구조체와 배열을 처리할 때는 사용해야 함

```solidity
contract SandwichFactory {
	struct Sandwich {
		string name;
		string status;
	}

	Sandwich[] sandwiches;

	function eatSandwich(uint _index) public {
		//Sandwich mySandwich = sandwiches[_index];
		// ^ 꽤 간단해 보이나, 솔리디티는 여기서 
    // `storage`나 `memory`를 명시적으로 선언해야 한다는 경고 메시지를 발생한다. 
    // 그러므로 `storage` 키워드를 활용하여 다음과 같이 선언해야 한다:
		Sandwich storage mySandwich = sandwiches[_index];
    // ...이 경우, `mySandwich`는 저장된 `sandwiches[_index]`를 가리키는 포인터이다.
    // 그리고 
    mySandwich.status = "Eaten!";
    // ...이 코드는 블록체인 상에서 `sandwiches[_index]`을 영구적으로 변경한다.
		
		// 단순히 복사를 하고자 한다면 `memory`를 이용하면 된다: 
    Sandwich memory anotherSandwich = sandwiches[_index + 1];
    // ...이 경우, `anotherSandwich`는 단순히 메모리에 데이터를 복사하는 것이 된다. 
    // 그리고 
    anotherSandwich.status = "Eaten!";
    // ...이 코드는 임시 변수인 `anotherSandwich`를 변경하는 것으로 
    // `sandwiches[_index + 1]`에는 아무런 영향을 끼치지 않는다. 그러나 다음과 같이 코드를 작성할 수 있다: 
    sandwiches[_index + 1] = anotherSandwich;
    // ...이는 임시 변경한 내용을 블록체인 저장소에 저장하고자 하는 경우이다.
  }
}
```

# More on Function Visibility

### Internal 과 External

솔리디티에는 public과 private 이외에도 internal과 external이라는 함수 접근 제어자가 있다.

- **internal**
    - 함수가 정의된 컨트랙트를 상속하는 컨트랙트에서도 접근이 가능하다는 점을 제외하면 private과 동일하다.
- **external**
    - 함수가 컨트랙트 바깥에서만 호출될 수 있고, 컨트랙트 내의 다른 함수에 의해 호출될 수 없다는 점을 제외하면 public과 동일하다.
- 선언은 public, private과 동일

```solidity
contract Sandwich {
	uint private snadwichesEaten = 0;
	
	function eat() internal {
		snadwichesEaten++;
	}
}

contract BLT is Sandwich {
	uint private baconSandwichesEaten = 0;

	function eatWithBacon() public returns (string) {
		baconSandwichesEaten++;
		// eat 함수가 internal로 선언되었기 때문에 여기서 호출이 가능
		eat();
	}
}
```

# Interacting with other contracts

> 다른 컨트랙트와 상호작용하기
> 

블록체인 상에 있으면서 소유하지 않은 컨트랙트와 나의 컨트랙트가 상호작용을 하려면 우선 **인터페이스(interface)**를 정의해야 한다.

```solidity
contract LuckyNumber {
	mapping (address => uint) numbers;
	
	function setNum(uint _num) public {
		numbers[msg.sender] = _num;
	}
	
	function getNum(address _myAddress) public view returns (uint) {
		return numbers[_myAddress];
	}
}
```

- 아무나 자신의 행운의 수를 저장할 수 있는 간단한 컨트랙트
- 각자의 이더리움 주소와 연관, 이 주소를 이용해서 행운의 수를 찾아볼 수 있다.

getNum함수를 이용해 이 컨트랙트에 있는 데이터를 읽고자 하는 external함수가 있으면, 먼저 LuckyNumber 컨트랙트의 인터페이스를 정의해야 한다.

```solidity
contract NumberInterface {
	function getNum(address _myAddress) public view returns (uint);
}
```

- 인터페이스를 정의하는 것은 컨트랙트를 정의하는 것과 유사하다
    - 다른 컨트랙트와 상호작용하고자 하는 함수만을 선언
    - 다른 함수나 상태 변수를 언급하지는 않는다
    - 함수 몸체를 정의하지 않는다
    - 중괄호를 쓰지않고 함수 선언을 세미콜론(;)으로 간단하게 끝낸다.
    

# Using an Interface

> 인터페이스 활용하기
> 

```solidity
contract NumberInterface {
	function getNum(address _myAddress) public view returns (uint);
}
```

위와 같이 인터페이스가 정의되면 다음과 같이 컨트랙트에서 인터페이스를 이용할 수 있다.

```solidity
contract MyContract {
	address NumberInterfaceAddress = 0xab38...
	// ^ 이더리움상의 FavoriteNumber 컨트랙트 주소
	NumberInterface numberContract = NumberInterface(NumberInterfaceAddress)
	// 이제 'numberContract'는 다른 컨트랙트를 가르킨다.

	function someFunction() public {
		// 이제 'numbercontract'가 가리키는 컨트랙트에서 'getNum' 함수를 호출할 수 있다.
		uint num = numberContract.getNum(msg.sender);
		// ...그리고 여기서 'num'으로 무언가를 할 수 있음
```

상호작용하는 함수가 public이나 external로 선언되어 있다면 내 컨트랙트는 이더리움 블록체인 상의 다른 어떤 컨트랙트와도 상호작용할 수 있다.

# Handling Multiple Return Values

> 다수의 반환값 처리하기
> 

```solidity
function multipleReturns() internal returns (uint a, uint b, uint c) {
	return (1, 2, 3);
}

function processMultipleReturns() external {
	uint a;
	uint b;
	uint c;
	// 다음과 같이 다수 값을 할당한다
	(a, b, c) = multipleReturns();
}

// 혹은 단 하나의 값에만 관심이 있을 경우:
function getLastReturnValue() external {
	uint c;
	// 다른 필드는 빈칸으로 놓기만 하면 된다.
	(,,c) = multipleReturns();
}
```

# If statements

솔리디티에서 if문은 자바스크립트의 if문과 동일하다
```solidity
function  eatBLT(string sandwich) public {
	// 스트링 간의 동일 여부를 판단하기 위해 keccak256해시 함수 이용
	if (keccak256(sandwich) == keccak256("BLT")) {
		eat();
	}
}
```
